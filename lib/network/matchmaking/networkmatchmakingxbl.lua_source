--[[-----------------------------------------------------------------------------------------------

 Match making abstraction layer

 To do:

 find_game(settings): id
	This method should always return a valid game. If the matchmaking service fails to find a
	game then the AL needs to create a game and make it available for other people to find.
	(Given the settings provided, if the setting specify a private game, it should not be
	searchable for other people)
	Settings: table
		"gamemode": string
			Which game mode to play. Current modes: "dm", "tdm", "ctf"
		"numplayers": int
			How many players the game should be able to host
		"private": boolean
			If other people can find the game when matchmaking

 cancel_find()
 	Since the find_game can, in theory, take infinite amount of time the player needs to have the
 	possibility to cancel a find.
 
 register_callback(event, callback):
	event: string
		The event that the function should be linked to. Events:
			"create_failed"		: When we for some reason fail to create a game.
			"found_server"
			"player_joined"
			"player_left"
			"unknown_error"	When something unexpected happened. If this callback is called we
							should probably gracefully exit the current menu

	callback: function
		The function to use

 join_game(id)
	When we've found a game through the matchmaking service or received a game ID from our
	group leader we need to join that specific game.

 start_game()
	Simply register that the game is running and no more people can join, start ranking etc.

 end_game()
	The game should end. All the clients should now send all their game statistics somewhere
	for proper ranking. That "somewhere" will have to be in this class, but the details
	have not been decided yet.

 signin_state(): string
 	Return if we're signed into our matchmaking service or not
 	(might be expanded, that's why we don't return a boolean)
 	Possible returns:
 		"signed in"
 		"not signed in"

 is_game_owner(): boolean
 	Returns true if this peer is the game owner. Returns false otherwise

 remove_ping_watch()
	Remove the ping watch related to matchmaking. I.e., on the server we stop ping watching
	the clients and on clients we stop ping watching our server

-----------------------------------------------------------------------------------------------]]--

NetworkMatchMakingXBL = NetworkMatchMakingXBL or class()
NetworkMatchMakingXBL.OPEN_SLOTS = 4
NetworkMatchMakingXBL.GAMEVERSION = 1

function NetworkMatchMakingXBL:init()
	cat_print( 'lobby', "matchmake = NetworkMatchMakingXBL")
	self._callback_map = {}
	self._distance_filter = -1
	self._difficulty_filter = 0
	self._try_re_enter_lobby = nil -- "lost" we need to ask the host to re-open the lobby. "asked" = we have asked the host to re-open the lobby. "open" = the host has confirmed the lobby is open, "joining" = we are re-joining
	self._players = {}
	self:set_server_joinable( true )
	
	managers.platform:add_event_callback( "invite_accepted", callback( self, self, "invite_accepted_callback" ) )
end

function NetworkMatchMakingXBL:invite_accepted_callback( player_index  )
	print( "NetworkMatchMakingXBL:invite_accepted_callback", player_index )
	local invitation = XboxLive:accepted_invite( player_index )
	
	if not invitation then
		Application:error( "NetworkMatchMakingXBL:invite_accepted_callback Invitation didn't contain anything" )
		return
	end
	
	Global.boot_invite = Global.boot_invite or {}
	
	if not Global.user_manager.user_index or not Global.user_manager.active_user_state_change_quit then -- This is a boot up invite
		print( "BOOT UP INVITE" )
		Global.boot_invite[ player_index ] = invitation
		return
	end
	
	if managers.user:get_platform_id() ~= player_index then
		print( "INACTIVE USER RECIEVED INVITE" )
		-- Sign out.. show message
		Global.boot_invite[ player_index ] = invitation
		
		managers.menu:show_inactive_user_accepted_invite( { ok_func = nil } )
		
		-- local f = function()
			managers.user:invite_accepted_by_inactive_user()
		-- end
		
		-- managers.menu:show_inactive_user_accepted_invite( { ok_func = f } )
		
		return
	end
	
	if game_state_machine:current_state_name() ~= "menu_main" then -- In a game somewhere, queue as boot invite and exit to menu
		print( "INGAME INVITE" )
		Global.boot_invite[ player_index ] = invitation
		MenuCallbackHandler:_dialog_end_game_yes()
		return
	end
	
	self:_check_invite_requirements( invitation )
	
	--[[self._test_invitation = invitation
	print( "invitation\n", inspect( invitation ) )
	
		
	if not managers.menu:_enter_online_menus_x360() then
		return
	end
	
	if self._session and self._session:id() == invitation.host_info:id() then -- Dont join if we are in this game
		print( "Allready in that session" )
		return
	end
		
	self._has_pending_invite = true
	self:_join_invite_accepted( invitation.host_info  )]]
		
	--[[self._has_pending_invite = true
	print( "_recived_join_invite" ) 
	local dialog_data = {}
	dialog_data.title = managers.localization:text( "dialog_mp_groupinvite_title" )
	dialog_data.text = managers.localization:text("dialog_mp_groupinvite_message", {GROUP = "WHAT HERE"} )

	local yes_button = {}
	yes_button.text = managers.localization:text( "dialog_yes" )
	yes_button.callback_func = callback( self, self, "_join_invite_accepted", invitation.host_info ) -- callback( self, self, "_dialog_progress_resetted_ok" )
	local no_button = {}
	no_button.text = managers.localization:text( "dialog_no" )
	no_button.callback_func = function() self._has_pending_invite = nil end -- callback( self, self, "_dialog_progress_resetted_ok" )
	dialog_data.button_list = { yes_button, no_button }

	managers.system_menu:show( dialog_data )]]
end

function NetworkMatchMakingXBL:join_boot_invite()
	local invitation = Global.boot_invite[ managers.user:get_platform_id() ]
	print( "NetworkMatchMakingXBL:join_boot_invite()", invitation )
	if not invitation then
		return
	end
	self:_check_invite_requirements( invitation )
	Global.boot_invite[ managers.user:get_platform_id() ] = nil
end

function NetworkMatchMakingXBL:_check_invite_requirements( invitation )	
	Global.game_settings.single_player = false
	self._test_invitation = invitation
	print( "invitation\n", inspect( invitation ) )
	
	
	--[[if PSN:user_age() < MenuManager.ONLINE_AGE and PSN:parental_control_settings_active() then -- AGE RESTRICTION CHECK
		managers.menu:show_err_under_age()
		return
	end]]
	
	if not managers.menu:_enter_online_menus_x360() then
		return
	end
	
	if self._session and self._session:id() == invitation.host_info:id() then -- Dont join if we are in this game
		print( "Allready in that session" )
		return
	end
	
	--[[if message.version ~= self:_game_version() then
		managers.menu:show_invite_wrong_version_message()
		return 
	end]]
	
	self._has_pending_invite = true
	self:_join_invite_accepted( invitation.host_info  )

end

function NetworkMatchMakingXBL:_join_invite_accepted( host_info )
	managers.system_menu:close( "server_left_dialog" )
	print( "_join_invite_accepted", host_info )
	self._has_pending_invite = nil
	self._invite_host_info = host_info
	if self._session then
		print( "MUST LEAVE session" )
		MenuCallbackHandler:_dialog_leave_lobby_yes()
		-- return
	end
	
	self:join_server_with_check( host_info:id(), true, {} )
end

function NetworkMatchMakingXBL:register_callback(event, callback)
	self._callback_map[ event ] = callback
end

function NetworkMatchMakingXBL:_call_callback( name, ... )
	if( self._callback_map[ name ] ) then
		return self._callback_map[ name ]( ... )
	else
		Application:error( "Callback " .. name .. " not found." )
	end
end

function NetworkMatchMakingXBL:_has_callback( name )
	if( self._callback_map[ name ] ) then
		return true
	end
	
	return false
end


function NetworkMatchMakingXBL:_split_attribute_number( attribute_number, splitter )
	if not splitter or splitter == 0 or type(splitter) ~= "number" then
		Application:error( "NetworkMatchMakingXBL:_split_attribute_number. splitter needs to be a non 0 number!", "attribute_number", attribute_number, "splitter", splitter )
		Application:stack_dump()
		return 1, 1
	end
	return attribute_number%splitter, math.floor(attribute_number/splitter)
end


function NetworkMatchMakingXBL:destroy_game()
	self:leave_game()
end

function NetworkMatchMakingXBL:leave_game()
	print( "NetworkMatchMakingXBL:leave_game()", self._session and self._session:state() )
	Application:stack_dump()
	if( self._session ) then
		local player_index = managers.user:get_platform_id()
		
		print( "managers.user:get_platform_id()", managers.user:get_platform_id() )
		print( "  _leave and destroy", self._session )
		XboxLive:leave_local( self._session, player_index )
		print( " has left" )
		XboxLive:delete_session( self._session )
		print( " has deleted" )
		
		if self:_is_server() then
			self._server_last_alive_t = nil
			self._next_time_out_check_t = nil
		end
		
		self._session = nil
		self:set_server_joinable( true )
	
		--[[if( self:_is_client() ) then
			PSN:leave_session( self._room_id )
		else
			self._server_last_alive_t = nil
			self._next_time_out_check_t = nil
			PSN:set_matchmaking_callback( "fetch_session_attributes", function() end )
			XboxLive:destroy_session( self._session )
		end]]
		print( "NetworkMatchMakingXBL:leave_game() done" )
		Application:stack_dump()
	else
		cat_print( "multiplayer", "Dont have a session!?" )
	end
	
	self:_is_server( false )
	self:_is_client( false )
	self._game_owner_name = nil
end

function NetworkMatchMakingXBL:_load_globals()
	if Global.xbl and Global.xbl.match then
		self._session 			= Global.xbl.match.session
		self._server_rpc		= Global.xbl.match.server_rpc
		self._game_owner_name	= Global.xbl.match.game_owner_name
		self._num_players		= Global.xbl.match.num_players
		self._is_server_var		= Global.xbl.match.is_server
		self._is_client_var		= Global.xbl.match.is_client
		self._players			= Global.xbl.match.players
		
		Global.xbl.match = nil
	end
	
	--[[if( Global.steam and Global.steam.match ) then
		
		self.lobby_handler = Global.steam.match.lobby_handler
		self._lobby_attributes = Global.steam.match.lobby_attributes
		
		if self.lobby_handler then
			self.lobby_handler:setup_callbacks(NetworkMatchMakingXBL._on_memberstatus_change, NetworkMatchMakingXBL._on_data_update, NetworkMatchMakingXBL._on_chat_message )
		end
		
		self._try_re_enter_lobby = Global.steam.match.try_re_enter_lobby
		self._server_rpc = Global.steam.match.server_rpc
		
		Global.steam.match = nil
	end]]
end
function NetworkMatchMakingXBL:_save_globals()
	Global.xbl = Global.xbl or {}
	Global.xbl.match = {}
	
	Global.xbl.match.session 			= self._session
	Global.xbl.match.server_rpc 		= self._server_rpc
	Global.xbl.match.game_owner_name 	= self._game_owner_name
	Global.xbl.match.num_players 		= self._num_players
	Global.xbl.match.is_server 			= self._is_server_var
	Global.xbl.match.is_client 			= self._is_client_var
	Global.xbl.match.players 			= self._players
	
	--[[if( not Global.steam ) then
		Global.steam = {}
	end

	Global.steam.match = {}
	
	Global.steam.match.lobby_handler = self.lobby_handler
	Global.steam.match.lobby_attributes = self._lobby_attributes
	Global.steam.match.try_re_enter_lobby = self._try_re_enter_lobby
	Global.steam.match.server_rpc = self._server_rpc]]
end


function NetworkMatchMakingXBL:update()
	
end

--[[function NetworkMatchMakingXBL:leave_game()
	self._server_rpc = nil
	
	
	print( "NetworkMatchMakingXBL:leave_game()" )
end]]


function NetworkMatchMakingXBL:get_friends_lobbies()
	
end

function NetworkMatchMakingXBL:search_friends_only()
	return self._search_friends_only
end

function NetworkMatchMakingXBL:distance_filter()
	return self._distance_filter
end

function NetworkMatchMakingXBL:set_distance_filter( filter )
	self._distance_filter = filter
end

function NetworkMatchMakingXBL:difficulty_filter()
	return self._difficulty_filter
end

function NetworkMatchMakingXBL:set_difficulty_filter( filter )
	self._difficulty_filter = filter
end

function NetworkMatchMakingXBL:search_lobby( friends_only )
	if self._searching_lobbys then
		print( "Allready searching lobbys, waiting result" )
		return
	end
	self._search_friends_only = friends_only
	
	if not self:_has_callback( "search_lobby" ) then
		return
	end
	
	local player_index = managers.user:get_platform_id()
	
	local prop = {} --  LEVELINDEX = 98 }
	prop.MINLEVEL = managers.experience:current_level()
	prop.GAMEVERSION = self.GAMEVERSION
	
	local con = {}
	--prop["min victory points"] = self._last_settings["skill"]-NetworkMatchMakingXBL._SKILL_HALF_RANGE
	--prop["max victory points"] = self._last_settings["skill"]+NetworkMatchMakingXBL._SKILL_HALF_RANGE
	-- prop["Public"] = self._last_settings["public"]
	con["GAME_TYPE"] = "STANDARD" -- settings["game_type"]
	con["game_mode"] = "ONLINE" -- settings["game_mode"]
	
	-- print( "properties", inspect( prop ) )

	self._searching_lobbys = true
	-- Ask match making service
	XboxLive:search_session("Find Matches", player_index, 50, prop, con, callback( self, self, "_find_server_callback") )
end

function NetworkMatchMakingXBL:_find_server_callback( servers, mode )
	self._searching_lobbys = nil
	if self._cancel_find then
		return
	end
	
	self._last_mode = mode
	
	print( "find_server_callback", mode, inspect( servers ) )
	
	if not servers then
		print( "SEaRCH FAILED" )
		return 
	end
	
	local info = {
		room_list = {},
		attribute_list = {}
	}
		
	for _,server in ipairs( servers ) do
		self._test_server = server
		print( inspect( server ) )
		
		table.insert( info.room_list, { owner_id = nil, owner_name = server.properties.GAMERHOSTNAME, room_id = server.info:id(), info = server.info } )
		table.insert( info.attribute_list, { numbers = self:_server_to_numbers( server ) } )
	end
	
	self:_call_callback( "search_lobby", info )
	
	local player_index = managers.user:get_platform_id()
	
--[[	if( servers and #servers > 0 ) then
		cat_print( 'lobby', "found server, trying to connect")
		self:_try_servers( servers, mode )
	elseif mode == "join_game" then
        self:_call_callback("join_failed")
	else
		self:_create_server( self._last_settings )
	end
	]]
end


function NetworkMatchMakingXBL:search_lobby_done()
	managers.system_menu:close( "find_server" )
	self.browser = nil
end


function NetworkMatchMakingXBL:game_owner_name()
	return self._game_owner_name
	-- return managers.network.matchmake.lobby_handler:get_lobby_data( "owner_name" )
end

-- settings.number
-- index 1 - level index (from level tweak data)			-- i%1000 = level index,  math.floor(i/1000) = job index
-- index 2 - difficulty 1-4 (easy,normal,hard,overkill)
-- index 3 - permission 1-3 (public,friends-only,private)
-- index 4 - server state 1-2 (lobby,ingame)
-- index 5 - num players
-- index 6 - allow drop-in 0-1
-- index 7 - minimum level 0+
-- index 8 - job_id		-- needs to merge with index 1 if consoles doesn't like index 8
function NetworkMatchMakingXBL:is_server_ok( friends_only, session_id, attributes_numbers )
	local permission = tweak_data:index_to_permission( attributes_numbers[3] )
	
	-- print( "MenuSTEAMHostBrowser:_is_server_ok ", attributes_numbers[3], permission )
	-- print( "level dindex", attributes_numbers[1] )
	-- print( "requires_dls", tweak_data.levels:requires_dlc_by_index( attributes_numbers[1] ) )
--[[
	local requires_dls = tweak_data.levels:requires_dlc_by_index( attributes_numbers[1] )
	if requires_dls then
		if requires_dls == "dlc2" and not managers.dlc:has_dlc2() then
			return false, 4
		elseif requires_dls == "dlc3" and not managers.dlc:has_dlc3() then
			return false, 4
		end
	end
]]

	local level_index, job_index = self:_split_attribute_number( attributes_numbers[ 1 ], 1000 )
	if not tweak_data.levels:get_level_name_from_index( level_index ) then
		Application:error( "No level data for index "..level_index..". Payday1 data not compatible with Payday2." )
		return false
	end
	
	if (not NetworkManager.DROPIN_ENABLED or attributes_numbers[6] == 0) and attributes_numbers[4] ~= 1 then
		return false, 1
	end
	
	if managers.experience:current_level() < attributes_numbers[7] then
		return false, 3
	end
	
	-- Never allowed to join private sessions
	if permission == "private" then
		return false, 2
	end
	 
	-- All public servers are ok
	if permission == "public" then
		return true
	end

	return true
end

-- data contains host_name
function NetworkMatchMakingXBL:join_server_with_check( session_id, skip_permission_check, data )
	print( "NetworkMatchMakingXBL:join_server_with_check", session_id )
	local player_index = managers.user:get_platform_id()
	managers.menu:show_joining_lobby_dialog()
	
	local empty = function() end
	local f = function( servers )
		print( "servers", servers )
		if not servers or not servers[1] then -- Server no longer exists
			managers.system_menu:close( "join_server" )
			-- managers.menu:show_failed_joining_dialog()
			if managers.user:signed_in_state() ~= "signed_in_to_live" then
				managers.menu:xbox_disconnected()
			else
				managers.menu:show_game_no_longer_exists()
			end
			return
		end
		
		print( "NetworkMatchMakingXBL:join_server_with_check f", inspect( servers[1] ) )
		print( "SELF", self, player_index )
					 
		-- local attributes = self:_server_to_numbers( server[1] ) !!!						

		--[[if NetworkMatchMakingXBL._BUILD_SEARCH_INTEREST_KEY then
			local ikey = lobby:key_value( NetworkMatchMakingXBL._BUILD_SEARCH_INTEREST_KEY )
			if ikey == "value_missing" or ikey == "value_pending" then
				print( "Wrong version!!" )
				managers.system_menu:close( "join_server" )
				managers.menu:show_failed_joining_dialog()
				return
			end
		end]]
		
		--[[servers[1].filled_private_slots
		servers[1].filled_public_slots
		servers[1].open_private_slots
		servers[1].open_public_slots]]
	
		-- print( inspect( attributes ) )
		local server_ok, ok_error = true, nil -- self:is_server_ok( nil, session_id, attributes )
		if server_ok then
			print( "CALL JOIN SERVER", servers[1].info )
			self._game_owner_name = data.host_name
			self:join_server( session_id, servers[1], true )
		else
			managers.system_menu:close( "join_server" )
		
			if ok_error == 1 then
				managers.menu:show_game_started_dialog()
			elseif ok_error == 2 then
				managers.menu:show_game_permission_changed_dialog()
			elseif ok_error == 3 then
				managers.menu:show_too_low_level()
			elseif ok_error == 4 then
				managers.menu:show_does_not_own_heist()
			end
			
			self:search_lobby( self:search_friends_only() )
		end
	end

	--[[lobby:setup_callback( f )
	if lobby:key_value( "state" ) == "value_pending" then
		print( "NetworkMatchMakingXBL:join_server_with_check value_pending" )
		lobby:request_data()
	else
		f()
	end]]
		
	XboxLive:search_session_by_id( session_id, player_index, f )
end

--[[function NetworkMatchMakingXBL._on_member_left( steam_id, status )
	if not managers.network:session() then
		return
	end
	
	local peer = managers.network:session():peer_by_user_id( steam_id )
	if not peer then
		return
	end
	
	if peer == managers.network:session():local_peer() and managers.network:session():is_server() then
		managers.network:session():on_peer_left( peer, peer_id )
		return
	elseif peer == managers.network:session():local_peer() and not managers.network:session():closing() then
		Application:error( "OMG I LEFT THE LOBBY" )
		managers.network.matchmake._try_re_enter_lobby = "lost"
	end
	managers.network:session():on_peer_left_lobby( peer )
end

function NetworkMatchMakingXBL._on_memberstatus_change( memberstatus )
	print( "[NetworkMatchMakingXBL._on_memberstatus_change]", memberstatus )

	local user, status = unpack( string.split( memberstatus, ":" ) )
	if status == "lost_steam_connection" or
		status == "left_become_owner" or
		status == "left" or
		status == "kicked" or
		status == "banned" or
		status == "invalid" then
		NetworkMatchMakingXBL._on_member_left( user, status )
	end]]
	
--[[	
	lost_steam_connection
	invalid
	left_become_owner
	left
	entered
	kicked
	banned	
	]]
-- end

function NetworkMatchMakingXBL._on_data_update( ... )
	-- print( "[NetworkMatchMakingXBL._on_data_update]", ... )
end

function NetworkMatchMakingXBL._on_chat_message( user, message )
	print( "[NetworkMatchMakingXBL._on_chat_message]", user, message )
	-- NetworkMatchMakingXBL._handle_chat_message( user, message )
end
function NetworkMatchMakingXBL._handle_chat_message( user, message )
	local s = ""..message
	-- print( s )
	-- Crashes on едц for example.
	managers.chat:receive_message_by_name( ChatManager.GLOBAL, user:name(), s )
end

function NetworkMatchMakingXBL:join_server( session_id, server, skip_showing_dialog )
	local xs_info = server.info
	if not skip_showing_dialog then
		managers.menu:show_joining_lobby_dialog()
	end
		
	local player_index = managers.user:get_platform_id()
	
	print( "join_server", xs_info, xs_info:id() )
	if self._session then
		XboxLive:leave_local( self._session, player_index )
		XboxLive:delete_session( self._session )
	end
	
	XboxLive:set_context( player_index, "GAME_TYPE", "STANDARD" )
	XboxLive:set_context( player_index, "game_mode", "ONLINE" )
	
	-- How will this be private? If accepting an invite with that kind of info?
	local permission = server.open_private_slots > 0 and "private" or "public" -- tweak_data:index_to_permission( attributes_numbers[3] )
	
	--slots
	local pub_slots = self.OPEN_SLOTS
	local priv_slots = 0
	self._private = false
	if( permission == "private" ) then
		pub_slots = 0
		priv_slots = self.OPEN_SLOTS
		self._private = true
	end
	
	self._session = XboxLive:create_client_session( "live_multiplayer_standard", player_index, pub_slots, priv_slots, xs_info ) -- , callback( self, self, "_join_server_callback") )
	
	local result = "success" -- !!!
	if not self._session then
		print( "FAILED CREATE CLIENT SESSION" )
		result = "failed" 
		-- return
	end
	
	XboxLive:join_local( self._session, player_index, self._private )
	-- XboxLive:join_remote( self._session, XboxLive:xuid( player_index ), false )

	print( "self._session", self._session )
	
		print( "[NetworkMatchMakingXBL:join_server:f]" )
		managers.system_menu:close( "join_server" )
		if result == "success" then
			print( "Success!" )
			-- self.lobby_handler = handler

			-- local _, host_id, owner = self.lobby_handler:get_server_details()
			print( "[NetworkMatchMakingXBL:join_server] server details", self._session:ip(), self._session:id() )

			print( "Gonna handshake now!" )
			self._server_rpc = Network:handshake( self._session:ip(), managers.network.DEFAULT_PORT, "TCP_IP" )
			print( "Handshook!" )
			
			print( "Server RPC:", self._server_rpc and self._server_rpc:ip_at_index(0) )
			
			if not self._server_rpc then
				return
			end
			
			self._players = {}
			self:_is_server( false )
			self:_is_client( true )
			
			-- self.lobby_handler:setup_callbacks(NetworkMatchMakingXBL._on_memberstatus_change, NetworkMatchMakingXBL._on_data_update, NetworkMatchMakingXBL._on_chat_message )
			
			managers.network.voice_chat:open_session()
			managers.network:start_client()
			managers.menu:show_waiting_for_server_response( { cancel_func = function() managers.network:session():on_join_request_cancelled() end } )
	
			local joined_game = function( res, level_index, difficulty_index, state_index )
				managers.system_menu:close( "waiting_for_server_response" )
				print( "[NetworkMatchMakingXBL:join_server:joined_game]", res, level_index, difficulty_index, state_index )
				if res == "JOINED_LOBBY" then
					-- managers.network.voice_chat:open_session()
					MenuCallbackHandler:crimenet_focus_changed( nil, false )
					managers.menu:on_enter_lobby() -- In MenuManagerPD2
					-- managers.menu:close_menu( "menu_main" )
					-- managers.menu:open_menu( "lobby_menu" )
				elseif res == "JOINED_GAME" then	-- The desired host has answered positively. load the level
					-- managers.network.voice_chat:set_drop_in( { room_id = self._room_id } )
					local level_id = tweak_data.levels:get_level_name_from_index( level_index )
					--local level_name = tweak_data.levels[ level_id ].world_name
					-- managers.network:session():load_level( level_name, nil, nil, nil, level_id, nil )
					Global.game_settings.level_id = level_id
					managers.network:session():ok_to_load_level()
				elseif res == "KICKED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					-- managers.network:stop_network( true )
					managers.network:queue_stop_network()
					managers.menu:show_peer_kicked_dialog()
				elseif res == "TIMED_OUT" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_request_timed_out_dialog()
				elseif res == "GAME_STARTED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_game_started_dialog()
				elseif res == "DO_NOT_OWN_HEIST" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_does_not_own_heist()
				elseif res == "CANCELLED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
				elseif res == "FAILED_CONNECT" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_failed_joining_dialog()
				elseif res == "GAME_FULL" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_game_is_full()
				elseif res == "LOW_LEVEL" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_too_low_level()
				elseif res == "WRONG_VERSION" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_wrong_version_message()
				else
					Application:error("[NetworkMatchMakingXBL:join_server] FAILED TO START MULTIPLAYER!", res)
				end
			end
		
			managers.network:join_game_at_host_rpc( self._server_rpc, joined_game )
		else
			self:leave_game()
			managers.menu:show_failed_joining_dialog()
			self:search_lobby( self:search_friends_only() )
		end
	
	

	--[[ local f = function( result, handler )
		print( "[NetworkMatchMakingXBL:join_server:f]", result, handler )
		managers.system_menu:close( "join_server" )
		if result == "success" then
			print( "Success!" )
			self.lobby_handler = handler

			local _, host_id, owner = self.lobby_handler:get_server_details()
			print( "[NetworkMatchMakingXBL:join_server] server details", _, host_id )

			print( "Gonna handshake now!" )
			self._server_rpc = Network:handshake( host_id:tostring(), nil, "STEAM" )
			print( "Handshook!" )
			
			print( "Server RPC:", self._server_rpc and self._server_rpc:ip_at_index(0) )
			
			if not self._server_rpc then
				return
			end
			
			self.lobby_handler:setup_callbacks(NetworkMatchMakingXBL._on_memberstatus_change, NetworkMatchMakingXBL._on_data_update, NetworkMatchMakingXBL._on_chat_message )

			
			managers.network:start_client()
			managers.menu:show_waiting_for_server_response( { cancel_func = function() managers.network:session():on_join_request_cancelled() end } )
	
			local joined_game = function( res, level_index, difficulty_index, state_index )
				managers.system_menu:close( "waiting_for_server_response" )
				print( "[NetworkMatchMakingXBL:join_server:joined_game]", res, level_index, difficulty_index, state_index )
				if res == "JOINED_LOBBY" then
					-- managers.network.voice_chat:open_session( self._room_id )
					MenuCallbackHandler:crimenet_focus_changed( nil, false )
					managers.menu:on_enter_lobby() -- In MenuManagerPD2
					-- managers.menu:close_menu( "menu_main" )
					-- managers.menu:open_menu( "lobby_menu" )
				elseif res == "JOINED_GAME" then	-- The desired host has answered positively. load the level
					-- managers.network.voice_chat:set_drop_in( { room_id = self._room_id } )
					local level_id = tweak_data.levels:get_level_name_from_index( level_index )
					--local level_name = tweak_data.levels[ level_id ].world_name
					-- managers.network:session():load_level( level_name, nil, nil, nil, level_id, nil )
					Global.game_settings.level_id = level_id
					managers.network:session():ok_to_load_level()
				elseif res == "KICKED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					-- managers.network:stop_network( true )
					managers.network:queue_stop_network()
					managers.menu:show_peer_kicked_dialog()
				elseif res == "TIMED_OUT" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_request_timed_out_dialog()
				elseif res == "GAME_STARTED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_game_started_dialog()
				elseif res == "DO_NOT_OWN_HEIST" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_does_not_own_heist()
				elseif res == "CANCELLED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
				elseif res == "FAILED_CONNECT" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_failed_joining_dialog()
				else
					Application:error("[NetworkMatchMakingXBL:join_server] FAILED TO START MULTIPLAYER!", res)
				end
			end
		
			managers.network:join_game_at_host_rpc( self._server_rpc, joined_game )
		else
			managers.menu:show_failed_joining_dialog()
			self:search_lobby( self:search_friends_only() )
		end
	end

	Steam:join_lobby( room_id, f )]]
end


function NetworkMatchMakingXBL:send_join_invite( friend )
end


function NetworkMatchMakingXBL:set_server_attributes( settings )
	self:set_attributes( settings )
end

function NetworkMatchMakingXBL:create_lobby( settings )
	local attributes_numbers = settings.numbers
	self._num_players = nil
	
	self:set_server_joinable( true )
	print( "NetworkMatchMakingXBL:create_lobby", inspect( settings ) )
	settings.numbers[4] = 1
	self:set_attributes( settings )
	local player_index = managers.user:get_platform_id()
	--[[XboxLive:set_property( player_index, "LEVELINDEX", settings.numbers[1] )
	XboxLive:set_property( player_index, "DIFFICULTY", settings.numbers[2] )
	XboxLive:set_property( player_index, "PERMISSION", settings.numbers[3] )
	XboxLive:set_property( player_index, "SERVERSTATE", settings.numbers[4] )
	XboxLive:set_property( player_index, "NUMPLAYERS", 1 )
 	XboxLive:set_property( player_index, "ALLOWDROPIN", settings.numbers[6] )
 	XboxLive:set_property( player_index, "MINLEVEL", settings.numbers[7] )]]
	
	--[[
		<property id="0x10000001" name="LEVELINDEX" type="int32"/>
		<property id="0x10000002" name="DIFFICULTY" type="int32"/>
		<property id="0x10000003" name="PERMISSION" type="int32"/>
		<property id="0x10000004" name="SERVERSTATE" type="int32"/>
		<property id="0x10000005" name="NUMPLAYERS" type="int32"/>
		<property id="0x10000006" name="ALLOWDROPIN" type="int32"/>
		<property id="0x10000007" name="MINLEVEL" type="int32"/>
	]]
	
	local gt = "STANDARD" -- settings["game_type"]
	local gm = "ONLINE" -- settings["game_mode"]
	-- local public = settings["public"]
	
	--[[if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end]]

	XboxLive:set_context( player_index, "GAME_TYPE", gt )
	XboxLive:set_context( player_index, "game_mode", gm )

	-- XboxLive:set_property( player_index, "Public", public)

	--leave existing, if there is any
	if( self._session and self._session:state() == "started" ) then
		XboxLive:leave_local( self._session, player_index )
		XboxLive:delete_session( self._session, function() print( "DELETED SESSION") end )
	end
	
	-- local create_prop = { "live_multiplayer_standard", "-uses_presence" } -- , "join_in_progress_disabled"}
	--[[local create_prop = { "group_lobby" } -- , "join_in_progress_disabled"}
	if( self._uses_arbitration == true ) then
		table.insert( create_prop, "uses_arbitration" )
	end]]
	
	local permission = tweak_data:index_to_permission( attributes_numbers[3] )
	
	--slots
	local pub_slots = self.OPEN_SLOTS
	local priv_slots = 0
	self._private = false
	if( permission == "private" ) then
		pub_slots = 0
		priv_slots = self.OPEN_SLOTS
		self._private = true
	end
	
	local dialog_data = {}
	dialog_data.title = managers.localization:text( "dialog_creating_lobby_title" )
	dialog_data.text = managers.localization:text( "dialog_wait" )

	dialog_data.id = "create_lobby"
	dialog_data.no_buttons = true

	managers.system_menu:show( dialog_data )
	

	--create session
	-- print( "creating session\n", inspect( create_prop ) )
	local success = XboxLive:create_session( "live_multiplayer_standard", player_index, pub_slots, priv_slots, callback( self, self, "_create_lobby_callback", settings ) )
	print( "create return value", success )
	
	
	
	--[[local dialog_data = {}
	dialog_data.title = managers.localization:text( "dialog_creating_lobby_title" )
	dialog_data.text = managers.localization:text( "dialog_wait" )

	dialog_data.id = "create_lobby"
	dialog_data.no_buttons = true

	managers.system_menu:show( dialog_data )


	local f = function( result, handler )
		print( "Create lobby callback!!", result, handler )
		
		if result == "success" then
			self.lobby_handler = handler
			self:set_attributes( settings )
			self.lobby_handler:publish_server_details()
			self.lobby_handler:set_joinable( true )
			
			self.lobby_handler:setup_callbacks(NetworkMatchMakingXBL._on_memberstatus_change, NetworkMatchMakingXBL._on_data_update, NetworkMatchMakingXBL._on_chat_message )
			
			managers.system_menu:close( "create_lobby" )
			managers.menu:created_lobby()
		else
			managers.system_menu:close( "create_lobby" )
		
			local title = managers.localization:text( "dialog_error_title" )
			local dialog_data = { title = title, text = managers.localization:text( "dialog_err_failed_creating_lobby" ) }
			dialog_data.button_list = { { text = managers.localization:text( "dialog_ok" ) } }
			managers.system_menu:show( dialog_data )
		end
	end

	return Steam:create_lobby( f, NetworkMatchMakingXBL.OPEN_SLOTS, "invisible" )]]
end

function NetworkMatchMakingXBL:_create_lobby_failed()
	self:_create_lobby_done()
	
	local title = managers.localization:text( "dialog_error_title" )
	local dialog_data = { title = title, text = managers.localization:text( "dialog_err_failed_creating_lobby" ) }
	dialog_data.button_list = { { text = managers.localization:text( "dialog_ok" ) } }
	managers.system_menu:show( dialog_data )
end

function NetworkMatchMakingXBL:_create_lobby_done()
	self._creating_lobby = nil
	managers.system_menu:close( "create_lobby" )
end

function NetworkMatchMakingXBL:_create_lobby_callback( settings, session )
	if self._cancel_find then
		-- If we managed to create one but have cancled we need to remove the one we created		
		-- managers.network.shared:delete_session( session )		
		cat_print( "lobby", "create_server canceled")
		return
	end

	print( "NetworkMatchMakingXBL:_create_server_callback", inspect( settings ) )
	local player_index = managers.user:get_platform_id()
	
	if( not session ) then
		print( "CREATE SESSION FAILED" ) 
		self:_create_lobby_failed()
		-- self:_call_callback( "create_failed" )
		return
	end

	XboxLive:join_local( session, player_index, self._private )
	
	-- self:set_attributes( settings )

	-- Set self._session
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end

	self._session = session
	print( " Created mm session ", self._session:id() )
			
	self._trytime = nil 
	
	self._players = {}
	self._server_rpc = nil
	self:_is_server( true )
	self:_is_client( false )
	
	managers.network.voice_chat:open_session()
	
	self:_create_lobby_done()
	managers.menu:created_lobby()

--[[
	managers.network:bind_port()

	self:_call_callback( "found_game", self._session, true )
	self:_call_callback( "player_joined", {
		player_id=managers.network.account:player_id(),
		group_id=managers.network.group:group_id(),
		managers.network.account:username(),
		rpc=Network:self(),
		external_address=nil,
		} )]]
end

function NetworkMatchMakingXBL:set_num_players( num )
	print( "NetworkMatchMakingXBL:set_num_players", num )
	local player_index = managers.user:get_platform_id()

	self._num_players = num
	XboxLive:set_property( player_index, "NUMPLAYERS", self._num_players )
end

function NetworkMatchMakingXBL:set_server_state( state )
	local player_index = managers.user:get_platform_id()
	
	local state_id = tweak_data:server_state_to_index( state )
	XboxLive:set_property( player_index, "SERVERSTATE", state_id )
end

function NetworkMatchMakingXBL:set_server_joinable( state )
	print( "[NetworkMatchMakingXBL:set_server_joinable]", state )
	local player_index = managers.user:get_platform_id()
		
	XboxLive:set_property( player_index, "SERVERJOINABLE", state and 1 or 0 )
end

function NetworkMatchMakingXBL:is_server_joinable()
	local player_index = managers.user:get_platform_id()
		
	return XboxLive:get_property( player_index, "SERVERJOINABLE" ) == 1
end

function NetworkMatchMakingXBL:server_state_name()
	-- return tweak_data:index_to_server_state( self._lobby_attributes.state )	
end

function NetworkMatchMakingXBL:on_peer_added( peer )
	print( "NetworkMatchMakingXBL:on_peer_added", peer:id(), peer:xuid(), self._session, self._private )
	if managers.network:session() and managers.network:session():local_peer() == peer 
		or type( peer:xuid() ) == "string" then
		print( "  Dont add local peer or empty string" ) 
		return
	end
	if not self._session then
		Application:error( "NetworkMatchMakingXBL:on_peer_added, had no session!" )
		return
	end
	self._players[ peer:id() ] = peer:xuid()
	XboxLive:join_remote( self._session, peer:xuid(), self._private or false )
	
	local player_info = {}
	player_info.name = peer:name()
	player_info.player_id = peer:xuid()
	player_info.external_address = peer:xnaddr()
	managers.network.voice_chat:open_channel_to( player_info, "game" )
end

function NetworkMatchMakingXBL:on_peer_removed( peer )
	print( "NetworkMatchMakingXBL:on_peer_removed", peer:id(), peer:xuid(), self._session )
	if type( peer:xuid() ) == "string" then
		print( "  Dont remove peer with empty string" )
		return
	end
	if not self._session then
		Application:error( "NetworkMatchMakingXBL:on_peer_removed, had no session!" )
		return
	end
	
	self._players[ peer:id() ] = nil
	XboxLive:leave_remote( self._session, peer:xuid() )
	
	local player_info = {}
	player_info.name = peer:name()
	player_info.player_id = peer:xuid()
	managers.network.voice_chat:close_channel_to( player_info )
end

-- settings.number
-- index 1 - level index (from level tweak data)
-- index 2 - difficulty 1-4 (easy,normal,hard,overkill)
-- index 3 - permission 1-3 (public,friends-only,private)
-- index 4 - server state 1-2 (lobby,ingame)
-- index 5 - num players
-- index 6 - allow drop-in 0-1
-- index 7 - minimum level 0+
-- index 8 - job_id		-- needs to merge with index 1 if consoles doesn't like 8 indexes
function NetworkMatchMakingXBL:set_attributes( settings )
	local player_index = managers.user:get_platform_id()
	
	XboxLive:set_property( player_index, "LEVELINDEX", settings.numbers[1] )
	XboxLive:set_property( player_index, "DIFFICULTY", settings.numbers[2] )
	XboxLive:set_property( player_index, "PERMISSION", settings.numbers[3] )
	XboxLive:set_property( player_index, "SERVERSTATE", settings.numbers[4] or XboxLive:get_property( player_index, "SERVERSTATE" ) )
	XboxLive:set_property( player_index, "NUMPLAYERS", self._num_players or 1 )
 	XboxLive:set_property( player_index, "ALLOWDROPIN", settings.numbers[6] )
 	XboxLive:set_property( player_index, "MINLEVEL", settings.numbers[7] )
 	XboxLive:set_property( player_index, "GAMEVERSION", self.GAMEVERSION )

	--[[ 
	local permissions = { "public", "friend", "invisible" }
	
	local level_index, job_index = self:_split_attribute_number( settings.numbers[1], 1000 )
	local lobby_attributes = { 
		owner_name = managers.network.account:username_id(), 
		owner_id = managers.network.account:player_id(), 
		level = level_index,
		difficulty = settings.numbers[2],
		permission = settings.numbers[3], 
		state = settings.numbers[4] or (self._lobby_attributes and self._lobby_attributes.state) or 1,
		min_level = settings.numbers[7] or 0,
		num_players = self._num_players or 1,
		drop_in = settings.numbers[6] or 1,
		job_id = job_index or 0,
	}
	if self._BUILD_SEARCH_INTEREST_KEY then
		lobby_attributes[ self._BUILD_SEARCH_INTEREST_KEY ] = "true"
	end
	
	self._lobby_attributes = lobby_attributes
	
	self.lobby_handler:set_lobby_data( lobby_attributes )]]
	-- self.lobby_handler:set_lobby_type( permissions[settings.numbers[3]] )
	
end

function NetworkMatchMakingXBL:_server_to_numbers( server )
	local properties = server.properties
	return {tonumber( properties.LEVELINDEX ), 
			tonumber( properties.DIFFICULTY ), 
			tonumber( properties.PERMISSION ), 
			tonumber( properties.SERVERSTATE ), 
			tonumber( properties.NUMPLAYERS ),
			tonumber( properties.ALLOWDROPIN ),
			tonumber( properties.MINLEVEL ),}
end

function NetworkMatchMakingXBL:external_address( rpc )
	if not self._session then
		Application:error( "NetworkMatchMakingXBL:translate_to_xnaddr, had no session!" )
		return ""
	end
	
	return XboxLive:external_address( rpc )
end

function NetworkMatchMakingXBL:internal_address( xnaddr )
	if not self._session then
		Application:error( "NetworkMatchMakingXBL:internal_address, had no session!" )
		return ""
	end

	return XboxLive:internal_address( self._session, xnaddr )
end

function NetworkMatchMakingXBL:from_host_lobby_re_opened( status )
	print( "[NetworkMatchMakingXBL::from_host_lobby_re_opened]", self._try_re_enter_lobby, status )
	if self._try_re_enter_lobby == "asked" then
		if status then
			self._try_re_enter_lobby = "open"
		else
			self._try_re_enter_lobby = nil
			managers.network.matchmake:leave_game()
		end
	end
end



function NetworkMatchMakingXBL:_test_search( settings )
	local player_index = managers.user:get_platform_id()
	
	local prop = {} --  LEVELINDEX = 98 }
	prop.MINLEVEL = managers.experience:current_level()
	
	local con = {}
	--prop["min victory points"] = self._last_settings["skill"]-NetworkMatchMakingXBL._SKILL_HALF_RANGE
	--prop["max victory points"] = self._last_settings["skill"]+NetworkMatchMakingXBL._SKILL_HALF_RANGE
	-- prop["Public"] = self._last_settings["public"]
	con["GAME_TYPE"] = settings["game_type"]
	con["game_mode"] = settings["game_mode"]

	-- Ask match making service
	XboxLive:search_session("Find Matches", player_index, 25, prop, con, callback( self, self, "_find_test_server_callback"))
end

function NetworkMatchMakingXBL:_find_test_server_callback( servers, mode )
	if self._cancel_find then
		return
	end
	
	self._last_mode = mode
	
	print( "_find_test_server_callback", mode, inspect( servers ) )
	
	if not servers then
		print( "SEaRCH FAILED" )
		return 
	end
	
	self._test_servers = {}
	for _,server in ipairs( servers ) do
		self._test_server = server
		table.insert( self._test_servers, server )
		print( inspect( server ) )
	end
	
	local player_index = managers.user:get_platform_id()
	
--[[	if( servers and #servers > 0 ) then
		cat_print( 'lobby', "found server, trying to connect")
		self:_try_servers( servers, mode )
	elseif mode == "join_game" then
        self:_call_callback("join_failed")
	else
		self:_create_server( self._last_settings )
	end
	]]
end

function NetworkMatchMakingXBL:_test_join( xs_info, skip_showing_dialog )
	xs_info = xs_info or self._test_server.info
	local player_index = managers.user:get_platform_id()
	
	print( "_test_join", xs_info )
	if self._session then
		XboxLive:leave_local( self._session, player_index )
		XboxLive:delete_session( self._session )
	end
	
	self._session = XboxLive:create_client_session( "live_multiplayer_standard", player_index, 4, 0, xs_info ) -- , callback( self, self, "_join_server_callback") )
	
	if not self._session then
		print( "FAILED CREATE CLIENT SESSION" )
		return
	end
	
	XboxLive:join_local( self._session, player_index, true )
	-- XboxLive:join_remote( self._session, player_index, true )

	print( "self._session", self._session )
	
		local result = "failed" -- !!!
	
		print( "[NetworkMatchMakingXBL:join_server:f]" )
		managers.system_menu:close( "join_server" )
		if result == "success" then
			print( "Success!" )
			-- self.lobby_handler = handler

			-- local _, host_id, owner = self.lobby_handler:get_server_details()
			print( "[NetworkMatchMakingXBL:join_server] server details", self._session:ip(), self._session:id() )

			print( "Gonna handshake now!" )
			self._server_rpc = Network:handshake( self._session:ip(), nil, "TCP_IP" )
			print( "Handshook!" )
			
			print( "Server RPC:", self._server_rpc and self._server_rpc:ip_at_index(0) )
			
			if not self._server_rpc then
				return
			end
			
			-- self.lobby_handler:setup_callbacks(NetworkMatchMakingXBL._on_memberstatus_change, NetworkMatchMakingXBL._on_data_update, NetworkMatchMakingXBL._on_chat_message )
			
			managers.network:start_client()
			managers.menu:show_waiting_for_server_response( { cancel_func = function() managers.network:session():on_join_request_cancelled() end } )
	
			local joined_game = function( res, level_index, difficulty_index, state_index )
				managers.system_menu:close( "waiting_for_server_response" )
				print( "[NetworkMatchMakingXBL:join_server:joined_game]", res, level_index, difficulty_index, state_index )
				if res == "JOINED_LOBBY" then
					managers.network.voice_chat:open_session()
					MenuCallbackHandler:crimenet_focus_changed( nil, false )
					managers.menu:on_enter_lobby() -- In MenuManagerPD2
					-- managers.menu:close_menu( "menu_main" )
					-- managers.menu:open_menu( "lobby_menu" )
				elseif res == "JOINED_GAME" then	-- The desired host has answered positively. load the level
					-- managers.network.voice_chat:set_drop_in( { room_id = self._room_id } )
					local level_id = tweak_data.levels:get_level_name_from_index( level_index )
					--local level_name = tweak_data.levels[ level_id ].world_name
					-- managers.network:session():load_level( level_name, nil, nil, nil, level_id, nil )
					Global.game_settings.level_id = level_id
					managers.network:session():ok_to_load_level()
				elseif res == "KICKED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					-- managers.network:stop_network( true )
					managers.network:queue_stop_network()
					managers.menu:show_peer_kicked_dialog()
				elseif res == "TIMED_OUT" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_request_timed_out_dialog()
				elseif res == "GAME_STARTED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_game_started_dialog()
				elseif res == "DO_NOT_OWN_HEIST" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_does_not_own_heist()
				elseif res == "CANCELLED" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
				elseif res == "FAILED_CONNECT" then
					managers.network.matchmake:leave_game()
					managers.network.voice_chat:destroy_voice()
					managers.network:queue_stop_network()
					managers.menu:show_failed_joining_dialog()
				else
					Application:error("[NetworkMatchMakingXBL:join_server] FAILED TO START MULTIPLAYER!", res)
				end
			end
		
			managers.network:join_game_at_host_rpc( self._server_rpc, joined_game )
		else
			managers.menu:show_failed_joining_dialog()
			self:search_lobby( self:search_friends_only() )
		end
	-- end
	
	
	

	--network handshake
	--[[local rpc = Network:handshake( session:ip(), managers.network._standard_port )
	if( not rpc ) then
		XboxLive:leave_local(self._session, player_index)
		-- managers.network.shared:delete_session( session )		
		Application:error( "Didnt get a ip back from the session" )
		return
	end]]
end

function NetworkMatchMakingXBL:_join_server_callback()
end


function NetworkMatchMakingXBL:_test_create(settings)
	print( "settings\n", inspect( settings ) )
	
	if settings == nil then
		--[[cat_print_inspect( "lobby", "NetworkMatchMakingXBL:_create_server: settings is nil. Cant create a server with no parameters." )
		cat_stack_dump("lobby")	
		self:_call_callback( "cancel_done" )
		return]]
	end
	
	local gt = settings["game_type"]
	local gm = settings["game_mode"]
	local skl = settings["skill"]
	-- local public = settings["public"]
	
	local player_index = managers.user:get_platform_id()

	--[[if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end]]

	XboxLive:set_context( player_index, "GAME_TYPE", gt )
	XboxLive:set_context( player_index, "game_mode", gm )

	-- XboxLive:set_property( player_index, "Public", public)

	--leave existing, if there is any
	if( self._session and self._session:state() == "started" ) then
		XboxLive:leave_local( self._session, player_index )
		XboxLive:delete_session( self._session, function() print( "DELETED SESSION") end )
	end
	
	-- managers.network.shared:delete_session( self._session )		


	-- local create_prop = { "live_multiplayer_standard", "-uses_presence" } -- , "join_in_progress_disabled"}
	local create_prop = { "group_lobby" } -- , "join_in_progress_disabled"}
	if( self._uses_arbitration == true ) then
		table.insert( create_prop, "uses_arbitration" )
	end
	
	--slots
	local pub_slots = self.OPEN_SLOTS
	local priv_slots = 0
	--[[self._private = false
	if( public == 0 ) then
		pub_slots = 0
		priv_slots = self.OPEN_SLOTS
		self._private = true
	end]]

	--create session
	print( "creating session\n", inspect( create_prop ) )
	local session = XboxLive:create_session( "live_multiplayer_standard", player_index, pub_slots, priv_slots, callback( self, self, "_create_server_callback") )
	print( "create return value", session )
end

function NetworkMatchMakingXBL:_create_server_callback( session  )
	if self._cancel_find then
		-- If we managed to create one but have cancled we need to remove the one we created		
		-- managers.network.shared:delete_session( session )		
		cat_print( "lobby", "create_server canceled")
		return
	end

	print( "NetworkMatchMakingXBL:_create_server_callback")
	local player_index = managers.user:get_platform_id()
	
	if( not session ) then
		print( "CREATE SESSION FAILED" ) 
		-- self:_call_callback( "create_failed" )
		return
	end

	XboxLive:join_local( session, player_index, self._private )

	-- Set self._session
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end

	self._session = session
	print( " Created mm session ", self._session:id() ) 
	
	self._players = {}
	self._server_rpc = nil
	--[[self:_is_server( true )
	self:_is_client( false )

	managers.network:bind_port()

	self:_call_callback( "found_game", self._session, true )
	self:_call_callback( "player_joined", {
		player_id=managers.network.account:player_id(),
		group_id=managers.network.group:group_id(),
		managers.network.account:username(),
		rpc=Network:self(),
		external_address=nil,
		} )]]
end


--[[-----------------------------------------------------------------------------------------------

 Match making abstraction layer

 find_game(settings)
	This method should always give a valid game through the "found_game" callback. If the
	matchmaking service fails to find a game then the AL needs to create a game and make
	it available for other people to find.
	Settings: table
		"game mode": string
			Which game mode to play. Current modes: "dm", "tdm", "race", "ctf"
		"game type": string
			"ranked" or "not ranked"
		"num players": int
			How many players the game should be able to host

 create_private_game(settings)
	Will create a private game that only the people in the group can play in. When created
	the "found_game" callback is called, so the game can simply replace find_game() with
	create_private_game() and the flow would remain the same.

 cancel_find()
 	Since the find_game can, in theory, take infinite amount of time the player needs to have the
 	possibility to cancel a find.

 register_callback(event, callback):
	event: string
		The event that the function should be linked to. Events:
			"create_failed"		: When we for some reason fail to create a game.
			"found_game"
			"player_joined"
			"player_left"
			"game_started"		: When start_game has finished executing
			"unknown_error"		: When something unexpected happened. If this callback is called we
								  should probably gracefully exit the current menu
			----- DEPRECATED ------ "start_game"		: When the server have decided to start the game, all clients will receive
			----- DEPRECATED ------ 					  this callback. They should call start_game() at this point.

	callback: function
		The function to use

 join_game(id)
	When we've found a game through the matchmaking service or received a game ID from our
	group leader we need to join that specific game.

 leave_game()
	Leave the current game

 start_game()
	Simply register that the game is running and no more people can join, start ranking etc.

 end_game()
	The game should end. All the clients should now send all their game statistics somewhere
	for proper ranking. That "somewhere" will have to be in this class, but the details
	have not been decided yet.

 signin_state(): string
 	Return if we're signed into our matchmaking service or not
 	(might be expanded, that's why we don't return a boolean)
 	Possible returns:
 		"signed in"
 		"not signed in"

 is_game_owner(): boolean
 	Returns true if this peer is the game owner. Returns false otherwise

 remove_ping_watch()
	Remove the ping watch related to matchmaking. I.e., on the server we stop ping watching
	the clients and on clients we stop ping watching our server

----

Commented code is under construction or is not valid right now.

-----------------------------------------------------------------------------------------------]]--



--[[

Debug code that keeps track of all sessions created. This does not work
when we start creating sessions asyncronously since we doesn't overload
the callback. It could be done by creating a new callback that we pass
to Xbox LIVE and that callback calls the callback registered by the user.

No need to implement that at the moment though.
--]]
--[[if XboxLive then
	Global.xbl_sessions = Global.xbl_sessions or {}
	Global.xbl_get_session_debug_index = function(session)
		for k,v in pairs(Global.xbl_sessions) do
			if v.session and (v.session:id() == session:id()) then
				return k
			end
		end

		Application:throw_exception("Session request="..tostring(session).." (id="..tostring(session:id()))
		return nil
	end

	Global.xbl_get_session_debug = function(session)
		local k = Global.xbl_get_session_debug_index(session)
		return Global.xbl_sessions[k]
	end

	Global.xbl_create_session = Global.xbl_create_session or XboxLive.create_session
	getmetatable(XboxLive).create_session = function(s, t, p, pubs, pris, cb)
		if cb then
			local current_stack = Application:stack()
			local new_cb = function(session, ...)
				if session then
					local id = Global.xbl_session_id or 0
					local dbg = {session=session, _1_cr_cb=true, id=id, stack=current_stack} -- created
					table.insert(Global.xbl_sessions, dbg)
					Global.xbl_session_id = id + 1
				end
				cb(session, ...)
			end
			Global.xbl_create_session(s, t, p, pubs, pris, new_cb)
		else
			local ret = Global.xbl_create_session(s, t, p, pubs, pris)
			if ret == nil then
				return nil
			end
			local id = Global.xbl_session_id or 0
			local dbg = {session = ret, _1_cr=true, id=id, stack=Application:stack()} -- created
			table.insert(Global.xbl_sessions, dbg)
			Global.xbl_session_id = id + 1
			return ret
		end
	end
    
	Global.xbl_create_client_session = Global.xbl_create_client_session or XboxLive.create_client_session
	getmetatable(XboxLive).create_client_session = function(...)
		local ret = Global.xbl_create_client_session(...)
		if ret == nil then
			return nil
		end
		local id = Global.xbl_session_id or 0
		local dbg = {session = ret, _1_clcr=true, id=id, stack=Application:stack()} -- client created
		table.insert(Global.xbl_sessions, dbg)
		Global.xbl_session_id = id + 1
		return ret
	end

	Global.xbl_create_system_link_session = Global.xbl_create_system_link_session or XboxLive.create_system_link_session
	getmetatable(XboxLive).create_system_link_session = function(...)
		local ret = Global.xbl_create_system_link_session(...)
		if ret == nil then
			return nil
		end
		local id = Global.xbl_session_id or 0
		local dbg = {session = ret, _1_cr_sl=true, id=id, stack=Application:stack()} -- created
		table.insert(Global.xbl_sessions, dbg)
		Global.xbl_session_id = id + 1
		return ret
	end

	Global.xbl_start_session = Global.xbl_start_session or XboxLive.start_session
	getmetatable(XboxLive).start_session = function(xbl, session, ...)
		local ret = Global.xbl_start_session(xbl, session, ...)
		local dbg = Global.xbl_get_session_debug(session)
		dbg._2_strt = true -- started
		return ret
	end

	Global.xbl_end_session = Global.xbl_end_session or XboxLive.end_session
	getmetatable(XboxLive).end_session = function(xbl, session, ...)
		local ret = Global.xbl_end_session(xbl, session, ...)
		local dbg = Global.xbl_get_session_debug(session)
		dbg._3_end = true -- ended
		return ret
	end

	Global.xbl_delete_session = Global.xbl_delete_session or XboxLive.delete_session
	getmetatable(XboxLive).delete_session = function(xbl, session, ...)
		cat_print("session_handling_xbox", "XboxLive:delete_session()" )
		cat_stack_dump("session_handling_xbox")	
		local dbg = Global.xbl_get_session_debug(session)
		local ret = Global.xbl_delete_session(xbl, session, ...)
		dbg._4_del = true -- deleted
		return ret
	end
end

]]


--[[
NetworkMatchMakingXBL = NetworkMatchMakingXBL or class()

function NetworkMatchMakingXBL:init()
	cat_print( 'lobby', "match = NetworkMatchMakingXBL")

	self._callbacks = {}
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end

	self._session = nil
	self._last_settings = nil

	self._is_server_var = false
	self._is_client_var = false

	self._players = {}

	self._uses_arbitration = true
	
	self:_load_global()

	self._private 			= false
	self._force_private 	= nil 		-- used for join_game()
	self._lostconnection 	= false		-- True when disconnected so we know not to send any more messages to server as we close down.
	self._cancel_find 		= false		-- True when we have cancel a find and should ignore any responding servers.
	self.OPEN_SLOTS = NetworkGeneric._MAX_PLAYERS
	
	self._disconnect_callback_id = managers.platform:add_event_callback( "disconnect", callback( self, self, "disconnect_callback" ) )
end

function NetworkMatchMakingXBL:disconnect_callback( ... )
	local arguments = {...}

	cat_print( 'lobby', "NetworkMatchMakingXBL:disconnect_callback - disconnect : " .. tostring( arguments[1] ) )

	-- Send leave messages for all clients to screen
	for k, v in pairs( self._players ) do
		self:_call_callback( "player_left", { player_id=v.xuid, reason="Not logged in." } )
	end

	-- Nil current group and clear.
	if self._session then
		if arguments[1] then
			local player_index = managers.platform:get_active_player_index()

			if( not player_index ) then
				Application:error( "Player map not ready yet." )
				player_index = 0
			end

			XboxLive:leave_local(self._session, player_index)
		end
		
		cat_print("lobby", "Deleting mm session", self._session:id() )
		managers.network.shared:delete_session( self._session )
		
		if alive(self._session)  then
			cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
			cat_stack_dump("lobby")	
		end

		self._session = nil
	end

	self:_clear()
	self._cancel_find = true
end


function NetworkMatchMakingXBL:find_game(settings, ignore_settings)
	local player_index = managers.platform:get_active_player_index()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end
	
	self._force_private = nil --Reset this flag just incase the player breaks out somehow, and this happen to be still on.
	self._cancel_find 	= false
	
	cat_print_inspect("lobby", settings)

	-- Convert settings from game to XBL
	if( not ignore_settings ) then
		settings["public"] = 1
		self._last_settings = self:_translate_settings(settings)
	end
	
	--sigma/mu for group searches
	local xuids = { managers.network.account:player_id() }
	for k, v in pairs( managers.network.group._players ) do
		table.insert( xuids, v.xuid )
	end
	if( #xuids > 1 ) then
		XboxLive:read_stats( xuids, self._last_settings.skill_board, callback( self, self, "_find_game_group" ) )
		return
	end

	-- Fill XBL match making service properties
	local prop = {}
	local con = {}
	--prop["min victory points"] = self._last_settings["skill"]-NetworkMatchMakingXBL._SKILL_HALF_RANGE
	--prop["max victory points"] = self._last_settings["skill"]+NetworkMatchMakingXBL._SKILL_HALF_RANGE
	prop["Public"] = self._last_settings["public"]
	con["GAME_TYPE"] = self._last_settings["game_type"]
	con["game_mode"] = self._last_settings["game_mode"]

	-- Ask match making service
	XboxLive:search_session("Find Matches", player_index, 25, prop, con, callback( self, self, "_find_server_callback"))
end

function NetworkMatchMakingXBL:create_private_game(settings)
	settings["public"] = 0
	self._cancel_find = false
	self:_create_server(self:_translate_settings(settings))
end

function NetworkMatchMakingXBL:cancel_find()
	cat_print("lobby", "NetworkMatchMakingXBL cancel find for MP games")

	self._cancel_find = true

	-- Close any Session and server Rpc
	if( self._session and self._session:state() == "started" ) then
		XboxLive:leave_local( self._session, player_index )
	end
	managers.network.shared:delete_session( self._session )		
		
	if( self._server_rpc ) then
		Network:reset_connection( self._server_rpc )
	end
	
	if alive(self._session) and self._session:state() == "started" then
		cat_print("lobby", "Trying to remove self._session in state started" )
		cat_stack_dump("lobby")	
	end
	
	self._server_rpc 	= nil
	self._session 		= nil
	self._try_time		= nil
	
	self:_call_callback( "cancel_done" )
end

function NetworkMatchMakingXBL:register_callback(event, callback)
	self._callbacks[ event ] = callback
end

function NetworkMatchMakingXBL:join_game(id, private)
	if not managers.network.group:is_group_leader() then
		self._cancel_find = false
	end
	
	local player_index = managers.platform:get_active_player_index()
	
	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end
	
	self._force_private = private
	local f = function(session)
		self:_find_server_callback(session, "join_game")
	end
	XboxLive:search_session_by_id( id, player_index, f )
end

function NetworkMatchMakingXBL:leave_game()
	self:remove_ping_watch()

	if( self:_is_client() ) then
		if( self._server_rpc and self._lostconnection == false) then
			self._server_rpc:unregister_player( true, managers.network.account:player_id() )
		end

		self._time_to_close = TimerManager:wall():time() + 2
		if managers.network.group:_is_client() then
			self:_call_callback("group_leader_left_match")
		end
		return
	end

	--
	if( self:_is_server() ) then
		for k, v in pairs( self._players ) do
			v.rpc:mm_server_left()
		end
		self._time_to_close = TimerManager:wall():time() + 2
		return
	end

	self:_clear()
	cat_print("lobby", "Now calling left game in NetworkMatchMakingXBL:leave_game")
	
	self:_call_callback( "left_game" )
end

function NetworkMatchMakingXBL:start_game()
	managers.networkgame:save_players_list(self._players)	

	-- If the game is arbitrated then we need to do the arbitration call async and wait for callback
	-- Otherwise we can do start_session immediately
	local call_start_immediately = true

	if( self:_is_server() ) then
		if( self._uses_arbitration == true ) then
			-- Game uses arbitration. We need to wait for this operation to be done before starting the session
			call_start_immediately = false
			XboxLive:arbitration_register( self._session, callback(self, self, "_cb_arbitration_register_done") )
		end
	end

	-- Game isn't arbitrated. We can start the session immediately
	if call_start_immediately == true then
		self:_cb_arbitration_register_done()
	end
end

function NetworkMatchMakingXBL:_cb_arbitration_register_done()
	XboxLive:start_session( self._session, callback(self, self, "_cb_start_session_done") )
end

function NetworkMatchMakingXBL:_cb_start_session_done(success)
	if success then
		self:_call_callback( "game_started" )
	else
		self:_call_callback( "game_start_failed" )
	end
end

function NetworkMatchMakingXBL:end_game()
	if not self._session then
		--Application:throw_exception("Can't call end_game without a started game")
		return
	end

end



function NetworkMatchMakingXBL:destroy_game()
	-- Moved end_session from end_game to here because we
	-- need to submit scores to Xbox LIVE after end_game
	-- but the session can't be ended when submitting
	-- score to Xbox LIVE
	if( self._disconnect_callback_id ) then
		managers.platform:remove_event_callback( self._disconnect_callback_id )
		self._disconnect_callback_id = nil
	end

	managers.network.shared:delete_session( self._session )
	
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end
	
	self._session = nil
end

function NetworkMatchMakingXBL:update( time )
	managers.network.shared:update_deadsessions()

	if( self._try_time ) then
		if( TimerManager:wall():time() - self._try_time > 5 ) then
			self:_try_servers()
		end
	end

	if( self._restart_search and TimerManager:wall():time() > self._restart_search ) then
		self._restart_search = nil
		self:find_game( nil, true )
	end

	if( self._time_to_close and TimerManager:wall():time() > self._time_to_close ) then
		cat_print("lobby", "_time_to_close" )
		
		if( self._session and self._session:state() ~= "ended" ) then
			cat_print("lobby", "Delete Session in progress" )

			if ( self._server_rpc ) then
				Network:reset_connection( self._server_rpc )
				self._server_rpc = nil
			end
			
			if( self._players ) then
				for k, v in pairs( self._players ) do
					if( v.rpc ) then
						Network:reset_connection( v.rpc )
					end
				end
			end
			
			managers.network.shared:delete_session( self._session )
			
			if alive(self._session)  then
				cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
				cat_stack_dump("lobby")	
			end
						
			self._session = nil
			self._players = {}
		end
		self._time_to_close = nil

		self:_call_callback( "left_game" )
	end
end

function NetworkMatchMakingXBL:is_game_owner()
	return self:_is_server() == true
end

function NetworkMatchMakingXBL:remove_ping_watch()
	cat_print("lobby", "NetworkMatchMakingXBL:remove_ping_watch()")
	if self:is_game_owner() then
		cat_print("lobby", "is_game_owner")
		-- We are server.
		-- Shut down ping watching to all clients
		for k, v in pairs( self._players ) do
			if( v.rpc ) then
				cat_print("lobby", "remove ping watch for player RPC. Player key="..tostring(k).." client name="..tostring(v.name))
				managers.network.generic:ping_remove( v.rpc, true )
			else
				Application:throw_exception("No RPC to client "..tostring(v.name)..".")
			end
		end
	else
		cat_print("lobby", "We are game client")
		-- We are game client.
		-- Shut down the ping watch to the server
		managers.network.generic:ping_remove( self._server_rpc, true )
	end
end

function NetworkMatchMakingXBL:get_mm_id( name )
	if( name == managers.network.account:username() ) then
		return managers.network.account:player_id()
	else
		for k, v in pairs( self._players ) do
			if( v.name == name ) then
				return v.xuid
			end
		end
	end
	
	return nil
end

function NetworkMatchMakingXBL:is_closing()
	if( self._time_to_close ) then
		return true
	end
	return false
end

function NetworkMatchMakingXBL:end_session()
	if( (not managers.network._matchabort) and self._session and self._session:state() == "started" ) then
		XboxLive:end_session( self._session )
	end
end

function NetworkMatchMakingXBL:session_ended()
	if self._session then
		return self._session:state() == "ended"
	end
	return false
end

function NetworkMatchMakingXBL:session_started()
	if self._session then
		return self._session:state() == "started"
	end
	return false
end

function NetworkMatchMakingXBL:say( message )
	if( self:_is_server() ) then
		for k, v in pairs( self._players ) do
			v.rpc:say_toclient( message)
		end
	end
end

function NetworkMatchMakingXBL:membervoted( player, votes )
	if( self:_is_server() ) then
		for k, v in pairs( self._players ) do
			v.rpc:membervoted_toclient( player, votes )
		end
	end
end

---------------------------------------------------------------------------------------------------
--
-- Internal functions
--
---------------------------------------------------------------------------------------------------
NetworkMatchMakingXBL._SKILL_HALF_RANGE = 4

function NetworkMatchMakingXBL:_clear()
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end
	
	self._session = nil
	self._last_settings = nil

	self._is_server_var = false
	self._is_client_var = false

	self._players = {}

	self._uses_arbitration = true
	
	self._private = false
	self._force_private = nil
	self._lostconnection = false
	self._try_time = nil
end

function NetworkMatchMakingXBL:_load_global()
	if( Global.live and Global.live.match ) then
		if alive(self._session) and self._session:state() == "started" then
			cat_print("lobby", "Trying to remove self._session in state started" )
			cat_stack_dump("lobby")	
		end
		self._session 		= Global.live.match._session
		self._is_server_var = Global.live.match._is_server
		self._is_client_var = Global.live.match._is_client
		self._players 		= Global.live.match._players
		self._last_settings = Global.live.match._settings
		self._uses_arbitration = Global.live.match._arbitration

		Global.live.match = nil
	end
end

function NetworkMatchMakingXBL:_save_global()
	if( Global.live and Global.live.match ) then
		Global.live.match = nil
	end

	if( not Global.live ) then
		Global.live = {}
	end

	Global.live.match = {}

	Global.live.match._session 		= self._session
	Global.live.match._is_server 	= self._is_server_var
	Global.live.match._is_client 	= self._is_client_var
	Global.live.match._players 		= self._players
	Global.live.match._settings 	= self._last_settings
	Global.live.match._arbitration  = self._uses_arbitration
end

function NetworkMatchMakingXBL:_find_game_group( values )
	local mu_t = {}
	local sigma_t = {}
	
	for k, v in pairs( values ) do
		if( k ~= "total_rows" ) then
			table.insert( mu_t, v["gamermu"] )
			table.insert( sigma_t, v["gamersigma"] )
		end
	end
	
	local mu, sigma = XboxLive:calculate_skill( mu_t, sigma_t )
	
	-- Fill XBL match making service properties
	local prop = {}
	local con = {}
	--prop["min victory points"] = self._last_settings["skill"]-NetworkMatchMakingXBL._SKILL_HALF_RANGE
	--prop["max victory points"] = self._last_settings["skill"]+NetworkMatchMakingXBL._SKILL_HALF_RANGE
	prop["Public"] = self._last_settings["public"]
	con["GAME_TYPE"] = self._last_settings["game_type"]
	con["game_mode"] = self._last_settings["game_mode"]
	
	prop["GAMERMU"] = mu
	prop["GAMERSIGMA"] = sigma
	
	-- Ask match making service
	local player_index = managers.platform:get_active_player_index()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end

	XboxLive:search_session("Find Matches", player_index, 25, prop, con, callback( self, self, "_find_server_callback"))
end

function NetworkMatchMakingXBL:_set_nonce( key )
	self._session:set_nonce( key )
	
	XboxLive:arbitration_register( self._session )
end

function NetworkMatchMakingXBL:_translate_signin_state(xbl_state)
	if xbl_state == "signed_in_to_live" then
		return "signed in"
	end

	return "not signed in"
end

function NetworkMatchMakingXBL:_call_callback( name, ... )
	if( self._callbacks[ name ] ) then
		return self._callbacks[ name ]( ... )
	else
		Application:error( "Callback " .. name .. " not found." )
	end
end
]]
function NetworkMatchMakingXBL:_is_server( set )
 	if( (set == true) or (set == false) ) then
		self._is_server_var = set
	else
		return self._is_server_var
	end
end

function NetworkMatchMakingXBL:_is_client( set )
 	if( (set == true) or (set == false) ) then
		self._is_client_var = set
	else
		return self._is_client_var
	end
end
--[[
function NetworkMatchMakingXBL:_translate_settings(settings)
	if not settings then
		-- Extra harsh on the reference implementation of the AL
		Application:throw_exception("find_game must have a settings table to find a game that match")
		return nil
	end

	-- Skill level. Shouldn't actually be used on the Xbox.
	if not settings["skill"] then
		-- Extra harsh on reference implementation
		Application:throw_exception("find_game settings must have a 'skill' entry")
		return nil
	end
	local skill = settings["skill"]

	-- If the game is "public" it can be found by everyone
	if not settings["public"] then
		-- Extra harsh on reference implementation
		Application:throw_exception("find_game settings must have a 'public' entry")
		return nil
	end
	local public = settings["public"]

	-- Read game type from the settings array
	local game_type = nil
	if settings["game_type"] == "not ranked" then
		game_type = "STANDARD"
		self._uses_arbitration = false
		Application:throw_exception("There shouldn't be any STANDARD matches, only RANKED")
	elseif settings["game_type"] == "ranked" then
		game_type = "RANKED"
		self._uses_arbitration = true
	end
	if not game_type then
		-- Extra harsh on reference implementation
		Application:throw_exception("game_type in settings not a valid value")
		return nil
	end

	local game_mode = string.upper(settings["game_mode"]) --xlast now wants uppercase, this is the best place to set to uppercase without trashing everything else
	local skill_board = game_mode .. "_skill_" .. string.lower(game_type)

	-- XBL usable settings
	local xbl_settings = {}
	xbl_settings.game_type = game_type
	xbl_settings.game_mode = game_mode
	xbl_settings.skill = skill
	xbl_settings.skill_board = skill_board
	xbl_settings.public = public

	return xbl_settings
end

function NetworkMatchMakingXBL:_find_server_callback( servers, mode )
	if self._cancel_find then
		return
	end
	
	self._last_mode = mode
	
	cat_print( 'lobby', "find_server_callback")
	
	if( servers and #servers > 0 ) then
		cat_print( 'lobby', "found server, trying to connect")
		self:_try_servers( servers, mode )
	elseif mode == "join_game" then
        self:_call_callback("join_failed")
	else
		self:_create_server( self._last_settings )
	end
end

function NetworkMatchMakingXBL:_create_server(settings)
	cat_print( 'lobby', "no servers returned, creating one.")
	
	cat_print_inspect( "lobby", "settings\n", settings )
	
	if settings == nil then
		cat_print_inspect( "lobby", "NetworkMatchMakingXBL:_create_server: settings is nil. Cant create a server with no parameters." )
		cat_stack_dump("lobby")	
		self:_call_callback( "cancel_done" )
		return
	end
	
	local gt = settings["game_type"]
	local gm = settings["game_mode"]
	local skl = settings["skill"]
	local public = settings["public"]
	
	local player_index = managers.platform:get_active_player_index()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end

	XboxLive:set_context( player_index, "GAME_TYPE", gt )
	XboxLive:set_context( player_index, "game_mode", gm )

	XboxLive:set_property( player_index, "Public", public)

	--leave existing, if there is any
	if( self._session and self._session:state() == "started" ) then
		XboxLive:leave_local( self._session, player_index )
	end
	managers.network.shared:delete_session( self._session )		


	local create_prop = {"live_multiplayer_standard", "-uses_presence", "join_in_progress_disabled"}
	if( self._uses_arbitration == true ) then
		table.insert( create_prop, "uses_arbitration" )
	end
	
	--slots
	local pub_slots = self.OPEN_SLOTS
	local priv_slots = 0
	self._private = false
	if( public == 0 ) then
		pub_slots = 0
		priv_slots = self.OPEN_SLOTS
		self._private = true
	end

	--create session
	cat_print_inspect( "lobby", "creating session\n", create_prop )
	local session = XboxLive:create_session( create_prop, player_index, pub_slots, priv_slots, callback( self, self, "_create_server_callback") )
end

function NetworkMatchMakingXBL:_create_server_callback( session  )
	if self._cancel_find then
		-- If we managed to create one but have cancled we need to remove the one we created		
		managers.network.shared:delete_session( session )		
		cat_print( "lobby", "create_server canceled")
		return
	end

	cat_print( "lobby", "_create_server_callback")
	local player_index = managers.platform:get_active_player_index()
	
	if( not session ) then
		self:_call_callback( "create_failed" )
		return
	end

	XboxLive:join_local( session, player_index, self._private )

	-- Set self._session
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end

	self._session = session
	cat_print("lobby", "Created mm session ", self._session:id() ) 
	
	self._players = {}
	self._server_rpc = nil
	self:_is_server( true )
	self:_is_client( false )

	managers.network:bind_port()

	self:_call_callback( "found_game", self._session, true )
	self:_call_callback( "player_joined", {
		player_id=managers.network.account:player_id(),
		group_id=managers.network.group:group_id(),
		managers.network.account:username(),
		rpc=Network:self(),
		external_address=nil,
		} )
end

function NetworkMatchMakingXBL:_try_servers( servers )
	if self._cancel_find then
		return
	end
	
	self._players = {}
	
	if( servers ) then
		self._try_id = 1
		self._try_server_list = servers
	end

	if self._try_id <= #self._try_server_list then
		cat_print( 'lobby', "trying #" .. tostring(self._try_id) )
		self:_join_server( self._try_server_list[ self._try_id ] )
		if( self._try_id ) then
			self._try_id = self._try_id + 1
		end
	else
		cat_print( 'lobby', "end of servers to try" )
		self._try_time = nil
		if( self._server_rpc ) then
			Network:reset_connection( self._server_rpc )
		end
		if self._last_mode == "join_game" then
			self:_call_callback("join_failed")
		else
			self:_create_server( self._last_settings )
		end
	end
end

function NetworkMatchMakingXBL:_join_server(server)
	local player_index = managers.platform:get_active_player_index()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end
	
	--leave existing, if there is any
	if( self._session ) then
		if( self._session and self._session:state() == "started" ) then
			XboxLive:leave_local( self._session, player_index )
		end
		managers.network.shared:delete_session( self._session )		
		
		if( self._server_rpc ) then
			Network:reset_connection( self._server_rpc )
		end

		if alive(self._session)  then
			cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
			cat_stack_dump("lobby")	
		end

		self._session = nil
		self._server_rpc = nil
	end
	
	self._players = {}
	self._server_rpc = nil
	
	--set context
	for k, v in pairs( server.contexts ) do
		XboxLive:set_context( player_index, k, v )
	end

	self._try_time = TimerManager:wall():time()

	local players = 1
	for k, v in pairs( managers.network.group._players ) do
		players = players + 1
	end
	
	--check if its a private game or not
	local private_game = true
	if( server.properties.Public and server.properties.Public == 1 ) then
		private_game = false
	end
	
	--force set, for join_id()
	if( self._force_private ~= nil ) then
		private_game = self._force_private
		self._force_private = nil
	end

	--just check the limit for safety
	if( private_game == true ) then
		if( server.open_private_slots < players ) then
			cat_print( 'lobby', "Not enough free private slots" )
			return
		end
	else
		if( server.open_public_slots < players ) then
			cat_print( 'lobby', "Not enough free public slots" )
			return
		end
	end

	local create_prop = {"live_multiplayer_standard", "-uses_presence", "join_in_progress_disabled"}
	if( self._uses_arbitration == true ) then
		table.insert( create_prop, "uses_arbitration" )
	end

	--slots
	local pub_slots = self.OPEN_SLOTS
	local priv_slots = 0
	self._private = false
	if( private_game == true ) then
		pub_slots = 0
		priv_slots = self.OPEN_SLOTS
		self._private = true
	end
	
	local session = XboxLive:create_client_session( create_prop, player_index, pub_slots, priv_slots, server.info )
	if( not session ) then
		Application:error( "Didnt get a session from the create_client_session" )
		return
	end

	XboxLive:join_local( session, player_index, self._private )

	--network handshake
	local rpc = Network:handshake( session:ip(), managers.network._standard_port )
	if( not rpc ) then
		XboxLive:leave_local(session, player_index)
		managers.network.shared:delete_session( session )		
		Application:error( "Didnt get a ip back from the session" )
		return
	end

	--
	local xnaddr = XboxLive:external_address(rpc)
	cat_print("lobby", xnaddr)
	cat_print("lobby", server.address)
	cat_print("lobby", "id="..session:id())
	cat_print("lobby", "state="..session:state())

	--
	self._server_rpc = rpc
	-- Set self._session
	if alive(self._session)  then
		cat_print("lobby", "Trying to remove self._session", self._session:id(), "in state", self._session:state() )
		cat_stack_dump("lobby")	
	end

	self._session = session

	managers.network:bind_port()

	--check so server is alive
	rpc:test_message("This is a Unreliable Message")
	rpc:is_mm_server_alive()
end

function NetworkMatchMakingXBL:xbh_server_is_alive( server )
	if self._cancel_find then
		return
	end
	
	-- If we time out waiting for a server and create our own it can happen that our _server_rpc is nil when the answer for the server arrive later. Mantis: 0019218  
	if self._server_rpc == nil then
		return
	end
	
	if( self._server_rpc:ip_at_index(0) ~= server:ip_at_index(0) ) then
		Application:error( "not the right server...." )
		return
	end

	--clear values
	self._try_info = {}
	self._try_info.time 		= self._try_time
	self._try_info.servers 		= self._try_server_list
	self._try_info.id 			= self._try_id
		self._try_time 			= nil
		self._try_servers_list	= nil
		self._try_id 			= nil

	self:_is_server(false)
	self:_is_client(true)

	if( managers.network.group:is_group_leader() ) then
		local players = managers.network.group._players

		local x_num = 1
		local xuids = {}
		table.insert( xuids, managers.network.account:player_id() )

		for k, v in pairs( players ) do
			x_num = x_num + 1
			table.insert( xuids, v.xuid )
		end

		local p = {}
		for i=1,NetworkGeneric._MAX_PLAYERS,1 do
			p[ "player" .. tostring(i) ] = xuids[i] or managers.network.account:player_id()
		end

		self._server_rpc:group_register( x_num, p )
	else
		local xnaadr = XboxLive:external_address( Network:self() )
		managers.network.generic:ping_watch( self._server_rpc, true, callback( self, self, "_server_timed_out" ) )
		self._server_rpc:register_player( true, managers.network.account:username(), managers.network.account:player_id(), managers.network.group:group_id(), xnaadr )
	end
end

function NetworkMatchMakingXBL:xbh_group_register_ok( rpc )
	self._try_info = nil

	self:_is_server( false )
	self:_is_client( true )

	managers.network.generic:ping_watch( self._server_rpc, true, callback( self, self, "_server_timed_out" ) )

	self:_call_callback( "found_game", self._session )
	
	local xnaadr = XboxLive:external_address( Network:self() )
	self._server_rpc:register_player( true, managers.network.account:username(), managers.network.account:player_id(), managers.network.group:group_id(),xnaadr )
end

function NetworkMatchMakingXBL:xbh_group_register_fail( rpc )
	self._try_time 			= self._try_info.time
	self._try_server_list 	= self._try_info.servers
	self._try_id 			= self._try_info.id

	self._try_info = nil
end

function NetworkMatchMakingXBL:xbh_group_register( num_xuids, xuids, rpc )
	local p = 1 + #self._players + self:_call_callback( "reserved_slots" )
	
	cat_print( "lobby", "players in now", p, "vs", self.OPEN_SLOTS, "+", num_xuids )

	local can_join = self:_call_callback( "can_join" )	
	if can_join == false then
		cat_print( "lobby", "Can join is off as server is starting, group_register ignored." )
	end

	if( p+num_xuids > self.OPEN_SLOTS or not can_join) then
		rpc:group_register_fail()
		return
	end
	
	for i=1,num_xuids,1 do
		self:_call_callback( "reserv_slot", xuids[ "player" .. tostring(i) ]  )
	end
	
	rpc:group_register_ok()
end

function NetworkMatchMakingXBL:xbh_set_nonce(key)
	managers.network.matchmake:_set_nonce(key)
end

function NetworkMatchMakingXBL:_register_player( name, xuid, group_id, xnaddr, rpc )
	cat_print( 'lobby', "register xuid", name, tostring( xuid ) )
	
	if self._session == nil then
 		Application:error( "NetworkMatchMakingXBL:_register_player, _session is nil." )
  
  		if MultiplayerScreen.instance then
   			Application:error( "	MultiplayerScreen.instance state = ", MultiplayerScreen.instance._state )
  		end

		return
	end
	
	if( self:_is_server() ) then
		if( not self:_call_callback( "have_reservation", xuid ) ) then
			--send bakc error msg
			return
		end
	end
	
	XboxLive:join_remote( self._session, {xuid}, {self._private} )

	local new_player = {}
	new_player.name = name
	new_player.xuid = xuid
	new_player.group = group_id
	new_player.external_address = xnaddr  

	--distribute
	if( self:_is_server() ) then
		new_player.rpc = rpc

		local server_xna = XboxLive:external_address( Network:self() )
		rpc:register_player( true, managers.network.account:username(), managers.network.account:player_id(), managers.network.group:group_id(), server_xna )

		for k, v in pairs( self._players ) do
			v.rpc:register_player( true, name, xuid, group_id,xnaddr )
			rpc:register_player( true, v.name, v.xuid, v.group,v.external_address )
		end

		managers.network.generic:ping_watch( rpc, true, callback( self, self, "_client_timed_out" ) )

		if( self._uses_arbitration == true ) then
			rpc:mm_nonce( self._session:nonce() )
			cat_print("Karl", "nonce is(server)", self._session:nonce() )
		end
	end

	table.insert( self._players, new_player )
	
	if( self:_is_server() ) then
		local xuids = {}
		
		table.insert( xuids, managers.network.account:player_id() )
		
		for k, v in pairs( self._players ) do
			table.insert( xuids, v.xuid )
		end

		self:_queue_modify_skill( self._session, xuids )
		--XboxLive:modify_skill( self._session, xuids )
	end

	local p_name = name
	self:_call_callback( "player_joined", {
		player_id=xuid,
		group_id=group_id,
		name=p_name,
		rpc=new_player.rpc,
		external_address=xnaddr } )
end

function NetworkMatchMakingXBL:_unregister_player( xuid, rpc )
	cat_print( 'lobby', "unregister xuid", tostring( xuid ) )

	if self._session == nil then
 		Application:error( "NetworkMatchMakingXBL:_unregister_player, _session is nil." )
  
  		if MultiplayerScreen.instance then
   			Application:error( "	MultiplayerScreen.instance state = ", MultiplayerScreen.instance._state )
  		end

		return
	end

	XboxLive:leave_remote( self._session, xuid )
	
	local new_list = {}
	for k, v in pairs( self._players ) do
		if( v.xuid ~= xuid ) then
			table.insert( new_list, v )
		end
	end
	self._players = new_list
		
	if( self:_is_server() ) then
		managers.network.generic:ping_remove( rpc, true )
		
		for k, v in pairs( self._players ) do
			v.rpc:unregister_player( true, xuid )
		end
		
		local xuids = {}
		table.insert( xuids, managers.network.account:player_id() )
		for k, v in pairs( self._players ) do
			table.insert( xuids, v.xuid )
		end
		self:_queue_modify_skill( self._session, xuids )
		--XboxLive:modify_skill( self._session, xuids )
	end
	
	self:_call_callback( "player_left", { player_id=xuid, reason="went home to mama" } )
end

function NetworkMatchMakingXBL:_queue_modify_skill(session, player_list)
	-- It's not a long list of requests, we only remember the last request made
	self._queued_modify_skill = {session=session, player_list=player_list}
	self:_flush_modify_skill_queue()
end

function NetworkMatchMakingXBL:_flush_modify_skill_queue()
	cat_print("lobby", "NetworkMatchMakingXBL:_flush_modify_skill_queue()")
	local can_start_new = (not self._queued_modify_skill_in_progress)
	local has_something_to_start = (self._queued_modify_skill ~= nil)
	if can_start_new and has_something_to_start then
		-- Request a modify_skill and wait for the result
		XboxLive:modify_skill( self._queued_modify_skill.session, self._queued_modify_skill.player_list, callback(self, self, "_cb_modify_skill_done" ))
		self._queued_modify_skill_in_progress = true
		self._queued_modify_skill = nil
	end
end

function NetworkMatchMakingXBL:_cb_modify_skill_done()
	cat_print("lobby", "NetworkMatchMakingXBL:_cb_modify_skill_done()")
	-- Modify skill is done. See if we have more modify_skills in queue
	self._queued_modify_skill_in_progress = nil
	self:_flush_modify_skill_queue()
end

function NetworkMatchMakingXBL:_server_timed_out( rpc )
	cat_print( 'lobby', "Timed out connection to MatchMaker server")
	
	self._lostconnection = true
	self:_call_callback( "server_timedout" )
	self._time_to_close = 0
end

function NetworkMatchMakingXBL:_client_timed_out( rpc )
	for k,v in pairs( self._players ) do
		if( v.rpc:ip_at_index(0) == rpc:ip_at_index(0) ) then
			self:_unregister_player( v.xuid, v.rpc )
			return
		end
	end
end

function NetworkMatchMakingXBL:write_stats( board, xuid, prop )
	XboxLive:write_session_stats( self._session, xuid, board, prop, callback( self, self, "_write_stats_cb" ) )
end

function NetworkMatchMakingXBL:_write_stats_cb()
end

function NetworkMatchMakingXBL:_flush_session()
	XboxLive:flush_session_stats( self._session )
end

function NetworkMatchMakingXBL:_read_xbl()
	XboxLive:read_stats(managers.network.account:player_id(), "dm", callback(self,self, "_read_xbl_cb"))
end

function NetworkMatchMakingXBL:_read_xbl_cb(data)
	cat_print_inspect( "Karl", "xbl data", data )
end]]

