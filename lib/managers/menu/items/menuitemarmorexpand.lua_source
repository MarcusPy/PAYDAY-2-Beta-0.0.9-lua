core:import("CoreMenuItem")
core:import("CoreMenuItemOption")

MenuItemArmorExpand = MenuItemArmorExpand or class( MenuItemExpand )
MenuItemArmorExpand.TYPE = "weapon_expand"

function MenuItemArmorExpand:init( data_node, parameters )
	MenuItemArmorExpand.super.init( self, data_node, parameters )
	
	self._type = MenuItemArmorExpand.TYPE
	
	--[[local data_node = {
					type		= "MenuItemArmorAction",
					name		= self._parameters.armor_id.."buy",
					text_id		= "menu_buy",
					action_type = "buy",
					callback 	= "buy_armor",
					visible_callback = "can_buy_armor",
					armor_id	= self._parameters.armor_id,
				}
	local item = CoreMenuNode.MenuNode.create_item( self, data_node )
	self:add_item( item )]]
	
	local data_node = {
					type		= "MenuItemArmorAction",
					name		= self._parameters.armor_id.."equip",
					text_id		= "menu_equip",
					action_type = "equip",
					callback 	= "equip_armor",
					visible_callback = "owns_armor",
					armor_id	= self._parameters.armor_id,
				}
	local item = CoreMenuNode.MenuNode.create_item( self, data_node )
	self:add_item( item )
		
	--[[local data_node = {
					type		= "MenuItemArmorAction",
					name		= self._parameters.armor_id.."repair",
					text_id		= "menu_repair",
					action_type = "repair",
					callback	= "repair_armor",
					visible_callback = "owns_armor",
					-- condition	= self._parameters.condition,
					armor_id	= self._parameters.armor_id,
				}
	local item = CoreMenuNode.MenuNode.create_item( self, data_node )
	self:add_item( item )]]
	
	--[[
	local data_node = {
					type		= "MenuItemArmorAction",
					name		= self._parameters.armor_id.."buy_upgrades",
					text_id		= "menu_buy_upgrades",
					action_type = "buy_upgrades",
					callback	= "buy_weapon_upgrades",
					visible_callback = "owns_weapon",
					armor_id	= self._parameters.armor_id,
					weapon_slot = self._parameters.weapon_slot,
					next_node	= "buy_upgrades",
				}
	local parameters = {
						next_node_parameters = { self._parameters.weapon_id },
						} 
	local item = CoreMenuNode.MenuNode.create_item( self, data_node, parameters )
	self:add_item( item )
	self:_show_items( nil )]]
end

function MenuItemArmorExpand:expand_value()
	return 0
end

function MenuItemArmorExpand:toggle( ... )
	if not self:parameter( "unlocked" ) then
		-- return
	end
	MenuItemArmorExpand.super.toggle( self, ... )
end

function MenuItemArmorExpand:can_expand()
	return self:parameter( "unlocked" )
end

-- GUI
function MenuItemArmorExpand:setup_gui( node, row_item )
	-- print( "MenuItemArmorExpand:setup_gui", self._parameters.weapon_id )
	local scaled_size = managers.gui_data:scaled_size()
	
	row_item.gui_panel = node.item_panel:panel( { w = node.item_panel:w() } )
	row_item.armor_name = node._text_item_part( node, row_item, row_item.gui_panel, node.align_line_padding( node ) )
	row_item.armor_name:set_font_size( 22 )
		
	local _,_,w,h = row_item.armor_name:text_rect()
	row_item.armor_name:set_h( h )
		
	row_item.gui_panel:set_left( node._mid_align( node ) + self._parameters.expand_value )
	row_item.gui_panel:set_w( scaled_size.width - row_item.gui_panel:left() )
	row_item.gui_panel:set_h( h )
	-- row_item.gui_panel:set_debug( true )
	
	--[[local texture, rect = tweak_data.hud_icons:get_icon_data( tweak_data.weapon[ self._parameters.weapon_id ].hud_icon )
	
	row_item.weapon_icon = row_item.gui_panel:bitmap( { texture = texture, texture_rect = rect, layer = node.layers.items } )
	row_item.weapon_icon:set_size( h, h )
	row_item.armor_name:set_left( h + 4 )]]
		
	local texture, rect = tweak_data.hud_icons:get_icon_data( "icon_equipped" )
	row_item.equipped_icon = row_item.gui_panel:parent():bitmap( { visible = self._parameters.equipped, texture = texture, texture_rect = rect, layer = node.layers.items } )
	row_item.equipped_icon:set_center( h/2, row_item.gui_panel:y() + h/2 )
	row_item.equipped_icon:set_right( row_item.gui_panel:x() )
		
	local texture, rect = tweak_data.hud_icons:get_icon_data( "icon_locked" )
	row_item.locked_icon = row_item.gui_panel:bitmap( { visible = not self._parameters.unlocked, texture = texture, texture_rect = rect, layer = node.layers.items } )
	row_item.locked_icon:set_center( h/2, h/2 )
	row_item.locked_icon:set_right( row_item.locked_icon:parent():w() - 4 )
	
	if( row_item.align == "right" ) then
		row_item.armor_name:set_right( row_item.locked_icon:left() - 10 )
	end
	-- Repair		
	local texture, rect = tweak_data.hud_icons:get_icon_data( "icon_circlebg" )
	row_item.circlefill = row_item.gui_panel:bitmap( { visible = self._parameters.unlocked and self._parameters.owned, texture = texture, texture_rect = rect, layer = node.layers.items } )
	row_item.circlefill:set_position( row_item.locked_icon:position() )
	
	-- Repair
	--[[local texture, rect = tweak_data.hud_icons:get_icon_data( "icon_circlefill"..self._parameters.condition )
	row_item.repair_circle = row_item.gui_panel:bitmap( { visible = self._parameters.unlocked and self._parameters.owned, texture = texture, texture_rect = rect, layer = node.layers.items + 1, color = self:_repair_circle_color( self._parameters.condition ) } )
	row_item.repair_circle:set_position( row_item.circlefill:position() )]]
		
	row_item.expanded_indicator = row_item.gui_panel:parent():bitmap ( { visible = false, texture = "guis/textures/menu_selected", x = 0, y = 0, layer = 0 } )
	row_item.expanded_indicator:set_w( row_item.gui_panel:w() )
	row_item.expanded_indicator:set_height( 64 * row_item.gui_panel:height()/32 )
	-- row_item.expanded_indicator:set_rotation( 180 )
	
	row_item.bottom_line = row_item.gui_panel:parent():bitmap( { texture = "guis/textures/headershadowdown", layer = node.layers.items + 1, color = Color.white, w = row_item.gui_panel:w(), y = 100 } )
	-- self:on_item_position( row_item, node )
	
	return true
end

-- GUI
function MenuItemArmorExpand:on_item_position( row_item, node )
	row_item.expanded_indicator:set_position( row_item.gui_panel:position() )
	row_item.expanded_indicator:set_center_y( row_item.gui_panel:center_y() )
	row_item.equipped_icon:set_center_y( row_item.gui_panel:center_y() )
	
	row_item.expand_line:set_lefttop( row_item.gui_panel:leftbottom() )
	row_item.expand_line:set_left( row_item.expand_line:left() )
end

-- GUI
function MenuItemArmorExpand:on_item_positions_done( row_item, node )
	if self:expanded() then
		local child = self._items[ #self._items ]
		local row_child = node.row_item( node, child )
						
		if row_child then -- CASE FOR NO CHILDREN
			row_item.bottom_line:set_lefttop( row_child.gui_panel:leftbottom() )
			row_item.bottom_line:set_top( row_item.bottom_line:top() - 1 )
		end
	end
end

function MenuItemArmorExpand:on_buy( node )
	self:update_expanded_items( node )
end

function MenuItemArmorExpand:on_equip( node )
	for _,item in ipairs( self:parameters().parent_item:items() ) do
		-- print( "on_equip", item:name() )
		local row_item = node:row_item( item )
		item:reload( row_item, node )
	end
end

function MenuItemArmorExpand:on_repair( node ) --, condition )
	--[[local row_item = node:row_item( self )
	self._parameters.condition = Global.blackmarket_manager.armors[ self._parameters.armor_id ].condition
	local texture, rect = tweak_data.hud_icons:get_icon_data( "icon_circlefill"..self._parameters.condition )
	row_item.repair_circle:set_texture_rect( rect[1], rect[2], rect[3], rect[4] )
	row_item.repair_circle:set_size( rect[3], rect[4] )
	row_item.repair_circle:set_color( self:_repair_circle_color( self._parameters.condition ) )]]
end

function MenuItemArmorExpand:_repair_circle_color( condition )
	return condition < 4 and Color( 1, 0.5, 0 ) or Color.white
end

function MenuItemArmorExpand:get_h( row_item, node )
	local h = row_item.gui_panel:h()
	if self:expanded() then
		-- print( #self:items() )
		for _,item in ipairs( self:items() ) do
		-- print( "on_equip", item:name() )
			local child_row_item = node:row_item( item )
			if child_row_item then
				h = h + child_row_item.gui_panel:h()
			end
		end
	end
	-- print( "h", h )
	return h
end

-- GUI
function MenuItemArmorExpand:reload( row_item, node )
	-- print( "MenuItemArmorExpand:reload", self:name() )
	-- print( "  item:parameters().parent_item", self:parameters().parent_item:name() )
	MenuItemArmorExpand.super.reload( self, row_item, node )
	row_item.expanded_indicator:set_position( row_item.gui_panel:position() )
	row_item.expanded_indicator:set_center_y( row_item.gui_panel:center_y() )
	row_item.expanded_indicator:set_visible( self:expanded() )
	
	row_item.expand_line:set_w( row_item.gui_panel:w() )
	
	-- row_item.bottom_line:set_lefttop( row_item.gui_panel:leftbottom() )
	-- row_item.bottom_line:set_center_y( row_item.gui_panel:center_y() )
	row_item.bottom_line:set_visible( self:expanded() and self:parameter( "unlocked" ) )
	row_item.circlefill:set_visible( not self:expanded() and self:parameter( "unlocked" ) and self:parameter( "owned" ) )
	-- row_item.repair_circle:set_visible( not self:expanded() and self:parameter( "unlocked" ) and self:parameter( "owned" ) )
	self._parameters.equipped = Global.blackmarket_manager.armors[ self:parameter( "armor_id" ) ].equipped
	row_item.equipped_icon:set_visible( self._parameters.equipped )
	-- row_item.expand_line:set_visible( false )
	if self:expanded() then
		row_item.expanded_indicator:set_color( node.row_item_color ) -- Color.white )
		row_item.menu_unselected:set_color( node.row_item_hightlight_color ) -- Color( 1, 0.5, 0 ) )
	else
		row_item.menu_unselected:set_color( node.row_item_color ) -- Color( 0.5, 0.5, 0.5 ) )
	end
	
	self:_set_row_item_state( node, row_item )
end

-- GUI
function MenuItemArmorExpand:highlight_row_item( node, row_item, mouse_over )
	self:_set_row_item_state( node, row_item )
	row_item.armor_name:set_color( row_item.color )
	row_item.armor_name:set_font( tweak_data.menu.default_font_no_outline_id )
end

-- GUI
function MenuItemArmorExpand:fade_row_item( node, row_item, mouse_over )
	self:_set_row_item_state( node, row_item )
end

-- GUI
function MenuItemArmorExpand:_set_row_item_state( node, row_item )
	if self:expanded() or row_item.highlighted then
		row_item.armor_name:set_color( node.row_item_hightlight_color ) -- Color.black )
		row_item.armor_name:set_font( tweak_data.menu.default_font_no_outline_id )
	else
		row_item.armor_name:set_color( self:parameter( "owned" ) and self:parameter( "unlocked" ) and row_item.color or Color( 1, 0.5, 0.5, 0.5 ) )
		row_item.armor_name:set_font( tweak_data.menu.default_font_id )
	end
end

-- GUI
function MenuItemArmorExpand:on_delete_row_item( row_item, ... )
	MenuItemArmorExpand.super.on_delete_row_item( self, row_item, ... )
	row_item.gui_panel:parent():remove( row_item.equipped_icon )
	row_item.gui_panel:parent():remove( row_item.bottom_line )
end

function MenuItemArmorExpand:condition()
	 return self:parameter( "condition" )
end

function MenuItemArmorExpand:_max_condition()
	return 16
end

function MenuItemArmorExpand:_at_max_condition()
	return self._parameters.condition == self:_max_condition()
end

----------------------------------------------------------------------------------

MenuItemArmorAction = MenuItemArmorAction or class( MenuItemExpandAction )
