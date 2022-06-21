NavObstacleElement = NavObstacleElement or class( MissionElement )

function NavObstacleElement:init( unit )
	NavObstacleElement.super.init( self, unit )
	
	self._guis = {}
	self._obstacle_units = {}
	
	self._hed.obstacle_list = {}  -- { unit_id = unit_id, obj_name = obj_name }
	
	--self._obstacle_unit = nil
	--self._obstacle_obj_name_indented = nil
	self._all_object_names = {}
	
	--self._hed.obstacle_unit_id = nil
	--self._hed.obstacle_obj_name = nil
	self._hed.operation = "add" -- "add" or "remove"
	
	-- table.insert( self._save_values, "obstacle_unit_id" )
	-- table.insert( self._save_values, "obstacle_obj_name" )
	table.insert( self._save_values, "obstacle_list" )
	table.insert( self._save_values, "operation" )
end

-- When the layer is finished, get the units this element is linked to.
function NavObstacleElement:layer_finished()
	MissionElement.layer_finished( self )
	
	if self._hed.obstacle_unit_id then -- CONVERT
		table.insert( self._hed.obstacle_list, { unit_id = self._hed.obstacle_unit_id, obj_name =  self._hed.obstacle_obj_name } )
	end
	
	for _,data in pairs( self._hed.obstacle_list ) do
		local unit = managers.worlddefinition:get_unit_on_load( data.unit_id, callback( self, self, "load_unit" ) )
		if unit then
			self._obstacle_units[ unit:unit_data().unit_id ] = unit
		end
	end
	
	
	
	
	--[[if not self._hed.obstacle_unit_id then
		return
	end
	self._obstacle_unit = managers.worlddefinition:get_unit_on_load( self._hed.obstacle_unit_id, callback( self, self, "load_unit" ) )
	if self._obstacle_unit and self._hed.obstacle_obj_name then
		local obj = self._obstacle_unit:get_object( self._hed.obstacle_obj_name )
		if obj then
			self._obstacle_obj_name_indented = self._get_indented_obj_name( obj )
		else
			self._hed.obstacle_unit_id = nil
		end
	end
	
	self:_on_unit_set()]]
end

function NavObstacleElement:load_unit( unit )
	if unit then
		self._obstacle_units[ unit:unit_data().unit_id ] = unit
	end
	
	
	

	--[[if unit then
		self._obstacle_unit = unit
		if self._obstacle_unit and self._hed.obstacle_obj_name then
			local obj = self._obstacle_unit:get_object( self._hed.obstacle_obj_name )
			if obj then
				self._obstacle_obj_name_indented = self._get_indented_obj_name( obj )
			else
				self._hed.obstacle_unit_id = nil
			end
		end
		
		self:_on_unit_set()
	end]]
end

function NavObstacleElement:update_selected()
	self:_check_alive_units_and_draw( "selected" )
	
	--[[if self._obstacle_unit then
		if not alive( self._obstacle_unit ) then
			self._hed.obstacle_unit_id = nil
			self._obstacle_unit = nil
			
			self:_on_unit_set()
			return
		end
	else
		return
	end
	
	local params = {
		from_unit = self._unit,
		to_unit = self._obstacle_unit,
		r = 1,
		g = 0,
		b = 0
	}
	self:_draw_link( params )
	Application:draw( self._obstacle_unit, 0.75, 0, 0 )]]
	-- self:_draw_object( 1, 0, 0 )
end

function NavObstacleElement:update_unselected( t, dt, selected_unit, all_units )
	self:_check_alive_units_and_draw( "unselected" )
	
	--[[if self._obstacle_unit and not alive( self._obstacle_unit ) then
		self._hed.obstacle_unit_id = nil
		self._obstacle_unit = nil
		
		self:_on_unit_set()
		return
	end]]
end

function NavObstacleElement:_check_alive_units_and_draw( type )
	local r = type == "selected" and 1 or 0.5
	local g = 0
	local b = 0 -- type == "selected" and 1 or 0.5
	for id,unit in pairs( self._obstacle_units ) do
		if not alive( unit ) then
			self:_remove_by_unit_id( id )
			self._obstacle_units[ id ] = nil
		else
			local params = {
				from_unit = self._unit,
				to_unit = unit,
				r = r,
				g = g,
				b = b
			}
			self:_draw_link( params )
			Application:draw( unit, r/2, g/2, b/2 )
			
			for _,data in pairs( self._hed.obstacle_list ) do
				if data.unit_id == id then
					local obj = unit:get_object( data.obj_name )
					if obj then
						Application:draw( obj, r, g, b )
					end
				end
			end		
		end
	end
end

function NavObstacleElement:draw_links_unselected( ... )
	NavObstacleElement.super.draw_links_unselected( self, ... )
	
	--[[if self._obstacle_unit then
		if not alive( self._obstacle_unit ) then
			self._hed.obstacle_unit_id = nil
			self._obstacle_unit = nil
			
			self:_on_unit_set()
			return
		end
	else
		return
	end
	
	local params = {
		from_unit = self._unit,
		to_unit = self._obstacle_unit,
		r = 0.5,
		g = 0,
		b = 0
	}
	self:_draw_link( params )
	Application:draw( self._obstacle_unit, 0.25, 0, 0 )]]
	-- self:_draw_object( 0.5, 0, 0 )
end

--[[function NavObstacleElement:_draw_object( r, g, b )
	if self._hed.obstacle_obj_name then
		local obj = self._obstacle_unit:get_object( self._hed.obstacle_obj_name )
		if obj then
			Application:draw( obj, r, g, b )
		end
	end
end]]

function NavObstacleElement:update_editing()
	local ray = managers.editor:unit_by_raycast( { sample = true, mask = managers.slot:get_mask( "all" ), ray_type = "body editor" } )
	if ray and ray.unit then
		Application:draw( ray.unit, 0, 1, 0 )
	end
end

function NavObstacleElement:select_unit()
	local ray = managers.editor:unit_by_raycast( { sample = true, mask = managers.slot:get_mask( "all" ), ray_type = "body editor" } )
	
	--[[if ray and ray.unit then
		local unit = ray.unit
		
		if unit == self._obstacle_unit then
			self._obstacle_unit = nil
			self._hed.obstacle_unit_id = nil
			
			self:_on_unit_set()
		else
			self._obstacle_unit = unit
			self._hed.obstacle_unit_id = unit:unit_data().unit_id
			self._hed.obstacle_obj_name = unit:orientation_object():name()
			
			self:_on_unit_set()
		end
	end]]
	
	if ray and ray.unit then
		local unit = ray.unit
		
		local all_object_names = self:_get_objects_by_unit( unit )
		
		self._obstacle_units[ unit:unit_data().unit_id ] = unit
		local obstacle_list_data = { unit_id = unit:unit_data().unit_id, obj_name = Idstring( self._unindent_obj_name( all_object_names[1] ) ) }
		table.insert( self._hed.obstacle_list, obstacle_list_data )
			
		self:_add_unit( unit, all_object_names, obstacle_list_data )
	end
end

function NavObstacleElement:_remove_by_unit_id( unit_id )
	local remove_entries = {}
	for id,entry in pairs( self._guis ) do
		if entry.unit_id == unit_id then
			table.insert( remove_entries, id )
		end
	end
	for _,id in ipairs( remove_entries ) do
		self:remove_entry( id )
	end
	
	for i,data in ipairs( clone( self._hed.obstacle_list ) ) do
		if data.unit_id == unit_id then
			table.remove( self._hed.obstacle_list, i )
		end 
	end
	
	self:_remove_from_obstacle_list( unit_id )
end

function NavObstacleElement:remove_entry( id )
	local unit_id = self._guis[ id ].unit_id 
	self._guis[ id ].unit_id_ctrlr:destroy()
	self._guis[ id ].obj_names:destroy()
	self._guis[ id ].name_ctrlr:destroy()
	self._guis[ id ].toolbar:destroy()
	self._guis[ id ] = nil
	
	self._panel:layout()
		
	for i,entry in pairs( clone( self._hed.obstacle_list ) ) do
		if entry.guis_id == id then
			table.remove( self._hed.obstacle_list, i )	
		end
	end
	
	for _,guis in pairs( self._guis ) do
		if guis.unit_id == unit_id then
			return
		end
	end
	
	self._obstacle_units[ unit_id ] = nil
end

function NavObstacleElement:_remove_from_obstacle_list( unit_id )
	for i,entry in pairs( clone( self._hed.obstacle_list ) ) do
		if entry.unit_id == unit_id then
			table.insert( self._hed.obstacle_list, i )
		end
	end
end

function NavObstacleElement:_add_unit( unit, all_object_names, obstacle_list_data )
	local panel = self._panel
	local panel_sizer = self._panel_sizer
		
	local unit_id = EWS:StaticText( panel, ""..unit:unit_data().name_id, 0, "")
	panel_sizer:add( unit_id, 0, 0, "EXPAND" )
	
	local h_sizer = EWS:BoxSizer( "HORIZONTAL" )
	panel_sizer:add( h_sizer, 0, 1, "EXPAND,LEFT" )
			
	-- Obj names
	local obj_names_params = {
		name 				= "Object:",
		panel 				= panel,
		sizer 				= h_sizer,
		options				= all_object_names,
		value 				= self._get_indented_obj_name( nil, unit, obstacle_list_data.obj_name ),
		tooltip 			= "Select an object from the combobox",
		name_proportions 	= 1,
		ctrlr_proportions 	= 2,
		sizer_proportions	= 1,
		sorted				= true
	}
	local obj_names = CoreEws.combobox( obj_names_params )
		
	self._guis_id = self._guis_id or 0
	self._guis_id = self._guis_id + 1
	
	obstacle_list_data.guis_id = self._guis_id 
	
	local toolbar = EWS:ToolBar( panel, "", "TB_FLAT,TB_NODIVIDER" )
		toolbar:add_tool( "SELECT", "Select dialog", CoreEws.image_path( "toolbar\\delete_16x16.png" ), nil )
		toolbar:connect( "SELECT", "EVT_COMMAND_MENU_SELECTED", callback( self, self, "remove_entry" ), self._guis_id )
		toolbar:realize() 
		
	self._guis[ self._guis_id ] = { unit_id_ctrlr = unit_id, unit = unit, unit_id = unit:unit_data().unit_id, obj_names = obj_names, name_ctrlr = obj_names_params.name_ctrlr, toolbar = toolbar, guis_id = self._guis_id }
	
	h_sizer:add( toolbar, 0, 1, "EXPAND,LEFT" )
		
	obj_names:connect( "EVT_COMMAND_COMBOBOX_SELECTED", callback( self, self, "set_obj_name_data" ), self._guis_id )
	
	panel:layout()
end

function NavObstacleElement:set_obj_name_data( guis_id )
	local obj_name = self._guis[ guis_id ].obj_names:get_value()
	for i,entry in pairs( self._hed.obstacle_list ) do
		if entry.guis_id == guis_id then
			entry.obj_name = Idstring( self._unindent_obj_name( obj_name ) ) -- obj_name
			break	
		end
	end
end

function NavObstacleElement:add_triggers( vc )
	vc:add_trigger( Idstring( "lmb" ), callback( self, self, "select_unit" ) )
end

function NavObstacleElement:_build_panel( panel, panel_sizer )
	--print( "[NavObstacleElement:_build_panel] self._obstacle_obj_name_indented", self._obstacle_obj_name_indented, "self._all_object_names", inspect( self._all_object_names ) )
	self:_create_panel()
	
	panel = panel or self._panel
	panel_sizer = panel_sizer or self._panel_sizer
	
	-- Operation
	local operation_params = {
		name 				= "Operation:",
		panel 				= panel,
		sizer 				= panel_sizer,
		options				= { "add", "remove" },
		value 				= self._hed.operation,
		tooltip 			= "Choose if you want to add or remove an obstacle.",
		name_proportions 	= 1,
		ctrlr_proportions 	= 2,
		sorted				= false
	}
	local operation = CoreEWS.combobox( operation_params )
	operation:connect( "EVT_COMMAND_COMBOBOX_SELECTED", callback( self, self, "set_element_data" ), { ctrlr = operation, value = "operation" } )
	
	-- object name
	--[[local obstacle_object_params = {
		name 				= "Object:",
		panel 				= panel,
		sizer 				= panel_sizer,
		options				= self._all_object_names,
		value 				= self._obstacle_obj_name_indented,
		tooltip 			= "Select an obstacle object (its children are automatically included).",
		name_proportions 	= 1,
		ctrlr_proportions 	= 2,
		sorted				= false
	}
	self._obstacle_object_combobox_params = obstacle_object_params
	self._obstacle_object_combobox = CoreEWS.combobox( obstacle_object_params )
	
	self._obstacle_object_combobox:connect( "EVT_COMMAND_COMBOBOX_SELECTED", callback( self, self, "set_obstacle_obj_name" ), { ctrlr = self._obstacle_object_combobox_params } )
	]]
	
	for _,data in pairs( clone( self._hed.obstacle_list ) ) do
		local unit = self._obstacle_units[ data.unit_id ]
		if not alive( unit ) then
			self:_remove_by_unit_id( data.unit_id )
		else
			local all_object_names = self:_get_objects_by_unit( unit )
			self:_add_unit( unit, all_object_names, data ) 
		end
	end
end

function NavObstacleElement:_get_objects_by_unit( unit )
	local all_object_names = {}
	
	if unit then
		local root_obj = unit:orientation_object()
		all_object_names = {}
		local tree_depth = 1
		
		local _process_object_tree
		_process_object_tree = function( obj, depth )
			local indented_name = obj:name():s()
			
			for i=1, depth do
				indented_name = "-"..indented_name
			end
			table.insert( all_object_names, indented_name )
			
			local children = obj:children()
			for _, child in ipairs( children ) do
				_process_object_tree( child, depth + 1 )
			end
		end
		
		_process_object_tree( root_obj, 0 )
		
		--[[print( "\nall_object_names" )
		for i, k in ipairs( all_object_names ) do
			print(k)
		end]]
	end
	
	return all_object_names
end

--[[ function NavObstacleElement:_on_unit_set()
	self._all_object_names = {}
	
	if self._obstacle_unit then
		local root_obj = self._obstacle_unit:orientation_object()
		self._all_object_names = {}
		local tree_depth = 1
		
		local _process_object_tree
		_process_object_tree = function( obj, depth )
			local indented_name = obj:name():s()
			
			for i=1, depth do
				indented_name = "-"..indented_name
			end
			table.insert( self._all_object_names, indented_name )
			
			local children = obj:children()
			for _, child in ipairs( children ) do
				_process_object_tree( child, depth + 1 )
			end
		end
		
		_process_object_tree( root_obj, 0 )
		]]
		--[[print( "\nself._all_object_names" )
		for i, k in ipairs( self._all_object_names ) do
			print(k)
		end]]
--[[	end
	
	local is_current_name_valid
	for _, obj_name_ind in ipairs( self._all_object_names ) do
		if obj_name_ind == self._obstacle_obj_name_indented then
			is_current_name_valid = true
			break
		end
	end
	if not is_current_name_valid then
		self._obstacle_obj_name_indented = self._all_object_names[1] or ""
	end
	self._obstacle_obj_name = Idstring( self._unindent_obj_name( self._obstacle_obj_name_indented ) )
	
	if self._obstacle_object_combobox_params then
		CoreEWS.update_combobox_options( self._obstacle_object_combobox_params, self._all_object_names )
		CoreEWS.change_combobox_value( self._obstacle_object_combobox_params, self._all_object_names[1] or "" )
	end
end]]

--[[function NavObstacleElement:set_obstacle_obj_name( params )
	--print( "NavObstacleElement:set_obstacle_obj_name" )
	local value = self._obstacle_object_combobox_params.value
	self._obstacle_obj_name_indented = value
	--print( "self._obstacle_obj_name_indented", self._obstacle_obj_name_indented )
	self._hed.obstacle_obj_name = Idstring( self._unindent_obj_name( self._obstacle_obj_name_indented ) )
	--print( "self._hed.obstacle_obj_name", self._hed.obstacle_obj_name:s() )
end]]

function NavObstacleElement._unindent_obj_name( obj_name )
	while string.sub( obj_name, 1, 1 ) == "-" do
		obj_name = string.sub( obj_name, 2 )
	end
	return obj_name
end

function NavObstacleElement._get_indented_obj_name( obj, parent, obj_name )
	obj = (parent and parent:get_object( obj_name )) or obj
	local obj_name = (obj_name or obj:name()):s()
	
	while obj:parent() do
		obj = obj:parent()
		obj_name = "-"..obj_name
	end
	
	return obj_name
end