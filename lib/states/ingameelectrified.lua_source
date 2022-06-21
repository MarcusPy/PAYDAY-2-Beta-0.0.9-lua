require "lib/states/GameState"

IngameElectrifiedState = IngameElectrifiedState or class( IngamePlayerBaseState )

function IngameElectrifiedState:init( game_state_machine )
	IngamePlayerBaseState.super.init( self, "ingame_electrified", game_state_machine )
end

function IngameElectrifiedState:update( t, dt )
	local player = managers.player:player_unit()
	
	if not alive( player ) then
		return
	end
		
	--[[if player:character_damage():update_downed( t, dt ) then
		managers.statistics:downed( { death = true } )
		IngameFatalState.client_died()
	
		player:base():set_enabled( false )
		game_state_machine:change_state_by_name( "ingame_waiting_for_respawn" )
		player:character_damage():set_invulnerable( true )
		player:base():_unregister() -- remove from GroupAI criminals list
		player:base():set_slot( player, 0 )
	end]]
	
end

function IngameElectrifiedState:at_enter()
	local players = managers.player:players()
	
	for k, player in ipairs( players ) do
		local vp = player:camera():viewport()
		
		if( vp ) then
			vp:set_active( true )
		else
			Application:error( "No viewport for player " .. tostring( k ) )	
		end
	end

	-- managers.statistics:downed( { bleed_out = true } )
	
	local player = managers.player:player_unit()
	if player then
		player:base():set_enabled( true )
	end

	managers.hud:show( PlayerBase.PLAYER_INFO_HUD )
	managers.hud:show( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )
	-- managers.hud:show( PlayerBase.PLAYER_DOWNED_HUD )	
end

function IngameElectrifiedState:at_exit()
	local player = managers.player:player_unit()
	if player then
		player:base():set_enabled( false )
	end

	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD )
	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )
	-- managers.hud:hide( PlayerBase.PLAYER_DOWNED_HUD )
end

function IngameElectrifiedState:on_server_left()
	IngameCleanState.on_server_left( self )
end

function IngameElectrifiedState:on_kicked()
	IngameCleanState.on_kicked( self )
end

function IngameElectrifiedState:on_disconnected()
	IngameCleanState.on_disconnected( self )
end