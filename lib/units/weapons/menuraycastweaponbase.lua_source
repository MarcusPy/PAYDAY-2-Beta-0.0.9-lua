NewRaycastWeaponBase = NewRaycastWeaponBase or class()

function NewRaycastWeaponBase:init( unit )
	self._unit = unit
end

function NewRaycastWeaponBase:set_factory_data( factory_id )
	self._factory_id = factory_id
	-- Could/should be blueprint
end

--[[function NewRaycastWeaponBase:get_name_id()
	return "m4"
end]]

function NewRaycastWeaponBase:set_npc( npc )
	self._npc = npc
end

function NewRaycastWeaponBase:is_npc()
	return self._npc or false
end

function NewRaycastWeaponBase:assemble( factory_id, skip_queue )
	local third_person = self:is_npc()
	self._parts, self._blueprint = managers.weapon_factory:assemble_default( factory_id, self._unit, third_person, callback( self, self, "_assemble_completed" ), skip_queue )
	self:_update_stats_values()
	if true then return end

	local third_person = self:is_npc()
	self._parts, self._blueprint = managers.weapon_factory:assemble_default( factory_id, self._unit, third_person )
	-- print( "NewRaycastWeaponBase:assemble", inspect( self._blueprint ) )
	
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:assemble_from_blueprint( factory_id, blueprint, skip_queue )
	local third_person = self:is_npc()
	self._parts, self._blueprint = managers.weapon_factory:assemble_from_blueprint( factory_id, self._unit, blueprint, third_person, callback( self, self, "_assemble_completed" ), skip_queue )
	self:_update_stats_values()
	if true then return end
	-- print( "1 NewRaycastWeaponBase:assemble_from_blueprint", factory_id, inspect( blueprint ) )
	-- Application:stack_dump()
	local third_person = self:is_npc()
	self._parts, self._blueprint = managers.weapon_factory:assemble_from_blueprint( factory_id, self._unit, blueprint, third_person )
	-- print( "2 NewRaycastWeaponBase:assemble_from_blueprint", factory_id, inspect( self._blueprint ), inspect( self._parts ) )
	
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:_assemble_completed( parts, blueprint )
	print( "NewRaycastWeaponBase:_assemble_completed", parts, blueprint )
	
	self._parts = parts
	self._blueprint = blueprint
	
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:check_npc()
end

-----------------------------------------------------------------------------------

function NewRaycastWeaponBase:change_part( part_id )
	-- print( "self._blueprint", inspect( self._blueprint ) )
	self._parts = managers.weapon_factory:change_part( self._unit, self._factory_id, part_id or "wpn_fps_m4_uupg_b_sd", self._parts, self._blueprint )
	-- print( "change", self._parts )
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:remove_part( part_id )
	self._parts = managers.weapon_factory:remove_part( self._unit, self._factory_id, part_id, self._parts, self._blueprint )
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:remove_part_by_type( type )
	self._parts = managers.weapon_factory:remove_part_by_type( self._unit, self._factory_id, type, self._parts, self._blueprint )
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:change_blueprint( blueprint )
	self._blueprint = blueprint
	self._parts = managers.weapon_factory:change_blueprint( self._unit, self._factory_id, self._parts, blueprint )
	self:_update_fire_object()
	self:_update_stats_values()
end

function NewRaycastWeaponBase:blueprint_to_string()
	-- print( inspect( self._blueprint ) )
	local s = managers.weapon_factory:blueprint_to_string( self._factory_id, self._blueprint )
	-- print( s )
	-- print( inspect( managers.weapon_factory:unpack_blueprint_from_string( self._factory_id, s ) ) )
	return s 
end

-----------------------------------------------------------------------------------
function NewRaycastWeaponBase:_update_fire_object()
	local fire = managers.weapon_factory:get_part_from_weapon_by_type( "barrel_ext", self._parts )
						or managers.weapon_factory:get_part_from_weapon_by_type( "slide", self._parts )
						or managers.weapon_factory:get_part_from_weapon_by_type( "barrel", self._parts )
	
	-- self:change_fire_object( fire.unit:get_object( Idstring( "fire" ) ) )
end

function NewRaycastWeaponBase:_update_stats_values()
	if true then return end
	
	local base_stats = self:weapon_tweak_data().stats
	if not base_stats then
		--print( "no stats" )
		return
	end 
	
	-- print( inspect( base_stats ) )
	local parts_stats = managers.weapon_factory:get_stats( self._factory_id, self._blueprint )
	-- print( inspect( parts_stats ) )
	
	self._silencer = managers.weapon_factory:has_perk( "silencer", self._factory_id, self._blueprint )
	
	local stats = deep_clone( base_stats )
	local tweak_data = tweak_data.weapon.stats
	
	if( stats.zoom ) then
		stats.zoom = math.min( stats.zoom + managers.player:upgrade_value( self:weapon_tweak_data()[ "category" ], "zoom_increase", 0 ), #tweak_data.zoom )		-- not that base + skill increase will ever go off the chart (table), but still
	end

	for stat,_ in pairs( stats ) do
		if parts_stats[ stat ] then
			stats[ stat ] = math_clamp( stats[ stat ] + parts_stats[ stat ], 1, #tweak_data[ stat ] )
		end
	end
	
	self._current_stats = {}
	for stat, i in pairs( stats ) do
		-- print( stat, tweak_data[ stat ][ i ] )
		self._current_stats[ stat ] = tweak_data[ stat ][ i ]
	end
	
	self._alert_size = self._current_stats.alert_size or self._alert_size
	self._suppression = self._current_stats.suppression or self._suppression
	self._zoom = self._current_stats.zoom or self._zoom
	self._spread = self._current_stats.spread or self._spread
	self._recoil = self._current_stats.recoil or self._recoil
	self._spread_moving = self._current_stats.spread_moving or self._spread_moving
end

-----------------------------------------------------------------------------------

function NewRaycastWeaponBase:stance_id()
	-- Could mayby be base on something else
	return "new_m4"
end

function NewRaycastWeaponBase:weapon_hold()
	return self:weapon_tweak_data().weapon_hold
end

function NewRaycastWeaponBase:stance_mod()
	if not self._parts then
		return nil
	end
	
	local factory = tweak_data.weapon.factory
	for part_id, data in pairs( self._parts ) do
		if factory.parts[ part_id ].stance_mod then
			if factory.parts[ part_id ].stance_mod[ self._factory_id ] then
				return { translation = factory.parts[ part_id ].stance_mod[ self._factory_id ].translation }
			end   
		end
	end
	-- Could mayby be base on something else
	return nil
end

-----------------------------------------------------------------------------------

function NewRaycastWeaponBase:tweak_data_anim_play( anim, speed_multiplier )
	-- print( "NewRaycastWeaponBase:tweak_data_anim_play", anim )
	
	-- On Unit
	local data = tweak_data.weapon.factory[ self._factory_id ]
	if data.animations and data.animations[ anim ] then
		local anim_name = data.animations[ anim ]
		local length = self._unit:anim_length( Idstring( anim_name ) )
		speed_multiplier = speed_multiplier or 1
		
		self._unit:anim_stop( Idstring( anim_name ) )
		self._unit:anim_play_to( Idstring( anim_name ), length, speed_multiplier )
	end
	
	-- On parts	
	for part_id, data in pairs( self._parts ) do
		if data.animations and data.animations[ anim ] then
			local anim_name = data.animations[ anim ]
			local length = data.unit:anim_length( Idstring( anim_name ) )
			speed_multiplier = speed_multiplier or 1
			-- print( data.unit, anim_name, length )
		
			data.unit:anim_stop( Idstring( anim_name ) )
			data.unit:anim_play_to( Idstring( anim_name ), length, speed_multiplier )
		end
	end
	
	return true -- NewRaycastWeaponBase.super.tweak_data_anim_play( self, anim, speed_multiplier )
end

function NewRaycastWeaponBase:tweak_data_anim_stop( anim )
	-- print( "NewRaycastWeaponBase:tweak_data_anim_stop", anim )
	
	-- On Unit
	local data = tweak_data.weapon.factory[ self._factory_id ]
	if data.animations and data.animations[ anim ] then
		local anim_name = data.animations[ anim ]
		self._unit:anim_stop( Idstring( anim_name ) )
	end
	
	-- On parts	
	for part_id, data in pairs( self._parts ) do
		if data.animations and data.animations[ anim ] then
			local anim_name = data.animations[ anim ]
			data.unit:anim_stop( Idstring( anim_name ) )
		end
	end
end

-----------------------------------------------------------------------------------

function NewRaycastWeaponBase:_set_parts_enabled( enabled )
	if self._parts then
		for part_id,data in pairs( self._parts ) do
			if alive( data.unit ) then
				data.unit:set_enabled( enabled )
			end
		end
	end
end

function NewRaycastWeaponBase:on_enabled( ... )
	NewRaycastWeaponBase.super.on_enabled( self, ... )
	self:_set_parts_enabled( true )
end

function NewRaycastWeaponBase:on_disabled( ... )
	-- NewRaycastWeaponBase.super.on_disabled( self, ... )
	self:gadget_off()
	
	self:_set_parts_enabled( false )
end

-----------------------------------------------------------------------------------

function NewRaycastWeaponBase:has_gadget()
	return managers.weapon_factory:get_part_from_weapon_by_type( "gadget", self._parts ) and true or false	
end

function NewRaycastWeaponBase:gadget_on()
	self._gadget_on = true
	local gadget = managers.weapon_factory:get_part_from_weapon_by_type( "gadget", self._parts )
	if gadget then
		gadget.unit:base():set_state( self._gadget_on )
	end
end

function NewRaycastWeaponBase:gadget_off()
	self._gadget_on = false
	local gadget = managers.weapon_factory:get_part_from_weapon_by_type( "gadget", self._parts )
	if gadget then
		gadget.unit:base():set_state( self._gadget_on )
	end
end

function NewRaycastWeaponBase:toggle_gadget()
	self._gadget_on = not self._gadget_on
	local gadget = managers.weapon_factory:get_part_from_weapon_by_type( "gadget", self._parts )
	if gadget then
		gadget.unit:base():set_state( self._gadget_on )
	end
end

----------------------------------------------------

function NewRaycastWeaponBase:check_stats()
	local base_stats = self:weapon_tweak_data().stats
	if not base_stats then
		print( "no stats" )
		return
	end 
	
	-- print( inspect( base_stats ) )
	local parts_stats = managers.weapon_factory:get_stats( self._factory_id, self._blueprint )
	-- print( inspect( parts_stats ) )
	
	local stats = deep_clone( base_stats )
	local tweak_data = tweak_data.weapon.stats
	
	stats.zoom = math.min( stats.zoom + managers.player:upgrade_value( self:weapon_tweak_data()[ "category" ], "zoom_increase", 0 ), #tweak_data.zoom )		-- not that base + skill increase will ever go off the chart (table), but still
	
	for stat,_ in pairs( stats ) do
		if parts_stats[ stat ] then
			stats[ stat ] = math_clamp( stats[ stat ] + parts_stats[ stat ], 1, #tweak_data[ stat ] )
		end
	end
	
	self._current_stats = {}
	for stat, i in pairs( stats ) do
		-- print( "stat", stat, inspect( tweak_data.weapon.stats ) )
		-- print( tweak_data.weapon.stats[ stat ] )
		-- print( stat, tweak_data[ stat ][ i ] )
		self._current_stats[ stat ] = tweak_data[ stat ][ i ]
	end
	self._current_stats.alert_size = tweak_data.alert_size[ math_clamp( stats.suppression, 1, #tweak_data.alert_size ) ]
	
		
	return stats
end

----------------------------------------------------

function NewRaycastWeaponBase:spread_multiplier()
	local multiplier = NewRaycastWeaponBase.super.spread_multiplier( self )
	if( self._silencer ) then
		multiplier = multiplier * managers.player:upgrade_value( "weapon", "silencer_spread_multiplier", 1 )
		multiplier = multiplier * managers.player:upgrade_value( self:weapon_tweak_data()[ "category" ], "silencer_spread_multiplier", 1 )
	end
	
	return multiplier
end

function NewRaycastWeaponBase:recoil_multiplier()
	local multiplier = NewRaycastWeaponBase.super.recoil_multiplier( self )
	multiplier = multiplier * managers.player:upgrade_value( "weapon", "passive_recoil_multiplier", 1 )
	if( self._silencer ) then
		multiplier = multiplier * managers.player:upgrade_value( "weapon", "silencer_recoil_multiplier", 1 )
		multiplier = multiplier * managers.player:upgrade_value( self:weapon_tweak_data()[ "category" ], "silencer_recoil_multiplier", 1 )
	end
	
	return multiplier
end

function NewRaycastWeaponBase:enter_steelsight_speed_multiplier()
	local multiplier = NewRaycastWeaponBase.super.enter_steelsight_speed_multiplier( self )
	if( self._silencer ) then
		multiplier = multiplier * managers.player:upgrade_value( "weapon", "silencer_enter_steelsight_speed_multiplier", 1 )
		multiplier = multiplier * managers.player:upgrade_value( self:weapon_tweak_data()[ "category" ], "silencer_enter_steelsight_speed_multiplier", 1 )
	end
	
	return multiplier
end

--[[function NewRaycastWeaponBase:toggle_flashlight()
	self._flashlight_on = not self._flashlight_on
	self:check_flashlight_enabled()
end

function NewRaycastWeaponBase:check_flashlight_enabled()
	if self._flashlight_on then
		local gadget = managers.weapon_factory:get_part_from_weapon_by_type( "gadget", self._parts )
		local obj = gadget.unit:get_object( Idstring( "a_laser" ) )
			
		if not alive( self._light ) then
			self._light = World:create_light( "spot|specular" )
			self._light:set_spot_angle_end( 45 )
			self._light:set_far_range( 1000 )
		end
		
		self._light:set_spot_angle_end( 0.5 )
		self._light:set_far_range( 4000 )
		self._light:set_color( Vector3( 5, 0, 0 ) )
		
		self._light:link( obj )
		-- self._light:set_rotation( obj:rotation() )
		self._light:set_rotation( Rotation( obj:rotation():z(), -obj:rotation():x(), -obj:rotation():y() ) )
		
		if not self._light_effect then
			self._light_effect = World:effect_manager():spawn( { effect = Idstring( "effects/particles/weapons/flashlight/fp_flashlight" ), parent = obj } )
		end
		self._light:set_enable( true )
		World:effect_manager():set_hidden( self._light_effect, true )
	elseif alive( self._light ) then
		self._light:set_enable( false )
		World:effect_manager():set_hidden( self._light_effect, true )
	end
end]]

-----------------------------------------------------------------------------------

function NewRaycastWeaponBase:destroy( unit )
	-- NewRaycastWeaponBase.super.destroy( self, unit )
	
	--[[if self._light then
		World:delete_light( self._light )
	end
	if self._light_effect then
		World:effect_manager():kill( self._light_effect )
		self._light_effect = nil
	end]]
	
	managers.weapon_factory:disassemble( self._parts )
end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------
