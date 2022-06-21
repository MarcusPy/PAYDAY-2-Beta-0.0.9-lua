HUDTeammate = HUDTeammate or class()

function HUDTeammate:init( i, teammates_panel, is_player, width )
	self._id = i
	local small_gap = 8
	local gap = 0
	local pad = 4
	
	local main_player = i == HUDManager.PLAYER_PANEL
	self._main_player = main_player

	local names = { "WWWWWWWWWWWWQWWW", "AI Teammate", "FutureCatCar", "WWWWWWWWWWWWQWWW" }

	-- local teammate_panel = teammates_panel:panel( { name =""..i, w = math.round(pw), x = math.floor((pw + small_gap) * (i-1) + (i == 4 and gap or 0)) } )
	local teammate_panel = teammates_panel:panel( { visible = false, name =""..i, w = math.round(width), x = 0 } )
	if not main_player then
		teammate_panel:set_h( 84 )
		teammate_panel:set_bottom( teammates_panel:h() )
	end
	self._player_panel = teammate_panel:panel( { name ="player" } ) -- A panel containing player only components
	-- self._sides = BoxGuiObject:new( self._player_panel, { sides = { 1, 1, 1, 1 } } )
	-- teammate_panel:set_debug( true )
		
	local name = teammate_panel:text( { name = "name", text=" "..utf8.to_upper(names[i]), layer = 1, color = Color.white, y = 0, vertical = "bottom", font_size = tweak_data.hud_players.name_size, font = tweak_data.hud_players.name_font } )
	local _,_,name_w,_ = name:text_rect()
	managers.hud:make_fine_text( name )
	-- name:set_h( name:h() - 2 ) -- Warning, packing the string height 
	-- name:set_leftbottom( 0, teammate_panel:h() + 1 )
	name:set_leftbottom( name:h(), teammate_panel:h() - (64 + 6) )
	
	if not main_player then
		name:set_x( 48 + name:h() + 4 )
		name:set_bottom( teammate_panel:h() - (24 + 6) )
	end
	
	local tabs_texture = "guis/textures/pd2/hud_tabs"
	-- local bg_rect = { 0, 45, 67, 19 }
	local bg_rect = { 84, 0, 44, 32 }
	local cs_rect = { 84, 34, 19, 19 }
	local csbg_rect = { 105, 34, 19, 19 }
	
	local bg_color = Color.white/3
	
	-- teammate_panel:rect( { name = "name_bg", visible = true, layer = 0, color = bg_color, x = name:x(), y = name:y() - 1 , w = name_w + 4, h = name:h() - 1 } )
	teammate_panel:bitmap( { name = "name_bg", texture = tabs_texture, texture_rect = bg_rect, visible = true, layer = 0, color = bg_color, x = name:x(), y = name:y() -1, w = name_w + 4, h = name:h() } )
	-- teammate_panel:rect( { name = "callsign", visible = true, color = tweak_data.chat_colors[i]:with_alpha( 0.68 ), blend_mode = "normal",  x = name_w + 4, y = name:y() - 1, w = teammate_panel:w() - name_w - 4, h = name:h() - 1 } )
	-- teammate_panel:rect( { name = "callsign", visible = true, color = tweak_data.chat_colors[i]:with_alpha( 0.68 ), blend_mode = "normal",  x = 0, y = name:y() - 1, w = name:h() - 1, h = name:h() - 1 } )
	teammate_panel:bitmap( { name = "callsign_bg", texture = tabs_texture, texture_rect = csbg_rect, layer = 0, color = bg_color, blend_mode = "normal", x = name:x() - name:h(), y = name:y()+1, w = name:h() - 2, h = name:h() - 2 } )
	teammate_panel:bitmap( { name = "callsign", texture = tabs_texture, texture_rect = cs_rect, layer = 1, color = tweak_data.chat_colors[i]:with_alpha( 1 ), blend_mode = "normal",  x = name:x() - name:h(), y = name:y()+1, w = name:h() - 2, h = name:h() - 2 } )
		
	local box_ai_bg = teammate_panel:bitmap( { visible = false, name = "box_ai_bg", texture = "guis/textures/pd2/box_ai_bg", color = Color.white, alpha = 0, y = 0, w = teammate_panel:w() } )
	box_ai_bg:set_bottom( name:top() )
	local box_bg = teammate_panel:bitmap( { visible = false, name = "box_bg", texture = "guis/textures/pd2/box_bg", color = Color.white, y = 0, w = teammate_panel:w() } )
	box_bg:set_bottom( name:top() ) 
		
	-- local data = tweak_data.hud_icons.mask_clown1
	local texture, rect = tweak_data.hud_icons:get_icon_data( "pd2_mask_"..i )
	-- local y = teammate_panel:h()/2 -- - rect[4]/2
	local size = 64 -- rect[3]*1.25
	local mask_pad = 2
	local mask_pad_x = 3
	local y = teammate_panel:h() - name:h() - size + mask_pad
	local mask = teammate_panel:bitmap( { visible = false, name = "mask", layer = 1, color = Color.white, texture = texture, texture_rect = rect, x = -mask_pad_x, w = size, h = size, y = y } )
	
	--[[
		local v2 = true
		local weapon = teammate_panel:bitmap( { name = "weapon", visible = false and not v2, layer = 1, color = Color.white, w = 48, h = 64, x = mask:right() - 10, y = y } )
		-- weapon:set_debug( true )
		weapon:set_bottom( mask:bottom() + 11 - 3 ) -- 19 ) -- 19 is from height of health_bg + offset from bottom
		local center_y_line = math.round( weapon:center_y() )
			
		local weapon_panel1 = teammate_panel:panel( { name = "weapon_panel1", visible = true and not v2, layer = 3, w = 64, h = 64, x = mask:right() + pad, y = y } )
		weapon_panel1:set_center( weapon:center() )
		-- weapon_panel1:set_debug( true )
		-- weapon_panel1:bitmap( { halign = "scale", valign = "scale", name = "bg", texture = "guis/textures/pd2/weapons", texture_rect = { 64, 0, 64, 64 }, blend_mode = "normal" } )
		weapon_panel1:bitmap( { halign = "scale", valign = "scale", name = "weapon", texture = "guis/textures/pd2/weapons", texture_rect = { 0, 64, 64, 64 }, layer = 1 } )
			
		local weapon_panel2 = teammate_panel:panel( { name = "weapon_panel2", visible = true and not v2, layer = 3, w = 64, h = 64, x = mask:right() + pad, y = y } )
		weapon_panel2:set_center( weapon:center() )
		-- weapon_panel2:set_position( weapon_panel2:x() + 8, weapon_panel2:y() - 8 )
		-- weapon_panel2:set_debug( true )
		-- weapon_panel2:bitmap( { halign = "scale", valign = "scale", name = "bg", texture = "guis/textures/pd2/weapons", texture_rect = { 64, 0, 64, 64 }, blend_mode = "normal" } )
		weapon_panel2:bitmap( { halign = "scale", valign = "scale", name = "weapon", color = Color.white, alpha = 0, texture = "guis/textures/pd2/weapons", texture_rect = { 0, 0, 64, 64 }, layer = 1 } ) -- , texture = "guis/textures/pd2/weapons", texture_rect = { 64, 0, 64, 64 } } )
				
		local ammo_pad = self._player_panel:bitmap( { name = "ammo_pad", visible = true and not v2, layer = 2, texture = "guis/textures/ammocounter", x = weapon:right() - 11, y = weapon_panel1:y() } )
		ammo_pad:set_center_y( weapon:center_y() )
		-- ammo_pad:set_x( math.round( ammo_pad:x() ) )
		-- ammo_pad:set_y( math.round( ammo_pad:y() ) )
		local ammo_total = self._player_panel:text( { name = "ammo_total", visible = true and not v2, text=""..math.random( 150 ), color = Color.black, blend_mode= "normal", layer = 3, w = 44, h = 64, x = mask:right() + pad, y = y, vertical = "center", align = "right", font_size = 29, font = tweak_data.hud_players.ammo_font } )
		ammo_total:set_center( ammo_pad:center() )
		ammo_total:set_y( math.round( ammo_total:y() + 1 ) )
			
		local ammo_clip = self._player_panel:rect( { name = "ammo_clip", visible = true and not v2, layer = 3, color = Color.black:with_alpha( 1 ), w = 4, h = 21, x = ammo_pad:x() + 14, y = ammo_pad:y() + 22 } )
		ammo_clip:set_bottom( ammo_pad:y() + 43 )		
	
		local ammo_pad_secondary = teammate_panel:bitmap( { name = "ammo_pad_secondary", color = Color( 0.75, 0.5, 0.5, 0.5 ), visible = is_player and not v2, layer = 0, texture = "guis/textures/ammocounter", w = 52, h = 52, x = weapon:right() - 8, y = weapon_panel1:y() } )
		ammo_pad_secondary:set_center_x( ammo_pad:right() + 3 )
		ammo_pad_secondary:set_center_y( ammo_pad:center_y() )
	
		local ammo_total_secondary = teammate_panel:text( { name = "ammo_total_secondary", text="999", color = Color.black:with_alpha( 0.75 ), visible = is_player and not v2, blend_mode= "normal", layer = 1, w = 38, h = 52, x = mask:right() + pad, y = y, vertical = "center", align = "right", font_size = 20, font = tweak_data.hud_players.ammo_font } )
		ammo_total_secondary:set_center( ammo_pad_secondary:center() ) -- , ammo_total_secondary:center_y() )
		ammo_total_secondary:set_y( math.round( ammo_total_secondary:y() ) )
			
		-- local ammo_total = teammate_panel:rect( { name = "ammo_total", visible = true, layer = 1, color = Color.black:with_alpha( 0.0 ), w = 48, h = 48, x = weapon:x(), y = weapon:y() } )
		-- ammo_total:set_bottom( mask:bottom() )
		-- local ammo_clip = teammate_panel:text( { name = "ammo_clip", text="Q", color = Color.white, layer = 5, w = 48, h = 48, x = mask:right() + pad, y = y, vertical = "bottom", align = "right", font_size = 24, font = tweak_data.hud_players.name_font } )
		-- ammo_clip:set_bottom( mask:bottom() )
		
	]]
									
	-- local health_bg = teammate_panel:rect( { name = "health_bg", layer = 1, color = Color( 0.5, 0.5, 0.5 ), w = teammate_panel:w() - weapon:left() , h = 12, x = weapon:left(), y = mask:y() } )
	-- local health = teammate_panel:rect( { name = "health", layer = 2, color = Color.green, w = (health_bg:w()-2)/2, h =  health_bg:h()-2, x = health_bg:x() + 1, y = health_bg:y() + 1 } )
	-- local armor = teammate_panel:rect( { name = "armor", layer = 3, color = Color.white, w = (health_bg:w()-2)/2, h =  health_bg:h()-2, x = (health_bg:x())+health_bg:w()/2, y = health_bg:y() + 1 } )
	
	--[[
	local bitmap_w = 236
	-- 2, 18, 232, 11
	local health_panel = self._player_panel:panel( { visible = false, name = "health_panel", layer = 1, w = teammate_panel:w() - weapon:left(), x = weapon:left(), y = mask:y() } )
	local health_bg = health_panel:bitmap( { name = "health_bg", texture = "guis/textures/pd2/healthshield", texture_rect = { 0, 0, bitmap_w, 16 }, color = Color( 1, 1, 1 ), w = (health_panel:w()) - weapon:w(), x = weapon:w() } )
	health_panel:set_height( health_bg:h() )
	-- health_bg:set_bottom( mask:bottom() - 4 )
	health_panel:set_top( mask:top() + 2 )
	health_bg:set_h( health_bg:h() - 4 )
	health_bg:set_y( 4 )
	-- local health = teammate_panel:bitmap( { name = "health", texture = "guis/textures/pd2/healthshield", texture_rect = { 2, 18, bitmap_w/2, 11 }, layer = 2, color = Color( 102/256, 204/256, 51/256 ), w = (health_bg:w()-4)/2, x = health_bg:x() + 2, y = health_bg:y() + 2 } )
	local health = health_panel:bitmap( { name = "health", texture = "guis/textures/pd2/healthshield", texture_rect = { 0, 16, bitmap_w, 16 }, layer = 2, color = Color.white, w = (health_panel:w()) - weapon:w(), x = weapon:w(), y = 4, h = health_panel:h() - 4, layer = 2 } )
	-- local armor = teammate_panel:bitmap( { name = "armor", texture = "guis/textures/pd2/healthshield", texture_rect = { 2 + bitmap_w/2, 18, bitmap_w/2, 11 }, layer = 3, color = Color( 34/256, 155/256, 205/256 ), w = (health_bg:w()-4)/2, x = (health_bg:x())+health_bg:w()/2, y = health_bg:y() + 2 } )
	
	local revives_left = health_panel:text( { name = "revives_left", visible = true, text= "", layer = 2, color = Color.white, w = weapon:w() - 4, x = 0, y = 4, h = health_panel:h() - 4, vertical = "center", align = "right", font_size = tweak_data.hud.small_font_size, font = tweak_data.hud.small_font } )
	
	
	local armor = health_panel:bitmap( { name = "armor", texture = "guis/textures/pd2/healthshield", texture_rect = { 0, 32, bitmap_w, 16 }, color = Color.white, w = (health_panel:w()), y = 0, h = 4, layer = 1 } )
	
	-- self._healtharmor_dividers = {}
	
	local healtharmor_divider
	
	for i=1, tweak_data.player.damage.ARMOR_STEPS do
		healtharmor_divider = health_panel:bitmap( { name = "healtharmor_divider_"..i, texture = "guis/textures/pd2/healthshield_divider", layer = 4, color = Color( 1, 1, 1 ), x = armor:left(), y = armor:y(), h = armor:h() } )
		healtharmor_divider:set_center_x( (i / tweak_data.player.damage.ARMOR_STEPS) * health_panel:w() )
	end
	
	-- local healtharmor_divider = health_panel:bitmap( { name = "healtharmor_divider", texture = "guis/textures/pd2/healthshield_divider", layer = 4, color = Color( 1, 1, 1 ), x = armor:left(), y = armor:y() } )
	]]
	
	local radial_size = main_player and 64 or 48
	
		
	----- RADIAL HEALTH
	local radial_health_panel = self._player_panel:panel( { name = "radial_health_panel", layer = 1, w = radial_size + 4, h = radial_size + 4, x = 0, y = mask:y() } )
	radial_health_panel:set_bottom( self._player_panel:h() )
	-- radial_health_panel:set_debug( true )
	
	local radial_bg = radial_health_panel:bitmap( { name = "radial_bg", texture = "guis/textures/pd2/hud_radialbg", alpha = 1, w = radial_health_panel:w(), h = radial_health_panel:h(), layer = 0 } )
	
	local radial_health = radial_health_panel:bitmap( { name = "radial_health", texture = "guis/textures/pd2/hud_health", texture_rect = { 64, 0, -64, 64 }, render_template = "VertexColorTexturedRadial", blend_mode = "add", alpha = 1, w = radial_health_panel:w(), h = radial_health_panel:h(), layer = 2 } )
	radial_health:set_color( Color( 1, 1, 0, 0 ) )
	
	local radial_shield = radial_health_panel:bitmap( { name = "radial_shield", texture = "guis/textures/pd2/hud_shield", texture_rect = { 64, 0, -64, 64 }, render_template = "VertexColorTexturedRadial", blend_mode = "add", alpha = 1, w = radial_health_panel:w(), h = radial_health_panel:h(), layer = 1 } )
	radial_shield:set_color( Color( 1, 1, 0, 0 ) )
	
	local damage_indicator = radial_health_panel:bitmap( { name = "damage_indicator", texture = "guis/textures/pd2/hud_radial_rim", blend_mode = "add", alpha = 0, w = radial_health_panel:w(), h = radial_health_panel:h(), layer = 1 } )
	damage_indicator:set_color( Color( 1, 1, 1, 1 ) )
	
	local x,y,w,h = radial_health_panel:shape()
	teammate_panel:bitmap( { name = "condition_icon", layer = 4, visible = false, color = Color.white, x = x, y = y, w = w, h = h } )
	local condition_timer = teammate_panel:text( { name = "condition_timer", visible = false, text="000", layer = 5, color = Color.white, y = 0, align = "center", vertical = "center", font_size = tweak_data.hud_players.timer_size, font = tweak_data.hud_players.timer_font } )
	condition_timer:set_shape( radial_health_panel:shape() )
	
	----- Weapon panels
	local w_selection_w = 13
	local weapon_panel_w = 80
	local extra_clip_w = 4
	local ammo_text_w = (weapon_panel_w - w_selection_w)/2
	
	local font_bottom_align_correction = 3
	
	local tabs_texture = "guis/textures/pd2/hud_tabs"
	local bg_rect = { 0, 0, 67, 32 }
	local weapon_selection_rect1 = { 67, 0, 13, 32 }
	local weapon_selection_rect2 = { 67, 32, 13, 32 }
	
	local weapons_panel = self._player_panel:panel( { name = "weapons_panel", visible = true, layer = 0, w = weapon_panel_w, h = radial_health_panel:h(), x = radial_health_panel:right() + 4, y = radial_health_panel:y() } )

	local primary_weapon_panel = weapons_panel:panel( { name = "primary_weapon_panel", visible = false, layer = 1, w = weapon_panel_w, h = 32, x = 0, y = 0 } )
	primary_weapon_panel:bitmap( { name = "bg", texture = tabs_texture, texture_rect = bg_rect, visible = true, layer = 0, color = bg_color, w = weapon_panel_w, x = 0 } )
	primary_weapon_panel:text( { name = "ammo_clip", visible = main_player and true, text="0"..math.random( 40 ), color = Color.white, blend_mode= "normal", layer = 1, w = ammo_text_w + extra_clip_w, h = primary_weapon_panel:h(), x = 0, y = 0 + font_bottom_align_correction, vertical = "bottom", align = "center", font_size = 32, font = tweak_data.hud_players.ammo_font } )

	primary_weapon_panel:text( { name = "ammo_total", visible = true, text="000", color = Color.white, blend_mode= "normal", layer = 1, w = ammo_text_w - extra_clip_w, h = primary_weapon_panel:h(), x = ammo_text_w + extra_clip_w, y = 0 + font_bottom_align_correction, vertical = "bottom", align = "center", font_size = 24, font = tweak_data.hud_players.ammo_font } )
	primary_weapon_panel:bitmap( { name = "weapon_selection", texture = tabs_texture, texture_rect = weapon_selection_rect1, visible = main_player and true, layer = 1, color = Color.white, w = w_selection_w, x = weapon_panel_w - w_selection_w } )
	
	if not main_player then
		local ammo_total = primary_weapon_panel:child( "ammo_total" )
		-- ammo_total:set_debug( true )
		local _x,_y,_w,_h = ammo_total:text_rect()
		primary_weapon_panel:set_size( _w + 8, _h )
		ammo_total:set_shape( 0, 0, primary_weapon_panel:size() )
		ammo_total:move( 0, font_bottom_align_correction )
		
		primary_weapon_panel:set_x( 0 )
		primary_weapon_panel:set_bottom( weapons_panel:h() )
		local eq_rect = { 84, 0, 44, 32 }
		primary_weapon_panel:child( "bg" ):set_image( tabs_texture, eq_rect[1], eq_rect[2], eq_rect[3], eq_rect[4] )
		primary_weapon_panel:child( "bg" ):set_size( primary_weapon_panel:size() )
	end
	
	local secondary_weapon_panel = weapons_panel:panel( { name = "secondary_weapon_panel", visible = false, layer = 1, w = weapon_panel_w, h = 32, x = 0, y = primary_weapon_panel:bottom() } )
	secondary_weapon_panel:bitmap( { name = "bg", texture = tabs_texture, texture_rect = bg_rect, visible = true, layer = 0, color = bg_color, w = weapon_panel_w, x = 0 } )
	secondary_weapon_panel:text( { name = "ammo_clip", visible = main_player and true, text=""..math.random( 40 ), color = Color.white, blend_mode= "normal", layer = 1, w = ammo_text_w + extra_clip_w, h = secondary_weapon_panel:h(), x = 0, y = 0 + font_bottom_align_correction, vertical = "bottom", align = "center", font_size = 32, font = tweak_data.hud_players.ammo_font } )
	secondary_weapon_panel:text( { name = "ammo_total", visible = true, text="000", color = Color.white, blend_mode= "normal", layer = 1, w = ammo_text_w - extra_clip_w, h = secondary_weapon_panel:h(), x = ammo_text_w + extra_clip_w, y = 0 + font_bottom_align_correction, vertical = "bottom", align = "center", font_size = 24, font = tweak_data.hud_players.ammo_font } )
	secondary_weapon_panel:bitmap( { name = "weapon_selection", texture = tabs_texture, texture_rect = weapon_selection_rect2, visible = main_player and true, layer = 1, color = Color.white, w = w_selection_w, x = weapon_panel_w - w_selection_w } )
	secondary_weapon_panel:set_bottom( weapons_panel:h() )
	
	if not main_player then
		local ammo_total = secondary_weapon_panel:child( "ammo_total" )
		-- ammo_total:set_debug( true )
		local _x,_y,_w,_h = ammo_total:text_rect()
		secondary_weapon_panel:set_size( _w + 8, _h )
		ammo_total:set_shape( 0, 0, secondary_weapon_panel:size() )
		ammo_total:move( 0, font_bottom_align_correction )
		
		secondary_weapon_panel:set_x( primary_weapon_panel:right() )
		secondary_weapon_panel:set_bottom( weapons_panel:h() )
		local eq_rect = { 84, 0, 44, 32 }
		secondary_weapon_panel:child( "bg" ):set_image( tabs_texture, eq_rect[1], eq_rect[2], eq_rect[3], eq_rect[4] )
		secondary_weapon_panel:child( "bg" ):set_size( secondary_weapon_panel:size() )
	end
	
	----- Deployable equipment
	local eq_rect = { 84, 0, 44, 32 }
	local temp_scale = 1
	
	local deployable_equipment_panel = self._player_panel:panel( { name = "deployable_equipment_panel", layer = 1, w = 48, h = 32, x = weapons_panel:right() + 4, y = weapons_panel:y() } )
	-- deployable_equipment_panel:rect( { name = "bg", visible = true, layer = 0, color = bg_color, w = deployable_equipment_panel:w(), x = 0 } )
	deployable_equipment_panel:bitmap( { name = "bg", texture = tabs_texture, texture_rect = eq_rect, visible = true, layer = 0, color = bg_color, w = deployable_equipment_panel:w(), x = 0 } )
	local equipment = deployable_equipment_panel:bitmap( { name = "equipment", visible = false, layer = 1, color = Color.white, w = deployable_equipment_panel:h() * temp_scale, h = deployable_equipment_panel:h() * temp_scale, x = -(deployable_equipment_panel:h()*temp_scale - deployable_equipment_panel:h())/2, y = -(deployable_equipment_panel:h()*temp_scale - deployable_equipment_panel:h())/2 } )
	local amount = deployable_equipment_panel:text( { name = "amount", visible = false, text = tostring(12), font = "fonts/font_medium_mf", font_size = 22, color = Color.white, align = "right", vertical = "center", layer = 2, x = -2, y = 2, w = deployable_equipment_panel:w(), h = deployable_equipment_panel:h() } )
	-- deployable_equipment_panel:set_debug( true )
	if not main_player then
		local scale = 0.75
		-- deployable_equipment_panel:set_debug( true )
		deployable_equipment_panel:set_size( deployable_equipment_panel:w() * 0.9, deployable_equipment_panel:h() * scale )
		equipment:set_size( equipment:w() * scale, equipment:h() * scale )
		equipment:set_center_y( deployable_equipment_panel:h()/2 )
		equipment:set_x( equipment:x() + 4 )
		amount:set_center_y( deployable_equipment_panel:h()/2 )
		amount:set_right( deployable_equipment_panel:w() - 4 )
		deployable_equipment_panel:set_x( weapons_panel:right() - 8 )
		deployable_equipment_panel:set_bottom( weapons_panel:bottom() )
		local bg = deployable_equipment_panel:child( "bg" )
		bg:set_size( deployable_equipment_panel:size() )
	end
	
	----- Cable ties
	local texture, rect = tweak_data.hud_icons:get_icon_data( tweak_data.equipments.specials.cable_tie.icon )
	local cable_ties_panel = self._player_panel:panel( { name = "cable_ties_panel", visible = true, layer = 1, w = 48, h = 32, x = weapons_panel:right() + 4, y = weapons_panel:y() } )
	cable_ties_panel:bitmap( { name = "bg", texture = tabs_texture, texture_rect = eq_rect, visible = true, layer = 0, color = bg_color, w = deployable_equipment_panel:w(), x = 0 } )
	local cable_ties = cable_ties_panel:bitmap( { name = "cable_ties", visible = false, texture = texture, texture_rect = rect, layer = 1, color = Color.white, w = deployable_equipment_panel:h() * temp_scale, h = deployable_equipment_panel:h() * temp_scale, x = -(deployable_equipment_panel:h()*temp_scale - deployable_equipment_panel:h())/2, y = -(deployable_equipment_panel:h()*temp_scale - deployable_equipment_panel:h())/2 } )
	local amount = cable_ties_panel:text( { name = "amount", visible = false, text = tostring(12), font = "fonts/font_medium_mf", font_size = 22, color = Color.white, align = "right", vertical = "center", layer = 2, x = -2, y = 2, w = deployable_equipment_panel:w(), h = deployable_equipment_panel:h() } )
	cable_ties_panel:set_bottom( weapons_panel:bottom() )
	if not main_player then
		local scale = 0.75
		--- cable_ties_panel:set_debug( true )
		cable_ties_panel:set_size( cable_ties_panel:w() * 0.9, cable_ties_panel:h() * scale )
		cable_ties:set_size( cable_ties:w() * scale, cable_ties:h() * scale )
		cable_ties:set_center_y( cable_ties_panel:h()/2 )
		cable_ties:set_x( cable_ties:x() + 4 )
		amount:set_center_y( cable_ties_panel:h()/2 )
		amount:set_right( cable_ties_panel:w() - 4 )
		cable_ties_panel:set_x( deployable_equipment_panel:right() )
		cable_ties_panel:set_bottom( deployable_equipment_panel:bottom() )
		local bg = cable_ties_panel:child( "bg" )
		bg:set_size( cable_ties_panel:size() )
	end
	
	----- CARRY
	local bag_rect = { 32, 33, 32, 31 }
	local bg_rect = { 84, 0, 44, 32 }
	
	local bag_w = bag_rect[3] -- * 1.5
	local bag_h = bag_rect[4] -- * 1.5
	
	-- local carry_panel = self._player_panel:panel( { name = "carry_panel", visible = false, layer = 1, w = self._player_panel:w(), h = bag_rect[ 4 ] + 2, x = name:x(), y = name:top() - 4 - (bag_rect[ 4 ] + 2) } )
	local carry_panel = self._player_panel:panel( { name = "carry_panel", visible = false, layer = 1, w = bag_w, h = bag_h + 2, x = 0, y = radial_health_panel:top() - (bag_h) } )
	carry_panel:set_x( 24 - (bag_w)/2 )
	carry_panel:set_center_x( radial_health_panel:center_x() )
	carry_panel:bitmap( { name = "bg", texture = tabs_texture, texture_rect = bg_rect, visible = false, layer = 0, color = bg_color, x = 0, y = 0 , w = 100, h = carry_panel:h() } )
	carry_panel:bitmap( { name = "bag", texture = tabs_texture, w = bag_w, h = bag_h, texture_rect = bag_rect, visible = true, layer = 0, color = Color.white, x = 1, y = 1 } )
	carry_panel:text( { name = "value", visible = false, text= "", layer = 0, color = Color.white, x = bag_rect[ 3 ] + 4, y = 0, vertical = "center", font_size = tweak_data.hud.small_font_size, font = "fonts/font_small_mf" } )
	
	-- Interact
	local interact_panel = self._player_panel:panel( { name = "interact_panel", visible = false, layer = 3,  } )
	interact_panel:set_shape( weapons_panel:shape() )
	interact_panel:set_shape( radial_health_panel:shape() )
	interact_panel:set_size( radial_size*1.25, radial_size*1.25 )
	interact_panel:set_center( radial_health_panel:center() )
	local radius = interact_panel:h()/2 - 4
	self._interact = CircleBitmapGuiObject:new( interact_panel, { use_bg = true, rotation = 360, radius = radius, blend_mode = "add", color = Color.white, layer = 0 } )
	self._interact:set_position( 4, 4 )
	-- self._interact:set_current( 0.5 )
	
	self._special_equipment = {}
	self._panel = teammate_panel
	
	-- self:_rec_round_object( self._panel )
end

function HUDTeammate:_rec_round_object( object )
	if object.children then
		for i, d in ipairs( object:children() ) do
			self:_rec_round_object( d )
		end
	end
	
	local x, y = object:position()
	object:set_position( math.round(x), math.round(y) )
end

function HUDTeammate:panel()
	return self._panel
end

-- Shows the panel
function HUDTeammate:add_panel()
	local teammate_panel = self._panel
	 
	teammate_panel:set_visible( true )
	-- self._hud.teammate_panels_data[ id ].taken = true
end

-- Hides the panel
function HUDTeammate:remove_panel()
	local teammate_panel = self._panel
	 
	teammate_panel:set_visible( false )
	-- self._hud.teammate_panels_data[ id ].taken = false
	local special_equipment = self._special_equipment
	while special_equipment[ 1 ] do
		teammate_panel:remove( table.remove( special_equipment ) )
	end
	self:set_condition( "mugshot_normal" )
			
	self._player_panel:child( "weapons_panel" ):child( "secondary_weapon_panel" ):set_visible( false )
	self._player_panel:child( "weapons_panel" ):child( "primary_weapon_panel" ):set_visible( false )
		
	local deployable_equipment_panel = self._player_panel:child( "deployable_equipment_panel" )
	deployable_equipment_panel:child( "equipment" ):set_visible( false )
	deployable_equipment_panel:child( "amount" ):set_visible( false )
	
	local cable_ties_panel = self._player_panel:child( "cable_ties_panel" )
	cable_ties_panel:child( "cable_ties" ):set_visible( false )
	cable_ties_panel:child( "amount" ):set_visible( false )
		
	self._player_panel:child( "carry_panel" ):set_visible( false )
	self._player_panel:child( "carry_panel" ):child( "value" ):set_text( "" )
		
	self._peer_id = nil
	self._ai = nil
end

function HUDTeammate:set_peer_id( peer_id )
	self._peer_id = peer_id
end

function HUDTeammate:set_ai( ai )
	self._ai = ai
end

function HUDTeammate:_set_weapon_selected( id, hud_icon )
	-- print( "_set_weapon_selected", id, hud_icon )
	-- Application:stack_dump()
	local is_secondary = id == 1
	local secondary_weapon_panel = self._player_panel:child( "weapons_panel" ):child( "secondary_weapon_panel" )
	local primary_weapon_panel = self._player_panel:child( "weapons_panel" ):child( "primary_weapon_panel" )
	
	primary_weapon_panel:set_alpha( is_secondary and 0.5 or 1 )
	secondary_weapon_panel:set_alpha( is_secondary and 1 or 0.5 )
end

function HUDTeammate:set_weapon_selected( id, hud_icon )
	self:_set_weapon_selected( id, hud_icon )
	
	--[[
	local teammate_panel = self._panel
	
	if id == 3 then -- Makes switch to 3 weapon work for now
		if self._current_weapon_id then
			id = (self._current_weapon_id == 2 and 1) or 2
		else
			id = 2
		end
	end
	self._current_weapon_id = id
	--if id < 3 then
		local source_id = (id == 2 and 1) or 2
		local ammo_total_secondary = teammate_panel:child( "ammo_total_secondary" )
		local ammo_pad_secondary = teammate_panel:child( "ammo_pad_secondary" )
		
		local target = teammate_panel:child( "weapon_panel"..id )
		local source = teammate_panel:child( "weapon_panel"..source_id )
		local align_x_obj = teammate_panel:child( "weapon" ) -- :center_x()
		local align_y_obj = teammate_panel:child( "weapon" ) -- :center_y()
		local ammo_total = self._player_panel:child( "ammo_total" )
		-- print( ammo_total:font(), ammo_total:font_size(), ammo_total:text(), ammo_total:x(), ammo_total:y(), ammo_total:w(), ammo_total:h() )
		local ammo_total_clone = managers.hud:text_clone( ammo_total )
		
		-- Sets color range since it cannot be cloned
		local zero = (tonumber(ammo_total_clone:text()) < 10 and "00") or (tonumber(ammo_total_clone:text()) < 100 and "0") or ""
		ammo_total_clone:set_text( zero..tostring( tonumber(ammo_total_clone:text()) ) )
		ammo_total_clone:set_range_color(0, string.len( zero ), Color( 0.4, 0.4, 0.4 ) )
		
		local ammo_total_secondary_clone = managers.hud:text_clone( ammo_total_secondary )
		
		target:animate( callback( self, self, "_animate_weapon_switch" ), source, ammo_total, ammo_total_clone, ammo_total_secondary, ammo_total_secondary_clone, ammo_pad_secondary, align_x_obj, align_y_obj )
		
		-- source_id
		if managers.hud._hud.weapons[source_id] then -- WARNING, Accessing hud in wrong way to access weapons
			self:update_second_weapon_ammo_info( managers.hud._hud.weapons[source_id].unit )
		end
				
		local icon, texture_rect = tweak_data.hud_icons:get_icon_data( hud_icon )
		target:child( "weapon" ):set_image( icon, unpack( texture_rect ) )
	-- end
	
	]]
	
	--[[local teammate_panel = self._teammate_panels[ 4 ]
	for i = 1, 2 do
		local weapon_panel = teammate_panel:child( "weapon_panel"..i )
		weapon_panel:child( "weapon" ):set_visible( id == i )
	end]]
	
	-- self:set_weapon_ammo_by_unit( data.unit )
	--[[for i,data in pairs( self._hud.weapons ) do
		data.bitmap:set_visible( id == i )
		
		-- data.bitmap:set_color( data.bitmap:color():with_alpha( id == i and 1.0 or 0.4 ) )
		--data.amount:set_visible( id ~= i )
		-- self:set_weapon_ammo_by_unit( data.unit )
		-- if id == i then
		-- 	self:_set_hud_ammo( data.unit )
		--end
	end]]
end

--[[function HUDTeammate:update_second_weapon_ammo_info( unit )
	local teammate_panel = self._panel
	local ammo_total_secondary = teammate_panel:child( "ammo_total_secondary" )
	local ammo_pad_secondary = teammate_panel:child( "ammo_pad_secondary" )
	
	local max_clip,current_clip,ammo_total,ammo_max = unit:base():ammo_info()
					
	local zero = (ammo_total < 10 and "00") or (ammo_total < 100 and "0") or ""
	ammo_total_secondary:set_text( zero..tostring( ammo_total ) )
	-- ammo_total_secondary:set_range_color( 0, string.len( zero ), Color.black ) -- ( 0.4, 0.4, 0.4 ) )
	
	local reload = current_clip <= math.round( max_clip/4 )
	local low_ammo = ammo_total <= math.round( max_clip/2 )
	local out_of_ammo = ammo_total <= 0
	
	local color = out_of_ammo and Color( 0.75, 0.9, 0.3, 0.3 )
	color = color or low_ammo and Color( 0.75, 0.9, 0.9, 0.3 )
	-- color = color or reload and Color( 0.75,0.9, 0.6, 0.3 ) -- Color.white
	color = color or Color( 0.75, 0.5, 0.5, 0.5 )
	
	ammo_pad_secondary:set_color( color )
end]]

function HUDTeammate:set_ammo_amount_by_type( type, max_clip, current_clip, current_left, max )
	local weapon_panel = self._player_panel:child( "weapons_panel" ):child( type.."_weapon_panel" )
	weapon_panel:set_visible( true )
	
	-- local reload = current_clip <= math.round( max_clip/4 )
	local low_ammo = current_left <= math.round( max_clip/2 )
	local low_ammo_clip = current_clip <= math.round( max_clip/4 )
	local out_of_ammo_clip = current_clip <= 0
	local out_of_ammo = current_left <= 0
		
	local color_total = out_of_ammo and Color( 1, 0.9, 0.3, 0.3)
	color_total = color_total or low_ammo and Color( 1, 0.9, 0.9, 0.3)
	-- color = color or reload and Color( 1, 0.9, 0.6, 0.3) -- Color.white
	color_total = color_total or Color.white
	
	local color_clip = out_of_ammo_clip and Color( 1, 0.9, 0.3, 0.3)
	color_clip = color_clip or low_ammo_clip and Color( 1, 0.9, 0.9, 0.3)
	-- color = color or reload and Color( 1, 0.9, 0.6, 0.3) -- Color.white
	color_clip = color_clip or Color.white
	
	local ammo_clip = weapon_panel:child( "ammo_clip" )
	local zero = (current_clip < 10 and "00") or (current_clip < 100 and "0") or ""
	ammo_clip:set_text( zero..tostring( current_clip ) )
	ammo_clip:set_color( color_clip )
	ammo_clip:set_range_color(0, string.len( zero ), color_clip:with_alpha( 0.5 ) )-- Color( 0.4, 0.4, 0.4 ) )
	
	local ammo_total = weapon_panel:child( "ammo_total" )
	local zero = (current_left < 10 and "00") or (current_left < 100 and "0") or ""
	ammo_total:set_text( zero..tostring( current_left ) )
	ammo_total:set_color( color_total )
	ammo_total:set_range_color(0, string.len( zero ), color_total:with_alpha( 0.5 ) )-- Color( 0.4, 0.4, 0.4 ) )
end

--[[
function HUDTeammate:set_ammo_amount( max_clip, current_clip, current_left, max )
	local teammate_panel = self._panel:child( "player" )
	
	local ammo_total = teammate_panel:child( "ammo_total" )
	local zero = (current_left < 10 and "00") or (current_left < 100 and "0") or ""
	ammo_total:set_text( zero..tostring( current_left ) )
	ammo_total:set_range_color(0, string.len( zero ), Color( 0.4, 0.4, 0.4 ) )
			
	local ammo_clip = teammate_panel:child( "ammo_clip" )
	local bottom = ammo_clip:bottom()
	ammo_clip:set_h( 21 * (current_clip/max_clip) )
	-- local ammo_pad = teammate_panel:child( "ammo_pad" )
	ammo_clip:set_bottom( bottom )
	
	-- local ammo_clip = teammate_panel:child( "ammo_clip" )
	-- ammo_clip:set_text( tostring( current_clip ) )
	
		local ammo_pad = teammate_panel:child( "ammo_pad" )
		
		local reload = current_clip <= math.round( max_clip/4 )
		local low_ammo = current_left <= math.round( max_clip/2 )
		local out_of_ammo = current_left <= 0
		
		local color = out_of_ammo and Color( 1, 0.9, 0.3, 0.3)
		color = color or low_ammo and Color( 1, 0.9, 0.9, 0.3)
		color = color or reload and Color( 1, 0.9, 0.6, 0.3) -- Color.white
		color = color or Color.white
		
		-- ammo_total:set_color( Color.black ) -- :with_alpha( 1 ) )
		-- ammo_pad:set_color( color )
	
	-- teammate_panel:child( "ammo_pad" ):stop()	
	-- teammate_panel:child( "ammo_pad" ):animate( callback( self, self, "_animate_weapon_fired" ) )
end]]

function HUDTeammate:set_health( data )
	local teammate_panel = self._panel:child( "player" )
	
	--[[local bitmap_w = 236
	local health_panel = teammate_panel:child("health_panel")
	local health_bg = health_panel:child( "health_bg" )
	local health = health_panel:child( "health" )
	local revives_left = health_panel:child( "revives_left" )
	health:set_w( (health_panel:w() - health:x()) * (data.current/data.total) )
	-- health_panel:set_debug(false)
	local armor = teammate_panel:child("health_panel"):child( "armor" )
	-- armor:set_left( health:right() )
	-- armor:set_texture_rect( unpack( { 2 + (bitmap_w/2) * (data.current/data.total), 18, bitmap_w/2, 11 } ) )
	-- local healtharmor_divider = teammate_panel:child("health_panel"):child( "healtharmor_divider" )
	-- healtharmor_divider:set_left( armor:left() )
	
	-- data.current = data.current + 2
	-- self:set_armor( data )
	
	local revives = data.revives or 0
	local text = "  "
	if( revives > 1 ) then
		text = "x" .. tostring(revives-1)
	end
	revives_left:set_text( text )
	
	local rect = { 0, 16, bitmap_w * (data.current/data.total), 16 }
	health:set_texture_rect( unpack( rect ) )]]
	
	
	local radial_health_panel = teammate_panel:child( "radial_health_panel" )
	local radial_health = radial_health_panel:child( "radial_health" )
	
	local red = (data.current/data.total)
	if red < radial_health:color().red then
		-- print( "DAMAGE TAKEN ON HEALTH", red, radial_health:color().red )
		self:_damage_taken()
	end
	
	
	radial_health:set_color( Color( 1, red, 1, 1 ) )
end

function HUDTeammate:set_armor( data )
	local teammate_panel = self._panel:child( "player" )
	
	--[[local bitmap_w = 236
	local health_panel = teammate_panel:child("health_panel")
	local armor = health_panel:child( "armor" )
	local health_bg = health_panel:child( "health_bg" )
	armor:set_w( (health_panel:w() - armor:x()) * (data.current/data.total) )
	
	local start_x = health_bg:x()
	
	-- local rect_x_mod = (armor:x()-start_x) / ((health_bg:w() - 4)/2)
	-- print( (armor:x()-start_x) / ((health_bg:w() - 4)/2) )
	-- print( "x", (armor:x()-start_x) )
	-- local rect_x_mod = 1 - ((armor:x()-start_x) - (bitmap_w/2)) / ((health_bg:w() - 4)/2)
	-- print( "rect_x_mod", rect_x_mod )
	-- armor:set_texture_rect( unpack( { 2 + (bitmap_w/2) * rect_x_mod, 18, bitmap_w/2 * (data.current/data.total), 11 } ) )
	local rect = { 0, 32, bitmap_w * (data.current/data.max), 16 }
	-- print( inspect( rect ) )
	armor:set_texture_rect( unpack( rect ) )]]
	
	local radial_health_panel = teammate_panel:child( "radial_health_panel" )
	local radial_shield = radial_health_panel:child( "radial_shield" )
	
	local red = (data.current/data.total)
	if red < radial_shield:color().red then
		-- print( "DAMAGE TAKEN ON ARMOR", red, radial_shield:color().red )
		self:_damage_taken()
	end
	radial_shield:set_color( Color( 1, red, 1, 1 ) )
end

function HUDTeammate:_damage_taken()
	local teammate_panel = self._panel:child( "player" )
	local radial_health_panel = teammate_panel:child( "radial_health_panel" )
	local damage_indicator = radial_health_panel:child( "damage_indicator" )
	
	damage_indicator:stop()
	damage_indicator:animate( callback( self, self, "_animate_damage_taken" ) )
end

function HUDTeammate:_animate_damage_taken( damage_indicator )
	damage_indicator:set_alpha( 1 )
	local st = 3
	local t = st
	local st_red_t = 0.5
	local red_t = st_red_t
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		red_t = math.clamp( red_t - dt, 0, 1 )
		-- damage_indicator:set_color( Color( 1, 1-red_t, 1-red_t ) ) 
		-- damage_indicator:set_color( Color( 1, 0, 0 ) )
		damage_indicator:set_color( Color( 1, red_t/st_red_t, red_t/st_red_t ) )
		-- damage_indicator:set_alpha( math.clamp( t/st + red_t, 0, 1 ) )
		damage_indicator:set_alpha( t/st )
	end
	damage_indicator:set_alpha( 0 )
end

function HUDTeammate:set_name( teammate_name )
	local teammate_panel = self._panel
	
	local name = teammate_panel:child( "name" )
	local name_bg = teammate_panel:child( "name_bg" )
	local callsign = teammate_panel:child( "callsign" )
	name:set_text( utf8.to_upper( " "..teammate_name ) )
	local h = name:h() -- Height has been manipulated before and should not be changed by make_fine_text
	managers.hud:make_fine_text( name )
	name:set_h( h )
	name_bg:set_w( name:w() + 4 )
	-- callsign:set_x( name_bg:right() )
	-- callsign:set_w( teammate_panel:w() - name_bg:right()  )
end

function HUDTeammate:set_callsign( id ) -- Id is between 1 and 4, should be peer id when player
	local teammate_panel = self._panel
	
	print( "id", id )
	Application:stack_dump()
	
	local callsign = teammate_panel:child( "callsign" )
	local alpha = callsign:color().a 
	callsign:set_color( tweak_data.chat_colors[id]:with_alpha( alpha ) )
end

function HUDTeammate:set_cable_tie( data )
	local teammate_panel = self._panel:child( "player" )
		
	local icon, texture_rect = tweak_data.hud_icons:get_icon_data( data.icon )
	local cable_ties_panel = self._player_panel:child( "cable_ties_panel" )
	local cable_ties2 = cable_ties_panel:child( "cable_ties" )
	cable_ties2:set_image( icon, unpack( texture_rect ) )
	cable_ties2:set_visible( true )
		
	self:set_cable_ties_amount( data.amount )
	
	return nil
end

function HUDTeammate:set_cable_ties_amount( amount )
	local visible = amount ~= 0
	local cable_ties_panel = self._player_panel:child( "cable_ties_panel" )
	local cable_ties_amount = cable_ties_panel:child( "amount" )
	cable_ties_amount:set_visible( visible )
	cable_ties_amount:set_text( amount == -1 and "--" or tostring( amount ) )
	local cable_ties = cable_ties_panel:child( "cable_ties" )
	cable_ties:set_visible( visible )
end

-- local player_components = {
							 -- "health_bg", "health", "armor", "healtharmor_divider",
--							} --"ammo_pad", "ammo_total", "ammo_clip" }
function HUDTeammate:set_state( state ) -- state, ai, player
	local teammate_panel = self._panel
	
	local is_player = state == "player"
	
	--[[for _,name in ipairs( player_components ) do
		local child = teammate_panel:child( name )
		if child then
			local bitmap_visible = not child.texture_name or child:texture_name() ~= Idstring( "" )
			child:set_visible( is_player and bitmap_visible )
		end
	end]]
	
	teammate_panel:child( "player" ):set_alpha( is_player and 1 or 0 )
	
	local name = teammate_panel:child( "name" )
	local name_bg = teammate_panel:child( "name_bg" )
	local callsign_bg = teammate_panel:child( "callsign_bg" )
	local callsign = teammate_panel:child( "callsign" )
	
	
	--[[teammate_panel:bitmap( { name = "name_bg", texture = tabs_texture, texture_rect = bg_rect, visible = true, layer = 0, color = bg_color, x = name:x(), y = name:y() - 1 , w = name_w + 4, h = name:h() - 1 } )
	teammate_panel:bitmap( { name = "callsign_bg", texture = tabs_texture, texture_rect = csbg_rect, layer = 0, color = bg_color, blend_mode = "normal", x = name:x() - name:h(), y = name:y() - 1, w = name:h() - 1, h = name:h() - 1 } )
	teammate_panel:bitmap( { name = "callsign", texture = tabs_texture, texture_rect = cs_rect, layer = 1, color = tweak_data.chat_colors[i]:with_alpha( 1 ), blend_mode = "normal",  x = name:x() - name:h(), y = name:y() - 1, w = name:h() - 1, h = name:h() - 1 } )
	]]
	
	if not self._main_player then
		if is_player then
			name:set_x( 48 + name:h() + 4 )
			name:set_bottom( teammate_panel:h() - (24 + 6) )
		else
			name:set_x( 48 + name:h() + 4 )
			name:set_bottom( teammate_panel:h() )
		end
		name_bg:set_position( name:x(), name:y() - 1 )
		callsign_bg:set_position( name:x() - name:h(), name:y() + 1 )
		callsign:set_position( name:x() - name:h(), name:y() + 1 )
	end
	
	--[[local target_weapon_y = teammate_panel:child( "weapon" ):y() -- 54 -- is_player and 38 or 54
	
	teammate_panel:child( "weapon" ):stop()
	teammate_panel:child( "weapon" ):animate( callback( self, self, "_animate_state_switch" ), teammate_panel:child( "player" ), teammate_panel:child( "weapon_panel1" ), teammate_panel:child( "weapon_panel2" ), target_weapon_y
														,is_player, teammate_panel:child( "box_ai_bg" ), teammate_panel:child( "box_bg" )  )]]
end

-----------------------------------------------------------------

function HUDTeammate:set_perk_equipment( data )
	
end

function HUDTeammate:set_deployable_equipment( data )
	local icon, texture_rect = tweak_data.hud_icons:get_icon_data( data.icon )
	
	local deployable_equipment_panel = self._player_panel:child( "deployable_equipment_panel" )
	local equipment = deployable_equipment_panel:child( "equipment" )
	equipment:set_visible( true )
	equipment:set_image( icon, unpack( texture_rect ) )
	
	self:set_deployable_equipment_amount( 1, data )
end

function HUDTeammate:set_deployable_equipment_amount( index, data )
	local teammate_panel = self._panel:child( "player" )
		
	local deployable_equipment_panel = self._player_panel:child( "deployable_equipment_panel" )
	local amount = deployable_equipment_panel:child( "amount" )
	deployable_equipment_panel:child( "equipment" ):set_visible( data.amount ~= 0 )
	amount:set_text( tostring( data.amount ) )
	amount:set_visible( data.amount ~= 0 )
end

function HUDTeammate:set_carry_info( carry_id, value )
	local carry_panel = self._player_panel:child( "carry_panel" )
	carry_panel:set_visible( true )
	
	local value_text = carry_panel:child( "value" )
	value_text:set_text( managers.experience:cash_string( value ) )
	
	local _,_,w,_ = value_text:text_rect()
	local bg = carry_panel:child( "bg" )
	bg:set_w( carry_panel:child( "bag" ):w() + w + 4 )
end

function HUDTeammate:remove_carry_info()
	local carry_panel = self._player_panel:child( "carry_panel" )
	carry_panel:set_visible( false )
end

-----------------------------------------------------------------

function HUDTeammate:add_special_equipment( data )
	-- print( "HUDTeammate:add_special_equipment", inspect( data ) )
		
	local teammate_panel = self._panel
	-- local special_equipments = self._hud.teammate_panels_data[ i ].special_equipments
	local special_equipment = self._special_equipment
	local id = data.id
	
	local equipment_panel = teammate_panel:panel( { name = id, layer = 0, y = 0 } )
	
	local icon, texture_rect = tweak_data.hud_icons:get_icon_data( data.icon )
	equipment_panel:set_size( 32, 32 ) -- texture_rect[3]*0.75, texture_rect[4]*0.75 )
	local bitmap = equipment_panel:bitmap( { name = "bitmap", texture = icon, color = Color.white, layer = 1, texture_rect = texture_rect, w = equipment_panel:w(), h = equipment_panel:w() } )
	
	local amount, amount_bg
	if data.amount then
		amount = equipment_panel:child( "amount" ) or equipment_panel:text( { name = "amount", text = tostring(data.amount), font = "fonts/font_small_noshadow_mf", font_size = 12, color = Color.black, align = "center", vertical = "center", layer = 4, w = equipment_panel:w(), h = equipment_panel:h() } )
--		amount = equipment_panel:text( { name = "amount", text = tostring(data.amount), font = tweak_data.hud.medium_font, font_size = tweak_data.hud.equipment_font_size, color = Color.white, align = "right", vertical = "bottom", layer = 4, w = bitmap:w(), h = bitmap:h() } )
		amount:set_visible( data.amount > 1 )
		
		amount_bg = equipment_panel:child( "amount_bg" ) or equipment_panel:bitmap( { name = "amount_bg", texture = "guis/textures/pd2/equip_count", color = Color.white, layer = 3 } )
		amount_bg:set_visible( data.amount > 1 )
		-- amount_bg:set_center( bitmap:center() )
		-- amount_bg:move( 11, 11 )
	end
			
	local flash_icon = equipment_panel:bitmap( { name = "bitmap", texture = icon, color = tweak_data.hud.prime_color, layer = 2, texture_rect = texture_rect, w = equipment_panel:w() + 2, h = equipment_panel:w() + 2 } )  
	
	table.insert( special_equipment, equipment_panel )
			
	-- equipment_panel:set_x( (bitmap:w() + 0) * (#special_equipment - 1) )
	local w = teammate_panel:w()
	equipment_panel:set_x( w - (equipment_panel:w() + 0) * (#special_equipment) )
	-- bitmap:set_bottom( (self._hud.special_equipments[ #self._hud.special_equipments - 1 ] and self._hud.special_equipments[ #self._hud.special_equipments - 1 ].bitmap:top() or sy ) )
	
	if amount then
		amount_bg:set_center( bitmap:center() )
		amount_bg:move( 7, 7 )
	
		-- amount:set_center( bitmap:center() )
		amount:set_center( amount_bg:center() )
	end
	
	local hud = managers.hud:script( PlayerBase.PLAYER_INFO_HUD_PD2 )
	flash_icon:set_center( bitmap:center() )
	flash_icon:animate( hud.flash_icon, nil, equipment_panel )
	
	self:layout_special_equipments()
end

function HUDTeammate:remove_special_equipment( equipment )
	local teammate_panel = self._panel
	local special_equipment = self._special_equipment
	
	for i,panel in ipairs( special_equipment ) do
		if panel:name() == equipment then
			local data = table.remove( special_equipment, i )
			teammate_panel:remove( panel )
			--[[if alive( data.flash_icon ) then
				teammate_panel:remove( data.flash_icon )
			end
			if data.amount then
				teammate_panel:remove( data.amount )
			end]]
			self:layout_special_equipments()
			return
		end
	end
end

function HUDTeammate:set_special_equipment_amount( equipment_id, amount )
	local teammate_panel = self._panel
	local special_equipment = self._special_equipment
	
	for i,panel in ipairs( special_equipment ) do
		if panel:name() == equipment_id then
			panel:child( "amount" ):set_text( tostring( amount ) )
			panel:child( "amount" ):set_visible( amount > 1 )
			panel:child( "amount_bg" ):set_visible( amount > 1 )
			return
		end
	end
end

function HUDTeammate:clear_special_equipment()
	-- CLEAR ALL HERE, FOR END SIMULATION
	self:remove_panel()
	self:add_panel()
	--[[local teammate_panel = self._panel
	local special_equipment = self._special_equipment
	
	while special_equipment[ 1 ] do
		teammate_panel:remove( table.remove( special_equipment ) )
	end]]
end

function HUDTeammate:layout_special_equipments()
	local teammate_panel = self._panel
	local special_equipment = self._special_equipment
	local name = teammate_panel:child( "name" )
		
	local w = teammate_panel:w()
	for i,panel in ipairs( special_equipment ) do
		if self._main_player then
			-- local j = 1 + math.mod( i-1, 3 )
			-- local y = i > 3 and 0 or panel:h()
			-- local x_offset = i > 3 and panel:w()/2 or 0
			-- panel:set_x( w - (panel:w() + 0) * (j) - x_offset )
			-- panel:set_y( y )
			panel:set_x( w - (panel:w() + 0) * (i) )
			panel:set_y( 0 )
		else
			panel:set_x( 48 + (panel:w() * (i-1)) )
			panel:set_y( 0 )
		end
	end
end

------------------------------------------------------------------------

function HUDTeammate:set_condition( icon_data, text )
	local condition_icon = self._panel:child( "condition_icon" )
	if icon_data == "mugshot_normal" then
		condition_icon:set_visible( false )
	else
		condition_icon:set_visible( true )
		local icon, texture_rect = tweak_data.hud_icons:get_icon_data( icon_data)
		condition_icon:set_image( icon, texture_rect[1], texture_rect[2], texture_rect[3], texture_rect[4] )
	end
end

------------------------------------------------------------------------

function HUDTeammate:teammate_progress( enabled, tweak_data_id, timer, success  )
	if true then
		-- return
	end
	-- self._player_panel:child( "primary_weapon_panel" )
	-- self._player_panel:child( "secondary_weapon_panel" )
	-- self._player_panel:child( "weapons_panel" ):set_visible( not enabled )

	self._player_panel:child( "radial_health_panel" ):set_alpha( enabled and 0.2 or 1 )
	-- self._player_panel:child( "weapons_panel" ):set_alpha( enabled and 0.2 or 1 )
	self._player_panel:child( "interact_panel" ):stop()
	self._player_panel:child( "interact_panel" ):set_visible( enabled )
	-- self._player_panel:child( "interact_panel" ):set_position( self._player_panel:child( "radial_health_panel" ):position() )
	-- self._interact:set_size( 100, 100 )
	if enabled then
		self._player_panel:child( "interact_panel" ):animate( callback( HUDManager, HUDManager, "_animate_label_interact" ), self._interact, timer )
	elseif success then
		local panel = self._player_panel
					
		local bitmap = panel:bitmap( { rotation = 360, texture = "guis/textures/pd2/hud_progress_active", blend_mode = "add", align = "center", valign = "center", --[[texture_rect = { 1024 - 128, 1024 - 128, 128, 128 },]] layer = 2 } )
		bitmap:set_size( self._interact:size() )
		bitmap:set_position( self._player_panel:child( "interact_panel" ):x() + 4, self._player_panel:child( "interact_panel" ):y() + 4 ) -- self._interact_circle:position() )
							
		local radius = self._interact:radius()
		local circle = CircleBitmapGuiObject:new( panel, { rotation = 360, radius = radius, color = Color.white:with_alpha( 1 ), blend_mode = "normal", layer = 3 } )
		circle:set_position( bitmap:position() )		
		
		bitmap:animate( callback( HUDInteraction, HUDInteraction, "_animate_interaction_complete" ), circle )
	end
end

------------------------------------------------------------------------

function HUDTeammate:start_timer( time )
	self._timer_paused = 0
	self._timer = time
	self._panel:child( "condition_timer" ):set_font_size( tweak_data.hud_players.timer_size )
	self._panel:child( "condition_timer" ):set_color( Color.white )
	self._panel:child( "condition_timer" ):stop()
	self._panel:child( "condition_timer" ):set_visible( true )
	self._panel:child( "condition_timer" ):animate( callback( self, self, "_animate_timer" ) )
end

function HUDTeammate:set_pause_timer( pause )
	if not self._timer_paused then
		return
	end
	self._timer_paused = self._timer_paused + (pause and 1 or -1) -- 0 means no to pause 
end

function HUDTeammate:stop_timer()
	if not alive( self._panel ) then
		return
	end
	self._panel:child( "condition_timer" ):set_visible( false )
	self._panel:child( "condition_timer" ):stop()
end

function HUDTeammate:is_timer_running()
	return self._panel:child( "condition_timer" ):visible()
end

function HUDTeammate:_animate_timer()
	local rounded_timer = math.round( self._timer )
	while self._timer >= 0 do
		local dt = coroutine.yield()
		if self._timer_paused == 0 then
			self._timer = self._timer - dt
			local text = self._timer  < 0 and "00" or (math.round( self._timer  ) < 10 and "0" or "")..math.round( self._timer  )
			self._panel:child( "condition_timer" ):set_text( text )
			
			if rounded_timer > math.round( self._timer ) then
				rounded_timer = math.round( self._timer )
				if rounded_timer < 11 then
					self._panel:child( "condition_timer" ):animate( callback( self, self, "_animate_timer_flash" ) )
				end
			end
		end
	end
end

function HUDTeammate:_animate_timer_flash()
	-- print( "HUDAssaultCorner:flash_point_of_no_return_timer", beep )
	local t = 0
	
	local condition_timer = self._panel:child( "condition_timer" )
	
	while t < 0.5 do
		t = t + coroutine.yield()
		
		local n = 1 - math.sin( t * 180 )
		local r = math.lerp( 1 or self._point_of_no_return_color.r, 1, n )
		local g = math.lerp( 0 or self._point_of_no_return_color.g, 0.8, n )
		local b = math.lerp( 0 or self._point_of_no_return_color.b, 0.2, n )
		
		-- print( t, n )
		
		condition_timer:set_color( Color( r, g, b ) )
		condition_timer:set_font_size( math.lerp( tweak_data.hud_players.timer_size, tweak_data.hud_players.timer_flash_size, n ) )
	end
	
	-- condition_timer:set_color( Color.white )
	condition_timer:set_font_size( 30 )
end

------------------------------------------------------------------------

--[[
function HUDTeammate:_animate_weapon_switch( target, source, ammo_total, ammo_total_clone, ammo_total_secondary, ammo_total_secondary_clone, ammo_pad_secondary, CENTER_X_OBJ, CENTER_Y_OBJ )
	local TOTAL_T = 0.4
	local t = TOTAL_T
	local MAX_DIST = 20
	local PAD_SECONDARY_MAX_DIST = 30
	local MAX_SIZE = 64
	ammo_total:set_alpha( 0 )
	ammo_total_secondary:set_alpha( 0 )
	-- ammo_total:set_color( ammo_total:color():with_alpha( 0 ) )
	-- ammo_total_secondary:set_color( ammo_total_secondary:color():with_alpha( 0 ) )
	local ammo_pad_secondary_right = ammo_pad_secondary:right()
	-- local ammo_pad_secondary_hide = ammo_pad_secondary_right - 10
	local ammo_total_secondary_right = ammo_total_secondary:right()
	-- CENTER_X = math.round( CENTER_X )
	-- CENTER_Y = math.round( CENTER_Y )
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		target:set_center_y(  CENTER_Y_OBJ:center_y() + MAX_DIST * -math.sin( (180/TOTAL_T) * t ) )
		source:set_center_y(  CENTER_Y_OBJ:center_y() + MAX_DIST * math.sin( (180/TOTAL_T) * t ) )
		local secondary_offset = PAD_SECONDARY_MAX_DIST * -math.sin( (180/TOTAL_T) * t )
		ammo_pad_secondary:set_right( ammo_pad_secondary_right + secondary_offset )
		ammo_total_secondary:set_right( ammo_total_secondary_right + secondary_offset )
		ammo_total_secondary_clone:set_right( ammo_total_secondary_right + secondary_offset )
		if t < (TOTAL_T/2) then -- After half the time, start fading and scaling away the source (old weapon)
			local a = t/(TOTAL_T/2) -- 1 -> 0
			-- source:child( "weapon" ):set_color( source:child( "weapon" ):color():with_alpha( a ) )
			source:child( "weapon" ):set_alpha( a ) 
			local size = (MAX_SIZE/2) * (1-a)
			source:set_size( MAX_SIZE - size, MAX_SIZE - size )
			-- ammo_total:set_color( ammo_total:color():with_alpha( 1-a ) )
			ammo_total:set_alpha( 1-a )
			-- ammo_total_secondary:set_color( ammo_total_secondary:color():with_alpha( (1-a)*0.75 ) )
			-- ammo_total:set_color( ammo_total:color():with_alpha( 1 ) )
			-- ammo_total_clone:set_color( ammo_total_clone:color():with_alpha( 0 ) )
			
			-- ammo_total_secondary:set_color( ammo_total_secondary:color():with_alpha( (1)*0.75 ) )
			ammo_total_secondary:set_alpha( (1)*0.75 )
			-- ammo_total_secondary_clone:set_color( ammo_total_secondary_clone:color():with_alpha( 0 ) )
			ammo_total_secondary_clone:set_alpha( 0 )
		else  -- First half the time, start fading and scaling up the target (new weapon)
			local a = (TOTAL_T - t)/(TOTAL_T/2) -- 0 -> 1
			-- target:child( "weapon" ):set_color( target:child( "weapon" ):color():with_alpha( a ) )
			target:child( "weapon" ):set_alpha( a )
			local size = (MAX_SIZE/2) * a
			target:set_size( (MAX_SIZE/2) + size, (MAX_SIZE/2) + size )
			
			-- ammo_total_clone:set_color( ammo_total_clone:color():with_alpha( 1-a ) )
			ammo_total_clone:set_alpha( 1-a )
			-- ammo_total_secondary_clone:set_color( ammo_total_secondary_clone:color():with_alpha( (1-a)*0.75 ) )
		end
		target:set_center_x( CENTER_X_OBJ:center_x() )
		source:set_center_x( CENTER_X_OBJ:center_x() )
	end
	target:set_size( MAX_SIZE, MAX_SIZE )
	source:set_size( MAX_SIZE/2, MAX_SIZE/2 )
	target:set_center_y( CENTER_Y_OBJ:center_y() )
	source:set_center_y( CENTER_Y_OBJ:center_y() )
	target:set_center_x( CENTER_X_OBJ:center_x() )
	source:set_center_x( CENTER_X_OBJ:center_x() )
	-- target:child( "weapon" ):set_color( target:child( "weapon" ):color():with_alpha( 1 ) )
	target:child( "weapon" ):set_alpha( 1 )
	-- source:child( "weapon" ):set_color( source:child( "weapon" ):color():with_alpha( 0 ) )
	source:child( "weapon" ):set_alpha( 0 )
		
	ammo_pad_secondary:set_right( ammo_pad_secondary_right )
	ammo_total_secondary:set_right( ammo_total_secondary_right )
	ammo_total_secondary_clone:set_right( ammo_total_secondary_right )
	
	ammo_total_clone:parent():remove( ammo_total_clone )
	ammo_total_secondary_clone:parent():remove( ammo_total_secondary_clone )
	-- print( "done", "CENTER_Y", CENTER_Y, "CENTER_X", CENTER_X )
end]]

--[[function HUDTeammate:_animate_state_switch( weapon, player_panel, weapon_panel1, weapon_panel2, TARGET_Y, is_player, box_ai_bg, box_bg )
	local TOTAL_T = 0.25
	local t = TOTAL_T
	local weapon_y = weapon:y()
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		local a = t/TOTAL_T
		-- box_ai_bg:set_color( box_ai_bg:color():with_alpha( is_player and (a) or (1-a) ) )
		box_ai_bg:set_alpha( is_player and (a) or (1-a) )
		-- box_bg:set_color( box_bg:color():with_alpha( is_player and (1-a) or (a) ) )
		box_bg:set_alpha( is_player and (1-a) or (a) )
				
		player_panel:set_alpha( is_player and (1-a) or (a) )
		
		local y = math.lerp( TARGET_Y, weapon_y, t/TOTAL_T )
		weapon:set_y( y )
		weapon_panel1:set_y( y )
		weapon_panel2:set_y( y )
	end
	weapon:set_y( TARGET_Y )
	weapon_panel1:set_y( TARGET_Y )
	weapon_panel2:set_y( TARGET_Y )
	-- box_ai_bg:set_color( box_ai_bg:color():with_alpha( is_player and 0 or 1 ) )
	box_ai_bg:set_alpha( is_player and 0 or 1 )
	-- box_bg:set_color( box_bg:color():with_alpha( is_player and 1 or 0 ) )
	box_bg:set_alpha( is_player and 1 or 0 )
	player_panel:set_alpha( is_player and 1 or 0 )
	-- print( "_animate_state_switch done" )
end]]

--[[function HUDTeammate:_animate_weapon_fired( ammo_pad )
	local t = 1
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt * 4
		local b = math.lerp( 1, 0.6, t )
		ammo_pad:set_color( Color( 1, b, b ) )
	end
	ammo_pad:set_color( Color.white )
	-- print( "done" )
end]]


