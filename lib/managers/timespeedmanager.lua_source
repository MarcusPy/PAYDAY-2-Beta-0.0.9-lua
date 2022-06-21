TimeSpeedManager = TimeSpeedManager or class()

function TimeSpeedManager:init()
	self._pausable_timer = TimerManager:pausable()
	self._game_timer = TimerManager:game()
	self._game_anim_timer = TimerManager:game_animation()
	--self._playing_effects = nil
	--self._affected_timers = nil -- when there are effects playing it looks like this: self._affected_timers[ timer_key ] = { timer = affect_timer, mul = 1 }
	
	self._game_speed_rtpc = 1
end

-----------------------------------------------------------------------------------

function TimeSpeedManager:update()
	if self._playing_effects then
		self:_update_playing_effects()
	end
end

-----------------------------------------------------------------------------------

function TimeSpeedManager:_update_playing_effects()
	local slowest_speed
	
	local playing_effects = self._playing_effects
	
	if playing_effects then
		
		-- Reset all multipliers
		local all_affected_timers = self._affected_timers
		for timer_key, timer_info in pairs( all_affected_timers ) do
			timer_info.mul = 1
		end
		
		-- calculate fresh multipliers
		for effect_id, effect in pairs( playing_effects ) do
			local time = effect.timer:time()
			
			local effect_speed
			if time < effect.fade_in_delay_end_t then
			elseif time < effect.fade_in_end_t then
				effect_speed = math.lerp( 1, effect.desc.speed, ( ( time - effect.start_t ) / effect.desc.fade_in )^0.5 )
			elseif not effect.sustain_end_t or ( time < effect.sustain_end_t ) then
				effect_speed = effect.desc.speed
			elseif time < effect.effect_end_t then
				effect_speed = math.lerp( effect.desc.speed, 1, ( ( time - effect.sustain_end_t ) / effect.desc.fade_out )^0.5 )
			else
				self:_on_effect_expired( effect_id )
			end
			
			if effect_speed then
				for timer_key, affect_timer in pairs( effect.affect_timers ) do
					local timer_info = self._affected_timers[ timer_key ]
					timer_info.mul = math.min( timer_info.mul, effect_speed ) -- keep the lowest multiplier of all playing effects
				end
				slowest_speed = (not slowest_speed and effect_speed) or math.min( slowest_speed, effect_speed )
			end
		end
		
		
			-- (ugly amount of if's), changes the sound device rtpc "game_speed" 0:slo-mo, 1:normal, 2:speed up
		local game_speed_rtpc_changed = false
		if( slowest_speed ) then
			if( slowest_speed < 0.5 ) then
				if( self._game_speed_rtpc ~= 0 ) then
					self._game_speed_rtpc = 0
					game_speed_rtpc_changed = true
				end
			elseif( slowest_speed > 1.5 ) then
				if( self._game_speed_rtpc ~= 2 ) then
					self._game_speed_rtpc = 2
					game_speed_rtpc_changed = true
				end
			else
				if( self._game_speed_rtpc ~= 1 ) then
					self._game_speed_rtpc = 1
					game_speed_rtpc_changed = true
				end
			end
		else
			if( self._game_speed_rtpc ~= 1 ) then
				self._game_speed_rtpc = 1
				game_speed_rtpc_changed = true
			end
		end
		
		if( game_speed_rtpc_changed ) then
			-- print( "[TimeSpeedManager]", self._game_speed_rtpc, slowest_speed )
			SoundDevice:set_rtpc( "game_speed", self._game_speed_rtpc )
		end
		
		
		-- apply the new multipliers
		if self._affected_timers then
			for timer_key, timer_info in pairs( all_affected_timers ) do
				timer_info.timer:set_multiplier( timer_info.mul )
			end
		end
	end
end

-----------------------------------------------------------------------------------

--[[
	effect_desc = {
		speed = 0.5 -- multiplier to the timer when in full blend
		,fade_in = 0.25 -- how long it takes to reach full blend from 1. (sec)
		,sustain = 0.8 -- how long the effect stays in full blend. (sec)
		,fade_out = 0.7 -- how long it takes to reach zero blend from full blend. (sec)
		,timer = "game" -- Timer to be used for progression of the effect. Can be "game" or "pausable".
		,affect_timer = { Idstring( "player" ), Idstring( "game_animation" ) } -- which timers dos this effect affect? can be a table with timer names or a single timer name
	}
]]

function TimeSpeedManager:play_effect( id, effect_desc )
	local effect = {
		desc = effect_desc
	}
	effect.timer = effect_desc.timer == "pausable" and self._pausable_timer or self._game_timer
	effect.start_t = effect.timer:time()
	effect.fade_in_delay_end_t = effect.start_t + ( effect_desc.fade_in_delay or 0 )
	effect.fade_in_end_t = effect.fade_in_delay_end_t + effect_desc.fade_in
	effect.sustain_end_t = effect_desc.sustain and ( effect.fade_in_end_t + effect_desc.sustain )
	effect.effect_end_t = effect.sustain_end_t and ( effect.sustain_end_t + ( effect_desc.fade_out or 0 ) )
	
	if effect_desc.affect_timer then
		if type( effect_desc.affect_timer ) == "table" then
			effect.affect_timers = {}
			for _, ids_timer_name in ipairs( effect_desc.affect_timer ) do
				local timer = TimerManager:timer( ids_timer_name )
				effect.affect_timers[ timer:key() ] = timer
			end
		else
			local timer = TimerManager:timer( effect_desc.affect_timer )
			effect.affect_timers = { [ timer:key() ] = timer }
		end
	else
		effect.affect_timers = { [ self._game_timer:key() ] = self._game_timer, [ self._game_anim_timer:key() ] = self._game_anim_timer }
	end
	
	self._affected_timers = self._affected_timers or {}
	for timer_key, affect_timer in pairs( effect.affect_timers ) do
		if self._affected_timers[ timer_key ] then
			self._affected_timers[ timer_key ].ref_count = self._affected_timers[ timer_key ].ref_count + 1
		else
			self._affected_timers[ timer_key ] = { timer = affect_timer, mul = 1, ref_count = 1 }
		end
	end
	
	self._playing_effects = self._playing_effects or {}
	self._playing_effects[ id ] = effect
	
	if effect_desc.sync and managers.network:session() and not managers.network:session():closing() then
		managers.network:session():send_to_peers_synched( "start_timespeed_effect", id, effect_desc.timer, effect_desc.speed, effect_desc.fade_in or 0, effect_desc.sustain or 0, effect_desc.fade_out or 0 )
	end
end

-----------------------------------------------------------------------------------

function TimeSpeedManager:stop_effect( id, fade_out_duration )
	if not self._playing_effects then
		return
	end
	
	if managers.network:session() and not managers.network:session():closing() then
		local effect_instance = self._playing_effects[ id ]
		if effect_instance and effect_instance.desc.sync then
			local sync_fade_out_duration
			if fade_out_duration and fade_out_duration ~= 0 then
				sync_fade_out_duration = fade_out_duration
			else
				sync_fade_out_duration = 0
			end
			managers.network:session():send_to_peers_synched( "start_timespeed_effect", id, sync_fade_out_duration )
		end
	end
	
	if fade_out_duration and fade_out_duration ~= 0 then
		local effect_instance = self._playing_effects[ id ]
		if not effect_instance then
			return
		end
		
		local t = effect_instance.timer:time()
		effect_instance.sustain_end_t = t
		effect_instance.effect_end_t = t + fade_out_duration
	else
		self:_on_effect_expired( id )
	end
end

-----------------------------------------------------------------------------------

function TimeSpeedManager:_on_effect_expired( effect_id )
	local effect = self._playing_effects[ effect_id ]
	
	for timer_key, affect_timer in pairs( effect.affect_timers ) do
		local timer_info = self._affected_timers[ timer_key ]
		if timer_info.ref_count == 1 then -- no more effects affect this timer
			timer_info.timer:set_multiplier( 1 )
			self._affected_timers[ timer_key ] = nil
		else
			timer_info.ref_count = timer_info.ref_count - 1
		end
	end
	
	self._playing_effects[ effect_id ] = nil
	if not next( self._playing_effects ) then
		self._playing_effects = nil
		self._affected_timers = nil
	end
end

-----------------------------------------------------------------------------------

function TimeSpeedManager:in_effect()
	return self._playing_effects and true
end

-----------------------------------------------------------------------------------

function TimeSpeedManager:destroy()
	while self._playing_effects do
		local eff_id, eff = next( self._playing_effects )
		self:_on_effect_expired( eff_id )
	end
end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------