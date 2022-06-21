core:import( "CoreUnit" )

require "lib/states/GameState"

IngameWaitingForPlayersState = IngameWaitingForPlayersState or class( GameState )
IngameWaitingForPlayersState.GUI_SAFERECT = Idstring( "guis/waiting_saferect" )
IngameWaitingForPlayersState.GUI_FULLSCREEN = Idstring( "guis/waiting_fullscreen" )
IngameWaitingForPlayersState.PLAYER_HUD = Idstring( "guis/player_hud" )
IngameWaitingForPlayersState.PLAYER_INFO_HUD = Idstring( "guis/player_info_hud" )
IngameWaitingForPlayersState.PLAYER_INFO_HUD_FULLSCREEN = Idstring( "guis/player_info_hud_fullscreen" )
IngameWaitingForPlayersState.PLAYER_DOWNED_HUD = Idstring( "guis/player_downed_hud" )
IngameWaitingForPlayersState.LEVEL_INTRO_GUI = Idstring( "guis/level_intro" )

function IngameWaitingForPlayersState:init( game_state_machine )
	GameState.init( self, "ingame_waiting_for_players", game_state_machine )
		
	self._intro_source = SoundDevice:create_source( "intro_source" )
	
	self._start_cb = callback( self, self, "_start" )
	self._skip_cb = callback( self, self, "_skip" )
	self._controller = nil
end

function IngameWaitingForPlayersState:setup_controller()
	if( not self._controller ) then
		self._controller = managers.controller:create_controller( "waiting_for_players", managers.controller:get_default_wrapper_index(), false )
		-- self._controller:add_trigger( "continue", self._skip_cb )
	end
	
	self._controller:set_enabled( true )
end

function IngameWaitingForPlayersState:set_controller_enabled( enabled )
	if self._controller then
	--	self._controller:set_enabled( enabled )
	end
end

function IngameWaitingForPlayersState:_skip()
	if not Network:is_server() then
		return
	end
	
	if not self._audio_started then
		return
	end
	
	if self._skipped then
		return
	end
	
	self:sync_skip()
	managers.network:session():send_to_peers_synched( "sync_waiting_for_player_skip" )
end

function IngameWaitingForPlayersState:sync_skip()
	print( "SKIP" )
	
	self._skipped = true
	-- self._intro_source:stop()
	managers.briefing:stop_event( true )
	self:_start_delay()
end

function IngameWaitingForPlayersState:_start()
	if not Network:is_server() then
		return
	end

	local variant = managers.groupai:state():blackscreen_variant() or 0
	self:sync_start( variant )
	managers.network:session():send_to_peers_synched( "sync_waiting_for_player_start", variant )
end

function IngameWaitingForPlayersState:sync_start( variant )
	self._kit_menu.renderer:set_all_items_enabled( false )
	-- self:_clear_controller()
	
	self._briefing_start_t = nil
	managers.briefing:stop_event()
	
	managers.music:post_event( tweak_data.levels:get_music_event( "intro" ) )
	self._fade_out_id = managers.overlay_effect:play_effect( tweak_data.overlay_effects.fade_out_permanent )
	local level_data = Global.level_data.level_id and tweak_data.levels[ Global.level_data.level_id ]
	self._intro_text_id = level_data and level_data.intro_text_id
	self._intro_event = level_data and ( variant == 0 and level_data.intro_event or level_data.intro_event[ variant ] )
	-- self._intro_cues = level_data and ( variant == 0 and level_data.intro_cues or level_data.intro_cues[ variant ] )
	
	self._blackscreen_started = true
	managers.menu_component:close_asset_mission_briefing_gui()
	
	if self._intro_event then -- Start audio event sequence
		self._delay_audio_t = Application:time() + 1
	else 					-- Start spawn sequence
		self:_start_delay()
	end
end

function IngameWaitingForPlayersState:blackscreen_started()
	return self._blackscreen_started or false
end

function IngameWaitingForPlayersState:_start_audio()
	managers.hud:show( self.LEVEL_INTRO_GUI )
	managers.hud:set_blackscreen_mid_text( self._intro_text_id and managers.localization:text( self._intro_text_id ) or "" )
	managers.hud:set_blackscreen_job_data()
	managers.hud:blackscreen_fade_in_mid_text()
	self._intro_cue_index = 1
	self._audio_started = true
	managers.menu:close_menu( "kit_menu" )	-- Close menu here allready to avoid having visible mouse pointer
	
	local event_started
	local job_data = managers.job:current_job_data()
	if job_data and managers.job:current_job_id() == "safehouse" and Global.mission_manager.saved_job_values.playedSafeHouseBefore then
	else
		event_started = managers.briefing:post_event( self._intro_event, { show_subtitle = true, listener = { clbk=callback( self, self, "_audio_done" ), end_of_event=true } } )
	end
	if not event_started then
		print( "failed to start audio, or played safehouse before" )
		if Network:is_server() then
			self:_start_delay()
		end
	end
	
	--[[if not self._intro_source:post_event( self._intro_event, self._audio_done, self, "marker", "end_of_event" ) then
		print( "failed to start audio" )
		if Network:is_server() then
			self:_start_delay()
		end
	end]]
end

function IngameWaitingForPlayersState:_start_delay()
	if self._delay_start_t then
		return
	end
	self._delay_start_t = Application:time() + 1
end

-- function IngameWaitingForPlayersState:_audio_done( instance, event_type, self, sound_source, label, identifier, position )
function IngameWaitingForPlayersState:_audio_done( event_type, label, cookie )
	self:_start_delay()
--[[
	if event_type == "end_of_event" or ( event_type == "marker" and sound_source and sound_source == "end" ) then
		self:_start_delay()
		-- managers.subtitle:set_visible( false )
		-- managers.subtitle:set_enabled( false )
	else
		managers.subtitle:set_visible( true )
		managers.subtitle:set_enabled( true )
				
		if self._intro_cues then
			local cue = managers.drama:cue( self._intro_cues[ self._intro_cue_index ] )		-- next line in cue table
			managers.subtitle:show_subtitle( cue.string_id, cue.duration )
			self._intro_cue_index = self._intro_cue_index + 1
		end
	end]]
end

function IngameWaitingForPlayersState:_briefing_callback( event_type, label, cookie )
	print( "[IngameWaitingForPlayersState]", "event_type", event_type, "label", label, "cookie", cookie )
	
	managers.menu_component:set_mission_briefing_description( label )
end

--[[function IngameWaitingForPlayersState:_clear_controller()
	self._controller:remove_trigger( "continue", self._start_cb )
	self._controller:set_enabled( false )
	self._controller:destroy()
	self._controller = nil
end]]

function IngameWaitingForPlayersState:update( t, dt )
	if t > self._camera_data.next_t then
		self:_next_camera()
	end
	
	if self._briefing_start_t then
		if self._briefing_start_t < t then
			self._briefing_start_t = nil
			if managers.job:has_active_job() then
				local stage_data = managers.job:current_stage_data()
				local level_data = managers.job:current_level_data()
				local briefing_dialog = stage_data.briefing_dialog or level_data.briefing_dialog
				
				if type( briefing_dialog ) == "table" then
					briefing_dialog = briefing_dialog[ math.random( #briefing_dialog ) ]
				end
				
				local job_data = managers.job:current_job_data()
				if job_data and managers.job:current_job_id() == "safehouse" and Global.mission_manager.saved_job_values.playedSafeHouseBefore then
				else
					managers.briefing:post_event( briefing_dialog, { show_subtitle = false, listener = { clbk=callback( self, self, "_briefing_callback" ), marker=false } } )
				end
				-- if briefing_dialog then
				-- 	managers.dialog:queue_dialog( briefing_dialog, { skip_idle_check=true } )
				-- end
			end
		end
	end
	-- managers.hud:script( self.GUI_SAFERECT ).date:set_text( Application:date( "%Y-%m-%d %H:%M:%S") )
	
	if self._delay_audio_t then
		if t > self._delay_audio_t then
			self._delay_audio_t = nil
			self:_start_audio()
		end
	end
	
	if self._delay_start_t then
		if t > self._delay_start_t then
			self._delay_start_t = nil
			-- self._started_from_beginning = true
			managers.hud:blackscreen_fade_out_mid_text()
			-- managers.music:post_event( tweak_data.levels:get_music_event( "intro" ) )
			if Network:is_server() then
				self._delay_spawn_t = Application:time() + 1
			end
			FadeoutGuiObject:new( tweak_data.overlay_effects.level_fade_in )
		end
	end	
		
	-- Delay spawn with same time as sustain in overlay effect level_fade_in. Gives time for clients to finish their sounds
	if self._delay_spawn_t then
		if t > self._delay_spawn_t then
			self._delay_spawn_t = nil
			if managers.network:game() then
				managers.network:game():spawn_players()
			end
		end
	end
	
	local in_foucs = managers.menu:active_menu() == self._kit_menu
	--[[if self._controller and in_foucs then
		local btn_stats_screen_press = not self._stats_screen and self._controller:get_input_bool( "stats_screen" )
		local btn_stats_screen_release = self._stats_screen and self._controller:get_input_released( "stats_screen" )
							
		if btn_stats_screen_press then
			self._stats_screen = true
			self._kit_menu.input._accept_input = false
			managers.hud:show_stats_screen()
		elseif btn_stats_screen_release then
			self._stats_screen = false
			self._kit_menu.input._accept_input = true
			managers.hud:hide_stats_screen()
		end
	elseif self._stats_screen then
		self._stats_screen = false
		managers.hud:hide_stats_screen()
		if self._controller and not in_foucs then
			self._kit_menu.input._accept_input = true
		end
	end]]
	
	local in_focus = not managers.menu:active_menu() and Network:is_server()
	if in_focus then
		if self._audio_started and not self._skipped then
			if self._controller then
				local btn_skip_press = self._controller:get_input_bool( "continue" )
				if btn_skip_press and not self._skip_data then
					self._skip_data = { total = 1, current = 0 }
				elseif not btn_skip_press and self._skip_data then
					self._skip_data = nil 
					managers.hud:set_blackscreen_skip_circle( 0, 1 )
				end
			end
			
			if self._skip_data then
				self._skip_data.current = self._skip_data.current + dt
				managers.hud:set_blackscreen_skip_circle( self._skip_data.current, self._skip_data.total )
				if self._skip_data.current > self._skip_data.total then
					managers.hud:blackscreen_skip_circle_done()
					self:_skip()
				end
			end
		end
	elseif self._skip_data then
		self._skip_data = nil 
		managers.hud:set_blackscreen_skip_circle( 0, 1 )
	end
end

function IngameWaitingForPlayersState:at_enter()
	self._started_from_beginning = true
	--print( "[IngameWaitingForPlayersState:at_enter()]" )

	-- managers.overlay_effect:play_effect( tweak_data.overlay_effects.fade_in )
	self:setup_controller()
	
	self._sound_listener = SoundDevice:create_listener( "lobby_menu" )
	self._sound_listener:set_position( Vector3( 0, -50000, 0 ) )
	self._sound_listener:activate( true )
	-- managers.subtitle:set_presenter( CoreSubtitlePresenter.OverlayPresenter:new( tweak_data.hud.medium_font, 28 ) )
		
	managers.hud:load_hud( self.GUI_SAFERECT, false, true, true, {} )
	managers.hud:show( self.GUI_SAFERECT )
	
	managers.hud:load_hud( self.GUI_FULLSCREEN, false, true, false, {}, nil, nil, true )
	managers.hud:show( self.GUI_FULLSCREEN )
	
	-- Load player hud
	if( not managers.hud:exists( self.PLAYER_HUD ) ) then
		managers.hud:load_hud( self.PLAYER_HUD, false, false, true, {} )
	end
	if( not managers.hud:exists( self.PLAYER_INFO_HUD_FULLSCREEN ) ) then
		managers.hud:load_hud( self.PLAYER_INFO_HUD_FULLSCREEN, false, false, false, {} )
	end
	if( not managers.hud:exists( self.PLAYER_INFO_HUD ) ) then
		managers.hud:load_hud( self.PLAYER_INFO_HUD, false, false, true, {} )
	end
	if( not managers.hud:exists( self.PLAYER_DOWNED_HUD ) ) then
		managers.hud:load_hud( self.PLAYER_DOWNED_HUD, false, false, true, {} )
	end
	--
	if( not managers.hud:exists( self.LEVEL_INTRO_GUI ) ) then
		managers.hud:load_hud( self.LEVEL_INTRO_GUI, false, false, true, {} )
	end
	
	managers.menu:close_menu()				-- Safety close pasue menu if mashing on esc during loading
	managers.menu:open_menu( "kit_menu" )
	self._kit_menu = managers.menu:get_menu( "kit_menu" )
		
	self:_get_cameras()
	self._cam_unit = CoreUnit.safe_spawn_unit( "units/gui/background_camera_01/waiting_camera_01", Vector3(), Rotation() )
	
	self._camera_data = {}
	self._camera_data.index = 0
	self:_next_camera()
	
	self._briefing_start_t = Application:time() + 2
	
	if managers.network:session():is_client() and managers.network:session():server_peer() then
		Global.local_member:sync_lobby_data( managers.network:session():server_peer() )
		Global.local_member:sync_data( managers.network:session():server_peer() )
	end
	
	if managers.job:interupt_stage() then
		managers.menu_component:post_event( "escape_menu" )
	end
end

function IngameWaitingForPlayersState:start_game_intro()
	if self._starting_game_intro then
		return
	end
	
	self._starting_game_intro = true
	self:_start()
end


function IngameWaitingForPlayersState:set_dropin( char_name )
	self._started_from_beginning = false
--	self._is_dropin = true
	print( "Joining as " .. char_name )
end


function IngameWaitingForPlayersState:at_exit()
	print( "[IngameWaitingForPlayersState:at_exit()]" )

	-- self._intro_source:stop()
	
	managers.briefing:stop_event( true )
	
	managers.assets:clear_asset_textures()
	managers.menu:close_menu( "kit_menu" )
	managers.statistics:start_session( { from_beginning = self._started_from_beginning, drop_in = not self._started_from_beginning } )
	-- managers.overlay_effect:play_effect( tweak_data.overlay_effects.fade_in )
	-- managers.overlay_effect:stop_effect( self._fade_out_id )
	managers.hud:hide( self.GUI_SAFERECT )
	managers.hud:hide( self.GUI_FULLSCREEN )
	
	World:delete_unit( self._cam_unit )
	
	managers.menu_component:hide_game_chat_gui()
	managers.menu_component:close_mission_briefing_gui()
	
	-- local level_fade_in = tweak_data.overlay_effects.level_fade_in
	managers.overlay_effect:play_effect( tweak_data.overlay_effects.level_fade_in )
	managers.overlay_effect:stop_effect( self._fade_out_id )
	
	if self._sound_listener then
		self._sound_listener:delete()
		self._sound_listener = nil
	end
	managers.hud:hide( self.LEVEL_INTRO_GUI ) 									-- Make sure that it hasn't stuck
	if self._started_from_beginning then
		managers.music:post_event( tweak_data.levels:get_music_event( "intro" ) ) 	-- Make sure that the music starts
	end
	managers.platform:set_presence( "Playing" )
	managers.platform:set_rich_presence( Global.game_settings.single_player and "SPPlaying" or "MPPlaying" )
	
	managers.game_play_central:start_heist_timer()
end

function IngameWaitingForPlayersState:_get_cameras()
	self._cameras = {}
	
	for _,unit in ipairs( managers.helper_unit:get_units_by_type( "waiting_camera" ) ) do
		table.insert( self._cameras, { pos = unit:position(), rot = unit:rotation(), nr = math.random( 20 ) } )
	end
	
	if #self._cameras == 0 then	
		-- Get camera position and rotations from world
		table.insert( self._cameras, { pos = Vector3(-196, -496, 851), rot = Rotation(90, 0, -0), nr = math.random( 20 ) } )
		table.insert( self._cameras, { pos = Vector3(-1897, -349, 365), rot = Rotation(0, 0, -0), nr = math.random( 20 ) } )
		table.insert( self._cameras, { pos = Vector3(-2593, 552, 386), rot = Rotation(-90, 0, 0), nr = math.random( 20 ) } )
	end
end

function IngameWaitingForPlayersState:_next_camera()
	self._camera_data.next_t = Application:time() + 8 + math.rand( 4 )
	self._camera_data.index = self._camera_data.index + 1
	if self._camera_data.index > #self._cameras then
		self._camera_data.index = 1
	end
	
	self._cam_unit:set_position( self._cameras[ self._camera_data.index ].pos )
	self._cam_unit:set_rotation( self._cameras[ self._camera_data.index ].rot )
	self._cam_unit:camera():start( math.rand( 30 ) )
	
	-- local text = self._cameras[ self._camera_data.index ].nr < 10 and "0"..self._cameras[ self._camera_data.index ].nr or self._cameras[ self._camera_data.index ].nr
	-- managers.hud:script( self.GUI_SAFERECT ).cam_nr:set_text( "CAM-"..text )
end

function IngameWaitingForPlayersState:on_server_left()
	IngameCleanState.on_server_left( self )
end

function IngameWaitingForPlayersState:on_kicked()
	IngameCleanState.on_kicked( self )
end

function IngameWaitingForPlayersState:on_disconnected()
	IngameCleanState.on_disconnected( self )
end