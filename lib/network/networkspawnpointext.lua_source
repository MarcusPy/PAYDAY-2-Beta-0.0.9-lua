NetworkSpawnPointExt = NetworkSpawnPointExt or class()

function NetworkSpawnPointExt:init( unit )
	if managers.network then
		-- managers.network:register_spawn_point( unit )
	end
end

-- Called from network manager when we start a game
function NetworkSpawnPointExt:get_data( unit )
	return { position = unit:position(), rotation = unit:rotation() }
end

function NetworkSpawnPointExt:destroy( unit )
	--  managers.network:unregister_spawn_point( unit:key() )
end