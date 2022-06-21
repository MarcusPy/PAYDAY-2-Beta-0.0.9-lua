HostStateInLobby = HostStateInLobby or class( HostStateBase )


function HostStateInLobby:on_join_request_received( data, peer_name, client_preferred_character, dlcs, xuid, peer_level, gameversion, join_attempt_identifier, sender )
	print( "HostStateInLobby:on_join_request_received peer_level", peer_level, join_attempt_identifier, gameversion ) 
	
	if self:_has_peer_left_PSN( peer_name ) then
		print( "this CLIENT has left us from PSN, ignore his request", peer_name )
		return
	end
	
	local my_user_id = data.local_peer:user_id() or ""
	
	if self:_is_kicked( data, peer_name, sender ) then
		print( "YOU ARE IN MY KICKED LIST", peer_name )
		--sender:join_request_reply( 2, 0, "", 1, 1, 0, "", my_user_id, "", 0, 0, 0, 0 )
		self:_send_request_denied( sender, 2, my_user_id )
		return
	end
		
	if peer_level < Global.game_settings.reputation_permission then -- To low level check
		self:_send_request_denied( sender, 6, my_user_id )
		return
	end
		
	-- Only valid on X360 
	if gameversion ~= -1 and gameversion ~= managers.network.matchmake.GAMEVERSION then -- Wrong Gameversion check
		self:_send_request_denied( sender, 7, my_user_id )
		return
	end
	
	--[[if not self:_chk_peer_owns_current_dlc( data, dlcs )
		-- Reject because peer do not own the current level
		print( "YOU DO NOT OWN THIS LEVEL", peer_name )
		-- sender:join_request_reply( 4, 0, "", 1, 1, 0, "", my_user_id, "", 0, 0, 0, 0 )
		self:_send_request_denied( sender, 4, my_user_id )
		return
	end]]
	
	if data.wants_to_load_level then
		-- sender:join_request_reply( 0, 0, "", 1, 1, 0, "", my_user_id, "", 0, 0, 0, 0 )	-- He may not join at this moment. he may try again after we have loaded
		self:_send_request_denied( sender, 0, my_user_id )
		return
	end
	
	--print( "[HostNetworkSession:on_join_request_received] peers", inspect( self._peers ) )
	local old_peer = data.session:chk_peer_already_in( sender )
	if old_peer then	-- this is a re-join attempt. he needs to time out first
		if join_attempt_identifier ~= old_peer:join_attempt_identifier() then -- new join attempt when already in? No!
			data.session:remove_peer( old_peer, old_peer:id(), "lost" )
			-- sender:join_request_reply( 0, 0, "", 1, 1, 0, "", my_user_id, "", 0, 0, 0, 0 )	-- He may not join
			self:_send_request_denied( sender, 0, my_user_id )
		end
		return
	end
	
	if table.size( data.peers ) >= 3 then
		print( "server is full" )
		-- sender:join_request_reply( 0, 0, "", 1, 1, 0, "", my_user_id, "", 0, 0, 0, 0 )	-- He may not join
		self:_send_request_denied( sender, 5, my_user_id )
		return
	end
	
	print( "[HostStateInLobby:on_join_request_received] new peer accepted", peer_name )
	
	local character = managers.network:game():check_peer_preferred_character( client_preferred_character )
	
	local xnaddr = ""
	if SystemInfo:platform() == Idstring( "X360" ) then
		xnaddr = managers.network.matchmake:external_address( sender )
	end
	-- Add the new guy to our peers
	local new_peer_id, new_peer
	new_peer_id, new_peer = data.session:add_peer( peer_name, nil, true, false, false, nil, character, sender:ip_at_index(0), xuid, xnaddr )--( name, rpc, in_lobby, loading, synched, id, character, user_id )
	if not new_peer_id then
		print( "there was no clean peer_id" )
		-- sender:join_request_reply( 0, 0, "", 1, 1, 0, "", my_user_id, "", 0, 0, 0, 0 )	-- He may not join
		self:_send_request_denied( sender, 0, my_user_id )
		return
	end
	new_peer:set_dlcs( dlcs )
	new_peer:set_xuid( xuid )
	new_peer:set_join_attempt_identifier( join_attempt_identifier )
	
	local new_peer_rpc
	if managers.network:protocol_type() == "TCP_IP" then
		new_peer_rpc = managers.network:session():resolve_new_peer_rpc( new_peer, sender )
	else
		new_peer_rpc = sender
	end
	
	new_peer:set_rpc( new_peer_rpc )
	new_peer:set_ip_verified( true )
	Network:add_client( new_peer:rpc() )
	
	new_peer:set_entering_lobby( true )
	
	local level_index = tweak_data.levels:get_index_from_level_id( Global.game_settings.level_id )
	local difficulty_index = tweak_data:difficulty_to_index( Global.game_settings.difficulty )
	
	local job_id_index = 0
	local job_stage = 0
	local alternative_job_stage = 0
	local interupt_job_stage_level_index = 0
	if managers.job:has_active_job() then
		job_id_index = tweak_data.narrative:get_index_from_job_id( managers.job:current_job_id() )
		job_stage = managers.job:current_stage()
		alternative_job_stage = managers.job:alternative_stage() or 0
		local interupt_stage_level = managers.job:interupt_stage()
		interupt_job_stage_level_index = interupt_stage_level and tweak_data.levels:get_index_from_level_id( interupt_stage_level ) or 0
	end
	-- Tell him he is accepted
	-- new_peer:send( "join_request_reply", 1, new_peer_id, level_index, difficulty_index, 1, data.local_peer:mask_set(), my_user_id, Global.game_settings.mission, job_id_index, job_stage, alternative_job_stage, interupt_job_stage_level_index )
	local server_xuid = SystemInfo:platform() == Idstring( "X360" ) and managers.network.account:player_id() or ""
	new_peer:send( "join_request_reply", 1, new_peer_id, character, level_index, difficulty_index, 1, data.local_peer:character(), my_user_id, Global.game_settings.mission, job_id_index, job_stage, alternative_job_stage, interupt_job_stage_level_index, server_xuid )
	new_peer:send( "set_loading_state", false )	 -- Only really valid when we are in the lobby
	if SystemInfo:platform() == Idstring( "X360" ) then
		new_peer:send( "request_player_name_reply", managers.network:session():local_peer():name() )
	end
	
	self:_introduce_new_peer_to_old_peers( data, new_peer, false, peer_name, new_peer:character(), "remove", new_peer:xuid(), new_peer:xnaddr() )--( data, new_peer, loading, peer_name, character, mask_set )
	self:_introduce_old_peers_to_new_peer( data, new_peer )
	
	self:on_handshake_confirmation( data, new_peer, 1 )
	
	Global.local_member:sync_lobby_data( new_peer )
end

------------------------------------------------------------------

function HostStateInLobby:is_joinable( data )
	return not data.wants_to_load_level
end

------------------------------------------------------------------
------------------------------------------------------------------