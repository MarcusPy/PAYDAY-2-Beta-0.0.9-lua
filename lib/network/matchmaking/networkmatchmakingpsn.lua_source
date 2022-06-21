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

NetworkMatchMakingPSN = NetworkMatchMakingPSN or class()
NetworkMatchMakingPSN.OPEN_SLOTS = 4
NetworkMatchMakingPSN.MAX_SEARCH_RESULTS = 20

function NetworkMatchMakingPSN:init()
	cat_print( 'lobby', "matchmake = NetworkMatchMakingPSN")

	self._players = {}
	
	self._TRY_TIME_INC = 10
	self._JOIN_SERVER_TRY_TIME_INC = 20
	
	self._room_id = nil
	self._join_cb_room = nil
	
	self._server_rpc = nil
	
	self._is_server_var = false
	self._is_client_var = false
	
	self._difficulty_filter = 0
	
	self._private = false

	self._callback_map = {}
	
	self._hidden = false
	self._server_joinable = true
	
	self._connection_info = {}
	
	local f = function(info) self:_error_cb( info ) end
	PSN:set_matchmaking_callback( "error", f )
	
	self:_load_globals()
	
	self._cancelled = false
	
	self._peer_join_request_remove = {}
	
	local f = function(...) self:_custom_message_cb(...) end
	PSN:set_matchmaking_callback( "custom_message", f )
		
	-- PSN:set_matchmaking_callback( "invitation_received", callback( self, self, "_invitation_received_cb" ) )
	PSN:set_matchmaking_callback( "invitation_received_result", callback( self, self, "_invitation_received_result_cb" ) )
	PSN:set_matchmaking_callback( "join_invite_accepted_xmb", callback( self, self, "_xmb_join_invite_cb" ) )
		
	PSN:set_matchmaking_callback( "worlds_fetched", callback( self, self, "_worlds_fetched_cb" ) )
		
	local js = function(...) self:cb_connection_established(...) end
	PSN:set_matchmaking_callback( "connection_etablished", js )
end

function NetworkMatchMakingPSN:_xmb_join_invite_cb( message )
	-- print( inspect( message ) )
	local ok_func = function()
						if(managers.network.account:signin_state()=="not signed in")then
							managers.network.account:show_signin_ui()
						end
					end
	managers.menu:show_invite_join_message( { ok_func = ok_func } )
end

function NetworkMatchMakingPSN:start_server_time_out_check()
	PSN:set_matchmaking_callback( "fetch_session_attributes", callback( self, self, "_server_time_out_check_cb" ) )
	self:_trigger_time_out_check()
end

function NetworkMatchMakingPSN:_trigger_time_out_check()
	self._next_time_out_check_t = Application:time() + 4
	self._testing_connection = true
	PSN:get_session_attributes( { self._room_id }, { numbers = { 1, 2, 3, 4, 5, 6, 7, 8 } } )
end

function NetworkMatchMakingPSN:_server_time_out_check_cb()
	-- print( "GOT ALIVE" )
	self._server_last_alive_t = Application:time()
	self._testing_connection = nil
end

function NetworkMatchMakingPSN:_worlds_fetched_cb( ... )
	print( "_worlds_fetched_cb" )
	self._getting_world_list = nil
	managers.system_menu:close( "get_world_list" )
	if Global.boot_invite and Global.boot_invite.pending then
		self:join_boot_invite()
	end
end

function NetworkMatchMakingPSN:_getting_world_list_failed()
	print( "failed_getting_world_list" )
	managers.menu:back( true )
	managers.menu:show_no_connection_to_game_servers_dialog()
	self:_worlds_fetched_cb()
end

function NetworkMatchMakingPSN:getting_world_list()
	print( "getting_world_list" )
	self._getting_world_list = true
	managers.menu:show_get_world_list_dialog( { cancel_func = callback( self, self, "_getting_world_list_failed" ) } )
end

function NetworkMatchMakingPSN:_session_destroyed_cb(room_id, ... )
	print( "NetworkMatchMakingPSN:_session_destroyed_cb", room_id, ... )
	cat_print("lobby", "NetworkMatchMakingPSN:_session_destroyed_cb" )
	if room_id == self._room_id then
		-- self._room_id = nil -- Couldn't nil here, client wouldn't really leave the room if did.
		if not self._is_server_var then
			managers.network:queue_stop_network()
			-- self:leave_game()
			PSN:leave_session( self._room_id ) -- This will result in leaving room twice, which is not good but found no other way.
			self._room_id = nil
			self:leave_game()
		end
		self._room_id = nil
		
		-- Host lost, and we still need to have the server alive for this to make sense. Could be that the server left us nicely
		if Network:is_client() and managers.network:session() and managers.network:session():server_peer() then -- not self._skip_destroy_cb then
			if game_state_machine:current_state().on_server_left then
				Global.on_server_left_message = "dialog_connection_to_host_lost"
				game_state_machine:current_state():on_server_left()
			end
		elseif self._joining_lobby then
			self:_error_cb( { error = "80022b13" } ) -- This is stupid, something with cb has changed in the engine last week /Martin 110828
		end
	end
	self._skip_destroy_cb = nil
end

function NetworkMatchMakingPSN:room_id()
	return self._room_id
end

--[[function NetworkMatchMakingPSN:find_game(settings)
	self._last_settings = settings
	
	self._private = false
	
	self._cancelled = false
	
	self._TRY_TIME_INC = 10
	
	local world_list = PSN:get_world_list()
	
	if( Global.psn_invite_id ) then
		Global.psn_invite_id = Global.psn_invite_id + 1
		if( Global.psn_invite_id > 990 ) then
			Global.psn_invite_id = 1
		end
	end
	
	local f = function( info )
		if( info and #info.room_list > 0 ) then
			self:_try_servers( info.room_list )
		else
			self:_create_server()
		end
	end
	PSN:set_matchmaking_callback( "session_search", f )
	
	-- PSN:set_matchmaking_callback( "session_search", function( info ) print( inspect( info ) ) end )

	local table_description = {}
	local attributes = {}
	
	local gt = {}
	gt.type = "GAME_NUM"
	gt.value = self:_translate_settings( settings, "game_mode" )
	
	local priv = {}
	priv.type = "GAME_NUM"
	priv.value = 1
	
	attributes[1] = gt
	attributes[2] = priv
	table_description.attributes = attributes
	
	PSN:search_session( table_description, world_list[2].world_id )
end]]

function NetworkMatchMakingPSN:create_private_game( settings )
	self._cancelled = false
	
	self._private = true
	
	if( Global.psn_invite_id ) then
		Global.psn_invite_id = Global.psn_invite_id + 1
		if( Global.psn_invite_id > 990 ) then
			Global.psn_invite_id = 1
		end
	end
	
	self._last_settings = settings
	self:_create_server( true )
end

function NetworkMatchMakingPSN:cancel_find()
	self._cancelled = true
	
	self._is_server_var = false
	self._is_client_var = false

	self._players = {}
	self._server_rpc = nil
	
	self._try_list = nil
	self._try_index = nil
	self._trytime = nil
	
	self:destroy_game()
	self._room_id = nil
	
	if( managers.network.group:room_id() ) then
		managers.network.voice_chat:open_session( managers.network.group:room_id() )
	end
	
	if( not self._join_cb_room ) then
		self:_call_callback( "cancel_done" )
	else
		self._cancel_time = TimerManager:wall():time() + 10
	end
end

function NetworkMatchMakingPSN:remove_ping_watch()
	if( self:_is_client() ) then
		if( self._server_rpc ) then
--			managers.network.generic:ping_remove( self._server_rpc, true )
		end
	else
		for k, v in pairs( self._players ) do
			if( v.rpc ) then
	--			managers.network.generic:ping_remove( v.rpc, true )
			end
		end
	end
end

function NetworkMatchMakingPSN:leave_game()
	local sent = false

	-- No more ping watch after we've left a game
	-- leave_game is only called by the screen
	self:remove_ping_watch()
	self._server_last_alive_t = nil
	self._next_time_out_check_t = nil
	PSN:set_matchmaking_callback( "fetch_session_attributes", function() end )
	
	-- Let everyone know we're leaving the game
	if( self:_is_client() ) then
		if( self._server_rpc ) then
			-- self._server_rpc:psn_mm_unregister_player( managers.network.account:player_id(), false )
			sent = true
		end
		if managers.network.group:_is_client() then
			self:_call_callback("group_leader_left_match")
		end
	else
		for k, v in pairs( self._players ) do
			if( v.rpc ) then
				-- v.rpc:psn_mm_unregister_player( managers.network.account:player_id(), true )
				sent = true
			end
		end
	end
	
	if( sent == true ) then
		if self._room_id then
			local dialog_data = {}
			dialog_data.title = managers.localization:text( "dialog_leaving_lobby_title" )
			dialog_data.text = managers.localization:text( "dialog_wait" )
			dialog_data.id = "leaving_game"
			dialog_data.no_buttons = true
			managers.system_menu:show( dialog_data )
		end
		self._leave_time = TimerManager:wall():time() + 2
	else
		self._leave_time = TimerManager:wall():time() + 0
	end
end

function NetworkMatchMakingPSN:register_callback(event, callback)
	self._callback_map[ event ] = callback
end

function NetworkMatchMakingPSN:join_game(id, private)
	--[[self._cancelled = false
	
	self._private = private or false
	
	local room = {}
	room.room_id = PSN:convert_string_to_sessionid(id)
	
	self._TRY_TIME_INC = 30
	self:_join_server( room )]]
end

function NetworkMatchMakingPSN:start_game()
	--[[PSN:hide_session( self._room_id )
	
	Network:reset_all_timeouts()

	managers.networkgame:save_players_list(self._players)	

	self:_call_callback( "game_started" )]]
end

function NetworkMatchMakingPSN:end_game()
	-- self:remove_ping_watch()
end

function NetworkMatchMakingPSN:destroy_game()
	if( self._room_id ) then
		if( self:_is_client() ) then
			PSN:leave_session( self._room_id )
		else
			self._server_last_alive_t = nil
			self._next_time_out_check_t = nil
			PSN:set_matchmaking_callback( "fetch_session_attributes", function() end )
			PSN:destroy_session( self._room_id )
		end
	else
		cat_print( "multiplayer", "Dont got a room id!?" )
	end
end

function NetworkMatchMakingPSN:is_game_owner()
	return self:_is_server() == true
end

function NetworkMatchMakingPSN:game_owner_name() 
	return tostring( self._game_owner_id )
end

function NetworkMatchMakingPSN:is_full()
	if( #self._players == (self.OPEN_SLOTS-1) ) then
		return true
	end
	return false
end

function NetworkMatchMakingPSN:get_mm_id( name )
	if( name == managers.network.account:username() ) then
		return managers.network.account:player_id()
	else
		for k, v in pairs( self._players ) do
			if( v.name == name ) then
				return v.pnid
			end
		end
	end
	
	return nil
end

function NetworkMatchMakingPSN:user_in_lobby( id )
	if not self._room_id then
		return false
	end
	
	local memberlist = PSN:get_info_session( self._room_id ).memberlist 
	for _,member in ipairs( memberlist ) do
		if member.user_id == id then
			return true
		end
	end
	return false
end

--[[function NetworkMatchMakingPSN:say( message )
	if( self:_is_server() ) then
		for k, v in pairs( self._players ) do
			v.rpc:say_toclient( message)
		end
	end
end

function NetworkMatchMakingPSN:membervoted( player, votes )
	if( self:_is_server() ) then
		for k, v in pairs( self._players ) do
			v.rpc:membervoted_toclient( player, votes )
		end
	end
end]]

function NetworkMatchMakingPSN:update( time )

	if self._no_longer_in_session then -- Detected at load
		if self._no_longer_in_session == 0 then
			if Network:is_client() and managers.network:session() and managers.network:session():server_peer() then
				if game_state_machine:current_state().on_server_left then
					Global.on_server_left_message = "dialog_connection_to_host_lost"
					game_state_machine:current_state():on_server_left()
				end
			elseif Network:is_server() and managers.network:session() then
				if game_state_machine:current_state().on_disconnected then
					game_state_machine:current_state():on_disconnected()
				end
			end	
			
			self._no_longer_in_session = nil
		else
			self._no_longer_in_session = self._no_longer_in_session - 1
		end
	end

	if self._is_server_var then
		if self._server_last_alive_t and (self._server_last_alive_t + 20) < Application:time() then
			self._server_last_alive_t = nil
			-- print( "LOST CONNTECTION TO PSN" )
			if managers.network:session() and managers.network:session():_local_peer_in_lobby() then
				-- print( "out from lobby" )
				managers.menu:psn_disconnected()
			elseif managers.network:game() then
				-- print( "out from game" )
				managers.network:game():psn_disconnected()
			end
		elseif self._next_time_out_check_t and self._next_time_out_check_t < Application:time() then
			-- print( "TEST AGAIN" )
			self:_trigger_time_out_check()
		end
	end
	
	if( self._trytime and TimerManager:wall():time() > self._trytime ) then
		self._trytime = nil
		
		print( "self._trytime run out!", inspect( self ) )
		
		local is_server = self._is_server_var
		
		self._is_server_var = false
		self._is_client_var = false
		managers.platform:set_presence( "Signed_in" )
		
		self._players = {}
		self._server_rpc = nil
		
		if self._joining_lobby then
			self:_error_cb( { error = "8002231d" } )
		end
		
		if( self._room_id ) then
			if( not is_server ) then
				print( " LEAVE SESSION BECAUSE OF TIME OUT", self._room_id )
					-- PSN:leave_session( self._room_id )
				self:leave_game()
				-- self._room_id = nil
			 	-- self._trytime = TimerManager:wall():time() + 5
			end
		else
			if( not self._last_settings ) then
				self:_call_callback( "cancel_done" )
			else
				-- self:_try_servers()
			end
		end
	end
	
	if( self._leave_time and TimerManager:wall():time() > self._leave_time ) then
		local closed = false
		
		if( self._room_id ) then
			closed = true
			
			if( PSN:is_online() and managers.network.group:room_id() ) then
				managers.network.voice_chat:open_session( managers.network.group:room_id() )
			end
			
			if( not self._call_server_timed_out ) then
				self._leaving_timer = TimerManager:wall():time() + 10
			end
			
			if( self:_is_client() ) then
				print( "leave session HERE" )
				PSN:leave_session( self._room_id )
			else
				PSN:destroy_session( self._room_id )
			end
		end
		
		self._players = {}
		self._game_owner_id = nil
		self._server_rpc = nil
		self._leave_time = nil
		
		self._is_client_var = false
		self._is_server_var = false
		
		if( self._call_server_timed_out == true ) then
			self._call_server_timed_out = nil
			self:_call_callback( "server_timedout" )
		else
			if( closed == false ) then
				print( "left game callback" )
				managers.system_menu:close( "leaving_game" )
				if self._invite_room_id then
					--self:join_server( self._invite_room_id )
					self:join_server_with_check( self._invite_room_id, true )
				end
				self:_call_callback( "left_game" )
			end
		end
	end
	
	if( self._leaving_timer and TimerManager:wall():time() > self._leaving_timer ) then
		print( "self._leaving_timer left_game" )
		self._room_id = nil
		self._leaving_timer = nil
		self:_call_callback( "left_game" )
	end
	
	if( self._cancel_time and TimerManager:wall():time() > self._cancel_time ) then
		self._cancel_time = nil
		self._join_cb_room = nil
		self:_call_callback( "cancel_done" )
	end
end

-- // --

function NetworkMatchMakingPSN:_load_globals()
	if( Global.psn and Global.psn.match ) then
		
		self._game_owner_id	= Global.psn.match._game_owner_id
		self._room_id 		= Global.psn.match._room_id
		self._is_server_var = Global.psn.match._is_server
		self._is_client_var = Global.psn.match._is_client
		self._players		= Global.psn.match._players
		self._server_rpc	= Global.psn.match._server_ip and Network:handshake( Global.psn.match._server_ip, nil, "TCP_IP" ) -- Global.psn.match._server_rpc
		self._attributes_numbers = Global.psn.match._attributes_numbers
		self._connection_info = Global.psn.match._connection_info
		self._hidden = Global.psn.match._hidden
		self._num_players = Global.psn.match._num_players
		
		Global.psn.match = nil
		
		if self._is_server_var then
			self:start_server_time_out_check()
		end
		
		-- Check if we are still in the session after loading.
		if self._room_id then
			local info_session = PSN:get_info_session( self._room_id )
			if not info_session or #info_session.memberlist == 0 then
				self._no_longer_in_session = 1 -- Wait one frame before displaying, we have other systmes in place that detect this as well.
			end
		end
	end
end
function NetworkMatchMakingPSN:_save_globals()
	if( not Global.psn ) then
		Global.psn = {}
	end

	Global.psn.match = {}
	
	Global.psn.match._game_owner_id = self._game_owner_id
	Global.psn.match._room_id 		= self._room_id
	Global.psn.match._is_server 	= self._is_server_var
	Global.psn.match._is_client 	= self._is_client_var
	Global.psn.match._players 		= self._players
	-- Global.psn.match._server_rpc 	= self._server_rpc
	Global.psn.match._server_ip 	= self._server_rpc and self._server_rpc:ip_at_index(0)
	Global.psn.match._attributes_numbers = self._attributes_numbers
	Global.psn.match._connection_info = self._connection_info
	Global.psn.match._hidden = self._hidden
	Global.psn.match._num_players = self._num_players
end

function NetworkMatchMakingPSN:_call_callback( name, ... )
	if( self._callback_map[ name ] ) then
		return self._callback_map[ name ]( ... )
	else
		Application:error( "Callback " .. name .. " not found." )
	end
end

function NetworkMatchMakingPSN:_clear_psn_callback( cb )
	local f = function() end
	PSN:set_matchmaking_callback( cb, f )
end

function NetworkMatchMakingPSN:psn_member_joined( info )
	print( "psn_member_joined" )
	print( inspect( info ) )
	if( info and info.room_id == self._room_id ) then
		if( not self._private ) then
			managers.network.voice_chat:open_session( self._room_id )
		end
		
		if( info.user_id == managers.network.account:player_id() ) then
		else
			local time_left = 10
			--[[if( MultiplayerScreen.instance and MultiplayerScreen.instance._gamestart_time ) then
				time_left = MultiplayerScreen.instance._gamestart_time - TimerManager:wall():time()
			end
			
			if( self:_is_server() and time_left > 5 ) then
				self:_call_callback( "reserv_slot", info.user_id  )
			end]]
		end
	end
	
	if Network:is_server() then
		self._peer_join_request_remove[ tostring( info.user_id ) ] = nil
		print( "   remove from remove list", tostring( info.user_id ) )
	end
end

function NetworkMatchMakingPSN:psn_member_left( info )
	if( info and info.room_id == self._room_id ) then
		if( info.user_id == managers.network.account:player_id() ) then
			print( "IT WAS ME WHO LEFT" )
			self._skip_destroy_cb = true
			self._connection_info = {}
			managers.platform:set_presence( "Signed_in" )
			managers.system_menu:close( "leaving_game" )
			if( self._try_time ) then 
				self._trytime = nil 
				if( not self._last_settings ) then
					self:_call_callback( "cancel_done" )
				else
					-- self:_try_servers()
				end
				if self._invite_room_id then
					--self:join_server( self._invite_room_id )
					self:join_server_with_check( room_id, true )
				end
				return
			end 
			
			if( self._leaving_timer ) then 
				self._room_id = nil
				self._leaving_timer = nil 
				self:_call_callback( "left_game" )
				if self._invite_room_id then
					--self:join_server( self._invite_room_id )
					self:join_server_with_check( room_id, true )
				end
				return
			end
		else
			print( "SOMEONE ELSE LEFT", info.user_id )
			local user_name = tostring( info.user_id )
			self:_remove_peer_by_user_id( info.user_id )
			if( self:_is_server() ) then
				self:_call_callback( "remove_reservation", info.user_id  )
			end
		end
	end
end

-- This might not have a result if the standard peer left package is recieved first.
function NetworkMatchMakingPSN:_remove_peer_by_user_id( user_id )
	-- print( "_remove_peer_by_user_id", user_id )
	self._connection_info[ user_id ] = nil
	if not managers.network:session() then
		-- We have allready shut down the network before recieving this call
		return
	end
	
	local user_name = tostring( user_id )
	for pid, peer in pairs( managers.network:session():peers() ) do
		if peer:name() == user_name then
			print( " _remove_peer_by_user_id on_peer_left", peer:id(), pid )
			managers.network:session():on_peer_left( peer, pid )
			return
		end
	end
		
	if Network:is_server() then
		self._peer_join_request_remove[ user_id ] = true
		print(  "queue to remove if we get a request", user_id )
	end
end

function NetworkMatchMakingPSN:check_peer_join_request_remove( user_id )
	local has = self._peer_join_request_remove[ user_id ]
	self._peer_join_request_remove[ user_id ] = nil
	return has
end

function NetworkMatchMakingPSN:_is_server( set )
 	if( (set == true) or (set == false) ) then
		self._is_server_var = set
	else
		return self._is_server_var
	end
end

function NetworkMatchMakingPSN:_is_client( set )
 	if( (set == true) or (set == false) ) then
		self._is_client_var = set
	else
		return self._is_client_var
	end
end

-- settings.number
-- index 1 - level index (from level tweak data)			-- i%1000 = level index,  math.floor(i/1000) = job index
-- index 2 - difficulty 1-4 (easy,normal,hard,overkill)
-- index 3 - premission 1-3 (public,friends-only,private)
-- index 4 - server state 1-3 (lobby,loading, ingame)
-- index 5 - version (trial, fullgame.v1, fullgame.v2, ... )
-- index 6 - allow drop-in 0-1
-- index 7 - reputation limit 0-145
-- index 8 - #players 1-4

function NetworkMatchMakingPSN:_game_version()
	return (managers.dlc:is_trial() and 7 or 8)
end

function NetworkMatchMakingPSN:create_lobby( settings )
	print( "NetworkMatchMakingPSN:create_group_lobby()", inspect( settings ) )
	
	self._server_joinable = true
	self._num_players = nil
	self._server_rpc = nil
	self._players = {}
	self._peer_join_request_remove = {}
	
	local world_list = PSN:get_world_list()
	
	local session_created = function(roomid) managers.network.matchmake:_created_lobby(roomid) end
	
	PSN:set_matchmaking_callback( "session_created", session_created )
	
	local numbers = settings and settings.numbers or { 2, 3, 1, 1, 1, 1, 0, 1 }
	numbers[4] = 1
	numbers[5] = self:_game_version()
	numbers[8] = 1
	
	local table_description = {
		numbers = numbers,
		-- strings = { "baloon" }
	}
	self._attributes_numbers = numbers
		
	--[[local attributes = {}
	
	local gt = {}
	gt.type = "GAME_NUM"
	gt.value = 1
	
	local priv = {}
	priv.type = "GAME_NUM"
	priv.value = 2
	
	attributes[1] = gt
	attributes[2] = priv
	
	table_description.attributes = attributes]]
	
	local dialog_data = {}
	dialog_data.title = managers.localization:text( "dialog_creating_lobby_title" )
	dialog_data.text = managers.localization:text( "dialog_wait" )

	dialog_data.id = "create_lobby"
	dialog_data.no_buttons = true
	
	managers.system_menu:show( dialog_data )
	
	self._creating_lobby = true
	PSN:create_session( table_description, world_list[1].world_id, 0, self.OPEN_SLOTS, 0 )
end

function NetworkMatchMakingPSN:_create_lobby_failed()
	self:_create_lobby_done()
end

function NetworkMatchMakingPSN:_create_lobby_done()
	self._creating_lobby = nil
	managers.system_menu:close( "create_lobby" )
end

function NetworkMatchMakingPSN:_created_lobby( room_id )
	self:_create_lobby_done()
	managers.menu:created_lobby()
	print( "NetworkMatchMakingPSN:_created_lobby( room_id )", room_id )
	-- self:_clear_psn_callback( "connection_etablished" )
	self._trytime = nil
	
	-- managers.platform:set_presence( "Is_server" )
	self:_is_server( true )
	self:_is_client( false )
	
	self._room_id = room_id
	
	managers.network.voice_chat:open_session( self._room_id )
	
	local playerinfo = {}
	playerinfo.name 		= managers.network.account:username()
	playerinfo.player_id 	= managers.network.account:player_id()
	playerinfo.group_id 	= tostring( managers.network.group:room_id() )
	playerinfo.rpc 			= Network:self( "TCP_IP" )
	
	self:_call_callback( "found_game", self._room_id, true )
	self:_call_callback( "player_joined", playerinfo )
		
	self:start_server_time_out_check()
end

function NetworkMatchMakingPSN:searching_friends_only()
	return self._friends_only
end

function NetworkMatchMakingPSN:difficulty_filter()
	return self._difficulty_filter
end

function NetworkMatchMakingPSN:set_difficulty_filter( filter )
	self._difficulty_filter = filter
end

function NetworkMatchMakingPSN:start_search_lobbys( friends_only )
	if self._searching_lobbys then
		return
	end
	self._searching_lobbys = true
	
	self._search_lobbys_index = 1
	self._lobbys_info_list = {}
	self._friends_only = friends_only -- or self._friends_only
	-- managers.menu:show_retrieving_servers_dialog()
	
	if not self._friends_only then	
		local f = function( info )
			table.insert( self._lobbys_info_list, info )
			
			if self._search_lobbys_index > 100 or #info.room_list == 0 then -- Break if reach a server limit or no more rooms
				print( "search done" )
				self:_call_callback( "search_lobby", self._lobbys_info_list )
				-- managers.network.matchmake:search_lobby_done()
				-- managers.menu:active_menu().logic:refresh_node( "play_online", true, self._lobbys_info_list, self._friends_only )
			else
				self._search_lobbys_index = self._search_lobbys_index + self.MAX_SEARCH_RESULTS
				self:search_lobby()
				print( "search again", self._search_lobbys_index )
			end
		end
		
		PSN:set_matchmaking_callback( "session_search", f )
		self:search_lobby()
	else 
		-- Search by friends
		-- Done in two steps
		-- First retrieve room ids based on friends
		-- Second retrieve room info list based on room ids
		print( "start_search_lobbys friends" )
				
		local f = function( results, ... )
			-- print( "fetch_user_info cb" )
			-- print( inspect( results ), inspect( ... ) )
			
			local room_ids = {}
			
			local info = {}
			info.attribute_list = {}
			info.request_id = 0
			info.room_list = {}
			
			local reverse_lookup = {}
			
			for i_user, user_info in ipairs( results.users ) do
				-- print( "user_info", inspect( user_info ) )
				if user_info.joined_sessions then
					-- print( "joined_sessions", inspect( user_info.joined_sessions ) )
					local room_id = user_info.joined_sessions[1]
					table.insert( room_ids, room_id )
										
					local friend_id = user_info.user_id
					reverse_lookup[ tostring( room_id ) ] = friend_id
				end
			end
						
			local f2 = function( results )
				if results.rooms then
					local info = {}								-- Recreate table structure recieved from search_session cb
					info.attribute_list = {}
					info.request_id = 0
					info.room_list = {}
					table.insert( self._lobbys_info_list, info )
					 
					for _,room_info in ipairs( results.rooms ) do				
						local attributes = room_info.attributes
						local full = room_info.full
						local closed = room_info.closed
						local owner_id = room_info.owner
						local room_id = room_info.room_id
						local friend_id = reverse_lookup[ tostring( room_id ) ] -- Which friend provided us with the room
						-- local has_permission = self:is_server_ok( nil, nil, attributes ) -- Permission will be checked when adding the menu item anyways
						if not full and not closed and ( attributes.numbers[5] == self:_game_version() ) then
							table.insert( info.attribute_list, attributes )
							table.insert( info.room_list, { owner_id = owner_id, friend_id = friend_id, room_id = room_id } )
						end
					end
				end
				
				self:_call_callback( "search_lobby", self._lobbys_info_list )
				-- managers.menu:active_menu().logic:refresh_node( "play_online", true, self._lobbys_info_list, self._friends_only )
				-- managers.network.matchmake:search_lobby_done()
			end
						
			if #room_ids > 0 then
				PSN:set_matchmaking_callback( "fetch_session_attributes", f2 )
				local wanted_attributes = {	numbers = { 1, 2, 3, 4, 5, 6, 7, 8 } }
				PSN:get_session_attributes( room_ids, wanted_attributes )
			else
				self:_call_callback( "search_lobby", self._lobbys_info_list )
				-- managers.menu:active_menu().logic:refresh_node( "play_online", true, self._lobbys_info_list, self._friends_only )
				-- managers.network.matchmake:search_lobby_done()
			end
		end
		
		local friends = managers.network.friends:get_npid_friends_list()
		PSN:set_matchmaking_callback( "fetch_user_info", f )
		if #friends == 0 or (not PSN:request_user_info( friends )) then -- Finish searching for friends imidiatly if no friends or fails (could be we waiting for friends to be initiated) 
			self:_call_callback( "search_lobby", self._lobbys_info_list )
			-- managers.menu:active_menu().logic:refresh_node( "play_online", true, self._lobbys_info_list, self._friends_only )
			-- managers.network.matchmake:search_lobby_done()
		end
	end
end

function NetworkMatchMakingPSN:search_lobby( settings )
	local numbers = settings and settings.numbers or { 1, 2, 3, 4, 5, 6, 7, 8 }
	
	local table_description = {
		numbers = numbers,
		-- strings = { "baloon" }
	}
	
	local filter = {
		closed = nil,					-- nil = don't care, false = must be open, true = must be closed
		full = false,					-- 	nil = don't care, false = must not be full, true = must be full
		numbers = { -- { { param_index, "==", value }, {...}, ... } operators: "==", "!=", "<", "<=", ">", ">="
			{ 3, "!=", 3 },
			{ 7, "<=", managers.experience:current_level() },
			{ 5, "==", self:_game_version() }
		}
	}
	
	if not NetworkManager.DROPIN_ENABLED then
		table.insert( filter.numbers, { 4, "==", 1 } )
	end
	
	if self._difficulty_filter and self._difficulty_filter ~= 0 then
		table.insert( filter.numbers, { 2, "==", self._difficulty_filter } )
	end
	
	
	--[[local attributes = {}
	
	local gt = {}
	gt.type = "GAME_NUM"
	gt.value = 2
	
	local priv = {}
	priv.type = "GAME_NUM"
	priv.value = 1
	
	attributes[1] = gt
	attributes[2] = priv
	
	table_description.attributes = attributes]]
	
	-- managers.menu:show_retrieving_servers_dialog()
	PSN:search_session( table_description, filter, PSN:get_world_list()[1].world_id, self._search_lobbys_index, self.MAX_SEARCH_RESULTS ) -- Last parameter indicates server index
end

function NetworkMatchMakingPSN:_search_lobby_failed()
	self:search_lobby_done()
end

function NetworkMatchMakingPSN:search_lobby_done()
	self._searching_lobbys = nil
	managers.system_menu:close( "find_server" )
end

function NetworkMatchMakingPSN:set_num_players( num )
	self._num_players = num
	
	if self._attributes_numbers then
		self:_set_attributes()
	end
end

function NetworkMatchMakingPSN:_set_attributes( settings )
	if not self._room_id then
		return
	end
	
	self._attributes_numbers = settings and settings.numbers or self._attributes_numbers
	
	local numbers = self._attributes_numbers
	numbers[8] = self._num_players or 1
	
	local attributes = {
		numbers = numbers,
		-- strings = { "baloon" }
	}
	PSN:set_session_attributes( self._room_id, attributes )
end

function NetworkMatchMakingPSN:set_server_attributes( settings )
	if not self._room_id then
		return
	end
	self._attributes_numbers[ 1 ] = settings.numbers[ 1 ]
	self._attributes_numbers[ 2 ] = settings.numbers[ 2 ]
	self._attributes_numbers[ 3 ] = settings.numbers[ 3 ]
	self._attributes_numbers[ 6 ] = settings.numbers[ 6 ]
	self._attributes_numbers[ 7 ] = settings.numbers[ 7 ]
	self:_set_attributes( { numbers = self._attributes_numbers } )
end

function NetworkMatchMakingPSN:set_server_state( state )
	if not self._room_id then
		return
	end
	local state_id = tweak_data:server_state_to_index( state )
	self._attributes_numbers[ 4 ] = state_id
	self:_set_attributes( { numbers = self._attributes_numbers } )
end

function NetworkMatchMakingPSN:server_state_name()
	return tweak_data:index_to_server_state( self._attributes_numbers[4] )	
end

function NetworkMatchMakingPSN:test_search()
	local f = function( info )
				print( inspect( info ) ) 
				print( inspect( info.room_list[1] ) )
				print( inspect( info.room_list[1].owner_id ) )
				print( help( info.room_list[1].owner_id ) )
				print( inspect( info.room_list[1].room_id ) )
				print( help( info.room_list[1].room_id ) )
				print( inspect( PSN:get_info_session( info.room_list[1].room_id ) ) )
			end 
	PSN:set_matchmaking_callback( "session_search", f )
end

function NetworkMatchMakingPSN:test_search_session()
	local search_params = {
		numbers = { 1, 2, 3, 4 },
		--strings = { 1 }
	}
	PSN:search_session( search_params, {}, PSN:get_world_list()[1].world_id )
end

function NetworkMatchMakingPSN:_custom_message_cb( message )
	print( "_custom_message_cb" )
	print( inspect( message.custom_table ) )
	print( "message.sender", message.sender )
	print( "message", inspect( message ) )
	
	if( message.custom_table and message.custom_table.join_invite ) then
		if self._room_id == message.room_id then -- Dont show invite id we are in or joining same game
			return
		end
		if message.custom_table.version ~= self:_game_version() then
			return		
		end
		if not self._game_owner_id or self._game_owner_id ~= message.sender then
			if not self._has_pending_invite then
				print( "managers.platform:presence() ~= Idle", managers.platform:presence() ~= "Idle" )
				if managers.platform:presence() ~= "Idle" then
					self:_recived_join_invite( message )
				end
			end
		end
			
		if( self._join_enable ) then
			-- self._invite_id = message.custom_table.invite_id
			-- self:_call_callback( "receive_group_lobby_invite", message, message.sender )
		end
	end
end

function NetworkMatchMakingPSN:_invitation_received_cb( message, ... )
	print( "_invitation_received_cb" )
	print( inspect( message.custom_table ) )
	print( "message.sender", message.sender )
	print( "message", inspect( message ) )
	print( "...", inspect( ... ) )
	
	-- RULES
	-- PSN:display_message_invitation()
end

function NetworkMatchMakingPSN:_invitation_received_result_cb( message )
	print( "_invitation_received_result_cb" )
	print( inspect( message.custom_table ) )
	print( "message.sender", message.sender )
	print( "message", inspect( message ) )
	
	
	if PSN:user_age() < MenuManager.ONLINE_AGE and PSN:parental_control_settings_active() then -- AGE RESTRICTION CHECK
		managers.menu:show_err_under_age()
		return
	end
	
	if not managers.menu:_enter_online_menus() then
		return
	end
		
	if self._room_id == message.room_id then -- Dont join if we are in this game
		return
	end
	
	if message.version ~= self:_game_version() then
		managers.menu:show_invite_wrong_version_message()
		return 
	end
	
	self:_join_invite_accepted( message.room_id )
end

function NetworkMatchMakingPSN:join_boot_invite()
	print( "[NetworkMatchMakingPSN:join_boot_invite]", inspect( Global.boot_invite ) )
	if not Global.boot_invite.pending then
		return
	end
		
	Global.boot_invite.used = true
	Global.boot_invite.pending = false
	
	if PSN:user_age() < MenuManager.ONLINE_AGE and PSN:parental_control_settings_active() then -- AGE RESTRICTION CHECK
		managers.menu:show_err_under_age()
		return
	end
	
	if not managers.menu:_enter_online_menus() then
		return
	end
	
	local message = PSN:get_boot_invitation() -- { version = 2, room_id = PSN:convert_string_to_sessionid( "281479271680579" ) } -- PSN:get_boot_invitation()
	if not message then
		print( "[NetworkMatchMakingPSN:join_boot_invite] PSN:get_boot_invitation failed" )
		return
	end
	print( "[NetworkMatchMakingPSN:join_boot_invite] message: ", message )
	for i, k in pairs( message ) do
		print( i, k )
	end
	if self._room_id == message.room_id then -- Dont join if we are in this game
		print( "[NetworkMatchMakingPSN:join_boot_invite] we are already joined" )
		return
	end
	
	if message.version ~= self:_game_version() then
		print( "[NetworkMatchMakingPSN:join_boot_invite] WRONG VERSION, INFORM USER" )
		managers.menu:show_invite_wrong_version_message()
		return
	end
	
	managers.network:ps3_determine_voice( false )
	self:_join_invite_accepted( message.room_id )
end

--[[function NetworkMatchMakingPSN:create_server( settings )
	self._last_settings = { game_mode = "coop" }
	self:_create_server()
end

function NetworkMatchMakingPSN:_create_server( private )
	local world_list = PSN:get_world_list()
	
	self._server_rpc = nil
	self._players = {}
	
	local session_created = function(roomid) managers.network.matchmake:_create_server_cb(roomid) end
	PSN:set_matchmaking_callback( "session_created", session_created )
	
	local table_description = {}
	local attributes = {}
	
	local gt = {}
	gt.type = "GAME_NUM"
	gt.value = self:_translate_settings( self._last_settings, "game_mode" )
	
	local priv = {}
	priv.type = "GAME_NUM"
	if( private and private == true ) then
		priv.value = 2
	else
		priv.value = 1
	end
	
	attributes[1] = gt
	attributes[2] = priv
	table_description.attributes = attributes
	
	PSN:create_session( table_description, world_list[1].world_id, 0, self.OPEN_SLOTS, 0 )
end

function NetworkMatchMakingPSN:_create_server_cb( roomid )
	self:_clear_psn_callback( "connection_etablished" )
	self._trytime = nil
	
	self:_is_server( true )
	self:_is_client( false )
	
	self._room_id = roomid
	
	local playerinfo = {}
	playerinfo.name 		= managers.network.account:username()
	playerinfo.player_id 	= managers.network.account:player_id()
	playerinfo.group_id 	= tostring( managers.network.group:room_id() )
	playerinfo.rpc 			= Network:self( "TCP_IP" )
	
	self:_call_callback( "found_game", self._room_id, true )
	self:_call_callback( "player_joined", playerinfo )
end]]

--[[function NetworkMatchMakingPSN:_try_servers( list )
	if( self._cancelled == true )  then
		return
	end
	
	if( list ) then
		self._try_list = list
		self._try_index = 1
		managers.network.matchmake:_join_server( self._try_list[ self._try_index ] )
		return
	else
		if( self._try_list and self._try_index < #self._try_list ) then
			self._try_index = self._try_index + 1
			managers.network.matchmake:_join_server( self._try_list[ self._try_index ] )
			return
		end
	end
	
	self._try_list = nil
	self._try_index = nil
	self._trytime = nil
	
	self:_create_server()
end]]

function NetworkMatchMakingPSN:is_server_ok( friends_only, owner_id, attributes_numbers, skip_permission_check )
	local permission = attributes_numbers and tweak_data:index_to_permission( attributes_numbers[3] ) or "public"
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
	if not (attributes_numbers[6] == 1 and NetworkManager.DROPIN_ENABLED) and attributes_numbers[4] ~= 1 then
		return false, 1
	end
	
	-- Never allowed to join private sessions
	-- if permission == "private" then -- This is solved with search filter
	-- 	return false
	-- end


	if managers.experience:current_level() < attributes_numbers[7] then
		return false, 3
	end
	
	
	-- My search criterea is that I only want to join my friends
	-- This should soon not be needed since the list should only consist of servers with friends
	if friends_only then
		-- return managers.network.friends:is_friend( owner_id )
	end
	 
	-- All public servers are ok
	if skip_permission_check or permission == "public" then
		return true
	end
	
	-- I can only join if I am a friend to the server
	if permission == "friends_only" then
		return managers.network.friends:is_friend( owner_id ), 2
	end
	
	return false, 2 -- If a private server is found when searching friends games for example
end

function NetworkMatchMakingPSN:check_server_attributes_failed()
	self:check_server_attributes_done()
end

function NetworkMatchMakingPSN:check_server_attributes_done()
	self._checking_server_attributes = nil
	self._check_room_id = nil
end

function NetworkMatchMakingPSN:join_server_with_check( room_id, skip_permission_check )
	-- Here I need to request the room attributes to make sure they hasn't changed, like it is private now, I should not be allowed to join
	-- Should probably start showing "join_server" dialog here already
	self._joining_lobby = true
	if not managers.system_menu:is_active_by_id( "join_server" ) then
		managers.menu:show_joining_lobby_dialog()
	end
	self._check_room_id = room_id
	self._checking_server_attributes = true
	
	local f = function( results )
		local room_id = self._check_room_id
		self:check_server_attributes_done()
		if not results.rooms then -- Can it happen?
			self:join_server( room_id ) -- Mayby room is dead? Find out by joining
			-- managers.menu:show_game_permission_changed_dialog()
			return
		end
				
		local room_info = results.rooms[ 1 ] -- Should only be one result
		local attributes = room_info.attributes
		local owner_id = room_info.owner
			
		local server_ok, ok_error = self:is_server_ok( nil, owner_id, attributes.numbers, skip_permission_check )	
		if server_ok then
			self:join_server( room_id )
		else
			self:_joining_lobby_done()
			
			if ok_error == 1 then
				managers.menu:show_game_started_dialog()
			elseif ok_error == 2 then
				managers.menu:show_game_permission_changed_dialog()
			elseif ok_error == 3 then
				managers.menu:show_too_low_level()
			elseif ok_error == 4 then
				managers.menu:show_does_not_own_heist()
			end
			
			managers.network.matchmake:start_search_lobbys( self._friends_only ) -- RESEARCH LIST, mayby it disappeares
		end
	end
		
	PSN:set_matchmaking_callback( "fetch_session_attributes", f )
			
	local wanted_attributes = {	numbers = { 1, 2, 3, 4, 5, 6, 7, 8 } }
	PSN:get_session_attributes( { room_id }, wanted_attributes )
	-- self:join_server( room_id )
end

function NetworkMatchMakingPSN:join_server( room_id )
	local room = {}
	room.room_id = room_id
	
	self:_join_server( room )
end

function NetworkMatchMakingPSN:_join_server( room )
	self._connection_info = {}
	self._joining_lobby = true
	
	self._server_rpc = nil
	self._players = {}
	
	-- managers.platform:set_presence( "Is_client" )	
	self:_is_server( false )
	self:_is_client( true )
	
	self._room_id = room.room_id
	
	if not managers.system_menu:is_active_by_id( "join_server" ) then -- It might allready be showing	
		managers.menu:show_joining_lobby_dialog()
	end

	-- local js = function(...) managers.network.matchmake:cb_connection_established(...) end
	-- PSN:set_matchmaking_callback( "connection_etablished", js )
	
	self._join_cb_room = self._room_id
	PSN:join_session( room.room_id )
	
	self._trytime = TimerManager:wall():time() + self._JOIN_SERVER_TRY_TIME_INC
end

function NetworkMatchMakingPSN:_joining_lobby_done_failed()
	self:_joining_lobby_done()
end

function NetworkMatchMakingPSN:_joining_lobby_done()
	managers.system_menu:close( "join_server" )
	self._joining_lobby = nil
end

function NetworkMatchMakingPSN:cb_connection_established( info )
	if self._is_server_var then
		return
	end
	
	print( "NetworkMatchMakingPSN:cb_connection_established" )
	print( inspect( info ) )
	print( "connection established to", info.user_id )
	self._connection_info[ tostring( info.user_id ) ] = info 
	if info.dead then
		--[[if managers.network:session() and managers.network:session():is_client() then
			managers.network:session():on_PSN_connection_established( tostring( info.user_id ) ) -- check_handshake( tostring( info.user_id ) )
		end]]
		if managers.network:session() then
			local peer = managers.network:session():peer_by_name( tostring( info.user_id ) )
			if peer then
				managers.network:session():on_peer_lost( peer, peer:id() )
			end
		end
		return
	end
	-- print( inspect( info ) )
	self._invite_room_id = nil
	if( info.room_id == self._room_id and info.user_id == info.owner_id ) then
		self:_joining_lobby_done()
		self._room_id = info.room_id
		self._game_owner_id = info.owner_id
		
		if( info.external_ip and info.port ) then
			self._server_rpc = Network:handshake( info.external_ip, info.port, "TCP_IP" )
			if( not self._server_rpc ) then
				self._trytime = TimerManager:wall():time()
				return
			end
		else
			self._trytime = TimerManager:wall():time()
			return
		end
				
		self._trytime = nil 
		managers.network:start_client()
		managers.menu:show_waiting_for_server_response( { cancel_func = function() 
																				if managers.network:session() then
																					managers.network:session():on_join_request_cancelled()
																				end
																		end } )
		
		local f = function( res, level_index, difficulty_index, state_index )
			managers.system_menu:close( "waiting_for_server_response" )
			if res == "JOINED_LOBBY" then
				managers.network.voice_chat:open_session( self._room_id )
				MenuCallbackHandler:crimenet_focus_changed( nil, false )
				managers.menu:on_enter_lobby() -- In MenuManagerPD2
				-- managers.menu:close_menu( "menu_main" )
				-- managers.menu:open_menu( "lobby_menu" )
				-- managers.network:session():send_to_peers_loaded( "set_peer_entered_lobby" )
			elseif res == "JOINED_GAME" then	-- The desired host has answered positively. load the level
				managers.network.voice_chat:set_drop_in( { room_id = self._room_id } )
				local level_id = tweak_data.levels:get_level_name_from_index( level_index )
				--[[local level_name = tweak_data.levels[ level_id ].world_name
				managers.network:session():load_level( level_name, nil, nil, nil, level_id, nil )]]
				Global.game_settings.level_id = level_id
				managers.network:session():ok_to_load_level()
			elseif res == "KICKED" then
				managers.network.matchmake:leave_game()
				managers.network.voice_chat:destroy_voice()
				-- managers.network:stop_network( true )
				managers.network:queue_stop_network()
				managers.menu:show_peer_kicked_dialog()
			elseif res == "TIMED_OUT" or res == "FAILED_CONNECT" then
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
				Application:error("[NetworkMatchMakingPSN:connect_to_host_rpc] FAILED TO START MULTIPLAYER!")
			end
		end
				
		managers.network:join_game_at_host_rpc( self._server_rpc, f )
		
		-- Network:set_timeout( self._server_rpc, self._TRY_TIME_INC )
		
		-- self._server_rpc:psn_mm_hello()
		-- self._trytime = TimerManager:wall():time() + self._TRY_TIME_INC
		
-- 	else
-- 		PSN:leave_session( info.room_id )
	elseif info then
		if managers.network:session() then
			managers.network:session():on_PSN_connection_established( tostring( info.user_id ), info.external_ip..":"..info.port ) -- check_handshake( tostring( info.user_id ) )
		end
	end
	
	if( self._join_cb_room and info.room_id == self._join_cb_room ) then
		if( self._cancelled == true ) then
			self._cancel_time = nil
			self:_call_callback( "cancel_done" )
		end
		self._join_cb_room = nil
	end
end

function NetworkMatchMakingPSN:get_connection_info( npid_name )
	return self._connection_info[ npid_name ]
end

--[[function NetworkMatchMakingPSN:_server_alive( server )
	self:_clear_psn_callback( "connection_etablished" )
	
	self._trytime = TimerManager:wall():time() + self._TRY_TIME_INC
	
	if( self._server_rpc and self._server_rpc:ip_at_index(0) == server:ip_at_index(0) ) then
		if( managers.network.group:is_group_leader() ) then
			local players = managers.network.group._players

			local p_num = 1
			local pnids = {}
			table.insert( pnids, managers.network.account:player_id() )

			for k, v in pairs( players ) do
				p_num = p_num + 1
				table.insert( pnids, v.pnid )
			end

			local p = {}
			for i=1,NetworkGeneric._MAX_PLAYERS,1 do
				p[ "player" .. tostring(i) ] = pnids[i] or managers.network.account:player_id()
			end

			self._server_rpc:psn_mm_group_register( p_num, p )
		else
			self._server_rpc:psn_mm_register_player( managers.network.account:username(), managers.network.account:player_id(), tostring( managers.network.group:room_id() ), false )
		end
	end
end]]

--[[function NetworkMatchMakingPSN:_register_group( ids, pnids, rpc )
	local p = 1 + #self._players + self:_call_callback( "reserved_slots", ids, pnids )

	local can_join = self:_call_callback( "can_join" )	
	if can_join == false then
		cat_print( "lobby", "Can join is off as server is starting, group_register ignored." )
	end
	
	if( p+ids > self.OPEN_SLOTS or not can_join ) then
		rpc:psn_mm_group_register_fail()
		return
	end
	
	for i=1,ids,1 do
		self:_call_callback( "reserv_slot", pnids[ "player" .. tostring(i) ]  )
	end
	
	rpc:psn_mm_group_register_ok()
end

function NetworkMatchMakingPSN:_group_register_ok()
	self._trytime = nil
	if( self._server_rpc ) then
		self:_call_callback( "found_game", self._room_id )
		self:_call_callback( "player_joined", {player_id=managers.network.account:player_id(), group_id=tostring( managers.network.group:room_id() ), name=managers.network.account:username(), rpc=Network:self( "TCP_IP" )} )
		
		self._server_rpc:psn_mm_register_player( managers.network.account:username(), managers.network.account:player_id(), tostring( managers.network.group:room_id() ), false )
	end
end

function NetworkMatchMakingPSN:_group_register_fail()
	self._trytime = TimerManager:wall():time()
end]]

--[[function NetworkMatchMakingPSN:_register_player( name, pnid, group, rpc, is_server )
	if( self:_is_server() ) then
		if( not self:_call_callback( "have_reservation", pnid ) ) then
			--send bakc error msg
			return
		end
	end
	
	self._trytime = nil
	if( self._server_rpc ) then
		Network:set_timeout( self._server_rpc, 3000 )
	end
	
	local new_player = {}
	new_player.name = name
	new_player.pnid = pnid
	new_player.group = group
	new_player.rpc = rpc
	
	if( self:_is_server() ) then
		rpc:psn_mm_register_player( managers.network.account:username(), managers.network.account:player_id(), tostring( managers.network.group:room_id() ), true )

		for k, v in pairs( self._players ) do
			v.rpc:psn_mm_register_player( name, pnid, group, false )
			rpc:psn_mm_register_player( v.name, v.pnid, v.group, false )
		end
		
		managers.network.generic:ping_watch( rpc, true, callback( self, self, "_client_timed_out" ), pnid )
	end
	
	if( is_server and is_server == true ) then
		if( not self._private ) then
			managers.network.voice_chat:open_session( self._room_id )
		end
		
		managers.network.generic:ping_watch( self._server_rpc, true, callback( self, self, "_server_timed_out" ), pnid )
	end
	
	table.insert( self._players, new_player )
	
	if( self:_is_server() and self:is_full() ) then
		self:_set_room_hidden( true )
	end
	
	self:_call_callback( "player_joined", {player_id=pnid, group_id=group, name=name, rpc=new_player.rpc} )
end

function NetworkMatchMakingPSN:_unregister_player( pnid, is_server, rpc )
	if( self:_is_client() and is_server == true ) then
		self:remove_ping_watch()
		self._players = {}
		self._server_rpc = nil
-- 		self._room_id = nil
		self._call_server_timed_out = true
		self:leave_game()
		return
	end
	
	local new_list = {}
	for k, v in pairs( self._players ) do
		if( v.pnid ~= pnid ) then
			table.insert( new_list, v )
		end
	end
	self._players = new_list
	
	if( self:_is_server() ) then
		for k, v in pairs( self._players ) do
			v.rpc:psn_mm_unregister_player( pnid, false )
		end
		
		self:_set_room_hidden( false )
		
		managers.network.generic:ping_remove( rpc, true )
	end
	
	self:_call_callback( "player_left", { player_id=pnid, reason="went home to mama" } )
end]]

function NetworkMatchMakingPSN:_in_list( id )
	for k, v in pairs( self._players ) do
		if( tostring(v.pnid) == tostring(id) ) then
			return true
		end
	end
	return false
end

function NetworkMatchMakingPSN:_translate_settings( settings, value )
	if( value == "game_mode" ) then
		local game_mode_in_settings = settings["game_mode"]
		
		if game_mode_in_settings == "coop" then
			return 1
		end
		
		--[[if game_mode_in_settings == "dm" then
			return 1
		elseif game_mode_in_settings == "tdm" then
			return 2
		elseif game_mode_in_settings == "race" then
			return 3
		elseif game_mode_in_settings == "ctf" then
			return 4
		end]]
		
		Application:error( "Not a supported game mode" )
	end
end

function NetworkMatchMakingPSN:_error_cb( info )
	if( info ) then
		print( " _error_cb" )
		print( inspect( info ) )
		
		managers.system_menu:close( "join_server" )
		
		self:_error_message_solver( info )
		
		if info.error == "8002232c" then	-- Context destroy, basicly we have signed out.
			-- print( "CONTEXT DEATROY WE ARE OUUUUT", game_state_machine:current_state_name()  ) 
			-- if game_state_machine:current_state().on_disconnected then
			-- 	game_state_machine:current_state():on_disconnected()
			-- end
		end
		
		if info.error == "8002233a" 				-- Unknown error, failed if creating lobby, failed if searching lobbys
			or info.error == "8002231d" then		-- Request timed out, failed if creating lobby, failed if searching lobbys
		end
		
		if self._checking_server_attributes then
			self:check_server_attributes_failed()
		end
		
		if self._searching_lobbys then
			self:_search_lobby_failed()
		end
		
		if self._creating_lobby then
			self:_create_lobby_failed()
		end
		
		if self._joining_lobby then
			self:_joining_lobby_done_failed()
		end
		
		if self._getting_world_list then
			self:_getting_world_list_failed()
		end
				
		if ( info.error == "80022b19" 			-- server is full
			or info.error == "80022b0f"			-- server is closed
			or info.error == "80022b15"			-- Specified room instance no longer exists.
			or info.error == "80022b13" ) then	-- server no longer exists
			-- Updates lobbys
			if not self._invite_room_id then
				managers.network.matchmake:start_search_lobbys( self._friends_only )
				-- managers.network.matchmake:search_lobby()
			end	
		end
		self._invite_room_id = nil
				
		-- 8002ad23 -- PSN Network sign out
		
		if ( info.error == "80022b19" 		-- server is full
			or info.error == "80022b0f" 	-- server is closed
			or info.error == "80022328"		-- allready joined session
			or info.error == "8002232c"		-- Context destroy, basicly we have signed out.
			or info.error == "80022b13"		-- server no longer exists
			or info.error == "80022b15"		-- Specified room instance no longer exists. 
			and self._trytime ) then
			self._trytime = 0
			self._room_id = nil
		end
	end
end

-- Should return 
function NetworkMatchMakingPSN:_error_message_solver( info )
	if info.error == "8002232c" then	-- Context destroy, basicly we have signed out during loading.
		-- No message needed
		return
	end
	
	if info.error == "8002233a" then
		if self._testing_connection then
			self._testing_connection = nil
			-- No message needed
			return
		end
	end

	local error_texts = { 	["80022b19"] = "dialog_err_room_is_full",		-- server is full
							["80022b0f"] = "dialog_err_room_is_closed", 	-- server is closed
							["80022328"] = "dialog_err_room_allready_joined",  -- allready joined session
							["80022b13"] = "dialog_err_room_no_longer_exists",  -- server no longer exists
							["80022b15"] = "dialog_err_room_no_longer_exists",  -- Specified room instance no longer exists
							["8002233a"] =  (self._creating_lobby and "dialog_err_failed_creating_lobby") or			-- Unknown error, failed if creating lobby
											(self._searching_lobbys and "dialog_err_failed_searching_lobbys") or	  	-- Unknown error, failed if searching lobby
											(self._joining_lobby and "dialog_err_failed_joining_lobby") or nil, 		-- Unknown error, failed if joining lobby
							["8002231d"] =  (self._creating_lobby and "dialog_err_failed_creating_lobby") or  			-- Request timed out, failed if creating lobby
											(self._searching_lobbys and "dialog_err_failed_searching_lobbys") or 		-- Request timed out, failed if seacrhing lobby
											(self._joining_lobby and "dialog_err_failed_joining_lobby") or nil, }		-- Request timed out, failed if joining lobby
															
	local text_id = error_texts[ info.error ]
	if not Application:production_build() then -- Basicly, display no errors without test in release 
		if not text_id then
			return
		end
	end
	
	-- local title = managers.localization:text( "dialog_error_title" )..( text_id and "" or (" ["..info.error.."]"))
	local title = managers.localization:text( "dialog_error_title" )..( Application:production_build() and (" ["..info.error.."]") or "")
		
	local dialog_data = {}
	dialog_data.title = title
	dialog_data.text = text_id and managers.localization:text( text_id ) or info.error_text
	local ok_button = {}
	ok_button.text = managers.localization:text( "dialog_ok" )
	dialog_data.button_list = { ok_button }
	managers.system_menu:show( dialog_data )
end

function NetworkMatchMakingPSN:send_join_invite( friend )
	if not self._room_id then
		return
	end
	local body = managers.localization:text("dialog_mp_invite_message" )
	local len = string.len( body )
	for i = 1, 512 - len do
		body = body .. " "
	end 
	PSN:send_message_gui( {
							attachment = { version = self:_game_version(), room_id = self._room_id },
							body = body,  
							subject = managers.localization:text( "dialog_mp_invite_title" ),
							type = "INVITE",
							list_npid={tostring(friend)} }
							)
	--[[						

	local dialog_data = {}
	dialog_data.title = managers.localization:text( "dialog_mp_send_invite_title" )
	dialog_data.text = managers.localization:text("dialog_mp_send_invite_message", {FRIEND = tostring(friend)} )

	local yes_button = {}
	yes_button.text = managers.localization:text( "dialog_yes" )
	yes_button.callback_func = function() PSN:send_message_custom( friend, self._room_id, { join_invite = true, version = self:_game_version(), invite_id = 1 } ) end -- callback( self, self, "_join_invite_accepted", message.room_id ) -- callback( self, self, "_dialog_progress_resetted_ok" )
	local no_button = {}
	no_button.text = managers.localization:text( "dialog_no" )
	no_button.callback_func = function() end -- callback( self, self, "_dialog_progress_resetted_ok" )
	dialog_data.button_list = { yes_button, no_button }
	
	managers.system_menu:show( dialog_data )
	]]
end

function NetworkMatchMakingPSN:_recived_join_invite( message )
	self._has_pending_invite = true
	print( "_recived_join_invite" ) 
	local dialog_data = {}
	dialog_data.title = managers.localization:text( "dialog_mp_groupinvite_title" )
	dialog_data.text = managers.localization:text("dialog_mp_groupinvite_message", {GROUP = tostring(message.sender)} )

	local yes_button = {}
	yes_button.text = managers.localization:text( "dialog_yes" )
	yes_button.callback_func = callback( self, self, "_join_invite_accepted", message.room_id ) -- callback( self, self, "_dialog_progress_resetted_ok" )
	local no_button = {}
	no_button.text = managers.localization:text( "dialog_no" )
	no_button.callback_func = function() self._has_pending_invite = nil end -- callback( self, self, "_dialog_progress_resetted_ok" )
	dialog_data.button_list = { yes_button, no_button }

	managers.system_menu:show( dialog_data )
end

-- * Application:is_booted_from_invitation()
-- * PSN:get_boot_invitation()

function NetworkMatchMakingPSN:_join_invite_accepted( room_id )
	print( "_join_invite_accepted", room_id )
	self._has_pending_invite = nil
	self._invite_room_id = room_id
	if self._room_id then
		print( "MUST LEAVE ROOM" )
		MenuCallbackHandler:_dialog_leave_lobby_yes()
		return
	end
	
--	self:join_server( room_id )
	self:join_server_with_check( room_id, true )
end

function NetworkMatchMakingPSN:_set_room_hidden( set )
	if set == self._hidden or not self._room_id then
		return
	end
	
	PSN:set_session_visible( self._room_id, not set )
	PSN:set_session_open( self._room_id, not set )
	
	self._hidden = set
end

function NetworkMatchMakingPSN:_server_timed_out( rpc )
	-- self:_unregister_player( nil, true, rpc )
end

function NetworkMatchMakingPSN:_client_timed_out( rpc )
	for k, v in pairs( self._players ) do
		if( v.rpc:ip_at_index(0) == rpc:ip_at_index(0) ) then
			-- self:_unregister_player( v.pnid, false, v.rpc )
			return
		end
	end
end

function NetworkMatchMakingPSN:set_server_joinable( state )
	self._server_joinable = state
	
	self:_set_room_hidden( not state )
end

function NetworkMatchMakingPSN:is_server_joinable()	
	return self._server_joinable
end