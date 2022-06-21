NetworkGroupLobby = NetworkGroupLobby or class()

function NetworkGroupLobby:init()
	--[[ Used variables:
	self._invite_changing_control = nil
	]]
end

function NetworkGroupLobby:_server_timed_out( rpc )
	--[[
	if( managers.networkgui:started() ) then
		managers.networkgui:showdlg_error( "lostserver", callback( self, self, "lost_server_dialog_callback" ) )
	end
	]]
end

--[[
function NetworkGroupLobby:lost_server_dialog_callback()
	if( Global.gametype ) then
		Global.gametype:close_message_leave_group()
	else
		managers.networkgui:hidedlgbox()
	end
end
]]

function NetworkGroupLobby:is_invite_changing_control()
	return self._invite_changing_control
end
