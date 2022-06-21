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

NetworkFriendsXBL = NetworkFriendsXBL or class()

function NetworkFriendsXBL:init()
	self._callback = {}
end

function NetworkFriendsXBL:destroy()
end

function NetworkFriendsXBL:set_visible( set )
end

function NetworkFriendsXBL:get_friends_list()
	local player_index = managers.user:get_platform_id()

	if( not player_index ) then
		Application:error( "Player map not ready yet." )
		player_index = 0
	end

	local friend_list = XboxLive:friends( player_index )
	local friends = {}
	for i, friend in ipairs(friend_list) do
		table.insert( friends, NetworkFriend:new( friend.xuid, friend.gamertag ) )
	end
	return friends
end

function NetworkFriendsXBL:get_friends_by_name()
	local player_index = managers.user:get_platform_id()
	local friend_list = XboxLive:friends( player_index )
	local friends = {}
	for i, friend in ipairs(friend_list) do
		friends[ friend.gamertag ] = friend
	end
	return friends
end

function NetworkFriendsXBL:get_friends()
	if not self._initialized then
		self._initialized = true
		self._callback["initialization_done"]( )
	end
end

function NetworkFriendsXBL:register_callback(event, callback)
	self._callback[event] = callback
end

function NetworkFriendsXBL:send_friend_request(nickname)
end

function NetworkFriendsXBL:remove_friend(id)
end

function NetworkFriendsXBL:has_builtin_screen()
	return true
end

function NetworkFriendsXBL:accept_friend_request(player_id)
end
 
function NetworkFriendsXBL:ignore_friend_request(player_id)
end

function NetworkFriendsXBL:num_pending_friend_requests()
	return 0
end

--Internal functions ------------------------------------------------------------------------------
