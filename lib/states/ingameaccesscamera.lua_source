require "lib/states/GameState"

IngameAccessCamera = IngameAccessCamera or class( IngamePlayerBaseState )
IngameAccessCamera.GUI_SAFERECT = Idstring( "guis/access_camera_saferect" )
IngameAccessCamera.GUI_FULLSCREEN = Idstring( "guis/access_camera_fullrect" )

function IngameAccessCamera:init( game_state_machine )
	IngameAccessCamera.super.init( self, "ingame_access_camera", game_state_machine )
end

function IngameAccessCamera:_setup_controller()
	self._controller = managers.controller:create_controller( "ingame_access_camera", managers.controller:get_default_wrapper_index(), false )
	self._leave_cb = callback( self, self, "cb_leave" )
	self._prev_camera_cb = callback( self, self, "_prev_camera" )
	self._next_camera_cb = callback( self, self, "_next_camera" )
	self._controller:add_trigger( "jump", self._leave_cb )
	self._controller:add_trigger( "primary_attack", self._prev_camera_cb )
	self._controller:add_trigger( "secondary_attack", self._next_camera_cb )
	-- self._next_player_cb = callback( self, self, "cb_next_player" )
	-- self._prev_player_cb = callback( self, self, "cb_prev_player" )
	-- self._controller:add_trigger( "left", self._prev_player_cb )
	-- self._controller:add_trigger( "right", self._next_player_cb )
	-- self._controller:add_trigger( "primary_attack", self._prev_player_cb )
	-- self._controller:add_trigger( "secondary_attack", self._next_player_cb )	
	
	self._controller:set_enabled( true )
end

------------------------------------------------------------------------------------------

function IngameAccessCamera:_clear_controller()
	if self._controller then
		self._controller:remove_trigger( "jump", self._leave_cb )
		self._controller:remove_trigger( "primary_attack", self._prev_camera_cb )
		self._controller:remove_trigger( "secondary_attack", self._next_camera_cb )
		-- self._controller:remove_trigger( "left", self._prev_player_cb )
		-- self._controller:remove_trigger( "right", self._next_player_cb )
		-- self._controller:remove_trigger( "primary_attack", self._prev_player_cb )
		-- self._controller:remove_trigger( "secondary_attack", self._next_player_cb )
		self._controller:set_enabled( false )
		self._controller:destroy()
		self._controller = nil
	end
end

function IngameAccessCamera:set_controller_enabled( enabled )
	if self._controller then
		self._controller:set_enabled( enabled )
	end
end

function IngameAccessCamera:cb_leave()
	game_state_machine:change_state_by_name( self._old_state )
end

function IngameAccessCamera:_get_cameras()
	self._cameras = {}
	
	for _,script in pairs( managers.mission:scripts() ) do
		local access_cameras = script:element_group( "ElementAccessCamera" )
		if access_cameras then
			for _,access_camera in ipairs( access_cameras ) do
				-- if access_camera:enabled() then -- Take all camera, skip it when accessing next camera instead 
					-- local pos = access_camera:value( "position" )
					-- local rot = access_camera:value( "rotation" )
					
					table.insert( self._cameras, { access_camera = access_camera } )
				-- end
			end
		end
	end
	
	
--	for _,unit in ipairs( managers.helper_unit:get_units_by_type( "waiting_camera" ) ) do
--		table.insert( self._cameras, { pos = unit:position(), rot = unit:rotation(), nr = math.random( 20 ) } )
--	end
	
	--[[if #self._cameras == 0 then	
		-- Get camera position and rotations from world
		table.insert( self._cameras, { pos = Vector3(-196, -496, 851), rot = Rotation(90, 0, -0), nr = math.random( 20 ) } )
		table.insert( self._cameras, { pos = Vector3(-1897, -349, 365), rot = Rotation(0, 0, -0), nr = math.random( 20 ) } )
		table.insert( self._cameras, { pos = Vector3(-2593, 552, 386), rot = Rotation(-90, 0, 0), nr = math.random( 20 ) } )
	end]]
end

function IngameAccessCamera:_next_index()
	self._camera_data.index = self._camera_data.index + 1
	if self._camera_data.index > #self._cameras then
		self._camera_data.index = 1
	end
	
	if not self._cameras[ self._camera_data.index ].access_camera:enabled() then
		self:_next_index()
	end
end

function IngameAccessCamera:_prev_index()
	self._camera_data.index = self._camera_data.index - 1
	if self._camera_data.index < 1 then
		self._camera_data.index = #self._cameras
	end
	
	if not self._cameras[ self._camera_data.index ].access_camera:enabled() then
		self:_prev_index()
	end
end

function IngameAccessCamera:_prev_camera()
	if self._no_feeds then
		return
	end
	self:_prev_index()
	-- self._do_show_camera = true
	self:_show_camera()
end

function IngameAccessCamera:_next_camera()
	if self._no_feeds then
		return
	end
	self:_next_index()
	-- self._do_show_camera = true
	self:_show_camera()
end

function IngameAccessCamera:on_destroyed()
	local access_camera = self._cameras[ self._camera_data.index ].access_camera
	managers.hud:set_access_camera_destroyed( access_camera:value( "destroyed" ) )
end

function IngameAccessCamera:_show_camera()
	-- print( "self._camera_data.index", self._camera_data.index )
	self._sound_source:post_event( "camera_monitor_change" )
	if self._last_access_camera then
		self._last_access_camera:remove_trigger( "IngameAccessCamera", "destroyed" )
	end
	local access_camera = self._cameras[ self._camera_data.index ].access_camera
	access_camera:add_trigger( "IngameAccessCamera", "destroyed", callback( self, self, "on_destroyed" ) )
	self._last_access_camera = access_camera
	 
	-- print( "IngameAccessCamera:_next_camera()", self._camera_data.index, access_camera._id, access_camera:value( "text_id" ), access_camera:value( "destroyed" ) )
		access_camera:trigger_accessed( managers.player:player_unit() )
	
	self._cam_unit:set_position( access_camera:camera_position() )
	self._cam_unit:camera():set_rotation( access_camera:value( "rotation" ) ) -- Stores the rotation to be able to modify it
	self._cam_unit:camera():start( math.rand( 30 ) )
	
	self._yaw = 0
	self._pitch = 0
	self._target_yaw = 0
	self._target_pitch = 0
	
	self._yaw_limit = access_camera:value( "yaw_limit" ) or 25
	self._pitch_limit = access_camera:value( "pitch_limit" ) or 25 
	
	-- self._cam_unit:camera():set_destroyed( access_camera:value( "destroyed" ) )
	managers.hud:set_access_camera_destroyed( access_camera:value( "destroyed" ) )
	
	-- self._cam_unit:camera()._viewport:vp():set_post_processor_effect( "World", Idstring("color_grading_post"), Idstring( "color_sin" ) )
		
	local text_id = access_camera:value( "text_id" ) ~= "debug_none" and access_camera:value( "text_id" ) or "hud_cam_access_camera_test_generated"
	local number = (self._camera_data.index < 10 and "0" or "")..self._camera_data.index
	managers.hud:set_access_camera_name( managers.localization:text( text_id, { NUMBER = number } ) )
	-- local text = self._cameras[ self._camera_data.index ].nr < 10 and "0"..self._cameras[ self._camera_data.index ].nr or self._cameras[ self._camera_data.index ].nr
	-- managers.hud:script( self.GUI_SAFERECT ).cam_nr:set_text( "CAM-"..text )
end

function IngameAccessCamera:update( t, dt )
	if self._no_feeds then
		return
	end
	
	t = managers.player:player_timer():time()
	dt = managers.player:player_timer():delta_time()
	
	-- Modify camera rotation
	local look_d = self._controller:get_input_axis( "look" )
	local zoomed_value = self._cam_unit:camera():zoomed_value()
	self._target_yaw = self._target_yaw - look_d.x * zoomed_value
	if self._yaw_limit ~= -1 then
		self._target_yaw = math.clamp( self._target_yaw, -self._yaw_limit, self._yaw_limit )
	end 
	self._target_pitch = self._target_pitch + look_d.y * zoomed_value
	if self._pitch_limit ~= -1 then
		self._target_pitch = math.clamp( self._target_pitch + look_d.y * zoomed_value, -self._pitch_limit, self._pitch_limit )
	end
		
	self._yaw = math.step( self._yaw, self._target_yaw, dt * 10 )
	self._pitch =  math.step( self._pitch, self._target_pitch, dt * 10 )
	
	-- self._cam_unit:camera():set_offset_rotation( Rotation( self._yaw, self._pitch, 0 ) )
	self._cam_unit:camera():set_offset_rotation( self._yaw, self._pitch )
		
	-- Modify fov/zoom
	local move_d = self._controller:get_input_axis( "move" )
	self._cam_unit:camera():modify_fov( - move_d.y * (dt * 12) )
		
	-- print( self._cam_unit:camera()._viewport:vp():get_post_processor_effect( "World", Idstring("color_grading_post") ) ) 
	if self._do_show_camera then
		self._do_show_camera = false
		local access_camera = self._cameras[ self._camera_data.index ].access_camera
		managers.hud:set_access_camera_destroyed( access_camera:value( "destroyed" ) )
		-- self._cam_unit:camera()._viewport:vp():set_post_processor_effect( "World", Idstring("color_grading_post"), Idstring( "color_sin" ) )
	end
	-- Shouldn't need to set this every frame.
	-- self._cam_unit:camera()._viewport:vp():set_post_processor_effect( "World", Idstring("color_grading_post"), Idstring( "color_sin" ) )
	
	-- Add face trackers
	local units = World:find_units_quick( "all", 3, 16, 21, managers.slot:get_mask( "enemies" ) )
	local amount = 0
			-- local brush = Draw:brush( Color.red:with_alpha( 0.5 ) )  
	for i,unit in ipairs( units ) do
		-- if unit:movement():m_head_pos()
		if World:in_view_with_options( unit:movement():m_head_pos(), 0, 0, 4000 ) then
			local ray
			
			if self._last_access_camera and self._last_access_camera:has_camera_unit() then
				ray = self._cam_unit:raycast( "ray", unit:movement():m_head_pos(), self._cam_unit:position(), "slot_mask", managers.slot:get_mask( "world_geometry" ), "ignore_unit", self._last_access_camera:camera_unit(), "report" )
			else
				ray = self._cam_unit:raycast( "ray", unit:movement():m_head_pos(), self._cam_unit:position(), "slot_mask", managers.slot:get_mask( "world_geometry" ), "report" )
			end
			
			
			if not ray then
				amount = amount + 1
				managers.hud:access_camera_track( amount, self._cam_unit:camera()._camera, unit:movement():m_head_pos() )
				if managers.player:upgrade_value( "player", "sec_camera_highlight", false ) then
					if managers.groupai:state():whisper_mode() then
						if tweak_data.character[ unit:base()._tweak_table ].silent_priority_shout then
							self:add_enemy_contour( unit )
						end
					end
				end
			end
		end
	end
	
	-- self:update_player_stamina( t, dt )
	
	managers.hud:access_camera_track_max_amount( amount )
end

function IngameAccessCamera:add_enemy_contour( unit )
	if self._enemy_contours[ unit:key() ] then
		if self._enemy_contours[ unit:key() ] > Application:time() then
			return
		end
	end
	
	self._enemy_contours[ unit:key() ] = Application:time() + 9
	managers.game_play_central:add_enemy_contour( unit, false )
	managers.network:session():send_to_peers_synched( "mark_enemy", unit, false )
end

function IngameAccessCamera:update_player_stamina( t, dt )
	local player = managers.player:player_unit()
	
	if player and player:movement() then
		player:movement():update_stamina( t, dt, true )
	end
end

function IngameAccessCamera:_player_damage( info )
	self:cb_leave()
end

function IngameAccessCamera:at_enter( old_state, ... )	
	local player = managers.player:player_unit()
	if player then
		player:base():set_enabled( false )
		player:base():set_visible( false )
		player:character_damage():add_listener( "IngameAccessCamera", { "hurt", "death" }, callback( self, self, "_player_damage" )  )
	
		SoundDevice:set_rtpc( "stamina", 100 )
	end
		
	self._sound_source = self._sound_source or SoundDevice:create_source( "IngameAccessCamera" )
	self._sound_source:post_event( "camera_monitor_engage" )
	
	-- Disable AI LOD
	managers.enemy:set_gfx_lod_enabled( false )
			
	self._old_state = old_state:name()
	
	if not managers.hud:exists( self.GUI_SAFERECT ) then
		managers.hud:load_hud( self.GUI_FULLSCREEN, false, true, false, {} )
		managers.hud:load_hud( self.GUI_SAFERECT, false, true, true, {} )
	end
	managers.hud:show( self.GUI_SAFERECT )
	managers.hud:show( self.GUI_FULLSCREEN )
	managers.hud:start_access_camera()
	
	--[[managers.hud:hide( PlayerBase.PLAYER_HUD )
	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD )
	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )]]
	
	self._saved_default_color_grading = managers.environment_controller:default_color_grading()
	managers.environment_controller:set_default_color_grading( "color_sin" )
	
	self._cam_unit = CoreUnit.safe_spawn_unit( "units/gui/background_camera_01/access_camera", Vector3(), Rotation() )
	-- self._cam_unit:camera():start( 0 )
	
	
	-- self._cam_unit:camera()._viewport:vp():set_post_processor_effect( "World", Idstring("color_grading_post"), Idstring( "color_sin" ) )
	-- self._vp:vp():set_post_processor_effect( "World", Idstring("color_grading_post"), Idstring( managers.user:get_setting( "video_color_grading" ) ) )
	
	self:_get_cameras()
	self._camera_data = {}
	self._camera_data.index = 0
	self._no_feeds = not self:_any_enabled_cameras()
	if self._no_feeds then
		managers.hud:set_access_camera_destroyed( true, true )
	else
		self:_next_camera()
	end
	
	self._enemy_contours = {}
	self:_setup_controller()
end

function IngameAccessCamera:_any_enabled_cameras()
	if not self._cameras or #self._cameras == 0 then
		return false
	end
	
	for _,data in ipairs( self._cameras ) do
		if data.access_camera:enabled() then
			return true
		end
	end
	
	return false
end

function IngameAccessCamera:at_exit()
	self._sound_source:post_event( "camera_monitor_leave" )
	-- self._sound_source:delete()
		
	managers.environment_controller:set_default_color_grading( self._saved_default_color_grading )
	
	-- Enable AI LOD
	managers.enemy:set_gfx_lod_enabled( true )
	
	self:_clear_controller()
	
	World:delete_unit( self._cam_unit )
	
	managers.hud:hide( self.GUI_SAFERECT )
	managers.hud:hide( self.GUI_FULLSCREEN )
	managers.hud:stop_access_camera()
	
	-- managers.environment_controller:set_dof_distance()
	--[[managers.hud:hide( PlayerBase.PLAYER_HUD )
	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD )
	managers.hud:hide( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )]]
	
	if self._last_access_camera then
		self._last_access_camera:remove_trigger( "IngameAccessCamera", "destroyed" )
	end
		
	local player = managers.player:player_unit()
	if player then
		player:base():set_enabled( true )
		player:base():set_visible( true )
		player:character_damage():remove_listener( "IngameAccessCamera" )
	end
	
end

function IngameAccessCamera:on_server_left()
	IngameCleanState.on_server_left( self )
end

function IngameAccessCamera:on_kicked()
	IngameCleanState.on_kicked( self )
end

function IngameAccessCamera:on_disconnected()
	IngameCleanState.on_disconnected( self )
end