--[[-----------------------------------------------------------------------------------------------

 Friends abstraction layer

 get_friends_list()
	Return a table of friend ID objects. Friend ID is platform dependent

 get_friends()
	Iterate through all the friends and call a "status_change" for each friend

 register_callback(event, callback)
	Register a callback that will trigger for friends event, such as status change (come
	online/go offline), changes to the list (friend added/removed)
		event: string
			The event this callback should be used for. Events are as follows:
				"friend_added"
					When a friend have accepted to become your friend
				"friend_denied" (Not sure yet. Probably not)
					If your friend request was denied
				"friend_removed"
					If your friend removed you from his list, he'll disappear from your list too
				"friend_request"
					If someone request to be your friend.
				"status_change"
					Friend has come online/gone offline
		callback: function
			The function that should be called when an event has occured

 send_friend_request(nickname)
	Search Rendezvous for users with the nickname provided and send them a friend request
	When he have accepted, we'll receive a "friend_added" event
	We haven't decided what to do if he declines. We'll probably do nothing.

 remove_friend(id)
	Remove friend from your friends list. A request to remove you from that persons friends
	list is sent.

 has_builtin_screen()
 	Return true if this friends abstraction layer provide a built in screen (Xbox) or if
 	the screen has to be implemented by the game (Rendezvous)
 
 accept_friend_request(player_id)
	Call this if you want to accept a friend request
 
 ignore_friend_request(player_id)
	Call this if you don't want to accept a friend request

 num_pending_friend_requests()
	Returns true if there are pending friend requests. False otherwise.

-----------------------------------------------------------------------------------------------]]--

NetworkFriendsPSN = NetworkFriendsPSN or class()

function NetworkFriendsPSN:init()
	cat_print( 'lobby', "friends = NetworkFriendsPSN")
	self._friends = {}
	
	self._callback = {}
	
	self._updated_list_friends = PSN:update_list_friends() --slow, so only run it here.
	
	self._last_info = {}
	self._last_info.friends = 0
	self._last_info.friends_map = {}
	self._last_info.friends_status_map = {}
	--self._last_info.online = 0
	--self._last_info.uninvitable = 0
	--self._last_info.offline = 0
	
	PSN:set_matchmaking_callback( "friends_updated", function() managers.network.friends:psn_update_friends() end )
	PSN:update_async_friends( true, 20 )
end

function NetworkFriendsPSN:destroy()
	PSN:set_matchmaking_callback( "friends_updated", function() end )
	PSN:update_async_friends( false, 20 )
end

function NetworkFriendsPSN:set_visible( set )
	if( set == true ) then
		PSN:update_async_friends( true, 5 )
	else
		PSN:update_async_friends( true, 20 )
	end
end

function NetworkFriendsPSN:call_callback( func, ... )
	if( self._callback[ func ] ) then
		self._callback[ func ]( ... )
	else
		Application:error("Callback", func, "is not registred.")
	end
end
function NetworkFriendsPSN:call_silent_callback( func, ... )
	if( self._callback[ func ] ) then
		self._callback[ func ]( ... )
	end
end

function NetworkFriendsPSN:get_friends_list()
	do
		return self._friends
	end
	
	local npids = {}

	local friends = PSN:get_list_friends()
	for _,f in pairs(friends) do
		table.insert(npids, f)
	end

	return npids
end

function NetworkFriendsPSN:get_names_friends_list()
	if not self._updated_list_friends then
		self._updated_list_friends = PSN:update_list_friends()
	end
	
	local names = {}
	local friends = PSN:get_list_friends()
	if not friends then
		return names
	end
	
	for _,f in pairs( friends ) do
		if f.friend ~= PSN:get_local_userid() then
			names[ tostring( f.friend ) ] = true
		end
	end
	return names
end

function NetworkFriendsPSN:get_npid_friends_list()
	local npids = {}
	local friends = PSN:get_list_friends()
	if not friends then
		return npids
	end
	
	for _,f in pairs( friends ) do
		if f.friend ~= PSN:get_local_userid() then
			table.insert( npids, f.friend )
		end
	end
	return npids
end

function NetworkFriendsPSN:get_friends()	
	cat_print("lobby", "NetworkFriendsPSN:get_friends()" )
	if not self._updated_list_friends then
		self._updated_list_friends = PSN:update_list_friends()
	end
	
	self._friends = {}
		
	local name = managers.network.account:player_id()
		
	local friends = PSN:get_list_friends()
	if( friends ) then
		self._last_info.friends = #friends
		self:_fill_li_friends_map( friends )
		
		--self._last_info.online = 0
		--self._last_info.uninvitable = 0
		--self._last_info.offline = 0
		
		self._last_info.friends_status_map = {}
		
		for k, v in pairs( friends ) do
			-- for i = 1, 34 do
			if( tostring( v.friend ) ~= name ) then
				local online_status = "not_signed_in"
				local info_mod = 1
				
				-- if( v.status == 2 and v.info and v.info == managers.platform:presence() ) then
				--	info_mod = -1
				-- end
				self._last_info.friends_status_map[ tostring( v.friend ) ] = v.status * info_mod
								
				if managers.network.matchmake:user_in_lobby(v.friend) then
					online_status = "in_group"
				elseif managers.network:session() and managers.network:session():is_kicked( tostring( v.friend ) ) then
					online_status = "banned"
				elseif managers.network.group:find(v.friend) then
					online_status = "in_group"
				elseif( v.status == 0 ) then
					-- online_status = "not_signed_in"
					--self._last_info.offline = self._last_info.offline + 1
				elseif( v.status == 1 ) then
					online_status = "signed_in"
					--self._last_info.uninvitable = self._last_info.uninvitable + 1
				elseif( v.status == 2 ) then
					--if( v.info and v.info == "MPLobby" ) then
					-- if( v.info and v.info == "Signed_in" ) then
						online_status = "signed_in"
						--self._last_info.online = self._last_info.online + 1
					-- else
					--	online_status = "uninvitable"
						--self._last_info.uninvitable = self._last_info.uninvitable + 1
					-- end
				end
				
				local f = NetworkFriend:new( v.friend, tostring(v.friend), online_status )
				table.insert(self._friends, f )
				self:call_callback( "status_change", f )
			end
			-- end
		end
		self:call_callback( "get_friends_done", self._friends )
	end
end

function NetworkFriendsPSN:register_callback(event, callback)
	self._callback[ event ] = callback
end

function NetworkFriendsPSN:send_friend_request(nickname)
end

function NetworkFriendsPSN:remove_friend(id)
end

function NetworkFriendsPSN:has_builtin_screen()
	return false
end

function NetworkFriendsPSN:accept_friend_request(player_id)
end
 
function NetworkFriendsPSN:ignore_friend_request(player_id)
end

function NetworkFriendsPSN:num_pending_friend_requests()
	return 0
end

function NetworkFriendsPSN:debug_update(t, dt)
end

--Internal functions ------------------------------------------------------------------------------

function NetworkFriendsPSN:psn_disconnected()
	self._updated_list_friends = false
end

function NetworkFriendsPSN:psn_update_friends()
	-- print( "## NetworkFriendsPSN:psn_update_friends()", PSN:get_list_friends() ) 
	local friends = PSN:get_list_friends() or {}
	if( #friends >= 0 ) then
		local change_of_friends = false
		
		for k, v in pairs( friends ) do
			local friend_in_list = self._last_info.friends_map[ tostring(v.friend) ]
			if( not friend_in_list ) then
				change_of_friends = true
				break
			end
			self._last_info.friends_map[ tostring(v.friend) ] = nil
		end
		
		for k, v in pairs( self._last_info.friends_map ) do
			change_of_friends = true
			break
		end
		
		self:_fill_li_friends_map( friends )
		
		--if( self._last_info.friends ~= #friends ) then
		if( change_of_friends ) then
			self._last_info.friends = #friends
			self._updated_list_friends = PSN:update_list_friends()
			
			self:call_silent_callback( "friends_reset" )
			return
		else
			if( self:_count_online( friends ) ) then
				self:call_silent_callback( "friends_reset" )
				return
			end
		end
	end
end

function NetworkFriendsPSN:is_friend( id )
	-- for _,friend in ipairs( self:get_friends_list() ) do
	local friends = PSN:get_list_friends()
	if not friends then
		return false
	end
	
	for _,data in ipairs( friends ) do
		if data.friend == id then
			return true
		end
	end
	return false
end

function NetworkFriendsPSN:_fill_li_friends_map( friends )
	self._last_info.friends_map = {}
	
	for k, v in pairs( friends ) do
		self._last_info.friends_map[ tostring( v.friend ) ] = true
	end
end

function NetworkFriendsPSN:_fill_li_friends_status_map( friends )
	self._last_info.friends_status_map = {}
	
	for k, v in pairs( friends ) do
		local info_mod = 1
		
		if( v.status == 2 and v.info and v.info == managers.platform:presence() ) then
			info_mod = -1
		end
		
		self._last_info.friends_status_map[ tostring( v.friend ) ] = v.status * info_mod
	end
end

function NetworkFriendsPSN:_count_online( friends )
	local name = managers.network.account:player_id()
	
	--[[
	local online_count = 0
	local uninvitable_count = 0
	local offline_count = 0
	
	for k, v in pairs( friends ) do
		if( tostring( v.friend ) ~= name ) then
			if( v.status == 0 ) then
				offline_count = offline_count + 1
			elseif( v.status == 1 ) then
				uninvitable_count = uninvitable_count + 1
			elseif( v.status == 2 ) then
				if( v.info and v.info == managers.platform:presence() ) then
					online_count = online_count + 1
				else
					uninvitable_count = uninvitable_count + 1
				end
			end
		end
	end
	
	if( self._last_info.online == online_count and self._last_info.uninvitable == uninvitable_count and self._last_info.offline == offline_count ) then
		return false
	end
	
	self._last_info.online = online_count
	self._last_info.uninvitable = uninvitable_count
	self._last_info.offline = offline_count
	]]
	
	local status_changed = false
	for k, v in pairs( friends ) do
		local friend_status = self._last_info.friends_status_map[ tostring( v.friend ) ] or 42
		local info_mod = 1
		
		--if( v.status == 2 and v.info and v.info == managers.platform:presence() ) then
		--	info_mod = -1
		--end
		
		if( tostring( v.friend ) ~= name ) then
			if( friend_status ~= v.status * info_mod ) then
				status_changed = true
				break
			end
		end
	end
	
	if( not status_changed ) then
		return false
	end
	
	self:_fill_li_friends_status_map( friends )
	return true
end
