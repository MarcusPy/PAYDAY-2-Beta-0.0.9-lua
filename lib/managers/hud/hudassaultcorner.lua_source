HUDAssaultCorner = HUDAssaultCorner or class()

function HUDAssaultCorner:init( hud, full_hud )
	self._hud_panel = hud.panel
	self._full_hud_panel = full_hud.panel
	
	if( self._hud_panel:child( "assault_panel" ) ) then
		self._hud_panel:remove( self._hud_panel:child( "assault_panel" ) )
	end
	
	local size = 200
	local assault_panel = self._hud_panel:panel( { visible = false, name = "assault_panel", w = 400, h = 100 } )
	assault_panel:set_top( 0 )
	-- assault_panel:set_center( self._hud_panel:w(), 0 )
	assault_panel:set_right( self._hud_panel:w() )
	-- assault_panel:set_center( self._hud_panel:w()/2, self._hud_panel:h()/2 )
	
	-- assault_panel:set_debug( true )
	
	-- self._assault_color = Color( 1, 255/255, 224/255, 50/255 )
	self._assault_color = Color( 1, 255/255, 255/255, 0/255 )
		
	local icon_assaultbox = assault_panel:bitmap( { halign="right", valign="top", color = Color.yellow, name = "icon_assaultbox", blend_mode = "add", visible = true, layer = 0, texture = "guis/textures/pd2/hud_icon_assaultbox", x = 0, y = 0, w = 24, h = 24 } )
	icon_assaultbox:set_right( icon_assaultbox:parent():w() )
		
	self._bg_box = HUDBGBox_create( assault_panel, { w = 242, h = 38, x = 0, y = 0 }, { color = self._assault_color, blend_mode = "add" } )
	self._bg_box:set_right( icon_assaultbox:left() - 3 )
	-- self._bg_box:animate( callback( nil, _G, "HUDBGBox_animate_open_left" ), nil, 242, function() end, { attention_color = self._assault_color } )
	
	local yellow_tape = assault_panel:rect( { visible = false, name="yellow_tape", h=tweak_data.hud.location_font_size * 1.5, w=size*3, color=Color(1, 0.8, 0), layer=1 } )
	yellow_tape:set_center( 10, 10 )
	yellow_tape:set_rotation( 30 )
	yellow_tape:set_blend_mode( "add" )
	
	assault_panel:panel( { name="text_panel", layer=1, w=yellow_tape:w() } ):set_center( yellow_tape:center() )
	-- assault_panel:text( { name="assault_title", text="ASSAULT", layer = 11, valign="top", align = "center", vertical = "center", x = w-size, y = 0, color = Color.black, font_size = tweak_data.hud_corner.assault_size, font = tweak_data.hud_corner.assault_font } )
	
	-- Hostages -------------------------------------------------------	
	if self._hud_panel:child( "hostages_panel" ) then	
		self._hud_panel:remove( self._hud_panel:child( "hostages_panel" ) )
	end
	
	local hostages_panel = self._hud_panel:panel( { name = "hostages_panel", w = size, h = size, x = self._hud_panel:w() - size } )
	-- hostages_panel:set_debug( true )
	self._hostages_bg_box = HUDBGBox_create( hostages_panel, { w = 38, h = 38, x = 0, y = 0 }, {} )
	self._hostages_bg_box:set_right( hostages_panel:w() )
	local num_hostages = self._hostages_bg_box:text( { name = "num_hostages", text = "0", valign="center", align = "center", vertical = "center", w = self._hostages_bg_box:w(), h = self._hostages_bg_box:h(), layer = 1, x = 0 , y = 0, color = Color.white, font = tweak_data.hud_corner.assault_font, font_size = tweak_data.hud_corner.numhostages_size } )
	
	-- local num_hostages = hostages_panel:text( { name = "num_hostages", text = "0", valign="top", align = "right", vertical = "center", w = 40, h = 48, layer = 1, x = hostages_panel:w() - 42 , y = 0, color = Color.white, font = tweak_data.hud_corner.assault_font } )
	-- local texture, rect = tweak_data.hud_icons:get_icon_data( "interaction_trade" )
	-- local hostages_icon = hostages_panel:bitmap( { name = "hostages_icon", texture = texture, texture_rect = rect, valign="top", layer = 1, x = hostages_panel:w() - num_hostages:w() - rect[4], y = 0 } )
	local hostages_icon = hostages_panel:bitmap( { name = "hostages_icon", texture = "guis/textures/pd2/hud_icon_hostage", valign="top", layer = 1, x = 0, y = 0 } )
	hostages_icon:set_right( self._hostages_bg_box:left() )
	hostages_icon:set_center_y( self._hostages_bg_box:h()/2 )
		
	-- Point of no return -------------------------------------------------------
	if self._hud_panel:child( "point_of_no_return_panel" ) then	
		self._hud_panel:remove( self._hud_panel:child( "point_of_no_return_panel" ) )
	end
	
	local size = 300
	local point_of_no_return_panel = self._hud_panel:panel( { visible = false, name = "point_of_no_return_panel", w = size, h = 40, x = self._hud_panel:w() - size } )
	-- point_of_no_return_panel:set_debug( true )
	
	self._noreturn_color = Color( 1, 255/255, 0/255, 0/255)
	
	local icon_noreturnbox = point_of_no_return_panel:bitmap( { halign="right", valign="top", color = self._noreturn_color, name = "icon_noreturnbox", blend_mode = "add", visible = true, layer = 0, texture = "guis/textures/pd2/hud_icon_noreturnbox", x = 0, y = 0, w = 24, h = 24 } )
	icon_noreturnbox:set_right( icon_noreturnbox:parent():w() )
	
	self._noreturn_bg_box = HUDBGBox_create( point_of_no_return_panel, { w = 242, h = 38, x = 0, y = 0 }, { color = self._noreturn_color, blend_mode = "add" } )
	self._noreturn_bg_box:set_right( icon_noreturnbox:left() - 3 )
		
	local w = point_of_no_return_panel:w()
	local size = 200 - tweak_data.hud.location_font_size
	
	local point_of_no_return_text = self._noreturn_bg_box:text( { name = "point_of_no_return_text", text = "", blend_mode = "add", layer = 1, valign="center", align = "right", vertical = "center", x = 0, y = 0, color = self._noreturn_color, font_size = tweak_data.hud_corner.noreturn_size, font = tweak_data.hud_corner.assault_font } )
	point_of_no_return_text:set_text( utf8.to_upper( managers.localization:text( "hud_assault_point_no_return_in", { time = "" } ) ) )
	point_of_no_return_text:set_size( self._noreturn_bg_box:w(), self._noreturn_bg_box:h() )
	
	local point_of_no_return_timer = self._noreturn_bg_box:text( { name = "point_of_no_return_timer", text = "", blend_mode = "add", layer = 1, valign="center", align = "center", vertical = "center", x = 0, y = 0, color = self._noreturn_color, font_size = tweak_data.hud_corner.noreturn_size, font = tweak_data.hud_corner.assault_font } )
	local _,_,w,h = point_of_no_return_timer:text_rect()
	point_of_no_return_timer:set_size( 46, self._noreturn_bg_box:h() ) -- w * 2, w * 2 )
	point_of_no_return_timer:set_right( point_of_no_return_timer:parent():w() )
	-- point_of_no_return_timer:set_debug( true )
	-- point_of_no_return_timer:set_rotation( 45 )
	point_of_no_return_text:set_right( math.round( point_of_no_return_timer:left() ) )
	
	
	-- Casing -------------------------------------------------------
	if self._hud_panel:child( "casing_panel" ) then	
		self._hud_panel:remove( self._hud_panel:child( "casing_panel" ) )
	end
	
	local size = 300
	local casing_panel = self._hud_panel:panel( { visible = false, name = "casing_panel", w = size, h = 40, x = self._hud_panel:w() - size } )
	-- point_of_no_return_panel:set_debug( true )
	
	self._casing_color = Color.white -- Color( 0, 119/255, 178/255 ) -- Color( 1, 0/255, 255/255, 0/255)
	
	local icon_casingbox = casing_panel:bitmap( { halign="right", valign="top", color = self._casing_color, name = "icon_casingbox", blend_mode = "add", visible = true, layer = 0, texture = "guis/textures/pd2/hud_icon_stealthbox", x = 0, y = 0, w = 24, h = 24 } )
	icon_casingbox:set_right( icon_casingbox:parent():w() )
	
	self._casing_bg_box = HUDBGBox_create( casing_panel, { w = 242, h = 38, x = 0, y = 0 }, { color = self._casing_color, blend_mode = "add" } )
	self._casing_bg_box:set_right( icon_casingbox:left() - 3 )
		
	local w = casing_panel:w()
	local size = 200 - tweak_data.hud.location_font_size
	
	casing_panel:panel( { name="text_panel", layer=1, w=yellow_tape:w() } ):set_center( yellow_tape:center() )
end

function HUDAssaultCorner:_animate_assault( bg_box )
	local assault_panel = self._hud_panel:child( "assault_panel" )
	local icon_assaultbox = assault_panel:child( "icon_assaultbox" )
	
	-- print( inspect( self._bg_box:script().text_list ) )
	
	-- Flash icon
	icon_assaultbox:stop()
	icon_assaultbox:animate( callback( self, self, "_show_icon_assaultbox" ) )
			
	self._bg_box:animate( callback( nil, _G, "HUDBGBox_animate_open_left" ), 0.75, 242, function() end, { attention_color = self._assault_color, attention_forever = true } )
	
	local text_panel = self._bg_box:child( "text_panel" )
	text_panel:stop()
	text_panel:animate( callback( self, self, "_animate_text" ) )
end

-- Creates and move list of texts
function HUDAssaultCorner:_animate_text( text_panel, bg_box, color )
	local text_list = (bg_box or self._bg_box):script().text_list
	local text_index = 0
	local texts = {}
	local padding = 10
	
	local create_new_text = function( text_panel, text_list, text_index, texts )
		if( texts[text_index] and texts[text_index].text ) then
			text_panel:remove( texts[text_index].text )
			texts[text_index] = nil
		end
		local text = text_panel:text( { text=utf8.to_upper( managers.localization:text( text_list[text_index] ) ), layer=1, align="center", vertical="center", blend_mode = "add", color=color or self._assault_color, font_size=tweak_data.hud_corner.assault_size, font=tweak_data.hud_corner.assault_font, w=10, h=10 } )
		local _, _, w, h = text:text_rect()
		text:set_size( w, h )
		
		texts[text_index] = { x=text_panel:w() + w*0.5 + padding*2, text=text }
	end
	
	while true do
		local dt = coroutine.yield()
		local last_text = texts[text_index]
		if( last_text and last_text.text ) then
			if( last_text.x + last_text.text:w()*0.5 + padding < text_panel:w() ) then
				text_index = (text_index % #text_list) + 1
				create_new_text( text_panel, text_list, text_index, texts )
			end
		else
			text_index = (text_index % #text_list) + 1
			create_new_text( text_panel, text_list, text_index, texts )
		end
		
		local speed = 90
	
		for i, data in pairs( texts ) do
			if( data.text ) then
				data.x = data.x - dt * speed
				data.text:set_center_x( data.x )
				data.text:set_center_y( text_panel:h()*0.5 )
				
				if( data.x + data.text:w()*0.5 < 0 ) then
					text_panel:remove( data.text )
					data.text = nil
				end
			end
		end
		
	end
end

--[[
function HUDAssaultCorner:_animate_assault_old( assault_panel )
	local text_panel = assault_panel:child("text_panel")
	local text_list = text_panel:script().text_list
	local text_index = 0
	local padding = 10
	local y = text_panel:h() * 0.75
	
	local dt = 0
	
	local entering = true
	
	local texts = {}
	text_panel:clear()
	
	local offset = 50
	local tape_center_x = assault_panel:parent():w() - offset
	local tape_center_y = 0 + offset
	local offscreen_pos = offset + 200
	local current_pos = offscreen_pos
	
	assault_panel:set_center( tape_center_x + current_pos, tape_center_y - current_pos )
	
	local create_new_text = function( text_panel, text_list, text_index, texts )
		if( texts[text_index] and texts[text_index].text ) then
			text_panel:remove( texts[text_index].text )
			texts[text_index] = nil
		end
		local text = text_panel:text( { text=utf8.to_upper( managers.localization:text( text_list[text_index] ) ), layer=1, align="center", vertical="center", rotation=30, color=Color.black, font_size=tweak_data.hud_corner.assault_size, font=tweak_data.hud_corner.assault_font, w=10, h=10 } )
		local _, _, w, h = text:text_rect()
		text:set_size( w, h )
		
		texts[text_index] = { x=text_panel:w() + w*0.5 + padding*2, text=text }
	end
	
	while true do
		dt = coroutine.yield()
		local last_text = texts[text_index]
		if( last_text and last_text.text ) then
			if( last_text.x + last_text.text:w()*0.5 + padding <  text_panel:w() ) then
				text_index = (text_index % #text_list) + 1
				create_new_text( text_panel, text_list, text_index, texts )
			end
		else
			text_index = (text_index % #text_list) + 1
			create_new_text( text_panel, text_list, text_index, texts )
		end
		
		local speed = 90
		if( not self._assault ) then
			current_pos = current_pos + dt * (130+offset)
			assault_panel:set_center( tape_center_x + current_pos, tape_center_y - current_pos )
			
			if( current_pos >= offscreen_pos ) then
				break;	-- done!
			end
			speed = math.lerp( 90, 1240, (current_pos/offscreen_pos)^0.5 )
		elseif( entering ) then
			current_pos = current_pos - dt * (130+offset)
			assault_panel:set_center( tape_center_x + current_pos, tape_center_y - current_pos )
			
			speed = math.lerp( 1240, 90, ((offscreen_pos-current_pos)/offscreen_pos)^2 )
			if( current_pos <= 0 ) then
				assault_panel:set_center( tape_center_x, tape_center_y )
				entering = false
				current_pos = 0
			end
		end
		
		for i, data in pairs( texts ) do
			if( data.text ) then
				data.x = data.x - dt * speed
				data.text:set_center_x( math.cos(30) * data.x )
				data.text:set_center_y( math.sin(30) * (data.x - text_panel:w()*0.5) - y )
				
				if( data.x + data.text:w()*0.5 < 0 ) then
					text_panel:remove( data.text )
					data.text = nil
				end
			end
		end
	end
	
	text_panel:clear()
	assault_panel:hide()
	self:_show_hostages()
end]]

function HUDAssaultCorner:sync_start_assault( data )
	if self._point_of_no_return then
		return
	end
	
	self:_hide_hostages()
	-- self:_start_assault( { "hud_assault_assault", "hud_assault_cover", "hud_assault_end_line", "hud_assault_assault", "hud_assault_cover", "hud_assault_end_line", "hud_assault_assault", "hud_assault_cover", "hud_assault_end_line" } )
	self:_start_assault( { "hud_assault_assault", "hud_assault_end_line", "hud_assault_assault", "hud_assault_end_line", "hud_assault_assault", "hud_assault_end_line" } )
end

function HUDAssaultCorner:sync_end_assault( result )

	if self._point_of_no_return then
		return
	end
	
	self:_end_assault()
end

function HUDAssaultCorner:_start_assault( text_list )
	text_list = text_list or { "" }
	
	local assault_panel = self._hud_panel:child( "assault_panel" )
	local text_panel = assault_panel:child("text_panel")
	text_panel:script().text_list = {}
	self._bg_box:script().text_list = {}
	
	for _, text_id in ipairs( text_list ) do
		table.insert( text_panel:script().text_list, text_id )
		table.insert( self._bg_box:script().text_list, text_id )
	end
	
	self._assault = true
	
	if self._bg_box:child( "text_panel" ) then
		self._bg_box:child( "text_panel" ):stop()
		-- self._bg_box:remove( self._bg_box:child( "text_panel" ) )
		self._bg_box:child( "text_panel" ):clear()
	else
		self._bg_box:panel( { name = "text_panel" } )
	end
	
	self._bg_box:child( "bg" ):stop()
	--[[local bg = self._bg_box:child( "bg" )
	bg:stop()
	bg:animate( callback( nil, _G, "HUDBGBox_animate_bg_attention" ), { color = self._assault_color } )]]
	self._bg_box:stop()
	-- self._bg_box:animate( callback( nil, _G, "HUDBGBox_animate_open_left" ), nil, 242, function() end )
	-- assault_panel:stop()
	assault_panel:set_visible( true )
	self._bg_box:animate( callback( self, self, "_animate_assault" ) )
end

function HUDAssaultCorner:_end_assault()
	if not self._assault then
		return
	end
	
	self._assault = false
	-- self._hud_panel:child( "assault_panel" ):set_visible( false )
	-- self._full_hud_panel:child( "assault_panel" ):stop()
	-- self._exiting = true
	
	-- self._hud_panel:child( "hostages_panel" ):set_visible( true )
	
	self._bg_box:child( "text_panel" ):stop()
	self._bg_box:child( "text_panel" ):clear()
	
	local close_done = function()
		self._bg_box:set_visible( false )
		
		-- Flash icon
		local icon_assaultbox = self._hud_panel:child( "assault_panel" ):child( "icon_assaultbox" )
		icon_assaultbox:stop()
		icon_assaultbox:animate( callback( self, self, "_hide_icon_assaultbox" ) )
	end
	
	self._bg_box:stop()
	self._bg_box:animate( callback( nil, _G, "HUDBGBox_animate_close_left" ), close_done )
end

function HUDAssaultCorner:_show_icon_assaultbox( icon_assaultbox )
	local TOTAL_T = 2
	local t = TOTAL_T
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		local alpha = math.round( math.abs( ( (math.sin( (t) * 360 * 2 ) ) ) ) )
			
		icon_assaultbox:set_alpha( alpha )
	end
	icon_assaultbox:set_alpha( 1 )
end

function HUDAssaultCorner:_hide_icon_assaultbox( icon_assaultbox )
	local TOTAL_T = 1
	local t = TOTAL_T
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		local alpha = math.round( math.abs( ( (math.cos( (t) * 360 * 2 ) ) ) ) )
			
		icon_assaultbox:set_alpha( alpha )
	end
	icon_assaultbox:set_alpha( 0 )
	self:_show_hostages()
end

function HUDAssaultCorner:_show_hostages()
	if not self._point_of_no_return then
		self._hud_panel:child( "hostages_panel" ):show()
	end
end

function HUDAssaultCorner:_hide_hostages()
	self._hud_panel:child( "hostages_panel" ):hide()
end

-- Sets hostages amount
function HUDAssaultCorner:set_control_info( data )
	-- self._hud_panel:child( "hostages_panel" ):child( "num_hostages" ):set_text( ""..data.nr_hostages )
	self._hostages_bg_box:child( "num_hostages" ):set_text( ""..data.nr_hostages )
	local bg = self._hostages_bg_box:child( "bg" )
	bg:stop()
	bg:animate( callback( nil, _G, "HUDBGBox_animate_bg_attention" ), {} )
end

function HUDAssaultCorner:feed_point_of_no_return_timer( time, is_inside )
	-- print( "HUDAssaultCorner:feed_point_of_no_return_timer", time )
		
	time = math.floor( time )
	local minutes = math.floor( time/60 )
	local seconds = math.round( time - minutes*60 )
	local text = ((minutes < 10 and "0"..minutes) or minutes)..":"..((seconds<10 and "0"..seconds) or seconds)
		
	-- local t_text = managers.localization:text( "hud_assault_point_no_return_in", { time = text } )
	self._noreturn_bg_box:child( "point_of_no_return_timer" ):set_text( text )
end

function HUDAssaultCorner:show_point_of_no_return_timer()
	-- print( "HUDAssaultCorner:show_point_of_no_return_timer" )
	local delay_time = self._assault and 1.2 or 0
	
	self:_end_assault()
	
	local point_of_no_return_panel = self._hud_panel:child( "point_of_no_return_panel" )
				
	-- point_of_no_return_panel:set_visible( true )
	
	self._hud_panel:child( "hostages_panel" ):set_visible( false )
	
	point_of_no_return_panel:stop()
	point_of_no_return_panel:animate( callback( self, self, "_animate_show_noreturn" ), delay_time )
	
	self._point_of_no_return = true
end

function HUDAssaultCorner:hide_point_of_no_return_timer()
	-- print( "HUDAssaultCorner:hide_point_of_no_return_timer" )
	--[[
		This might not really ever happen, but if it does, it need to know if we are in assault instead.
	]]
	self._noreturn_bg_box:stop()
	self._hud_panel:child( "point_of_no_return_panel" ):set_visible( false )
	
	self._hud_panel:child( "hostages_panel" ):set_visible( true )
	
	self._point_of_no_return = false
end

function HUDAssaultCorner:flash_point_of_no_return_timer( beep )
	-- print( "HUDAssaultCorner:flash_point_of_no_return_timer", beep )
	
	local flash_timer = function (o)
		local t = 0
		
		while t < 0.5 do
			t = t + coroutine.yield()
			
			local n = 1 - math.sin( t * 180 )
			local r = math.lerp( 1 or self._point_of_no_return_color.r, 1, n )   -- 1, 0.8, 0.2
			local g = math.lerp( 0 or self._point_of_no_return_color.g, 0.8, n )
			local b = math.lerp( 0 or self._point_of_no_return_color.b, 0.2, n )
			
			o:set_color( Color( r, g, b ) )
			o:set_font_size( math.lerp( tweak_data.hud_corner.noreturn_size, tweak_data.hud_corner.noreturn_size * 1.25, n) )
		end
  	end
  	 	
  	-- local point_of_no_return_timer = self._hud_panel:child( "point_of_no_return_panel" ):child( "point_of_no_return_timer" )
  	local point_of_no_return_timer = self._noreturn_bg_box:child( "point_of_no_return_timer" )

	point_of_no_return_timer:animate( flash_timer )
end

------------------------------------------------------------------------

function HUDAssaultCorner:show_casing()
	-- print( "HUDAssaultCorner:show_point_of_no_return_timer" )
	local delay_time = self._assault and 1.2 or 0
	
	self:_end_assault()
	
	local casing_panel = self._hud_panel:child( "casing_panel" )
	local text_panel = casing_panel:child("text_panel")
	text_panel:script().text_list = {}
	self._casing_bg_box:script().text_list = {}
	
	for _, text_id in ipairs( { "hud_casing_mode_ticker", "hud_assault_end_line", "hud_casing_mode_ticker", "hud_assault_end_line" } ) do
		table.insert( text_panel:script().text_list, text_id )
		table.insert( self._casing_bg_box:script().text_list, text_id )
	end
	
	if self._casing_bg_box:child( "text_panel" ) then
		self._casing_bg_box:child( "text_panel" ):stop()
		-- self._bg_box:remove( self._bg_box:child( "text_panel" ) )
		self._casing_bg_box:child( "text_panel" ):clear()
	else
		self._casing_bg_box:panel( { name = "text_panel" } )
	end
	
	self._casing_bg_box:child( "bg" ):stop()
				
	-- point_of_no_return_panel:set_visible( true )
	
	self._hud_panel:child( "hostages_panel" ):set_visible( false )
	
	casing_panel:stop()
	casing_panel:animate( callback( self, self, "_animate_show_casing" ), delay_time )
	
	self._casing = true
end

function HUDAssaultCorner:hide_casing()
	-- print( "HUDAssaultCorner:hide_point_of_no_return_timer" )
	--[[
		This might not really ever happen, but if it does, it need to know if we are in assault instead.
	]]
		
	if self._casing_bg_box:child( "text_panel" ) then
		self._casing_bg_box:child( "text_panel" ):stop()
		self._casing_bg_box:child( "text_panel" ):clear()
	end
	
	-- Flash icon
		local icon_casingbox = self._hud_panel:child( "casing_panel" ):child( "icon_casingbox" )
		icon_casingbox:stop()
	
	local close_done = function()
		self._casing_bg_box:set_visible( false )
		
		-- Flash icon
		local icon_casingbox = self._hud_panel:child( "casing_panel" ):child( "icon_casingbox" )
		icon_casingbox:stop()
		icon_casingbox:animate( callback( self, self, "_hide_icon_assaultbox" ) )
	end
		
	self._casing_bg_box:stop()
	self._casing_bg_box:animate( callback( nil, _G, "HUDBGBox_animate_close_left" ), close_done )
	
--[[	
	self._casing_bg_box:stop()
	self._hud_panel:child( "casing_panel" ):set_visible( false )
	
	self._hud_panel:child( "hostages_panel" ):set_visible( true )]]
	
	self._casing = false
end

function HUDAssaultCorner:_animate_simple_text( text )
	local _,_,w,_ = text:text_rect()
	text:set_w( w + 10 )
	text:set_visible( true )
	text:set_x( text:parent():w() )
	-- text:set_debug( false )
	local x = text:x()
	local t = 0
	local speed = 90
	while( true ) do
		local dt = coroutine.yield()
		t = t + dt
		x = x - speed * dt
		text:set_x( x )
		if text:right() < 0 then
			text:set_x( text:parent():w() )
			x = text:x()
		end
		-- text:set_text( text:text() )
	end
	
end

function HUDAssaultCorner:_animate_show_casing( casing_panel, delay_time )
	local icon_casingbox = casing_panel:child( "icon_casingbox" )
	
	wait( delay_time )
	
	casing_panel:set_visible( true )
	
	-- Flash icon
	icon_casingbox:stop()
	icon_casingbox:animate( callback( self, self, "_show_icon_assaultbox" ) )
	
	local open_done = function()
		-- casing_text:animate( callback( self, self, "_animate_simple_text" ) )	
	end
	
	self._casing_bg_box:stop()		
	self._casing_bg_box:animate( callback( nil, _G, "HUDBGBox_animate_open_left" ), 0.75, 242, open_done, { attention_color = self._casing_color, attention_forever = true } )
	
	local text_panel = self._casing_bg_box:child( "text_panel" )
	text_panel:stop()
	text_panel:animate( callback( self, self, "_animate_text" ), self._casing_bg_box, Color.white )
end

------------------------------------------------------------------------

function HUDAssaultCorner:_animate_show_noreturn( point_of_no_return_panel, delay_time )
	-- local point_of_no_return_panel = self._hud_panel:child( "point_of_no_return_panel" )
	local icon_noreturnbox = point_of_no_return_panel:child( "icon_noreturnbox" )
	
	local point_of_no_return_text = self._noreturn_bg_box:child( "point_of_no_return_text" )
	point_of_no_return_text:set_visible( false )
	local point_of_no_return_timer = self._noreturn_bg_box:child( "point_of_no_return_timer" )
	point_of_no_return_timer:set_visible( false )
	
	wait( delay_time )
	
	point_of_no_return_panel:set_visible( true )
	
	-- Flash icon
	icon_noreturnbox:stop()
	icon_noreturnbox:animate( callback( self, self, "_show_icon_assaultbox" ) )
	
	local open_done = function()
		point_of_no_return_text:animate( callback( self, self, "_animate_show_texts" ), { point_of_no_return_text, point_of_no_return_timer } )	
	end
	
	self._noreturn_bg_box:stop()		
	self._noreturn_bg_box:animate( callback( nil, _G, "HUDBGBox_animate_open_left" ), 0.75, 242, open_done, { attention_color = self._casing_color, attention_forever = true } )
end

function HUDAssaultCorner:_animate_show_texts( anim_object, texts )
	for _,text in ipairs( texts ) do
		text:set_visible( true )
	end
	local TOTAL_T = 0.5
	local t = TOTAL_T
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		local alpha = math.round( math.abs( ( (math.sin( (t) * 360 * 3 ) ) ) ) )
		
		for _,text in ipairs( texts ) do
			text:set_alpha( alpha )
		end
	end
	for _,text in ipairs( texts ) do
		text:set_alpha( 1 )
	end
end
	
------------------------------------------------------------------------

function HUDAssaultCorner:test()
	self._hud_panel:child( "point_of_no_return_panel" ):animate( callback( self, self, "_animate_test_point_of_no_return" ) )
end

function HUDAssaultCorner:_animate_test_point_of_no_return( panel )
	managers.hud:show_point_of_no_return_timer()
	local t = 15
	local prev_time = t
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		local sec = math.floor( t )
		if math.floor( prev_time ) > sec then
			prev_time = sec
			managers.hud:flash_point_of_no_return_timer( sec <= 10 )
		end
		
		managers.hud:feed_point_of_no_return_timer( math.max( t, 0 ), false )
		-- self._test_circle:set_current( 10 *( t/2) )
	end
	-- print( "_animate_point_of_no_return done" )
	managers.hud:hide_point_of_no_return_timer()
end

