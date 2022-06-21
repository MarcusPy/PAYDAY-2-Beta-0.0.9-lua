require "lib/network/matchmaking/NetworkAccount"

NetworkAccountXBL = NetworkAccountXBL or class( NetworkAccount )

--[[-----------------------------------------------------------------------------------------------

	Network account abstraction layer
	
	create_account( name, password, email)
	
	reset_password( name, email)
		
	login(name, password, cdkey)
	
	logout()

 register_callback(event, callback)
	event: string
		The event that the function should be linked to. Events:
			"login"				: Called when the login have finished. A string containing a
								  result is passwed to the callback. Results can be:
								  "success" - login was successful
								  "fail_user" - unknown user
								  "fail_password" - user/password mismatch
								  "fail_unknown" - unknown error (probably network related)
			"create_account"	: Called when the create account has finished. A string
								  containing a result is passwed to the callback.
								  Results can be:
								  "success"
								  "fail_user" - Username already taken
								  "fail_unknown" - unknown error (probably network related)

	callback: function
		The function to use
	
	
-----------------------------------------------------------------------------------------------]]--

function NetworkAccountXBL:init()
	NetworkAccount.init( self )
end

function NetworkAccountXBL:signin_state()
	local xbl_state = managers.user:signed_in_state( managers.user:get_index() )
	local game_signin_state = self:_translate_signin_state(xbl_state)
	return game_signin_state
end

function NetworkAccountXBL:local_signin_state()
	local xbl_state = managers.user:signed_in_state( managers.user:get_index() )

	if xbl_state == "not_signed_in" then		return "not signed in"	end
	if xbl_state == "signed_in_locally" then	return "signed in"		end
	if xbl_state == "signed_in_to_live" then	return "signed in"		end

	return "not signed in"
end

function NetworkAccountXBL:show_signin_ui()
end

function NetworkAccountXBL:username_id()
	return Global.user_manager.user_map[ Global.user_manager.user_index ].username
	-- return XboxLive:name()
	-- return managers.platform:get_username( nil )
end

function NetworkAccountXBL:player_id()
	return managers.user:get_xuid( nil )
end

function NetworkAccountXBL:is_connected()
	return true
end

function NetworkAccountXBL:lan_connection()
	return true
end

function NetworkAccountXBL:challenges_loaded()
	self._challenges_loaded = true
	-- self:_check_for_unawarded_achievements()
end

function NetworkAccountXBL:experience_loaded()
	self._experience_loaded = true
	-- self:_check_for_unawarded_achievements()
end

--[[-------------------------------------------------------------------------------------------]]--

function NetworkAccountXBL:_translate_signin_state(xbl_state)
	if xbl_state == "signed_in_to_live" then
		return "signed in"
	end

	return "not signed in"
end

