M79GrenadeBase = M79GrenadeBase or class()

function M79GrenadeBase.spawn( unit_name, pos, rot ) -- Only called from server
	local unit = World:spawn_unit( Idstring( unit_name ), pos, rot )
	return unit
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:init( unit )
	self._unit = unit
	self._new_pos = unit:position()	--stores the newly calculated position after self:_upd_movement( t )
	self._collision_slotmask = managers.slot:get_mask( "bullet_impact_targets" )
	self._spawn_pos = unit:position()
	self._hidden = true
	self._unit:set_visible( false )
	
	self._player_damage = 9
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:launch( params )
	self._owner = params.owner
	self._user = params.user
	self._damage = params.damage
	self._range = params.range
	self._curve_pow = params.curve_pow
	
	self._velocity = params.dir * 4000
	
	self._last_pos = self._unit:position()
	self._last_last_pos = mvector3.copy( self._last_pos )
	self._upd_interval = 0.1	-- update every 0.1 sec
	self._last_upd_t = TimerManager:game():time()
	self._next_upd_t = self._last_upd_t + self._upd_interval
	self._auto_explode_t = self._last_upd_t + 3
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:update( unit, t, dt )
	if t > self._auto_explode_t then
		self:_detonate()
		return
	end
	if t < self._next_upd_t then	-- do not update too often ( I do not trust raycast queries over too short distances. george )
		return
	end
	
	local dt = t - self._last_upd_t
	
	mvector3.set( self._last_last_pos, self._last_pos )
	mvector3.set( self._last_pos, self._new_pos )	-- self._last_pos is now same as unit:position()
	
	self:_upd_velocity( dt ) -- calculate self._velocity and self._new_pos
	
	if self:_chk_collision() then
		self:_detonate()
		return
	end
	
	self:_upd_position()
	
	if self._hidden then
		local safe_dis_sq = 120
		safe_dis_sq = safe_dis_sq*safe_dis_sq
		if mvector3.distance_sq( self._spawn_pos, self._last_pos ) > safe_dis_sq then
			self._hidden = false
			self._unit:set_visible( true )
		end
	end
	
	self._last_upd_t = t
	self._next_upd_t = t + self._upd_interval
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:_upd_velocity( dt )
	local new_vel_z = mvector3.z( self._velocity ) - dt * 981
	mvector3.set_z( self._velocity, new_vel_z )

	-- the following threee lines are: self._new_pos = self._last_pos + self._velocity * dt
	mvector3.set( self._new_pos, self._velocity )
	mvector3.multiply( self._new_pos, dt )
	mvector3.add( self._new_pos, self._last_pos )
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:_upd_position()
	self._unit:set_position( self._new_pos )
	--[[
	--Do we care about the rotation?
		local new_rotation = Rotation( self._velocity, math.UP )
		self._unit:set_rotation( new_rotation )
	]]
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:_chk_collision()
	local col_ray = World:raycast( "ray", self._last_pos, self._new_pos, "slot_mask", self._collision_slotmask )
	--Draw:brush( Color( 1, 0, 0, 1 ), nil, 1 ):line( self._last_pos, self._new_pos )
	col_ray = col_ray or World:raycast( "ray", self._last_last_pos, self._new_pos, "slot_mask", self._collision_slotmask )
	--Draw:brush( Color( 1, 0, 1, 0 ), nil, 1 ):line( self._last_last_pos, self._new_pos )
	if col_ray then
		self._col_ray = col_ray
		return true
	end
end

-----------------------------------------------------------------------------------

function M79GrenadeBase:_detonate()
	if self._detonated then
		debug_pause( "[M79GrenadeBase:_detonate] grenade has already detonated", self._unit, alive( self._unit ) and self._unit:slot() )
		if self._unit:slot() == 0 then
			self._unit:set_slot( 14 ) -- workaround for the unit not being deleted for some reason
		end
		self._unit:set_slot( 0 )
		return
	end
	self._detonated = true
	-- the following threee lines are: expl_normal = -self._velocity:normalized()
	local expl_normal = mvector3.copy( self._velocity )
	mvector3.negate( expl_normal )
	mvector3.normalize( expl_normal )
	
	-- the following threee lines are: expl_pos = ( self._col_ray and self._col_ray.position or self._new_pos ) + expl_normal * 30
	local expl_pos = mvector3.copy( expl_normal )
	mvector3.multiply( expl_pos, 30 )
	if self._col_ray then
		mvector3.add( expl_pos, self._col_ray.position )
	else
		mvector3.add( expl_pos, self._new_pos )
	end
	GrenadeBase._play_sound_and_effects( expl_pos, expl_normal, self._range )

	self._unit:set_slot( 0 )
	
	if not alive( self._owner ) or not alive( self._user ) then
		return
	end

	GrenadeBase._detect_and_give_dmg( self, expl_pos )
	managers.network:session():send_to_peers_synched( "m79grenade_explode_on_client", expl_pos, expl_normal, self._user, self._damage, self._range, self._curve_pow )
end


-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------