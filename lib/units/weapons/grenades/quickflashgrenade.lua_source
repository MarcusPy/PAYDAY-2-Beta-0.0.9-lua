QuickFlashGrenade = QuickFlashGrenade or class( QuickSmokeGrenade )

function QuickFlashGrenade:preemptive_kill()
	self._unit:set_slot( 0 )
end

-----------------------------------------------------------------------------------

function QuickFlashGrenade:_play_sound_and_effects()
	if self._state == 1 then
		local sound_source = SoundDevice:create_source( "grenade_fire_source" )
		sound_source:set_position(self._shoot_position)
	
		sound_source:post_event( "grenade_gas_npc_fire" )
	elseif self._state == 2 then
	
		-- Play bounce at some mid-point
		local bounce_point = Vector3()
		
		-- Don't bounce at the exact mid-point, bounce at around 65% between origin and end position
		mvector3.lerp(bounce_point, self._shoot_position, self._unit:position(), 0.65)
		
		local sound_source = SoundDevice:create_source( "grenade_bounce_source" )
		sound_source:set_position(bounce_point)
		
		sound_source:post_event( "grenade_gas_bounce" )
	
	elseif self._state == 3 then
		--World:effect_manager():spawn( { effect = Idstring( "effects/particles/explosions/explosion_smoke_grenade" ), position = self._unit:position(), normal = self._unit:rotation():y() } )
		self._unit:sound_source():post_event( "flashbang_explosion" )
		
		local parent = self._unit:orientation_object()
		--self._smoke_effect = World:effect_manager():spawn( { effect = Idstring( "effects/particles/explosions/smoke_grenade_smoke" ), parent = parent } ) -- position = self._unit:position(), normal = self._unit:rotation():y() } )
		local detonate_pos = self._unit:position()
		local range = 1000
		local affected, line_of_sight, travel_dis, linear_dis = self:_chk_dazzle_local_player( detonate_pos, range )
		if affected then
			managers.environment_controller:set_flashbang( detonate_pos, line_of_sight, travel_dis, linear_dis )
			
			local sound_eff_mul = math.clamp( 1 - ( travel_dis or linear_dis ) / range, 0.3, 1 )
			managers.player:player_unit():character_damage():on_flashbanged( sound_eff_mul )
		end
		managers.groupai:state():propagate_alert( { "aggression", detonate_pos, 10000, managers.groupai:state():get_unit_type_filter( "civilians_enemies" ), nil } )
	end
end

-----------------------------------------------------------------------------------

function QuickFlashGrenade:_chk_dazzle_local_player( detonate_pos, range )
	local player = managers.player:player_unit()
	if not alive( player ) then
		return
	end
	
	local detonate_pos = detonate_pos or self._unit:position() + math.UP * 150
	local m_pl_head_pos = player:movement():m_head_pos()
	
	--local brush = Draw:brush( Color.green:with_alpha( 0.5 ), 2 )
	--brush:sphere( detonate_pos, 4 )
	
	local linear_dis = mvector3.distance( detonate_pos, m_pl_head_pos )
	if linear_dis > range then
		return
	end
	
	local slotmask = managers.slot:get_mask( "bullet_impact_targets" )
	
	local _vis_ray_func = function( from, to, boolean )
		return World:raycast( "ray", from, to, "slot_mask", slotmask, boolean and "report" or nil )
	end
	
	if not _vis_ray_func( m_pl_head_pos, detonate_pos, true ) then
		return true, true, nil, linear_dis
	end
	
	-- no line of sight, check bounce
	
	--local bounce_brush = Draw:brush( Color.green:with_alpha( 0.5 ), 2 )
	--local bounce_brush_fail = Draw:brush( Color.red:with_alpha( 0.5 ), 2 )
	
	local random_rotation = Rotation( 360 * math.random(), 360 * math.random(), 360 * math.random() )
	
	local raycast_dir = Vector3()
	local bounce_pos = Vector3()
	for _, axis in ipairs( { "x", "y", "z" } ) do
		for _, polarity in ipairs( { 1, -1 } ) do
			mvector3.set_zero( raycast_dir )
			mvector3[ "set_"..axis ]( raycast_dir, polarity ) -- ex: mvector3.set_z( raycast_vec, -1 )
			mvector3.rotate_with( raycast_dir, random_rotation )
			
			mvector3.set( bounce_pos, raycast_dir )
			mvector3.multiply( bounce_pos, range )
			mvector3.add( bounce_pos, detonate_pos )
			
			local bounce_ray = _vis_ray_func( detonate_pos, bounce_pos )
			
			if bounce_ray then -- we hit something
				--bounce_brush:sphere( bounce_ray.position, 20 )
				mvector3.set( bounce_pos, raycast_dir )
				mvector3.multiply( bounce_pos, -1 * math.min( bounce_ray.distance, 10 ) )
				mvector3.add( bounce_pos, bounce_ray.position )
				
				local return_ray = _vis_ray_func( m_pl_head_pos, bounce_pos, true )
				if not return_ray then -- we have line of sight
					local travel_dis = bounce_ray.distance + mvector3.distance( m_pl_head_pos, bounce_pos )
					if travel_dis < range then
						return true, false, travel_dis, linear_dis
					end
				end
			--else -- no bounce
				--bounce_brush_fail:sphere( bounce_ray.position, 20 )
			end
		end
	end
end

-----------------------------------------------------------------------------------

function QuickFlashGrenade:destroy()
end

-----------------------------------------------------------------------------------
