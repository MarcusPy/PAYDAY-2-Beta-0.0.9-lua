core:import( "CoreUnit" )

require "lib/states/GameState"

IngameLobbyMenuState = IngameLobbyMenuState or class( GameState )
IngameLobbyMenuState.GUI_LOOTSCREEN 	= Idstring( "guis/lootscreen/lootscreen_fullscreen" )

function IngameLobbyMenuState:init( game_state_machine )
	GameState.init( self, "ingame_lobby_menu", game_state_machine )
	
	if( managers.hud ) then
		self._setup = true
		managers.hud:load_hud( self.GUI_LOOTSCREEN, false, true, false, {}, nil, nil, true )
		managers.hud:hide( self.GUI_LOOTSCREEN )
		-- self._sound_source = SoundDevice:create_source( "MenuComponentManager" )
	end
	
	
	-- self._controller = nil
	self._continue_cb = callback( self, self, "_continue" )
	-- self._continue_block_timer = 0
end

function IngameLobbyMenuState:setup_controller()
	if( not self._controller ) then
		self._controller = managers.controller:create_controller( "ingame_lobby_menu", managers.controller:get_default_wrapper_index(), false )
		if( Network:is_server() or managers.dlc:is_trial() ) then
			self._controller:add_trigger( "continue", self._continue_cb )
		end
		
		self._controller:set_enabled( true )
	end
end

function IngameLobbyMenuState:_clear_controller()
	if not self._controller then
		return
	end
	
	if( Network:is_server() or managers.dlc:is_trial() ) then
		self._controller:remove_trigger( "continue", self._continue_cb )
	end
	
	self._controller:set_enabled( false )
	self._controller:destroy()
	self._controller = nil
end

function IngameLobbyMenuState:_continue()
	self:continue()
end

function IngameLobbyMenuState:continue()
	if self:_continue_blocked() then
		return
	end
	
	if managers.network:session() then
		if Network:is_server() then
			managers.network.matchmake:set_server_joinable( true )
		end
	end
	
	if Global.game_settings.single_player then
		MenuCallbackHandler:_dialog_end_game_yes()
	elseif (Network:is_server() or managers.dlc:is_trial()) then
		MenuCallbackHandler:load_start_menu_lobby()
	else
		setup:load_start_menu()
	end
end

function IngameLobbyMenuState:_continue_blocked()
	local in_focus = managers.menu:active_menu() == self._loot_menu
	if not in_focus then
		return true
	end
	if managers.hud:showing_stats_screen() then
		return true
	end
	
	if managers.system_menu:is_active() then
		return true
	end
	
	if managers.menu_component:input_focus() == 1 then
		return true
	end
	
	if self._continue_block_timer > Application:time() then
		return true
	end
	
	return false
end

function IngameLobbyMenuState:set_controller_enabled( enabled )
	if self._controller then
	-- self._controller:set_enabled( enabled )
	end
end

function IngameLobbyMenuState:update( t, dt )
	
end

function IngameLobbyMenuState:fake_loot_pc( debug_pc )
	local max_pc = debug_pc or math.ceil( math.min( managers.experience:current_level() / 10, managers.job:current_job_and_difficulty_stars() ) )
	local chance = 0.85 / math.max ( 2^(#tweak_data.lootdrop.STARS[max_pc].pcs-1), 1)
	chance = chance * managers.player:upgrade_value( "player", "passive_loot_drop_multiplier", 1 ) -- * managers.player:upgrade_value( "player", "loot_drop_multiplier", 1 )

	
	-- chance = chance / math.max( #tweak_data.lootdrop.STARS[max_pc].pcs, 1 )
	for i=1, 5 do
		if math.rand(1) < 0.20 then
			chance = chance + chance
		end
	end
	
	-- Application:debug( "START CHANCE FOR OTHER LOOTDROPS" , chance , "max_pc" , max_pc )
	
	for i = 1, #tweak_data.lootdrop.STARS[max_pc].pcs do
		if math.rand(1) < chance then
			return math.ceil( tweak_data.lootdrop.STARS[max_pc].pcs[i] / 10 )
		end
		chance = chance + chance
	end
	return 0
end
		
function IngameLobbyMenuState:at_enter()
	managers.music:stop()
	managers.platform:set_presence( "Mission_end" )
	managers.platform:set_rich_presence( Global.game_settings.single_player and "SPEnd" or "MPEnd" )
	managers.hud:remove_updator( "point_of_no_return" )
	-- managers.menu_component:pre_set_game_chat_leftbottom( 0, 0 )
	print( "[IngameLobbyMenuState:at_enter()]" )
	
	if managers.network:session() then
		if Network:is_server() then
			managers.network.matchmake:set_server_state( "in_lobby" )
			-- managers.network.matchmake:set_server_joinable( true )
			managers.network:session():set_state( "in_lobby" )
		else
			managers.network:session():send_to_peers_loaded( "set_peer_entered_lobby" )
		end
	end
	
	managers.mission:pre_destroy()
	
	self._continue_block_timer = Application:time() + 0.5
	-- self:setup_controller()
			
	managers.menu:close_menu()				-- Safety close pasue menu if mashing on esc during loading
	
	if managers.job:stage_success() then
		managers.job:next_stage()
	end
	
	if( managers.job:is_job_finished() ) then	-- show the loot screen
	-- if( managers.job:is_job_finished() and not managers.job:interupt_stage() ) then	-- MAYBE FOR ESCAPE LAST
		if( not self._setup ) then
			self._setup = true
			managers.hud:load_hud( self.GUI_LOOTSCREEN, false, true, false, {}, nil, nil, true )
		end
		managers.hud:show( self.GUI_LOOTSCREEN )
		
		--[[
		self._sound_listener = SoundDevice:create_listener( "lobby_menu" )
		self._sound_listener:set_position( Vector3( 0, -50000, 0 ) )
		self._sound_listener:activate( true )
		self._sound_source = SoundDevice:create_source( "MenuComponentManager" )
		self._sound_source:post_event( "music_loot_drop" )]]
		
		managers.menu:open_menu( "loot_menu" )
		self._loot_menu = managers.menu:get_menu( "loot_menu" )
		managers.menu_component:set_max_lines_game_chat( 6 )
		managers.menu_component:pre_set_game_chat_leftbottom( 0, 0 )
		
		self._lootdrop_data = {}
		managers.lootdrop:make_drop( self._lootdrop_data )
		
		local global_values = { normal=1, superior=2, exceptional=3, infamous=4 }
		
		local peer = managers.network:session() and managers.network:session():local_peer() or false
		local global_value = global_values[ self._lootdrop_data.global_value ] or 1
		local item_category = self._lootdrop_data.type_items
		local item_id = self._lootdrop_data.item_entry
		local max_pc = self._lootdrop_data.total_stars
		local item_pc = self._lootdrop_data.joker and 0 or math.ceil( self._lootdrop_data.item_payclass / 10 )
		local card_left_pc = self:fake_loot_pc()
		local card_right_pc = self:fake_loot_pc()
		
		local lootdrop_data = {peer, self._lootdrop_data.global_value, item_category, item_id, max_pc, item_pc, card_left_pc, card_right_pc }
		managers.hud:feed_lootdrop_hud( lootdrop_data )
		
		if not Global.game_settings.single_player and managers.network:session() then
			managers.network:session():send_to_peers( "feed_lootdrop", global_value, item_category, item_id, max_pc, item_pc, card_left_pc, card_right_pc )
		end
	elseif Network:is_client() then
		if not self._setup then
			self._setup = true
			managers.hud:load_hud( self.GUI_LOOTSCREEN, false, true, false, {}, nil, nil, true )
		end
		managers.hud:hide( self.GUI_LOOTSCREEN )
		
		managers.menu:open_menu( "loot_menu" )
		self._loot_menu = managers.menu:get_menu( "loot_menu" )
		managers.menu_component:set_max_lines_game_chat( 6 )
		managers.menu_component:pre_set_game_chat_leftbottom( 0, 0 )
	end
	
	if Network:is_server() or managers.dlc:is_trial() then
		if( not managers.job:is_job_finished() ) then
		-- if( not managers.job:is_job_finished() or managers.job:interupt_stage() ) then	-- MAYBY FOR ESCAPE LAST
		
			if managers.network:session() then
				if Network:is_server() then
					managers.network.matchmake:set_server_joinable( true )
				end
			end
		
			if not managers.job:stage_success() then
				if managers.job:is_current_job_professional() then
					MenuCallbackHandler:load_start_menu_lobby()
				else
					MenuCallbackHandler:retry_job_stage()
				end
			else
				MenuCallbackHandler:on_stage_success()
				MenuCallbackHandler:lobby_start_the_game()
			end
		end
	end
end

function IngameLobbyMenuState:at_exit()
	print( "[IngameLobbyMenuState:at_exit()]" )

	if( managers.job:is_job_finished() ) then
		managers.menu:close_menu( "loot_menu" )
		managers.hud:hide( self.GUI_LOOTSCREEN )
	end
	
	-- listener moved to HUDLootScreen
	--[[
	if self._sound_listener then
		self._sound_listener:delete()
		self._sound_listener = nil
	end]]
	
	-- self:_clear_controller()
	managers.menu_component:hide_game_chat_gui()
	-- managers.platform:set_presence( "Playing" )
end

function IngameLobbyMenuState:on_server_left()
	Application:debug( "IngameLobbyMenuState:on_server_left()" )
	managers.menu_component:set_lootdrop_state( "on_server_left" )
	-- IngameCleanState.on_server_left( self )
end

function IngameLobbyMenuState:on_kicked()
	Application:debug( "IngameLobbyMenuState:on_kicked()" )
	managers.menu_component:set_lootdrop_state( "on_kicked" )
	-- IngameCleanState.on_kicked( self )
end

function IngameLobbyMenuState:on_disconnected()
	Application:debug( "IngameLobbyMenuState:on_disconnected()" )
	managers.menu_component:set_lootdrop_state( "on_disconnected" )
	-- IngameCleanState.on_disconnected( self )
end