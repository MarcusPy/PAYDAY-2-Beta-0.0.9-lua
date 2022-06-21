require "lib/states/GameState"

IngameBleedOutState = IngameBleedOutState or class( IngamePlayerBaseState )

function IngameBleedOutState:init( game_state_machine )
	IngameBleedOutState.super.init( self, "ingame_bleed_out", game_state_machine )
end

function IngameBleedOutState:update( t, dt )
	local player = managers.player:player_unit()
	
	if not alive( player ) then
		return
	end
		
	if player:movement():nav_tracker() and player:character_damage():update_downed( t, dt ) then
		managers.player:force_drop_carry()
		
		managers.statistics:downed( { death = true } )
		IngameFatalState.client_died()
	
		player:base():set_enabled( false )
		game_state_machine:change_state_by_name( "ingame_waiting_for_respawn" )
		player:character_damage():set_invulnerable( true )
		player:character_damage():set_health( 0 )
		player:base():_unregister() -- remove from GroupAI criminals list
		player:base():set_slot( player, 0 )
	end
	
end

function IngameBleedOutState:at_enter()
	local players = managers.player:players()
	
	for k, player in ipairs( players ) do
		local vp = player:camera():viewport()
		
		if( vp ) then
			vp:set_active( true )
		else
			Application:error( "No viewport for player " .. tostring( k ) )	
		end
	end

	managers.statistics:downed( { bleed_out = true } )
	local player = managers.player:player_unit()
	if player then
		player:base():set_enabled( true )
	end

		--[[
	if Global.game_settings.single_player then
		local title = managers.localization:text( "hud_payback" )
		local type_text = managers.localization:text( "hud_payback" )
		local text = managers.localization:text( "hud_payback" ) 
		
		local icon = nil
		managers.hud:present_mid_text( { text = text, title = title, icon = icon, time = 1.5, event = "stinger_objectivecomplete" } )
	end]]

	
	managers.hud:show( PlayerBase.PLAYER_INFO_HUD )
	managers.hud:show( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )
	managers.hud:show( PlayerBase.PLAYER_DOWNED_HUD )	
end

--[[function IngameBleedOutState:pause_timer()
	self._timer_paused = true
	managers.hud:pd_pause_timer()
end

function IngameBleedOutState:unpause_timer()
	self._timer_paused = false
	managers.hud:pd_unpause_timer()
end]]

function IngameBleedOutState:at_exit()
	managers.challenges:reset( "exit_bleed_out" )
	
	
	
	local player = managers.player:player_unit()
	if player then
		player:base():set_enabled( false )
		
		
	end

	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD )
	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )
	managers.hud:hide( PlayerBase.PLAYER_DOWNED_HUD )
	--[[
	local players = managers.player:players()

	for _, player in ipairs( players ) do
		local vp = player:camera():viewport()
		
		if( vp ) then
			vp:set_active( false )
		end
	end
	]]
	-- managers.hud:hide( self._MASK_OFF_HUD )
	
	-- self._ws:hide()
end

function IngameBleedOutState:on_server_left()
	IngameCleanState.on_server_left( self )
end

function IngameBleedOutState:on_kicked()
	IngameCleanState.on_kicked( self )
end

function IngameBleedOutState:on_disconnected()
	IngameCleanState.on_disconnected( self )
end

