local tmp_vec3 = Vector3()

GrenadeBase = GrenadeBase or class( UnitBase )

function GrenadeBase.spawn( unit_name, pos, rot ) -- Only called from server
	--[[local brush = Draw:brush( Color.red:with_alpha( 0.5 ) )
	brush:set_persistance( 2 ) 
	brush:sphere( pos, 2 )
	brush:set_color( Color.blue:with_alpha( 0.5 ) )
	brush:cylinder( pos, pos + rot:z() * 50, 1 )
	brush:set_color( Color.green:with_alpha( 0.5 ) )
	brush:cylinder( pos, pos + rot:y() * 50, 1 )]]
	
	local unit = World:spawn_unit( Idstring( unit_name ), pos, rot )
	return unit
end

-----------------------------------------------------------------------------------

function GrenadeBase:init( unit )
	UnitBase.init( self, unit, true )
	self._unit = unit
	
	self:_setup()
end

-----------------------------------------------------------------------------------

function GrenadeBase:_setup()
	self._slotmask = managers.slot:get_mask( "trip_mine_targets" )
	self._timer = self._init_timer or 3
end

-----------------------------------------------------------------------------------

function GrenadeBase:set_active( active )
	self._active = active
	self._unit:set_extension_update_enabled( Idstring( "base" ), self._active )
	
end

-----------------------------------------------------------------------------------

function GrenadeBase:active()
	return self._active
end

-----------------------------------------------------------------------------------


-- position, normal, user_unit, dmg, range, curve_pow
-- function GrenadeBase._detect_and_give_dmg( hit_pos, slotmask, user_unit, dmg, range, curve_pow )
-- _collision_slotmask, _user, _damage, _range, _curve_pow, _owner
function GrenadeBase:_detect_and_give_dmg( hit_pos )
	local slotmask = self._collision_slotmask
	local user_unit = self._user
	local dmg = self._damage
	local player_dmg = self._player_damage or dmg
	local range = self._range
	
	
	--[[local brush = Draw:brush( Color.green:with_alpha( 0.5 ), 2 )
	brush:sphere( self._unit:position(), 4 )
	brush:cylinder( self._unit:position(), hit_pos, 2 )]]
	
	
	local player = managers.player:player_unit()
	if alive( player ) and player_dmg ~= 0 then
		player:character_damage():damage_explosion( { position = hit_pos, range = range, damage = player_dmg } )
	end
	
	local bodies = World:find_bodies( "intersect", "sphere", hit_pos, range, slotmask )
	--print( "Found " .. tostring( table.size( bodies ) ) .. " bodies" )

	if user_unit then
		managers.groupai:state():propagate_alert( { "aggression", hit_pos, 10000, self._alert_filter, user_unit } )
	end
	
	-- TODO: Throw 6 or so "splinters" here to raycast against
	local splinters = { mvector3.copy( hit_pos ) }
	
	local dirs = { 
		Vector3( range, 0, 0 ),
		Vector3(-range, 0, 0 ),
		Vector3( 0, range, 0 ),
		Vector3( 0,-range, 0 ),
		Vector3( 0, 0, range ),
		Vector3( 0, 0,-range )
	}

	--local splinter_brush = Draw:brush( Color.green:with_alpha( 0.5 ), 2 )
	--local splinter_brush_fail = Draw:brush( Color.red:with_alpha( 0.5 ), 2 )
	
	local pos = Vector3()
	for _, dir in ipairs( dirs ) do
		mvector3.set( pos, dir )
		mvector3.add( pos, hit_pos )
		local splinter_ray = World:raycast( "ray", hit_pos, pos, "slot_mask", slotmask )
		
		pos = ( splinter_ray and splinter_ray.position or pos ) - dir:normalized() * math.min( splinter_ray and splinter_ray.distance or 0, 10 )
		
		
		--[[if splinter_ray then
			print( "collision", pos )
			splinter_brush_fail:sphere( pos, 20 )
		else
			print( "no collision", pos )
			splinter_brush:sphere( pos, 20 )
		end]]
		
		local near_splinter = false
		for _, s_pos in ipairs( splinters ) do
			if mvector3.distance_sq( pos, s_pos ) < 900 then
				near_splinter = true
				break
			end
		end
		
		if not near_splinter then
			table.insert( splinters, mvector3.copy( pos ) )
		end
	end

	
	--[[for _, s_pos in ipairs( splinters ) do
		splinter_brush:sphere( s_pos, 10 )
	end]]
	
	
	local characters_hit = {}
 	local units_to_push = {}
 	
	for _, hit_body in ipairs( bodies ) do
		local character = hit_body:unit():character_damage() and hit_body:unit():character_damage().damage_explosion
		local apply_dmg = hit_body:extension() and hit_body:extension().damage
		
		units_to_push[ hit_body:unit():key() ] = hit_body:unit()
		
		local dir, len, damage, ray_hit
		if character and not characters_hit[ hit_body:unit():key() ] then
			for i_splinter, s_pos in ipairs( splinters ) do
				ray_hit = not World:raycast( "ray", s_pos, hit_body:center_of_mass(), "slot_mask", slotmask, "ignore_unit", { hit_body:unit() }, "report" )
				if ray_hit then
					--debug_pause_unit( hit_body:unit(), "hit unit", hit_body:unit() )
					--splinter_brush:cylinder( s_pos, hit_body:center_of_mass(), 3 )
					--Application:draw_line( s_pos, hit_body:center_of_mass(), 0, 1, 0 )
					characters_hit[ hit_body:unit():key() ] = true
					break
				--else
					--splinter_brush_fail:cylinder( s_pos, hit_body:position(), 3 )
				end
			end
		elseif apply_dmg or hit_body:dynamic() then
			ray_hit = true
		end
		
	
		if ray_hit then
			dir = hit_body:center_of_mass()
			len = mvector3.direction( dir, hit_pos, dir )
			damage = dmg * math.pow( math.clamp( 1 - len / range, 0, 1 ), self._curve_pow )
			damage = math.max( damage, 1 ) -- under 1 damage is generally not allowed 
			
			local hit_unit = hit_body:unit()
			if apply_dmg then
				local normal = dir
			
				hit_body:extension().damage:damage_explosion( user_unit, normal, hit_body:position(), dir, damage )
				hit_body:extension().damage:damage_damage( user_unit, normal, hit_body:position(), dir, damage )
								
				if hit_unit:id() ~= -1 then
					if alive( user_unit ) then
						managers.network:session():send_to_peers_synched( "sync_body_damage_explosion", hit_body, user_unit, normal, hit_body:position(), dir, damage )
					else
						managers.network:session():send_to_peers_synched( "sync_body_damage_explosion_no_attacker", hit_body, normal, hit_body:position(), dir, damage )
					end
				end
			end
			
--			print( "final damage", hit_unit, damage )
			
			if character then
				local action_data = {}
				action_data.variant = "explosion"
				action_data.damage = damage
				action_data.attacker_unit = user_unit
				action_data.weapon_unit = self._owner
				action_data.col_ray = self._col_ray or { position = hit_body:position(), ray = dir }
				
				hit_unit:character_damage():damage_explosion( action_data )
			end
		end
	end
	
	GrenadeBase._units_to_push( units_to_push, hit_pos, range )
	
	if self._owner then
		managers.challenges:reset_counter( "m79_law_simultaneous_kills" )
		managers.challenges:reset_counter( "m79_simultaneous_specials" )
		
		managers.statistics:shot_fired( { hit = next( characters_hit ) and true or false, weapon_unit = self._owner } )
	end
end

function GrenadeBase._units_to_push( units_to_push, hit_pos, range )
	for u_key, unit in pairs( units_to_push ) do
		if alive( unit ) then
			local is_character = unit:character_damage() and unit:character_damage().damage_explosion
			if not is_character or unit:character_damage():dead() then
				if is_character then
					if unit:movement()._active_actions[1] and unit:movement()._active_actions[1]:type() == "hurt" then
						unit:movement()._active_actions[1]:force_ragdoll()
					end
				end
				local nr_u_bodies = unit:num_bodies()
				local i_u_body = 0
				while i_u_body < nr_u_bodies do
					local u_body = unit:body( i_u_body )
					if u_body:enabled() and u_body:dynamic() then
						local body_mass = u_body:mass()
						local len = mvector3.direction( tmp_vec3, hit_pos, u_body:center_of_mass() )
						local body_vel = u_body:velocity()
						local vel_dot = mvector3.dot( body_vel, tmp_vec3 )
						local max_vel = 800
						if vel_dot < max_vel then
							local push_vel = ( 1 - len / range ) * ( max_vel - math.max( vel_dot, 0 ) )
							mvector3.multiply( tmp_vec3, push_vel )
							u_body:push_at( body_mass/math.random(2), tmp_vec3, u_body:position() ) -- Create some rotation on the bodies
							-- u_body:push( body_mass, tmp_vec3 )
						end
					end
					i_u_body = i_u_body + 1
				end
			end
		end
	end
end

-- Used by grenade launcher explosion to sync what happens on clients
function GrenadeBase._explode_on_client( position, normal, user_unit, dmg, range, curve_pow )
	GrenadeBase._play_sound_and_effects( position, normal, range )
	GrenadeBase._client_damage_and_push( position, normal, user_unit, dmg, range, curve_pow )
end

function GrenadeBase._client_damage_and_push( position, normal, user_unit, dmg, range, curve_pow )
	local bodies = World:find_bodies( "intersect", "sphere", position, range, managers.slot:get_mask( "bullet_impact_targets" ) )

	local units_to_push = {}
	for _, hit_body in ipairs( bodies ) do
		units_to_push[ hit_body:unit():key() ] = hit_body:unit()
		
		-- Only apply damage do units that are not network synced
		local apply_dmg = hit_body:extension() and hit_body:extension().damage and (hit_body:unit():id() == -1)
		local dir, len, damage
		if apply_dmg then
			dir = hit_body:center_of_mass()
			len = mvector3.direction( dir, position, dir )
			damage = dmg * math.pow( math.clamp( 1 - len / range, 0, 1 ), curve_pow )
			damage = math.max( damage, 1 ) -- under 1 damage is generally not allowed
			
			local normal = dir

			hit_body:extension().damage:damage_explosion( user_unit, normal, hit_body:position(), dir, damage )
			hit_body:extension().damage:damage_damage( user_unit, normal, hit_body:position(), dir, damage )
		end
	end
	
	GrenadeBase._units_to_push( units_to_push, position, range )
end

function GrenadeBase._play_sound_and_effects( position, normal, range )
	GrenadeBase._player_feedback( position, normal, range )
	GrenadeBase._spawn_sound_and_effects( position, normal, range )
end

function GrenadeBase._player_feedback( position, normal, range )
	local player = managers.player:player_unit()
	
	if player then
		local feedback = managers.feedback:create( "mission_triggered" )
		local distance = mvector3.distance_sq( position, player:position() )
		local mul = math.clamp( 1 - distance / (range * range), 0, 1 )

		feedback:set_unit( player )
		feedback:set_enabled( "camera_shake", true )
		feedback:set_enabled( "rumble", true )
		feedback:set_enabled( "above_camera_effect", false )
		
		local params = {
			"camera_shake", "multiplier", 	mul,
			"camera_shake", "amplitude", 	0.50,
			"camera_shake", "attack", 		0.05,
			"camera_shake", "sustain", 		0.15,
			"camera_shake", "decay", 		0.50,
			
			"rumble", "multiplier_data", 	mul,
			"rumble", "peak", 				0.50,
			"rumble", "attack", 			0.05,
			"rumble", "sustain", 			0.15,
			"rumble", "release", 			0.50,
		}
		
		feedback:play( unpack( params ) )
	end
end

function GrenadeBase._spawn_sound_and_effects( position, normal, range, effect_name )
	effect_name = effect_name or "effects/particles/explosions/explosion_grenade_launcher"
	if effect_name ~= "none" then
		World:effect_manager():spawn( { effect = Idstring( effect_name ), position = position, normal = normal } )
	end
	
	local sound_source = SoundDevice:create_source( "M79GrenadeBase" )
	sound_source:set_position( position )
	sound_source:post_event( "trip_mine_explode" )
	managers.enemy:add_delayed_clbk( "M79expl", callback( GrenadeBase, GrenadeBase, "_dispose_of_sound", { sound_source = sound_source } ), TimerManager:game():time() + 2 )
end

function GrenadeBase._dispose_of_sound( ... ) -- When this callback is called the table parameter is unreferenced and the sound source can be garbage collected
end


-----------------------------------------------------------------------------------

function GrenadeBase:throw( params )
	self._owner = params.owner
	local velocity = params.dir * 1000
	velocity = Vector3( velocity.x, velocity.y, velocity.z + 100 )
	
	local mass = math.max( 2 * (1-math.abs( params.dir.z )), 1 ) -- Tweaking the mass used to throw pending on looking angle (throws a bit looser if looking up or down)
	self._unit:push_at( mass, velocity, self._unit:position() )
	
	--[[self._unit:body( 0 ):set_collision_script_tag( Idstring( "bounce" ) )
	self._unit:body( 0 ):set_collision_script_filter( 1 )
	self._unit:body( 0 ):set_collision_script_quiet_time( 1 )
	self._unit:set_body_collision_callback( callback( self, self, "_bounce" ) )]]
end

function GrenadeBase:_bounce( ... )
	print( "_bounce", ... )
end

-----------------------------------------------------------------------------------

function GrenadeBase:update( unit, t, dt )
	
	if self._timer then
		self._timer = self._timer - dt
		if self._timer <= 0 then
			self._timer = nil
			self:__detonate()
		end
	end
end

-----------------------------------------------------------------------------------

function GrenadeBase:detonate()
	if not self._active then
		return
	end
	
end

function GrenadeBase:__detonate()
	-- self:_play_sound_and_effects()
	
	if not self._owner then
		return
	end
	
	self:_detonate()
end

function GrenadeBase:_detonate()
	print( "no detonate function for grenade" )
end

--[[function GrenadeBase:sync_trip_mine_explode()
	self:_play_sound_and_effects()
	self._unit:set_slot( 0 )
end]]

--[[function GrenadeBase:_play_sound_and_effects()
	World:effect_manager():spawn( { effect = Idstring( "effects/particles/explosions/explosion_grenade" ), position = self._unit:position(), normal = self._unit:rotation():y() } )
	self._unit:sound_source():post_event( "trip_mine_explode" )
end]]

-----------------------------------------------------------------------------------

function GrenadeBase:save( data )
	local state = {}
	state.timer = self._timer 
	data.GrenadeBase = state
	
end

function GrenadeBase:load( data )
	local state = data.GrenadeBase
	self._timer = state.timer 
end

-----------------------------------------------------------------------------------

function GrenadeBase:destroy()

end

