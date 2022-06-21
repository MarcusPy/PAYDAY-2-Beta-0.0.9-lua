require "lib/managers/menu/WalletGuiObject"

local NOT_WIN_32 = not (SystemInfo:platform() == Idstring( "WIN32" ))
local WIDTH_MULTIPLIER = NOT_WIN_32 and 0.69 or 0.75
local BOX_GAP = 54 * 0.25
local GRID_H_MUL = ( NOT_WIN_32 and 7 or 6 ) / 8

local massive_font = tweak_data.menu.pd2_massive_font
local large_font = tweak_data.menu.pd2_large_font
local medium_font = tweak_data.menu.pd2_medium_font
local small_font = tweak_data.menu.pd2_small_font

local massive_font_size = tweak_data.menu.pd2_massive_font_size
local large_font_size = tweak_data.menu.pd2_large_font_size
local medium_font_size = tweak_data.menu.pd2_medium_font_size
local small_font_size = tweak_data.menu.pd2_small_font_size

BlackMarketGuiItem = BlackMarketGuiItem or class()

function BlackMarketGuiItem:init( main_panel, data, x, y, w, h )
	self._main_panel = main_panel
	self._panel = main_panel:panel( { name=tostring(data.name), x=x, y=y, w=w, h=h } )
	self._data = data or {}
	
	self._name = data.name
	self._child_panel = nil
	
	self._alpha = 1
	
	-- self:deselect( true )
	-- self:set_highlight( false )
end

function BlackMarketGuiItem:inside( x, y )
	return self._panel:inside( x, y )
end

function BlackMarketGuiItem:select( instant, no_sound )
	if not self._selected then
		self._selected = true
		self:refresh()
		
		if not instant and not no_sound then
			managers.menu_component:post_event( "highlight" )
		end
	end
end

function BlackMarketGuiItem:deselect( instant )
	if self._selected then
		self._selected = false
	end
	self:refresh()
end

function BlackMarketGuiItem:set_highlight( highlight, no_sound )
	if self._highlighted ~= highlight then
		self._highlighted = highlight
		self:refresh()
		
		if highlight and not no_sound then
			managers.menu_component:post_event( "highlight" )
		end
	end
end

function BlackMarketGuiItem:refresh()
	-- self._panel:set_alpha(  )		-- need to set alpha on each object so that I can controll wich object gets the alpha
	self._alpha = (self._selected and 1) or (self._highlighted and 0.85) or 0.7
	
	for _, object in ipairs( self._panel:children() ) do
		object:set_alpha( self._alpha )
	end
	
	if( self._child_panel and alive( self._child_panel ) ) then
		self._child_panel:set_visible( self._selected )
	end
end

function BlackMarketGuiItem:mouse_pressed( button, x, y )
	return self._panel:inside( x, y ) 
end

function BlackMarketGuiItem:destroy()
end


BlackMarketGuiTabItem = BlackMarketGuiTabItem or class( BlackMarketGuiItem )

function BlackMarketGuiTabItem:init( main_panel, data, node, size_data, hide_select_rect, scroll_tab_table )
	BlackMarketGuiTabItem.super.init( self, main_panel, data, 0, 0, main_panel:w(), main_panel:h() )
	
	local grid_panel_w = size_data.grid_w -- self._panel:w()*0.85
	local grid_panel_h = size_data.grid_h -- self._panel:h()*0.75
	
	local square_w = size_data.square_w -- grid_panel_w / 3 - 10
	local square_h = size_data.square_h -- grid_panel_h / 3 - 10
	local padding_w = size_data.padding_w -- ( grid_panel_w - ( square_w * 3 ) ) / 4
	local padding_h = size_data.padding_h -- ( grid_panel_h - ( square_h * 3 ) ) / 4
	local left_padding = size_data.left_padding
	local top_padding = size_data.top_padding
	
	self._node = node
	
	local slots = data.override_slots or { 3, 3 }
	slots[1] = math.max( 1, slots[1] )
	slots[2] = math.max( 1, slots[2] )
	
	self.my_slots_dimensions = slots
	
	square_w = square_w * 3 / slots[1]
	square_h = square_h * 3 / slots[2]
	
	self._tab_panel = scroll_tab_table.panel:panel( { name="tab_panel" } )
	self._tab_text_string = utf8.to_upper( data.name_localized or managers.localization:text(data.name) )
	-- self._tab_text_string = self._tab_text_string .. self._tab_text_string
	local text = self._tab_panel:text( { name="tab_text", align="center", vertical="center", 
																		text=self._tab_text_string, 
																		font_size=medium_font_size, font=medium_font, 
																		color=tweak_data.screen_colors.button_stage_3, blend_mode="add", layer=1, visible=(not hide_select_rect) } )
	
	BlackMarketGui.make_fine_text( self, text )
	local _, _, tw, th = text:text_rect()
	self._tab_panel:set_size( tw + 15, th + 10 )
	self._tab_panel:child( "tab_text" ):set_size( self._tab_panel:size() )
	
	self._tab_panel:set_center_x( self._panel:w()/2 )
	self._tab_panel:set_y( 0 )
	self._tab_panel:bitmap( { name = "tab_select_rect", texture="guis/textures/pd2/shared_tab_box", w=self._tab_panel:w(), h=self._tab_panel:h(), layer=0, color=tweak_data.screen_colors.text:with_alpha(hide_select_rect and 0 or 1), visible=false } )
	
	table.insert( scroll_tab_table, self._tab_panel )
	
	self._child_panel = self._panel:panel()
	-- grid_panel_h = ( ( self._panel:h() - self._tab_panel:h() - 70 ) * GRID_H_MUL )
	-- square_h = grid_panel_h / 3
	
	self._grid_panel = self._child_panel:panel( { name="grid_panel", w=grid_panel_w, h=grid_panel_h, layer=1 } )
	self._grid_panel:set_left( 0 ) -- set_center_x( self._child_panel:w()/2 + left_padding )
	self._grid_panel:set_top( self._tab_panel:bottom() - 2 + top_padding ) -- set_center_y( self._child_panel:h()/2 ) -- set_y( self._tab_panel:bottom() )
	-- self._grid_panel:rect( { color=Color.black:with_alpha( 0.3 ) } )
	
	self._node:parameters().menu_component_tabs = self._node:parameters().menu_component_tabs or {}
	self._node:parameters().menu_component_tabs[data.name] = self._node:parameters().menu_component_tabs[data.name] or {}
	
	self._my_node_data = self._node:parameters().menu_component_tabs[data.name]
	
	-- text:set_center( self._tab_panel:w()/2, self._tab_panel:h()/2 )
	
	
	-- 
	-- { name=bm_menu_primaries, on_create_func=callback(self, self, "populate_primaries") }
	
	-- if( self._data.on_create_func ) then
	self._data.on_create_func( self._data )
	-- end
	
	self._slots = {}
	local slot_equipped = 1
	for index, data in ipairs( self._data ) do
		local new_slot_class = BlackMarketGuiSlotItem
		-- 
		if data.unique_slot_class then
			new_slot_class = _G[ data.unique_slot_class ]
		end
		
		table.insert( self._slots, new_slot_class:new( self._grid_panel, data, padding_w + ((index-1)%slots[1] * (square_w+padding_w)), padding_h + (math.floor((index-1)/slots[1]) * (square_h+padding_h)), square_w, square_h ) )
		if( data.equipped ) then
			slot_equipped = index
		end
	end
	
	self:check_new_drop()
	
	self._slot_selected = #self._slots > 0 and ( self._my_node_data.selected or slot_equipped )
	self._slot_highlighted = nil
	
	self:deselect( true )
	self:set_highlight( false )
end

function BlackMarketGuiTabItem:destroy()
	for i, slot in ipairs( self._slots ) do
		slot:destroy()
	end
end

function BlackMarketGuiTabItem:set_tab_text( new_text )
	local text = self._tab_panel:child( "tab_text" )
	text:set_text( new_text )
	
	BlackMarketGui.make_fine_text( self, text )
	local _, _, tw, th = text:text_rect()
	self._tab_panel:set_size( tw + 15, th + 10 )
	text:set_size( self._tab_panel:size() )
	
	self._tab_panel:child( "tab_select_rect" ):set_size( self._tab_panel:size() )
end

function BlackMarketGuiTabItem:check_new_drop( first_time )
	local got_new_drop = false
	for _, slot in pairs( self._slots ) do
		-- print(slot._data.new_drop_data)
		if slot._data.new_drop_data and slot._data.new_drop_data.icon then
			got_new_drop = true
			break
		end
	end
	
	local tab_text_string = self._tab_text_string
	if got_new_drop then
		tab_text_string = tab_text_string .. "" .. managers.localization:get_default_macro( "BTN_INV_NEW" )
		--[[
		if not self._new_drop_icon then
			self._new_drop_icon = self._tab_panel:bitmap( { texture="guis/textures/pd2/blackmarket/inv_newdrop", w=16, h=16, rotation=360 } )
			self._new_drop_icon:set_visible( self._tab_panel:child( "tab_text" ):visible() )
			self._new_drop_icon:set_leftbottom( 0, 0 ) 
		end
	elseif self._new_drop_icon then
		self._new_drop_icon:parent():remove( self._new_drop_icon )
		self._new_drop_icon = nil]]
	else
		
	end
	self:set_tab_text( tab_text_string )
end

function BlackMarketGuiTabItem:refresh()
	self._alpha = 1
	
	if self._selected then
		self._tab_panel:child( "tab_text" ):set_color( tweak_data.screen_colors.button_stage_1 )
		self._tab_panel:child( "tab_text" ):set_blend_mode( "normal" )
		self._tab_panel:child( "tab_select_rect" ):show()
	elseif self._highlighted then
		self._tab_panel:child( "tab_text" ):set_color( tweak_data.screen_colors.button_stage_2 )
		self._tab_panel:child( "tab_text" ):set_blend_mode( "add" )
		self._tab_panel:child( "tab_select_rect" ):hide()
	else
		self._tab_panel:child( "tab_text" ):set_color( tweak_data.screen_colors.button_stage_3 )
		self._tab_panel:child( "tab_text" ):set_blend_mode( "add" )
		self._tab_panel:child( "tab_select_rect" ):hide()
	end
	
	-- local color = (self._selected and tweak_data.screen_colors.button_stage_1) or (self._highlighted and tweak_data.screen_color_blue_highlighted) or tweak_data.screen_color_blue_noselected
	
	-- self._tab_panel:child( "tab_text" ):set_color( color )
	if( self._child_panel and alive( self._child_panel ) ) then
		self._child_panel:set_visible( self._selected )
	end
end

function BlackMarketGuiTabItem:set_tab_position( x )
	self._tab_panel:set_x( x )
	local _, _, tw, th = self._tab_panel:child("tab_text"):text_rect()
	self._tab_panel:set_size( tw + 15, th + 10 )
	self._tab_panel:child( "tab_text" ):set_size( self._tab_panel:size() )
	
	-- self._tab_panel:child("tab_text"):set_center_x( math.round( self._tab_panel:w()/2 ) )
		
	if self._new_drop_icon then
		self._new_drop_icon:set_leftbottom( 0, 0 ) 
	end
	return math.round( x + tw + 15 + 5 )
end

function BlackMarketGuiTabItem:inside( x, y )
	if( self._tab_panel:inside( x, y ) ) then
		return true
	end
	if( not self._selected ) then
		return
	end
	
	local update_select = false
	if( not self._slot_highlighted ) then
		update_select = true
	elseif( self._slots[ self._slot_highlighted ] and not self._slots[ self._slot_highlighted ]:inside( x, y ) ) then
		self._slots[ self._slot_highlighted ]:set_highlight( false )
		self._slot_highlighted = nil
		update_select = true
	end
	
	if( update_select ) then
		for i, slot in ipairs( self._slots ) do
			if( slot:inside( x, y ) ) then
				self._slot_highlighted = i
				self._slots[ self._slot_highlighted ]:set_highlight( true )
				return 1
			end
		end
	end
end

function BlackMarketGuiTabItem:mouse_pressed( button, x, y )
	if not self._slots[ self._slot_highlighted ] then
		return
	end
	
	if self._slots[ self._slot_selected ] == self._slots[ self._slot_highlighted ] then
		return
	end

	if( self._slots[ self._slot_highlighted ] and self._slots[ self._slot_highlighted ]:inside( x, y ) ) then
		if( self._slots[ self._slot_selected ] ) then
			self._slots[ self._slot_selected ]:deselect( false )
		end
		
		return self:select_slot( self._slot_highlighted )
		-- self._slot_selected = self._slot_highlighted
		-- self._my_node_data.selected = self._slot_selected
		-- return self._slots[ self._slot_selected ]:select( false )
		
		
		-- return true
		-- managers.menu:open_node( "blackmarket_node", { self._data[ self._slots[ self._slot_highlighted ]._name ] } )
	end
end

function BlackMarketGuiTabItem:selected_slot_center()
	if( not self._slots[ self._slot_selected ] ) then
		return 0, 0
	end
	
	local x = self._slots[ self._slot_selected ]._panel:world_center_x()
	local y = self._slots[ self._slot_selected ]._panel:world_center_y()
	
	return x, y
end

function BlackMarketGuiTabItem:select_slot( slot, instant )
	slot = (not slot and self._slot_selected) or (self._slots[ slot ] and slot) or 1
	
	local no_sound = false
	if self._slots[slot]._name == "empty" then
		slot = self._slot_selected
		no_sound = true
	-- 	return self._slots[ self._slot_selected ]
	end
	
	if( self._slots[ self._slot_selected ] ) then
		self._slots[ self._slot_selected ]:deselect( instant )
	end
	self._slot_selected = slot
	self._my_node_data.selected = self._slot_selected
	
	local selected_slot = self._slots[ self._slot_selected ]:select( instant, no_sound )
	self:check_new_drop()	-- needs to check after slot selected
	managers.menu_component:set_blackmarket_tab_positions()
	
	return selected_slot
end

function BlackMarketGuiTabItem:slots()
	return self._slots
end


BlackMarketGuiSlotItem = BlackMarketGuiSlotItem or class( BlackMarketGuiItem )

function BlackMarketGuiSlotItem:init( main_panel, data, x, y, w, h )
	BlackMarketGuiSlotItem.super.init( self, main_panel, data, x, y, w, h )
	
	-- self._rect = self._panel:bitmap( { name="rect", texture="guis/textures/pd2/blackmarket/slot_bg", w=self._panel:w(), h=self._panel:h() } ) -- self._panel:rect( { name="rect", color=Color( 0.8, 0.2, 0.2, 0.2 ) } )
	-- local text = self._panel:text( { name="text", text=(data.name_localized or utf8.to_upper( managers.localization:text(data.name) )), align="right", vertical="top", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_blue, layer=3 } )
	
	if data.hide_bg then
		-- self._rect:hide()
	end
	
	if data.mid_text then
		local text = self._panel:text( { name="text", text=utf8.to_upper( data.mid_text.noselected_text ), align=data.mid_text.align or "center", vertical=data.mid_text.vertical or "center", font_size=data.mid_text.font_size or medium_font_size, font=data.mid_text.font or medium_font, color=data.mid_text.noselected_color, blend_mode=data.mid_text.blend_mode or "add", layer=2 } )
		text:grow( -10, -10 )
		text:move( 5, 5 )
		self._text_in_mid = true
	end
	
	local animate_loading_texture = function(o)
		o:set_color( o:color():with_alpha( 0 ) )
		local time = coroutine.yield()
		
		o:set_color( o:color():with_alpha( 1 ) )
		while( true ) do
			o:set_rotation( time * 180 )
			
			time = ( time + coroutine.yield() ) % 2
		end
	end
	
	self._extra_textures = {}
	if( data.extra_bitmaps ) then
		local color
		for i, bitmap in ipairs( data.extra_bitmaps ) do
			color = data.extra_bitmaps_colors and data.extra_bitmaps_colors[i] or Color.white
			table.insert( self._extra_textures, self._panel:bitmap( { texture=bitmap, color=color, w=32, h=32, layer=0 } ) )
		end
	end
	
	local texture_loaded_clbk = callback( self, self, "texture_loaded_clbk" )
	if data.mini_icons then
		local padding = data.mini_icons.borders and 14 or 5
		for k, icon_data in ipairs( data.mini_icons ) do
			icon_data.padding = padding
			if not icon_data.texture then
				local new_icon
				if icon_data.text then
					new_icon = self._panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_font_size, text=icon_data.text, color=icon_data.color or Color.white, w=icon_data.w or 32, h=icon_data.h or 32, layer=icon_data.layer or 1 } )
				else
					new_icon = self._panel:rect( { color=icon_data.color or Color.white, w=icon_data.w or 32, h=icon_data.h or 32, layer=icon_data.layer or 1 } )
				end
				if icon_data.visible == false then
					new_icon:set_visible( false )
				end
				
				if icon_data.left then
					new_icon:set_left( padding + icon_data.left )
				elseif icon_data.right then
					new_icon:set_right( self._panel:w()-padding - icon_data.right )
				end
				
				if icon_data.top then
					new_icon:set_top( padding + icon_data.top )
				elseif icon_data.bottom then
					new_icon:set_bottom( self._panel:h()-padding - icon_data.bottom )
				end
				
				if icon_data.name == "new_drop" and data.new_drop_data then
					data.new_drop_data.icon = new_icon
				end
			elseif icon_data.stream then
				if( DB:has( Idstring( "texture" ), icon_data.texture ) ) then
					icon_data.request_index = managers.menu_component:request_texture( icon_data.texture, callback( self, self, "icon_loaded_clbk", icon_data ) )
					-- self._requested_texture = data.bitmap_texture
					
					-- TextureCache:request( icon_data.texture, "NORMAL", callback( self, self, "icon_loaded_clbk", icon_data ) )
				end
			else
				local new_icon = self._panel:bitmap( { texture=icon_data.texture, color=icon_data.color or Color.white, w=icon_data.w or 32, h=icon_data.h or 32, layer=icon_data.layer or 1 } )
				if icon_data.render_template then
					new_icon:set_render_template( icon_data.render_template )
				end
				
				if icon_data.visible == false then
					new_icon:set_visible( false )
				end
				if icon_data.left then
					new_icon:set_left( padding + icon_data.left )
				elseif icon_data.right then
					new_icon:set_right( self._panel:w()-padding - icon_data.right )
				end
				
				if icon_data.top then
					new_icon:set_top( padding + icon_data.top )
				elseif icon_data.bottom then
					new_icon:set_bottom( self._panel:h()-padding - icon_data.bottom )
				end
				
				if icon_data.name == "new_drop" and data.new_drop_data then
					data.new_drop_data.icon = new_icon
				end
				-- new_icon:set_center( self._panel:w() - ( icon_data.pos_x - 1 ) * 18 - 16 - 5, self._panel:h() - ( icon_data.pos_y - 1 ) * 18 - 16 - 5 )
			end
		end
		if data.mini_icons.borders then
			local tl_side = self._panel:rect( { w=10, h=2, color=Color.white, alpha=0.4, blend_mode="add" } )
			local tl_down = self._panel:rect( { w=2, h=10, color=Color.white, alpha=0.4, blend_mode="add" } )
			
			local tr_side = self._panel:rect( { w=10, h=2, color=Color.white, alpha=0.4, blend_mode="add" } )
			local tr_down = self._panel:rect( { w=2, h=10, color=Color.white, alpha=0.4, blend_mode="add" } )
			
			local bl_side = self._panel:rect( { w=10, h=2, color=Color.white, alpha=0.4, blend_mode="add" } )
			local bl_down = self._panel:rect( { w=2, h=10, color=Color.white, alpha=0.4, blend_mode="add" } )
			
			local br_side = self._panel:rect( { w=10, h=2, color=Color.white, alpha=0.4, blend_mode="add" } )
			local br_down = self._panel:rect( { w=2, h=10, color=Color.white, alpha=0.4, blend_mode="add" } )

			tl_side:set_lefttop( self._panel:w() - 54 , 8 )
			tl_down:set_lefttop( self._panel:w() - 54 , 8 )
			
			tr_side:set_righttop( self._panel:w() - 8 , 8 )
			tr_down:set_righttop( self._panel:w() - 8 , 8 )
			
			bl_side:set_leftbottom( self._panel:w() - 54 , self._panel:h() - 8 )
			bl_down:set_leftbottom( self._panel:w() - 54 , self._panel:h() - 8 )
			
			br_side:set_rightbottom( self._panel:w() - 8 , self._panel:h() - 8 )
			br_down:set_rightbottom( self._panel:w() - 8 , self._panel:h() - 8 )
		end
	end
	
	if( data.bitmap_texture ) then
		if( DB:has( Idstring( "texture" ), data.bitmap_texture ) ) then
			self._loading_texture = true -- Idstring(data.bitmap_texture)
			if( data.stream ) then
				self._requested_texture = data.bitmap_texture
				self._request_index = managers.menu_component:request_texture( self._requested_texture, texture_loaded_clbk )
				-- TextureCache:request( data.bitmap_texture, "NORMAL", texture_loaded_clbk )
			else
				texture_loaded_clbk( data.bitmap_texture, Idstring(data.bitmap_texture) )
			end
		end
		
		if( not self._bitmap ) then
			self._bitmap = self._panel:bitmap( { name="item_texture", texture="guis/textures/pd2/endscreen/exp_ring", color=Color(0.2, 1, 1), w=32, h=32, layer=#self._extra_textures+1, render_template="VertexColorTexturedRadial" } )
			-- self._bitmap = self._panel:bitmap( { name="item_texture", texture=data.bitmap_texture, w=self._panel:w(), h=self._panel:h(), layer=1 } )
			self._bitmap:set_center( self._panel:w()/2, self._panel:h()/2 )
			
			self._bitmap:animate( animate_loading_texture )
		end
		-- text:set_w( self._panel:w() - 12 )
		-- text:set_position( 5, 5 )
		-- if( data.equipped ) then
			-- text:set_text("")
		-- end
	elseif( data.bg_texture ) then
		if( DB:has( Idstring( "texture" ), data.bg_texture ) ) then
			self._loading_texture = true -- Idstring(data.bg_texture)
			if( data.stream ) then
				self._requested_texture = data.bg_texture
				self._request_index = managers.menu_component:request_texture( self._requested_texture, texture_loaded_clbk )
				-- TextureCache:request( data.bg_texture, "NORMAL", texture_loaded_clbk )
			else
				texture_loaded_clbk( data.bg_texture, Idstring(data.bg_texture) )
			end
		end
		
		if( not self._bitmap ) then
			self._bitmap = self._panel:bitmap( { name="item_texture", texture="guis/textures/pd2/endscreen/exp_ring", color=Color(0.2, 1, 1), w=32, h=32, layer=#self._extra_textures+1, render_template="VertexColorTexturedRadial" } )
			-- self._bitmap = self._panel:bitmap( { name="item_texture_bg", texture=data.bg_texture, w=self._panel:w(), h=self._panel:h(), layer=1 } )
			self._bitmap:set_center( self._panel:w()/2, self._panel:h()/2 )
			
			self._bitmap:animate( animate_loading_texture )
		end
		-- text:set_w( self._panel:w() - 12 )
		-- text:set_position( 5, 5 )
		-- text:set_color( tweak_data.screen_color_white )
	end
	
	-- local btn_text = self._panel:text( { name="btn_text", text="", align="center", vertical="center", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=5 } )
	-- btn_text:move( 0, self._panel:h() * 0.3 )
	
	
	if( data.equipped ) then
		local equipped_string = data.equipped_text or managers.localization:text( "bm_menu_equipped" )
		local equipped_text = self._panel:text( { name="equipped_text", text=utf8.to_upper( equipped_string ), align="left", vertical="bottom", font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text, layer=2 } )
		equipped_text:move( 5, -5 )
		-- self._equipped_rect = self._main_panel:bitmap( { name="equipped_rect", w=self._panel:w()+10, h=self._panel:h()+10, layer=4, texture="guis/textures/pd2/blackmarket/equipped_box" } )  -- self._panel:rect( { name="equipped_rect", color=Color.blue, w=25, h=25, x=15, y=15, layer=2 } )
		-- self._equipped_rect:set_center( self._panel:center() )
	end
	
	local red_box = false
	local number_text = false
	
	self._conflict = data.conflict
	self._level_req = data.level
	
	if data.lock_texture then -- ( (not data.unlocked or data.can_afford == false) and not data.mid_text ) then
		red_box = true
		-- text:set_text("")
	end
	
	if( type( data.unlocked ) == "number" ) then
		number_text = math.abs( data.unlocked )
		if data.unlocked < 0 then 	
			red_box = true
			-- text:set_text()
			self._item_req = true
			-- number_text = false
		
		--[[elseif data.unlocked == 0 and self._bitmap then
			self._bitmap:set_color( self._bitmap:color():with_alpha( 0.4 ) ) 
			if( self._loading_texture ) then
				self._post_load_alpha = 0.4
			end]]
		end
	end
	if data.mid_text then
		if self._bitmap then
			self._bitmap:set_color( self._bitmap:color():with_alpha( 0.4 ) ) 
		end
		if( self._loading_texture ) then
			self._post_load_alpha = 0.4
		end
	end
	
	if( red_box ) then
		-- self._rect:set_color( Color( 0.5, 0, 0 ) )
		if( self._bitmap ) then
			self._bitmap:set_color( Color.white:with_alpha(0.8) ) -- self._level_req and Color.black:with_alpha(0.8) or Color.white:with_alpha( 0.2 ) )
			for _, bitmap in pairs( self._extra_textures ) do
				bitmap:set_color( bitmap:color():with_alpha( 0.2 ) )
			end
			self._bitmap:set_blend_mode( "sub" )
		end
		if( self._loading_texture ) then
			self._post_load_alpha = 0.2
			self._post_load_blend_mode = "sub"
		end
		
		if not data.unlocked or data.can_afford ~= false then
			self._lock_bitmap = self._panel:bitmap( { name="lock", texture=data.lock_texture or "guis/textures/pd2/skilltree/padlock", w=32, h=32, color=tweak_data.screen_colors.important_1, layer=#self._extra_textures+2 } )
			self._lock_bitmap:set_center( self._panel:w()/2, self._panel:h()/2 )
		end
		-- self._rect:set_layer(#self._extra_textures+3)
	end
	
	if( number_text ) then
		-- local num_text = self._panel:text( { name="amount", text=managers.localization:text( "bm_menu_item_amount", {amount=tostring( number_text )} ), align="right", vertical="bottom", font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text, layer=2 } )
		-- num_text:grow( -5, -5 )
	end
--[[
	if( false and self._data.comparision_data and ( data.unlocked or data.equipped ) ) then
		self._comparision_panel = self._panel:panel( { name="comparision_panel", layer=2, h=22, w=30 } )
		self._comparision_panel:set_leftbottom( 5, self._panel:h() - 5 )
		
		local damage = math.clamp( self._data.comparision_data.damage or 1, 1, 10 )
		local spread = math.clamp( self._data.comparision_data.spread or 1, 1, 10 )
		local recoil = math.clamp( self._data.comparision_data.recoil or 1, 1, 10 )
		local suppression = math.clamp( self._data.comparision_data.suppression or 1, 1, 10 )
		local concealment = math.clamp( self._data.comparision_data.concealment or 1, 1, 10 )
		local alert_size = math.clamp( self._data.comparision_data.alert_size or 1, 1, 10 )
		self._damage = self._comparision_panel:rect( { name="damage", 			y=0, w=(damage/20)*30, h=2, color=tweak_data.screen_color_light_grey, layer=1 } )
		self._spread = self._comparision_panel:rect( { name="spread", 			y=4, w=(spread/20)*30, h=2, color=tweak_data.screen_color_light_grey, layer=1 } )
		self._recoil = self._comparision_panel:rect( { name="recoil", 			y=8, w=(recoil/20)*30, h=2, color=tweak_data.screen_color_light_grey, layer=1 } )
		self._suppression = self._comparision_panel:rect( { name="suppression",	y=12, w=(suppression/20)*30, h=2, color=tweak_data.screen_color_light_grey, layer=1 } )
		self._concealment = self._comparision_panel:rect( { name="concealment",	y=16, w=(concealment/20)*30, h=2, color=tweak_data.screen_color_light_grey, layer=1 } )
		self._alert_size = self._comparision_panel:rect( { name="alert_size", 	y=20, w=(alert_size/20)*30, h=2, color=tweak_data.screen_color_light_grey, layer=1 } )		
		
		self._comparision_panel:rect( { name="damage_bg", 			y=0, w=30, h=2, color=tweak_data.screen_color_grey } )
		self._comparision_panel:rect( { name="spread_bg", 			y=4, w=30, h=2, color=tweak_data.screen_color_grey } )
		self._comparision_panel:rect( { name="recoil_bg", 			y=8, w=30, h=2, color=tweak_data.screen_color_grey } )
		self._comparision_panel:rect( { name="suppression_bg",	y=12, w=30, h=2, color=tweak_data.screen_color_grey } )
		self._comparision_panel:rect( { name="concealment_bg",	y=16, w=30, h=2, color=tweak_data.screen_color_grey } )
		self._comparision_panel:rect( { name="alert_size_bg", 	y=20, w=30, h=2, color=tweak_data.screen_color_grey } )	
	end]]
	
	self:deselect( true )
	self:set_highlight( false, true )
end

function BlackMarketGuiSlotItem:get_texure_size( debug )
	if( self._bitmap ) then
		--[[
		local texture_width = self._bitmap:texture_width()
		local texture_height = self._bitmap:texture_height()
		
		local panel_width, panel_height = self._panel:size()
		local size_height = math.max( texture_height, 128 )
		local size_width = size_height * 2
		
		local ratio = ( panel_width * size_height ) / math.max( panel_height * size_width, 1 )  -- (panel_width / panel_height) / (texture_width / size_height)
		
		return math.round(texture_width/size_width*panel_width/ratio), math.round(texture_height/size_height*panel_height)
]]
		local texture_width = self._bitmap:texture_width()
		local texture_height = self._bitmap:texture_height()
		local panel_width, panel_height = self._panel:size()
		
		if texture_width == 0 or texture_height == 0 or panel_width == 0 or panel_height == 0 then
			if debug then
				Application:error( "BlackMarketGuiSlotItem:get_texure_size(): " .. self._data.bitmap_texture or self._data.bg_texture or "" )
				Application:debug( "BlackMarketGuiSlotItem:", "texture_width " .. texture_width, "texture_height " .. texture_height, "panel_width " .. panel_width, "panel_height " .. panel_height )
			end
			return 0, 0
		end
		
		local aspect = panel_width / panel_height
		
		local sw = math.max( texture_width, texture_height * aspect )
		local sh = math.max( texture_height, texture_width / aspect )
		
		local dw = texture_width / sw
		local dh = texture_height / sh
		return math.round(dw * panel_width), math.round(dh * panel_height)
	end
	return 0, 0
end

function BlackMarketGuiSlotItem:icon_loaded_clbk( icon_data, texture_idstring, ... )
-- print( icon_data, texture_idstring, inspect({...}) )
	if( not alive( self._panel ) ) then
		-- if Application:production_build() then
			-- print( "[BlackMarketGuiSlotItem] icon_loaded_clbk(): Removing loaded texture from dead GUI", "texture_idstring", texture_idstring )
		-- end
		-- TextureCache:unretrieve( texture_idstring )
		-- local temp = managers.menu_component._main_panel:bitmap( { texture=texture_idstring, visible=false } )
		-- temp:parent():remove( temp )
		Application:error( "[BlackMarketGuiSlotItem] icon_loaded_clbk(): This code should no longer occur!!" )
		return
	end
	
	local padding = icon_data.padding or 5
	local new_icon = self._panel:bitmap( { texture=texture_idstring, color=icon_data.color or Color.white, w=icon_data.w or 32, h=icon_data.h or 32, layer=icon_data.layer or 1 } )
	if icon_data.render_template then
		new_icon:set_render_template( icon_data.render_template )
	end
	
	if icon_data.visible == false then
		new_icon:set_visible( false )
	end
	if icon_data.left then
		new_icon:set_left( padding + icon_data.left )
	elseif icon_data.right then
		new_icon:set_right( self._panel:w()-padding - icon_data.right )
	end
	
	if icon_data.top then
		new_icon:set_top( padding + icon_data.top )
	elseif icon_data.bottom then
		new_icon:set_bottom( self._panel:h()-padding - icon_data.bottom )
	end
	
	if icon_data.name == "new_drop" and self._data.new_drop_data then
		self._data.new_drop_data.icon = new_icon
	end
	icon_data.request_index = false
	-- TextureCache:unretrieve( texture_idstring )
	-- new_icon:set_center( self._panel:w() - ( icon_data.pos_x - 1 ) * 18 - 16 - 5, self._panel:h() - ( icon_data.pos_y - 1 ) * 18 - 16 - 5 )
end

function BlackMarketGuiSlotItem:destroy()
	if self._data and self._data.mini_icons then
		for i, icon_data in ipairs( self._data.mini_icons ) do
			if icon_data.stream then
				managers.menu_component:unretrieve_texture( icon_data.texture, icon_data.request_index )
			end
		end
	end
	if self._requested_texture then
		managers.menu_component:unretrieve_texture( self._requested_texture, self._request_index )
	end
end

function BlackMarketGuiSlotItem:texture_loaded_clbk( texture_idstring )
	-- local texture = TextureCache:retrieve( texture_idstring, "NORMAL" )
	
	if( not alive( self._panel ) ) then
		-- print( "BYE", texture_idstring )
		-- TextureCache:unretrieve( texture_idstring )
		Application:error( "[BlackMarketGuiSlotItem] texture_loaded_clbk(): This code should no longer occur!!" )
		return
		--[[
		local temp = managers.menu_component._main_panel:bitmap( { texture=texture_idstring, visible=false } )
		temp:parent():remove( temp )
		
		if Application:production_build() then
			print( "[BlackMarketGuiSlotItem] texture_loaded_clbk(): Removing loaded texture for dead GUI", "texture_idstring", texture_idstring )
		end
		return]]
	end
	
	if( self._bitmap ) then
		self._bitmap:stop()
		self._bitmap:set_rotation( 0 )
		self._bitmap:set_color( Color.white )
		self._bitmap:set_image( texture_idstring )
		self._bitmap:set_render_template( self._data.render_template or Idstring("VertexColorTextured") )
		self._bitmap:set_blend_mode( "normal" )
		for _, bitmap in pairs( self._extra_textures ) do
			bitmap:set_color( bitmap:color():with_alpha( 1 ) )
			bitmap:set_blend_mode( "normal" )
		end
	else
		self._bitmap = self._panel:bitmap( { name="item_texture", texture=texture_idstring, blend_mode="normal", layer=1 } )
		self._bitmap:set_render_template( self._data.render_template or Idstring("VertexColorTextured") )
	end
	self._bitmap:set_size( self:get_texure_size( true ) )
	self._bitmap:set_center( self._panel:w()/2, self._panel:h() / 2 )
	
	for _, bitmap in pairs( self._extra_textures ) do
		bitmap:set_size( self._bitmap:size() )
		bitmap:set_center( self._bitmap:center() )
	end
	
	if( self._post_load_alpha ) then
		self._bitmap:set_color( Color.white:with_alpha( self._post_load_alpha ) )
		self._bitmap:set_blend_mode( self._post_load_blend_mode or "normal" )
		for _, bitmap in pairs( self._extra_textures ) do
			bitmap:set_color( bitmap:color():with_alpha( self._post_load_alpha ) )
			bitmap:set_blend_mode( self._post_load_blend_mode or "normal" )
		end
		
		self._post_load_alpha = nil
		self._post_load_blend_mode = nil
	end
	
	self._loading_texture = nil
	self._request_index = nil

	self:set_highlight( self._highlighted, true )
	if( self._selected ) then
		self:select(true)
	else
		self:deselect(true)
	end
	self:refresh()
end

function BlackMarketGuiSlotItem:set_btn_text( text )
--[[
	if( text and self._conflict ) then
		self._panel:child( "btn_text" ):set_text( utf8.to_upper( managers.localization:text( "bm_menu_conflict", { conflict=self._conflict } ) ) )
		self._panel:child( "btn_text" ):set_color( tweak_data.screen_color_red )
		return
	end
	if( text and self._level_req ) then
		self._panel:child( "btn_text" ):set_text( utf8.to_upper( managers.localization:text( (self._level_req == 0 and "bm_menu_skilltree_locked" or "bm_menu_level_req"), { level=self._level_req } ) ) )
		self._panel:child( "btn_text" ):set_color( tweak_data.screen_color_red )
		return 
	end
	if( text and self._item_req ) then
		self._panel:child( "btn_text" ):set_text( utf8.to_upper( managers.localization:text( "bm_menu_no_items" ) ) )
		self._panel:child( "btn_text" ):set_color( tweak_data.screen_color_red )
		return 
	end
	
	self._panel:child( "btn_text" ):set_color( tweak_data.screen_color_white )
	
	self._panel:child( "btn_text" ):set_text( text or "" )
	
	]]
end
	
function BlackMarketGuiSlotItem:set_highlight( highlight, instant )
	-- BlackMarketGuiSlotItem.super.set_highlight( self, highlight )
	
	if( self._bitmap and not self._loading_texture ) then
		
		if highlight then
			local animate_select = function(o, panel, instant, width, height)
				local w = o:w()
				local h = o:h()
				local end_w = width * 0.85
				local end_h = height * 0.85
				local center_x, center_y = o:center()
				if( (w == end_w) or instant ) then
					o:set_size( end_w, end_h )
					o:set_center( center_x, center_y )
					return
				end
				
				over( math.abs(end_w-w)/end_w, function(p) o:set_size( math.lerp( w, end_w, p ), math.lerp( h, end_h, p ) ) o:set_center( center_x, center_y ) end )
			end
			
			local w, h = self:get_texure_size()
			self._bitmap:stop()
			self._bitmap:animate( animate_select, self._panel, instant, w, h )
			
			for _, bitmap in pairs( self._extra_textures ) do
				bitmap:stop()
				bitmap:animate( animate_select, self._panel, instant, w, h )
			end
		else
			local animate_deselect = function(o, panel, instant, width, height)
				local w = o:w()
				local h = o:h()
				local end_w = width * 0.65
				local end_h = height * 0.65
				local center_x, center_y = o:center()
				if( (w == end_w) or instant ) then
					o:set_size( end_w, end_h )
					o:set_center( center_x, center_y )
					return
				end
				
				over( math.abs(end_w-w)/end_w, function(p) o:set_size( math.lerp( w, end_w, p ), math.lerp( h, end_h, p ) ) o:set_center( center_x, center_y ) end )
			end
			
			local w, h = self:get_texure_size()
			self._bitmap:stop()
			self._bitmap:animate( animate_deselect, self._panel, instant, w, h )
			for _, bitmap in pairs( self._extra_textures ) do
				bitmap:stop()
				bitmap:animate( animate_deselect, self._panel, instant, w, h )
			end
		end
	end
end

function BlackMarketGuiSlotItem:select( instant, no_sound )
	BlackMarketGuiSlotItem.super.select( self, instant, no_sound )
	
	if not managers.menu:is_pc_controller() then	
		self:set_highlight( true, instant )
	end
	if self._text_in_mid and alive(self._panel:child("text")) then
		self._panel:child("text"):set_color( self._data.mid_text.selected_color or Color.white )
		self._panel:child("text"):set_text( utf8.to_upper( self._data.mid_text.selected_text or "" ) )
	end
	
	if self._data.new_drop_data then
		local newdrop = self._data.new_drop_data
		
		if newdrop[1] and newdrop[2] and newdrop[3] then
			managers.blackmarket:remove_new_drop( newdrop[1], newdrop[2], newdrop[3] )
			-- if not managers.blackmarket:got_new_drop( unpack( newdrop[2] ) ) then
				if newdrop.icon then
					newdrop.icon:parent():remove( newdrop.icon )
				end
				self._data.new_drop_data = nil
			-- end
		end
	end
	
--[[
	if( self._bitmap and not self._loading_texture ) then
		local animate_select = function(o, panel, instant, width, height)
			local w = o:w()
			local h = o:h()
			local end_w = width * 0.95
			local end_h = height * 0.95
			local center_x, center_y = o:center()
			if( (w == end_w) or instant ) then
				o:set_size( end_w, end_h )
				o:set_center( center_x, center_y )
				return
			end
			
			over( math.abs(end_w-w)/end_w, function(p) o:set_size( math.lerp( w, end_w, p ), math.lerp( h, end_h, p ) ) o:set_center( center_x, center_y ) end )
		end
		
		local w, h = self:get_texure_size()
		self._bitmap:stop()
		self._bitmap:animate( animate_select, self._panel, instant, w, h )
		
		for _, bitmap in pairs( self._extra_textures ) do
			bitmap:stop()
			bitmap:animate( animate_select, self._panel, instant, w, h )
		end
	end
	]]
	return self
end

function BlackMarketGuiSlotItem:deselect( instant )
	BlackMarketGuiSlotItem.super.deselect( self, instant )
	
	if not managers.menu:is_pc_controller() then	
		self:set_highlight( false, instant )
	end
	if self._text_in_mid and alive(self._panel:child("text")) then
		self._panel:child("text"):set_color( self._data.mid_text.noselected_color or Color.white )
		self._panel:child("text"):set_text( utf8.to_upper( self._data.mid_text.noselected_text or "" ) )
	end
	
	--[[
	if( self._comparision_panel ) then
		self._damage:set_color( tweak_data.screen_color_light_grey )
		self._spread:set_color( tweak_data.screen_color_light_grey )
		self._recoil:set_color( tweak_data.screen_color_light_grey )
		self._suppression:set_color( tweak_data.screen_color_light_grey )
		self._concealment:set_color( tweak_data.screen_color_light_grey )
		self._alert_size:set_color( tweak_data.screen_color_light_grey )
	end
	
	if( self._bitmap and not self._loading_texture ) then
		local animate_deselect = function(o, panel, instant, width, height)
			local w = o:w()
			local h = o:h()
			local end_w = width * 0.8
			local end_h = height * 0.8
			local center_x, center_y = o:center()
			if( (w == end_w) or instant ) then
				o:set_size( end_w, end_h )
				o:set_center( center_x, center_y )
				return
			end
			
			over( math.abs(end_w-w)/end_w, function(p) o:set_size( math.lerp( w, end_w, p ), math.lerp( h, end_h, p ) ) o:set_center( center_x, center_y ) end )
		end
		
		local w, h = self:get_texure_size()
		self._bitmap:stop()
		self._bitmap:animate( animate_deselect, self._panel, instant, w, h )
		for _, bitmap in pairs( self._extra_textures ) do
			bitmap:stop()
			bitmap:animate( animate_deselect, self._panel, instant, w, h )
		end
	end
	]]
end

function BlackMarketGuiSlotItem:refresh()
	BlackMarketGuiSlotItem.super.refresh( self )
	if( self._bitmap ) then
		self._bitmap:set_alpha( 1 )
		for _, bitmap in pairs( self._extra_textures ) do
			bitmap:set_alpha( 1 )
		end
	end
end

BlackMarketGuiMaskSlotItem = BlackMarketGuiMaskSlotItem or class ( BlackMarketGuiSlotItem )

function BlackMarketGuiMaskSlotItem:init( main_panel, data, x, y, w, h )
	BlackMarketGuiMaskSlotItem.super.init( self, main_panel, data, x, y, w, h )
	
	-- print( inspect( data.my_part_data ), inspect( data.my_true_part_data ) )
	
	-- Idstring( "VertexColorTexturedPatterns" )
	
	local cx, cy = self._panel:w()/2, self._panel:h()/2
	-- self._panel:set_debug(true)
	
	self._box_panel = self._panel:panel( { w=self._panel:w()*0.5, h=self._panel:w()*0.5 } )
	self._box_panel:set_center( cx, cy )
	
	if not data.my_part_data.is_good then
		BoxGuiObject:new( self._box_panel, { sides = { 1, 1, 1, 1 } } )
	end
	
	self._mask_text = self._panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size } )
	self._mask_text:set_position( self._box_panel:left(), self._box_panel:bottom() + 10 )
	self._mask_text:set_text( utf8.to_upper( data.name_localized .. ": " ) )
	BlackMarketGui.make_fine_text( self, self._mask_text )

	self._mask_name_text = self._panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, wrap=true, word_wrap=true } )
	self._mask_name_text:set_position( self._mask_text:right(), self._mask_text:top() )
	self._mask_name_text:set_text( data.my_part_data.is_good and managers.localization:text(data.my_part_data.text) or "NOT SELECTED" )
	self._mask_name_text:set_blend_mode( data.my_part_data.is_good and "normal" or "add" )
	self._mask_name_text:set_color( data.my_part_data.is_good and tweak_data.screen_colors.text or tweak_data.screen_colors.important_1 )
	self._mask_name_text:grow( -self._mask_name_text:x()-5, 0 )
	
	local _, _, _, texth = self._mask_name_text:text_rect()
	
	if data.my_part_data.override then
		self._mask_error_text = self._panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, wrap=true, word_wrap=true, color=tweak_data.screen_colors.important_1, blend_mode="add" } )
		self._mask_error_text:set_position( self._mask_text:left(), self._mask_text:top() + texth )
		self._mask_error_text:set_text( managers.localization:to_upper_text( "menu_bm_overwrite" , { category=managers.localization:text( "bm_menu_"..data.my_part_data.override ) } ) )
	end
	
	local current_match_with_true = true -- (not data.my_part_data.is_good) and (not data.my_true_part_data)
	if data.my_part_data.is_good and data.my_true_part_data then
		current_match_with_true = data.my_part_data.id == data.my_true_part_data.id
		-- print(data.my_part_data.id , data.my_true_part_data.id)
	end
	
	if not current_match_with_true then
		if self._bitmap then
			self._bitmap:set_color( Color.white:with_alpha(0.3) )
			for _, bitmap in pairs( self._extra_textures ) do
				bitmap:set_color( bitmap:color():with_alpha( 0.3 ) )
			end
		end
		if self._loading_texture then
			self._post_load_alpha = 0.3
		end
		
		self._mask_text:set_color( self._mask_text:color():with_alpha( 0.5 ) )
		self._mask_name_text:set_color( self._mask_name_text:color():with_alpha( 0.5 ) )
		if self._mask_error_text then
			self._mask_error_text:set_color( self._mask_error_text:color():with_alpha( 0.5 ) )
		end
	end
	
	self:deselect( true )
	self:set_highlight( false, true )
end

function BlackMarketGuiMaskSlotItem:set_highlight( highlight, instant )
	if( self._bitmap and not self._loading_texture ) then
		
		if highlight then
			local animate_select = function(o, panel, instant, width, height)
				local w = o:w()
				local h = o:h()
				local end_w = width * 0.55
				local end_h = height * 0.55
				local center_x, center_y = o:center()
				if( (w == end_w) or instant ) then
					o:set_size( end_w, end_h )
					o:set_center( center_x, center_y )
					return
				end
				
				over( math.abs(end_w-w)/end_w, function(p) o:set_size( math.lerp( w, end_w, p ), math.lerp( h, end_h, p ) ) o:set_center( center_x, center_y ) end )
			end
			
			local w, h = self:get_texure_size()
			self._bitmap:stop()
			self._bitmap:animate( animate_select, self._panel, instant, w, h )
			
			for _, bitmap in pairs( self._extra_textures ) do
				bitmap:stop()
				bitmap:animate( animate_select, self._panel, instant, w, h )
			end
		else
			local animate_deselect = function(o, panel, instant, width, height)
				local w = o:w()
				local h = o:h()
				local end_w = width * 0.45
				local end_h = height * 0.45
				local center_x, center_y = o:center()
				if( (w == end_w) or instant ) then
					o:set_size( end_w, end_h )
					o:set_center( center_x, center_y )
					return
				end
				
				over( math.abs(end_w-w)/end_w, function(p) o:set_size( math.lerp( w, end_w, p ), math.lerp( h, end_h, p ) ) o:set_center( center_x, center_y ) end )
			end
			
			local w, h = self:get_texure_size()
			self._bitmap:stop()
			self._bitmap:animate( animate_deselect, self._panel, instant, w, h )
			for _, bitmap in pairs( self._extra_textures ) do
				bitmap:stop()
				bitmap:animate( animate_deselect, self._panel, instant, w, h )
			end
		end
	end
end


BlackMarketGuiButtonItem = BlackMarketGuiButtonItem or class( BlackMarketGuiItem )

function BlackMarketGuiButtonItem:init( main_panel, data, x )
	BlackMarketGuiButtonItem.super.init( self, main_panel, data, 0, 0, 10, 10 )
	
	local up_font_size = (NOT_WIN_32 and (RenderSettings.resolution.y<720) and self._data.btn == "BTN_STICK_R") and 2 or 0
	self._btn_text = self._panel:text( { name="text", text="", align="left",  x = 10, font_size=small_font_size+up_font_size, font=small_font, color=tweak_data.screen_colors.button_stage_3, blend_mode="add", layer=1 } )
	self._btn_text_id = data.name
	self._btn_text_legends = data.legends
	
	BlackMarketGui.make_fine_text( self, self._btn_text )
	
	-- local _, _, w, h = self._btn_text:text_rect()
	self._panel:set_size( main_panel:w() - x*2, medium_font_size )
	-- self._btn_text:set_size( w, h )
	self._panel:rect( { name="select_rect", blend_mode="add", color=tweak_data.screen_colors.button_stage_3, alpha=0.3, valign="scale", halign="scale" } )
	
	if not managers.menu:is_pc_controller() then
		-- self._btn_text:set_blend_mode( "normal" )
		self._btn_text:set_color( tweak_data.screen_colors.text )
	end
	
	self._panel:set_left( x )
	self._panel:hide()
	self:set_order( data.prio )
	
	self._btn_text:set_right( self._panel:w() )
	
	self:deselect( true )
	self:set_highlight( false )
end

function BlackMarketGuiButtonItem:hide()
	self._panel:hide()
end

function BlackMarketGuiButtonItem:show()
	self._panel:show()
end

function BlackMarketGuiButtonItem:refresh()
	if managers.menu:is_pc_controller() then
		self._btn_text:set_color( (self._highlighted and tweak_data.screen_colors.button_stage_2) or tweak_data.screen_colors.button_stage_3 )
	end
	
	self._panel:child("select_rect"):set_visible( self._highlighted )
end

function BlackMarketGuiButtonItem:visible()
	return self._panel:visible()
end

function BlackMarketGuiButtonItem:set_order( prio )
	self._panel:set_y( (prio-1) * (small_font_size) )
	-- self._panel:set_world_y( math.round( self._panel:world_y() ) )
end

function BlackMarketGuiButtonItem:set_text_btn_prefix( prefix )
	self._btn_prefix = prefix
end

function BlackMarketGuiButtonItem:set_text_params( params )
	local prefix = self._btn_prefix and managers.localization:get_default_macro( self._btn_prefix ) or ""
	local btn_text = prefix 
	
	if self._btn_text_id then
		btn_text = btn_text .. utf8.to_upper( managers.localization:text(self._btn_text_id, params) )
	end
	
	if self._btn_text_legends then
		local legend_string = ""
		for i, legend in ipairs( self._btn_text_legends ) do
			if i > 1 then
				legend_string = legend_string .. " | "
			end
			legend_string = legend_string .. managers.localization:text( legend )
		end
		btn_text = btn_text .. utf8.to_upper( legend_string )
	end
	
	self._btn_text:set_text( btn_text )
	BlackMarketGui.make_fine_text( self, self._btn_text )
	local _, _, w, h = self._btn_text:text_rect()
	self._panel:set_h( h )
	self._btn_text:set_size( w, h )
	self._btn_text:set_right( self._panel:w() )
	-- self._btn_text:set_selection( 0, #self._btn_text:text() ) -- utf8.len(prefix)
end

function BlackMarketGuiButtonItem:btn_text()
	return self._btn_text:text()
end



BlackMarketGui = BlackMarketGui or class()
BlackMarketGui.identifiers = {
	weapon = Idstring( "weapon" ),
	armor = Idstring( "armor" ),
	mask = Idstring( "mask" ),
	weapon_mod = Idstring( "weapon_mod" ),
	mask_mod = Idstring( "mask_mod" ),
	deployable = Idstring( "deployable" ),
	character = Idstring( "character" ),
}

function BlackMarketGui:init( ws, fullscreen_ws, node )
	managers.menu:active_menu().renderer.ws:hide()
	
	self._ws = ws
	self._fullscreen_ws = fullscreen_ws
	self._init_layer = self._ws:panel():layer()
	
	self._node = node
	local component_data = self._node:parameters().menu_component_data
	local do_animation = not component_data and not self._data
	local is_start_page = (not component_data) and true or false
	
	managers.menu_component:close_contract_gui()
	self:_setup( is_start_page, component_data )
	
	if do_animation then
		managers.menu_component:test_camera_shutter_tech()
		local fade_me_in_scotty = function(o)
			over(0.1, function(p) o:set_alpha(p) end)
		end
		self._panel:animate( fade_me_in_scotty )
		self._fullscreen_panel:animate( fade_me_in_scotty )
	end
end

function BlackMarketGui:set_layer( layer )
	self._panel:set_layer( self._init_layer + layer )
end

function BlackMarketGui:make_fine_text( text )
	local x,y,w,h = text:text_rect()
	text:set_size( w, h )
	text:set_position( math.round( text:x() ), math.round( text:y() ) )
end

function BlackMarketGui:_setup( is_start_page, component_data )
	if alive( self._panel ) then
		self._ws:panel():remove( self._panel )
	end
	
	self._panel = self._ws:panel():panel( {} )
	self._fullscreen_panel = self._fullscreen_ws:panel():panel( { layer=40 } )
	self:set_layer( 45 )
	
	-- self._panel:text( { name="wip1", text="WIP", align="left", vertical="top", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=1 } )
	-- self._panel:text( { name="wip2", text="WIP", align="right", vertical="top", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=1 } )
	-- self._panel:text( { name="wip3", text="WIP", align="left", vertical="bottom", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=1 } )
	-- self._panel:text( { name="wip4", text="WIP", align="right", vertical="bottom", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=1 } )
	
	WalletGuiObject.set_wallet( self._panel )
	-- self._wallet = WalletGuiObject:new( self._panel )
	--[[ W A L L E T ]]
	--[[
	do
		local money_icon = self._panel:bitmap( { name = "wallet_money_icon", texture="guis/textures/pd2/shared_wallet_symbol" } )
		local level_icon = self._panel:bitmap( { name = "wallet_level_icon", texture="guis/textures/pd2/shared_level_symbol" } )
	
		local money_text = self._panel:text( { name="wallet_money_text", text=managers.money:total_string(), font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_colors.text, vertical="center" } )
		local level_text = self._panel:text( { name="wallet_level_text", text=tostring(managers.experience:current_level()), font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_colors.text, vertical="center" } )
	
		money_icon:set_leftbottom( 2, self._panel:h() - 2 )
		level_icon:set_leftbottom( 2, money_icon:top() - 2 )
		
		money_text:set_left( money_icon:right() + 2 )
		money_text:set_center_y( money_icon:center_y() )
		
		level_text:set_left( level_icon:right() + 2 )
		level_text:set_center_y( level_icon:center_y() )
	end
	]]
	--[[ W A L L E T ]]
	
	self._data = component_data or self:_start_page_data()
	self._node:parameters().menu_component_data = self._data
	
	self._data.blur_fade = self._data.blur_fade or 0
	
	local blur = self._fullscreen_panel:bitmap( { texture="guis/textures/test_blur_df", w=self._fullscreen_ws:panel():w(), h=self._fullscreen_ws:panel():h(), render_template="VertexColorTexturedBlur3D", layer=-1 } )
	local func = function(o, component_data)
		local start_blur = component_data.blur_fade
		over( 0.6 - (0.6*component_data.blur_fade), function(p) component_data.blur_fade = math.lerp( start_blur, 1, p ) o:set_alpha( component_data.blur_fade ) end)
	end	
	blur:animate( func, self._data )
	
	self._panel:text( { name="back_button", text=utf8.to_upper( managers.localization:text("menu_back") ), align="right", vertical="bottom", font_size=large_font_size, font=large_font, color=tweak_data.screen_colors.button_stage_3 } )
	self:make_fine_text( self._panel:child("back_button") )
	self._panel:child("back_button"):set_right( self._panel:w() )
	self._panel:child("back_button"):set_bottom( self._panel:h() )
	self._panel:child("back_button"):set_visible( managers.menu:is_pc_controller() )
	
	self._pages = ( #self._data > 1 ) or self._data.show_tabs
	
	-- ( ( parent_panel:h() - 70 ) / 8 )
	
	local grid_panel_w = self._panel:w() * WIDTH_MULTIPLIER -- 270 * 3 + 5 * 4 -- self._panel:w() * 0.85
	local grid_panel_h = ( ( self._panel:h() - (medium_font_size + 10) - 70 ) * GRID_H_MUL ) -- (self._panel:h() * 0.5) -- (270 * 3) / 2 + 5 * 4 -- self._panel:h() * 0.75
	
	local square_w = grid_panel_w / 3
	local square_h = grid_panel_h / 3
	local padding_w = 0 -- 5 -- ( grid_panel_w - ( square_w * 3 ) ) / 4
	local padding_h = 0 -- 5 -- ( grid_panel_h - ( square_h * 3 ) ) / 4
	local left_padding = 0 -- 30
	local top_padding = 70

	local size_data = {}
	size_data.grid_w = grid_panel_w
	size_data.grid_h = grid_panel_h
	
	size_data.square_w = square_w
	size_data.square_h = square_h
	size_data.padding_w = padding_w
	size_data.padding_h = padding_h
	size_data.left_padding = left_padding
	size_data.top_padding = top_padding
	
	-- self._panel:set_debug(true)
	
	-- self._fullscreen_panel:bitmap( { name="bg", texture="guis/textures/pd2/blackmarket/bg", w=self._fullscreen_panel:w(), h=self._fullscreen_panel:h(), blend_mode="normal", layer=1 } )

	self._inception_node_name = self._node:parameters().menu_component_next_node_name or "blackmarket_node"
	self._preview_node_name = self._node:parameters().menu_component_preview_node_name or "blackmarket_preview_node"
	
	self._tabs = {}
	self._btns = {}
	
	self._title_text = self._panel:text( { name="title_text", text=utf8.to_upper( managers.localization:text( self._data.topic_id, self._data.topic_params ) ), font_size=large_font_size, font=large_font, color=tweak_data.screen_colors.text, } )
	self:make_fine_text(self._title_text)
	-- self._title_text:move( 139, top_padding )
	
	-- local _, _, text_width, _ = self._title_text:text_rect()
	-- local tab_x = self._title_text:x() + text_width + 45
	
	self._tab_scroll_panel = self._panel:panel( { w=grid_panel_w, y=top_padding } )
	-- self._tab_scroll_panel:set_debug(true)
	
	self._tab_scroll_table = { panel = self._tab_scroll_panel }
	
	for i, data in ipairs( self._data ) do
		if( data.on_create_func_name ) then
			data.on_create_func = callback( self, self, data.on_create_func_name )
		end
		local new_tab = BlackMarketGuiTabItem:new( self._panel, data, self._node, size_data, not self._pages, self._tab_scroll_table )
		
		table.insert( self._tabs, new_tab )
	end
	
	if self._data.open_callback_name then
		local clbk_func = callback( self, self, self._data.open_callback_name, self._data.open_callback_params )
		if clbk_func then
			clbk_func()
		end
	end
	
	--[[
	if( self._pages ) then
		local index = 0
		for i, data in ipairs( self._data ) do
			if( data.on_create_func_name ) then
				data.on_create_func = callback( self, self, data.on_create_func_name )
			end
			local new_tab = BlackMarketGuiTabItem:new( self._panel, data, self._node, size_data, nil, self._tab_scroll_table )
			-- tab_x = new_tab:set_tab_position( tab_x )
			table.insert( self._tabs, new_tab )
			
			index = index + 1
		end
	else -- if( #self._data > 0 ) then
		if( self._data[1].on_create_func_name ) then
			self._data[1].on_create_func = callback( self, self, self._data[1].on_create_func_name )
		end
		local new_tab = BlackMarketGuiTabItem:new( self._panel, self._data[1], self._node, size_data, true, self._tab_scroll_table )
		-- new_tab:set_tab_position( tab_x )
		new_tab._tab_panel:child( "tab_text" ):hide()
		table.insert( self._tabs, new_tab )
	end
	]]
	-- self._title_text:set_left( self._tabs[1]._grid_panel:x() + 5 )
	
	self._selected = self._node:parameters().menu_component_selected or 1
	
	self._select_rect = self._panel:panel( { name="select_rect", w=square_w, h=square_h, layer=8 } ) -- bitmap( { name="select_rect", w=square_w+6, h=square_h+6, layer=8, texture="guis/textures/pd2/blackmarket/select_box" } )  -- self._panel:polyline( { name="select_rect", color=Color.yellow, points={ Vector3( 1, 1, 0 ), Vector3( square_w-1, 1, 0 ), Vector3( square_w-1, square_h-1, 0 ), Vector3( 1, square_h-1, 0 ) }, w=square_w, h=square_h, layer=5, closed=true } ) 
	if( self._tabs[ self._selected ] ) then
		self._tabs[ self._selected ]:select( true )
		
		local slot_dim_x = self._tabs[self._selected].my_slots_dimensions[1]
		local slot_dim_y = self._tabs[self._selected].my_slots_dimensions[2]
		
		self._select_rect:set_size( square_w * 3 / slot_dim_x, square_h * 3 / slot_dim_y )
		self._select_rect_box = BoxGuiObject:new( self._select_rect, { sides = { 2, 2, 2, 2 } } )
		
		self._box_panel = self._panel:panel()
		self._box_panel:set_shape( self._tabs[ self._selected ]._grid_panel:shape() )
		self._box = BoxGuiObject:new( self._box_panel, { sides = { 1, 1, 1 + (#self._tabs > 1 and 1 or 0), 1 } } )
		
		local info_box_panel = self._panel:panel( { name="info_box_panel" } )
		info_box_panel:set_size( self._panel:w()*(1-WIDTH_MULTIPLIER)-BOX_GAP, self._box_panel:h() )
		info_box_panel:set_right( self._panel:w() )
		info_box_panel:set_top( self._box_panel:top() )
		--[[
		info_box_panel:set_size( self._panel:w()*(1-WIDTH_MULTIPLIER)-BOX_GAP, self._panel:h() - ( NOT_WIN_32 and 20 or 90 ) )
		info_box_panel:set_right( self._panel:w() )
		info_box_panel:set_top( 20 )]]
		
		
		self._info_box = BoxGuiObject:new( info_box_panel, { sides = { 1, 1, 1, 1 } } )
		
		self._selected_slot = self._tabs[ self._selected ]:select_slot( nil, true )
		self._slot_data = self._selected_slot._data
		local x, y = self._tabs[ self._selected ]:selected_slot_center()
		
		self._select_rect:set_world_center( x, y )
		local BTNS =
		{
			-- weapon slot selected
			w_mod 		= { prio=2, btn="BTN_Y", 			pc_btn=Idstring("menu_modify_item"), name="bm_menu_btn_mod",			callback=callback( self, self, "choose_weapon_mods_callback" ) }, -- "choose_mod_type_callback"
			w_equip		= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_equip_weapon",		callback=callback( self, self, "equip_weapon_callback" ) },
			w_preview = { prio=3, btn="BTN_STICK_R",		pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview",	callback=callback( self, self, "preview_weapon_callback" ) },
			w_sell 		= { prio=4, btn="BTN_X", 			pc_btn=Idstring("menu_remove_item"), name="bm_menu_btn_sell",		callback=callback( self, self, "sell_item_callback" ) },
			
			-- empty weapon slot selected
			ew_buy 		= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_buy_new_weapon", 		callback=callback( self, self, "choose_weapon_buy_callback" ) },
			
			-- buy weapon slot selected
			bw_buy		= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_buy_selected_weapon", 		callback=callback( self, self, "buy_weapon_callback" ) },
			bw_preview= { prio=2, btn="BTN_STICK_R", 		pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview", callback=callback( self, self, "preview_buy_weapon_callback" ) },
			
			-- mod typ slot selected
			mt_choose	= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_choose",	callback=callback( self, self, "choose_mod_callback" ) },
		
			-- weapon mod slot selected 
			wm_buy 		= { prio=1, btn="BTN_A", 					pc_btn=nil, name="bm_menu_btn_craft_mod", 		callback=callback( self, self, "buy_mod_callback" ) },
			wm_preview = { prio=3, btn="BTN_STICK_R", 			pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview",	callback=callback( self, self, "preview_weapon_callback" ) },
			wm_preview_mod	= { prio=2, btn="BTN_Y", 		pc_btn=Idstring("menu_preview_item_alt"), name="bm_menu_btn_preview_with_mod",	callback=callback( self, self, "preview_weapon_with_mod_callback" ) },
			wm_remove_buy = { prio=1, btn="BTN_X",			pc_btn=Idstring("menu_remove_item"), name="bm_menu_btn_remove_mod", 		callback=callback( self, self, "remove_mod_callback" ) },
			wm_remove_preview_mod = { prio=2, btn="BTN_Y", 		pc_btn=Idstring("menu_preview_item_alt"), name="bm_menu_btn_preview_with_mod",	callback=callback( self, self, "preview_weapon_callback" ) },
			wm_remove_preview = { prio=3, btn="BTN_STICK_R",	pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview_no_mod",	callback=callback( self, self, "preview_weapon_without_mod_callback" ) },
			
			a_equip = { prio=1, btn="BTN_A", 				pc_btn=nil, name="bm_menu_btn_equip_armor",		callback=callback( self, self, "equip_armor_callback" ) },
			
			-- mask slot selected
			m_equip		= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_equip_mask",		callback=callback( self, self, "equip_mask_callback" ) },
			m_mod			= { prio=2, btn="BTN_Y", 			pc_btn=Idstring("menu_modify_item"), name="bm_menu_btn_mod_mask",			callback=callback( self, self, "mask_mods_callback" ) },
			m_preview = { prio=3, btn="BTN_STICK_R", 		pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview_mask",	callback=callback( self, self, "preview_mask_callback" ) },
			m_sell		= { prio=4, btn="BTN_X", 			pc_btn=Idstring("menu_remove_item"), name="bm_menu_btn_sell_mask", 		callback=callback( self, self, "sell_mask_callback" ) },
			
			-- empty mask slot selected // global value
			em_gv 		= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_buy_new_mask", 	callback=callback( self, self, "choose_mask_global_value_callback" ) },
			em_buy 		= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_buy_new_mask", 		callback=callback( self, self, "choose_mask_buy_callback" ) },
			
			-- mask mod slot selected 
			mm_choose_textures	= { prio=1, btn="BTN_A",	pc_btn="", name="bm_menu_choose_pattern",	callback=callback( self, self, "choose_mask_mod_callback", "textures" ) },
			mm_choose_materials	= { prio=2, btn="BTN_A",	pc_btn="", name="bm_menu_choose_material",	callback=callback( self, self, "choose_mask_mod_callback" , "materials" ) },
			mm_choose_colors	= { prio=3, btn="BTN_A", 		pc_btn="", name="bm_menu_choose_color",	callback=callback( self, self, "choose_mask_mod_callback", "colors" ) },
			mm_choose	= { prio=1, btn="BTN_A",						pc_btn=nil, name="bm_menu_btn_choose_mask_mod",	callback=callback( self, self, "choose_mask_type_callback" ) },
			
			-- mask options slot selected 			-- ADDED to mask mod slot selected 
			mm_buy	= { prio=5, btn="BTN_Y",				pc_btn=Idstring("menu_modify_item"), name="bm_menu_btn_customize_mask",	callback=callback( self, self, "buy_customized_mask_callback" ) },
			mm_preview	= { prio=4, btn="BTN_STICK_R",	pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview_mask",	callback=callback( self, self, "preview_customized_mask_callback" ) },
			
			-- mask mod type slot selected 
			mp_choose	= { prio=1, btn="BTN_A",			pc_btn=nil, name="bm_menu_btn_choose_mask_mod",	callback=callback( self, self, "choose_mask_part_callback" ) },
			mp_preview	= { prio=2, btn="BTN_STICK_R",	pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview_mask",	callback=callback( self, self, "preview_customized_mask_callback" ) },
			mp_preview_mod	= { prio=2, btn="BTN_STICK_R",	pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview_mask",	callback=callback( self, self, "preview_customized_mask_with_mod_callback" ) },
	
			-- buy mask slot selected
			bm_buy		= { prio=1, btn="BTN_A",			pc_btn=nil, name="bm_menu_btn_buy_selected_mask", 		callback=callback( self, self, "buy_mask_callback" ) },
			bm_preview = { prio=2, btn="BTN_STICK_R", 	pc_btn=Idstring("menu_preview_item"), name="bm_menu_btn_preview_mask",	callback=callback( self, self, "preview_buy_mask_callback" ) },
			bm_sell		= { prio=3, btn="BTN_X", 			pc_btn=Idstring("menu_remove_item"), name="bm_menu_btn_sell_mask", 		callback=callback( self, self, "sell_stashed_mask_callback" ) },
			
			-- character
			c_equip = { prio=1, btn="BTN_A", 				pc_btn=nil, name="bm_menu_btn_set_preferred", callback=callback( self, self, "set_preferred_character_callback" ) },
			
			-- loadout weapon slot selected
			lo_w_equip= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_equip_weapon", 	callback=callback( self, self, "equip_weapon_callback" ) },
			
			-- loadout deployable slot selected
			lo_d_equip= { prio=1, btn="BTN_A", 			pc_btn=nil, name="bm_menu_btn_equip_deployable", 	callback=callback( self, self, "lo_equip_deployable_callback" ) },
			
			
			-- controller back button legend
			back_btn = { prio=99, btn="BTN_B", no_btn=true, legends={"menu_legend_preview_move", "menu_legend_back"} }
		}--  
		-- e="$STICK_L;Move | $BTN_B;Back"/>
		local get_real_font_sizes = false
		local real_small_font_size = small_font_size
		if get_real_font_sizes then
			local test_text = self._panel:text( { font=small_font, font_size=small_font_size, text="TeWqjI-"..managers.localization:get_default_macro("BTN_BOTTOM_L"), visible=false } )
			local x, y, w, h = test_text:text_rect()
			real_small_font_size = h
			self._panel:remove( test_text )
			test_text = nil
		end
		self._real_small_font_size = real_small_font_size
		
		local real_medium_font_size = medium_font_size
		if get_real_font_sizes then
			local test_text = self._panel:text( { font=medium_font, font_size=medium_font_size, text="TeWqjI-"..managers.localization:get_default_macro("BTN_BOTTOM_L"), visible=false } )
			local x, y, w, h = test_text:text_rect()
			real_medium_font_size = h
			-- self._panel:remove( test_text )
			-- test_text = nil
			Global.test_text = test_text
		end
		self._real_medium_font_size = real_medium_font_size
		
		
		self._visibility_panel = self._panel:panel( { name="visibility_panel" } )
		self._visibility_panel:set_w( info_box_panel:w() )
		self._visibility_panel:set_h( self._real_small_font_size * 2)
		self._visibility_panel:set_leftbottom( info_box_panel:position() )
		-- self._visibility_panel:set_debug(true)
		
		
		local custom_data = {}
		
		if self._data.open_callback_params then
			-- print( inspect( self._data.open_callback_params  ) )
			custom_data[self._data.open_callback_params.category] = managers.blackmarket:get_crafted_category_slot( self._data.open_callback_params.category, self._data.open_callback_params.slot )
		end
		
		local suspicion_index = managers.blackmarket:get_real_concealment_index_from_custom_data( custom_data )
		suspicion_index = math.clamp( suspicion_index, 1, #tweak_data.weapon.stats.concealment )
		local visibility_index = 1
		local visibility_id = "" -- tweak_data.gui.suspicion_to_visibility -- [ suspicion_index ]
		local visibility_position = 0
		local min_index = 0
		for i, visibility in ipairs( tweak_data.gui.suspicion_to_visibility ) do
			if suspicion_index <= visibility.max_index then
				visibility_index = i
				visibility_id = visibility.name_id
				
				local a = visibility.max_index - min_index
				local b = suspicion_index - min_index
				
				visibility_position = b / a
				break
			end
			min_index = visibility.max_index - 1
		end
		
		local visibility_text = self._visibility_panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, text=managers.localization:to_upper_text("bm_menu_concealment") } )
		self:make_fine_text( visibility_text )
		visibility_text:set_left( 0 )
		visibility_text:set_bottom( self._visibility_panel:h() )
		
		local profile_text = nil
		local largest_profile_w = 0
		for i, visibility in ipairs( tweak_data.gui.suspicion_to_visibility ) do
			local new_profile_text = self._visibility_panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, text=managers.localization:to_upper_text(visibility.name_id), visible=false } )
			self:make_fine_text( new_profile_text )
			new_profile_text:set_right( self._visibility_panel:w() )
			new_profile_text:set_bottom( visibility_text:bottom() )
			
			largest_profile_w = math.max( largest_profile_w, new_profile_text:w() )
			if i == visibility_index then
				profile_text = new_profile_text
				profile_text:show()
			end
		end
		
		local visibility_desc_text = self._visibility_panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, text=managers.localization:text("bm_menu_concealment_desc"), rotation=360 } )
		self:make_fine_text( visibility_desc_text )
		visibility_desc_text:set_top( 0 )
		visibility_desc_text:set_right( self._visibility_panel:w() )
		visibility_desc_text:hide()
		
		local visibility_bar_panel = self._visibility_panel:panel( {name="bar"} )
		visibility_bar_panel:grow( -( visibility_text:w() + largest_profile_w + 8 ), -( visibility_desc_text:h() + 4 ) )
		visibility_bar_panel:set_left( visibility_text:right() + 4 )
		visibility_bar_panel:set_bottom( visibility_text:bottom() - 3 )
		
		local num_bars = #tweak_data.gui.suspicion_to_visibility
		local bar_w = ( visibility_bar_panel:w() - (num_bars-1) * 2 ) / num_bars
		local visibility_white_color = Color( 255, 134, 155, 179 )/255
		local visibility_black_color = Color( 255, 40, 43, 49 )/255
		for i=1, num_bars do
			local color = visibility_black_color
			if i<=visibility_index then
				color = visibility_white_color
			end
			local bar = visibility_bar_panel:rect( { name=tostring(i), color=color, blend_mode="add", alpha=0.5, layer=1, w=bar_w, x=(i-1)*(bar_w+2) } )
			bar:grow( -2, -2 )
			bar:move( 1, 1 )
			
			visibility_bar_panel:rect( { color=color, blend_mode="add", alpha=1, layer=0, w=bar_w, h=1, x=(i-1)*(bar_w+2) } )
			visibility_bar_panel:rect( { color=color, blend_mode="add", alpha=1, layer=0, w=bar_w, h=1, x=(i-1)*(bar_w+2), y=visibility_bar_panel:h()-1 } )
			visibility_bar_panel:rect( { color=color, blend_mode="add", alpha=1, layer=0, w=1, h=visibility_bar_panel:h()-2, x=(i-1)*(bar_w+2), y=1 } )
			visibility_bar_panel:rect( { color=color, blend_mode="add", alpha=1, layer=0, w=1, h=visibility_bar_panel:h()-2, x=(i-1)*(bar_w+2)+bar_w-1, y=1 } )
		end
		local current_visibility_bar = visibility_bar_panel:child( tostring(visibility_index) )
		local visiblity_line = visibility_bar_panel:rect( { name="line", color=Color.white, alpha=1, w=2, h=visibility_bar_panel:h(), y=0, blend_mode="add", layer=2 } )
		visiblity_line:set_center_x( current_visibility_bar:left() + current_visibility_bar:w() * visibility_position )
		
		self._visibility_diff_bar = visibility_bar_panel:rect( { color=Color.white, blend_mode="add", alpha=0.25, w=0 } )
		self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		
		
		self._btn_panel = self._panel:panel( { name="btn_panel", w=info_box_panel:w()-4, h=20 * 5 } )
		-- self._btn_panel:rect( { color=Color.black:with_alpha( 0.3 ) } )
		-- self._btn_panel:set_lefttop( self._tabs[ self._selected ]._grid_panel:x(), self._tabs[ self._selected ]._grid_panel:bottom() )
		self._btn_panel:set_rightbottom( self._panel:w()-2, info_box_panel:bottom()-2 )
		
		
		local btn_x = 10 -- (self._btn_panel:w()/3)*2
		-- local btn_text = self._btn_panel:text( { name="btn_text", text=managers.localization:text("menu_options")..":", align="right", font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=1 } )
		-- local btn_line = self._btn_panel:rect( { name="btn_line", w=2, x=(self._btn_panel:w()/3)*2 } )
		
		-- btn_text:set_right( btn_x - 5 )
		
		for btn, btn_data in pairs( BTNS ) do
			local new_btn = BlackMarketGuiButtonItem:new( self._btn_panel, btn_data, btn_x )
			self._btns[ btn ] = new_btn
		end
		
		-- self:show_btns( self._selected_slot )
		
		-- local money_text = self._panel:text( { name="money_text", text=managers.localization:text("menu_wallet", { money=managers.money:total_string() }), font_size=tweak_data.menu.pd2_small_font_size, font=tweak_data.menu.pd2_small_font, color=tweak_data.screen_color_white, layer=1 } )
		
		-- self:make_fine_text( money_text )
		-- money_text:set_world_right( btn_text:world_right() )
		-- money_text:set_bottom( self._panel:child("back_button"):bottom() )
		
				-- self._comparision_panel:hide()
		
		local h = real_small_font_size
		local longest_text_w = 0
		-- local comparision_h = 120
		
		if( self._data.info_callback ) then
			self._info_panel = self._panel:panel( { name="info_panel", layer=1, w=self._btn_panel:w() } )
			local info_table = self._data.info_callback()
			
			-- local i = 1
			for i, info in ipairs( info_table ) do
				local info_name = info.name or ""
				local info_string = info.text or ""
				local info_color = info.color or tweak_data.screen_colors.text
				
				local category_text = self._info_panel:text( { name="category_"..tostring(i), y=(i-1)*(h), w=0, h=h, font_size=h, font=small_font, layer=1, color=tweak_data.screen_colors.text, text=utf8.to_upper( managers.localization:text( "bm_menu_" .. tostring(info_name) ) ) } )
				local status_text = self._info_panel:text( { name="status_"..tostring(i), y=(i-1)*(h), w=0, h=h, font_size=h, font=small_font, layer=1, color=info_color, text=utf8.to_upper( managers.localization:text( info_string ) ) } )
				-- i = i + 1
				
				if( info_string == "" ) then
					category_text:set_color( info_color )
				end
				
				local _, _, w, _ = category_text:text_rect()
				if( w > longest_text_w ) then
					longest_text_w = w + 10
				end
			end
			
			for name, text in ipairs( self._info_panel:children() ) do
				if( string.split( text:name(), "_" )[1] == "category" ) then
					text:set_w( longest_text_w )
					text:set_x( 0 )
				else
					local _, _, w, _ = text:text_rect()
					text:set_w( w )
					text:set_x( math.round( longest_text_w + 5 ) )
				end
			end
		else
			self._comparision_panel = self._panel:panel( { name="compariaion_panel", layer=1, w=self._btn_panel:w() } )
			self._stats_shown = { "damage", "spread", "spread_moving", "concealment", "suppression", "recoil" }
			--[[
			for i, stat in ipairs( self._stats_shown ) do
				self._comparision_panel:text( { name=stat.."_text", y=(i-1)*(h+2), w=0, h=h, align="right", font_size=h+2, font=tweak_data.menu.pd2_small_font, layer=1, color=tweak_data.screen_colors.text, text=utf8.to_upper( managers.localization:text( "bm_menu_" .. stat ) ) } )
			end
			
			for _, text in ipairs( self._comparision_panel:children() ) do
				local _, _, w, _ = text:text_rect()
				if( w > longest_text_w ) then
					longest_text_w = w + 10
				end
			end
			for _, text in ipairs( self._comparision_panel:children() ) do
				text:set_w( longest_text_w )
			end]]
			
			-- comparision_h = 0
			self._comparision_bitmaps = {}
			for i, stat in ipairs( self._stats_shown ) do
				local y = (i-1) * (h)
				local stats_w = self._comparision_panel:w()/2 - 16
				local text = self._comparision_panel:text( { name=stat.."_text", y=y, h=h, align="right", vertical="center", font_size=small_font_size, font=small_font, layer=1, color=tweak_data.screen_colors.text, text=utf8.to_upper( managers.localization:text( "bm_menu_" .. stat ) ) } )
			
				local bg = self._comparision_panel:rect( { y=y, w=stats_w, h=h-6, layer=0, color=Color( 0.25, 0, 0, 0 ), blend_mode="normal" } )
				local base = self._comparision_panel:rect( { y=y, w=0, h=h-6, layer=1, color=Color(1, 1, 1, 1), blend_mode="normal" } )
				local neg = self._comparision_panel:rect( { y=y, w=0, h=h-6, layer=2, color=Color(1, 0, 0, 0), blend_mode="normal" } )
				local pos = self._comparision_panel:rect( { y=y, w=0, h=h-6, layer=2, color=Color(1, 0.5, 0.5, 0.5), blend_mode="normal" } )
				
				local plus  = self._comparision_panel:bitmap( { y=y, w=8, h=8, layer=3, texture="guis/textures/pd2/blackmarket/stat_plusminus", texture_rect={ 0, 0, 8, 8 } } )
				local minus  = self._comparision_panel:bitmap( { y=y, w=8, h=8, layer=3, texture="guis/textures/pd2/blackmarket/stat_plusminus", texture_rect={ 8, 0, 8, 8 } } )
				
				
				local end_line = self._comparision_panel:rect( { y=y, w=2, h=h-6, layer=3, color=Color(1, 1, 1, 1), blend_mode="normal" } )
				local mid_line = self._comparision_panel:rect( { y=y, w=2, h=h-6, layer=3, color=Color(1, 1, 1, 1), blend_mode="normal" } )
				--[[
				local diff_len = h * 0.5
				local diff_width = diff_len *0.33
				
				local plus_1 = self._comparision_panel:rect( { y=y, w=diff_len, h=diff_width, layer=3, color=Color(1, 1, 1, 1), blend_mode="normal" } )
				local plus_2 = self._comparision_panel:rect( { y=y, w=diff_width, h=diff_len, layer=3, color=Color(1, 1, 1, 1), blend_mode="normal" } )
				]]
				
				bg:set_right( self._comparision_panel:w() - 16 )
				base:set_left( bg:left() )
				neg:set_left( bg:left() )
				pos:set_left( bg:left() )
				text:set_right( bg:left() - 5 )
				end_line:set_right( bg:right() )
				
				local text_center_y = text:center_y()
				bg:set_center_y( text_center_y )
				base:set_center_y( text_center_y )
				neg:set_center_y( text_center_y )
				pos:set_center_y( text_center_y )
				mid_line:set_center_y( text_center_y )
				end_line:set_center_y( text_center_y )
				
				plus:set_center_y( text_center_y )
				minus:set_center_y( text_center_y )
				
				plus:set_left( math.round( bg:right() + 4 ) )
				minus:set_left( plus:left() )
				--[[
				plus_1:set_center_y( text_center_y )
				plus_2:set_center_y( text_center_y )
				
				plus_1:set_left( math.round( bg:right() + 4 ) )
				plus_2:set_center_x( plus_1:center_x() )
				]]
				
				self._comparision_bitmaps[stat] = {}
				self._comparision_bitmaps[stat].bg = bg
				self._comparision_bitmaps[stat].base = base
				self._comparision_bitmaps[stat].neg = neg
				self._comparision_bitmaps[stat].pos = pos
				self._comparision_bitmaps[stat].mid_line = mid_line
				self._comparision_bitmaps[stat].plus_icon = plus
				self._comparision_bitmaps[stat].minus_icon = minus

				--[[
				for x=1, 10 do
					local bg_base = self._comparision_panel:bitmap( { x=longest_text_w+x*(h*0.5625), y=(i-1)*(h+2), w=h, h=h, layer=0, texture="guis/textures/pd2/blackmarket/stat_bg_base" } )
					local bg_negative = self._comparision_panel:bitmap( { x=longest_text_w+x*(h*0.5625), y=(i-1)*(h+2), w=h, h=h, layer=1, texture="guis/textures/pd2/blackmarket/stat_bg_negative" } )
					local diff_negative = self._comparision_panel:bitmap( { x=longest_text_w+x*(h*0.5625), y=(i-1)*(h+2), w=h, h=h, layer=1, texture="guis/textures/pd2/blackmarket/stat_diff_negative" } )
					local diff_positive = self._comparision_panel:bitmap( { x=longest_text_w+x*(h*0.5625), y=(i-1)*(h+2), w=h, h=h, layer=1, texture="guis/textures/pd2/blackmarket/stat_diff_positive" } )
					local basefill = self._comparision_panel:bitmap( { x=longest_text_w+x*(h*0.5625), y=(i-1)*(h+2), w=h, h=h, layer=1, texture="guis/textures/pd2/blackmarket/stat_basefill" } )
					
					table.insert( self._comparision_bitmaps[stat], {bg_base=bg_base, bg_negative=bg_negative, diff_negative=diff_negative, diff_positive=diff_positive, basefill=basefill} )
				end
				]]
				-- comparision_h = comparision_h + h + 2
				-- self._comparision_panel:bitmap( { name=stat.."_line", x=0, y=(i-1)*(h+2)-9, w=h, h=h*2, layer=2, texture="guis/textures/pd2/blackmarket/stat_line" } )
			end
			
			self._comparision_panel:hide()
			-- self:show_comparision()
		end
		
		self._info_texts = {}
		self._info_texts_panel = self._panel:panel( { x=info_box_panel:x()+10, y=info_box_panel:y()+10, w=info_box_panel:w()-20, h=info_box_panel:h()-20-real_small_font_size*3 } )
		table.insert( self._info_texts, self._info_texts_panel:text( { name="info_text_1", font_size=small_font_size, font=small_font, layer=1, color=tweak_data.screen_colors.text, text="", wrap=true, word_wrap=true } ) )
		table.insert( self._info_texts, self._info_texts_panel:text( { name="info_text_2", font_size=small_font_size, font=small_font, layer=1, color=tweak_data.screen_colors.important_1, text="", wrap=true, word_wrap=true, blend_mode="add" } ) )
		table.insert( self._info_texts, self._info_texts_panel:text( { name="info_text_3", font_size=small_font_size, font=small_font, layer=1, color=tweak_data.screen_colors.text, text="", wrap=true, word_wrap=true } ) )
		
		-- self._title_text:set_lefttop( self._btn_panel:left() + 10, 0 )
		
		-- self._panel:child("back_button"):set_rightbottom( self._btn_panel:right() - 10, self._btn_panel:top() + comparision_h + 10 )
		-- self._panel:child("money_text"):set_bottom( self._panel:child("back_button"):bottom() )
		
		-- self._btn_panel:set_h( self._panel:child( "back_button" ):bottom() - self._btn_panel:top() )
		if self._comparision_panel then
			self._comparision_panel:set_h( (self._stats_shown and (#self._stats_shown) or 4) * (h) )
			self._comparision_panel:set_rightbottom( self._panel:w()-2, info_box_panel:bottom()- 10 - real_small_font_size * 5 - 5 ) -- self._btn_panel:top() - 5 )
		end
		if self._info_panel then
			self._info_panel:set_size( info_box_panel:w() - 20, self._info_panel:num_children() / 2 * h )
			self._info_panel:set_rightbottom( self._panel:w() - 10, self._btn_panel:top() - 10 )
		end
		
		local tab_x = 0 -- self._title_text:x() + text_width + 45
		if not managers.menu:is_pc_controller() and #self._tabs > 1 then
			local prev_page = self._panel:text( { name="prev_page", y=0, font_size=medium_font_size, font=medium_font, layer=2, color=tweak_data.screen_colors.text, text=managers.localization:get_default_macro("BTN_BOTTOM_L") } )
			local _, _, w, h = prev_page:text_rect()
			prev_page:set_w( w )
			prev_page:set_top( top_padding )
			prev_page:set_left( tab_x )
			prev_page:set_visible( self._selected > 1 )
			-- tab_x = tab_x + w + 30
			self._tab_scroll_panel:move( w + 15, 0 )
			self._tab_scroll_panel:grow( -( w + 15 ), 0 )
			
			-- prev_page:set_debug(true)
		end
		
		for _, tab in ipairs( self._tabs ) do
			tab_x = tab:set_tab_position( tab_x )
		end
		
		if not managers.menu:is_pc_controller() and #self._tabs > 1 then
			local next_page = self._panel:text( { name="next_page", y=0, font_size=medium_font_size, font=medium_font, layer=2, color=tweak_data.screen_colors.text, text=managers.localization:get_default_macro("BTN_BOTTOM_R") } )
			local _, _, w, h = next_page:text_rect()
			next_page:set_size( w, h )
			next_page:set_top( top_padding )
			next_page:set_right( grid_panel_w ) -- left( tab_x - 15 )
			next_page:set_visible( self._selected < #self._tabs )
			self._tab_scroll_panel:grow( -( w + 15 ), 0 )
			-- next_page:set_debug(true)
		end
		
		if managers.menu:is_pc_controller() and self._tab_scroll_table[#self._tab_scroll_table]:right() > self._tab_scroll_table.panel:w() then
			-- print( "NEED TO START SCROLLING" )
			
			local prev_page = self._panel:text( { name="prev_page", y=0, w=0, font_size=medium_font_size, font=medium_font, layer=2, color=tweak_data.screen_colors.button_stage_3, text="<", align="center" } )
			local _, _, w, h = prev_page:text_rect()
			prev_page:set_size( w, h )
			prev_page:set_top( top_padding )
			prev_page:set_left( 0 )
			prev_page:set_text(" ")
			-- prev_page:set_visible( self._selected > 1 )
			
			self._tab_scroll_panel:move( w + 15, 0 )
			self._tab_scroll_panel:grow( -( w + 15 ), 0 )
			
			local next_page = self._panel:text( { name="next_page", y=0, w=0, font_size=medium_font_size, font=medium_font, layer=2, color=tweak_data.screen_colors.button_stage_3, text=">", align="center" } )
			local _, _, w, h = next_page:text_rect()
			next_page:set_size( w, h )
			next_page:set_top( top_padding )
			next_page:set_right( grid_panel_w )
		-- 	next_page:set_visible( self._selected < #self._tabs )
			
			
			self._tab_scroll_table.left = prev_page
			self._tab_scroll_table.right = next_page
			
			self._tab_scroll_table.left_klick = false
			self._tab_scroll_table.right_klick = true
			
			
			if self._selected > 1 then
				self._tab_scroll_table.left_klick = true
				self._tab_scroll_table.left:set_text( "<" )
			else
				self._tab_scroll_table.left_klick = false
				self._tab_scroll_table.left:set_text( " " )
			end
			
			if self._selected < #self._tab_scroll_table then
				self._tab_scroll_table.right_klick = true
				self._tab_scroll_table.right:set_text( ">" )
			else
				self._tab_scroll_table.right_klick = false
				self._tab_scroll_table.right:set_text( " " )
			end
			
			self._tab_scroll_panel:grow( -( w + 15 ), 0 )
		end
		
		
		-- local h = self._tabs[ self._selected ]._tab_panel:h() + self._tabs[ self._selected ]._grid_panel:h() + self._btn_panel:h()
		
		-- self._panel:set_h( h )
		-- self._panel:set_center_y( self._panel:parent():h() / 2 )
				
	else
		self._select_rect:hide()
	end
	
	if( MenuBackdropGUI ) then
		local bg_text = self._fullscreen_panel:text( { text=self._title_text:text(), h=90, align="left", vertical="top", font_size=massive_font_size, font=massive_font, color=tweak_data.screen_colors.button_stage_3, alpha=0.4 } )
		local x, y = managers.gui_data:safe_to_full_16_9( self._title_text:world_x(), self._title_text:world_center_y() )
		bg_text:set_world_left( x )
		bg_text:set_world_center_y( y )
		bg_text:move( -13, 9 )
		
		MenuBackdropGUI.animate_bg_text( self, bg_text )
		
		if managers.menu:is_pc_controller() then
			local bg_back = self._fullscreen_panel:text( { name="back_button", text=utf8.to_upper( managers.localization:text("menu_back") ), h=90, align="right", vertical="bottom", font_size=massive_font_size, font=massive_font, color=tweak_data.screen_colors.button_stage_3, alpha=0.4, layer=0 } )
			local x, y = managers.gui_data:safe_to_full_16_9( self._panel:child("back_button"):world_right(), self._panel:child("back_button"):world_center_y() )
			bg_back:set_world_right( x )
			bg_back:set_world_center_y( y )
			bg_back:move( 13, -9 )
			
			MenuBackdropGUI.animate_bg_text( self, bg_back )
		end
	end
	if self._selected_slot then
		self:on_slot_selected( self._selected_slot )
	end
	
	
	local black_rect = self._fullscreen_panel:rect( { color=Color( 0.4, 0, 0, 0 ), layer=1 } )
	
	if is_start_page then
		-- Application:debug( "--- BLACKMARKETGUI: NEW UNLOCKS ---")
		local new_givens = managers.blackmarket:fetch_new_items_unlocked()
		local params = {}
		params.sound_event = "stinger_new_weapon"
		for _, unlocked_item in ipairs( new_givens ) do
			params.category = unlocked_item[1]
			params.value = unlocked_item[2]
			
			-- print( inspect( params ) )
			managers.menu:show_new_item_gained( params )
		end
		-- Application:debug( "--- BLACKMARKETGUI: NEW UNLOCKS ---")
	end
	self:set_tab_positions()
	self:_round_everything()
end


function BlackMarketGui:get_weapon_ammo_info( weapon_id, comparision_data )
	local weapon_tweak_data = tweak_data.weapon[weapon_id]
	local ammo_max_multiplier = managers.player:upgrade_value( "player", "extra_ammo_multiplier", 1 )
	ammo_max_multiplier = ammo_max_multiplier * managers.player:upgrade_value( weapon_tweak_data[ "category" ], "extra_ammo_multiplier", 1 )
	
	local get_ammo_max_per_clip = function( weapon_id )
		local upgrade_blocked = function( category, upgrade )
			if not weapon_tweak_data.upgrade_blocks then
				return false
			end
			
			if not weapon_tweak_data.upgrade_blocks[ category ] then
				return false
			end
			
			return table.contains( weapon_tweak_data.upgrade_blocks[ category ], upgrade )
		end
		
		local ammo = weapon_tweak_data.CLIP_AMMO_MAX 
		ammo = ammo + managers.player:upgrade_value( weapon_id, "clip_ammo_increase" )
		if not upgrade_blocked( "weapon", "clip_ammo_increase" ) then
			ammo = ammo + managers.player:upgrade_value( "weapon", "clip_ammo_increase", 0 )
		end
		
		ammo = ammo + (comparision_data and comparision_data.extra_ammo and tweak_data.weapon.stats.extra_ammo[comparision_data.extra_ammo] or 0)
		return ammo
	end
	
	
	local ammo_max_per_clip = get_ammo_max_per_clip( weapon_id )
	local ammo_max = math.round( (tweak_data.weapon[ weapon_id ].AMMO_MAX + managers.player:upgrade_value( weapon_id, "clip_amount_increase" ) * ammo_max_per_clip) * ammo_max_multiplier )
		
	return ammo_max_per_clip, ammo_max
end

function BlackMarketGui:update_info_text()		-- [1] = Title, [2] = Error text, [3] = Content text, ## bla bla ## = color coded, if resource_color is set ( Color )
	local slot_data = self._slot_data
	local tab_data = self._tabs[self._selected]._data
	local prev_data = tab_data.prev_node_data
	local ids_category = Idstring( slot_data.category )
	local identifier = tab_data.identifier
	
	local updated_texts = { {text=""}, {text=""}, {text=""} }
	
	-- print( inspect( slot_data ), inspect( tab_data ), identifier )
	
--------------------------------- W E A P O N -------------------------------------------------------------------------------
	if identifier == self.identifiers["weapon"] then
		local price = slot_data.price or 0 -- managers.money:get_weapon_slot_sell_value( slot_data.category, slot_data.slot )
		if not slot_data.empty_slot then
			updated_texts[1].text = slot_data.name_localized
			
			if price > 0 then
				updated_texts[1].text = updated_texts[1].text .. "\n" .. "##" .. managers.localization:to_upper_text(slot_data.not_moddable and "st_menu_cost" or "st_menu_value") .. " " .. managers.experience:cash_string( price ) .. "##"
				updated_texts[1].resource_color = slot_data.can_afford and tweak_data.screen_colors.resource or tweak_data.screen_colors.important_1
			end
			
			local clip_ammo, max_ammo = self:get_weapon_ammo_info( slot_data.name, slot_data.comparision_data )
			
			local ammo_id = "bm_menu_ammo_capacity"
			if slot_data.name == "saw" then
				ammo_id = "bm_menu_saw_ammo_capacity"
				max_ammo = math.max( math.floor( max_ammo/clip_ammo ), 0 )
				-- clip_ammo = 1
			end
			
			updated_texts[3].text = managers.localization:text( ammo_id, { clip=clip_ammo, max=max_ammo } )
			updated_texts[3].resource_color = tweak_data.screen_colors.text
			
			updated_texts[3].text = updated_texts[3].text .. " " .. managers.localization:text( tweak_data.weapon[ slot_data.name ].desc_id )
			
			if not self._slot_data.unlocked then
				updated_texts[2].text = ( utf8.to_upper( managers.localization:text( (slot_data.level == 0 and (slot_data.skill_name or "bm_menu_skilltree_locked") or "bm_menu_level_req"), { level=slot_data.level, SKILL=slot_data.name_localized } ) ) )
				
				-- if slot_data.last_weapon then
					updated_texts[2].text = updated_texts[2].text .. "\n"
				-- end
			elseif self._slot_data.can_afford == false then
				-- updated_texts[2].text = ( managers.localization:to_upper_text( "bm_menu_out_of_money_standard", { money=managers.experience:cash_string( (slot_data.price or 0) - managers.money:total() ) } ) )
				-- updated_texts[2].text = updated_texts[2].text .. "\n"
			end
			if slot_data.last_weapon then
				updated_texts[2].text = updated_texts[2].text .. managers.localization:text( "bm_menu_last_weapon_warning" )
			end
			
			if not slot_data.not_moddable then
				local equipped_mods = deep_clone( managers.blackmarket:get_weapon_blueprint( slot_data.category, slot_data.slot ) )
				
				local factory_id = managers.weapon_factory:get_factory_id_by_weapon_id( slot_data.name ) 
				local default_blueprint = managers.weapon_factory:get_default_blueprint_by_factory_id( factory_id )
				
				for _, default_part in ipairs( default_blueprint ) do
					table.delete( equipped_mods, default_part )
				end
				
				local weapon_factory_tweak_data = tweak_data.weapon.factory.parts
				local mods = {}
				local gadget
				for _, mod in ipairs( equipped_mods ) do
					if weapon_factory_tweak_data[mod] then
						if weapon_factory_tweak_data[mod].type == "gadget" then
							gadget = weapon_factory_tweak_data[mod].name_id
						else
							table.insert( mods, "bm_menu_" .. weapon_factory_tweak_data[mod].type )
						end
					end
				end
				
				if gadget then
					table.insert( mods, gadget )
				end
				
				local list_of_mods = ""
				local modded = true
				
				if #mods > 1 then
					for i=1, #mods do
						list_of_mods = list_of_mods .. utf8.to_lower( managers.localization:text( mods[i] ) )
						if i < #mods-1 then
							list_of_mods = list_of_mods .. ", "
						elseif i == #mods-1 then
							list_of_mods = list_of_mods .. " " .. managers.localization:text("bm_menu_last_of_kind") .. " "
						end
					end
				elseif #mods == 1 then
					list_of_mods = string.lower( managers.localization:text(  mods[1] ) )
				else
					modded = false
				end
				
				if modded then
					updated_texts[3].text = updated_texts[3].text .. " " .. managers.localization:text( "bm_menu_weapon_info_modded", {list_of_mods=list_of_mods} )
				end
			end
			-- "bm_menu_weapon_info_modded"
			-- "bm_menu_last_of_kind"
			-- list_of_mods 
		elseif not slot_data.is_loadout then
			local prefix = ""
			if not managers.menu:is_pc_controller() then
				prefix = managers.localization:get_default_macro( "BTN_A" )
			end
			updated_texts[1].text = prefix .. managers.localization:to_upper_text( "bm_menu_btn_buy_new_weapon" )
			updated_texts[3].text = managers.localization:text( "bm_menu_empty_weapon_slot_buy_info" )
		end
		
		if slot_data.unlocked and not slot_data.empty_slot then
			local weapon
			if slot_data.not_moddable then
				local weapon_id = slot_data.name
				local factory_id = managers.weapon_factory:get_factory_id_by_weapon_id( weapon_id )
				local blueprint = managers.weapon_factory:get_default_blueprint_by_factory_id( factory_id )
				weapon = { weapon_id=weapon_id, factory_id=factory_id, blueprint=blueprint }
			else
				weapon = managers.blackmarket:get_crafted_category_slot( slot_data.category, slot_data.slot )
			end
			
			
			-- managers.weapon_factory:change_part_blueprint_only( weapon.factory_id, part_id, blueprint )
			
			local suspicion_index = managers.blackmarket:get_real_concealment_index_of_local_player()
			local new_suspicion_index = suspicion_index - (managers.blackmarket:calculate_weapon_concealment(slot_data.category) - managers.blackmarket:calculate_weapon_concealment(weapon))
			new_suspicion_index = math.clamp( new_suspicion_index, 1, #tweak_data.weapon.stats.concealment )
			
			local visibility_index = 1
			local visibility_id = ""
			local visibility_position = 0
			local min_index = 0
			for i, visibility in ipairs( tweak_data.gui.suspicion_to_visibility ) do
				if new_suspicion_index <= visibility.max_index then
					visibility_index = i
					visibility_id = visibility.name_id
					
					local a = visibility.max_index - min_index
					local b = new_suspicion_index - min_index
					
					visibility_position = b / a
					break
				end
				min_index = visibility.max_index - 1
			end
			
			local visiblity_line = self._visibility_panel:child("bar"):child( "line" )
			local current_visibility_bar = self._visibility_panel:child("bar"):child( tostring(visibility_index) )
			
			-- self._visibility_diff_bar:set_w( 3 )
			-- self._visibility_diff_bar:set_center_x( current_visibility_bar:left() + current_visibility_bar:w() * visibility_position )
			self._visibility_diff_bar:set_w( math.round( visiblity_line:center_x() - (current_visibility_bar:left() + current_visibility_bar:w() * visibility_position) ) )
			self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		else
			local visiblity_line = self._visibility_panel:child("bar"):child( "line" )
			self._visibility_diff_bar:set_w( 0 )
			self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		end
--------------------------------- A R M O R ---------------------------------------------------------------------------------
	elseif identifier == self.identifiers["armor"] then
		updated_texts[1].text = self._slot_data.name_localized
		updated_texts[3].text = managers.localization:text( tweak_data.blackmarket.armors[ slot_data.name ].desc_id )
		
		if not self._slot_data.unlocked then
			updated_texts[2].text = ( utf8.to_upper( managers.localization:text( (slot_data.level == 0 and (slot_data.skill_name or "bm_menu_skilltree_locked") or "bm_menu_level_req"), { level=slot_data.level, SKILL=slot_data.name } ) ) )
			
			local visiblity_line = self._visibility_panel:child("bar"):child( "line" )
			self._visibility_diff_bar:set_w( 0 )
			self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		else
			local suspicion_index = managers.blackmarket:get_real_concealment_index_of_local_player()
			local new_suspicion_index = suspicion_index - (managers.blackmarket:calculate_armor_concealment() - managers.blackmarket:calculate_armor_concealment( slot_data.name ))
			new_suspicion_index = math.clamp( new_suspicion_index, 1, #tweak_data.weapon.stats.concealment )
			
			local visibility_index = 1
			local visibility_id = ""
			local visibility_position = 0
			local min_index = 0
			for i, visibility in ipairs( tweak_data.gui.suspicion_to_visibility ) do
				if new_suspicion_index <= visibility.max_index then
					visibility_index = i
					visibility_id = visibility.name_id
					
					local a = visibility.max_index - min_index
					local b = new_suspicion_index - min_index
					
					visibility_position = b / a
					break
				end
				min_index = visibility.max_index - 1
			end
			
			local visiblity_line = self._visibility_panel:child("bar"):child( "line" )
			local current_visibility_bar = self._visibility_panel:child("bar"):child( tostring(visibility_index) )
			
			-- self._visibility_diff_bar:set_w( 3 )
			-- self._visibility_diff_bar:set_center_x( current_visibility_bar:left() + current_visibility_bar:w() * visibility_position )
			self._visibility_diff_bar:set_w( math.round( visiblity_line:center_x() - (current_visibility_bar:left() + current_visibility_bar:w() * visibility_position) ) )
			self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		end
		-- self._visibility_diff_bar:set_color( self._visibility_diff_bar:w() > 0 and Color.grey or Color.white )
---------------------------------- M A S K ----------------------------------------------------------------------------------
	elseif identifier == self.identifiers["mask"] then
		local price = slot_data.price
		
		if not price then
			if type( slot_data.unlocked ) ~= "number" then
				price = managers.money:get_mask_slot_sell_value( slot_data.slot )
			else
				price = managers.money:get_mask_sell_value( slot_data.name, slot_data.global_value )
			end
		end
		
		if not slot_data.empty_slot then
			updated_texts[1].text = slot_data.name_localized
			
			local resource_colors = {}
			if price >= 0 and slot_data.slot ~= 1 then
				updated_texts[1].text = updated_texts[1].text .. "\n" .. "##" .. managers.localization:to_upper_text("st_menu_value") .. " " .. managers.experience:cash_string( price ) .. "##"
				table.insert( resource_colors, slot_data.can_afford~=false and tweak_data.screen_colors.resource or tweak_data.screen_colors.important_1 )
			end
			
			if slot_data.num_backs then
				updated_texts[1].text = updated_texts[1].text .. "   " .. "##" .. managers.localization:to_upper_text("bm_menu_item_amount", {amount=math.abs(slot_data.unlocked)}) .. "##"
				table.insert( resource_colors, math.abs(slot_data.unlocked) >= 5 and tweak_data.screen_colors.resource or math.abs(slot_data.unlocked) > 0 and Color( 1, 0.9, 0.9, 0.3) or tweak_data.screen_colors.important_1 )
			end
			if #resource_colors == 1 then
				updated_texts[1].resource_color = resource_colors[1]
			else
				updated_texts[1].resource_color = resource_colors
			end
			
			if slot_data.dlc_locked then
				updated_texts[2].text = managers.localization:to_upper_text( slot_data.dlc_locked )
			end
			local mask_id = slot_data.name --  = managers.blackmarket:get_crafted_category_slot( "masks", slot_data.slot )
			if mask_id then
				local desc_id = tweak_data.blackmarket.masks[ mask_id ].desc_id
				updated_texts[3].text = desc_id and managers.localization:text( desc_id ) or (Application:production_build() and ("Add ##desc_id## to ##"..mask_id.."## in tweak_data.blackmarket.masks") or "")
				

				if slot_data.global_value and slot_data.global_value~= "normal" then -- and slot_data.global_value == "infamous" then
					-- updated_texts[3].text = updated_texts[3].text .. "\n##" .. managers.localization:to_upper_text( "menu_l_infamous" ) .. "##"
					updated_texts[3].text = updated_texts[3].text .. "\n##" .. managers.localization:to_upper_text( tweak_data.lootdrop.global_values[slot_data.global_value].name_id ) .. "##"
					updated_texts[3].resource_color = tweak_data.lootdrop.global_values[slot_data.global_value].color
				end
				
				if Application:production_build() and not desc_id then
					updated_texts[3].resource_color = Color.red
				end
			end
		else
			if slot_data.cannot_buy then
				updated_texts[1].text = managers.localization:to_upper_text( "bm_menu_empty_mask_slot" )
				updated_texts[2].text = managers.localization:to_upper_text( "bm_menu_no_masks_in_stash_varning" )
			else
				local prefix = ""
				if not managers.menu:is_pc_controller() then
					prefix = managers.localization:get_default_macro( "BTN_A" )
				end
				updated_texts[1].text = prefix..managers.localization:to_upper_text("bm_menu_btn_buy_new_mask")
			end
			updated_texts[3].text = managers.localization:text( "bm_menu_empty_mask_slot_buy_info" )
		end
		
---------------------------------- W E A P O N  M O D -----------------------------------------------------------------------
	elseif identifier == self.identifiers["weapon_mod"] then
		local price = slot_data.price or managers.money:get_weapon_modify_price( prev_data.name, slot_data.name, slot_data.global_value )
		updated_texts[1].text = slot_data.name_localized
		
		local resource_colors = {}
		if price > 0 then
			updated_texts[1].text = updated_texts[1].text .. "\n" .. "##" .. managers.localization:to_upper_text("st_menu_cost") .. " " .. managers.experience:cash_string( price ) .. "##"
			table.insert( resource_colors, slot_data.can_afford and tweak_data.screen_colors.resource or tweak_data.screen_colors.important_1 )
		else
			updated_texts[1].text = updated_texts[1].text .. "\n"
		end
		
		local unlocked = slot_data.unlocked and (slot_data.unlocked~=true and slot_data.unlocked) or 0
		updated_texts[1].text = updated_texts[1].text .. (price > 0 and "   " or "") .. "##" .. managers.localization:to_upper_text("bm_menu_item_amount", {amount=tostring(math.abs(unlocked))}) .. "##"
		table.insert( resource_colors, math.abs(unlocked) >= 5 and tweak_data.screen_colors.resource or math.abs(unlocked) > 0 and Color( 1, 0.9, 0.9, 0.3) or tweak_data.screen_colors.important_1 )
		
		
		if #resource_colors == 1 then
			updated_texts[1].resource_color = resource_colors[1]
		else
			updated_texts[1].resource_color = resource_colors
		end
		
		local can_not_afford = slot_data.can_afford == false
		local conflicted = unlocked < 0 and slot_data.conflict
		local out_of_item = slot_data.unlocked and slot_data.unlocked~=true and slot_data.unlocked == 0
		
		if slot_data.dlc_locked then
			updated_texts[2].text = managers.localization:to_upper_text( slot_data.dlc_locked )
		elseif conflicted then
			updated_texts[2].text = managers.localization:text( "bm_menu_conflict", { conflict=slot_data.conflict } )
		--[[elseif out_of_item then
			updated_texts[2].text = managers.localization:text( "bm_menu_no_items" )
		elseif can_not_afford then
			local missing_money = managers.experience:cash_string( (slot_data.price or 0) - managers.money:total() )
			updated_texts[2].text = managers.localization:text( "bm_menu_out_of_money", {money=missing_money} )]]
		end
		
		
		local part_id = slot_data.name
		if part_id then
			local desc_id = tweak_data.blackmarket.weapon_mods[ part_id ].desc_id
			updated_texts[3].text = desc_id and managers.localization:text( desc_id, { BTN_GADGET=managers.localization:btn_macro( "weapon_gadget", true ) } ) or (Application:production_build() and ("Add ##desc_id## to ##"..part_id.."## in tweak_data.blackmarket.weapon_mods") or "")
		end
		
		if slot_data.global_value and slot_data.global_value~= "normal" then -- and slot_data.global_value == "infamous" then
			-- updated_texts[3].text = updated_texts[3].text .. "\n##" .. managers.localization:to_upper_text( "menu_l_infamous" ) .. "##"
			updated_texts[3].text = updated_texts[3].text .. "\n##" .. managers.localization:to_upper_text( tweak_data.lootdrop.global_values[slot_data.global_value].name_id ) .. "##"
			updated_texts[3].resource_color = tweak_data.lootdrop.global_values[slot_data.global_value].color
		end
		
		if (slot_data.equipped or (slot_data.unlocked==true or (slot_data.unlocked and slot_data.unlocked~=0))) and not slot_data.empty_slot then
			local weapon = deep_clone( managers.blackmarket:get_crafted_category_slot( slot_data.category, slot_data.slot ) )
			
			if slot_data.equipped then
				if slot_data.default_mod then
					managers.weapon_factory:change_part_blueprint_only( weapon.factory_id, slot_data.default_mod, weapon.blueprint )
				else
					managers.weapon_factory:remove_part_from_blueprint( part_id, weapon.blueprint )
				end
			else
				managers.weapon_factory:change_part_blueprint_only( weapon.factory_id, part_id, weapon.blueprint )
			end
			-- 
			local custom_data = {}
			custom_data[slot_data.category] = managers.blackmarket:get_crafted_category_slot( slot_data.category, slot_data.slot )
			local suspicion_index = managers.blackmarket:get_real_concealment_index_from_custom_data( custom_data )
			local new_suspicion_index = suspicion_index - (managers.blackmarket:calculate_weapon_concealment(custom_data[slot_data.category]) - managers.blackmarket:calculate_weapon_concealment(weapon))
			new_suspicion_index = math.clamp( new_suspicion_index, 1, #tweak_data.weapon.stats.concealment )
			
			local visibility_index = 1
			local visibility_id = ""
			local visibility_position = 0
			local min_index = 0
			for i, visibility in ipairs( tweak_data.gui.suspicion_to_visibility ) do
				if new_suspicion_index <= visibility.max_index then
					visibility_index = i
					visibility_id = visibility.name_id
					
					local a = visibility.max_index - min_index
					local b = new_suspicion_index - min_index
					
					visibility_position = b / a
					break
				end
				min_index = visibility.max_index - 1
			end
			
			local visiblity_line = self._visibility_panel:child("bar"):child( "line" )
			local current_visibility_bar = self._visibility_panel:child("bar"):child( tostring(visibility_index) )
			
			-- self._visibility_diff_bar:set_w( 3 )
			-- self._visibility_diff_bar:set_center_x( current_visibility_bar:left() + current_visibility_bar:w() * visibility_position )
			self._visibility_diff_bar:set_w( math.round( visiblity_line:center_x() - (current_visibility_bar:left() + current_visibility_bar:w() * visibility_position) ) )
			self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		else
			local visiblity_line = self._visibility_panel:child("bar"):child( "line" )
			self._visibility_diff_bar:set_w( 0 )
			self._visibility_diff_bar:set_right( visiblity_line:center_x() )
		end
		
----------------------------------- M A S K  M O D --------------------------------------------------------------------------
	elseif identifier == self.identifiers["mask_mod"] then
		if not managers.blackmarket:currently_customizing_mask() then
			return
		end
		local mask_mod_info = managers.blackmarket:info_customize_mask()
		
		updated_texts[1].text = "MASK CUSTOMIZATION" .. "\n"
		
		local material_text = managers.localization:to_upper_text( "bm_menu_materials" )
		local pattern_text = managers.localization:to_upper_text( "bm_menu_textures" )
		local colors_text = managers.localization:to_upper_text( "bm_menu_colors" )
		
		if mask_mod_info[1].overwritten then
			updated_texts[1].text = updated_texts[1].text .. material_text .. ": ".. "##".. managers.localization:to_upper_text( "menu_bm_overwritten" ) .."##" .. "\n"
		elseif mask_mod_info[1].is_good then
			updated_texts[1].text = updated_texts[1].text .. material_text .. ": ".. managers.localization:text( mask_mod_info[1].text )
			if mask_mod_info[1].price and mask_mod_info[1].price > 0 then
				updated_texts[1].text = updated_texts[1].text .. " " .. managers.experience:cash_string( mask_mod_info[1].price )
			end
			updated_texts[1].text =  updated_texts[1].text .. "\n"
		else
			updated_texts[1].text = updated_texts[1].text .. material_text .. ": ".. "##".. managers.localization:to_upper_text( "menu_bm_not_selected" ) .."##" .. "\n"
		end
		
		if mask_mod_info[2].overwritten then
			updated_texts[1].text = updated_texts[1].text .. pattern_text .. ": ".. "##".. managers.localization:to_upper_text( "menu_bm_overwritten" ) .."##" .. "\n"
		elseif mask_mod_info[2].is_good then
			updated_texts[1].text = updated_texts[1].text .. pattern_text .. ": ".. managers.localization:text( mask_mod_info[2].text )
			if mask_mod_info[2].price and mask_mod_info[2].price > 0 then
				updated_texts[1].text = updated_texts[1].text .. " " .. managers.experience:cash_string( mask_mod_info[2].price )
			end
			updated_texts[1].text =  updated_texts[1].text .. "\n"
		else
			updated_texts[1].text = updated_texts[1].text .. pattern_text .. ": ".. "##".. managers.localization:to_upper_text( "menu_bm_not_selected" ) .."##" .. "\n"
		end
		
		if mask_mod_info[3].overwritten then
			updated_texts[1].text = updated_texts[1].text .. colors_text .. ": ".. "##".. managers.localization:to_upper_text( "menu_bm_overwritten" ) .."##" .. "\n"
		elseif mask_mod_info[3].is_good then
			updated_texts[1].text = updated_texts[1].text .. colors_text .. ": ".. managers.localization:text( mask_mod_info[3].text )
			if mask_mod_info[3].price and mask_mod_info[3].price > 0 then
				updated_texts[1].text = updated_texts[1].text .. " " .. managers.experience:cash_string( mask_mod_info[3].price )
			end
			updated_texts[1].text =  updated_texts[1].text .. "\n"
		else
			updated_texts[1].text = updated_texts[1].text .. colors_text .. ": ".. "##".. managers.localization:to_upper_text( "menu_bm_not_selected" ) .."##" .. "\n"
		end
		
		
		updated_texts[1].text = updated_texts[1].text .. "\n"
		
		local price, can_afford = managers.blackmarket:get_customize_mask_value()
		if slot_data.global_value then	-- choosing mod
			updated_texts[3].text = "\n\n" .. managers.localization:to_upper_text("menu_bm_highlighted") .. "\n" .. slot_data.name_localized .. " " .. managers.experience:cash_string( managers.money:get_mask_part_price_modified( slot_data.category, slot_data.name, slot_data.global_value ) ) .. "\n"
			
			if slot_data.unlocked and slot_data.unlocked~=true and slot_data.unlocked ~= 0 then
				updated_texts[3].text = updated_texts[3].text .. managers.localization:to_upper_text( "bm_menu_item_amount", { amount=math.abs(slot_data.unlocked) } ) .. "\n"
			end
			
			if slot_data.global_value and slot_data.global_value~= "normal" then -- and slot_data.global_value == "infamous" then
				-- updated_texts[3].text = updated_texts[3].text .. "\n##" .. managers.localization:to_upper_text( "menu_l_infamous" ) .. "##"
				updated_texts[3].text = updated_texts[3].text .. "\n##" .. managers.localization:to_upper_text( tweak_data.lootdrop.global_values[slot_data.global_value].name_id ) .. "##"
				updated_texts[3].resource_color = tweak_data.lootdrop.global_values[slot_data.global_value].color
			end
			if slot_data.dlc_locked then
				updated_texts[2].text = managers.localization:to_upper_text( slot_data.dlc_locked )
			end
		end
		if price and price > 0 then
			updated_texts[1].text = updated_texts[1].text .. managers.localization:to_upper_text( "menu_bm_total_cost", {cost=( not can_afford and "##" or "" ) ..  managers.experience:cash_string( price ) .. ( not can_afford and "##" or "" )} )
		end
		updated_texts[1].resource_color = tweak_data.screen_colors.important_1
		-- updated_texts[1].resource_color = price <= managers.money:total() and tweak_data.screen_colors.resource or tweak_data.screen_colors.important_1
		
		--[[
		if slot_data.global_value and slot_data.global_value ~= "normal" then
			updated_texts[1].text = updated_texts[1].text .. " ##" .. managers.localization:text( tweak_data.lootdrop.global_values[slot_data.global_value].name_id ) .. "##"
		end]]
		
		if not managers.blackmarket:can_finish_customize_mask() then
			local list_of_mods = ""
			local missed_mods = {}
			
			for _, data in ipairs( mask_mod_info ) do
				if not data.is_good and not data.overwritten then
					table.insert( missed_mods, managers.localization:text( data.text ) )
				end
			end
			
			if #missed_mods > 1 then
				for i=1, #missed_mods do
					list_of_mods = list_of_mods .. missed_mods[i]
					if i < #missed_mods-1 then
						list_of_mods = list_of_mods .. ", "
					elseif i == #missed_mods-1 then
						list_of_mods = list_of_mods .. managers.localization:text("bm_menu_last_of_kind")
					end
				end
			elseif #missed_mods == 1 then
				list_of_mods = missed_mods[1]
			end
				
			updated_texts[2].text = managers.localization:to_upper_text( "bm_menu_missing_to_finalize_mask", { missed_mods=list_of_mods } ) .. "\n"
		end
		
----------------------------------- D E P L O Y A B L E ---------------------------------------------------------------------
	elseif identifier == self.identifiers["deployable"] then
		updated_texts[1].text = slot_data.name_localized
		if not self._slot_data.unlocked then
			updated_texts[2].text = ( managers.localization:to_upper_text( (slot_data.level == 0 and (slot_data.skill_name or "bm_menu_skilltree_locked") or "bm_menu_level_req"), { level=slot_data.level, SKILL=slot_data.name } ) )
			updated_texts[2].text = updated_texts[2].text .. "\n"
		end
		updated_texts[3].text = managers.localization:text( tweak_data.blackmarket.deployables[ slot_data.name ].desc_id, { BTN_INTERACT=managers.localization:btn_macro( "interact", true ), BTN_USE_ITEM=managers.localization:btn_macro( "use_item", true ) } )

----------------------------------- C H A R A C T E R -----------------------------------------------------------------------
	elseif identifier == self.identifiers["character"] then
		updated_texts[1].text = slot_data.name_localized
		updated_texts[3].text = managers.localization:text( slot_data.name .. "_desc" )
	end
	
	
-----------------------------------------------------------------------------------------------------------------------------
	-- tweak_data.blackmarket.masks
	
	
	if self._desc_mini_icons then
		for _, gui_object in pairs(self._desc_mini_icons) do
			self._panel:remove( gui_object[1] )
		end
	end
	self._desc_mini_icons = {}
		
	local desc_mini_icons = self._slot_data.desc_mini_icons
	local info_box_panel = self._panel:child("info_box_panel")
	
	if desc_mini_icons and table.size(desc_mini_icons) > 0 then
		for _, mini_icon in pairs( desc_mini_icons ) do
			local new_icon = self._panel:bitmap( { texture=mini_icon.texture, x=info_box_panel:left() + 10 + mini_icon.right, w=mini_icon.w or 32, h=mini_icon.h or 32 } )
			-- new_icon:set_world_top( self._info_texts[1]:world_top() + 5 )
			table.insert( self._desc_mini_icons, {new_icon, 1} )
		end
		updated_texts[1].text = string.rep( "     ", table.size(desc_mini_icons) ) .. updated_texts[1].text
		-- self._info_texts[1]:set_text( string.rep( "     ", table.size(desc_mini_icons) ) .. self._info_texts[1]:text() )
		-- self._info_texts[1]:set_x( 0 + table.size(desc_mini_icons)*18 )
	else
		-- 
		-- self._info_texts[1]:set_x( 0 )
	end
	
	if slot_data.lock_texture then
		local new_icon = self._panel:bitmap( { texture=slot_data.lock_texture, x=info_box_panel:left() + 10, w=20, h=20, color=self._info_texts[2]:color(), blend_mode="add" } )
		-- new_icon:set_world_center_y( self._info_texts[2]:world_center_y() )
		-- self._info_texts[2]:set_x( 0 + 22 )
		-- self._info_texts[2]:set_w( info_box_panel:w() - 20 - 22 )
		
		updated_texts[2].text = "     " .. updated_texts[2].text
		-- self._info_texts[2]:set_text( "     " .. self._info_texts[2]:text() )
		table.insert( self._desc_mini_icons, {new_icon, 2} )
	else
		-- self._info_texts[2]:set_x( 0 )
		-- self._info_texts[2]:set_w( info_box_panel:w() - 20 )
	end
	
	for id, _ in ipairs( self._info_texts ) do
		self:set_info_text( id, updated_texts[id].text, updated_texts[id].resource_color )
	end
	
	-- local bounce_anim = callback( self, self, "animate_text_bounce" )
	local _, _, _, th = self._info_texts[1]:text_rect()
	self._info_texts[1]:set_h( th )
	
	local y = self._info_texts[1]:bottom()
	-- for _, info_text in ipairs( self._info_texts ) do
	for i=2, #self._info_texts do
		local info_text = self._info_texts[i]
		info_text:set_font_size( small_font_size )
		-- info_text:stop()
		_, _, _, th = info_text:text_rect()
		
		info_text:set_y( y )
		info_text:set_h( th ) -- math.min( th, box_h - 20 - 3 * self._real_small_font_size ) )
		
		local scale = 1
		if info_text:h() + y > self._info_texts_panel:h() then
			scale = self._info_texts_panel:h() / (info_text:h() + info_text:y())
		end
		
		info_text:set_font_size( small_font_size * scale )
		_, _, _, th = info_text:text_rect()
		info_text:set_h( th )
		
		y = info_text:bottom()
	end
	
	for _, desc_mini_icon in ipairs( self._desc_mini_icons ) do
		desc_mini_icon[1]:set_world_top( self._info_texts[desc_mini_icon[2]]:world_top() + (1 - (desc_mini_icon[2]-1)*3) )
	end
end

function BlackMarketGui:animate_text_bounce( bounce_panel )
	local dt = 0
	local bounce_dir_up = true
	local top = bounce_panel:top()
	
	local move = 0
	while true do
		dt = coroutine.yield()
		
		if bounce_dir_up then
			
			-- bounce_panel:set_bottom( math.step( bounce_panel:bottom(), self._info_texts_panel:h(), -dt ) )
		else
			-- bounce_panel:set_top( math.step( bounce_panel:top(), top, dt ) )
		end
		
		bounce_panel:move( 0, move )
		
		if move == 0 then
			bounce_dir_up = not bounce_dir_up
		end
	end
end

					-- tweak_data.screen_colors.resource
function BlackMarketGui:set_info_text( id, new_string, resource_color )
	local info_text = self._info_texts[id]
	local text = new_string
	
	local start_ci
	local end_ci
	local first_ci
	if resource_color then
		local text_dissected = utf8.characters( text )
		local idsp = Idstring("#")
		
		start_ci = {}
		end_ci = {}
		first_ci = true
		for i, c in ipairs( text_dissected ) do
			if Idstring( c ) == idsp then
				local next_c = text_dissected[i+1]
				
				if next_c and Idstring(next_c) == idsp then
					if first_ci then
						table.insert( start_ci, i )
					else
						table.insert( end_ci, i )
					end
					first_ci = not first_ci
				end
			end
		end
		
		if #start_ci ~= # end_ci then
		else
			for i=1, #start_ci do
				start_ci[i] = start_ci[i] - ( (i - 1) * 4 + 1 )
				end_ci[i] = end_ci[i] - ( i * 4 - 1 )
			end
		end
	
		text = string.gsub( text, "##", "" )
	end

	info_text:set_text( text ) -- utf8.to_upper( text ) )
	
	if resource_color then
		info_text:clear_range_color( 1, utf8.len(text) )
		
		if #start_ci ~= # end_ci then
			Application:error( "BlackMarketGui: Not even amount of ##'s in :set_info_text() string!", id, #start_ci, #end_ci )
		else
			for i=1, #start_ci do
				info_text:set_range_color( start_ci[i], end_ci[i], type(resource_color)=="table" and resource_color[i] or resource_color )
			end
		end
	end
end

function BlackMarketGui:_round_everything()
	if alive( self._panel ) then
		for i, d in ipairs( self._panel:children() ) do
			self:_rec_round_object( d )
		end
	end
	
	if alive( self._fullscreen_panel ) then
		for i, d in ipairs( self._fullscreen_panel:children() ) do
			self:_rec_round_object( d )
		end
	end
end

function BlackMarketGui:_rec_round_object( object )
	local x, y, w, h = object:shape()
	object:set_shape( math.round(x), math.round(y), math.round(w), math.round(h) )
	
	if object.children then
		for i, d in ipairs( object:children() ) do
			self:_rec_round_object( d )
		end
	end
end

function BlackMarketGui:mouse_moved( o, x, y )
			-- tabs and grid
	local inside_tab_scroll = self._tab_scroll_panel:inside( x, y )
	local update_select = false
	if( not self._highlighted ) then
		update_select = true
	elseif( self._tabs[ self._highlighted ] and not self._tabs[ self._highlighted ]:inside( x, y ) or not inside_tab_scroll ) then
		self._tabs[ self._highlighted ]:set_highlight( not self._pages, not self._pages )
		self._highlighted = nil
		update_select = true
	end
	
	if( update_select ) then
		for i, tab in ipairs( self._tabs ) do
			update_select = inside_tab_scroll and tab:inside( x, y )
			if( update_select ) then
				self._highlighted = i
				self._tabs[ self._highlighted ]:set_highlight( self._selected ~= self._highlighted )
				
				-- if update_select == 1 then
				-- 	self:mouse_pressed( Idstring( "0" ), x, y )
				-- end
				-- break
			end
		end
	end
	
	if( self._panel:child("back_button"):inside( x, y ) ) then
		if not self._back_button_highlighted then
			self._back_button_highlighted = true
			self._panel:child("back_button"):set_color( tweak_data.screen_colors.button_stage_2 )
			managers.menu_component:post_event( "highlight" )
			return
		end
	elseif self._back_button_highlighted then
		self._back_button_highlighted = false
		self._panel:child("back_button"):set_color( tweak_data.screen_colors.button_stage_3 )
	end
	
			-- buttons
	update_select = false
	if( not self._button_highlighted ) then
		update_select = true
	elseif( self._btns[ self._button_highlighted ] and not self._btns[ self._button_highlighted ]:inside( x, y ) ) then
		self._btns[ self._button_highlighted ]:set_highlight( false )
		self._button_highlighted = nil
		update_select = true
	end
	
	if( update_select ) then
		for i, btn in pairs( self._btns ) do
			if( btn:visible() and btn:inside( x, y ) ) then
				self._button_highlighted = i
				self._btns[ self._button_highlighted ]:set_highlight( true )
			end
		end
	end
	
	if self._tab_scroll_table.left and self._tab_scroll_table.left_klick then
		local color
		if self._tab_scroll_table.left:inside( x, y ) then
			color = tweak_data.screen_colors.button_stage_2
		else
			color = tweak_data.screen_colors.button_stage_3
		end
		self._tab_scroll_table.left:set_color( color )
	end
	if self._tab_scroll_table.right and self._tab_scroll_table.right_klick then
		local color
		if self._tab_scroll_table.right:inside( x, y ) then
			color = tweak_data.screen_colors.button_stage_2
		else
			color = tweak_data.screen_colors.button_stage_3
		end
		self._tab_scroll_table.right:set_color( color )
	end
end

function BlackMarketGui:mouse_pressed( button, x, y )
	if button == Idstring( "mouse wheel down" ) then
		self:next_page( true )
		return
	elseif button == Idstring( "mouse wheel up" ) then
		self:previous_page( true )
		return
	end
	if( button ~= Idstring( "0" ) ) then
		return 
	end
	if( self._panel:child("back_button"):inside( x, y ) ) then
		managers.menu:back( true )
		return
	end
	
	if self._tab_scroll_table.left_klick and self._tab_scroll_table.left:inside( x, y ) then
		self:previous_page()
		return
	end
	
	if self._tab_scroll_table.right_klick and self._tab_scroll_table.right:inside( x, y ) then
		self:next_page()
		return
	end
	
	
	if( self._selected_slot and self._selected_slot._equipped_rect ) then
		self._selected_slot._equipped_rect:set_alpha( 1 )
	end
	
	if( self._tab_scroll_panel:inside( x, y ) and self._tabs[ self._highlighted ] and self._tabs[ self._highlighted ]:inside( x, y ) ) then
		if self._selected ~= self._highlighted then
			self:set_selected_tab( self._highlighted )
		end
		return
	elseif( self._tabs[ self._selected ] ) then
		local selected_slot = self._tabs[ self._selected ]:mouse_pressed( button, x, y )
		self:on_slot_selected( selected_slot )
		if selected_slot then
			return
		end
	end
	if( self._btns[ self._button_highlighted ] and self._btns[ self._button_highlighted ]:inside( x, y ) ) then
		local data = self._btns[ self._button_highlighted ]._data
		if data.callback and (not self._button_press_delay or self._button_press_delay < TimerManager:main():time() )  then
			managers.menu_component:post_event( "menu_enter" )
			data.callback( self._slot_data, self._data.topic_params )
			self._button_press_delay = TimerManager:main():time() + 0.2
		end
	end
	
	if( self._selected_slot and self._selected_slot._equipped_rect ) then
		self._selected_slot._equipped_rect:set_alpha( 0.6 )
	end
-- 	self:show_comparision()
end

function BlackMarketGui:mouse_released( o, button, x, y )
end

function BlackMarketGui:mouse_clicked( o, button, x, y )
	self._mouse_click_index = ( (self._mouse_click_index or 0) + 1 ) % 2
	
	self._mouse_click = self._mouse_click or {}
	self._mouse_click[self._mouse_click_index] = {}
	self._mouse_click[self._mouse_click_index].button = button
	self._mouse_click[self._mouse_click_index].x = x
	self._mouse_click[self._mouse_click_index].y = y
	self._mouse_click[self._mouse_click_index].selected_slot = self._selected_slot
end

function BlackMarketGui:mouse_double_click( o, button, x, y )
	if( not self._mouse_click or not self._mouse_click[0] or not self._mouse_click[1] ) then
		return
	end
	
	if( not self._slot_data or (self._mouse_click[0].selected_slot ~= self._mouse_click[1].selected_slot) ) then
		return
	end
	
	if( not self._selected_slot._panel:inside( x, y ) ) then
		return
	end
	
	if managers.system_menu and managers.system_menu:is_active() and not managers.system_menu:is_closing() then
		return
	end
	self:press_first_btn( button )
end

function BlackMarketGui:press_first_btn( button )
	local first_btn_callback = nil
	local first_btn_prio = 999
	local first_btn_visible = false
	
	if( button == Idstring( "0" ) ) then
		if self._slot_data.double_click_btn then
			local btn = self._btns[self._slot_data.double_click_btn]
			if btn then
				first_btn_prio = btn._data.prio
				first_btn_callback = btn._data.callback
				first_btn_visible = btn:visible()
			end
		else
			for _, btn in pairs( self._btns ) do
				if( btn:visible() and btn._data.prio < first_btn_prio ) then
					first_btn_prio = btn._data.prio
					first_btn_callback = btn._data.callback
					first_btn_visible = btn:visible()
				end
				if( btn:visible() and btn._data.prio == first_btn_prio ) then
					first_btn_prio = btn._data.prio
					first_btn_callback = btn._data.callback
					first_btn_visible = btn:visible()
				end
			end
		end
		
		if( first_btn_visible and first_btn_callback ) then
			managers.menu_component:post_event( "menu_enter" )
			first_btn_callback( self._slot_data, self._data.topic_params )
			return true
		else
			self:flash()
		end
	elseif( button == Idstring( "1" ) ) then
	end
	return false
end

function BlackMarketGui:set_selected_tab( tab, no_sound )
	local selected_slot = nil
	if( self._tabs[ self._selected ] ) then
		selected_slot = self._tabs[ self._selected ]._slot_selected
		self._tabs[ self._selected ]:deselect( false )
	end
	if( self._selected_slot ) then self._selected_slot:set_btn_text() end
	self._selected = tab
	self._node:parameters().menu_component_selected = self._selected
	self._tabs[ self._selected ]:select( false, no_sound )
	self._selected_slot = self._tabs[ self._selected ]:select_slot( selected_slot, true )
	self._slot_data = self._selected_slot._data
	
	
	local x, y = self._tabs[ self._selected ]:selected_slot_center()
	
	local grid_panel_w = self._panel:w() * WIDTH_MULTIPLIER
	local grid_panel_h = ( ( self._panel:h() - (self._real_medium_font_size + 10) - 70 ) * GRID_H_MUL )
	
	local square_w = grid_panel_w / 3
	local square_h = grid_panel_h / 3
	local slot_dim_x = self._tabs[self._selected].my_slots_dimensions[1]
	local slot_dim_y = self._tabs[self._selected].my_slots_dimensions[2]
	self._select_rect:set_size( square_w * 3 / slot_dim_x, square_h * 3 / slot_dim_y )
	self._select_rect_box:create_sides( self._select_rect, { sides = { 2, 2, 2, 2 } } )
	self._select_rect:set_world_center( x, y )
	self._select_rect:stop()
	self._select_rect_box:set_color( Color.white )
	
	self:show_btns( self._selected_slot )
	self:update_info_text()
	
	self:set_tab_positions()
	
	local visibility_visible = false
	if self._selected_slot then
		if self._selected_slot._equipped_rect then
			self._selected_slot._equipped_rect:set_alpha( 0.6 )
		end
		
		local slot_category = self._selected_slot._data.category
		visibility_visible = ( slot_category == "primaries" or slot_category == "secondaries" or slot_category == "armors" )
	end
	
	self._visibility_panel:set_visible( visibility_visible )
	self:show_comparision()
end

function BlackMarketGui:set_tab_positions()
	local first_x = self._tab_scroll_table[1]:left()
	local diff_x = self._tab_scroll_table[ self._selected ]:left()
	if diff_x < 0 then		-- left
		local tab_x = (first_x - diff_x)
		for _, tab in ipairs( self._tabs ) do
			tab_x = tab:set_tab_position( tab_x )
		end
	end
	
	diff_x = self._tab_scroll_table[ self._selected ]:right() - self._tab_scroll_table.panel:w()
	if diff_x > 0 then		-- right
		local tab_x = -(diff_x - first_x)
		for _, tab in ipairs( self._tabs ) do
			tab_x = tab:set_tab_position( tab_x )
		end
	end
	
	if managers.menu:is_pc_controller() then
		if self._tab_scroll_table.left then
			if self._selected > 1 then
				self._tab_scroll_table.left_klick = true
				self._tab_scroll_table.left:set_text( "<" )
			else
				self._tab_scroll_table.left_klick = false
				self._tab_scroll_table.left:set_text( " " )
			end
		end
		
		if self._tab_scroll_table.right then
			if self._selected < #self._tab_scroll_table then
				self._tab_scroll_table.right_klick = true
				self._tab_scroll_table.right:set_text( ">" )
			else
				self._tab_scroll_table.right_klick = false
				self._tab_scroll_table.right:set_text( " " )
			end
		end
	else
		if alive( self._panel:child("prev_page") ) then
			self._panel:child("prev_page"):set_visible( self._selected > 1 )
		end
		if alive( self._panel:child("next_page") ) then
			self._panel:child("next_page"):set_visible( self._selected < #self._tabs )
		end
	end
--[[
	local tab_x = 0
	for _, tab in ipairs( self._tabs ) do
		tab_x = tab:set_tab_position( tab_x )
	end]]
end

function BlackMarketGui:on_slot_selected( selected_slot )
	if( selected_slot ) then
		local x, y = self._tabs[ self._selected ]:selected_slot_center()
		self._select_rect:set_world_center( x, y )
		self._select_rect:stop()
		self._select_rect_box:set_color( Color.white )
		
		if( self._selected_slot ) then self._selected_slot:set_btn_text() end
		self._selected_slot = selected_slot
		self._slot_data = self._selected_slot._data
		self:show_btns( self._selected_slot )
	end
	
	self:update_info_text()
	
	local visibility_visible = false
	if self._selected_slot then
		if self._selected_slot._equipped_rect then
			self._selected_slot._equipped_rect:set_alpha( 0.6 )
		end
		
		local slot_category = self._selected_slot._data.category
		visibility_visible = ( slot_category == "primaries" or slot_category == "secondaries" or slot_category == "armors" )
	end
	
	self._visibility_panel:set_visible( visibility_visible )
	self:show_comparision()
end

function BlackMarketGui:move( mx, my )
	if not self._tabs[self._selected] then
		return
	end
	
	local slot = self._tabs[self._selected]._slot_selected
	if not slot then
		return
	end
	
	local dim_x = self._tabs[self._selected].my_slots_dimensions[1]
	local dim_y = self._tabs[self._selected].my_slots_dimensions[2]
	
	local x = ( (slot-1) % dim_x ) + 1
	local y = math.ceil(slot/dim_x)
	-- print( slot, dim_x, dim_y, x, y )
	
	x = math.clamp( x+mx, 1, dim_x )
	y = math.clamp( y+my, 1, dim_y )
	
	local new_selected = x + ( y-1 ) * dim_x
	
	-- local new_slot = self._tabs[self._selected]._slots[new_selected]
	
	-- if not new_slot then
	-- 	return
	-- end
	
	local slot = self._tabs[self._selected]:select_slot( new_selected, not (new_selected~=slot) )
	self:on_slot_selected( slot )
	
	-- self:mouse_moved( nil, new_slot._panel:center() )
	-- self:mouse_pressed( Idstring("0"), new_slot._panel:center() )
end

function BlackMarketGui:move_up()
	self:move( 0, -1 )
end

function BlackMarketGui:move_down()
	self:move( 0, 1 )
end

function BlackMarketGui:move_left()
	self:move( -1, 0 )
end

function BlackMarketGui:move_right()
	self:move( 1, 0 )
end

function BlackMarketGui:next_page( no_sound)
	if self._pages then
		-- local s = (self._selected ) % #self._tabs + 1
		local old_selected = self._selected
		local s = math.min( self._selected + 1, #self._tabs )
		if old_selected == s then
			return
		end
		self:set_selected_tab( s, no_sound )
	else
		self:move( 1, 0 )
	end
end

function BlackMarketGui:previous_page( no_sound)
	if self._pages then
		-- local s = ( self._selected - 2 ) % #self._tabs + 1
		local old_selected = self._selected
		local s = math.max( self._selected - 1, 1 )
		if old_selected == s then
			return
		end
		self:set_selected_tab( s, no_sound )
	else
		self:move( -1, 0 )
	end
end

function BlackMarketGui:press_pc_button( button )
	local btn = self._controllers_pc_mapping and self._controllers_pc_mapping[ button:key() ]
	if btn and btn._data and btn._data.callback then
		if not self._button_press_delay or self._button_press_delay < TimerManager:main():time() then
			managers.menu_component:post_event( "menu_enter" )
			btn._data.callback( self._slot_data, self._data.topic_params )
			self._button_press_delay = TimerManager:main():time() + 0.2
			return true
		end
	end
	return false
end

function BlackMarketGui:press_button( button )
	local btn = self._controllers_mapping and self._controllers_mapping[ button:key() ]
	if btn and btn._data and btn._data.callback then
		if not self._button_press_delay or self._button_press_delay < TimerManager:main():time() then
			managers.menu_component:post_event( "menu_enter" )
			btn._data.callback( self._slot_data, self._data.topic_params )
			self._button_press_delay = TimerManager:main():time() + 0.2
			return true
		end
	else
		if self._select_rect_box then
			self:flash()
		end
	end
	return false
end

function BlackMarketGui:flash()
	local box = self._select_rect_box
	
	local flash_anim = function( panel )
		local b_color = Color.white
		local s = 0
		over( 0.5, function(t)
			s = math.min( 1, math.sin( t*180 )*2 )
			box:set_color( math.lerp( b_color, tweak_data.screen_colors.important_1, s ) )
		end )
		box:set_color( b_color )
	end
	
	managers.menu_component:post_event( "selection_next", true )
	self._select_rect:animate( flash_anim )
end

function BlackMarketGui:confirm_pressed()
	if managers.menu:is_pc_controller() then
		return self:press_first_btn( Idstring( "0" ) )
	else
		return self:press_button( "BTN_A" )
	end
end

function BlackMarketGui:special_btn_pressed( button )
	return self:press_pc_button( button )
	
	--[[
	if button == Idstring( "1" ) then
		self:press_button( "BTN_Y" )
	elseif button == Idstring( "2" ) then
		self:press_button( "BTN_X" )
	elseif button == Idstring( "3" ) then
		self:press_button( "STICK_R" )
	end]]
end

function BlackMarketGui:input_focus()
	return (#self._data > -1) and 1 or true
end

function BlackMarketGui:visible()
	return self._visible
end

function BlackMarketGui:show_comparision()
	if( not self._comparision_panel ) then
		return
	end
	self._comparision_panel:hide()
	if( not self._slot_data ) then
		return
	end
	
	if( not self._slot_data.comparision_data ) then
		return
	end
	
	self._equipped_comparision_data = self._equipped_comparision_data or {}
	if( not self._equipped_comparision_data[self._slot_data.category] ) then
		self._no_compare = true
		
		if( self._tabs[ self._selected ] ) then
			for i, slot in ipairs( self._tabs[ self._selected ]:slots() ) do
				if( slot._data.equipped ) then
					self._equipped_comparision_data[ slot._data.category ] = slot._data.comparision_data
					self._no_compare = false
					break;
				end
			end
		end
	end
	if( self._no_compare ) then
		self._equipped_comparision_data[self._slot_data.category] = self._slot_data.comparision_data
	end
	
	if( not self._equipped_comparision_data[self._slot_data.category] ) then
		return
	end
	
	if not self._comparision_bitmaps then
		return
	end
	
	self._comparision_panel:show()
	
	local diff = {}
	local resolution = 100
	
	local total_stats
	local old_stat
	local new_stat
	local text_w
	local comp_panel = self._comparision_panel
	local comp_bitmap_stat
	local comp_bitmap_stat_x
	
	local negative_stats = { "recoil", "suppression", "concealment" }
	local is_negative = false
	
	for _, stat in pairs( self._stats_shown ) do
		is_negative = table.contains( negative_stats, stat )
		
		total_stats = math.max( #tweak_data.weapon.stats[stat], 1 )
		old_stat = math.clamp( self._equipped_comparision_data[self._slot_data.category][stat] or 0, 1, total_stats )
		new_stat = math.clamp( self._slot_data.comparision_data[stat] or 0, 1, total_stats )
		
		text_w = comp_panel:child( stat .. "_text" ):w()
		
		if is_negative then
			old_stat = total_stats - (old_stat-1)
			new_stat = total_stats - (new_stat-1)
		end
		
		old_stat = math.ceil((old_stat / total_stats) * resolution)
		new_stat = math.ceil((new_stat / total_stats) * resolution)
		
		diff[stat] = new_stat - old_stat
		comp_bitmap_stat = self._comparision_bitmaps[stat]
		
		if diff[stat] == 0 then
			comp_bitmap_stat.neg:set_visible( false )
			comp_bitmap_stat.pos:set_visible( false )
			comp_bitmap_stat.plus_icon:set_visible( false )
			comp_bitmap_stat.minus_icon:set_visible( false )
			comp_bitmap_stat.mid_line:set_visible( false )
			comp_bitmap_stat.base:set_w( math.round( ( old_stat/resolution ) * comp_bitmap_stat.bg:w() ) )
		elseif( diff[stat] < 0 ) then
			comp_bitmap_stat.neg:set_visible( true )
			comp_bitmap_stat.minus_icon:set_visible( true )
			
			comp_bitmap_stat.pos:set_visible( false )
			comp_bitmap_stat.plus_icon:set_visible( false )
			
			comp_bitmap_stat.mid_line:set_visible( true )
			
			comp_bitmap_stat.base:set_w( math.round( ( old_stat/resolution ) * comp_bitmap_stat.bg:w() ) )
			comp_bitmap_stat.neg:set_w( math.round( (diff[stat]/resolution) * comp_bitmap_stat.bg:w() ) )
			comp_bitmap_stat.mid_line:set_right( comp_bitmap_stat.base:right() )
			
			comp_bitmap_stat.neg:set_left( comp_bitmap_stat.base:right() )

			--[[
			for x=1, 10 do
				comp_bitmap_stat_x = comp_bitmap_stat[x]
				
				comp_bitmap_stat_x.bg_base:set_visible( x > old_stat )
				comp_bitmap_stat_x.bg_negative:set_visible( (x > new_stat) and (x <= old_stat) )
				comp_bitmap_stat_x.diff_negative:set_visible( x <= new_stat )
				comp_bitmap_stat_x.diff_positive:set_visible( false )
				comp_bitmap_stat_x.basefill:set_visible( false )
			end
			]]
		else
			comp_bitmap_stat.neg:set_visible( false )
			comp_bitmap_stat.minus_icon:set_visible( false )
			
			comp_bitmap_stat.pos:set_visible( true )
			comp_bitmap_stat.plus_icon:set_visible( true )
			
			comp_bitmap_stat.mid_line:set_visible( false )
			
			comp_bitmap_stat.base:set_w( math.round( ( old_stat/resolution ) * comp_bitmap_stat.bg:w() ) )
			comp_bitmap_stat.pos:set_w( math.round( (diff[stat]/resolution) * comp_bitmap_stat.bg:w() ) )
			comp_bitmap_stat.pos:set_left( comp_bitmap_stat.base:right() )
			
			--[[
			for x=1, 10 do
				comp_bitmap_stat_x = comp_bitmap_stat[x]
				
				comp_bitmap_stat_x.bg_base:set_visible( x > new_stat )
				comp_bitmap_stat_x.bg_negative:set_visible( false )
				comp_bitmap_stat_x.diff_negative:set_visible( false )
				comp_bitmap_stat_x.diff_positive:set_visible( (x > old_stat) and (x <= new_stat) )
				comp_bitmap_stat_x.basefill:set_visible( x <= old_stat )
			end
			]]
		end
		
		-- comp_panel:child( stat.."_line" ):set_center_x( text_w + (18*0.5625) * (old_stat+1) )
		
		--[[
		local color = (diff[stat] == 0) and tweak_data.screen_color_light_grey or (diff[stat] > 0) and tweak_data.screen_color_green or tweak_data.screen_color_red
		if( self._selected_slot._comparision_panel ) then
			self._selected_slot._comparision_panel:child( stat ):set_color( color )
		end
		]]
	end
end

function BlackMarketGui:show_btns( slot )
	local data = slot._data
	for _, btn in pairs( self._btns ) do
		btn:hide()
	end
	
	local btns = {}
	for i, btn in ipairs( data ) do
		if( self._btns[ btn ] ) then
			self._btns[ btn ]:show()
			table.insert( btns, self._btns[ btn ] )
		end
	end
	
	if not managers.menu:is_pc_controller() then
		local back_btn = self._btns.back_btn
		if back_btn then
			back_btn:show()
			table.insert( btns, back_btn )
		end
	end
	
	table.sort( btns, function( x, y ) return x._data.prio < y._data.prio end )
	
	self._controllers_mapping = {}
	self._controllers_pc_mapping = {}
	
	for i, btn in ipairs( btns ) do
		if not managers.menu:is_pc_controller() and not btn._data.no_btn then
			btn:set_text_btn_prefix( btn._data.btn )
		end
			
		if btn._data.pc_btn then
			self._controllers_pc_mapping[ btn._data.pc_btn:key() ] = btn
		end
		self._controllers_mapping[ btn._data.btn:key() ] = btn
		btn:set_text_params( data.btn_text_params )
		btn:set_order( i )
	end
	
	local num_btns = #btns
	local h = (self._real_small_font_size or small_font_size)
	local info_box_panel = self._panel:child( "info_box_panel" )
	-- self._btn_panel:set_debug(true)
	self._btn_panel:set_h( num_btns * h )
	self._btn_panel:set_rightbottom( self._panel:w()-2, info_box_panel:bottom()-2 )
	
	if self._comparision_panel then
		self._comparision_panel:set_h( (self._stats_shown and (#self._stats_shown) or 4) * (h) )
		self._comparision_panel:set_rightbottom( self._panel:w()-2, info_box_panel:bottom()- 10 - h * 5 - 5 ) -- self._btn_panel:top() - 5 )
	end
	if self._info_panel then
		self._info_panel:set_size( info_box_panel:w() - 20, self._info_panel:num_children() / 2 * h )
		self._info_panel:set_rightbottom( self._panel:w() - 10, self._btn_panel:top() - 10 )
	end
	
	if( managers.menu:is_pc_controller() and #btns > 0 ) then
		slot:set_btn_text( btns[1]:btn_text() )
	else
		slot:set_btn_text( "" )
	end
end


--[[----------------------------------------------------------------------------------------------------------------------------------
																	Population functions, to populate slots
------------------------------------------------------------------------------------------------------------------------------------]]

			-- start page
function BlackMarketGui:populate_weapon_category( category, data )
	local crafted_category = managers.blackmarket:get_crafted_category( category ) or {}
	
	local last_weapon = table.size( crafted_category ) == 1
	local last_unlocked_weapon = nil
	if not last_weapon then	-- need to check if weapons are unlocked or not, we might only have one unlocked weapon, then it is last weapon and cant be sold
		local category_size = table.size( crafted_category )
		for i, crafted in pairs( crafted_category ) do
			if not managers.blackmarket:weapon_unlocked( crafted.weapon_id ) then
				category_size = category_size - 1
			end
		end
		last_unlocked_weapon = category_size == 1
	end
	
	for i=1, 9 do
		data[i] = nil
	end
	
	local new_data = {}
	local index = 0
	for i, crafted in pairs( crafted_category ) do
		new_data = {}
		new_data.name = crafted.weapon_id
		new_data.name_localized = managers.weapon_factory:get_weapon_name_by_factory_id( crafted.factory_id )
		new_data.category = category
		new_data.slot = i
		new_data.unlocked = managers.blackmarket:weapon_unlocked( crafted.weapon_id )-- true
		new_data.level = (not new_data.unlocked and 0)
		new_data.lock_texture = not new_data.unlocked and (new_data.level==0 and "guis/textures/pd2/lock_skill" or "guis/textures/pd2/lock_level")
		new_data.can_afford = true
		new_data.equipped = crafted.equipped
		new_data.skill_name = new_data.level==0 and ( "bm_menu_skill_locked_" .. new_data.name )
		new_data.price = managers.money:get_weapon_slot_sell_value( category, i )
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/weapons/" .. tostring(crafted.weapon_id)
		new_data.comparision_data = managers.blackmarket:get_weapon_stats( category, i )
		-- new_data.last_weapon = ( new_data.unlocked and last_weapon or last_unlocked_weapon ) or nil
		
		if not new_data.unlocked then
			new_data.last_weapon = last_weapon
		else
			new_data.last_weapon = last_weapon or last_unlocked_weapon
		end
		local perks = managers.blackmarket:get_perks_from_weapon_blueprint( crafted.factory_id, crafted.blueprint )
		if table.size(perks) > 0 then
			new_data.mini_icons = {}
			
			local perk_index = 1
			for perk in pairs( perks ) do
				table.insert( new_data.mini_icons, { texture="guis/textures/pd2/blackmarket/inv_mod_" .. perk, right=(perk_index-1)*18, bottom=0, layer=1, w=16, h=16, stream=false } )
				perk_index = perk_index + 1
			end
		end
		
		if managers.blackmarket:got_new_drop( "normal", new_data.category, crafted.factory_id ) then
			new_data.mini_icons = new_data.mini_icons or {}
			table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
			new_data.new_drop_data = {} -- { new_data.global_value or "normal", "weapon_mods", mod_name }
		end
		
		-- if( new_data.comparision_data.alert_size ) then
		-- 	new_data.comparision_data.alert_size = #tweak_data.weapon.stats.alert_size - new_data.comparision_data.alert_size
		-- end
		if( new_data.equipped ) then
			self._equipped_comparision_data = self._equipped_comparision_data or {}
			self._equipped_comparision_data[category] = new_data.comparision_data
		end
		-- new_data.name ~= "glock_17" and new_data.name ~= "amcar"
		if( new_data.name ~= "saw" ) then table.insert( new_data, "w_mod" ) end
		if not new_data.last_weapon then table.insert( new_data, "w_sell" ) end
		if( not new_data.equipped and new_data.unlocked ) then table.insert( new_data, "w_equip" ) end
		table.insert( new_data, "w_preview" )
		
		data[i] = new_data
		index = i
	end
	
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "bm_menu_btn_buy_new_weapon"
			new_data.name_localized = managers.localization:text( "bm_menu_empty_weapon_slot" )
			new_data.mid_text = {}
			new_data.mid_text.selected_text = managers.localization:text( "bm_menu_btn_buy_new_weapon" )
			new_data.mid_text.selected_color = tweak_data.screen_colors.button_stage_2
			new_data.mid_text.noselected_text = new_data.name_localized
			new_data.mid_text.noselected_color = tweak_data.screen_colors.button_stage_3
			new_data.empty_slot = true
			new_data.category = category
			new_data.slot = i
			new_data.unlocked = true
			new_data.can_afford = true
			new_data.equipped = false
			table.insert( new_data, "ew_buy" )
			
			if managers.blackmarket:got_new_drop( new_data.category, "weapon_buy_empty", nil ) then
				new_data.mini_icons = new_data.mini_icons or {}
				table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false, visible=false } )
				new_data.new_drop_data = {} -- { new_data.global_value or "normal", "weapon_mods", mod_name }
			end
			
			data[i] = new_data
		end
	end
end

function BlackMarketGui:populate_primaries( data )
	self:populate_weapon_category( "primaries", data )
end

function BlackMarketGui:populate_secondaries( data )
	self:populate_weapon_category( "secondaries", data )
end

function BlackMarketGui:populate_characters( data )
	local new_data = {}
	for i=1, 4 do
		local character = CriminalsManager.character_workname_by_peer_id( i )
		
		new_data = {}
		new_data.name = character
		new_data.name_localized = managers.localization:text( "menu_"..new_data.name ) -- managers.criminals:convert_old_to_new_character_workname( new_data.name )
		new_data.category = "characters"
		new_data.slot = i
		new_data.unlocked = true
		new_data.equipped = managers.blackmarket:get_preferred_character() == character
		new_data.equipped_text = managers.localization:text( "bm_menu_preferred" )
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/characters/"..CriminalsManager.convert_old_to_new_character_workname( new_data.name )
		new_data.stream = false
		
		if not new_data.equipped then table.insert( new_data, "c_equip" ) end
		
		data[i] = new_data
	end
end

function BlackMarketGui:populate_deployables( data )
	local new_data = {}
	local sort_data = {}
	
	for id, d in pairs( tweak_data.blackmarket.deployables ) do
		table.insert( sort_data, { id, d } )
	end
	table.sort( sort_data, function( x, y )
		return x[1] < y[1]
	end )
	
	for i=1, 9 do
		data[i] = nil
	end
	
	local index = 0
	for i, deployable_data in ipairs( sort_data ) do
		new_data = {}
		new_data.name = deployable_data[1]
		new_data.name_localized = managers.localization:text( tweak_data.blackmarket.deployables[ new_data.name ].name_id )
		new_data.category = "deployables"
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/deployables/" .. tostring(new_data.name)
		new_data.slot = i
		new_data.unlocked = table.contains( managers.player:availible_equipment( 1 ), new_data.name )
		new_data.level = 0
		new_data.lock_texture = not new_data.unlocked and (new_data.level==0 and "guis/textures/pd2/lock_skill" or "guis/textures/pd2/lock_level")
		new_data.equipped = (managers.player:equipment_in_slot( 1 ) == new_data.name)
		new_data.stream = false
		new_data.skill_name = "bm_menu_skill_locked_" .. new_data.name
		-- table.insert( new_data, "btn_mod" )
		if( new_data.unlocked and not new_data.equipped ) then table.insert( new_data, "lo_d_equip" ) end
		
		data[i] = new_data
		index = i
	end
	
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = "deployables"
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			-- table.insert( new_data, "btn_open_buy" )
			
			data[i] = new_data
		end
	end
end

function BlackMarketGui:populate_masks( data )
	local new_data = {}
	local crafted_category = managers.blackmarket:get_crafted_category( "masks" ) or {}
	
	local mini_icon_helper = math.round( ( self._panel:h() - (tweak_data.menu.pd2_medium_font_size + 10) - 70 ) * GRID_H_MUL / 3 ) - 16
	for i=1, 9 do
		data[i] = nil
	end
	local index = 0
	for i, crafted in pairs( crafted_category ) do
		new_data = {}
		new_data.name = crafted.mask_id
		new_data.name_localized = managers.localization:text( tweak_data.blackmarket.masks[new_data.name].name_id )
		new_data.category = "masks"
		new_data.global_value = crafted.global_value
		new_data.slot = i
		new_data.unlocked = true
		new_data.equipped = crafted.equipped
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/masks/" .. new_data.name
		new_data.stream = false
		
		local is_locked = tweak_data.lootdrop.global_values[new_data.global_value].dlc and not tweak_data.dlc[new_data.global_value].free and not managers.dlc:has_dlc( new_data.global_value)
		
		if not is_locked then
			for _, part in pairs( crafted.blueprint ) do
				is_locked = tweak_data.lootdrop.global_values[part.global_value].dlc and not tweak_data.dlc[part.global_value].free and not managers.dlc:has_dlc( part.global_value)
				if is_locked then
					break
				end
			end
		end
		
		if is_locked then
			new_data.unlocked = false
			new_data.lock_texture = "guis/textures/pd2/lock_skill"
			new_data.dlc_locked = "bm_menu_dlc_locked"
		end
		
		if new_data.unlocked then
			if( not new_data.equipped ) then table.insert( new_data, "m_equip" ) end
			if( not crafted.modded and managers.blackmarket:can_modify_mask( i ) and i~=1 ) then table.insert( new_data, "m_mod" ) end
			if( i~=1 ) then table.insert( new_data, "m_preview" ) end
		end
		if( i~=1 ) then table.insert( new_data, "m_sell" ) end
		
		if( crafted.modded ) then
			new_data.mini_icons = {}
			local color_1 = tweak_data.blackmarket.colors[crafted.blueprint.color.id].colors[1]
			local color_2 = tweak_data.blackmarket.colors[crafted.blueprint.color.id].colors[2]
			table.insert( new_data.mini_icons, { texture=false, w=16, h=16, right=0, bottom=0, layer=1, color=color_2 } )
			table.insert( new_data.mini_icons, { texture=false, w=16, h=16, right=18, bottom=0, layer=1, color=color_1 } )
			
			local pattern = crafted.blueprint.pattern.id
			
			if pattern == "solidfirst" or pattern == "solidsecond" then
			else
				table.insert( new_data.mini_icons, { texture="guis/textures/pd2/blackmarket/icons/materials/" .. crafted.blueprint.material.id, right=-3, bottom=38 - (NOT_WIN_32 and 20 or 10), w=42, h=42, layer=1, stream=true } )
			end
			table.insert( new_data.mini_icons, { texture=tweak_data.blackmarket.textures[pattern].texture, right=-3, bottom=math.round(mini_icon_helper-6-6-42), w=42, h=42, layer=1, stream=true, render_template=Idstring( "VertexColorTexturedPatterns" ) } )
			
			new_data.mini_icons.borders = true
			
			--[[
			table.insert( new_data.mini_icons, { texture="guis/textures/pd2/blackmarket/icons/colors/color_small_01", pos_x=1, pos_y=1, layer=3, color=color_2 } )
			table.insert( new_data.mini_icons, { texture="guis/textures/pd2/blackmarket/icons/colors/color_small_02", pos_x=1, pos_y=1, layer=2, color=color_1 } )
			table.insert( new_data.mini_icons, { texture="guis/textures/pd2/blackmarket/icons/colors/color_small_bg", pos_x=1, pos_y=1, layer=1 } )
			table.insert( new_data.mini_icons, { texture="guis/textures/pd2/blackmarket/icons/materials/" .. crafted.blueprint.material.id, pos_x=1, pos_y=3, layer=1, stream=true } )
			
			table.insert( new_data.mini_icons, { texture=tweak_data.blackmarket.textures[crafted.blueprint.pattern.id].texture, pos_x=1, pos_y=5, layer=1, stream=true, render_template=Idstring( "VertexColorTexturedPatterns" ) } )
			]]
			-- ( ( self._panel:h() - (medium_font_size + 10) - 70 ) * GRID_H_MUL )
		else
			if i ~= 1 and managers.blackmarket:can_modify_mask( i ) and managers.blackmarket:got_new_drop( "normal", "mask_mods", crafted.mask_id ) then
				new_data.mini_icons = new_data.mini_icons or {}
				table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false, visible=true } )
				new_data.new_drop_data = {}
			end
		end
		
		-- table.insert( new_data, "w_preview" )
		
		data[i] = new_data
		index = i
	end
	
	local can_buy_masks = #managers.blackmarket:get_inventory_masks() > 0
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "bm_menu_btn_buy_new_mask"
			new_data.name_localized = managers.localization:text( "bm_menu_empty_mask_slot" )
			new_data.mid_text = {}
			new_data.mid_text.selected_text = can_buy_masks and managers.localization:text( "bm_menu_btn_buy_new_mask" ) or managers.localization:text( "bm_menu_empty_mask_slot" )
			new_data.mid_text.selected_color = tweak_data.screen_colors.button_stage_2
			new_data.mid_text.noselected_text = new_data.name_localized
			new_data.mid_text.noselected_color = tweak_data.screen_colors.button_stage_3
			new_data.empty_slot = true
			new_data.category = "masks"
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			new_data.num_backs = 0
			new_data.cannot_buy = not can_buy_masks
			
			if can_buy_masks then 
				table.insert( new_data, "em_buy" ) 
				
				if i ~= 1 and managers.blackmarket:got_new_drop( nil, "mask_buy", nil ) then
					new_data.mini_icons = new_data.mini_icons or {}
					table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false, visible=false } )
					new_data.new_drop_data = { }
				end
			end
			-- table.insert( new_data, "em_gv" )
			
			data[i] = new_data
		end
	end
end

function BlackMarketGui:populate_armors( data )
	local new_data = {}
	local sort_data = {}
	
	for i, d in pairs( tweak_data.blackmarket.armors ) do
		table.insert( sort_data, { i, d.name_id } )
	end
	table.sort( sort_data, function( x, y )
		return x[1] < y[1]
	end )
	
	local armor_level_data = {}
	for level, data in pairs( tweak_data.upgrades.level_tree ) do
		if data.upgrades then
			for _, upgrade in ipairs( data.upgrades ) do
				local def = tweak_data.upgrades.definitions[ upgrade ]
				if def.armor_id then
					armor_level_data[def.armor_id] = level
				end
			end
		end
	end
	
	local index = 0
	-- for armor_id, armor_data in pairs( tweak_data.blackmarket.armors ) do
	for i, armor_data in ipairs( sort_data ) do
		local armor_id = armor_data[1]
		local name_id = armor_data[2]
		local bm_data = Global.blackmarket_manager.armors[ armor_id ]
		-- if bm_data.owned then
			index = index + 1
			new_data = {}
			new_data.name = armor_id
			new_data.name_localized = managers.localization:text( name_id )
			new_data.category = "armors"
			new_data.slot = index
			new_data.unlocked = bm_data.unlocked
			new_data.level = armor_level_data[ armor_id ] or 0
			new_data.lock_texture = not new_data.unlocked and (new_data.level==0 and "guis/textures/pd2/lock_skill" or "guis/textures/pd2/lock_level")
			new_data.equipped = bm_data.equipped
			new_data.skill_name = new_data.level==0 and ( "bm_menu_skill_locked_" .. new_data.name )
			new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/armors/" .. new_data.name
			
			if i ~= 1 and managers.blackmarket:got_new_drop( "normal", "armors", armor_id ) then
				new_data.mini_icons = new_data.mini_icons or {}
				table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
				new_data.new_drop_data = { "normal", "armors", armor_id }
			end
			
			if( new_data.unlocked and not new_data.equipped ) then table.insert( new_data, "a_equip" ) end
			
			data[index] = new_data
		-- end
	end
	
	local max_armors = data.override_slots[1] * data.override_slots[2]
	for i=1, max_armors do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = "armors"
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
end

			-- weapon slot selected
function BlackMarketGui:populate_mod_types( data )
	local new_data = {}
	local index = 1
	for type, mods in pairs( data.on_create_data ) do
		new_data = {}
		new_data.name = type
		new_data.name_localized = managers.localization:text( "bm_menu_" .. tostring(type) )
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/mods/" .. new_data.name
		new_data.category = data.category
		new_data.slot = data.prev_node_data and data.prev_node_data.slot or 1
		new_data.unlocked = true
		new_data.equipped = false
		new_data.mods = mods
		table.insert( new_data, "mt_choose" )
		
		data[index] = new_data
		index = index + 1
	end
	
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
end

function BlackMarketGui:populate_mods( data )
	local new_data = {}
	local default_mod = data.on_create_data.default_mod
	local global_values = managers.blackmarket:get_crafted_category( data.prev_node_data.category )[data.prev_node_data.slot].global_values or {}
	
	local gvs = {}
	local mod_t = {}
	local num_steps = #data.on_create_data
	
	--[[
	-- for _, mod_t in ipairs( data.on_create_data ) do
	if not data.on_create_data.split_global_values then
		for i=num_steps, 1, -1 do
			mod_t = data.on_create_data[i]
			gvs = managers.blackmarket:get_inventory_item_global_values( "weapon_mods", mod_t[1] ) or {}
			local base_global_value = tweak_data.blackmarket.weapon_mods[ mod_t[1] ].infamous and "infamous" or "normal"
			local cgv = managers.blackmarket:get_crafted_part_global_value( data.category, data.slot or data.prev_node_data and data.prev_node_data.slot, mod_t[1] ) or "normal"
			
			if cgv ~= base_global_value then
				if not table.contains( gvs, cgv ) then
					table.insert( data.on_create_data, i, {mod_t[1], mod_t[2], cgv} )
				end
			end
			
			for index, gv in ipairs( gvs ) do
				if gv ~= base_global_value then
					table.insert( data.on_create_data, i, {mod_t[1], mod_t[2], gv} )
				end
			end
		end
	end
	data.on_create_data.split_global_values = true]]
	
	for index, mod_t in ipairs( data.on_create_data ) do
		local mod_name = mod_t[1]
		local mod_default = mod_t[2]
		local mod_global_value = mod_t[3] or "normal"
		
		new_data = {}
		new_data.name = mod_name or data.prev_node_data.name
		new_data.name_localized = (mod_name and managers.weapon_factory:get_part_name_by_part_id(mod_name)) or managers.localization:text( "bm_menu_no_mod" )
		new_data.category = data.category or data.prev_node_data and data.prev_node_data.category
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/mods/" .. new_data.name
		new_data.slot = data.slot or data.prev_node_data and data.prev_node_data.slot
		new_data.global_value = mod_global_value -- global_values[new_data.name] or "normal" -- tweak_data.blackmarket.weapon_mods[new_data.name].infamous and "infamous" or "normal"
		new_data.unlocked = mod_default or managers.blackmarket:get_item_amount( new_data.global_value, "weapon_mods", new_data.name )	-- can_modify_weapon
		new_data.equipped = false
		new_data.stream = true
		new_data.default_mod = default_mod
		
		
		if tweak_data.lootdrop.global_values[mod_global_value].dlc and not tweak_data.dlc[mod_global_value].free and not managers.dlc:has_dlc( mod_global_value) then -- Global.dlc_save.packages[ mod_global_value ] == false then
			new_data.unlocked = -math.abs(new_data.unlocked)
			new_data.lock_texture = "guis/textures/pd2/lock_skill"
			new_data.dlc_locked = "bm_menu_dlc_locked"
		end
		
		local weapon_id = managers.blackmarket:get_crafted_category( new_data.category )[new_data.slot].weapon_id
		new_data.price = managers.money:get_weapon_modify_price( weapon_id, new_data.name, new_data.global_value )
		new_data.can_afford = managers.money:can_afford_weapon_modification( weapon_id, new_data.name, new_data.global_value )
		
		
		if not new_data.unlocked or new_data.unlocked == 0 then
			new_data.mid_text = {}
			new_data.mid_text.selected_text = managers.localization:text( "bm_menu_no_items" )
			new_data.mid_text.selected_color = tweak_data.screen_colors.text
			new_data.mid_text.noselected_text = new_data.mid_text.selected_text
			new_data.mid_text.noselected_color = tweak_data.screen_colors.text
			new_data.mid_text.vertical="center"
		end
		
		if( mod_name ) then
			local forbids = managers.blackmarket:can_modify_weapon( new_data.category, new_data.slot, new_data.name )
			if( forbids and #forbids > 0 ) then
				if( type(new_data.unlocked) == "number" ) then
					new_data.unlocked = -new_data.unlocked
				else
					new_data.unlocked = false
				end
				new_data.lock_texture = new_data.unlocked and new_data.unlocked~=0 and "guis/textures/pd2/lock_incompatible"
				new_data.conflict = managers.localization:text( "bm_menu_" .. tostring( tweak_data.weapon.factory.parts[ forbids[1] ].type ) ) -- managers.weapon_factory:get_part_name_by_part_id( forbids[1] )
			end
			
			local perks = managers.blackmarket:get_perks_from_part( new_data.name )
			if table.size(perks) > 0 then
				new_data.desc_mini_icons = {}
				
				local perk_index = 1
				for perk in pairs( perks ) do
					table.insert( new_data.desc_mini_icons, { texture="guis/textures/pd2/blackmarket/inv_mod_" .. perk, right=(perk_index-1)*18, bottom=0, w=16, h=16 } )
					perk_index = perk_index + 1
				end
			end
			
			if not new_data.conflict and new_data.unlocked then
					new_data.comparision_data = managers.blackmarket:get_weapon_stats_with_mod( new_data.category, new_data.slot, mod_name )
				-- if( new_data.comparision_data and new_data.comparision_data.alert_size ) then
				-- 	new_data.comparision_data.alert_size = #tweak_data.weapon.stats.alert_size - new_data.comparision_data.alert_size
				-- end
			end
			
			if managers.blackmarket:got_new_drop( mod_global_value, "weapon_mods", mod_name ) then
				new_data.mini_icons = new_data.mini_icons or {}
				table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
				new_data.new_drop_data = { new_data.global_value or "normal", "weapon_mods", mod_name }
			end
		end
		
		if( mod_name and new_data.unlocked ) then
			if( type( new_data.unlocked ) ~= "number" or new_data.unlocked > 0 ) then
				if new_data.can_afford then
					table.insert( new_data, "wm_buy" )
				end
				table.insert( new_data, "wm_preview" )
				table.insert( new_data, "wm_preview_mod" )
			else
				table.insert( new_data, "wm_remove_preview" )
			end
		end
		-- view_weapon_with_mod
		
		data[index] = new_data
	end
		
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
	
	local weapon_blueprint = managers.blackmarket:get_weapon_blueprint( data.prev_node_data.category, data.prev_node_data.slot ) or {}
	local equipped = nil
	for i, mod in ipairs( data ) do
		for _, weapon_mod in ipairs( weapon_blueprint ) do
			if mod.name == weapon_mod and (not global_values[weapon_mod] or global_values[weapon_mod] == data[i].global_value) then
				-- self._equipped_part_id = weapon_mod
				equipped = i
				break
			end
		end
	end
	
	if equipped then
		data[equipped].equipped = true
		data[equipped].unlocked = data[equipped].unlocked or true
		data[equipped].mid_text = nil
		
		for i=1, #data[equipped] do
			table.remove( data[equipped], 1 )
		end
		
		data[equipped].price = 0
		data[equipped].can_afford = true
		-- if data[equipped].can_afford then
			table.insert( data[equipped], "wm_remove_buy" )
		-- end
		table.insert( data[equipped], "wm_remove_preview_mod" )
		table.insert( data[equipped], "wm_remove_preview" )
		
		
		if not data[equipped].conflict then
			if data[equipped].default_mod then
				data[equipped].comparision_data = managers.blackmarket:get_weapon_stats_with_mod( data[equipped].category, data[equipped].slot, data[equipped].default_mod )
			else
				data[equipped].comparision_data = managers.blackmarket:get_weapon_stats_without_mod( data[equipped].category, data[equipped].slot, data[equipped].name )
			end
		end
		-- if( #data[equipped] == 0 ) then
			-- table.insert( data[equipped], "wm_preview" )
		-- end
		--[[
		data[equipped].comparision_data = managers.blackmarket:get_weapon_stats_without_mod( data[equipped].category, data[equipped].slot, self._equipped_part_id )
	
		if( data[equipped].comparision_data.alert_size ) then
			data[equipped].comparision_data.alert_size = #tweak_data.weapon.stats.alert_size - data[equipped].comparision_data.alert_size
		end]]
	end
	
	--[[
	if( not data[1].comparision_data ) then
		if( self._equipped_part_id ) then
			data[1].comparision_data = managers.blackmarket:get_weapon_stats_without_mod( data[equipped].category, data[equipped].slot, self._equipped_part_id )
		else
			data[1].comparision_data = managers.blackmarket:get_weapon_stats( data[equipped].category, data[equipped].slot )
		end
		
		if( data[1].comparision_data.alert_size ) then
			data[1].comparision_data.alert_size = #tweak_data.weapon.stats.alert_size - data[1].comparision_data.alert_size
		end
	end]]
	
	-- print( "INS", data.category, data.slot, inspect(self._equipped_comparision_data[data.category]) )
end

function BlackMarketGui:set_equipped_comparision( data )
	local category = data.category
	local slot = data.slot
	
	self._equipped_comparision_data = {}
	self._equipped_comparision_data[category] = managers.blackmarket:get_weapon_stats( category, slot ) or {}
	
	-- if( self._equipped_comparision_data[category].alert_size ) then
	-- 	self._equipped_comparision_data[category].alert_size = #tweak_data.weapon.stats.alert_size - self._equipped_comparision_data[category].alert_size
	-- end
end

function BlackMarketGui:populate_buy_weapon( data )
	local new_data = {}
	
	for i=1, #data.on_create_data do
		new_data = {}
		new_data.name = data.on_create_data[i].weapon_id
		new_data.name_localized = managers.weapon_factory:get_weapon_name_by_factory_id( data.on_create_data[i].factory_id )
		new_data.category = data.category
		new_data.slot = data.prev_node_data and data.prev_node_data.slot
		new_data.unlocked = data.on_create_data[i].unlocked
		new_data.level = not new_data.unlocked and data.on_create_data[i].level
		new_data.lock_texture = not new_data.unlocked and (new_data.level==0 and "guis/textures/pd2/lock_skill" or "guis/textures/pd2/lock_level")
		new_data.equipped = false
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/weapons/" .. new_data.name
		new_data.comparision_data = new_data.unlocked and deep_clone( tweak_data.weapon[ new_data.name ].stats )
		new_data.skill_name = new_data.level==0 and ( "bm_menu_skill_locked_" .. new_data.name )
		new_data.can_afford = managers.money:can_afford_weapon( new_data.name )
		new_data.price = managers.money:get_weapon_price_modified( new_data.name )
		new_data.not_moddable = true
		
		if new_data.unlocked and not new_data.can_afford then
			new_data.mid_text = {}
			new_data.mid_text.selected_text = managers.localization:text( "bm_menu_not_enough_cash" )
			new_data.mid_text.selected_color = tweak_data.screen_colors.text
			new_data.mid_text.noselected_text = new_data.mid_text.selected_text
			new_data.mid_text.noselected_color = tweak_data.screen_colors.text
			new_data.mid_text.vertical="center"
		end
		-- if( new_data.comparision_data and new_data.comparision_data.alert_size ) then
		-- 	new_data.comparision_data.alert_size = #tweak_data.weapon.stats.alert_size - new_data.comparision_data.alert_size
		-- end
		if( new_data.unlocked ) then
			if( new_data.can_afford ) then
				table.insert( new_data, "bw_buy" )
			end
			table.insert( new_data, "bw_preview" )
		end
		
		local new_weapon = managers.blackmarket:got_new_drop( data.category, "weapon_buy", new_data.name )
		local got_mods = managers.blackmarket:got_new_drop( "normal", new_data.category, data.on_create_data[i].factory_id )
		
		if new_weapon or got_mods then
			new_data.mini_icons = new_data.mini_icons or {}
			table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
			new_data.new_drop_data = { "normal", data.category, new_data.name }
			if got_mods then
				new_data.new_drop_data = {}
			end
		end
		data[i] = new_data
	end
	
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
end

 -- masks
function BlackMarketGui:populate_mask_global_value( data )
	local new_data = {}
	if not managers.blackmarket:currently_customizing_mask() then
		return
	end
	
	for i=1, #data.on_create_data do
		new_data = {}
		new_data.name = data.on_create_data[i]
		new_data.name_localized = data.on_create_data[i]
		new_data.category = data.category
		new_data.slot = data.prev_node_data and data.prev_node_data.slot
		new_data.unlocked =  true
		new_data.equipped = false
		new_data.num_backs = data.prev_node_data.num_backs + 1
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/global_value/" .. new_data.name
		new_data.stream = true
		-- new_data.comparision_data = new_data.unlocked and deep_clone( tweak_data.weapon[ new_data.name ].stats )
		-- new_data.level = not new_data.unlocked and data.on_create_data[i].level
		
		if( new_data.unlocked ) then
			if( #managers.blackmarket:get_inventory_masks() > 0 ) then table.insert( new_data, "em_buy" ) end
			-- table.insert( new_data, "bw_buy" )
			-- table.insert( new_data, "bw_preview" )
		end
		data[i] = new_data
	end
	
	for i=1, 9 do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
end

function BlackMarketGui:populate_buy_mask( data )
	local new_data = {}
	
	local max_masks = data.override_slots[1] * data.override_slots[2]
	for i=1, max_masks do
		data[i] = nil
	end
	for i=1, #data.on_create_data do
		new_data = {}
		new_data.name = data.on_create_data[i].mask_id
		new_data.name_localized = managers.localization:text( tweak_data.blackmarket.masks[new_data.name].name_id )
		new_data.category = data.category
		new_data.slot = data.prev_node_data and data.prev_node_data.slot
		new_data.global_value = data.on_create_data[i].global_value
		new_data.unlocked = managers.blackmarket:get_item_amount( new_data.global_value, "masks", new_data.name ) -- data.on_create_data[i].amount
		new_data.equipped = false
		new_data.num_backs = data.prev_node_data.num_backs + 1
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/masks/" .. new_data.name
		new_data.stream = true
		-- new_data.comparision_data = new_data.unlocked and deep_clone( tweak_data.weapon[ new_data.name ].stats )
		-- new_data.level = not new_data.unlocked and data.on_create_data[i].level
		
		if tweak_data.lootdrop.global_values[new_data.global_value].dlc and not tweak_data.dlc[new_data.global_value].free and not managers.dlc:has_dlc( new_data.global_value) then
			new_data.unlocked = -math.abs(new_data.unlocked)
			new_data.lock_texture = "guis/textures/pd2/lock_skill"
			new_data.dlc_locked = "bm_menu_dlc_locked"
		end
		
		if new_data.unlocked and new_data.unlocked > 0 then
			table.insert( new_data, "bm_buy" )
			table.insert( new_data, "bm_preview" )
			table.insert( new_data, "bm_sell" )
		end
		
		if managers.blackmarket:got_new_drop( new_data.global_value or "normal", "masks", new_data.name ) then
			new_data.mini_icons = new_data.mini_icons or {}
			table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
			new_data.new_drop_data = { new_data.global_value or "normal", "masks", new_data.name }
		end
		data[i] = new_data
	end
	
	for i=1, max_masks do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
end

function BlackMarketGui:populate_mask_mod_types( data )
	local new_data = {}
	local max_page = data.override_slots[1] * data.override_slots[2]
	for i=1, max_page do
		data[i] = nil
	end
	
	local all_mods_by_type = {}
	local full_customization = true
	for type, mods in pairs( data.on_create_data ) do
		all_mods_by_type[type] = mods
		full_customization = full_customization and ( ( type == "materials" ) or ( mods and #mods > 0 ) )
	end
	
	local mask_blueprint = managers.blackmarket:info_customize_mask()
	local mask_true_blueprint = managers.blackmarket:get_customized_mask_blueprint()
	local name_converter = { materials="material", textures="pattern", colors="color" }
	
	local index = 1
	for type, mods in pairs( data.on_create_data ) do
		new_data = {}
		new_data.name = type
		new_data.name_localized = managers.localization:text( "bm_menu_" .. tostring(type) )
		new_data.category = type -- data.category
		new_data.slot = data.prev_node_data and data.prev_node_data.slot
		new_data.unlocked = type=="materials" or #mods > 0
		new_data.equipped = false
		new_data.mods = mods
		new_data.equipped_text = managers.localization:text( "bm_menu_chosen" )
		new_data.all_mods_by_type = all_mods_by_type
		-- new_data.global_value = mods.global_value
		-- new_data.comparision_data = new_data.unlocked and deep_clone( tweak_data.weapon[ new_data.name ].stats )
		-- new_data.level = not new_data.unlocked and data.on_create_data[i].level
		new_data.my_part_data = nil
		new_data.my_true_part_data = mask_true_blueprint[ name_converter[type] ]
		for i, data in ipairs( mask_blueprint ) do
			if data.name == type then
				new_data.my_part_data = data
				break
			end
		end
		-- new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/" .. new_data.name
		new_data.stream = type~="colors"
		
		if not new_data.my_part_data.is_good then
		else
			if type == "colors" then
				new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/colors/color_bg"
				
				new_data.extra_bitmaps = {}
				table.insert( new_data.extra_bitmaps, "guis/textures/pd2/blackmarket/icons/colors/color_02" )
				table.insert( new_data.extra_bitmaps, "guis/textures/pd2/blackmarket/icons/colors/color_01" )
				
				new_data.extra_bitmaps_colors = {}
				table.insert( new_data.extra_bitmaps_colors, tweak_data.blackmarket.colors[new_data.my_part_data.id].colors[2] )
				table.insert( new_data.extra_bitmaps_colors, tweak_data.blackmarket.colors[new_data.my_part_data.id].colors[1] )
			elseif type == "textures" then
				
				new_data.bitmap_texture = tweak_data.blackmarket.textures[new_data.my_part_data.id].texture
				new_data.render_template = Idstring( "VertexColorTexturedPatterns" )
			else
				new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/materials/" .. new_data.my_part_data.id
			end
		end
		
		new_data.unique_slot_class="BlackMarketGuiMaskSlotItem"
		
		new_data.btn_text_params = { type=managers.localization:text( "bm_menu_" .. tostring(type) ) }
		
		if managers.menu:is_pc_controller() then
			table.insert( new_data, "mm_choose_materials" )
			table.insert( new_data, "mm_choose_textures" )
			if data.on_create_data.colors and #data.on_create_data.colors > 0 then table.insert( new_data, "mm_choose_colors" ) end
		else
			if new_data.unlocked then table.insert( new_data, "mm_choose_" .. tostring(type) ) end
		end
			--[[
		if( full_customization ) then 
			new_data.btn_text_params = { type=managers.localization:text( "bm_menu_" .. tostring(type) ) }
			
			if managers.menu:is_pc_controller() then
				table.insert( new_data, "mm_choose_materials" )
				table.insert( new_data, "mm_choose_textures" )
				table.insert( new_data, "mm_choose_colors" )
			else
				table.insert( new_data, "mm_choose_" .. tostring(type) )
			end
		else
			if managers.menu:is_pc_controller() then
				table.insert( new_data, "mm_choose_materials" )
				table.insert( new_data, "mm_choose_materials" )
				table.insert( new_data, "mm_choose_textures" )
				table.insert( new_data, "mm_choose_colors" )
			elseif tostring(type) == "materials" then
				table.insert( new_data, "mm_choose_" .. tostring(type) )
			end
		end]]
		
		if not new_data.unlocked or new_data.unlocked == 0 then
			new_data.mid_text = {}
			new_data.mid_text.selected_text = managers.localization:text( "bm_menu_no_items" )
			new_data.mid_text.selected_color = tweak_data.screen_colors.text
			new_data.mid_text.noselected_text = new_data.mid_text.selected_text
			new_data.mid_text.noselected_color = tweak_data.screen_colors.text
			new_data.mid_text.vertical="center"
		end
		
		if managers.menu:is_pc_controller() then
			new_data.double_click_btn = "mm_choose_" .. tostring(type)
		end
		
		table.insert( new_data, "mm_preview" )
		if( managers.blackmarket:can_finish_customize_mask() ) then
			if managers.blackmarket:can_afford_customize_mask() then
				table.insert( new_data, "mm_buy" )
			end
		end
		
		if managers.blackmarket:got_new_drop( nil, new_data.category, nil ) then
			new_data.mini_icons = new_data.mini_icons or {}
			table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
			new_data.new_drop_data = {}
		end
		-- table.insert( new_data, "bm_preview" )
		
		data[index] = new_data
		index = index + 1
	end
	
	local name_values = { materials=1, textures=2, colors=3 }
	table.sort( data, function( x, y ) return name_values[x.name] < name_values[y.name] end )
	
	for i=1, max_page do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			--[[
			if managers.blackmarket:can_afford_customize_mask() then
				table.insert( new_data, "mm_buy" )
			end
			if( managers.blackmarket:can_finish_customize_mask() ) then
				table.insert( new_data, "mm_preview" )
			end
			]]
			data[i] = new_data
		end
	end
	
	--[[
	-- if( not data[9] ) then
		new_data = {}
		new_data.name = "bm_menu_mask_options"
		new_data.category = data.category
		new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/mask_options"
		new_data.hide_bg = true
		new_data.slot = 9
		new_data.unlocked = managers.blackmarket:can_finish_customize_mask()
		new_data.equipped = false
		new_data.stream = true
		
		if( managers.blackmarket:can_finish_customize_mask() ) then
			table.insert( new_data, "mm_buy" )
			table.insert( new_data, "mm_preview" )
		end
		data[9] = new_data
	-- end
	]]
end

function BlackMarketGui:populate_choose_mask_mod( data )
	local new_data = {}
	local index = 1
	local equipped_mod = managers.blackmarket:customize_mask_category_id( data.category )
	for type, mods in pairs( data.on_create_data ) do
		new_data = {}
		new_data.name = mods.id
		new_data.name_localized =  managers.localization:text( tweak_data.blackmarket[data.category][new_data.name].name_id )
		new_data.category = data.category
		new_data.slot = index -- data.prev_node_data and data.prev_node_data.slot
		new_data.unlocked = mods.default or mods.amount
		new_data.equipped = equipped_mod == mods.id
		new_data.equipped_text = managers.localization:text( "bm_menu_chosen" )
		new_data.mods = mods
		new_data.stream = data.category~="colors"
		new_data.global_value = mods.global_value
		
		local is_locked = false
		if tweak_data.lootdrop.global_values[new_data.global_value].dlc and not tweak_data.dlc[new_data.global_value].free and not managers.dlc:has_dlc( new_data.global_value) then
			new_data.unlocked = -math.abs(new_data.unlocked)
			new_data.lock_texture = "guis/textures/pd2/lock_skill"
			new_data.dlc_locked = "bm_menu_dlc_locked"
			is_locked = true
		end
		if data.category == "colors" then
			new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/colors/color_bg"
			
			new_data.extra_bitmaps = {}
			table.insert( new_data.extra_bitmaps, "guis/textures/pd2/blackmarket/icons/colors/color_02" )
			table.insert( new_data.extra_bitmaps, "guis/textures/pd2/blackmarket/icons/colors/color_01" )
			
			new_data.extra_bitmaps_colors = {}
			table.insert( new_data.extra_bitmaps_colors, tweak_data.blackmarket.colors[new_data.name].colors[2] )
			table.insert( new_data.extra_bitmaps_colors, tweak_data.blackmarket.colors[new_data.name].colors[1] )
		elseif data.category == "textures" then
			-- if mods.id == "no_color_full_material" then
			-- 	new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/clean_texture"
			-- else
				new_data.bitmap_texture = tweak_data.blackmarket[data.category][mods.id].texture
			-- end
			new_data.render_template = Idstring( "VertexColorTexturedPatterns" )
		else
			new_data.bitmap_texture = "guis/textures/pd2/blackmarket/icons/" .. tostring(data.category) .. "/" .. new_data.name
		end
		
		if managers.blackmarket:got_new_drop( new_data.global_value or "normal", new_data.category, new_data.name ) then
			new_data.mini_icons = new_data.mini_icons or {}
			table.insert( new_data.mini_icons, { name="new_drop", texture="guis/textures/pd2/blackmarket/inv_newdrop", left=0, top=0, layer=1, w=16, h=16, stream=false } )
			new_data.new_drop_data = { new_data.global_value or "normal", new_data.category, new_data.name }
		end
		-- new_data.comparision_data = new_data.unlocked and deep_clone( tweak_data.weapon[ new_data.name ].stats )
		-- new_data.level = not new_data.unlocked and data.on_create_data[i].level
		
		new_data.btn_text_params = { type=managers.localization:text( "bm_menu_" .. data.category ) }
		
		if not is_locked then
			table.insert( new_data, "mp_choose" )
			table.insert( new_data, "mp_preview" )
		end
		--[[
		if( managers.blackmarket:can_view_customized_mask_with_mod( data.category, mods.id, mods.global_value ) ) then 
			table.insert( new_data, "mp_preview_mod" )
		else
			-- table.insert( new_data, "mp_preview" )
		end]]
		-- if( managers.blackmarket:can_view_customized_mask_with_mod( data.category, mods.id, mods.global_value ) ) then table.insert( new_data, "mp_preview_mod" ) end
		
		-- table.insert( new_data, "bm_preview" )
		
		data[index] = new_data
		index = index + 1
	end
	
	local max_mask_mods = 9
	
	if data.override_slots then
		max_mask_mods = data.override_slots[1] * data.override_slots[2]
	end
	
	for i=1, max_mask_mods do
		if( not data[i] ) then
			new_data = {}
			new_data.name = "empty"
			new_data.name_localized = ""
			new_data.category = data.category
			new_data.slot = i
			new_data.unlocked = true
			new_data.equipped = false
			
			data[i] = new_data
		end
	end
end

			-- cleanup stash so that we dont get lua errors when blackmarket stuff is changed / removed
function BlackMarketGui:_cleanup_blackmarket()
	local blackmarket_tweak_data = tweak_data.blackmarket
	local blackmarket_inventory = Global.blackmarket_manager.inventory
	
	for global_value, gv_table in pairs( blackmarket_inventory ) do
		for type_id, type_table in pairs( gv_table ) do
			if type_id ~= "weapon_mods" then
				local item_data = blackmarket_tweak_data[type_id]
				if item_data then
					for item_id, item_amount in pairs( type_table ) do
						if not item_data[item_id] then
							print( "BlackMarketGui:_cleanup_blackmarket: Missing '" .. item_id .. "' in BlackMarketTweakData. Removing it from stash!" )
							type_table[item_id] = nil
						end
					end
				end
			else
				-- add for weapon mods?
			end
		end
	end
end


--[[----------------------------------------------------------------------------------------------------------------------------------
																	Button callbacks function, when you click buttons
------------------------------------------------------------------------------------------------------------------------------------]]


			-- start page, at enter on blackmarket, not on loadout
function BlackMarketGui:_start_page_data()
	local data = {}
	table.insert( data, { name="bm_menu_primaries", category="primaries", on_create_func_name="populate_primaries", identifier=self.identifiers["weapon"] } )
	table.insert( data, { name="bm_menu_secondaries", category="secondaries", on_create_func_name="populate_secondaries", identifier=self.identifiers["weapon"] } )
	table.insert( data, { name="bm_menu_armors", category="armors", on_create_func_name="populate_armors", override_slots={ 4, 2 }, identifier=self.identifiers["armor"] } )
	table.insert( data, { name="bm_menu_deployables", category="deployables", on_create_func_name="populate_deployables", identifier=Idstring( "deployable" ) } )
	table.insert( data, { name="bm_menu_masks", category="masks", on_create_func_name="populate_masks", identifier=self.identifiers["mask"] } )
	
	if not managers.network:session() then
		table.insert( data, { name="bm_menu_characters", category="characters", on_create_func_name="populate_characters", override_slots={ 2, 2 }, identifier=self.identifiers["character"] } )
	end
	data.topic_id = "menu_inventory"
	
	self:_cleanup_blackmarket()
	
	return data
end

			-- weapon slot selected
function BlackMarketGui:choose_weapon_mods_callback( data )
	local dropable_mods = managers.blackmarket:get_dropable_mods_by_weapon_id( data.name )  -- managers.weapon_factory:get_parts_from_weapon_id( data.name )
	
	local factory_id = managers.weapon_factory:get_factory_id_by_weapon_id( data.name ) 
	local default_blueprint = managers.weapon_factory:get_default_blueprint_by_factory_id( factory_id )
	
	
	for _, default_part in ipairs( default_blueprint ) do
		for i, mod_category in pairs( dropable_mods ) do
			-- table.delete( mod_category, default_part )
		end
	end
	
	
	--[[
	local equipped_slot_data = managers.blackmarket:get_crafted_category_slot( data.category, data.slot )
	local equipped_mods = deep_clone( equipped_slot_data.blueprint )
	
	local factory_id = managers.weapon_factory:get_factory_id_by_weapon_id( data.name ) 
	local default_blueprint = managers.weapon_factory:get_default_blueprint_by_factory_id( factory_id )
	
	for _, default_part in ipairs( default_blueprint ) do
		table.delete( equipped_mods, default_part )
		
		for i, mod_category in pairs( dropable_mods ) do
			table.delete( mod_category, default_part )
		end
	end
	
	local combined_mods = {}
	for drop_key, drop_data in pairs( dropable_mods ) do
		combined_mods[ drop_key ] = drop_data
	end
	
	local weapon_factory_tweak_data = tweak_data.weapon.factory.parts
	for _, d in ipairs( equipped_mods ) do
		if weapon_factory_tweak_data[d] then
			if combined_mods[weapon_factory_tweak_data[d].type] then
				table.insert( combined_mods[weapon_factory_tweak_data[d].type], d )
			end
		end
	end
	
	]]
	
	
	local mods = {}
	for i, d in pairs( dropable_mods ) do
		mods[ i ] = d -- table.list_union( d )
	end
	local new_node_data = {}
	local sort_mods = {}
	
	for id, _ in pairs( mods ) do
		table.insert( sort_mods, id )
	end
	table.sort( sort_mods, function( x, y ) return x < y end )
	
	for i, id in ipairs( sort_mods ) do
		local my_mods = deep_clone( mods[id] )
		
		local factory_id = managers.blackmarket:get_crafted_category( data.category )[data.slot].factory_id
		local default_blueprint = managers.weapon_factory:get_default_blueprint_by_factory_id( factory_id )
		local default_mod = nil
		
		local ids_id = Idstring( id )
		for i, d_mod in ipairs( default_blueprint ) do
			if( Idstring( tweak_data.weapon.factory.parts[d_mod].type ) == ids_id ) then
				default_mod = d_mod
				break;
			end
		end
		
		
		--[[
		local no_mod_choice = true
		local ids_id = Idstring( id )
		for i, d_mod in ipairs( default_blueprint ) do
			if( Idstring( tweak_data.weapon.factory.parts[d_mod].type ) == ids_id ) then
				table.insert( my_mods, 1, d_mod )
				no_mod_choice = false
				break;
			end
		end
		
		if( no_mod_choice ) then
			table.insert( my_mods, 1, false )
		end
		]]
		
		local sort_td = tweak_data.blackmarket.weapon_mods
		local x_td
		local y_td
		local x_pc
		local y_pc
		table.sort( my_mods, function( x, y )
			x_td = sort_td[ x[1] ]
			y_td = sort_td[ y[1] ]
			
			x_pc = x_td.pc or (x_td.pcs and x_td.pcs[1]) or 0
			y_pc = y_td.pc or (y_td.pcs and y_td.pcs[1]) or 0
			
			if x_td.infamous then
				x_pc = x_pc + 100
			end
			if y_td.infamous then
				y_pc = y_pc + 100
			end
			
			return ( x_pc < y_pc ) or ( x_pc == y_pc and x[1] < y[1] )
		end )
	
	
		local mod_data = {}
		for a=1, math.min( #my_mods, 9 ) do
			table.insert( mod_data, { my_mods[a][1], false, my_mods[a][2] } )
		end
		
		mod_data.default_mod = default_mod
		
		table.insert( new_node_data, { name=id, category=data.category, prev_node_data=data, name_localized=managers.localization:text( "bm_menu_" .. id ), on_create_func_name="populate_mods", on_create_data=mod_data, identifier=self.identifiers["weapon_mod"] } )
	end
	
	new_node_data.topic_id = "bm_menu_blackmarket_title"
	new_node_data.topic_params = { item=data.name_localized }
	new_node_data.show_tabs = true
	
	new_node_data.open_callback_name = "set_equipped_comparision"
	new_node_data.open_callback_params = { category=data.category, slot=data.slot }
	new_node_data.blur_fade = self._data.blur_fade
	
	managers.menu:open_node( self._inception_node_name, { new_node_data } )
end

function BlackMarketGui:choose_mod_type_callback( data )	-- old
	local mods = managers.blackmarket:get_dropable_mods_by_weapon_id( data.name )  -- managers.weapon_factory:get_parts_from_weapon_id( data.name )
	local new_node_data = {}
	
	local mods_list = {}
	for id, data in pairs( mods ) do
		table.insert( mods_list, id )
	end
	
	local mod_data = {}
	for i=1, math.max( 1, math.ceil( #mods_list / 9 ) ) do
		mod_data = {}
		for id=((i-1)*9)+1, math.min(i*9, #mods_list) do
			mod_data[ mods_list[id] ] = mods[ mods_list[id] ]
		end
		table.insert( new_node_data, { name=tostring(i), category=data.category, prev_node_data=data, name_localized=tostring(i) --[[managers.localization:text( "bm_menu_page", { page=i } )]], on_create_func_name="populate_mod_types", on_create_data=mod_data, identifier=self.identifiers["weapon_mod"] } )
	end
	
	new_node_data.topic_id = "bm_menu_blackmarket_title"
	new_node_data.topic_params = { item=data.name_localized }
	new_node_data.blur_fade = self._data.blur_fade
	
	managers.menu:open_node( self._inception_node_name, { new_node_data } )
end

function BlackMarketGui:set_preferred_character_callback( data )
	managers.blackmarket:set_preferred_character( data.name )
	self:reload()
end
function BlackMarketGui:equip_weapon_callback( data )	 -- for weapons in both blackmarket and loadout
	managers.blackmarket:equip_weapon( data.category, data.slot )
	self:reload()
end

function BlackMarketGui:equip_armor_callback( data )
	managers.blackmarket:equip_armor( data.name )
	self:reload()
end

function BlackMarketGui:equip_mask_callback( data )
	managers.blackmarket:equip_mask( data.slot )
	self:reload()
end

function BlackMarketGui:_open_preview_node()
	managers.menu:open_node( self._preview_node_name, {} )
end

function BlackMarketGui:_preview_weapon( data )
	managers.blackmarket:view_weapon( data.category, data.slot, callback( self, self, "_open_preview_node" ) )
end

function BlackMarketGui:preview_weapon_callback( data )
	-- local text = self._panel:text( { text="ASSEMBLING WEAPON\nPLEASE WAIT", align="center", vertical="center", font_size=tweak_data.menu.pd2_medium_font_size, font=tweak_data.menu.pd2_medium_font, layer=10 } )
	self:_preview_weapon( data )
	-- Setup:add_end_frame_callback( callback( self, self, "_preview_weapon", data ) )
end


function BlackMarketGui:_preview_mask( data )
	managers.blackmarket:view_mask( data.slot )
	managers.menu:open_node( "blackmarket_preview_mask_node", {} )
end

function BlackMarketGui:preview_mask_callback( data )
	-- local text = self._panel:text( { text="ASSEMBLING WEAPON\nPLEASE WAIT", align="center", vertical="center", font_size=tweak_data.menu.pd2_medium_font_size, font=tweak_data.menu.pd2_medium_font, layer=10 } )
	self:_preview_mask( data )
	-- Setup:add_end_frame_callback( callback( self, self, "_preview_weapon", data ) )
end

function BlackMarketGui:sell_item_callback( data )
	print( "sell_item_callback", inspect( data ) )
	local params = {}
	params.name = data.name_localized or data.name
	params.category = data.category
	params.slot = data.slot
	params.money = managers.experience:cash_string( managers.money:get_weapon_slot_sell_value( params.category, params.slot ) )
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_sell_weapon_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_sell( params )
end

function BlackMarketGui:sell_stashed_mask_callback( data )
	local blueprint = {}
	blueprint.color = { id = "nothing", global_value = "normal"}
	blueprint.pattern = { id = "no_color_no_material", global_value = "normal"}
	blueprint.material = { id = "plastic", global_value = "normal"}
	
	local params = {}
	params.name = data.name_localized or data.name
	params.global_value = data.global_value
	params.money = managers.experience:cash_string( managers.money:get_mask_sell_value( data.name, data.global_value, blueprint ) )
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_sell_inventory_mask_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_sell_no_slot( params )
	
	-- managers.blackmarket:on_sell_inventory_mask( data.name, data.global_value )
end

function BlackMarketGui:_sell_inventory_mask_callback( data )
	managers.menu_component:post_event( "item_sell" )
	managers.blackmarket:on_sell_inventory_mask( data.name, data.global_value )
	self:reload()
end

function BlackMarketGui:sell_mask_callback( data )
	local params = {}
	params.name = data.name_localized or data.name
	params.category = data.category
	params.slot = data.slot
	params.money = managers.experience:cash_string( managers.money:get_mask_slot_sell_value( data.slot ) )
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_sell_mask_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_sell( params )
end
 
function BlackMarketGui:_sell_weapon_callback( data )
	managers.menu_component:post_event( "item_sell" )
	-- check all category types and choose correct sell function prs
	managers.blackmarket:on_sell_weapon( data.category, data.slot )
	self:reload()
end

function BlackMarketGui:_sell_mask_callback( data )
	managers.menu_component:post_event( "item_sell" )
	managers.blackmarket:on_sell_mask( data.slot )
	self:reload()
end

			-- empty weapon slot selected
function BlackMarketGui:choose_weapon_buy_callback( data )
	local items = managers.blackmarket:get_weapon_category( data.category ) or {}
	local new_node_data = {}
	
	for _, item in ipairs( items ) do
		item.level = 0
		for level, level_data in pairs( tweak_data.upgrades.level_tree ) do
			for _, upgrade in ipairs( level_data.upgrades ) do
				if( upgrade == item.weapon_id ) then
					item.level = level
					break
				end
			end
			if( item.level ~= 0 ) then
				break
			end
		end
	end
	
	table.sort( items, function( x, y ) if( x.weapon_id == "saw" ) then return false end if( y.weapon_id == "saw" ) then return true end if( x.level and y.level ) then return x.level < y.level end end )
	
	local item_data = {}
	for i=1, math.ceil( #items / 9 ) do
		item_data = {}
		for id=((i-1)*9)+1, math.min(i*9, #items) do
			table.insert( item_data, items[id] )
		end
		table.insert( new_node_data, { name=tostring(i), category=data.category, prev_node_data=data, name_localized=tostring(i) --[[managers.localization:text( "bm_menu_page", { page=i } )]], on_create_func_name="populate_buy_weapon", on_create_data=item_data, identifier=self.identifiers["weapon"] } )
	end
	
	new_node_data.topic_id = "bm_menu_buy_weapon_title"
	new_node_data.topic_params = { weapon_category = managers.localization:text( "bm_menu_" .. data.category ) }
	new_node_data.blur_fade = self._data.blur_fade
	managers.menu:open_node( self._inception_node_name, { new_node_data } )
end


			-- empty mask slot selected
function BlackMarketGui:choose_mask_global_value_callback( data )
	local masks = managers.blackmarket:get_inventory_masks() or {}
	local new_node_data = {}
	
	local items = {}
	for _, mask in pairs( masks ) do
		local new_global_value = true
		for _, global_value in ipairs( items ) do
			if( global_value == mask.global_value ) then
				new_global_value = false
			end
		end
		if( new_global_value ) then
			table.insert( items, mask.global_value )
		end
	end
	
	table.sort( items, function( x, y ) 
		local global_values = { normal=1, superior=2, exceptional=3, infamous=4 }
		if( global_values[x] and global_values[y] ) then 
			return global_values[x] < global_values[y] 
		end 
	end )
	
	
	local item_data = {}
	for i=1, math.ceil( #items / 9 ) do
		item_data = {}
		for id=((i-1)*9)+1, math.min(i*9, #items) do
			table.insert( item_data, items[id] )
		end
		table.insert( new_node_data, { name=tostring(i), category=data.category, prev_node_data=data, name_localized=tostring(i) --[[managers.localization:text( "bm_menu_page", { page=i } )]], on_create_func_name="populate_mask_global_value", on_create_data=item_data, identifier=self.identifiers["mask"] } )
	end
	
	new_node_data.topic_id = "bm_menu_choose_global_value_title"
	new_node_data.topic_params = { category=managers.localization:text( "bm_menu_buy_mask_title" ) }
	new_node_data.blur_fade = self._data.blur_fade
	managers.menu:open_node( self._inception_node_name, { new_node_data } )
end

			-- empty mask slot selected
function BlackMarketGui:choose_mask_buy_callback( data )
	local masks = managers.blackmarket:get_inventory_masks() or {}
	local new_node_data = {}
	
	local items = {}
	for _, mask in pairs( masks ) do
		if( data.name == "bm_menu_btn_buy_new_mask" or mask.global_value == data.name ) then
			table.insert( items, mask )
		end
	end
	
	local sort_td = tweak_data.blackmarket.masks
	local x_td
	local y_td
	local x_pc
	local y_pc
	table.sort( items, function( x, y )
		x_td = sort_td[x.mask_id]
		y_td = sort_td[y.mask_id]
		
		x_pc = x_td.pc or (x_td.pcs and x_td.pcs[1]) or 0
		y_pc = y_td.pc or (y_td.pcs and y_td.pcs[1]) or 0
		
		x_pc = x_pc + (x.global_value and tweak_data.lootdrop.global_values[x.global_value].sort_number or 0)
		y_pc = y_pc + (y.global_value and tweak_data.lootdrop.global_values[y.global_value].sort_number or 0)
		
		return ( x_pc < y_pc ) or ( x_pc == y_pc and x.mask_id < y.mask_id )
	end )
	
	local max_x = math.clamp( math.round( #items / 6 ), 3, 6 )
	local max_y = 3
	
	local item_data = {}
	for i=1, math.ceil( #items / (max_x*max_y) ) do
		item_data = {}
		for id=((i-1)*(max_x*max_y) )+1, math.min(i*(max_x*max_y) , #items) do
			table.insert( item_data, items[id] )
		end
		table.insert( new_node_data, { name=tostring(i), category=data.category, prev_node_data=data, name_localized=tostring(i)--[[managers.localization:text( "bm_menu_page", { page=i } )]], on_create_func_name="populate_buy_mask", on_create_data=item_data, override_slots={max_x,max_y}, identifier=self.identifiers["mask"] } )
	end
	
	new_node_data.topic_id = "bm_menu_buy_mask_title"
	new_node_data.topic_params = {  }
	new_node_data.blur_fade = self._data.blur_fade
	managers.menu:open_node( self._inception_node_name, { new_node_data } )
end


			-- buy mask slot selected
function BlackMarketGui:buy_mask_callback( data )
	local params = {}
	params.name = data.name_localized or data.name
	params.category = data.category
	params.slot = data.slot
	params.weapon = data.name
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_buy_mask_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_assemble( params )
end


			-- mod mask selected
function BlackMarketGui:mask_mods_callback( data )
	local mods = { materials = managers.blackmarket:get_inventory_category( "materials" ), 
					textures = managers.blackmarket:get_inventory_category( "textures" ), 
					colors = managers.blackmarket:get_inventory_category( "colors" ) }
	local new_node_data = {}
	
	local mods_list = {}
	for id, data in pairs( mods ) do
		table.insert( mods_list, id )
	end
	local max_x = 3
	local max_y = 1
	local mod_data = {}
	for i=1, math.max( 1, math.ceil( #mods_list / (max_x*max_y) ) ) do
		mod_data = {}
		for id=((i-1)*(max_x*max_y))+1, math.min(i*(max_x*max_y), #mods_list) do
			mod_data[ mods_list[id] ] = mods[ mods_list[id] ]
		end
		table.insert( new_node_data, { name=tostring(i), category=data.category, prev_node_data=data, name_localized=tostring(i) --[[managers.localization:text( "bm_menu_page", { page=i } )]], on_create_func_name="populate_mask_mod_types", on_create_data=mod_data, override_slots={max_x,max_y}, identifier=self.identifiers["mask_mod"] } )
	end
	
	new_node_data.topic_id = "bm_menu_customize_mask_title"
	new_node_data.topic_params = { mask_name=data.name_localized }
	
	local params = {}
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_abort_customized_mask_callback" ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	
	new_node_data.back_callback = callback( self, self, "_warn_abort_customized_mask_callback", params ) --  function() managers.menu:show_confirm_blackmarket_abort( params ) return true end
	-- new_node_data.info_callback = self._data.info_callback or function() return managers.blackmarket:info_customize_mask() end
	
	managers.blackmarket:start_customize_mask( data.slot )
	new_node_data.blur_fade = self._data.blur_fade
	managers.menu:open_node( "blackmarket_mask_node", { new_node_data } )
end

function BlackMarketGui:choose_mask_mod_callback( type_category, data, prev_node_params )
	self:choose_mask_type_callback( data, prev_node_params, type_category )
end

function BlackMarketGui:choose_mask_type_callback( data, prev_node_params, type_category )
	if not managers.blackmarket:currently_customizing_mask() then
		return
	end
	
	local items = deep_clone( data.all_mods_by_type[type_category] ) or {}
	local new_node_data = {}
	local category = type_category or data.category
	
	local default = managers.blackmarket:customize_mask_category_default( category )
	local mods = {}
	
	if( default ) then
		table.insert( mods, default )
		mods[#mods].pcs = { 0 }
		mods[#mods].default = true
	end
	
	local td
	for i=1, #items do
		td = tweak_data.blackmarket[category][items[i].id]
		if( td.texture or td.colors ) then
			table.insert( mods, items[i] )
			
			mods[#mods].pc = td.pc or ( td.pcs and td.pcs[1] ) or 0
			mods[#mods].colors = td.colors
		end
	end
	
	local sort_td = tweak_data.blackmarket[category]
	local x_pc, y_pc
	table.sort( mods, function( x, y )
		if( x.colors and y.colors ) then
			for i=1, 2 do
				local x_color = x.colors[i]
				local x_max = math.max( x_color.r, x_color.g, x_color.b )
				local x_min = math.min( x_color.r, x_color.g, x_color.b )
				local x_diff = x_max - x_min
				local x_wl
				
				if( x_max == x_min ) then	-- x_diff == 0
					x_wl = 10 - x_color.r
				elseif( x_max == x_color.r ) then
					x_wl = ( x_color.g - x_color.b ) / x_diff % 6
				elseif( x_max == x_color.g ) then
					x_wl = ( x_color.b - x_color.r ) / x_diff + 2
				elseif( x_max == x_color.b ) then
					x_wl = ( x_color.r - x_color.g ) / x_diff + 4
				end
				
				local y_color = y.colors[i]
				local y_max = math.max( y_color.r, y_color.g, y_color.b )
				local y_min = math.min( y_color.r, y_color.g, y_color.b )
				local y_diff = y_max - y_min
				local y_wl
				
				if( y_max == y_min ) then
					y_wl = 10 - y_color.r
				elseif( y_max == y_color.r ) then
					y_wl = ( y_color.g - y_color.b ) / y_diff % 6
				elseif( y_max == y_color.g ) then
					y_wl = ( y_color.b - y_color.r ) / y_diff + 2
				elseif( y_max == y_color.b ) then
					y_wl = ( y_color.r - y_color.g ) / y_diff + 4
				end
				
				if( x_wl ~= y_wl ) then
					return x_wl < y_wl
				end
			end
		end
		x_pc = x.pc or (x.pcs and x.pcs[1]) or 1001
		y_pc = y.pc or (y.pcs and y.pcs[1]) or 1001
		
		x_pc = x_pc + (x.global_value and tweak_data.lootdrop.global_values[x.global_value].sort_number or 0)
		y_pc = y_pc + (y.global_value and tweak_data.lootdrop.global_values[y.global_value].sort_number or 0)
		
		return x_pc < y_pc
	end )
	
	local max_x = 6
	local max_y = 3
	local mod_data = {}
	for i=1, math.ceil( #mods / (max_x*max_y) ) do -- math.ceil( #mods / 9 ) do
		mod_data = {}
		for id=((i-1)*(max_x*max_y))+1, math.min(i*(max_x*max_y), #mods) do
			table.insert( mod_data, mods[id] )  -- mod_data[ id ] = mods[ id ]
		end
		table.insert( new_node_data, { name=tostring(i), category=category, prev_node_data=data, name_localized=tostring(i), on_create_func_name="populate_choose_mask_mod", on_create_data=mod_data, override_slots={ max_x, max_y }, identifier=self.identifiers["mask_mod"] } )
	end
	new_node_data.topic_id = "bm_menu_customize_mask_title"
	new_node_data.topic_params = { mask_name=prev_node_params.mask_name }
	
	new_node_data.open_callback_name = "update_mod_mask"
	
	-- new_node_data.info_callback = self._data.info_callback or function() return managers.blackmarket:info_customize_mask() end
	new_node_data.blur_fade = self._data.blur_fade
	managers.menu:open_node( "blackmarket_mask_node", { new_node_data } )
end

function BlackMarketGui:preview_customized_mask_callback( data )
	if( not managers.blackmarket:can_view_customized_mask() ) then
		-- return
	end
	managers.menu:open_node( "blackmarket_preview_mask_node", {} )
	managers.blackmarket:view_customized_mask()
end

function BlackMarketGui:preview_customized_mask_with_mod_callback( data )
	if( not managers.blackmarket:can_view_customized_mask_with_mod( data.category, data.name, data.global_value ) ) then
		return
	end
	managers.menu:open_node( "blackmarket_preview_mask_node", {} )
	managers.blackmarket:view_customized_mask_with_mod( data.category, data.name )
end

function BlackMarketGui:_warn_abort_customized_mask_callback( params )
	return managers.blackmarket:warn_abort_customize_mask( params )
end

function BlackMarketGui:_abort_customized_mask_callback()
	managers.blackmarket:abort_customize_mask()
	managers.menu:back( true )
end

function BlackMarketGui:buy_customized_mask_callback( data )
	local params = {}
	params.name = managers.localization:text( tweak_data.blackmarket.masks[managers.blackmarket:get_customize_mask_id()].name_id ) 
	params.category = data.category
	params.slot = data.slot
	params.money = managers.experience:cash_string( managers.blackmarket:get_customize_mask_value() )
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_buy_customized_mask_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_finalize( params )
end

function BlackMarketGui:_buy_customized_mask_callback( data )
	managers.menu_component:post_event( "item_buy" )
	managers.blackmarket:finish_customize_mask()
	managers.menu:back( true )
end

function BlackMarketGui:choose_mask_part_callback( data )
	managers.blackmarket:select_customize_mask( data.category, data.name, data.global_value )
	self:reload()
end

			-- buy weapon slot selected
function BlackMarketGui:buy_weapon_callback( data )
	local params = {}
	params.name = data.name_localized or data.name
	params.category = data.category
	params.slot = data.slot
	params.weapon = data.name
	params.money = managers.experience:cash_string( managers.money:get_weapon_price_modified( data.name ) )
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_buy_weapon_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_buy( params )
end


function BlackMarketGui:_buy_mask_callback( data )
	managers.menu_component:post_event( "item_buy" )
	managers.blackmarket:on_buy_mask_to_inventory( data.name, data.global_value, data.slot )
	managers.menu:back( true, math.max(data.num_backs-1, 0) )
end


function BlackMarketGui:_buy_weapon_callback( data )
	managers.menu_component:post_event( "item_buy" )
	managers.blackmarket:on_buy_weapon_platform( data.category, data.name, data.slot )
	managers.menu:back( true )
end

function BlackMarketGui:preview_buy_weapon_callback( data )
	-- managers.menu:open_node( self._preview_node_name, {} )
	managers.blackmarket:view_weapon_platform( data.name, callback( self, self, "_open_preview_node" ) )
end

function BlackMarketGui:preview_buy_mask_callback( data )
	managers.menu:open_node( "blackmarket_preview_mask_node", {} )
	managers.blackmarket:view_mask_with_mask_id( data.name )
end


			-- mod type slot selected
function BlackMarketGui:choose_mod_callback( data, prev_node_params )		-- OLD ONE
	local mods = deep_clone( data.mods ) or {}
	local new_node_data = {}
	
	local factory_id = managers.blackmarket:get_crafted_category( data.category )[data.slot].factory_id
	-- local default_blueprint = managers.weapon_factory:get_default_blueprint_by_factory_id( factory_id )
	
	--[[
	local no_mod_choice = true
	
	for i, d_mod in ipairs( default_blueprint ) do
		if( tweak_data.weapon.factory.parts[d_mod].type == data.name ) then
			table.insert( mods, 1, d_mod )
			no_mod_choice = false
			break;
		end
	end
	if( no_mod_choice ) then
		table.insert( mods, 1, false )
	end
	]]
	
	local mod_data = {}
	for i=1, math.ceil( #mods / 9 ) do
		mod_data = {}
		for id=((i-1)*9)+1, math.min(i*9, #mods) do
			table.insert( mod_data, { mods[id], false } )  -- mod_data[ id ] = mods[ id ]
		end
		table.insert( new_node_data, { name=tostring(i), category=data.category, prev_node_data=data, name_localized=tostring(i) --[[managers.localization:text( "bm_menu_page", { page=i } )]], on_create_func_name="populate_mods", on_create_data=mod_data, identifier=self.identifiers["weapon_mod"] } )
	end
	
	new_node_data.topic_id = "bm_menu_blackmarket_title"
	new_node_data.topic_params = { item=prev_node_params.weapon_name }
	new_node_data.show_tabs = true
	managers.menu:open_node( self._inception_node_name, { new_node_data } )
end


			-- mod slot selected
function BlackMarketGui:buy_mod_callback( data )
	local params = {}
	params.name = data.name_localized or data.name
	params.category = data.category
	params.slot = data.slot
	params.weapon_name = managers.weapon_factory:get_weapon_name_by_factory_id( managers.blackmarket:get_crafted_category( data.category )[data.slot].factory_id )
	params.add = true
		
	local weapon_id = managers.blackmarket:get_crafted_category( data.category )[data.slot].weapon_id 
	params.money = managers.experience:cash_string( managers.money:get_weapon_modify_price( weapon_id, data.name, data.global_value ) ) 
	
	local replaces, removes = managers.blackmarket:get_modify_weapon_consequence( data.category, data.slot, data.name )
	params.replaces = replaces or {}
	params.removes = removes or {}
	
	if data.default_mod then
		table.delete( replaces, data.default_mod )
		table.delete( removes, data.default_mod )
	end
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_buy_mod_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_mod( params )
end

function BlackMarketGui:_buy_mod_callback( data )
	-- local weapon_id = managers.blackmarket:get_crafted_category( data.category )[data.slot].weapon_id
	managers.menu_component:post_event( "item_buy" )
	managers.blackmarket:buy_and_modify_weapon( data.category, data.slot, data.global_value, data.name )
	self:reload()
	-- managers.menu:back( true )
end

function BlackMarketGui:preview_weapon_with_mod_callback( data )
	-- managers.menu:open_node( self._preview_node_name, {} )
	managers.blackmarket:view_weapon_with_mod( data.category, data.slot, data.name, callback( self, self, "_open_preview_node" ) )
end

function BlackMarketGui:remove_mod_callback( data )
	local params = {}
	params.name = managers.localization:text( tweak_data.weapon.factory.parts[data.name].name_id )
	params.category = data.category
	params.slot = data.slot
	params.weapon_name = managers.weapon_factory:get_weapon_name_by_factory_id( managers.blackmarket:get_crafted_category( data.category )[data.slot].factory_id )
	params.add = false
	
	local replaces, removes = managers.blackmarket:get_modify_weapon_consequence( data.category, data.slot, data.default_mod or data.name )
	
	local weapon_id = managers.blackmarket:get_crafted_category( data.category )[data.slot].weapon_id 
	params.money = managers.experience:cash_string( managers.money:get_weapon_modify_price( weapon_id, data.name, data.global_value ) ) 
	
	
	params.replaces = replaces
	params.removes = removes
	
	params.yes_func = callback( self, self, "_dialog_yes", callback( self, self, "_remove_mod_callback", data ) )
	params.no_func  = callback( self, self, "_dialog_no" )
	managers.menu:show_confirm_blackmarket_mod( params )
end

function BlackMarketGui:_remove_mod_callback( data )
	managers.menu_component:post_event( "item_sell" )
	if data.default_mod then
		managers.blackmarket:buy_and_modify_weapon( data.category, data.slot, data.global_value, data.default_mod, true )
	else
		managers.blackmarket:on_sell_weapon_part( data.category, data.slot, data.global_value, data.name )
		-- managers.blackmarket:remove_weapon_part( data.category, data.slot, data.name )
	end 
	self:reload()
	-- managers.menu:back( true )
end

function BlackMarketGui:preview_weapon_without_mod_callback( data )
	-- managers.menu:open_node( self._preview_node_name, {} )
	if data.default_mod then
		managers.blackmarket:view_weapon_with_mod( data.category, data.slot, data.default_mod, callback( self, self, "_open_preview_node" ) )
	else
		managers.blackmarket:view_weapon_without_mod( data.category, data.slot, data.name, callback( self, self, "_open_preview_node" ) )
	end
end


			-- loadout callbacks
function BlackMarketGui:lo_equip_deployable_callback( data )
	managers.blackmarket:equip_deployable( data.name )
	self:reload()
end


function BlackMarketGui:update_mod_mask()
	if not managers.blackmarket:currently_customizing_mask() then
		managers.menu:back( true )
	else
		managers.blackmarket:view_customized_mask()
	end
end


--[[----------------------------------------------------------------------------------------------------------------------------------
																	Dialog callback functions, when a dialog popsup
------------------------------------------------------------------------------------------------------------------------------------]]

function BlackMarketGui:_dialog_yes( clbk )
	if( clbk and type( clbk ) == "function" ) then
		clbk()
	end
end

function BlackMarketGui:_dialog_no( clbk )
	if( clbk and type( clbk ) == "function" ) then
		clbk()
	end
end




--[[----------------------------------------------------------------------------------------------------------------------------------
																	Other Stuff
------------------------------------------------------------------------------------------------------------------------------------]]




function BlackMarketGui:destroy()
	for i, tab in pairs( self._tabs ) do
		tab:destroy()
	end
end

function BlackMarketGui:close()
	WalletGuiObject.close_wallet( self._panel )
	-- self._wallet:destroy()
	-- self._wallet = nil
	
	self:destroy()
	
	self._ws:panel():remove( self._panel )
	self._fullscreen_ws:panel():remove( self._fullscreen_panel )
	
	if not managers.menu_component._menuscene_info_gui then
		managers.menu:active_menu().renderer.ws:show()
	end
end

function BlackMarketGui:_pre_reload()
	self._temp_panel = self._panel
	self._temp_fullscreen_panel = self._fullscreen_panel
	-- self._temp_tabs = self._tabs
	
	WalletGuiObject.close_wallet( self._panel )
	-- self._wallet:destroy()
	-- self._wallet = nil
	self:destroy()
	
	self._panel = nil
	self._fullscreen_panel = nil
	
	self._temp_panel:hide()
	self._temp_fullscreen_panel:hide()
	
	-- managers.menu:active_menu().renderer.ws:show()
end

function BlackMarketGui:_post_reload()
	self._ws:panel():remove( self._temp_panel )
	self._fullscreen_ws:panel():remove( self._temp_fullscreen_panel )
	self._temp_panel = nil
	self._temp_fullscreen_panel = nil
	
end

function BlackMarketGui:reload()
	self:_pre_reload()
	-- self:close()
	self._tabs = {}
	self._btns = {}
	self._equipped_comparision_data = {}
	BlackMarketGui._setup( self, false, self._data )
	-- BlackMarketGui.init( self, self._ws, self._fullscreen_ws, self._node )
	
	self:_post_reload()
end