core:import("CoreMenuItem")

MenuItemDivider = MenuItemDivider or class( CoreMenuItem.Item )
MenuItemDivider.TYPE = "divider"

function MenuItemDivider:init( data_node, parameters )
	MenuItemDivider.super.init( self, data_node, parameters )
	
	self._type = MenuItemDivider.TYPE
end

-- GUI
function MenuItemDivider:setup_gui( node, row_item )
	-- print( "MenuItemDivider:setup_gui", self._parameters.weapon_id )
	local scaled_size = managers.gui_data:scaled_size()
	
	row_item.gui_panel = node.item_panel:panel( { w = node.item_panel:w() } )
	
	local h = row_item.item:parameters().size or 10
	if row_item.text then
		print ("HAS TEXT", row_item.text )
		row_item.text = node._text_item_part( node, row_item, row_item.gui_panel, 0 ) --node.align_line_padding( node ) )
		local _,_,tw,th = row_item.text:text_rect()
		row_item.text:set_size( tw, th )
		h = th
	end
					
	row_item.gui_panel:set_left( node._mid_align( node ) )
	row_item.gui_panel:set_w( scaled_size.width - row_item.gui_panel:left() )
	row_item.gui_panel:set_h( h )
	
	return true
	-- row_item.bottom_line = row_item.gui_panel:parent():bitmap( { texture = "guis/textures/headershadowdown", layer = node.layers.items + 1, color = Color.white, w = row_item.gui_panel:w(), y = 100 } )
end

-- GUI
--[[function MenuItemDivider:on_item_position( row_item, node )
	row_item.expanded_indicator:set_position( row_item.gui_panel:position() )
	row_item.expanded_indicator:set_center_y( row_item.gui_panel:center_y() )
	row_item.equipped_icon:set_center_y( row_item.gui_panel:center_y() )
	
	row_item.expand_line:set_lefttop( row_item.gui_panel:leftbottom() )
	row_item.expand_line:set_left( row_item.expand_line:left() )
end

-- GUI
function MenuItemDivider:on_item_positions_done( row_item, node )
	if self:expanded() then
		local child = self._items[ #self._items ]
		local row_child = node.row_item( node, child )
						
		if row_child then -- CASE FOR NO CHILDREN
			row_item.bottom_line:set_lefttop( row_child.gui_panel:leftbottom() )
			row_item.bottom_line:set_top( row_item.bottom_line:top() - 1 )
		end
	end
end]]

-- GUI
function MenuItemDivider:reload( row_item, node )
	-- print( "MenuItemDivider:reload", self:name() )
	-- print( "  item:parameters().parent_item", self:parameters().parent_item:name() )
	MenuItemDivider.super.reload( self, row_item, node )
	
	-- row_item.bottom_line:set_lefttop( row_item.gui_panel:leftbottom() )
	-- row_item.bottom_line:set_center_y( row_item.gui_panel:center_y() )
	
	self:_set_row_item_state( node, row_item )
	return true
end

-- GUI
function MenuItemDivider:highlight_row_item( node, row_item, mouse_over )
	self:_set_row_item_state( node, row_item )
	return true
	-- row_item.clothing_name:set_color( row_item.color )
	-- row_item.clothing_name:set_font( tweak_data.menu.default_font_no_outline_id )
end

-- GUI
function MenuItemDivider:fade_row_item( node, row_item, mouse_over )
	self:_set_row_item_state( node, row_item )
	return true
end

-- GUI
function MenuItemDivider:_set_row_item_state( node, row_item )
	if row_item.highlighted then
		-- row_item.clothing_name:set_color( Color.black )
		-- row_item.clothing_name:set_font( tweak_data.menu.default_font_no_outline_id )
	else
		-- row_item.clothing_name:set_color( self:parameter( "owned" ) and self:parameter( "unlocked" ) and row_item.color or Color( 1, 0.5, 0.5, 0.5 ) )
		-- row_item.clothing_name:set_font( tweak_data.menu.default_font_id )
	end
end

function MenuItemDivider:menu_unselected_visible()
	return false
end

-- GUI
function MenuItemDivider:on_delete_row_item( row_item, ... )
	MenuItemDivider.super.on_delete_row_item( self, row_item, ... )
end

