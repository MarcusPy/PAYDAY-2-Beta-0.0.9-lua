MenuNodeButtonLayoutGui = MenuNodeButtonLayoutGui or class( MenuNodeGui )

function MenuNodeButtonLayoutGui:init( node, layer, parameters )
	MenuNodeButtonLayoutGui.super.init( self, node, layer, parameters )
		
	self:_setup( node )
end

function MenuNodeButtonLayoutGui:_setup_panels( node )
	MenuNodeButtonLayoutGui.super._setup_panels( self, node )
	
	local safe_rect_pixels = managers.viewport:get_safe_rect_pixels()
end

function MenuNodeButtonLayoutGui:_setup()
	self._coords = tweak_data:get_controller_help_coords() or {}
	
	--[[
	if SystemInfo:platform() == Idstring( "PS3" ) then
		self._coords[ "menu_button_sprint" ] = { x = 185, y = 255, align = "right", vertical ="top" }
		self._coords[ "menu_button_move" ] = { x = 185, y = 280, align = "right", vertical ="top" }
		self._coords[ "menu_button_melee" ] = { x = 329, y = 255, align = "left", vertical ="top" }
		self._coords[ "menu_button_look" ] = { x = 329, y = 280, align = "left", vertical ="top" }
		
		self._coords[ "menu_button_switch_weapon" ] = { x = 511, y = 112, align = "left" }
		self._coords[ "menu_button_reload" ] = { x = 511, y = 214, align = "left" }
		self._coords[ "menu_button_crouch" ] = { x = 511, y = 146, align = "left" }
		self._coords[ "menu_button_jump" ] = { x = 511, y = 178, align = "left" }
		
		self._coords[ "menu_button_shout" ] = { x = 511, y = 8, align = "left" }
		self._coords[ "menu_button_fire_weapon" ] = { x = 511, y = 36, align = "left" }
		self._coords[ "menu_button_deploy" ] = { x = 0, y = 8, align = "right" }
		self._coords[ "menu_button_aim_down_sight" ] = { x = 0, y = 36, align = "right" }
		
		self._coords[ "menu_button_ingame_menu" ] = { x = 290, y = 0, align = "left", vertical ="bottom" }
		self._coords[ "menu_button_stats_screen" ] = { x = 220, y = 0, align = "right", vertical ="bottom" }
		
		self._coords[ "menu_button_weapon_gadget" ] = { x = 0, y = 171, align = "right", vertical = "center" }
	else
		self._coords[ "menu_button_sprint" ] = { x = 0, y = 138, align = "right", vertical ="bottom" }
		self._coords[ "menu_button_move" ] = { x = 0, y = 138, align = "right", vertical ="top" }
		self._coords[ "menu_button_melee" ] = { x = 312, y = 256, align = "left", vertical ="top" }
		self._coords[ "menu_button_look" ] = { x = 312, y = 281, align = "left", vertical ="top" }
		
		self._coords[ "menu_button_switch_weapon" ] = { x = 512, y = 97, align = "left" }
		self._coords[ "menu_button_reload" ] = { x = 512, y = 180, align = "left" }
		self._coords[ "menu_button_crouch" ] = { x = 512, y = 125, align = "left" }
		self._coords[ "menu_button_jump" ] = { x = 512, y = 153, align = "left" }
		
		self._coords[ "menu_button_shout" ] = { x = 512, y = 49, align = "left" }
		self._coords[ "menu_button_fire_weapon" ] = { x = 512, y = 19, align = "left" }
		self._coords[ "menu_button_deploy" ] = { x = 0, y = 49, align = "right" }
		self._coords[ "menu_button_aim_down_sight" ] = { x = 0, y = 19, align = "right" }
		
		self._coords[ "menu_button_ingame_menu" ] = { x = 298, y = 0, align = "left", vertical ="bottom" }
		self._coords[ "menu_button_stats_screen" ] = { x = 213, y = 0, align = "right", vertical ="bottom" }
		
		self._coords[ "menu_button_weapon_gadget" ] = { x = 199, y = 256, align = "right", vertical = "top" }
	end
	]]
	
	for id,data in pairs( self._coords ) do -- 
		data.text = self.ws:panel():text( { text = managers.localization:to_upper_text( id ), font_size = self.font_size, font = self.font, layer = self.layers.items,
											align=data.align, vertical=data.vertical, 
											halign="center", valign="center", } )
	end
	
	self._blur = managers.menu_component._fullscreen_ws:panel():bitmap( { texture="guis/textures/test_blur_df", w=managers.menu_component._fullscreen_ws:panel():w(), h=managers.menu_component._fullscreen_ws:panel():h(), render_template="VertexColorTexturedBlur3D", layer = self.layers.background } )
	local func = function(o)
		local start_blur = 0
		over( 0.6, function(p) o:set_alpha( math.lerp( start_blur, 1, p ) ) end)
	end	
	self._blur:animate( func )
	
	self._bg = self.ws:panel():rect( { visible = false, color = Color( 1, 0.1, 0.1, 0.1 ), layer = self.layers.background } )
	self._controller = self.ws:panel():bitmap( { texture="guis/textures/controller", layer = self.layers.items, w=512, h=256 } )
	self:_layout()
end

function MenuNodeButtonLayoutGui:_layout()
	local safe_rect_pixels = managers.viewport:get_safe_rect_pixels()
	local res = RenderSettings.resolution
	local scale = tweak_data.scale.button_layout_multiplier
	
	self._bg:set_h( res.y - (tweak_data.menu.upper_saferect_border + safe_rect_pixels.y )* 2 + 2 )
	self._bg:set_center_y( res.y / 2)
	
	self._blur:set_size( managers.menu_component._fullscreen_ws:panel():w(), managers.menu_component._fullscreen_ws:panel():h() )
	
	self._controller:set_size( self._controller:w() * scale, self._controller:h() * scale ) 
	self._controller:set_center( self.ws:panel():w()/2, self.ws:panel():h()/2 )
	
	for id,data in pairs( self._coords ) do
		local _,_,w,h = data.text:text_rect()
		data.text:set_size( w, h )
		if data.x then
			local x = self._controller:x() + data.x * scale
			local y = self._controller:y() + data.y * scale
			if data.align == "left" then
				data.text:set_left( x )
			elseif data.align == "right" then
				data.text:set_right( x )
			elseif data.align == "center" then
				data.text:set_center_x( x )
			end
			if data.vertical == "top" then
				data.text:set_top( y )
			elseif data.vertical == "bottom" then
				data.text:set_bottom( y )
			else
				data.text:set_center_y( y )
			end
		end
	end
end

function MenuNodeButtonLayoutGui:_create_menu_item( row_item )
	MenuNodeButtonLayoutGui.super._create_menu_item( self, row_item )
end

function MenuNodeButtonLayoutGui:_setup_item_panel_parent( safe_rect )
	MenuNodeButtonLayoutGui.super._setup_item_panel_parent( self, safe_rect )
end

function MenuNodeButtonLayoutGui:_setup_item_panel( safe_rect, res )
	MenuNodeButtonLayoutGui.super._setup_item_panel( self, safe_rect, res )
end

function MenuNodeButtonLayoutGui:resolution_changed()
	MenuNodeButtonLayoutGui.super.resolution_changed( self )
	
	self:_layout()
end

function MenuNodeButtonLayoutGui:close( ... )
	self._bg:parent():remove( self._bg )
	self._blur:parent():remove( self._blur )
	
	MenuNodeButtonLayoutGui.super.close( self, ... )
end


