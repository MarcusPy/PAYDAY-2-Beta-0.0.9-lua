-- The EnvironmentEffectsManager holds different kinds of environment effects. The can be started by calling 
-- the use function with desired effect. They can be stopped using the stop function or all can be stopped with stop_all
-- All effects should inherit EnvironmentEffect

core:import( "CoreEnvironmentEffectsManager" )

EnvironmentEffectsManager = EnvironmentEffectsManager or class( CoreEnvironmentEffectsManager.EnvironmentEffectsManager )

function EnvironmentEffectsManager:init()
	EnvironmentEffectsManager.super.init( self )
	
	-- if not PackageManager:loaded( "packages/rain_effect" ) then
	-- 	PackageManager:load( "packages/rain_effect" )
	-- end
		
	self:add_effect( "rain", RainEffect:new() )
	self:add_effect( "raindrop_screen", RainDropScreenEffect:new() )
	self:add_effect( "lightning", LightningEffect:new() )
	
	-- self:add_effect( "lightning", LightningEffect:new() )
	
	self._camera_position = Vector3()
	self._camera_rotation = Rotation()
end

function EnvironmentEffectsManager:update( t, dt )
	self._camera_position = managers.viewport:get_current_camera_position()
	self._camera_rotation = managers.viewport:get_current_camera_rotation()
	EnvironmentEffectsManager.super.update( self, t, dt )
end

function EnvironmentEffectsManager:camera_position()
	return self._camera_position
end

function EnvironmentEffectsManager:camera_rotation()
	return self._camera_rotation
end

---------------------------------------------------------------

EnvironmentEffect = EnvironmentEffect or class()

function EnvironmentEffect:init( default )
	self._default = default
	
end

-- Load effects are called before start of the effect
-- Don't want the effects to be loaded if we are not going to use them
function EnvironmentEffect:load_effects()
	
end

function EnvironmentEffect:update( t, dt )

end

function EnvironmentEffect:start()

end

function EnvironmentEffect:stop()

end

function EnvironmentEffect:default()
	return self._default
end

---------------------------------------------------------------

RainEffect = RainEffect or class( EnvironmentEffect )

function RainEffect:init()
	EnvironmentEffect.init( self )

	-- self._slotmask = managers.slot:get_mask( "env_effect" )
	self._effect_name = Idstring( "effects/particles/rain/rain_01_a" )
end

function RainEffect:load_effects()
	
end

function RainEffect:update( t, dt )
	local vp = managers.viewport:first_active_viewport()
	if vp then
		if self._vp ~= vp then
			vp:vp():set_post_processor_effect( "World", Idstring("streaks"), Idstring("streaks_rain") )
			if alive( self._vp ) then
				self._vp:vp():set_post_processor_effect( "World", Idstring("streaks"), Idstring("streaks") )
			end
			self._vp = vp
		end
	end
		
	local c_rot = managers.environment_effects:camera_rotation()
	if not c_rot then
		return
	end

	local c_pos = managers.environment_effects:camera_position()
	if not c_pos then
		return
	end
	
	World:effect_manager():move_rotate( self._effect, c_pos, c_rot )
end

function RainEffect:start()
	self._effect = World:effect_manager():spawn( { effect = self._effect_name, position = Vector3(), rotation = Rotation() } )
end

function RainEffect:stop()
	World:effect_manager():kill( self._effect )
	self._effect = nil
	if alive( self._vp ) then
		self._vp:vp():set_post_processor_effect( "World", Idstring("streaks"), Idstring("streaks") )
		self._vp = nil
	end
end

--------------------------------------------------------------------------

LightningEffect = LightningEffect or class( EnvironmentEffect )

function LightningEffect:init()
	EnvironmentEffect.init( self )
end

function LightningEffect:load_effects()
end

function LightningEffect:_update_wait_start()
	if Underlay:loaded() then
		self:start()
	end
end

function LightningEffect:_update( t, dt )
	if self._flashing then
		self:_update_function( t, dt )
	end
	
	if self._sound_delay then
		self._sound_delay = self._sound_delay - dt
		if self._sound_delay <= 0 then
			-- print( "self._distance", self._distance )
			self._sound_source:post_event( "thunder" )
			-- print( "PLAY THUNDER" )
			
			self._sound_delay = nil
			-- self._thunder_sound:play()
		end
	end
	
	self._next = self._next - dt
	if self._next <= 0 then
		self:_set_lightning_values()
		self:_make_lightning()
		self._update_function = self._update_first
		self:_set_next_timer()
		self._flashing = true
	end
end

function LightningEffect:start()
	-- local sky = managers.environment:get_sky()
	if not Underlay:loaded() then
		self.update = self._update_wait_start
		return
	end
	self.update = self._update
	
	self._sky_material = Underlay:material( Idstring( "sky" ) )
	
	self._original_color0 = self._sky_material:get_variable( Idstring( "color0" ) )
	self._original_light_color = Global._global_light:color()
	self._original_sun_horizontal = Underlay:time( Idstring("sun_horizontal") )
		
	self._min_interval = 2
	self._rnd_interval = 10
	
	self._sound_source = SoundDevice:create_source( "thunder" )
	-- :post_event( "trip_mine_explode" )
	-- self._environment = managers.environment:get()
	-- Sound:add_soundbank ( "thunder_sound" )
	-- self._thunder_sound = Sound:make_bank( "thunder_sound", "thunder_clap" )
	self:_set_next_timer()
end

function LightningEffect:stop()
end

function LightningEffect:_update_first( t, dt )
	self._first_flash_time = self._first_flash_time - dt
	if self._first_flash_time <= 0 then
		self:_set_original_values()
		self._update_function = self._update_pause
	end
end

function LightningEffect:_update_pause( t, dt )
	self._pause_flash_time = self._pause_flash_time - dt
	if self._pause_flash_time <= 0 then
		self:_make_lightning()
		self._update_function = self._update_second
	end
end

function LightningEffect:_update_second( t, dt )
	self._second_flash_time = self._second_flash_time - dt
	if self._second_flash_time <= 0 then
		self:_set_original_values()
		self._flashing = false
	end
end

function LightningEffect:_set_original_values()
	self._sky_material:set_variable(Idstring( "color0" ), self._original_color0 )
	Global._global_light:set_color( self._original_light_color )
	Underlay:set_time( Idstring("sun_horizontal"), self._original_sun_horizontal )
end

function LightningEffect:_make_lightning()
	-- print( "LightningEffect:_make_lightning()", self._intensity_value )
	self._sky_material:set_variable( Idstring( "color0" ), self._intensity_value )
	Global._global_light:set_color( self._intensity_value )
	Underlay:set_time( Idstring("sun_horizontal"), self._flash_anim_time )
end

function LightningEffect:_set_lightning_values()
	self._first_flash_time = 0.1
	self._pause_flash_time = 0.1
	self._second_flash_time = 0.3
	self._flash_roll = math.rand( 360 )
	self._flash_dir = Rotation( 0, 0, self._flash_roll ):y()
	-- self._flash_anim_time = math.rand( 0.85, 1 )
	self._flash_anim_time = math.rand( 0, 1 )
	self._distance = math.rand( 1 )
	self._intensity_value = math.lerp( Vector3( 2,2,2 ), Vector3( 5,5,5 ), self._distance ) 
	local c_pos = managers.environment_effects:camera_position()
	if c_pos then
		local sound_speed = 30000 -- set to 300m/sec
		self._sound_delay = self._distance * 2
		-- self._sound_delay = math.rand( 1 ) -- this value multiplied with sound_speed will be the total distance to sound. (1sec = max 300m, 2sec= max 600m)
		-- self._sound_position = c_pos + self._flash_dir * (sound_speed * self._sound_delay)
		-- self._thunder_sound:set_position( self._sound_position )
		
		self._sound_source:set_rtpc( "lightning_distance", self._distance * 4000 )
	end
	
end

function LightningEffect:_set_next_timer()
	self._next = self._min_interval + math.rand( self._rnd_interval )
end

--[[RainEffect = RainEffect or class( EnvironmentEffect )

function RainEffect:init()
	EnvironmentEffect.init( self )

	self._slotmask = managers.slot:get_mask( "env_effect" )
end

function RainEffect:load_effects()
	self:set_grid()
end

function RainEffect:create_grid_table( name, x, y, sphere, ground_effect )
	local t = {}
	t.name = name
	t.pos = Vector3( x, y, 0 )
	t.sphere = sphere
	-- t.ground_effect = ground_effect
	t.elevated_offset = 250 + ( t.pos.y / 2 )
--	World:effect_manager():load( t.name )
	return t
end

function RainEffect:set_grid()
	self._grid = {}
	
	table.insert( self._grid, self:create_grid_table( "effects/particles/rain/rain_01_a", 0, 0, 50, "raindrop_01" ) )
	
	for i = 1, 2 do
		for j = -i, i, 2 do
			local step = 200
			table.insert( self._grid, self:create_grid_table( "effects/particles/rain/rain_01_b",j*step, i*step, 250, "raindrop_01" ) )
		end
	end
	
	for i = 2, 3 do
		for j = -i, i, 2 do
			local step = 300
			table.insert( self._grid, self:create_grid_table( "effects/particles/rain/rain_01_c", j*step, i*step, 350, "raindrop_01" ) )
		end
	end
	
	for i = 2, 5 do
		for j = -i, i, 2 do
			local step = 600
			table.insert( self._grid, self:create_grid_table( "effects/particles/rain/rain_01_c", j*step, i*step, 700, nil ) )
		end
	end]]
	
	--[[
	for i = 3, 5 do
		for j = -i, i, 2 do
			local step = 1000
			table.insert( self._grid, self:create_grid_table( "rain_01_e", j*step, i*step, false, nil ) )
		end
	end]]
	
	--[[ Optimized away these
	for i = 3, 6 do
		for j = -i, i, 2 do
			local step = 2000
			table.insert( self._grid, self:create_grid_table( "rain_01_e", j*step, i*step, false, nil ) )
		end
	end
	]]
	
--[[end

function RainEffect:update( t, dt )

	local c_pos = managers.environment_effects:camera_position()
	-- local c_pos = self._last_cpos
	if not c_pos then
		return
	end
	self._last_cpos = c_pos
	
	local dir = Vector3( 0, 0, -1 )
	local rot = managers.environment_effects:camera_rotation()
	if not rot then
		return
	end
	
	local rot_change = Rotation( rot:yaw(), 0, 0 )

	for i = 0, 2 do
		local t = self._grid[ self._grid_iterator ]

		if( ( i > 0 ) and t.sphere ) then
			break
		end

		local pos = c_pos + t.pos:rotate_with( rot_change )
		
		local up_ray]]
		--[[ Possible to check for roofs here
		if( t.sphere ) then
			up_ray = World:raycast( "ray", pos, pos - dir * 10000, "sphere_cast_radius", t.sphere, "bundle", 4, "slot_mask", self._slotmask )
		else
			up_ray = World:raycast( "ray", pos, pos - dir * 10000, "slot_mask", self._slotmask )
		end ]]
		--[[

		if( up_ray ) then
		 	if t.active then
				World:effect_manager():set_hidden( t.effect, true )
				World:effect_manager():set_frozen( t.effect, true )
				t.active = false
		 	end
		else
		 	if not t.active then
				World:effect_manager():set_hidden( t.effect, false )
				World:effect_manager():set_frozen( t.effect, false )
				t.active = true
		 	end
			
			if self._grid_iterator > 3 then -- First three is always moved
				World:effect_manager():move( t.effect, pos + Vector3( 0, 0, t.elevated_offset ) )
			end
		 	
		 	-- Ground effect is not added to the grid rain table (simply to avoid raycasting and playing effects for now)		 	
			if t.ground_effect then
				local ray_down = World:raycast( "ray", pos, pos + dir * 3000, "slot_mask", self._slotmask )
				if ray_down then
					World:effect_manager():spawn( { effect = t.ground_effect, position = ray_down.position, rotation = Rotation( 0, 0, math.random( 360 ) ) } )	
				end	
			end
		end

		self._grid_iterator = math.mod( self._grid_iterator, #self._grid ) + 1

		if( t.sphere ) then
			break
		end
	end

	-- Always update position of first three
	for i = 1, 3 do
		local t = self._grid[ i ]
		if t.active then
			local pos = mvector3.copy( c_pos )
			mvector3.add( pos, t.pos:rotate_with( rot_change ) )
			mvector3.set_z( pos,  mvector3.z( pos ) + t.elevated_offset )
			World:effect_manager():move( t.effect, pos )
		end
	end
	
	]]
	--[[ This moved al the rain points each frame, was very expensive
	for _,t in ipairs( self._grid ) do
		if( t.active ) then
			-- camera pos + local position rotated with camera roll rotation + elevated position based on distance
			mvector3.rotate_with( t.pos, rot_change )
			mvector3.add( pos, t.pos:rotate_with( rot_change ) )
			mvector3.set_z( pos,  mvector3.z( pos ) + t.elevated_offset )
			World:effect_manager():move( t.effect, pos )
		end
	 end
	 ]]
	 --[[
end

function RainEffect:start()
	for _,t in ipairs( self._grid ) do
		t.effect = World:effect_manager():spawn( { effect = Idstring( t.name ), position = t.pos, rotation = Rotation() } )
		t.active = true
	end
	self._grid_iterator = 1
end

function RainEffect:stop()
	for _,t in ipairs( self._grid ) do
		if t.effect then
			World:effect_manager():kill( t.effect )
			t.effect = nil
			t.active = false
		end
	end
end]]

---------------------------------------------------------------

RainDropEffect = RainDropEffect or class( EnvironmentEffect )

function RainDropEffect:init()
	EnvironmentEffect.init( self )
	
	self._under_roof = false
	self._slotmask = managers.slot:get_mask( "statics" )
end

function RainDropEffect:load_effects()

end

function RainDropEffect:update( t, dt )
	-- Update like this if disable because we are under roof
	--[[ local c_pos = managers.environment_effects:camera_position()
	if not c_pos then
		return
	end
	local dir = Vector3( 0, 0, -1 )
	local ray_up = World:raycast( "ray", c_pos, c_pos + dir * -10000, "slot_mask", self._slotmask )
	if( ray_up ) then
		if( not self._under_roof ) then
			self._under_roof = true
			World:effect_manager():set_simulator_var_float( self._raindrops, Idstring( "droplet" ), Idstring( "droplet" ), Idstring( "particle_lifetime" ), -1 ) 
			World:effect_manager():set_simulator_var_float( self._raindrops, Idstring( "running droplet" ), Idstring( "running droplet" ), Idstring( "particle_lifetime" ), -1 )
			
			World:effect_manager():set_simulator_var_float( self._extra_raindrops, Idstring( "droplet" ), Idstring( "droplet" ), Idstring( "particle_lifetime" ), -1 ) 	
			World:effect_manager():set_simulator_var_float( self._extra_raindrops, Idstring( "running droplet" ), Idstring( "running droplet" ), Idstring( "particle_lifetime" ), -1 ) 
		end
	else
		if( self._under_roof ) then
			self._under_roof = false
			World:effect_manager():set_simulator_var_float( self._raindrops, Idstring( "droplet" ), Idstring( "droplet" ), Idstring( "particle_lifetime" ), 5 ) 
			World:effect_manager():set_simulator_var_float( self._raindrops, Idstring( "running droplet" ), Idstring( "running droplet" ), Idstring( "particle_lifetime" ), 5 )
		end
		
		if( dir:normalized():dot( managers.environment_effects:camera_rotation():y() ) < -0.2 ) then
			World:effect_manager():set_simulator_var_float( self._extra_raindrops, Idstring( "droplet" ), Idstring( "droplet" ), Idstring( "particle_lifetime" ), 5 ) 	
			World:effect_manager():set_simulator_var_float( self._extra_raindrops, Idstring( "running droplet" ), Idstring( "running droplet" ), Idstring( "particle_lifetime" ), 5 ) 
		else
			World:effect_manager():set_simulator_var_float( self._extra_raindrops, Idstring( "droplet" ), Idstring( "droplet" ), Idstring( "particle_lifetime" ), -1 ) 	
			World:effect_manager():set_simulator_var_float( self._extra_raindrops, Idstring( "running droplet" ), Idstring( "running droplet" ), Idstring( "particle_lifetime" ), -1 ) 
		end
	end]]
end

function RainDropEffect:start()
	local t = { effect = self._effect_name, position = Vector3(), rotation = Rotation() }
	self._raindrops = World:effect_manager():spawn( t )
	self._extra_raindrops = World:effect_manager():spawn( t )
end

function RainDropEffect:stop()
	if self._raindrops then
		World:effect_manager():fade_kill( self._raindrops )
		World:effect_manager():fade_kill( self._extra_raindrops )
		self._raindrops = nil
	end
end

RainDropScreenEffect = RainDropScreenEffect or class( RainDropEffect )

function RainDropScreenEffect:init()
	RainDropEffect.init( self )

	self._effect_name = Idstring( "effects/particles/rain/raindrop_screen" )
end

---------------------------------------------------------------
--[[
LightningEffect = LightningEffect or class( EnvironmentEffect )

function LightningEffect:init()
	EnvironmentEffect.init( self )
	self._lightning_environment = "lightning"
end

function LightningEffect:load_effects()
	managers.environment:preload_environment( self._lightning_environment )
end

function LightningEffect:update( t, dt )
	if self._flashing then
		self:_update_function( t, dt )
	end
	
	if self._sound_delay then
		self._sound_delay = self._sound_delay - dt
		if self._sound_delay <= 0 then
			self._sound_delay = nil
			self._thunder_sound:play()
		end
	end
	
	self._next = self._next - dt
	if self._next <= 0 then
		self:_set_lightning_values()
		self:_make_lightning()
		self._update_function = self._update_first
		self:_set_next_timer()
		self._flashing = true
	end
end

function LightningEffect:_update_first( t, dt )
	self._first_flash_time = self._first_flash_time - dt
	if self._first_flash_time <= 0 then
		-- managers.environment:set( self._environment )
		managers.environment_area:set_to_current_environment()
		local sky = managers.environment:get_sky()
	--	sky:set_value( "rotation", self._original_rotation )
		managers.environment:setrotation( self._original_rotation  )
		self._update_function = self._update_pause
	end
end

function LightningEffect:_update_pause( t, dt )
	self._pause_flash_time = self._pause_flash_time - dt
	if self._pause_flash_time <= 0 then
		self:_make_lightning()
		self._update_function = self._update_second
	end
end

function LightningEffect:_update_second( t, dt )
	self._second_flash_time = self._second_flash_time - dt
	if self._second_flash_time <= 0 then
--		managers.environment:set( self._environment )
		managers.environment_area:set_to_current_environment()
		local sky = managers.environment:get_sky()
		-- sky:set_value( "rotation", self._original_rotation )
		managers.environment:setrotation( self._original_rotation )
		self._flashing = false
	end
end

function LightningEffect:_make_lightning()
	managers.environment:set( self._lightning_environment )
	local sky = managers.environment:get_sky()
--	sky:set_value( "rotation", self._flash_roll )
	managers.environment:setrotation( self._flash_roll  )
end

function LightningEffect:_set_lightning_values()
	self._first_flash_time = 0.1
	self._pause_flash_time = 0.1
	self._second_flash_time = 0.3
	self._flash_roll = math.rand( 360 )
	self._flash_dir = Rotation( 0, 0, self._flash_roll ):y()
	local camera = managers.viewport:get_current_camera()
	if camera then
		local sound_speed = 30000 -- set to 300m/sec
		self._sound_delay = math.rand( 1 ) -- this value multiplied with sound_speed will be the total distance to sound. (1sec = max 300m, 2sec= max 600m)
		self._sound_position = camera:position() + self._flash_dir * (sound_speed * self._sound_delay)
		self._thunder_sound:set_position( self._sound_position )
	end
	
end

function LightningEffect:_set_next_timer()
	self._next = self._min_interval + math.rand( self._rnd_interval )
end

function LightningEffect:start()
	local sky = managers.environment:get_sky()
	self._original_rotation = managers.environment:rotation()
	self._min_interval = 2
	self._rnd_interval = 10
	self._environment = managers.environment:get()
	Sound:add_soundbank ( "thunder_sound" )
	self._thunder_sound = Sound:make_bank( "thunder_sound", "thunder_clap" )
	self:_set_next_timer()
end

function LightningEffect:stop()
	
end
]]

CoreClass.override_class( CoreEnvironmentEffectsManager.EnvironmentEffectsManager, EnvironmentEffectsManager )