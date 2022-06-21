DebugStringsBoxGui = DebugStringsBoxGui or class( TextBoxGui )

function DebugStringsBoxGui:init( ws, title, text, content_data, config, file )
	self._file = file
	config = config or {}
	config.h = 300
	config.w = 300
	local x,y = ws:size()
	config.x = x - config.w 
	config.y = y - config.h - CoreMenuRenderer.Renderer.border_height + 10
	config.no_close_legend = true
	config.no_scroll_legend = true
		
	self._default_font_size = tweak_data.menu.default_font_size
	self._topic_state_font_size = 22
			
	DebugStringsBoxGui.super.init( self, ws, title, text, content_data, config )
		
	-- self:_create_friend_action_gui()
	
	self:set_layer( 10 )
end

function DebugStringsBoxGui:set_layer( layer )
	-- print( "[DebugStringsBoxGui:set_layer]", layer )
	-- Application:stack_dump()
	DebugStringsBoxGui.super.set_layer( self, layer )
end

function DebugStringsBoxGui:_create_text_box( ws, title, text, content_data, config )
	DebugStringsBoxGui.super._create_text_box( self, ws, title, text, content_data, config )
	if self._info_box then
		self._info_box:close()
		self._info_box = nil
	end
	local strings_panel = self._scroll_panel:panel( { name = "strings_panel", x = 0, h = 600, layer = 1 } )
	
	-- strings_panel:set_debug( true )
	-- strings_panel:set_h( h )
	
	local y = 0
	local i = 0
	local ids = managers.localization:debug_file( self._file )
	local sorted = {}
	for id,_ in pairs( ids ) do
		table.insert( sorted, id )
	end
	table.sort( sorted )
	for _,id in pairs( sorted ) do
		local localized = ids[ id ]
		local even = math.mod( i, 2 ) == 0
		local string_panel = strings_panel:panel( { name = id, y = y, w = 640 * 0.85 - 16 } )
		
		string_panel:rect( { name = "bg", color = (even and Color.white/1.5 or Color.white/2):with_alpha( 0.25 ), halign = "grow", layer = 1 } )
		
		local text_id = string_panel:text( { name = "id", text = id, font = tweak_data.menu.pd2_small_font, font_size = tweak_data.menu.pd2_small_font_size, y = 0, x = 16,
									align="left", halign="left", vertical="center", color = Color.white, layer = 2 } )
									
		local text = string_panel:text( { name = "text", text = localized, font = tweak_data.menu.pd2_small_font, font_size = tweak_data.menu.pd2_small_font_size, y = 0, x = string_panel:w()/2,
									align="left", vertical="top", color = Color.white, layer = 2, wrap = true, word_wrap = true } )
									
		local _,_,tw,th = text_id:text_rect()
		local _,_,tw2,th2 = text:text_rect()
		text_id:set_size( tw, th )
		text:set_size( tw2, th2 )
			
		string_panel:set_h( math.max( text_id:h(), text:h() ) + 4 )
				
		y = y + string_panel:h()
		i = i + 1
	end
	
	strings_panel:set_h( y + 14 )
	
	self._scroll_panel:set_h( math.max( self._scroll_panel:h(), strings_panel:h() ) )
	self:_set_scroll_indicator()
		
	--[[local friends_panel = self._scroll_panel:panel( { name = "friends_panel", x = 0, h = 600, layer = 1 } )
	local ingame_panel = friends_panel:panel( { name = "ingame_panel", x = 0, h = 100, layer = 0 } )
	local online_panel = friends_panel:panel( { name = "online_panel", x = 0, h = 100, layer = 0 } )
	local offline_panel = friends_panel:panel( { name = "offline_panel", x = 0, h = 100, layer = 0 } )
	
	local h = 0
	
	local ingame_text = friends_panel:text( { name = "ingame_text", text = managers.localization:text( "menu_ingame" ), font = tweak_data.menu.default_font, font_size = self._topic_state_font_size, y = h,
									align="left", halign="left", vertical="center", hvertical="center", color = Color( 0.75, 0.75, 0.75 ) } )
	local _,_,tw,th = ingame_text:text_rect()
	ingame_text:set_size( tw, th )
	h = h + th
	
	local online_text = friends_panel:text( { name = "online_text", text = managers.localization:text( "menu_online" ), font = tweak_data.menu.default_font, font_size = self._topic_state_font_size, y = h,
									align="left", halign="left", vertical="center", hvertical="center", color = Color( 0.75, 0.75, 0.75 ) } )
	local _,_,tw,th = online_text:text_rect()
	online_text:set_size( tw, th )
	h = h + th
		
	local offline_text = friends_panel:text( { name = "offline_text", text = managers.localization:text( "menu_offline" ), font = tweak_data.menu.default_font, font_size = self._topic_state_font_size, y = h,
									align="left", halign="left", vertical="center", hvertical="center", color = Color( 0.75, 0.75, 0.75 ) } )
	local _,_,tw,th = offline_text:text_rect()
	offline_text:set_size( tw, th )
	h = h + th
		
	friends_panel:set_h( h )
	
	self._scroll_panel:set_h( math.max( self._scroll_panel:h(), friends_panel:h() ) )
	self:_set_scroll_indicator()
	
	self:_layout_friends_panel()]]
end

function DebugStringsBoxGui:mouse_moved( x, y )
	
end

function DebugStringsBoxGui:_check_scroll_indicator_states()
	DebugStringsBoxGui.super._check_scroll_indicator_states( self )
end

function DebugStringsBoxGui:set_size( x, y )
	DebugStringsBoxGui.super.set_size( self, x, y )
	
	local strings_panel = self._scroll_panel:child( "strings_panel" )
	strings_panel:set_w( self._scroll_panel:w() )
	local hy = 0
	for _,child in ipairs( strings_panel:children() ) do
		child:set_w( strings_panel:w() )
		child:set_y( hy )
		
		local text_id = child:child( "id" )
		local text = child:child( "text" )
		text:set_x( child:w()/2 )
		text:set_w( child:w()/2 - 16 )
		text:set_shape( text:text_rect() )
		text:set_x( child:w()/2 )
		text:set_y( 0 )
		local _,_,tw2,th2 = text:text_rect()
		-- text:set_w( child:w()/2 - 16 )
		
		local _,_,tw,th = text_id:text_rect()
		-- local _,_,tw2,th2 = text:text_rect()
		-- text_id:set_size( tw, th )
		-- text:set_size( tw2, th2 )
		text_id:set_h( th )
		-- text:set_h( th2 )
				
		child:set_h( math.max( text_id:h(), text:h() ) + 4 )
		
		-- child:child( "bg" ):set_shape( text:shape() )
				
		hy = hy + child:h()
	end
	--[[local friends_panel = self._scroll_panel:child( "friends_panel" )
	
	friends_panel:set_w( self._scroll_panel:w() )
	
	local f = function( friends_panel, panel )
		panel:set_w( friends_panel:w() )
		for _,user_panel in ipairs( panel:children() ) do
			user_panel:set_w( panel:w() )
			user_panel:child( "lobby" ):set_right( user_panel:w() )
		end
	end
	
	f( friends_panel, friends_panel:child( "ingame_panel" ) )
	f( friends_panel, friends_panel:child( "online_panel" ) )
	f( friends_panel, friends_panel:child( "offline_panel" ) )]]
end

function DebugStringsBoxGui:set_visible( visible )
	DebugStringsBoxGui.super.set_visible( self, visible )
end

function DebugStringsBoxGui:close()
	print( "DebugStringsBoxGui:close()" )
	DebugStringsBoxGui.super.close( self )
end

