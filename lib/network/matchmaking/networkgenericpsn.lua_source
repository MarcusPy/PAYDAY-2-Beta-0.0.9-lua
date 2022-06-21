NetworkGenericPSN = NetworkGenericPSN or class()

function NetworkGenericPSN:init()
	cat_print( 'lobby', "generic = NetworkGenericPSN")
	
	local f = function() end
	PSN:set_matchmaking_callback( "room_invitation", f )
	
	local psn_left = function(...) self:psn_member_left(...) end
	PSN:set_matchmaking_callback( "member_left", psn_left )
	
	local psn_join = function(...) self:psn_member_joined(...) end
	PSN:set_matchmaking_callback( "member_joined", psn_join )
	
	local psn_destroyed = function(...) self:psn_session_destroyed(...) end
	PSN:set_matchmaking_callback( "session_destroyed", psn_destroyed )
end

function NetworkGenericPSN:update(time)
	managers.network.voice_chat:update()
end

function NetworkGenericPSN:start_game()
	--we need to save a ref to rendezvous
	Global.rendezvous = {}
	Global.rendezvous.rendevous = managers.network.shared_rdv:rendezvousonline()
	Global.rendezvous.is_online = managers.network.shared_rdv:is_online()

	managers.network.voice_chat:_save_globals( true )
	managers.network.group:_save_global()
	managers.network.matchmake:_save_global()
end

function NetworkGenericPSN:end_game()
	--we need to save a ref to rendezvous
	Global.rendezvous = {}
	Global.rendezvous.rendevous = managers.network.shared_rdv:rendezvousonline()
	Global.rendezvous.is_online = managers.network.shared_rdv:is_online()
	
	managers.network.generic:set_entermenu( true )
	managers.network.voice_chat:_save_globals( managers.network.group:room_id() or false )
	managers.network.group:_save_global()
end

--[[-------------------------------------------------------------------------------------------]]--

function NetworkGenericPSN:psn_member_joined( info )
	managers.network.matchmake:psn_member_joined( info )
end

function NetworkGenericPSN:psn_member_left( info )
	managers.network.matchmake:psn_member_left( info )
end

function NetworkGenericPSN:psn_session_destroyed( info )
	cat_print("lobby", "NetworkGenericPSN:_session_destroyed_cb" )
	cat_print_inspect("lobby", info )
	
-- 	local room_id = info["room_id"]
-- 	local reason = info["eventcause"]
-- 	local text = "If you have a console log and sit at Grin, talk to Karl. If you don't have a console log, write the following text as a comment to bug 21651 in mantis: \""
-- 	text = text .. tostring(reason)
-- 	text = text .. ", " .. tostring(room_id == self._room_id) .. ", " .. tostring(room_id == managers.network.matchmake._room_id) .. "\""
--	if reason ~= "np_signed_out" and reason ~= "leave" and reason ~= "member_disappeared" then
--		managers.menu:push_screen(DlgMessage,
--		{
--			label="The room has been unexpectedly destroyed",
--			text = text,
--			buttons=
--			{
--				{button="a", label="Ok", callback=function() managers.menu._trad_screen_stack[1]:close() end},
--			}
--		}, false)
--	end
	
	managers.network.voice_chat:psn_session_destroyed( info.room_id )
	managers.network.matchmake:_session_destroyed_cb( info.room_id )
	managers.network.group:_session_destroyed_cb( info.room_id )
end
-- Possible reasons:
--  case SCE_NP_MATCHING2_EVENT_CAUSE_LEAVE_ACTION    : cause_name = "leave";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_KICKOUT_ACTION   : cause_name = "kickout";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_GRANT_OWNER_ACTION  : cause_name = "grant_owner";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_SERVER_OPERATION    : cause_name = "server_operation";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_MEMBER_DISAPPEARED  : cause_name = "member_disappeared";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_SERVER_INTERNAL     : cause_name = "server_internal";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_CONNECTION_ERROR    : cause_name = "connection_error";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_NP_SIGNED_OUT       : cause_name = "np_signed_out";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_SYSTEM_ERROR        : cause_name = "system_error";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_CONTEXT_ERROR       : cause_name = "context_error";break;
--  case SCE_NP_MATCHING2_EVENT_CAUSE_CONTEXT_ACTION      : cause_name = "context_action";break; 
