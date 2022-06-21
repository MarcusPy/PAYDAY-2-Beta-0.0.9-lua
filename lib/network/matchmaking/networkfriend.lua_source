--[[-----------------------------------------------------------------------------------------------

The friend object:
	id:	Unspecified platform dependent ID
		The game will not do anything with this except pass it to various other match
		making methods.
	name: string
		The game can use this to display on screen, so you know who you are playing with.
	signin_status: string
		"signed_in"
		"not_signed_in". Use to show online status of your friends for instance
			
-----------------------------------------------------------------------------------------------]]--

NetworkFriend = NetworkFriend or class()

function NetworkFriend:init(id, name, signin_status)
	self._id = id
	self._name = name
	self._signin_status = signin_status
end

function NetworkFriend:id()
	return self._id
end

function NetworkFriend:name()
	return self._name
end

function NetworkFriend:signin_status()
	return self._signin_status
end
