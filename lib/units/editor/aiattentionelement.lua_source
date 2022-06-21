AIAttentionElement = AIAttentionElement or class( MissionElement )

function AIAttentionElement:init( unit )
	AIAttentionElement.super.init( self, unit )
	
	self._nav_link_filter = {}
	self._nav_link_filter_check_boxes = {}
	
	self._hed.preset = "none"
	--self._hed.ai_group = "enemies"
	self._hed.local_pos = nil
	self._hed.local_rot = nil
	self._hed.use_instigator = nil
	self._hed.instigator_ids = nil
	self._hed.parent_u_id = nil
	self._hed.parent_obj_name = nil
	self._hed.att_obj_u_id = nil
	self._hed.operation = "set"
	self._hed.override = "none"
	
	table.insert( self._save_values, "preset" )
	--table.insert( self._save_values, "ai_group" )
	table.insert( self._save_values, "local_pos" )
	--table.insert( self._save_values, "local_rot" )
	table.insert( self._save_values, "use_instigator" )
	table.insert( self._save_values, "instigator_ids" )
	table.insert( self._save_values, "parent_u_id" )
	table.insert( self._save_values, "parent_obj_name" )
	table.insert( self._save_values, "att_obj_u_id" )
	table.insert( self._save_values, "operation" )
	table.insert( self._save_values, "override" )
	
	self._parent_unit = nil
	self._parent_obj = nil
	self._att_obj_unit = nil
end

-----------------------------------------------------------------------------

function AIAttentionElement:post_init()
end

-----------------------------------------------------------------------------

-- When the layer is finished, get the unit this element is linked to.
function AIAttentionElement:layer_finished()
	AIAttentionElement.super.layer_finished( self )
	
	if self._hed.parent_u_id then
		self._parent_unit = managers.worlddefinition:get_unit_on_load( self._hed.parent_u_id, callback( self, self, "load_parent_unit" ) )
		if self._parent_unit then
			self._parent_obj = self._parent_unit:get_object( Idstring( self._hed.parent_obj_name ) )
		end
	end
	if self._hed.att_obj_u_id then
		self._att_obj_unit = managers.worlddefinition:get_unit_on_load( self._hed.att_obj_u_id, callback( self, self, "load_att_obj_unit" ) )
	end
	--print( "[AIAttentionElement:layer_finished] self._parent_unit", self._parent_unit, "self._att_obj_unit", self._att_obj_unit )
end

-----------------------------------------------------------------------------

function AIAttentionElement:load_parent_unit( unit )
	--print( "[AIAttentionElement:load_parent_unit]", unit )
	self._parent_unit = unit
	if self._parent_unit then
		self._parent_obj = self._parent_unit:get_object( Idstring( self._hed.parent_obj_name ) )
	end
end

-----------------------------------------------------------------------------

function AIAttentionElement:load_att_obj_unit( unit )
	--print( "[AIAttentionElement:load_att_obj_unit]", unit )
	self._att_obj_unit = unit
end

-----------------------------------------------------------------------------

function AIAttentionElement:draw_links( t, dt, selected_unit, all_units )
	AIAttentionElement.super.draw_links( self, t, dt, selected_unit )
	
	if not ( not selected_unit or self._parent_unit == selected_unit or self._parent_unit == selected_unit or self._unit == selected_unit ) then
		return
	end
	
	if self._parent_unit then
		self:_draw_link( { from_unit = self._unit, to_unit = self._parent_unit,  r = 0, g = 0.75, b = 0 } )
	end
	if self._att_obj_unit then
		self:_draw_link( { from_unit = self._unit, to_unit = self._att_obj_unit,  r = 0, g = 0, b = 0.75 } )
	end
	
	if self._hed.instigator_ids then
		for _, id in ipairs( self._hed.instigator_ids ) do
			local unit = all_units[ id ]
			self:_draw_link( { from_unit = unit, to_unit = self._unit, r = 0, g = 0, b = 0.75 } )
		end
	end
end

-----------------------------------------------------------------------------

function AIAttentionElement:update_selected( t, dt, selected_unit, all_units )
	
	self:_chk_units_alive()
	
	if not ( not selected_unit or self._parent_unit == selected_unit or self._att_obj_unit == selected_unit or self._unit == selected_unit ) then
		return
	end
	
	if self._parent_unit then
		self:_draw_link( { from_unit = self._unit, to_unit = self._parent_unit, r = 0, g = 0.75, b = 0 } )
	end
	
	if self._att_obj_unit then
		self:_draw_link( { from_unit = self._unit, to_unit = self._att_obj_unit, r = 0.75, g = 0, b = 0 } )
	end
	
	if self._hed.instigator_ids then
		for _, id in ipairs( self._hed.instigator_ids ) do
			local unit = all_units[ id ]
			self:_draw_link( { from_unit = unit, to_unit = self._unit, r = 0, g = 0, b = 0.75 } )
		end
	end
end

-----------------------------------------------------------------------------

function AIAttentionElement:update_unselected( t, dt, selected_unit, all_units )
	self:_chk_units_alive()
end

-----------------------------------------------------------------------------

function AIAttentionElement:_chk_units_alive()
	if self._parent_unit and not alive( self._parent_unit ) then
		self._parent_unit = nil
		self._parent_obj = nil
		self._hed.parent_obj_name = nil
		self._hed.parent_u_id = nil
		self:_chk_set_link_values()
	end
	if self._att_obj_unit and not alive( self._att_obj_unit ) then
		self._att_obj_unit = nil
		self._hed.att_obj_u_id = nil
		self:_chk_set_link_values()
	end
end

-----------------------------------------------------------------------------

function AIAttentionElement:update_editing()
	self:_find_parent_raycast()
	self:_find_att_obj_raycast()
	self:_find_instigator_raycast()
	self:_raycast()
end

-----------------------------------------------------------------------------

function AIAttentionElement:_find_parent_raycast()
	local from = managers.editor:get_cursor_look_point(0)
	local to = managers.editor:get_cursor_look_point(100000)
	local ray = World:raycast( from, to, nil, managers.slot:get_mask( "all" ) )
	if not ray then
		return
	end
	
	if ray.unit:id() == -1 then
		return
	end
	
	Application:draw( ray.unit, 0, 1, 0 )
	return ray
end

-----------------------------------------------------------------------------

function AIAttentionElement:_find_att_obj_raycast()
	local ray = managers.editor:unit_by_raycast( { mask = 38 } )
	if not ( ray and ray.unit ) then
		return
	end
	
	Application:draw( ray.unit, 0, 0, 1 )
	
	return ray.unit
end

-----------------------------------------------------------------------------

function AIAttentionElement:_find_instigator_raycast()
	local ray = managers.editor:unit_by_raycast( { mask = 10, ray_type = "editor" } )
	--print( "[AIAttentionElement:_find_instigator_raycast]", ray and ray.unit )
	if not ( ray and ray.unit ) then
		return
	end
	
	local id
	if string.find( ray.unit:name():s(), "ai_enemy_group", 1 ,true ) or
		string.find( ray.unit:name():s(), "ai_spawn_enemy", 1 ,true ) or
		string.find( ray.unit:name():s(), "ai_civilian_group", 1 ,true ) or
		string.find( ray.unit:name():s(), "ai_spawn_civilian", 1 ,true ) then
		id = ray.unit:unit_data().unit_id
		Application:draw( ray.unit, 0, 0, 1 )
	end
	
	return id
end

-----------------------------------------------------------------------------

function AIAttentionElement:_raycast()
	local from = managers.editor:get_cursor_look_point(0)
	local to = managers.editor:get_cursor_look_point(100000)
	local ray = World:raycast( from, to, nil, managers.slot:get_mask( "all" ) )
	if ray and ray.position then
		Application:draw_sphere( ray.position, 10, 1, 1, 1 )
		return ray.position
	end
	return nil
end

-----------------------------------------------------------------------------

function AIAttentionElement:_lmb()
	
	local unit = self:_find_att_obj_raycast()
	if unit then
		if self._att_obj_unit == unit then
			self._att_obj_unit = nil
			self._hed.att_obj_u_id = nil
		else
			self._att_obj_unit = unit
			self._hed.att_obj_u_id = unit:unit_data().unit_id
		end
		self:_chk_set_link_values()
		return
	end
	
	local id = SpecialObjectiveUnitElement._spawn_raycast( self )
	if id then
		if self._hed.instigator_ids then
			for i,si_id in ipairs( self._hed.instigator_ids ) do
				if si_id == id then
					table.remove( self._hed.instigator_ids, i )
					if not next( self._hed.instigator_ids ) then
						self._hed.instigator_ids = nil
					end
					return
				end
			end
		end
		
		self._hed.instigator_ids = self._hed.instigator_ids or {}
		table.insert( self._hed.instigator_ids, id )
		return
	end
	
	local ray = self:_find_parent_raycast()
	if ray then
		if self._parent_unit == ray.unit then
			self._parent_unit = nil
			self._parent_obj = nil
			self._hed.parent_obj_name = nil
			self._hed.parent_u_id = nil
		else
			self._parent_unit = ray.unit
			self._parent_obj = ray.body:root_object()
			self._hed.parent_obj_name = self._parent_obj:name():s()
			self._hed.parent_u_id = ray.unit:unit_data().unit_id
		end
		self:_chk_set_link_values()
		return
	end
	
end

-----------------------------------------------------------------------------

function AIAttentionElement:add_triggers( vc )
	vc:add_trigger( Idstring( "lmb" ), callback( self, self, "_lmb" ) )
end

-----------------------------------------------------------------------------

-- The values in the combobox differs between layers and can be changed from an other layer therefor it needs
-- to be updated.
function AIAttentionElement:selected()
	AIAttentionElement.super.selected( self )
	self:_chk_units_alive()
	self:_chk_set_link_values()
end

-----------------------------------------------------------------------------

function AIAttentionElement:_build_panel( panel, panel_sizer )
	self:_create_panel()
		
	panel = panel or self._panel
	panel_sizer = panel_sizer or self._panel_sizer	
	
	local use_instigator = EWS:CheckBox( panel, "Use instigator", "" )
	use_instigator:set_value( self._hed.use_instigator )
	use_instigator:connect( "EVT_COMMAND_CHECKBOX_CLICKED", callback( self, self, "set_element_data" ), { ctrlr = use_instigator, value = "use_instigator" } )
	panel_sizer:add( use_instigator, 0, 0, "EXPAND" )
	
	-- preset
	local preset_sizer = EWS:BoxSizer( "HORIZONTAL" )
	panel_sizer:add( preset_sizer, 0, 1, "EXPAND,LEFT" )
	
	local preset_names = clone( tweak_data.attention.indexes )
	
	local preset_params = {
		name 				= "Preset:",
		panel 				= panel,
		sizer 				= preset_sizer,
		options				= preset_names,
		value 				= self._hed.preset,
		default				= "none", -- preset_names[1] or "error",
		tooltip 			= "Select the attention preset.",
		name_proportions 	= 1,
		ctrlr_proportions 	= 2,
		sizer_proportions	= 1,
		sorted				= true
	}
	local preset = CoreEws.combobox( preset_params )
	
	preset:connect( "EVT_COMMAND_COMBOBOX_SELECTED", callback( self, self, "set_element_data" ), { ctrlr = preset, value = "preset" } )
	
	-- operation
	local preset_sizer = EWS:BoxSizer( "HORIZONTAL" )
	panel_sizer:add( preset_sizer, 0, 1, "EXPAND,LEFT" )
	
	local operation_params = {
		name 				= "Operation:",
		panel 				= panel,
		sizer 				= preset_sizer,
		options				= { "set", "add", "override" },
		value 				= self._hed.operation,
		--default				= "none", -- preset_names[1] or "error",
		tooltip 			= "Select the attention preset.",
		name_proportions 	= 1,
		ctrlr_proportions 	= 2,
		sizer_proportions	= 1,
		sorted				= true
	}
	local operation = CoreEws.combobox( operation_params )
	
	operation:connect( "EVT_COMMAND_COMBOBOX_SELECTED", callback( self, self, "set_element_data" ), { ctrlr = operation, value = "operation" } )
	
	
	-- override
	local override_sizer = EWS:BoxSizer( "HORIZONTAL" )
	panel_sizer:add( override_sizer, 0, 1, "EXPAND,LEFT" )
	
	local override_params = {
		name 				= "Override:",
		panel 				= panel,
		sizer 				= override_sizer,
		options				= preset_names,
		value 				= self._hed.override,
		default				= "none", -- preset_names[1] or "error",
		tooltip 			= "Select the attention preset to be overriden. (valid only with override operation)",
		name_proportions 	= 1,
		ctrlr_proportions 	= 2,
		sizer_proportions	= 1,
		sorted				= true
	}
	local override = CoreEws.combobox( override_params )
	
	override:connect( "EVT_COMMAND_COMBOBOX_SELECTED", callback( self, self, "set_element_data" ), { ctrlr = override, value = "override" } )
end

-----------------------------------------------------------------------------

function AIAttentionElement:add_to_mission_package()
	-- Wait with this until they are no longer loaded in game_base.package and uses different weapons then player (unit,model,etc)
	-- managers.editor:add_to_world_package( { category = "units", name = self._hed.enemy, continent = self._unit:unit_data().continent } )
end

-----------------------------------------------------------------------------
-- calculates and saves the attention unit's position (and rotation) in the local space of the parent object
function AIAttentionElement:_chk_set_link_values()
	if self._att_obj_unit and self._parent_unit then
		local att_obj_pos = self._att_obj_unit:position()
		local att_obj_rot = self._att_obj_unit:rotation()
		local parent_pos = self._parent_obj:position()
		local parent_rot = self._parent_obj:rotation()
		--print( "att_obj_pos", att_obj_pos, "att_obj_rot", att_obj_rot, "parent_pos", parent_pos, "parent_rot", parent_rot )
		
		local parent_inv_rot = parent_rot:inverse()
		
		local world_vec = att_obj_pos - parent_pos
		
		self._hed.local_pos = world_vec:rotate_with( parent_inv_rot )
		--self._hed.local_rot = att_obj_rot * parent_inv_rot
		--print( "local_pos", self._hed.local_pos, "local_rot", self._hed.local_rot )
	else
		self._hed.local_pos = nil
		self._hed.local_rot = nil
	end
end