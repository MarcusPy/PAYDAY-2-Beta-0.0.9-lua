require "lib/states/GameState"

ServerLeftState = ServerLeftState or class( MissionEndState )


function ServerLeftState:init( game_state_machine, setup )
	-- GameState.init( self, "gameoverscreen", game_state_machine )
	ServerLeftState.super.init( self, "server_left", game_state_machine, setup )
	
	--[[if( managers.hud ) then
		self._setup = true
		managers.hud:load_hud( self.GUI_SAFERECT, false, true, true, {} )
		managers.hud:load_hud( self.GUI_FULLSCREEN, false, true, false, {} )
	end

	self._exit_cb = callback( self, self, "exit" )
	self._controller = nil]]
end


function ServerLeftState:at_enter( ... )
	self._success = false
	self._server_left = true
	self._completion_bonus_done = true
	ServerLeftState.super.at_enter( self, ... )
		
	-- Stop network in this earliy state, otherwise server has left state might be triggered again
	if( Network:multiplayer() ) then
		self:_shut_down_network()
	end
	
	self:_create_server_left_dialog()
end

------------------------------------------------------------------------------------------
--[[
function ServerLeftState:at_exit()
	managers.hud:hide( self.GUI_SAFERECT )
	managers.hud:hide( self.GUI_FULLSCREEN )
	self:_clear_camera()]]
	--[[
	if not self._debug_continue then
		-- Should go to menu or load next chapter here
	end
	self._debug_continue = nil
	]]
	
	--[[
	if Network:is_server() then
		managers.groupai:state():set_AI_enabled( true )
	end
	]]
-- end

------------------------------------------------------------------------------------------

--[[ function ServerLeftState:continue()
	if( self._old_state ) then
		self:_clear_controller()
		
		cat_debug( "spam", "Going from GameOverState to " .. self._old_state:name() )
		self:gsm():change_state_by_name( self._old_state:name() )
	else
		Application:error( "Trying to continue from game over screen, but I have no state to goto" )
	end
end

------------------------------------------------------------------------------------------

function ServerLeftState:_clear_controller()
	self._controller:remove_trigger( "gameover_exit", self._exit_cb )
	self._controller:set_enabled( false )
	self._controller:destroy()
	self._controller = nil
end

------------------------------------------------------------------------------------------

function ServerLeftState:update( t, dt )
	managers.hud:script( self.GUI_SAFERECT ):update( t, dt )
	
	if t > self._exit_time and not self._has_exited then
		self:exit()
	end
end

------------------------------------------------------------------------------------------

function ServerLeftState:_setup_camera()
	self._camera_object = World:create_camera()
	self._camera_object:set_near_range( 3 )
	self._camera_object:set_far_range( 1000000 )
	self._camera_object:set_fov( 75 )

	self._viewport = managers.viewport:new_vp( 0, 0, 1, 1, "spectator", CoreManagerBase.PRIO_WORLDCAMERA )
	self._viewport:set_camera( self._camera_object )
	self._viewport:set_environment( managers.environment_area:default_environment() )
	self._viewport:set_active( true )
end

------------------------------------------------------------------------------------------

function ServerLeftState:_clear_camera()
	if self._viewport then
		self._viewport:destroy()
		self._viewport = nil
	end
	
	if self._camera_object then
		World:delete_camera( self._camera_object )
		self._camera_object = nil
	end
end]]

------------------------------------------------------------------------------------------

function ServerLeftState:on_server_left()
	-- Prevent server left transition when we allready are in server left state
end

function ServerLeftState:_create_server_left_dialog()
	MenuMainState._create_server_left_dialog( self )
end

function ServerLeftState:_load_start_menu()
	if not (managers.job:stage_success() and managers.job:on_last_stage()) then
		setup:load_start_menu()
	end
end

------------------------------------------------------------------------------------------

function ServerLeftState:on_server_left_ok_pressed()
	self._completion_bonus_done = true
	self:_set_continue_button_text()
end

------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------