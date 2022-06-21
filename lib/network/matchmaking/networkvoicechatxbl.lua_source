--[[-----------------------------------------------------------------------------------------------

 Voice chat abstraction layer - XboxLive implementation

 To do:

 open_channel_to(player_info, context):
 	player_info: table
 		the player info structure that comes from the player_joined callback
 	context: string
 		"group" - The player info comes from the group abstraction layer
 		"game" - The player info comes from the matchmaking abstraction layer
	Open the voice chat channel on this end. I.e. it will start to listen to
	what player_info.id is transmitting to me and transmit voice data from
	me to player_info.id

 close_channel_to(player_info):
 	player_info: table
 		the player info structure that comes from the player_joined callback
	Stop listening to or transmit to player_info.id

 Implementation notes:
	This implementation is heavily depending on the XboxLive matchmaking and group
	abstraction layer. See open_channel_to()

-----------------------------------------------------------------------------------------------]]--
NetworkVoiceChatXBL = NetworkVoiceChatXBL or class()

NetworkVoiceChatXBL._NUM_PEERS = 4

--[[-----------------------------------------------------------------------------------------------
	Public Functions 
-----------------------------------------------------------------------------------------------]]--

function NetworkVoiceChatXBL:init()
	self.DEFAULT_TEAM = 1			-- Standard team that all new peers are on.
	self._paused = true

	-- self._userid_callback_id 		= managers.platform:add_selected_user_changed_callback( callback( self, self, "user_id_update" ) )
	self._userid_callback_id 		= managers.user:add_user_state_changed_callback( callback( self, self, "user_id_update" ) )
end

function NetworkVoiceChatXBL:open_session()
	-- Global.category_print.lobby = true
	if self._paused == false then
		Application:throw_exception("Trying to re-initialize voice chat")
	end

	self._current_player_index = managers.user:get_platform_id()

	XboxVoice:start(NetworkVoiceChatXBL._NUM_PEERS)
	if self._current_player_index then
		cat_print("lobby", "Voice: Registring Talker", self._current_player_index)
		XboxVoice:register_talker( self._current_player_index )
	end
	
	self._team = self.DEFAULT_TEAM	-- The team we are in. We can only talk to people on our own team when playing.
	
	self._peers = {}
	self:_load_globals()

	self._has_headset = false		-- If this is false we need to disable send of all voice data. We can not disable ourself as talker as we must
									-- be a talker to check if we have a headset :/.
	self._can_communicate = true	-- False if we dont have the privilege to voice chat. If we dont we need to off all sends of voice and also
									-- remove all talkes (besides ourself).
	self._only_friends = false		-- If true we only wish to talk to people that we are friends with.

	self._paused = false			-- When voice system is paused no voice data will be sent but we can still update the flags and setting.
									-- They will take affect when we resume voice chat again.

	self._user_changed = true		-- Set to true if user or settings on user changedand we need to rescan parameters.

	self._number_of_users = 1		-- Number of signed in users.

	self._mute_callback_id 			= managers.platform:add_event_callback( "mute_list_changed", 		callback( self, self, "mute_callback" ) )
	self._voice_callback_id 		= managers.platform:add_event_callback( "voicechat_away", 			callback( self, self, "voicechat_away" ) )
	self._friendsadd_callback_id 	= managers.platform:add_event_callback( "friend_added", 			callback( self, self, "friends_update" ) )
	self._friendsdel_callback_id 	= managers.platform:add_event_callback( "friend_remove", 			callback( self, self, "friends_update" ) )
	self._signin_callback_id 		= managers.platform:add_event_callback( "signin_changed", 			callback( self, self, "user_update" ) )
	self._profile_callback_id 		= managers.platform:add_event_callback( "profile_setting_changed", 	callback( self, self, "user_update" ) )
end

function NetworkVoiceChatXBL:pause()
	cat_print("lobby", "NetworkVoiceChatXBL:pause")

	self._paused = true
	XboxVoice:stop()
end

function NetworkVoiceChatXBL:resume()
	cat_print("lobby", "NetworkVoiceChatXBL:resume")

	self:open_session()

	--[[
	This happens in self:open()

	self._paused = false
	local player_index = managers.user:get_platform_id()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end
	XboxVoice:register_talker(player_index)
	]]

	self:_update_all()
end

-- This implementation is heavily depending on the XboxLive matchmaking and group
-- abstraction layer (NetworkMatchMakingXBL and NetworkGroupLobbyXBL) by reading
-- managers.network.matchmake._lobby and managers.network.group._lobby depending
-- on context.
function NetworkVoiceChatXBL:open_channel_to(player_info, context)
	cat_print("lobby", "Opening Voice Channel to " , tostring(player_info.name))

	local player_index = managers.user:get_platform_id()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end

	local session = nil
	if context == "game" then
		session = managers.network.matchmake._session
	--[[elseif context == "group" then
		session = managers.network.group._lobby
	elseif context == "lan" then
		session = managers.network.systemlink:session()]]
	else
		Application:throw_exception("Context '"..tostring(context).."' not a valid context.")
	end

	-- Make sure we at least tried to get a context
	if session == nil then
		Application:throw_exception("Session retreived from context '"..tostring(context).."' is nil")
	end

	--
	local internal_address = XboxLive:internal_address(session, player_info.external_address)
	player_info.voice_rpc = Network:handshake(internal_address, managers.network.DEFAULT_PORT, "TCP_IP")
	if player_info.voice_rpc then
		-- player_info.voice_rpc:test_message("I " .. tostring( managers.platform:get_username( nil ) )  .. " have added you as a voice target")
		print( "Voice: Created rpc" )
	else
		Application:throw_exception("failed to create voice rpc from here to there")
	end
	
	--
	local peer_info 	= {}
	peer_info.xuid 		= player_info.player_id				
	peer_info.player_id	= tostring(player_info.player_id)	-- Used as index in lists (xuid userdata as index changes sometimes at restart and over network)
	peer_info.rpc 		= player_info.voice_rpc				-- Rpc to send our voice data.
	peer_info.team 		= self.DEFAULT_TEAM
	peer_info.listen 	= true								-- True if we wish to hear what this peer say.
	peer_info.talk		= true								-- True if we wish to talk to (send) the peer
	peer_info.name 		= player_info.name
	peer_info.why		= "open"							-- Why is a debug help that tell use why this channel is open or closed.
	peer_info.dead      = false								-- If true we have lost connection to this voice target.

	self._peers[ peer_info.player_id ] = peer_info

	if self._paused == false then
		self:_peer_flags( peer_info )
		self:_peer_update( peer_info )
	end
end

function NetworkVoiceChatXBL:playerid_to_name( player_id )
	return self._peers[ tostring(player_id) ].name
end

function NetworkVoiceChatXBL:ip_to_name( ip )
	for k, v in pairs( self._peers ) do
		if v.rpc and ip == tostring(v.rpc:ip_at_index(0)) then
			return v.name
		end
	end
end

function NetworkVoiceChatXBL:close_channel_to(player_info)
	cat_print("lobby", "Closing Voice Channel to " , tostring(player_info.name))

	local player_index = managers.user:get_platform_id()

	local peer_info = self._peers[ tostring(player_info.player_id) ]  	
	
	if peer_info then
		cat_print("lobby", "Voice: Stop talking to " , tostring(player_info.name))

		self:_close_peer( peer_info )		
		self._peers[ tostring(player_info.player_id) ] = nil
	end
end

function NetworkVoiceChatXBL:lost_peer( peer )
	-- No people chatting at all.
	if self._peers == nil then
		return
	end
	
	local player_index = managers.user:get_platform_id()

	cat_print("lobby", "Voice: Lost peer " , tostring(peer))

	for k, v in pairs( self._peers ) do
		if v.rpc and peer:ip_at_index(0) == v.rpc:ip_at_index(0) then
			cat_print("lobby", "	Voice: Lost Connection to Name = " , v.name)
			
			self:_close_peer( v )
			self:_peer_flags( v )
			self._peers[ k ] = nil
		end
	end
end

function NetworkVoiceChatXBL:close_all( )
	cat_print("lobby", "Voice: Close all channels ")

	-- XboxVoice:end_session()
	self._paused = true
	XboxVoice:stop()
	cat_print("lobby", "Voice: Close all channels End ")

	--for k, v in pairs( self._peers ) do
	--	Network:reset_connection( v.rpc )
	--end
	--
	self._peers = {}
	self._team = self.DEFAULT_TEAM
	
	-- XboxVoice:start( NetworkVoiceChatXBL._NUM_PEERS )
	-- local player_index = managers.user:get_platform_id()
	-- XboxVoice:register_talker( player_index )
end

-- Used by a local player to set the team he is in. He will then tell all other of his chat targets that he has changed team.
function NetworkVoiceChatXBL:set_team(team)
	cat_print("lobby", "Voice: set_team ", team)
	self._team = team
	
	for k, v in pairs( self._peers ) do
		if v.rpc then
			v.rpc:voice_team( managers.network.account:player_id(), team )
		end
	end
	self:_update_all()
end

-- RPC call from one voice target that tell us that he has changed team.
function NetworkVoiceChatXBL:peer_team( xuid, team, rpc )
	for k, v in pairs( self._peers ) do
		if( v.player_id == tostring(xuid) ) then
			v.team = team
			self:_update_all()
			return
		end
	end
end

-- Reset all peers to be part of the defualt team so everyone can speak to each other. For use at end of game.
function NetworkVoiceChatXBL:clear_team( )
	cat_print("lobby", "Voice: clear_team, eveyone can now speak to each other ")
	self._team = self.DEFAULT_TEAM
	
	for k, v in pairs( self._peers ) do
		v.team = self.DEFAULT_TEAM
	end

	self:_update_all()
end

function NetworkVoiceChatXBL:update( time )
	if self._paused == true then
		return
	end
	
	local player_index = managers.user:get_platform_id()
	
	-- If player index has changed change the active talker.
	if self._current_player_index ~= player_index then
		cat_print("lobby", "Voice: Talker Changing from ", self._current_player_index, " to ", player_index)
		if self._current_player_index then
			XboxVoice:unregister_talker( self._current_player_index )	
		end

		XboxVoice:register_talker( player_index )
		self._current_player_index = player_index
	end 
	
	-- If headset connected/disconnected update flag. 
	local headset = XboxVoice:has_headset( player_index )
	
	if headset ~= self._has_headset then 
		if headset then
			cat_print("lobby", "Voice: Headset connected ")
			self._has_headset = true
		else
			cat_print("lobby", "Voice: Headset disconneted ")
			self._has_headset = false
		end
		
		self:_update_all()
	end		

	-- If any user (Login/Settings) has changed update flags.
	if self._user_changed then
		cat_print("lobby", "Voice: Users (Login/Settings) has changed. Updating voice flags.")
		self._user_changed = false
		
		self:_update_numberofusers()
		
		self:_check_privilege()
		self:_update_all()
	end
end

--[[-----------------------------------------------------------------------------------------------
	Debug 
-----------------------------------------------------------------------------------------------]]--
-- Close down peer if voice chat is on and mark it as dead.
function NetworkVoiceChatXBL:_close_peer( peer )
	local player_index = managers.user:get_platform_id()
	
	if not self._paused and not peer.dead then
		XboxVoice:unregister_talker( peer.xuid )	
		XboxVoice:stop_sending_to(player_index, peer.rpc )	
		-- Network:reset_connection( peer.rpc ) -- This destroyed the connection resulting in final messages not being sent (kicked etc)
	end

	peer.dead = true
	peer.rpc = nil
end

-- Updates the state to a single peer depending on all the state flags, settings and other voice releated things.
function NetworkVoiceChatXBL:_peer_update( peer_info )
	if peer_info.dead then
		return
	end
	
	local player_index = managers.user:get_platform_id()

	if peer_info.listen then
		XboxVoice:register_talker(peer_info.xuid)
	else
		XboxVoice:unregister_talker( peer_info.xuid )	
	end

	if peer_info.talk then
		if self._has_headset then
			XboxVoice:send_to(player_index, peer_info.rpc)
		end
	else
		XboxVoice:stop_sending_to(player_index, peer_info.rpc )	
	end
end

-- Update the flags of a peer, the make them take affect we then use _peer_update
function NetworkVoiceChatXBL:_peer_flags( peer_info )
	-- If peer is dead there is nothing we can do
	if peer_info.dead then
		peer_info.why	 	= "Dead"
		return
	end

	local player_index = managers.user:get_platform_id()

	XboxVoice:unregister_talker( peer_info.xuid )	
	XboxVoice:stop_sending_to(player_index, peer_info.rpc )	

	-- Lets be optimistic and assume we wish to talk to people :).
	peer_info.listen = true	
	peer_info.talk 	 = true	
	peer_info.why	 = "Open"

	if self._can_communicate == false then
		peer_info.listen 	= false	
		peer_info.talk 		= false	
		peer_info.why	 	= "Communications off"
		return
	end

	-- If any logged in user has friends only on.
	if self._only_friends then
		-- If only one user logged in we go on
		if self._number_of_users == 1 then
			if XboxLive:is_friend(player_index, peer_info.xuid) == false then
				peer_info.listen 	= false	
				peer_info.talk 		= false	
				peer_info.why	 	= "Non friend"
				return
			end
		else
			-- With more then one player signed in we turn of voice.
			peer_info.listen 	= false	
			peer_info.talk 		= false	
			peer_info.why	 	= "Friend Limiting"
			return
		end
	end 

	-- First we check if this player has been muted or not. If he is muted then
	-- we stop listen to him and also dont send anything to him.
	if XboxLive:muted(player_index, peer_info.xuid) then
		cat_print("lobby", "Voice: Mute, stop talking to ", peer_info.name)
		peer_info.listen 	= false
		peer_info.talk 		= false	
		peer_info.why	 	= "Muted"
		-- Send IDontCareAboutWhatYouSay Message to remote to lower network traffic.
		return
	end

	-- Now we check teams. Only people of the same team can talk to each other. In lobby all players will be in team one.
	if peer_info.team ~= self._team then
		peer_info.listen 	= false
		peer_info.talk 		= false	
		peer_info.why	 	= "Other Team"
	end
end

function NetworkVoiceChatXBL:_update_all( )
	if self._paused == true then
		return
	end

	for k, v in pairs( self._peers ) do
		self:_peer_flags( v )
		self:_peer_update( v )
	end
end


function NetworkVoiceChatXBL:_save_globals()
	cat_print("lobby", "Voice: NetworkVoiceChatXBL:_save_globals ")
	
	Global.xvoice = nil
	Global.xvoice = {}
	
	Global.xvoice.peers = self._peers
	Global.xvoice.team  = self._team
	
	self:pause()
end

function NetworkVoiceChatXBL:_load_globals()
	cat_print("lobby", "Voice: NetworkVoiceChatXBL:_load_globals ")

	if Global.xvoice == nil then
		return 
	end

	self._team = Global.xvoice.team or self._team

	if( Global.xvoice and Global.xvoice.peers ) then
		self._peers = Global.xvoice.peers
	end	
	
	Global.xvoice = nil
end

function NetworkVoiceChatXBL:_update_numberofusers( )
	self._number_of_users = 0

	for i=0, 3 do 
		if XboxLive:signin_state( i ) ~= "not_signed_in" then
			self._number_of_users = self._number_of_users + 1
		end  
	end	

	cat_print("lobby", "   Voice: Number of users = ", self._number_of_users)
end

-- Returns false if any of the signed in users have communicate privilege turned off
--[[
function NetworkVoiceChatXBL:_check_can_communicate( )
	local cancommunicate
		
	for i=0, 3 do 
		if XboxLive:signin_state( i ) ~= "not_signed_in" then
			cancommunicate = XboxLive:check_privilege( i, "communications")	
			if cancommunicate == false then
				return false
			end 	
		end  
	end	
	
	return true
end

-- Return true if any of the logged in players has friends only set.
function NetworkVoiceChatXBL:_check_friendsonly( )
	local friendsonly
		
	for i=0, 3 do 
		if XboxLive:signin_state( i ) ~= "not_signed_in" then
			friendsonly = XboxLive:check_privilege( i, "communications_friends_only")	
			if friendsonly == true then
				cat_print("lobby", "   Voice: Friends ", i, true)
				return true
			else
				cat_print("lobby", "   Voice: Friends ", i, false)
			end 	
		end  
	end	
	
	return false
end
]]

function NetworkVoiceChatXBL:_get_privilege( userindex )
	local cancommunicate	= true
	local friendsonly		= false

	if XboxLive:signin_state( userindex ) ~= "not_signed_in" then
		cancommunicate 	= XboxLive:check_privilege( userindex, "communications")	
		friendsonly 	= XboxLive:check_privilege( userindex, "communications_friends_only")	
		
		if cancommunicate then
			friendsonly = false	
		else
			if friendsonly then
				cancommunicate = true	
			end		
		end 	
	end  

	return cancommunicate, friendsonly
end

-- This will update the self._can_communicate and self._only_friends flags and return true if any of them has changed.
function NetworkVoiceChatXBL:_check_privilege( )
	local cancommunicate 	= true
	local friendsonly		= false
	local usercancommunicate
	local userfriendsonly	
		
	for i=0, 3 do 
		usercancommunicate, userfriendsonly  = self:_get_privilege( i )
		if usercancommunicate == false then
			cancommunicate = false	
		end 
		if userfriendsonly == true then
			friendsonly = true
		end 
	end	
	
	local flagsupdate = false
	cat_print("lobby", "   Voice: Can Communicate = ", cancommunicate)
	if cancommunicate ~= self._can_communicate then 
		self._can_communicate = cancommunicate	
		flagsupdate = true
		return
	end

	cat_print("lobby", "   Voice: Friends only = ", friendsonly)
	if friendsonly ~= self._only_friends then 
		self._only_friends = friendsonly	
		flagsupdate = true
	end		

	return flagsupdate
end

function NetworkVoiceChatXBL:num_peers( )
	return true
end

function NetworkVoiceChatXBL:destroy_voice( disconnected )
	self:pause()
	--[[if( self._started == true ) then
		self._started = false
		
		if( self._room_id and not disconnected ) then
			self:close_session()
		end
		
		PSNVoice:destroy()
		
		self._closing = nil
		self._room_id = nil
		self._restart_session = nil
		self._team = 1
	end]]
end

function NetworkVoiceChatXBL:set_volume( new_value )
	print( "new_value", new_value ) -- Not done, should it? /Martin
end

function NetworkVoiceChatXBL:is_muted( xuid )
	local player_index = managers.user:get_platform_id()
	return XboxLive:muted( player_index, xuid )
end

function NetworkVoiceChatXBL:set_muted( xuid, state )
	local player_index = managers.user:get_platform_id()
	XboxLive:set_muted( player_index, xuid, state )
end
		
--[[-----------------------------------------------------------------------------------------------
	Callbacks 
-----------------------------------------------------------------------------------------------]]--
function NetworkVoiceChatXBL:user_id_update( id, changed_player_map )
end

function NetworkVoiceChatXBL:mute_callback( )
	cat_print("lobby", "Voice: Mute list changed")
	print( "Voice: Mute list changed")
	self:_update_all()
end

function NetworkVoiceChatXBL:voicechat_away( b )
	cat_print("lobby", "Voice: voicechat_away: ", tostring(b))
	print( "Voice: voicechat_away: ", tostring(b))
end

function NetworkVoiceChatXBL:friends_update( id )
	cat_print("lobby", "Voice: Friends update: ")
	print( "Voice: Friends update: ")
	self:_update_all()
end

function NetworkVoiceChatXBL:user_update( )
	cat_print("lobby", "Voice: NetworkVoiceChatXBL:user_update")
	print( "Voice: NetworkVoiceChatXBL:user_update")
	self._user_changed = true
end

--[[-----------------------------------------------------------------------------------------------
	Debug 
-----------------------------------------------------------------------------------------------]]--
function NetworkVoiceChatXBL:info( )
	self:info_script()
	self:info_engine()
end

-- Print-info about the current state of voice chat for debugging.
function NetworkVoiceChatXBL:info_script( )
	cat_print("lobby", "Voice Script Info")

	cat_print("lobby", "	Active Player:     ", self._current_player_index)
	cat_print("lobby", "	Has Headset:     ", self._has_headset)
	cat_print("lobby", "	Can Communicate: ", self._can_communicate)
	cat_print("lobby", "	Voice Paused:    ", self._paused)
	cat_print("lobby", "	Only Friends:    ", self._only_friends)
	cat_print("lobby", "	Self Team:       ", self._team)
	
	for k, v in pairs( self._peers ) do
		local info = "		" .. v.name 
		info = info .. " Team=" .. tostring( v.team )  	
		info = info .. ", Listen=" .. tostring( v.listen )  	
		info = info .. ", talk=" .. tostring( v.talk )  	
		info = info .. ", why=" .. tostring( v.why )  	
		cat_print("lobby", info)
	end		
end

-- Print-info about the current state of voice chat as the engine tells us.
function NetworkVoiceChatXBL:info_engine( )
	cat_print("lobby", "Voice Engine Info")

	cat_print("lobby", "   Registred Talkers")
	local talkers = XboxVoice:registered_talkers ()
	for k, v in pairs( talkers ) do
		local info
		 
		if type(v) == "number" then
			info = "      " .. tostring(v) .. " - Local Player"
		else
			info = "      " .. tostring(v) .. " - " .. self:playerid_to_name(v)
		end
		
		cat_print("lobby", info)
	end		

	cat_print("lobby", "   Registred Sends")
	local sends = XboxVoice:active_sends ()
	for k, v in pairs( sends ) do
		-- k = local player index, v = multicast rpc
		local num_peers = v:num_peers()
		cat_print("lobby", "      " .. tostring(k) .. " - " .. tostring(num_peers))

		local PeerNumber = 0
		while PeerNumber < num_peers do
			local ip = v:ip_at_index( PeerNumber )	
			cat_print("lobby", "         " .. tostring(ip) .. " - " .. self:ip_to_name( ip ) ) 

			PeerNumber = PeerNumber + 1	
		end
	end	
end

