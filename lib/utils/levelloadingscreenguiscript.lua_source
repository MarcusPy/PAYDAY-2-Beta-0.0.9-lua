-- The code below was originally in a GUI xml but was moved here since the loading thread can't access script from within GUI xml.
LevelLoadingScreenGuiScript = LevelLoadingScreenGuiScript or class()

function LevelLoadingScreenGuiScript:init( scene_gui, res, progress, base_layer )
	self._scene_gui = scene_gui
	self._res = res
	self._base_layer = base_layer
	
	self._level_tweak_data = arg.load_level_data.level_tweak_data
	self._gui_tweak_data = arg.load_level_data.gui_tweak_data
	self._menu_tweak_data = arg.load_level_data.menu_tweak_data
	self._scale_tweak_data = arg.load_level_data.scale_tweak_data
	self._coords = arg.load_level_data.controller_coords or false
		
	self._gui_data = arg.load_level_data.gui_data
	self._workspace_size = self._gui_data.workspace_size
	self._saferect_size = self._gui_data.saferect_size
	
	local challenges = arg.load_level_data.challenges
		
	local safe_rect_pixels = self._gui_data.safe_rect_pixels
	local safe_rect = self._gui_data.safe_rect
	local aspect_ratio = self._gui_data.aspect_ratio
	self._safe_rect_pixels = safe_rect_pixels
	self._safe_rect = safe_rect
	
	self._gui_data_manager = GuiDataManager:new( self._scene_gui, res, safe_rect_pixels, safe_rect, aspect_ratio )
	self._back_drop_gui = MenuBackdropGUI:new( nil, self._gui_data_manager, true )
	self._back_drop_gui:set_pattern( "guis/textures/loading/loading_foreground", 1 )
	
	-- local panel = self._back_drop_gui:get_new_background_layer()
	-- self._back_drop_gui:set_abstract_background_layers( 3 )
	
	local base_panel = self._back_drop_gui:get_new_base_layer()
	local level_image = base_panel:bitmap( { texture = self._gui_data.bg_texture, layer = 0 } )
	level_image:set_alpha( 0.5 )
	
	print( "self._gui_data.bg_texture", self._gui_data.bg_texture )
	
	level_image:set_size( level_image:parent():h() * (level_image:texture_width() / level_image:texture_height()), level_image:parent():h() )
	level_image:set_position( 0, 0 )
	-- level_image:set_debug( true )
	
	local background_fullpanel = self._back_drop_gui:get_new_background_layer()
	local background_safepanel = self._back_drop_gui:get_new_background_layer()
	
	self._back_drop_gui:set_panel_to_saferect( background_safepanel )
	-- background_safepanel:set_debug( true )
	
	self._indicator = background_safepanel:bitmap( { name = "indicator", texture = "guis/textures/icon_loading", layer = 0 } )
		
	self._level_title_text = background_safepanel:text( { y = 0, text_id = "debug_loading_level", font = "fonts/font_large_mf", font_size = 36, color = Color.white, align="left", halign="left", vertical="bottom", layer = 0, h = 36 } )
	self._level_title_text:set_text( utf8.to_upper( self._level_title_text:text() ) )
	
	local _,_,w,h = self._level_title_text:text_rect()
	self._level_title_text:set_size( w, h )
	
	self._indicator:set_right( self._indicator:parent():w() )
	self._level_title_text:set_right( self._indicator:left() )
	
	local bg_loading_text = background_fullpanel:text( { y = 0, text_id = "debug_loading_level", font = "fonts/font_eroded", font_size = 80, h = 80, color = Color( 0.3, 97/255, 214/255, 255/255 ), align = "right", vertical = "top", layer = 0 } )
	bg_loading_text:set_text( utf8.to_upper( bg_loading_text:text() ) )
	
	local x, y = self._level_title_text:world_right(), self._level_title_text:world_center_y()
	bg_loading_text:set_world_right( x )
	bg_loading_text:set_world_center_y( y )
	bg_loading_text:move( 13, 3 )	

	self._back_drop_gui:animate_bg_text( bg_loading_text )
	
	if self._coords then
		self._controller = background_safepanel:bitmap( { texture="guis/textures/controller", layer = 1, w=512, h=256 } )
		self._controller:set_center( background_safepanel:w()/2, background_safepanel:h()/2 )
		
		for id, data in pairs( self._coords ) do
			data.text = background_safepanel:text( { name = id, text = data.string, font_size = 24, font = "fonts/font_medium_mf", 
												align=data.align, vertical=data.vertical, 
												halign="center", valign="center" } )
			
			local _,_,w,h = data.text:text_rect()
			data.text:set_size( w, h )
			
			if data.x then
				local x = self._controller:x() + data.x
				local y = self._controller:y() + data.y
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
		
		-- self._bg = background_fullpanel:rect( { visible = false, color = Color( 1, 0.1, 0.1, 0.1 ) } )
	end
	-- local _,_,w,h = bg_loading_text:text_rect()
	-- bg_loading_text:set_size( w, h )
	-- bg_loading_text:set_center_y( self._level_title_text:center_y() + safe_rect_pixels.y + 3 )
	-- bg_loading_text:set_right( self._level_title_text:right() + safe_rect_pixels.x + 10 )
	
	
	-- OLD
	
	
	--[[self._saferect = self._scene_gui:create_screen_workspace()
	self._saferect:hide()
	-- local safe_rect = safe_rect_pixels -- managers.viewport:get_safe_rect_pixels()
	local scaled_size = { width = 1198, height = 674, x = 0, y = 0 } -- self:scaled_size()
		
	local w = scaled_size.width
	local h = scaled_size.height
	local sh = math.min( safe_rect_pixels.height, safe_rect_pixels.width/(w/h) )
	local sw = math.min( safe_rect_pixels.width, safe_rect_pixels.height*(w/h) )
	local x = math.round( self._res.x/2 - (sh*(w/h))/2 )
	local y = math.round( self._res.y/2 - (sw/(w/h))/2 )
	self._screen_y = y
	self._saferect:set_screen( w, h, x, y, sw )-- res.x-1 )
		
	-- self._saferect = self._scene_gui:create_scaled_screen_workspace( self._workspace_size.w * safe_rect.width, self._workspace_size.h * safe_rect.height, safe_rect_pixels.x, safe_rect_pixels.y, safe_rect_pixels.width, safe_rect_pixels.height )
	self._fullrect = self._scene_gui:create_scaled_screen_workspace( self._workspace_size.w, self._workspace_size.h, self._workspace_size.x, self._workspace_size.y, res.x, res.y )
		
	local dy = h/sh -- self._res.y
	local dx = w/sw -- self._res.x
	local fh = 1280 * ( self._res.y/self._res.x )
	self._fullrect:set_screen( 1280, fh, 0, 0, self._res.x )
	self._screen_x = x*dx -- (1280 - 1198)/2  -- Calculates amount of pixels offset for matching saferect screen
	self._screen_y = y*dy -- (fh - 674)/2 -- Calculates amount of pixels offset for matching saferect screen
	print( "##############################################################" )
	print( "._fullrect", self._fullrect:width(), self._fullrect:height(), self._fullrect:width()/self._fullrect:height() )
	print( "self._screen_x, self._screen_y", self._screen_x, self._screen_y )
	print( "dx, dy", dx, dy )
					  
	local saferect_panel = self._saferect:panel()
	local fullrect_panel = self._fullrect:panel()
	
	self._saferect_panel = saferect_panel
	
	self._bg_rect = fullrect_panel:rect( { color = Color.black, layer = base_layer - 2 } )
	self._bg_gui = saferect_panel:bitmap( { texture = self._gui_data.bg_texture, layer = base_layer - 1 } )
			
	self._upper_frame_rect = fullrect_panel:rect( { visible = false, x = 0, y = 0, w = fullrect_panel:w(), h = 48, layer = base_layer, orientation = "vertical", color = Color.black } )
	self._lower_frame_rect = fullrect_panel:rect( { visible = false, x = 0, y = 0, w = fullrect_panel:w(), h = 48, layer = base_layer, orientation = "vertical", color = Color.black } )
	
	self._tips_head_line = saferect_panel:text( { text_id = "tip_tips", font = "fonts/font_small_shadow_mf", font_size = 14 * self._scale_tweak_data.small_font_multiplier, color = Color.white, align="left", layer = base_layer + 1 } )
	self._tips_text = saferect_panel:text( { text_id = arg.load_level_data.tip_id, font = "fonts/font_small_shadow_mf", font_size = 14 * self._scale_tweak_data.small_font_multiplier, color = Color.white, align="left", wrap=true, word_wrap=true, layer = base_layer + 1 } ) -- align_s="left" vertical_s="bottom" layer="1" wrap="false" word_wrap="false" w="512" h="16" />
	self._tips_text:set_text( string.upper( self._tips_text:text() ) )
		
	self._indicator = saferect_panel:bitmap( { name = "indicator", texture = "guis/textures/icon_loading", layer = base_layer + 2 } )
		
	self._level_title_text = saferect_panel:text( { y = 0, text_id = "debug_loading_level", font = "fonts/font_medium_shadow_mf", font_size = 32 * self._scale_tweak_data.default_font_multiplier, color = Color.white, align="left", halign="left", vertical="bottom", layer = base_layer + 1, h = 24 } )
	self._level_title_text:set_text( string.upper( (self._level_tweak_data.name and self._level_tweak_data.name or "") .. " > " .. self._level_title_text:text() ) )
			
	self._stonecold_small_logo = saferect_panel:bitmap( { name = "stonecold_small_logo", texture = self._gui_tweak_data.stonecold_small_logo, texture_rect = {0,0,256,56}, layer = base_layer + 1, h = 56, } )
	self._stonecold_small_logo:set_size( 256 * self._scale_tweak_data.menu_logo_multiplier, 56 * self._scale_tweak_data.menu_logo_multiplier )
			
	self._challenges_topic = saferect_panel:text( { text_id = "menu_near_completion_challenges", font = "fonts/font_medium_shadow_mf", font_size = self._menu_tweak_data.loading_challenge_name_font_size, color = Color.white, align="left", layer = base_layer + 1 } )
	self._challenges_topic:set_shape( self._challenges_topic:text_rect() )
	self._challenges = {}
	for i,challenge in ipairs( challenges ) do
		local panel = saferect_panel:panel( { layer = base_layer, w = 140 * self._scale_tweak_data.loading_challenge_bar_scale, h = 22 * self._scale_tweak_data.loading_challenge_bar_scale } )
		local bg_bar = panel:rect( { x = 0, y = 0, w = panel:w(), h = panel:h(), color = Color.black:with_alpha( 0.5 ), align="center", halign="center", vertical="center", layer = base_layer + 1 } )
		local bar = panel:gradient( { orientation = "vertical", gradient_points = { 0, Color( 1,255/255,168/255,0), 1, Color( 1,154/255,102/255,0) },
										x = 2*self._scale_tweak_data.loading_challenge_bar_scale, y = 2*self._scale_tweak_data.loading_challenge_bar_scale, w = (bg_bar:w() - 4* self._scale_tweak_data.loading_challenge_bar_scale) * (challenge.amount/challenge.count), h = bg_bar:h() - 4*self._scale_tweak_data.loading_challenge_bar_scale, layer = base_layer + 2, align="center", halign="center", vertical="center", } )
		local progress_text = panel:text( { font = self.font, font_size = self._menu_tweak_data.loading_challenge_progress_font_size, font = "fonts/font_medium_shadow_mf", x = 0, y = 0, h = bg_bar:h(), w = bg_bar:w(),
										align="center", halign="center", vertical="center", valign="center", color = Color.white, layer = base_layer + 3,
										text = challenge.amount.."/"..challenge.count,
									} )
		local text = saferect_panel:text( { text = string.upper( challenge.name ), font = "fonts/font_medium_shadow_mf", font_size = self._menu_tweak_data.loading_challenge_name_font_size, color = Color.white, align="left", layer = base_layer + 1 } )
		text:set_shape( text:text_rect() )
		table.insert( self._challenges, { panel = panel, text = text } )
	end
		
	self._dot_count = 0
	self._max_dot_count = 4
	self._init_progress = 0
	self._fake_progress = 0
	self._max_bar_width = 0]]

	-- self:setup( res, progress )
end

function LevelLoadingScreenGuiScript:setup( res, progress )
	self._saferect_bottom_y = self._saferect_panel:h() - self._gui_tweak_data.upper_saferect_border
	
	self._level_title_text:set_shape( 0, 0, self._safe_rect_pixels.width, self._gui_tweak_data.upper_saferect_border - self._gui_tweak_data.border_pad )
	local _,_,w,_ = self._level_title_text:text_rect()
	self._level_title_text:set_w( w )

	-- self._bg_gui:set_size( self._bg_gui:parent():w(), self._bg_gui:parent():w()/2 )
	self._bg_gui:set_size( self._bg_gui:parent():h() * (self._bg_gui:texture_width() / self._bg_gui:texture_height()), self._bg_gui:parent():h() )
	self._bg_gui:set_center( self._bg_gui:parent():center() )
		
	if self._briefing_text then
		if self._res and self._res.y <= 601 then
			self._briefing_text:set_w( self._briefing_text:parent():w() )
			local _,_,w,h = self._briefing_text:text_rect()
			self._briefing_text:set_size( w, h )
			self._briefing_text:set_lefttop( 0, self._briefing_text:parent():h()/2  )
		else
			self._briefing_text:set_w( self._briefing_text:parent():w() * 0.5 )
			local _,_,w,h = self._briefing_text:text_rect()
			self._briefing_text:set_size( w, h )
			self._briefing_text:set_rightbottom( self._briefing_text:parent():w(),self._saferect_bottom_y - self._gui_tweak_data.border_pad )
		end
	end
	
	local border_size = 2
	local bar_size = 2
	
	self._stonecold_small_logo:set_righttop( self._stonecold_small_logo:parent():righttop() )
	self._stonecold_small_logo:set_bottom( self._gui_tweak_data.upper_saferect_border - self._gui_tweak_data.border_pad )
	
	self._top_y = self._safe_rect_pixels.y + self._gui_tweak_data.upper_saferect_border
	self._bottom_y = self._safe_rect_pixels.y + self._saferect_panel:h() - self._gui_tweak_data.upper_saferect_border
	
	self._upper_frame_rect:set_h( self._screen_y + self._gui_tweak_data.upper_saferect_border )
	
	self._lower_frame_rect:set_h( self._upper_frame_rect:h() )
	self._lower_frame_rect:set_bottom( self._lower_frame_rect:parent():h() )
		
	local tip_top = self._gui_tweak_data.upper_saferect_border + self._gui_tweak_data.border_pad + 14
	local _,_,w,h = self._tips_head_line:text_rect()
	self._tips_head_line:set_size( w, h )
	self._tips_head_line:set_top( tip_top )
	
	local offset = 20
	self._tips_text:set_w( self._saferect_panel:w() - self._tips_head_line:w() - offset )
	
	self._tips_text:set_top( tip_top )
	self._tips_text:set_left( self._tips_head_line:right() + offset )

	if( progress > 0 ) then
		self._init_progress = progress
	end
	
	for i,challenge in ipairs( self._challenges ) do
		local h = challenge.panel:h()
		challenge.panel:set_bottom( self._saferect_bottom_y - ( h + 2 ) * ( #self._challenges-i) )
		challenge.text:set_left( challenge.panel:right() + 8*self._scale_tweak_data.loading_challenge_bar_scale )
		challenge.text:set_center_y( challenge.panel:center_y() )
	end
	self._challenges_topic:set_visible( self._challenges[1] and true or false )
	if self._challenges[1] then
		self._challenges_topic:set_bottom( self._challenges[1].panel:top() - 4 )
	end
	
	self._indicator:set_left( self._level_title_text:right() + 8 )
	self._indicator:set_bottom( self._gui_tweak_data.upper_saferect_border - self._gui_tweak_data.border_pad )
	
	
	
	
	if self._coords then
		self._controller:set_center( self._saferect_panel:w()/2, self._saferect_panel:h()/2 )
		
		for id,data in pairs( self._coords ) do
			local _,_,w,h = data.text:text_rect()
			data.text:set_size( w, h )
			if data.x then
				local x = self._controller:x() + data.x
				local y = self._controller:y() + data.y
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
end

function LevelLoadingScreenGuiScript:update( progress, t, dt )
	-- self._dot_count = ( self._dot_count + dt * 2 ) % self._max_dot_count
		
	self._indicator:rotate( 180 * dt )
	
	--[[if( self._init_progress < 100 ) then
		if( progress == -1 ) then	-- Progress only available on bundle, so faking it.
			self._fake_progress = self._fake_progress + ( 100 / 5 ) * dt

			if( self._fake_progress > 100 ) then
				self._fake_progress = 100
			end

			progress = self._fake_progress
		end
	end]]
	
end

function LevelLoadingScreenGuiScript:get_loading_text( dot_count )
	return self._init_text .. string.rep( ".", math.floor( dot_count ) )
end

function LevelLoadingScreenGuiScript:set_text( text )
	self._text_gui:set_text( text )
	self._init_text = text
end

function LevelLoadingScreenGuiScript:destroy()
	if( alive( self._saferect ) ) then
		self._scene_gui:destroy_workspace( self._saferect )
		self._saferect = nil
	end
	if( alive( self._fullrect ) ) then
		self._scene_gui:destroy_workspace( self._fullrect )
		self._fullrect = nil
	end
	
	if( alive( self._ws ) ) then
		self._scene_gui:destroy_workspace( self._ws )
		self._ws = nil
	end
	
	if self._back_drop_gui then
		self._back_drop_gui:destroy()
		self._back_drop_gui = nil
	end
end

function LevelLoadingScreenGuiScript:visible()
	return self._ws:visible()
end

function LevelLoadingScreenGuiScript:set_visible( visible )
	if( visible ) then
		self._ws:show()
	else
		self._ws:hide()
	end
end
