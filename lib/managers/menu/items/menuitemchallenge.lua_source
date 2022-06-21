core:import("CoreMenuItem")

MenuItemChallenge = MenuItemChallenge or class( CoreMenuItem.Item )
MenuItemChallenge.TYPE = "challenge"

function MenuItemChallenge:init( data_node, parameters )
	CoreMenuItem.Item.init( self, data_node, parameters )
		
	self._parameters.description = parameters.description_text
	self._parameters.challenge = parameters.challenge
	self._type = MenuItemChallenge.TYPE
end

-- GUI ----------------------------------------------------

function MenuItemChallenge:setup_gui( node, row_item )
	local safe_rect = managers.gui_data:scaled_size()
	
	local challenge_data = managers.challenges:challenge( self:parameter( "challenge" ) )
	local progress_data = managers.challenges:active_challenge( self:parameter( "challenge" ) )
	
	if not progress_data then
		progress_data = { amount = challenge_data.count }
	end
	
	local chl_color = self:parameter( "awarded" ) and tweak_data.menu.awarded_challenge_color or row_item.color
	row_item.gui_panel = node.item_panel:panel( { w = node.item_panel:w() } )
	row_item.challenge_name = node._text_item_part( node, row_item, row_item.gui_panel, node._right_align( node ) )
	row_item.challenge_name:set_layer( node.layers.items + 1 )
	row_item.challenge_name:set_font_size( tweak_data.menu.challenges_font_size )
	row_item.challenge_name:set_kern( tweak_data.scale.upgrade_menu_kern )
	row_item.challenge_name:set_color( chl_color )
	
	row_item.gui_info_panel = node.safe_rect_panel:panel( { visible = false, layer = node.layers.items, x = 0, y = 0, w = node._left_align( node ), h = node._item_panel_parent:h() } )
	row_item.description_text = row_item.gui_info_panel:text( { text = self:parameter( "description" ), font = tweak_data.menu.small_font, font_size = tweak_data.menu.small_font_size, color = row_item.color, align = "left", vertical = "top", wrap = true, word_wrap = true } )
	--[[if challenge_data.count and challenge_data.count > 1 then
		row_item.count_panel = row_item.gui_info_panel:panel( { visible = true, layer = node.layers.items, x = 0, y = 0, w = node._left_align( node ), h = node._item_panel_parent:h() } )
		row_item.bg_rect = row_item.count_panel:rect( { color = row_item.color, layer = node.layers.items, w = 100, h = 12, } )
		row_item.progress_rect = row_item.count_panel:rect( { color = Color( 0.9, 0.9, 0.9 ), layer = node.layers.items+1, w = 100, h = 10, } )
		row_item.count_text = row_item.count_panel:text( { text = progress_data.amount.."/"..challenge_data.count, layer = node.layers.items, font = node.font, font_size = node.font_size, color = row_item.color, align = "right", vertical = "center" } )
	end]]
	row_item.challenge_hl = row_item.gui_info_panel:text( { text = row_item.text, layer = node.layers.items, font = node.font, font_size = tweak_data.menu.challenges_font_size, color = row_item.color, align = "left", vertical = "left", wrap = true, word_wrap = true } )
	row_item.reward_panel = row_item.gui_info_panel:panel( { visible = true, layer = node.layers.items, x = 0, y = 0, w = node._left_align( node ), h = node._item_panel_parent:h() } )
	local text = managers.localization:text( "menu_reward_xp", { XP = managers.experience:cash_string( challenge_data.xp ) }  ) 
	row_item.reward_text = row_item.reward_panel:text( { text = text, layer = node.layers.items, font = node.font, font_size = tweak_data.menu.challenges_font_size, color = row_item.color, align = "left", vertical = "left" } )
	
	--row_item.color = chl_color
	
	local _,_,w,h = row_item.challenge_name:text_rect()
	row_item.gui_panel:set_h( h )
	-- local bar_w = 192
	local bar_w =  node._left_align( node ) - safe_rect.width/2
	if challenge_data.count and challenge_data.count > 1 then
		local bg_bar = row_item.gui_panel:rect( {	
									x = node._left_align( node ) - bar_w, y = h/2 - 11, w = bar_w, h = 22,
									align="center", halign="center", vertical="center",
									color = Color.black:with_alpha( 0.5 ), -- node.row_item_color,
									layer = node.layers.items - 1,
									visible = false,
								} )
		row_item.bg_bar = bg_bar
		
		-- orientation_s="vertical" gradient_points="{ 0, Color( 1,255/255,168/255,0), 1, Color( 1,154/255,102/255,0) }
		
		local bar = row_item.gui_panel:gradient( {	
									-- x = node._right_align( node ) + 2, y = h/2 - 2, w = 252, h = 4,
									orientation = "vertical",
									-- gradient_points = { 0, Color( 0.5,255/255,168/255,0), 1, Color( 0.5,154/255,102/255,0) },
									gradient_points = { 0, tweak_data.screen_color_blue:with_alpha(0.5), 1, tweak_data.screen_color_blue:with_alpha(0.5) },
									-- x = node._left_align( node ) - bar_w + 2, y = bg_bar:y() + 2, w = (bg_bar:w() - 4) * (progress_data.amount/challenge_data.count), h = bg_bar:h() - 4,
									x = node._left_align( node ) - bar_w + 2, y = bg_bar:y() + 2, w = (safe_rect.width-node._mid_align( node ) - 0 ) * (progress_data.amount/challenge_data.count), h = bg_bar:h() - 4,
									align="center", halign="center", vertical="center",
									color = node.color,
									layer = node.layers.items,
								} )
		row_item.bar = bar
		local progress_text = row_item.gui_panel:text( {
									font_size = tweak_data.menu.challenges_font_size,
									x = node._left_align( node ) - bar_w, y = 0, h = h, w = bg_bar:w(),
									align="right", halign="right", vertical="center", valign="center",
									font = node.font,
									color = node.color,
									layer = node.layers.items + 1,
									text = progress_data.amount.."/"..challenge_data.count, -- ..math.floor( params.data*100 ).."%",
									render_template = Idstring("VertexColorTextured")
								} )
		row_item.progress_text = progress_text
	end
	self:_layout( node, row_item )
	return true
end

function MenuItemChallenge:highlight_row_item( node, row_item, mouse_over )
	row_item.gui_info_panel:set_visible( true )
	row_item.challenge_name:set_color( row_item.color )
	row_item.challenge_name:set_font( (row_item.font and Idstring(row_item.font)) or tweak_data.menu.default_font_no_outline_id )
	if row_item.bar then
		row_item.progress_text:set_color( row_item.color )
		row_item.progress_text:set_font( (row_item.font and Idstring(row_item.font)) or tweak_data.menu.default_font_no_outline_id )
		-- row_item.bar:set_color( row_item.bar:color():with_alpha( 1 ) )
		-- row_item.bar:set_gradient_points( { 0, Color( 1,255/255,168/255,0), 1, Color( 1,154/255,102/255,0) } )
		row_item.bar:set_gradient_points( { 0, tweak_data.screen_color_blue:with_alpha(1), 1, tweak_data.screen_color_blue:with_alpha(1) } )
	end
	
	return true
end

function MenuItemChallenge:fade_row_item( node, row_item )
	local chl_color = self:parameter( "awarded" ) and tweak_data.menu.awarded_challenge_color or row_item.color
		
	row_item.gui_info_panel:set_visible( false )
	row_item.challenge_name:set_color( chl_color )
	row_item.challenge_name:set_font( (row_item.font and Idstring(row_item.font)) or tweak_data.menu.default_font_id )
	if row_item.bar then
		row_item.progress_text:set_color( chl_color )
		row_item.progress_text:set_font( (row_item.font and Idstring(row_item.font)) or tweak_data.menu.default_font_id )
		-- row_item.bar:set_color( row_item.bar:color():with_alpha( 0.5 ) )
		-- row_item.bar:set_gradient_points( { 0, Color( 0.5,255/255,168/255,0), 1, Color( 0.5,154/255,102/255,0) } )
		row_item.bar:set_gradient_points( { 0, tweak_data.screen_color_blue:with_alpha(0.5), 1, tweak_data.screen_color_blue:with_alpha(0.5) } )
	end
			
	return true
end

local xl_pad = 64
function MenuItemChallenge:_layout( node, row_item )
	local safe_rect = managers.gui_data:scaled_size() -- managers.viewport:get_safe_rect_pixels()
		
	row_item.gui_panel:set_width( safe_rect.width/2 + xl_pad )
	row_item.gui_panel:set_x( safe_rect.width/2 - xl_pad )
	
	local x,y,w,h = row_item.challenge_name:text_rect()
	row_item.challenge_name:set_height( h )
	row_item.challenge_name:set_left( node._right_align( node ) - row_item.gui_panel:x() )
	row_item.gui_panel:set_height( h )
	
	local sh = math.min( h, 22 )
		
	if row_item.bg_bar then
		row_item.bg_bar:set_x( xl_pad )
		row_item.bg_bar:set_h( sh )
		row_item.bg_bar:set_center_y( row_item.gui_panel:h()/2 )
		row_item.bar:set_h( sh - 4 )
		-- row_item.bar:set_x( row_item.bg_bar:x() + 2 )
		
		row_item.bar:set_h( row_item.gui_panel:h() - 1 )
		row_item.bar:set_left( node._mid_align( node ) - row_item.gui_panel:x() + 0 )
		row_item.bar:set_y( 1 )
		
		-- row_item.bar:set_y( row_item.bg_bar:y() + 2 )
		-- row_item.progress_text:set_center( row_item.bg_bar:center() )
		
		row_item.progress_text:set_right( row_item.gui_panel:w() - node._align_line_padding )
	end
	
	node._align_item_gui_info_panel( node, row_item.gui_info_panel )
	-- row_item.gui_panel:set_left( node._right_align( node ) )
	
	-- row_item.gui_panel:set_width( safe_rect.width/2 )
	
	
	-- INFO PANEL, called :_setup_challenge_size() before, in MenuNodeGui
	
	
	-- row_item.gui_info_panel:set_shape( 0, 0, self:_left_align() - 192, self._item_panel_parent:h() )
	-- row_item.gui_info_panel:set_shape( 0, 0, self._item_panel_parent:w()/2, self._item_panel_parent:h() )
	node._align_item_gui_info_panel( node, row_item.gui_info_panel )
			
	--[[if row_item.count_panel then
		local challenge_data = managers.challenges:challenge( row_item.item:parameter( "challenge" ) )
		local progress_data = managers.challenges:active_challenge( row_item.item:parameter( "challenge" ) )
	
		row_item.count_panel:set_w( row_item.gui_info_panel:w() )
		local _,_,w,h = row_item.count_text:text_rect()
		row_item.count_panel:set_h( h )
		row_item.count_panel:set_bottom( row_item.gui_info_panel:h()*(7/8) )
		row_item.count_panel:set_w( row_item.gui_info_panel:w()*(4/5) )
		row_item.count_panel:set_center_x( row_item.gui_info_panel:center_x() )
		
		row_item.count_text:set_size( 120, h )
		row_item.count_text:set_right( row_item.count_panel:w() )
		
		row_item.bg_rect:set_w( row_item.count_panel:w() - row_item.count_text:w() )
		row_item.bg_rect:set_center_y( row_item.count_text:center_y() )
		
		row_item.progress_rect:set_w( row_item.bg_rect:w() * (progress_data.amount/challenge_data.count) )
		
		row_item.progress_rect:set_center_y( row_item.bg_rect:center_y() )
	end]]
	
	row_item.challenge_hl:set_w( row_item.gui_info_panel:w() )	
	local _,_,w,h = row_item.challenge_hl:text_rect()
	row_item.challenge_hl:set_h( h )
	row_item.challenge_hl:set_x( 0 )
	row_item.challenge_hl:set_y( 0 )
		
	local _,_,w,h = row_item.reward_text:text_rect()
	row_item.reward_panel:set_h( h )
	row_item.reward_panel:set_w( row_item.gui_info_panel:w() )
	row_item.reward_text:set_size( w, h )
	row_item.reward_panel:set_x( 0 )
	row_item.reward_panel:set_top( row_item.challenge_hl:bottom() + tweak_data.menu.info_padding )
	-- row_item.reward_panel:set_y( 0 )
	
	-- row_item.description_text:set_w( row_item.gui_info_panel:w()*(4/5) )
	row_item.description_text:set_w( row_item.gui_info_panel:w() )
	local _,_,w,h = row_item.description_text:text_rect()
	row_item.description_text:set_h( h ) -- + self.font_size is a sollution to a bug where the last line might be outside
	row_item.description_text:set_x( 0 )
	row_item.description_text:set_top( row_item.reward_panel:bottom() + tweak_data.menu.info_padding )
	
end
