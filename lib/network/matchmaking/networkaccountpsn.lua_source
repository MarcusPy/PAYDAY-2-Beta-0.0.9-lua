require "lib/network/matchmaking/NetworkAccount"

NetworkAccountPSN = NetworkAccountPSN or class( NetworkAccount )

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

function NetworkAccountPSN:init()
	NetworkAccount.init( self )
	
	-- PSN:init_matchmaking()
end

function NetworkAccountPSN:signin_state()
	if( PSN:is_online() == true ) then
		return "signed in"
	end
	return "not signed in"
end

function NetworkAccountPSN:local_signin_state()
	if( not PSN:cable_connected() ) then
		return false
	end
	local n = PSN:get_localinfo()
	if( not n ) then
		return false
	end
	if( not n.local_ip ) then
		return false
	end
	return true --self:signin_state()
end

function NetworkAccountPSN:show_signin_ui()
	PSN:display_online_connection()
end

function NetworkAccountPSN:username_id()
	local online_name = PSN:get_npid_user()
	if online_name then
		-- We have an online name. Use that as ID
		return online_name
	else
		local local_user_info_name = PS3:get_userinfo()
		if local_user_info_name then
			-- We have a local profile name. Use that as ID
			return local_user_info_name
		end
	end

	-- We don't have online or local user name. Use default.
	return managers.localization:text("menu_mp_player")
end

function NetworkAccountPSN:player_id()
	-- if( managers.network.systemlink:is_lan() ) then
	-- 	return self:_lan_ip()
	-- end
	
	if( PSN:get_npid_user() == nil ) then
		local n = PSN:get_localinfo()
		if( n and n.local_ip ) then
			return n.local_ip
		end
		
		Application:error('Could not get local ip, returning "player_id" VERY BAD!.')
		return "player_id"
-- 		Application:throw_exception( "Could not get local IP." )
	end
	
	return PSN:get_npid_user()
end

function NetworkAccountPSN:is_connected()
	return true
end

function NetworkAccountPSN:lan_connection()
	return PSN:cable_connected()
end

-- // --

function NetworkAccountPSN:_lan_ip()
	local l = PSN:get_lan_info()
	if( l and l.lan_ip ) then
		return l.lan_ip
	end
	return "player_lan"
end

function NetworkAccountPSN:has_mask( mask )
	return false
end


function NetworkAccountPSN:achievements_fetched()
	self._achievements_fetched = true
	self:_check_for_unawarded_achievements()
end

function NetworkAccountPSN:challenges_loaded()
	self._challenges_loaded = true
	self:_check_for_unawarded_achievements()
end

function NetworkAccountPSN:experience_loaded()
	self._experience_loaded = true
	self:_check_for_unawarded_achievements()
end


function NetworkAccountPSN:_check_for_unawarded_achievements()
	if not self._achievements_fetched or not self._challenges_loaded or not self._experience_loaded then
		return
	end



	print( "[NetworkAccountPSN:_check_for_unawarded_achievements]" )
	for _, challenge in ipairs( managers.challenges:get_completed() ) do
		local achievement = managers.challenges:get_awarded_achievment( challenge.id )
		if achievement and not managers.achievment:get_info( achievement ).awarded then
--			print( "[NetworkAccountPSN:_check_for_unawarded_achievements] Awarded unawarded achievements." )
--			managers.achievment:award( achievement )
			managers.challenges:add_already_awarded_challenge( challenge.id )
		end
	end




--[[
	if not managers.challenges:is_completed( "president" ) and managers.experience:current_level() >= 145 then
		managers.challenges:set_flag( "president" )
	end
]]
end