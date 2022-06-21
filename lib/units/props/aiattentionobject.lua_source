AIAttentionObject = AIAttentionObject or class()

AIAttentionObject.REACT_IDLE = 1 -- glance when idling
AIAttentionObject.REACT_CURIOUS = 2 -- inspect, stare persistently
AIAttentionObject.REACT_SUSPICIOUS = 3 -- as in curious but if "suspicion_range" parameter is present, the observer may see through the disquise of the attention object
AIAttentionObject.REACT_SURPRISED = 4 -- stare and show amazement
AIAttentionObject.REACT_SCARED = 5 -- stare and show fright
AIAttentionObject.REACT_AIM = 6 -- aim at the object
AIAttentionObject.REACT_ARREST = 7 -- arrest logic ( aim, say "freeze!" and call reenforcements )
AIAttentionObject.REACT_DISARM = 8 -- walk towards the object and put handcuffs on it
AIAttentionObject.REACT_SHOOT = 9 -- shoot at the object until it's dead
AIAttentionObject.REACT_MELEE = 10 -- hit the object in melee
AIAttentionObject.REACT_COMBAT = 11 -- engage in combat behaviour treating the object as a threat
AIAttentionObject.REACT_SPECIAL_ATTACK = 12 -- tase, spooc

AIAttentionObject.REACT_MIN = AIAttentionObject.REACT_IDLE
AIAttentionObject.REACT_MAX = AIAttentionObject.REACT_SPECIAL_ATTACK

function AIAttentionObject:init( unit, is_not_extension )
	self._unit = unit
	self._attention_data = nil
	
	self._listener_holder = ListenerHolder:new()
	
	self:setup_attention_positions( nil, nil ) -- set default attention positions
	
	self._is_extension = not is_not_extension
	if self._is_extension then
		self:set_update_enabled( true )
		if Network:is_client() and unit:unit_data().only_visible_in_editor then
			unit:set_visible( false )
		end
		
		if self._initial_settings then
			local preset_list = string.split( self._initial_settings, " " )
			for _, preset_name in ipairs( preset_list ) do
				local attention_desc = tweak_data.attention.settings[ preset_name ]
				local att_setting = PlayerMovement._create_attention_setting_from_descriptor( self, attention_desc, preset_name )
				self:add_attention( att_setting )
			end
		end
	end
end

--------------------------------------------------------------------------------------

function AIAttentionObject:update( unit, t, dt )
	self._attention_obj:m_position( self._observer_info.m_pos )
	--Application:draw_sphere( self._observer_info.m_pos, 30, 1, 0, 0 )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:set_update_enabled( state )
	self._unit:set_extension_update_enabled( Idstring( "attention" ), state )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:set_detection_object_name( obj_name )
	self._attention_obj_name = obj_name
	self:setup_attention_positions()
end

--------------------------------------------------------------------------------------

function AIAttentionObject:setup_attention_positions()
	if self._attention_obj_name then
		self._attention_obj = self._unit:get_object( Idstring( self._attention_obj_name ) )
	else
		self._attention_obj = self._unit:orientation_object()
	end
	
	self._observer_info = {
		m_pos = self._attention_obj:position()
	}
end

--------------------------------------------------------------------------------------

function AIAttentionObject:attention_data()
	return self._attention_data
end

--------------------------------------------------------------------------------------

function AIAttentionObject:unit()
	return self._unit
end

--------------------------------------------------------------------------------------

function AIAttentionObject:add_attention( settings )
	--print( "[AIAttentionObject:add_attention]", self._unit, inspect( settings ) )
	local needs_register
	if not self._attention_data then
		self._attention_data = {}
		needs_register = true
	end
	
	if self._overrides and self._overrides[ settings.id ] then
		self._override_restore = self._override_restore or {}
		self._override_restore[ settings.id ] = settings
		
		self._attention_data[ settings.id ] = self._overrides[ settings.id ]
	else
		self._attention_data[ settings.id ] = settings
	end
	
	if needs_register then
		self:_register()
	end
	
	self:_call_listeners()
end

--------------------------------------------------------------------------------------

function AIAttentionObject:remove_attention( id )
	--print( "[AIAttentionObject:remove_attention]", self._unit, id )
	if not self._attention_data then
		return
	end
	
	if self._override_restore and self._override_restore[ id ] then
		self._override_restore[ id ] = nil
		if not next( self._override_restore ) then
			self._override_restore = nil
		end
	end
	
	if self._attention_data[ id ] then
		self._attention_data[ id ] = nil
		if not next( self._attention_data ) then
			managers.groupai:state():unregister_AI_attention_object( ( self._parent_unit or self._unit ):key() )
		end
		self:_call_listeners()
	end
end

--------------------------------------------------------------------------------------

function AIAttentionObject:set_attention( settings, id )
	--print( "[AIAttentionObject:set_attention]", self._unit, inspect( settings ) )
	
	if self._override_restore then
		for att_id, att_setting in pairs( self._attention_data ) do
			if att_id ~= id and self._override_restore[ att_id ] then
				self._override_restore[ att_id ] = nil
			end
		end
		if not next( self._override_restore ) then
			self._override_restore = nil
		end
	end
	
	if self._attention_data then
		if settings then
			local override_setting = self._overrides and self._overrides[ id or settings.id ]
			self._attention_data = { [ id or settings.id ] = override_setting or settings }
		else
			self._attention_data = nil
			managers.groupai:state():unregister_AI_attention_object( ( self._parent_unit or self._unit ):key() )
		end
		self:_call_listeners()
	else
		if settings then
			self._attention_data = {}
			self._attention_data = { [ id or settings.id ] = settings }
			self:_register()
			self:_call_listeners()
		end
	end
end

--------------------------------------------------------------------------------------

function AIAttentionObject:override_attention( original_preset_name, override_preset )
	--print( "[AIAttentionObject:override_attention]", original_preset_name, inspect( override_preset ) )
	if override_preset then
		
		local original_preset = self._override_restore and self._override_restore[ original_preset_name ] or self._attention_data and self._attention_data[ original_preset_name ]
		if original_preset then
			self._override_restore = self._override_restore or {}
			self._override_restore[ original_preset_name ] = original_preset
		end
		
		self._overrides = self._overrides or {}
		self._overrides[ original_preset_name ] = override_preset
		
		if self._attention_data and self._attention_data[ original_preset_name ] then
			self._attention_data[ original_preset_name ] = override_preset
			self:_call_listeners()
		end
	elseif self._overrides then
		local original_preset = self._override_restore and self._override_restore[ original_preset_name ]
		if original_preset then
			self._override_restore[ original_preset_name ] = nil
			if not next( self._override_restore ) then
				self._override_restore = nil
			end
		end
		
		self._overrides[ original_preset_name ] = nil
		if not next( self._overrides ) then
			self._overrides = nil
		end
		
		if original_preset then
			self:add_attention( original_preset )
		else
			self:remove_attention( original_preset_name )
		end
	end
end

--------------------------------------------------------------------------------------

function AIAttentionObject:get_attention( filter, min, max )
	--print( "[AIAttentionObject:get_attention] filter", inspect(managers.navigation:convert_access_filter_to_table( filter )), "min", min, "max", max )
	if not self._attention_data then
		return
	end
	
	min = min or AIAttentionObject.REACT_MIN
	max = max or AIAttentionObject.REACT_MAX

	local nav_manager = managers.navigation
	local access_f = nav_manager.check_access
	
	local settings_match
	for id, settings in pairs( self._attention_data ) do
		--print( "testing", inspect( settings ) )
		if settings.reaction >= min and settings.reaction <= max and ( not settings_match or settings.reaction > settings_match.reaction ) and access_f( nav_manager, settings.filter, filter, 0 ) then
			--print( "good" )
			settings_match = settings
		end
	end
	--Application:stack_dump()
	return settings_match
end

--------------------------------------------------------------------------------------
-- Check if this attention setting is still the most suitable
function AIAttentionObject:verify_attention( test_settings, min, max )
	if not self._attention_data then
		return
	end
	
	local new_settings = self:get_attention( filter, min, max )
	return new_settings == test_settings
end

--------------------------------------------------------------------------------------

function AIAttentionObject:get_attention_m_pos( settings )
	return self._observer_info.m_pos
end

--------------------------------------------------------------------------------------

function AIAttentionObject:get_detection_m_pos()
	return self._observer_info.m_pos
end

--------------------------------------------------------------------------------------

function AIAttentionObject:get_ground_m_pos()
	return self._observer_info.m_pos
end

--------------------------------------------------------------------------------------

function AIAttentionObject:add_listener( key, clbk )
	self._listener_holder:add( key, clbk )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:remove_listener( key )
	self._listener_holder:remove( key )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:_call_listeners()
	local u_key = ( self._parent_unit or self._unit ):key()
	managers.groupai:state():on_AI_attention_changed( u_key )
	self._listener_holder:call( u_key )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:_register()
	managers.groupai:state():register_AI_attention_object( self._parent_unit or self._unit, self, nil )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:link( parent_unit, obj_name, local_pos )
	self._unit:unlink()
	
	if parent_unit then
		self._parent_unit = parent_unit
		self._parent_obj_name = obj_name
		self._local_pos = local_pos
		self._parent_unit_key = parent_unit:key()
		
		self._unit:body(0):set_enabled( false )
		self._unit:set_moving()
		
		--print( "[AIAttentionObject:link] parent_unit", parent_unit, "obj_name", obj_name, "local_pos", local_pos )
		local parent_obj = parent_unit:get_object( Idstring( obj_name ) )
		local parent_pos = parent_obj:position()
		local parent_rot = parent_obj:rotation()
		--print( "[AIAttentionObject:link] parent_pos", parent_pos, "parent_rot", parent_rot )
		local att_obj_w_pos = local_pos:rotate_with( parent_rot ) + parent_pos
		--print( "att_obj_w_pos", att_obj_w_pos )
		--local att_obj_w_rot = self._values.local_rot * parent_rot
		--print( "att_obj_w_rot", att_obj_w_rot )
		
		parent_unit:link( Idstring( obj_name ), self._unit )
		
		self._unit:set_position( att_obj_w_pos ) -- the child unit has to be positioned after linking instead of before for some reason
		--unit:set_rotation( att_obj_w_rot )
		
		if Network:is_server() then
			if self._parent_unit:id() == -1 then
				debug_pause_unit( self._parent_unit, "[AIAttentionObject:set_parent_unit] attention object parent is not network synched", self._parent_unit )
			end
			managers.network:session():send_to_peers_synched( "link_attention_no_rot", self._parent_unit, self._unit, obj_name, local_pos )
		end
		
		self:set_update_enabled( true )
	else
		self._parent_unit = nil
		self._parent_obj_name = nil
		self._local_pos = nil
		self._parent_unit_key = nil
		if Network:is_server() then
			managers.network:session():send_to_peers_synched( "unlink_attention", self._unit )
		end
		self:set_update_enabled( false )
	end
end

--------------------------------------------------------------------------------------

function AIAttentionObject:save( data )
	if alive( self._parent_unit ) then
		data.parent_u_id = self._parent_unit:unit_data().unit_id
		data.parent_obj_name = self._parent_obj_name
		data.local_pos = self._local_pos
	end
	--print( "[AIAttentionObject:save]", inspect( data ) )
end

--------------------------------------------------------------------------------------

function AIAttentionObject:load( data )
	--print( "[AIAttentionObject:load]", inspect( data ) )
	if not ( data and data.parent_u_id ) then
		return
	end
	
	local parent_unit
	if Application:editor() then
		parent_unit = managers.editor:unit_with_id( data.parent_u_id )
	else
		parent_unit = managers.worlddefinition:get_unit_on_load( data.parent_u_id, callback( self, self, "clbk_load_parent_unit" ) )
	end
	
	if parent_unit then
		self:link( parent_unit, data.parent_obj_name, data.local_pos )
	elseif not Application:editor() then
		self._load_data = data
	else
		debug_pause_unit( self._unit, "[AIAttentionObject:load] failed to link", self._unit )
	end
end

--------------------------------------------------------------------------------------

function AIAttentionObject:clbk_load_parent_unit( parent_unit )
	--print( "[AIAttentionObject:clbk_load_parent_unit]", parent_unit )
	if parent_unit then
		self:link( parent_unit, self._load_data.parent_obj_name, self._load_data.local_pos )
	end
	self._load_data = nil
end

--------------------------------------------------------------------------------------

function AIAttentionObject:destroy()
	self:set_attention( nil )
	
	local extensions = self._unit:extensions()
	local last_extension_name = extensions[ #extensions ]
	local last_extension = self._unit[ last_extension_name ]
	if self == last_extension then
		self._unit:base():pre_destroy( self._unit )
	end
end

--------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------