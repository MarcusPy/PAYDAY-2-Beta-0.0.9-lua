ChatManager = ChatManager or class()

ChatManager.GAME = 1
ChatManager.CREW = 2
ChatManager.GLOBAL = 3
-- User ID Channels are undecided at the moment

function ChatManager:init()
	self:_setup()
end

function ChatManager:_setup()
	self._chatlog = {} 		-- Contains all recent messages and user, grouped by channel_ids
	self._receivers = {} 	-- Contains classes who should receive messages
end

function ChatManager:register_receiver( channel_id, receiver )
	-- How to actually hanlde channels (private between users and game participants etc)
	-- receiver is a script class. Should have inherited ChatGuiBase
	self._receivers[ channel_id ] = self._receivers[ channel_id ] or {}
	table.insert( self._receivers[ channel_id ] ,receiver ) 
end

function ChatManager:unregister_receiver( channel_id, receiver )
	if not self._receivers[ channel_id ] then
		return
	end
	
	for i, rec in ipairs( self._receivers[ channel_id ] ) do
		if rec == receiver then
			table.remove( self._receivers[ channel_id ] , i )
			break
		end
	end
end

function ChatManager:send_message( channel_id, sender, message )
	-- sender shoud be local peer?
	if managers.network:session() then
		-- managers.network:session():send( "send_chat_message", channel_id, peer, message )
		sender = managers.network:session():local_peer()
		managers.network:session():send_to_peers_ip_verified( "send_chat_message", channel_id, message )
		self:receive_message_by_peer( channel_id, sender, message )
	else -- Sending offline or through some other technology
		
		self:receive_message_by_name( channel_id, sender, message )
	end
end

function ChatManager:receive_message_by_peer( channel_id, peer, message )
	-- local color_id = managers.criminals:character_color_id_by_name( managers.criminals:character_workname_by_peer_id( peer:id() ) )	
	local color_id = peer:id() -- (managers.criminals and managers.criminals:character_color_id_by_peer_id( peer:id() )) or peer:id()
	local color = tweak_data.chat_colors[ color_id ]
	self:_receive_message( channel_id, peer:name(), message, tweak_data.chat_colors[ color_id ] )
end

function ChatManager:receive_message_by_name( channel_id, name, message )
	self:_receive_message( channel_id, name, message, tweak_data.chat_colors[1] )
end

function ChatManager:_receive_message( channel_id, name, message, color )
	if not self._receivers[ channel_id ] then
		return
	end
	
	for i, receiver in ipairs( self._receivers[ channel_id ] ) do
		receiver:receive_message( name, message, color )
	end
	
	-- :receive_message( u_name or "Offline", text:text(), tweak_data.chat_colors[1] )
	-- Pass to recievers
end

-- not used
function ChatManager:save( data )
	--[[local state = {	}
	data.ChatManager = state]]
end

-- not used
function ChatManager:load( data )
	--[[local state = data.ChatManager
	]]
end

---------------------------------------------------------
-- All chat recievers should inherit this
ChatBase = ChatBase or class()

function ChatBase:init()
	
end

function ChatBase:receive_message( name, message, color )

end

---------------------------------------------------------
ChatGui = ChatGui or class( ChatBase ) -- TextBoxGui )

function ChatGui:init( ws ) -- , title, text, content_data, config, type )
	self._ws = ws
	self._hud_panel = ws:panel() -- hud.panel
	self:set_channel_id( ChatManager.GAME )
	
	-- ChatGui.super.init( self, ws, nil, nil, nil, {no_close_legend=true, no_scroll_legend=true, } )
	
	self._output_width = self._hud_panel:w() * 0.5 - 10
	self._panel_width = self._hud_panel:w() * 0.5 - 10
	
	self._panel_height = 500
	self._max_lines = 15
	
	self._lines = {}
	
	self._esc_callback = callback( self, self, "esc_key_callback" ) -- 0 --dummy
	self._enter_callback = callback( self, self, "enter_key_callback" ) -- 0 --dummy
	self._typing_callback = 0 --dummy
	self._skip_first = false
		
	self._panel = self._hud_panel:panel( { name = "chat_panel", x = 0, h = self._panel_height, w = self._panel_width, valign = "bottom" } )
	self:set_leftbottom( 0, 70 )
	self._panel:set_layer( 20 )
	-- self._panel:set_debug( true )
	
	local output_panel = self._panel:panel( { name = "output_panel", x = 20, h = 10, w = self._output_width - 20, layer = 1 } )
	-- output_panel:rect( { color = Color.red:with_alpha( 0.5 ), layer = -1, valign = "grow" } )
	-- output_panel:gradient( { name = "output_bg", gradient_points = { 0, Color.white:with_alpha( 0 ), 0.2, Color.white:with_alpha( 0.25 ), 1, Color.white:with_alpha( 0 ) }, layer = -1, valign = "grow", blend_mode = "sub" } )
	
	local scroll_panel = output_panel:panel( { name="scroll_panel", x=0, h=10, w=self._output_width } )
	
	-- output_panel:gradient( { name="scroll_indicator_top", x=0, h=20, w=self._output_width, layer=2, orientation="vertical", gradient_points={1, Color.black:with_alpha(0.2), 0.6, Color.white:with_alpha(0.25), 0, Color.white}, blend_mode="add" } )
	-- output_panel:gradient( { name="scroll_indicator_bottom", x=0, h=20, w=self._output_width, layer=2, orientation="vertical", gradient_points={0, Color.black:with_alpha(0.2), 0.4, Color.white:with_alpha(0.25), 1, Color.white}, blend_mode="add" } )
	
	-- self._panel:rect( { name="scroll_indicator_bar", x=2, w=6, layer=2, color=Color.red } )
	
	self._scroll_indicator_box_class = BoxGuiObject:new( output_panel, { sides = { 0, 0, 0, 0 } } )
	local scroll_up_indicator_shade = output_panel:bitmap( { name = "scroll_up_indicator_shade", texture = "guis/textures/headershadow", rotation = 180, layer = 2, color = Color.white, w = output_panel:w() } )
	-- scroll_up_indicator_shade:set_top( 0 )
	local texture, rect = tweak_data.hud_icons:get_icon_data( "scroll_up" )
	local scroll_up_indicator_arrow = self._panel:bitmap( { name = "scroll_up_indicator_arrow", texture = texture, texture_rect = rect, layer = 2, color = Color.white } )
	-- scroll_up_indicator_arrow:set_righttop( output_panel:left() - 2, output_panel:top() + 2 )
		
	local scroll_down_indicator_shade = output_panel:bitmap( { name = "scroll_down_indicator_shade", texture = "guis/textures/headershadow", layer = 2, color = Color.white, w = output_panel:w() } )
	-- scroll_down_indicator_shade:set_bottom( self._output_width )
	local texture, rect = tweak_data.hud_icons:get_icon_data( "scroll_dn" )
	local scroll_down_indicator_arrow = self._panel:bitmap( { name = "scroll_down_indicator_arrow", texture = texture, texture_rect = rect, layer = 2, color = Color.white } )
	-- scroll_down_indicator_arrow:set_rightbottom( output_panel:left() - 2, output_panel:bottom() - 2 )
	
	-- Scroll bar	
	local bar_h = scroll_down_indicator_arrow:top() - scroll_up_indicator_arrow:bottom()
	local texture, rect = tweak_data.hud_icons:get_icon_data( "scrollbar" )
	
	local scroll_bar = self._panel:panel( { name = "scroll_bar", layer = 2, h = bar_h, w = 15 } )
	local scroll_bar_box_panel = scroll_bar:panel( { name="scroll_bar_box_panel", w=4, x=5, halign="scale", valign="scale" } )
	self._scroll_bar_box_class = BoxGuiObject:new( scroll_bar_box_panel, { sides = { 2, 2, 0, 0 } } )
	-- local scroll_bar = self._panel:bitmap( { name = "scroll_bar", texture = texture, texture_rect = rect, layer = 2, h = bar_h, color = Color.white } )
	
	-- scroll_bar:set_bottom( scroll_down_indicator_arrow:top() )
	-- scroll_bar:set_center_x( scroll_down_indicator_arrow:center_x() )

	self._enabled = true
	
	output_panel:set_x( scroll_down_indicator_arrow:w() + 4 )
	
	self:_create_input_panel()
	self:_layout_input_panel()
	self:_layout_output_panel()
end

function ChatGui:set_leftbottom( left, bottom )
	self._panel:set_left( left )
	self._panel:set_bottom( self._panel:parent():h() - bottom )
end

function ChatGui:set_max_lines( max_lines )
	self._max_lines = max_lines
	self:_layout_output_panel()
end

function ChatGui:set_params( params )
	if params.max_lines then
		self:set_max_lines( params.max_lines )
	end
	
	if params.left and params.bottom then
		self:set_leftbottom( params.left, params.bottom  )
	end
end

function ChatGui:set_enabled( enabled )
	if( not enabled ) then
		self:_loose_focus()
	end
	self._enabled = enabled
end

function ChatGui:hide()
	self._panel:hide()
	self:set_enabled( false )
	
	local text = self._input_panel:child( "input_text" )
	text:set_text("")		-- Clear input line.
	text:set_selection(0,0)
end

function ChatGui:show()
	self._panel:show()
	self:set_enabled( true )
end

function ChatGui:set_layer( layer )
	self._panel:set_layer( layer )
end

function ChatGui:set_channel_id( channel_id )
	managers.chat:unregister_receiver( self._channel_id, self )
	self._channel_id = channel_id
	managers.chat:register_receiver( self._channel_id, self ) 
end

function ChatGui:esc_key_callback()
	if not self._enabled then
		return
	end
	-- print( "esc_key_callback" )
	self._esc_focus_delay = true
	self:_loose_focus()
end

-- Send the message
function ChatGui:enter_key_callback()
	if not self._enabled then
		return
	end
	-- print( "enter_key_callback" )
	local text = self._input_panel:child( "input_text" )
	local message = text:text()
	
	if Idstring( message ) == Idstring("/ready") then
		managers.menu_component:on_ready_pressed_mission_briefing_gui()
	-- elseif string.len( message ) > 0 then
	elseif string.len( message ) > 0 then
		local u_name = managers.network.account:username()
		managers.chat:send_message( self._channel_id, u_name or "Offline", message )
	else
		self._enter_loose_focus_delay = true
		self:_loose_focus()
	end
		
	text:set_text("")		-- Clear input line.
	text:set_selection(0,0)
	
	-- managers.hud:set_chat_focus( false )
end

function ChatGui:_create_input_panel()
	self._input_panel = self._panel:panel( { alpha = 0, name = "input_panel", x = 0, h = 24, w = self._panel_width, layer = 1 } )
	-- self._input_panel:rect( { color = Color( math.rand( 1 ), math.rand( 1 ), 0 ):with_alpha( 0.2 ) } )
	self._input_panel:rect( { name = "focus_indicator", visible = false, color = Color.black:with_alpha( 0.2 ), layer = 0 } )
	
	local say = self._input_panel:text( { name = "say", text = utf8.to_upper( managers.localization:text( "debug_chat_say" ) ), font = tweak_data.menu.pd2_small_font, font_size = tweak_data.menu.pd2_small_font_size, x = 0, y = 0,
									align="left", halign="left", vertical="center", hvertical="center", blend_mode="normal",
									color = Color.white, layer = 1 } )
	local _,_,w,h = say:text_rect()
	say:set_size( w, self._input_panel:h() )
	
	local input_text = self._input_panel:text( { name = "input_text", text = "", font = tweak_data.menu.pd2_small_font, font_size = tweak_data.menu.pd2_small_font_size, x = 0, y = 0,
									align="left", halign="left", vertical="center", hvertical="center", blend_mode="normal",
									color = Color.white, layer = 1, wrap = true, word_wrap = false } )
	local caret = self._input_panel:rect( { name="caret", layer = 2, x = 0, y = 0, w = 0, h = 0, color = Color(0.05, 1, 1, 1) } )
	
	self._input_panel:rect( { name="input_bg", color=Color.black:with_alpha(0.5), layer = -1, valign = "grow", h = self._input_panel:h() } ) -- gradient( { name = "input_bg", gradient_points = { 0, Color.white:with_alpha( 0.0 ), 0.2, Color.white:with_alpha( 0.25 ), 1, Color.white:with_alpha( 0.0 ) }, layer = -1, valign = "grow", blend_mode = "sub", h = self._input_panel:h() } )
	self._input_panel:child( "input_bg" ):set_w( self._input_panel:w() - w )
	self._input_panel:child( "input_bg" ):set_x( w )
	
	self._input_panel:stop()
	self._input_panel:animate( callback( self, self, "_animate_hide_input" ) )
	-- self._input_panel:gradient( { name = "input_bg", gradient_points = { 0, Color.black:with_alpha( 0.25 ), 1, Color.black:with_alpha( 0.25 ) }, layer = -1, valign = "grow", h = self._input_panel:h() } )
end

function ChatGui:_layout_output_panel()
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	
	scroll_panel:set_w( self._output_width )
	output_panel:set_w( self._output_width )
	
	-- Adjust size of lines and calculates number of lines
	local lines = 0
	for i = #self._lines ,1 , -1 do
		local line = self._lines[ i ][1]
		local line_bg = self._lines[ i ][2]
		line:set_w( output_panel:w() )
		local _,_,w,h = line:text_rect()
		line:set_h( h )
		line_bg:set_w( w + 2 )
		line_bg:set_h( h )
		lines = lines + line:number_of_lines()
	end
	
	local line_height = 22 -- tweak_data.menu.pd2_small_font_size
	local max_lines = self._max_lines -- 15
	
	local scroll_at_bottom = scroll_panel:bottom() == output_panel:h()
	
	-- Sets size of output panel
	output_panel:set_h( math.round( line_height * math.min( max_lines, lines ) ) )
	scroll_panel:set_h( math.round( line_height * lines ) )
	
	-- Position each line in the outputpanel
	local y = 0
	for i = #self._lines ,1 , -1 do
		local line = self._lines[ i ][1]
		local line_bg = self._lines[ i ][2]
		local _,_,w,h = line:text_rect()
		line:set_bottom( scroll_panel:h() - y )
		line_bg:set_bottom( line:bottom() )
		line:set_left( line:left() )
		y = y + h
	end
	
	output_panel:set_bottom( math.round( self._input_panel:top() ) )
	if( lines <= max_lines or scroll_at_bottom ) then
		scroll_panel:set_bottom( output_panel:h() )
	end
	self:set_scroll_indicators()
end

function ChatGui:_layout_input_panel()
	self._input_panel:set_w( self._panel_width - self._input_panel:x() )
	local say = self._input_panel:child( "say" )
	local input_text = self._input_panel:child( "input_text" )
	input_text:set_left( say:right() + 4 )
	input_text:set_w( self._input_panel:w() - input_text:left() )
	self._input_panel:child( "input_bg" ):set_w( input_text:w() )
	self._input_panel:child( "input_bg" ):set_x( input_text:x() )
	
	local focus_indicator = self._input_panel:child( "focus_indicator" )
	focus_indicator:set_shape( input_text:shape() )
	
	self._input_panel:set_y( self._input_panel:parent():h() - self._input_panel:h() )
	self._input_panel:set_x( self._panel:child( "output_panel" ):x() )
end

function ChatGui:set_scroll_indicators()
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	
	local scroll_up_indicator_shade = output_panel:child( "scroll_up_indicator_shade" )
	local scroll_up_indicator_arrow = self._panel:child( "scroll_up_indicator_arrow" )
	
	local scroll_down_indicator_shade = output_panel:child( "scroll_down_indicator_shade" )
	local scroll_down_indicator_arrow = self._panel:child( "scroll_down_indicator_arrow" )
	
	local scroll_bar = self._panel:child( "scroll_bar" )
	
	scroll_up_indicator_shade:set_top( 0 )
	scroll_down_indicator_shade:set_bottom( output_panel:h() )
	
	scroll_up_indicator_arrow:set_righttop( output_panel:left() - 2, output_panel:top() + 2 )
	scroll_down_indicator_arrow:set_rightbottom( output_panel:left() - 2, output_panel:bottom() - 2 )
	
	local bar_h = scroll_down_indicator_arrow:top() - scroll_up_indicator_arrow:bottom()
	
	if scroll_panel:h() ~= 0 then
		local old_h = scroll_bar:h()
		scroll_bar:set_h( bar_h * (output_panel:h()) / scroll_panel:h())
		if old_h ~= scroll_bar:h() then
			self._scroll_bar_box_class:create_sides( scroll_bar:child("scroll_bar_box_panel"), { sides = { 2, 2, 0, 0 } } )
		end
	end
	
	local sh = scroll_panel:h() ~= 0 and scroll_panel:h() or 1
	scroll_bar:set_y( scroll_up_indicator_arrow:bottom() - ( scroll_panel:y() * (output_panel:h()-scroll_up_indicator_arrow:h()*2) / sh ) ) 
	scroll_bar:set_center_x( scroll_up_indicator_arrow:center_x() )
	
	local visible = scroll_panel:h() > output_panel:h()
	local scroll_up_visible = visible and ( scroll_panel:top() < 0 )
	local scroll_dn_visible = visible and ( scroll_panel:bottom() > output_panel:h() )
	
	self:_layout_input_panel()
	scroll_bar:set_visible( visible )
	
	local update_scroll_indicator_box = false
	if scroll_up_indicator_arrow:visible() ~= scroll_up_visible then
		scroll_up_indicator_shade:set_visible( false ) -- scroll_up_visible )
		scroll_up_indicator_arrow:set_visible( scroll_up_visible )
		update_scroll_indicator_box = true
	end
	
	if scroll_down_indicator_arrow:visible() ~= scroll_dn_visible then
		scroll_down_indicator_shade:set_visible( false ) -- scroll_dn_visible )
		scroll_down_indicator_arrow:set_visible( scroll_dn_visible )
		update_scroll_indicator_box = true
	end
	if update_scroll_indicator_box then
		self._scroll_indicator_box_class:create_sides( output_panel, { sides={ 0, 0, scroll_up_visible and 2 or 0, scroll_dn_visible and 2 or 0 } } )
	end
end

function ChatGui:set_size( x, y )
	ChatGui.super.set_size( self, x, y )
	
	self:_layout_output_panel()
	self:_layout_input_panel()
end

function ChatGui:input_focus()
	if self._esc_focus_delay then
		self._esc_focus_delay = nil
		return 1
	end
	if self._enter_loose_focus_delay then
		self._enter_loose_focus_delay = nil
		return true
	end
	return self._focus
end


function ChatGui:mouse_moved( x, y )
	if not self._enabled then
		return
	end
	local inside = self._input_panel:inside( x, y )
	self._input_panel:child( "focus_indicator" ):set_visible( inside or self._focus )
	
	if( self:moved_scroll_bar( x, y ) ) then
		return true, "grab"
	elseif 	(self._panel:child("scroll_bar"):visible() and self._panel:child("scroll_bar"):inside( x, y )) or 
					(self._panel:child("scroll_down_indicator_arrow"):visible() and self._panel:child("scroll_down_indicator_arrow"):inside( x, y )) or 
					(self._panel:child("scroll_up_indicator_arrow"):visible() and self._panel:child("scroll_up_indicator_arrow"):inside( x, y )) then
		return false, "hand"
	end
	
	return false, inside and "arrow"
end

function ChatGui:moved_scroll_bar( x, y )
	if self._grabbed_scroll_bar then
		self._current_y = self:scroll_with_bar( y, self._current_y )
		return true
	end
	return false
end

function ChatGui:scroll_with_bar( target_y, current_y )
	local line_height = 22 -- tweak_data.menu.pd2_small_font_size
	local diff = current_y - target_y
	
	if( diff == 0 ) then
		return current_y
	end
	
	local dir = diff / math.abs(diff)
	while( math.abs( current_y - target_y ) >= line_height ) do
		current_y = current_y - line_height * dir
		
		if( dir > 0 ) then
			self:scroll_up()
			self:set_scroll_indicators()
		elseif( dir < 0 ) then
			self:scroll_down()
			self:set_scroll_indicators()
		end
	end
	return current_y
end

function ChatGui:mouse_released( o, button, x, y )
	if not self._enabled then
		return
	end
	self:release_scroll_bar()
end

function ChatGui:mouse_pressed( button, x, y )
	if not self._enabled then
		return
	end
	local inside = self._input_panel:inside( x, y )
	if inside then
		self:_on_focus()
		return true
	end
	
	if( self._panel:child( "output_panel" ):inside( x, y ) ) then
		if button == Idstring( "mouse wheel down" ) then
			if self:mouse_wheel_down( x, y ) then
				self:_on_focus()
			end
		elseif button == Idstring( "mouse wheel up" ) then
			if self:mouse_wheel_up( x, y ) then
				self:_on_focus()
			end
		elseif button == Idstring( "0" ) then
			if self:check_grab_scroll_panel( x, y ) then
				self:_on_focus()
			end
		end
		self:set_scroll_indicators()
		return true
	elseif button == Idstring( "0" ) then
		if self:check_grab_scroll_bar( x, y ) then
			self:set_scroll_indicators()
			self:_on_focus()
			return true
		end
	end
	
	self:_loose_focus()
end

function ChatGui:check_grab_scroll_panel( x, y )
	return false
end

function ChatGui:check_grab_scroll_bar( x, y )
	local scroll_bar = self._panel:child("scroll_bar")
	
	if scroll_bar:visible() and scroll_bar:inside( x, y ) then
		self._grabbed_scroll_bar = true
		self._current_y = y
		return true
	end
	if self._panel:child( "scroll_up_indicator_arrow" ):visible() and self._panel:child( "scroll_up_indicator_arrow" ):inside( x, y ) then
		self:scroll_up( x, y )
		self._pressing_arrow_up = true
		return true
	end
	if self._panel:child( "scroll_down_indicator_arrow" ):visible() and self._panel:child( "scroll_down_indicator_arrow" ):inside( x, y ) then
		self:scroll_down( x, y )
		self._pressing_arrow_down = true
		return true
	end
	return false
end

function ChatGui:release_scroll_bar()
	self._pressing_arrow_up = nil
	self._pressing_arrow_down = nil
	
	if self._grabbed_scroll_bar then
		self._grabbed_scroll_bar = nil
		return true
	end
	
	return false
end

function ChatGui:scroll_up()
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	if( scroll_panel:h() > output_panel:h() ) then
		if( scroll_panel:top() == 0 ) then
			self._one_scroll_dn_delay = true
		end
		scroll_panel:set_top( math.min( 0, scroll_panel:top() + 22 ) )
		return true
	end
end

function ChatGui:scroll_down()
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	if( scroll_panel:h() > output_panel:h() ) then
		if( scroll_panel:bottom() == output_panel:h() ) then
			self._one_scroll_up_delay = true
		end
		scroll_panel:set_bottom( math.max( scroll_panel:bottom() - 22, output_panel:h() ) )
		return true
	end
end

function ChatGui:mouse_wheel_up( x, y )
	if not self._enabled then
		return
	end
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	
	if( self._one_scroll_up_delay ) then
		self._one_scroll_up_delay = nil
		return true
	end
	
	return self:scroll_up()
end

function ChatGui:mouse_wheel_down( x, y )
	if not self._enabled then
		return
	end
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	
	if( self._one_scroll_dn_delay ) then
		self._one_scroll_dn_delay = nil
		return true
	end
	
	return self:scroll_down()
end

function ChatGui:open_page()
	-- self:_on_focus()
end

function ChatGui:close_page()
	self:_loose_focus()
end

function ChatGui:_on_focus()
	if not self._enabled then
		return
	end
	if self._focus then
		return
	end
	
	-- self._skip_first = true
	
	local output_panel = self._panel:child( "output_panel" ) 
	output_panel:stop()
	output_panel:animate( callback( self, self, "_animate_show_component" ), output_panel:alpha() )
	-- self:set_output_alpha( 1 )
		
	self._input_panel:stop()
	self._input_panel:animate( callback( self, self, "_animate_show_input" ) )
	
	-- print( "on focus" )
	self._focus = true
	self._input_panel:child( "focus_indicator" ):set_color( Color( 0, 0, 0 ):with_alpha( 0.2 ) )
	self._ws:connect_keyboard( Input:keyboard() )
	self._input_panel:key_press( callback( self, self, "key_press" ) )
	-- self._input_panel:enter_text( callback( self, self, "enter_text" ) )
	self._input_panel:key_release( callback( self, self, "key_release" ) )
	self._enter_text_set = false
		
	self._input_panel:child( "input_bg" ):animate( callback( self, self, "_animate_input_bg" ) )
	 --[[if focus then
    	skip_first = skip_f ~= nil and skip_f or false
        self:enter_text(enter_text)
        self:key_press(key_press)
        self:key_release(key_release)
    else
        self:enter_text(nil)
        self:key_press(nil)
        self:key_release(nil)
    end]]
    
   self:update_caret()
end

function ChatGui:_loose_focus()
	if not self._focus then
		return
	end
	
	self._one_scroll_up_delay = nil
	self._one_scroll_dn_delay = nil
	
	-- print( "loose focus" )
	self._focus = false
	self._input_panel:child( "focus_indicator" ):set_color( Color.black:with_alpha( 0.2 ) )
	self._ws:disconnect_keyboard()
		
	self._input_panel:key_press( nil )
	self._input_panel:enter_text( nil )
	self._input_panel:key_release( nil )
	
	-- Fade away output
	self._panel:child( "output_panel" ):stop()
	self._panel:child( "output_panel" ):animate( callback( self, self, "_animate_fade_output" ) )
	
	self._input_panel:stop()
	self._input_panel:animate( callback( self, self, "_animate_hide_input" ) )
	
	local text = self._input_panel:child( "input_text" )
	text:stop()
	
	self._input_panel:child( "input_bg" ):stop()
	
	self:update_caret()
end

function ChatGui:_shift()
	local k = Input:keyboard()
	return k:down("left shift") or k:down("right shift") or (k:has_button("shift") and k:down("shift"))
end


function ChatGui.blink(o)
		    while true do
		        o:set_color(Color(0, 1, 1 ,1))
		        wait(0.3)
		        o:set_color(Color.white)
		        wait(0.3)
		    end
end
		
function ChatGui:set_blinking(b)
	local caret = self._input_panel:child( "caret" )
	
		    if b == self._blinking then return end
		    if b then caret:animate(self.blink) else caret:stop() end
		    self._blinking = b
		    if not self._blinking then caret:set_color(Color.white) end
end
		
function ChatGui:update_caret()
			local text = self._input_panel:child( "input_text" )
			local caret = self._input_panel:child( "caret" )
			
		    local s,e = text:selection()
		    local x,y,w,h = text:selection_rect()
		    if(s == 0 and e==0)then
		    	if(text:align()=="center")then
		    		x = text:world_x() + text:w()/2
		  		else
		    		x = text:world_x()
		  		end
		    	y = text:world_y()
		    end
		    -- h = 25
		    h = text:h()
		    
		    if w<3 then w=3 end
		    if not self._focus then w=0 h=0 end
		    caret:set_world_shape(x,y+2,w,h-4)
		   	self:set_blinking( s==e and self._focus )
		   	
		   	local mid = x/self._input_panel:child( "input_bg" ):w()
		   	self._input_panel:child( "input_bg" ):set_color( Color.black:with_alpha(0.4) ) --[[:set_gradient_points( { 0, Color.white:with_alpha( 0.0 ),
		   																-- math.clamp( mid - 0.1, 0, 1 ), Color.white:with_alpha( 0.0 ), 
		   																mid, Color.white:with_alpha( 0.25 ),
		   																-- math.clamp( mid + 0.1, 0, 1 ), Color.white:with_alpha( 0.0 ),
		   																1, Color.white:with_alpha( 0.0 ) } )]]
		   -- if focus then text:set_selection_color(Color.white) else text:set_selection_color(Color.black) end
end

-- This is where the actual text is added to the input field.
function ChatGui:enter_text(o,s)
	-- print( "enter_text", s, self._skip_first )
	-- Prevent chat input in waiting screen if stats screen is shown
	if managers.hud and managers.hud:showing_stats_screen() then
		return
	end
	
	if self._skip_first then
		self._skip_first = false
		return
	end
	
	local text = self._input_panel:child( "input_text" )
	
	if( type(self._typing_callback) ~="number" ) then
		self._typing_callback()
	end
	text:replace_text(s)
	local lbs = text:line_breaks()
	
	if( #lbs > 1 ) then
		local s = lbs[2]
		local e = utf8.len(text:text())
	
		text:set_selection( s, e )
		text:replace_text( "" )
	end
	
	-- local _, _, w, h = text:text_rect()
	-- local input_bg = self._input_panel:child( "input_bg" )
	-- input_bg:set_w( math.clamp( w+input_bg:left(), self._output_width, self._input_panel:w() ) - input_bg:left() )
	
	self:update_caret()
end
	
-- This is where key down modifying keys are checked (basicly checks same functionality as key_press)
function ChatGui:update_key_down( o, k )
			wait( 0.6 )
			local text = self._input_panel:child( "input_text" )
			
			while( self._key_pressed == k ) do
				local s,e = text:selection()
				local n = utf8.len(text:text())
				local d = math.abs( e - s )
				
				if self._key_pressed == Idstring( "backspace" ) then
					if s==e and s>0 then
						text:set_selection(s-1,e)
					end
					text:replace_text("")
					
					if( utf8.len(text:text()) < 1 and type(self._esc_callback) ~= "number" ) then
						-- self._esc_callback()
					end
				elseif self._key_pressed == Idstring( "delete" ) then
					if s==e and s<n then
						text:set_selection(s,e+1)
					end
					text:replace_text("")
					
					if( utf8.len(text:text()) < 1 and type(self._esc_callback) ~= "number" ) then
						-- self._esc_callback()
					end
				elseif self._key_pressed == Idstring( "left" ) then
					if e>s then 
						text:set_selection(s,s)
					elseif s>0 then
						text:set_selection(s-1,s-1)
					end
				elseif self._key_pressed == Idstring( "right" ) then
					if e>s then
						text:set_selection(e,e)
					elseif s<n then
						text:set_selection(s+1,s+1)
					end
				else
					self._key_pressed = false
				end
				
				self:update_caret()
				wait( 0.03 )
			end
end
		
function ChatGui:key_release( o, k )
			if( self._key_pressed == k  ) then
				self._key_pressed = false
			end
end

-- This is where text modifier keys are checked. It also adds an update function to check if the buttons is hold down.
function ChatGui:key_press( o, k )
	-- print( "key_press" )
		
			if self._skip_first then
				if k== Idstring( "enter" ) then -- Special case where enter is bound to open chat
					self._skip_first = false
				end
				return
			end
		
		-- Set enter text callback here, seems to fix the problem with bound chat key being added to message (t)
		if not self._enter_text_set then
			self._input_panel:enter_text( callback( self, self, "enter_text" ) )
			self._enter_text_set = true
		end
			
	local text = self._input_panel:child( "input_text" )
			
			local s,e = text:selection()
			local n = utf8.len(text:text())
			local d = math.abs( e - s )
			
			self._key_pressed = k
				
			text:stop()
			text:animate( callback( self, self, "update_key_down" ), k )
				
			if k == Idstring( "backspace" ) then
				if s==e and s>0 then
					text:set_selection(s-1,e)
				end
				text:replace_text("")
				
				if( utf8.len(text:text()) < 1 and type(self._esc_callback) ~= "number" ) then
					-- self._esc_callback()
				end
			elseif k == Idstring( "delete" ) then
				if s==e and s<n then
					text:set_selection(s,e+1)
				end
				text:replace_text("")
				
				if( utf8.len(text:text()) < 1 and type(self._esc_callback) ~= "number" ) then
					-- self._esc_callback()
				end
			elseif k == Idstring( "left" ) then
				if e>s then 
					text:set_selection(s,s)
				elseif s>0 then
					text:set_selection(s-1,s-1)
				end
			elseif k == Idstring( "right" ) then
				if e>s then
					text:set_selection(e,e)
				elseif s<n then
					text:set_selection(s+1,s+1)
				end
			elseif self._key_pressed == Idstring( "end" ) then
					text:set_selection(n,n)
			elseif self._key_pressed == Idstring( "home" ) then
					text:set_selection(0,0)
			elseif k== Idstring( "enter" ) then
				if type(self._enter_callback) ~= "number" then
					self._enter_callback()
				end
			elseif k== Idstring( "esc" ) then
				if type(self._esc_callback) ~= "number" then
					text:set_text("")
					text:set_selection(0,0)
					-- print( "ESCAPE" )	
					self._esc_callback()
				end
			end
			self:update_caret()
end



function ChatGui:send_message( name, message )

end

function ChatGui:receive_message( name, message, color )
	-- print( "receive_message", name, message, color )
	if( not alive( self._panel ) ) then
		return
	end
	local output_panel = self._panel:child( "output_panel" )
	local scroll_panel = output_panel:child( "scroll_panel" )
	
	local len = utf8.len( name )+1
	
	local line = scroll_panel:text( { text = name..": "..message, font = tweak_data.menu.pd2_small_font, font_size = tweak_data.menu.pd2_small_font_size, x = 0, y = 0,
									align="left", halign="left", vertical="top", hvertical="top", blend_mode="normal", wrap = true, word_wrap = true,
									color = color, layer = 0 } )
	-- line:set_debug( true )
	local total_len = utf8.len( line:text() )
		
	line:set_range_color( 0, len, color )							
	line:set_range_color( len, total_len, Color.white )
	
	local _,_,w,h = line:text_rect()
	line:set_h( h )
	
	local line_bg = scroll_panel:rect( { color=Color.black:with_alpha(0.5), layer = -1, halign="left", hvertical="top" } ) -- gradient( { gradient_points = { 0, Color.white:with_alpha( 0.0 ), 0.2, Color.white:with_alpha( 0.25 ), 1, Color.white:with_alpha( 0.0 ) }, layer = -1, valign = "grow", blend_mode = "sub", h = self._input_panel:h() } )
	-- line_bg:set_x( line:x() )
	-- line_bg:set_w( output_panel:w() - w + 2 )
	line_bg:set_h( h )
	
	-- LIMIT AMOUNT OF LINES
	table.insert( self._lines, { line, line_bg } )
	
	self:_layout_output_panel()
	-- utf8.len
	
	-- Recieving message when not in focus. Shows the output panel and starts the fade out animation
	if not self._focus then
		-- local output_panel = self._panel:child( "output_panel" ) 
		output_panel:stop()
		output_panel:animate( callback( self, self, "_animate_show_component" ), output_panel:alpha() )
		-- Fade away output
		output_panel:animate( callback( self, self, "_animate_fade_output" ) )
		-- Fade away output
		-- scroll_panel:animate( callback( self, self, "_animate_fade_output" ) )
	end
end

function ChatGui:_animate_fade_output()
	local wait_t = 10
	local fade_t = 1
	local t = 0
	while t < wait_t do
		local dt = coroutine.yield()
		t = t + dt
	end
	local t = 0
	while t < fade_t do
		local dt = coroutine.yield()
		t = t + dt
		self:set_output_alpha( 1 - t/fade_t )
	end
	self:set_output_alpha( 0 )
end

function ChatGui:_animate_show_component( panel, start_alpha )
	local TOTAL_T = 0.25
	local t = 0
	start_alpha = start_alpha or 0
	while t < TOTAL_T do
		local dt = coroutine.yield()
		t = t + dt
		
		panel:set_alpha( start_alpha + t/TOTAL_T * (1-start_alpha) )
	end
	panel:set_alpha( 1 )
end

function ChatGui:_animate_show_input( input_panel )
	local TOTAL_T = 0.20
	local start_alpha = input_panel:alpha()
	local end_alpha = 1
	over( TOTAL_T, function(p) input_panel:set_alpha( math.lerp( start_alpha, end_alpha, p ) ) end )
end

function ChatGui:_animate_hide_input( input_panel )
	local TOTAL_T = 0.20
	local start_alpha = input_panel:alpha()
	local end_alpha = 0.7
	over( TOTAL_T, function(p) input_panel:set_alpha( math.lerp( start_alpha, end_alpha, p ) ) end )
	--[[
	while t < TOTAL_T do
		local dt = coroutine.yield()
		t = t + dt
		
		input_panel:set_alpha( ( 1 - t/TOTAL_T ) * end_alpha )
	end
	input_panel:set_alpha( end_alpha )
	]]
end

function ChatGui:_animate_input_bg( input_bg )
	local t = 0
	while( true ) do
		local dt = coroutine.yield()
		t = t + dt
		local a = 0.75 + (1 + math.sin( t * 200) ) / 8
		-- print( a )
		input_bg:set_alpha( a )
	end
end

function ChatGui:set_output_alpha( alpha )
	self._panel:child( "output_panel" ):set_alpha( alpha )
end

function ChatGui:close( ... )
	-- ChatGui.super.close( self, ... )
	
	self._panel:child( "output_panel" ):stop()
	self._input_panel:stop()
	
	self._hud_panel:remove( self._panel )
	
	managers.chat:unregister_receiver( self._channel_id, self )
end


