require "lib/managers/menu/MenuBackdropGUI"
require "lib/managers/menu/WalletGuiObject"

HUDPackageUnlockedItem = HUDPackageUnlockedItem or class()

function HUDPackageUnlockedItem:init( panel, row, params, hud_stage_end_screen )
	self._panel = panel:panel( { w=panel:w()-20, h=panel:h()*0.5-15-10, x=10, y=10+30, alpha=0 } )
	self._panel:move( 0, self._panel:h() * (row-1) )
	
	-- self._panel:rect( { color=Color.white, alpha=0.5, blend_mode="add" } )
	-- self._panel:set_debug(true)
	if row > 2 then				-- Currently, the gui only supports two rows
		self._panel:hide()
	end
	
	local announcement = params.announcement
	local upgrade = params.upgrade
	local bitmap_texture = "guis/textures/pd2/endscreen/test_icon_package"
	local text_string = ""
	local blend_mode = "normal"
	
	if announcement then
		bitmap_texture = "guis/textures/pd2/endscreen/announcement"
		text_string = managers.localization:to_upper_text( "menu_es_announcement" ) .. "\n" .. managers.localization:to_upper_text( announcement )
		blend_mode = "add"
	elseif upgrade then
		local upgrade_def = tweak_data.upgrades.definitions[ upgrade ]
		if upgrade_def then
			local category = Idstring( upgrade_def.category )
			if category == Idstring("weapon") then
				local weapon_name = managers.weapon_factory:get_weapon_name_by_factory_id( upgrade_def.factory_id )
				local weapon_class = managers.localization:text( "menu_" .. tweak_data.weapon[upgrade_def.weapon_id].category )
				local weapon_category = managers.localization:text( "bm_menu_" .. (tweak_data.weapon[ upgrade_def.weapon_id ].use_data.selection_index == 2 and "primaries" or "secondaries") )
				bitmap_texture = "guis/textures/pd2/blackmarket/icons/weapons/" .. upgrade_def.weapon_id
				text_string = managers.localization:text( "menu_es_package_weapon", {weapon=utf8.to_upper( weapon_name ), type=utf8.to_upper(weapon_class), category=weapon_category, INVENTORY_MENU=managers.localization:text("menu_inventory")} )
				
			elseif category == Idstring( "armor" ) then
				bitmap_texture = "guis/textures/pd2/blackmarket/icons/armors/" .. upgrade_def.armor_id
				text_string = managers.localization:text( "menu_es_package_armor", {armor=managers.localization:to_upper_text( upgrade_def.name_id )} )
			elseif category == Idstring( "rep_upgrade" ) then
				bitmap_texture = "guis/textures/pd2/endscreen/" .. upgrade_def.category
				text_string = managers.localization:to_upper_text( "menu_es_rep_upgrade", {point=upgrade_def.value or 2} )
				blend_mode = "add"
				
				hud_stage_end_screen:give_skill_points( upgrade_def.value or 2 )
			else
				bitmap_texture = "guis/textures/pd2/endscreen/" .. upgrade_def.category
			end
		end
	else
		Application:debug( "HUDPackageUnlockedItem: Something something unknown" )
	end
	
	-- if bitmap_texture then
		local bitmap = self._panel:bitmap( { texture=bitmap_texture, blend_mode=blend_mode } )
		local tw = bitmap:texture_width()
		local th = bitmap:texture_height()
		
		if th ~= 0 then
			local ratio = tw/th
			local size = self._panel:h() - 10
			
			local sw = math.max( size, size*ratio )
			local sh = math.max( size, size/ratio )
			
			bitmap:set_size( sw, sh )
			bitmap:set_center_x( self._panel:h() - 5 )
			bitmap:set_center_y( self._panel:h()/2 )
		-- end
		
		-- if text_string then
			local text = self._panel:text( { font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size, color=tweak_data.screen_colors.text, text=text_string, x=bitmap:right() + 10, y=bitmap:top(), vertical="center", wrap=true, word_wrap=true } )
			text:grow( -text:x() - 5, -text:y() - 5 )
			
			local _, _, _, h = text:text_rect()
			if h > text:h() then
				text:set_font( tweak_data.menu.pd2_small_font_id )
				text:set_font_size( tweak_data.menu.pd2_small_font_size )
			end
		-- end
	end
	
	self._panel:animate( callback( self, self, "create_animation" ) )
end

--[[
			local w = self._package_picture:texture_width()
			local h = self._package_picture:texture_height()
			local sh = math.min( self._package_forepanel:w()*0.30, self._package_forepanel:w()*0.30/(w/h) )
			local sw = math.min( self._package_forepanel:w()*0.30, self._package_forepanel:w()*0.30*(w/h) )
			
			local cx, cy = self._package_picture:center()
			self._package_picture:set_size( sw, sh )
			self._package_picture:set_center( cx, cy )
			
			local _, _, _, h = self._package_upgrade_text:text_rect()
			self._package_items:set_top( h + self._package_upgrade_text:y() )
			self._package_items:set_left( self._package_upgrade_text:left() )
			self._package_items:set_text( item_string )
			
			
if( data.announcements ) then -- got some announcements to make
	self._package_unlocked_text:set_text( utf8.to_upper( managers.localization:text( "menu_es_announcements" ) ) )
	self._package_upgrade_text:set_text( "" )
	self._package_picture:set_image( "guis/textures/pd2/mission_briefing/calendar_stamp" )
	
	self._package_unlocked_text:set_align( "center" )
	
	for _, string_id in ipairs( data.announcements ) do
		item_string = item_string .. managers.localization:text( string_id ) .. "\n"
	end
elseif( data.upgrades ) then	-- got upgrades
	self._package_unlocked_text:set_text( utf8.to_upper( managers.localization:text( "menu_package_unlocked" ) .. ": '" .. managers.localization:text( data.name_id ) .. "'" ) )
	self._package_upgrade_text:set_text( utf8.to_upper(managers.localization:text( "menu_package_upgrade", {package=data.name_id} ) ) )
	
	self._package_unlocked_text:set_align( "left" )
	for _, id in ipairs( data.upgrades ) do
		local upgrade = tweak_data.upgrades.definitions[ id ]
		
		if( upgrade ) then
			local category = upgrade.category
			local upgrade_string = ""
			local localized = false
			
			if category == "weapon" then
				upgrade_string = managers.weapon_factory:get_weapon_name_by_factory_id( upgrade.factory_id )
				localized = true
			elseif category =="crafting" then
				upgrade_string = tweak_data.weapon[ upgrade.weapon_id ].name_id
			elseif category == "equipment" or category =="armor" or category == "ammo" or category == "what_is_this" then
				upgrade_string = upgrade.name_id
			elseif( category == "rep_upgrade" ) then
				upgrade_string = "menu_es_rep_upgrade"
			end
			
			if( not localized ) then
				item_string = item_string .. managers.localization:text( upgrade_string ) .. "\n"
			else
				item_string = item_string .. upgrade_string .. "\n"
			end
		else
			item_string = item_string .. "! " .. id .. " !" .. "\n"
		end
	end
	local first_upgrade = tweak_data.upgrades.definitions[ data.upgrades[1] ]
	
	if( first_upgrade and first_upgrade.category == "weapon" ) then
		self._package_picture:set_image( "guis/textures/pd2/blackmarket/icons/weapons/" .. first_upgrade.weapon_id )
	else
		self._package_picture:set_image( "guis/textures/pd2/endscreen/test_icon_package" )
	end
end
]]

function HUDPackageUnlockedItem:create_animation()
	managers.menu_component:post_event( "stinger_new_weapon" )
	over( 0.3, function(p) self._panel:set_alpha( math.lerp( 0, 1, p ) ) end )
end

function HUDPackageUnlockedItem:destroy_animation()
	over( 0.1, function(p) self._panel:set_alpha( math.lerp( 1, 0.2, p ) ) end )
	over( 0.3, function(p) self._panel:set_alpha( math.lerp( 0.2, 0, p ) ) end )
	
	self._panel:parent():remove( self._panel )
	self._panel = nil
end

function HUDPackageUnlockedItem:close()
	if not alive(self._panel) then
		return
	end
	self._panel:stop()
	self._panel:animate( callback( self, self, "destroy_animation" ) )
end


HUDStageEndScreen = HUDStageEndScreen or class()

function HUDStageEndScreen:init( hud, workspace )
	self._backdrop = MenuBackdropGUI:new( workspace )
	self._backdrop:create_black_borders()
	
	self._hud = hud
	self._workspace = workspace
	
	self._singleplayer = Global.game_settings.single_player
	
	local bg_font = tweak_data.menu.pd2_massive_font
	local title_font = tweak_data.menu.pd2_large_font
	local content_font = tweak_data.menu.pd2_medium_font
	local small_font = tweak_data.menu.pd2_small_font
	
	local bg_font_size = tweak_data.menu.pd2_massive_font_size
	local title_font_size = tweak_data.menu.pd2_large_font_size
	local content_font_size = tweak_data.menu.pd2_medium_font_size
	local small_font_size = tweak_data.menu.pd2_small_font_size
	
	self._background_layer_safe = self._backdrop:get_new_background_layer()
	self._background_layer_full = self._backdrop:get_new_background_layer()
	self._foreground_layer_safe = self._backdrop:get_new_foreground_layer()
	self._foreground_layer_full = self._backdrop:get_new_foreground_layer()
	
	self._backdrop:set_panel_to_saferect( self._background_layer_safe )
	self._backdrop:set_panel_to_saferect( self._foreground_layer_safe )
	
	if( managers.job:has_active_job() ) then
		local current_contact_data = managers.job:current_contact_data()
		local contact_gui = self._background_layer_full:gui( current_contact_data.assets_gui, {empty=true} )
		-- contact_gui:clear()	-- only needs the pattern
		
		local contact_pattern = contact_gui:has_script() and contact_gui:script().pattern
		if( contact_pattern ) then
			self._backdrop:set_pattern( contact_pattern )
		end
	end
	
	
	------------------- Stage Name
	self._stage_name = managers.job:current_level_id() and managers.localization:to_upper_text( tweak_data.levels[ managers.job:current_level_id() ].name_id ) or ""
-- 	self._stage_name = managers.job:current_stage_data() and utf8.to_upper( managers.localization:text(managers.job:current_stage_data().level_id) ) or ""
	self._foreground_layer_safe:text( { name="stage_text", text=self._stage_name, h=title_font_size, align="left", vertical="center", font_size=title_font_size, font=title_font, color=tweak_data.screen_colors.text } )
	
	local bg_text = self._background_layer_full:text( { name="stage_text", text=self._stage_name, h=bg_font_size, align="left", vertical="top", font_size=bg_font_size, font=bg_font, color=tweak_data.screen_colors.button_stage_3, alpha=0.4 } )
	bg_text:set_world_center_y( self._foreground_layer_safe:child("stage_text"):world_center_y() )
	bg_text:set_world_x( self._foreground_layer_safe:child("stage_text"):world_x() )
	bg_text:move( -13, 9 )
	
	self._backdrop:animate_bg_text( bg_text )
	
	
	
	------------------- Level Progress
	self._lp_backpanel = self._background_layer_safe:panel( { name="lp_backpanel", w=self._background_layer_safe:w()/2-10, h=self._background_layer_safe:h()/2, y=70 } )
	self._lp_forepanel = self._foreground_layer_safe:panel( { name="lp_forepanel", w=self._foreground_layer_safe:w()/2-10, h=self._foreground_layer_safe:h()/2, y=70 } )
	
	self._lp_forepanel:text( { name="level_progress_text", text=managers.localization:to_upper_text( "menu_es_level_progress" ), align="left", vertical="top", h=content_font_size+2, font_size=content_font_size, font=content_font, color=tweak_data.screen_colors.text, x=10, y=10 } )
	
	local lp_bg_circle = self._lp_backpanel:bitmap( { name="bg_progress_circle", texture="guis/textures/pd2/endscreen/exp_ring", h=self._lp_backpanel:h()-content_font_size, w=self._lp_backpanel:h()-content_font_size, y=content_font_size, color=Color.black, alpha=0.6,  blend_mode="normal" } )
	self._lp_circle = self._lp_backpanel:bitmap( { name="progress_circle", texture="guis/textures/pd2/endscreen/exp_ring", h=self._lp_backpanel:h()-content_font_size, w=self._lp_backpanel:h()-content_font_size, y=content_font_size, color=Color(0, 1, 1), render_template="VertexColorTexturedRadial", blend_mode="add", layer=1 } )
	self._lp_text = self._lp_forepanel:text( { name="level_text", text="", align="center", vertical="center", font_size=bg_font_size, font=bg_font, h=self._lp_backpanel:h()-content_font_size, w=self._lp_backpanel:h()-content_font_size, y=content_font_size, color=tweak_data.screen_colors.text } )
	
	self._lp_curr_xp 		= self._lp_forepanel:text( { name="current_xp", text=managers.localization:to_upper_text( "menu_es_current_xp" ), align="left", vertical="center", h=small_font_size ,font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text } )
	self._lp_xp_gained 	= self._lp_forepanel:text( { name="xp_gained", text= managers.localization:to_upper_text( "menu_es_xp_gained" ), align="left", vertical="center", h=content_font_size ,font_size=content_font_size, font=content_font, color=tweak_data.screen_colors.text } )
	self._lp_next_level	= self._lp_forepanel:text( { name="next_level", text=managers.localization:to_upper_text( "menu_es_next_level" ), align="left", vertical="center", h=small_font_size ,font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text } )
	self._lp_skill_points	= self._lp_forepanel:text( { name="skill_points", text=managers.localization:to_upper_text( "menu_es_skill_points_gained" ), align="left", vertical="center", h=small_font_size ,font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text } )
	
	self._lp_xp_curr 	= self._lp_forepanel:text( { name="c_xp", text="", align="left", vertical="top", h=small_font_size, font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text } )
	self._lp_xp_gain	= self._lp_forepanel:text( { name="xp_g", text="", align="left", vertical="top", h=content_font_size, font_size=content_font_size, font=content_font, color=tweak_data.screen_colors.text } )
	self._lp_xp_nl		= self._lp_forepanel:text( { name="xp_nl", text="", align="left", vertical="top", h=small_font_size, font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text } )
	self._lp_sp_gain	= self._lp_forepanel:text( { name="sp_g", text="0", align="left", vertical="center", h=small_font_size, font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text } )
	
	local _, _, cw, ch = self._lp_curr_xp:text_rect()
	local _, _, gw, gh = self._lp_xp_gained:text_rect()
	local _, _, nw, nh = self._lp_next_level:text_rect()
	local _, _, sw, sh = self._lp_skill_points:text_rect()
	
	local w = math.ceil( math.max( cw, gw, nw, sw ) ) + 20
	
	local squeeze_more_pixels = false
	if not (SystemInfo:platform() == Idstring( "WIN32" )) and w > 170 then
		w = 170
		squeeze_more_pixels = true
	end
	
	self._num_skill_points_gained = 0
	
	
	self._lp_sp_info = self._lp_forepanel:text( { name="sp_info", text=managers.localization:text("menu_es_skill_points_info", {SKILL_MENU=managers.localization:to_upper_text("menu_skilltree")}), align="left", vertical="top", h=small_font_size, font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text, wrap=true, word_wrap=true } )
	self._lp_sp_info:grow( -self._lp_circle:right() - 20, 0 )
	
	local _, _, iw, ih = self._lp_sp_info:text_rect()
	self._lp_sp_info:set_h( ih )
	
	self._lp_sp_info:set_leftbottom( self._lp_circle:right()+0, self._lp_forepanel:h()-10 )
	
	
	self._lp_skill_points:set_h( sh )
	self._lp_skill_points:set_left( self._lp_sp_info:left() )
	self._lp_skill_points:set_bottom( self._lp_sp_info:top() )
	
	self._lp_sp_gain:set_h( sh )
	self._lp_sp_gain:set_left( self._lp_skill_points:left() + w )
	self._lp_sp_gain:set_top( self._lp_skill_points:top() )
	
	
	
	self._lp_next_level:set_h( nh )
	self._lp_next_level:set_left( self._lp_sp_info:left() )
	self._lp_next_level:set_bottom( self._lp_skill_points:top() )
	
	self._lp_xp_nl:set_h( nh )
	self._lp_xp_nl:set_left( self._lp_next_level:left() + w  )
	self._lp_xp_nl:set_top( self._lp_next_level:top() )
	
	
	
	self._lp_curr_xp:set_left( self._lp_sp_info:left() )
	self._lp_curr_xp:set_bottom( self._lp_next_level:top() )
	self._lp_curr_xp:set_h( gh )
	
	self._lp_xp_curr:set_left( self._lp_curr_xp:left() + w  )
	self._lp_xp_curr:set_top( self._lp_curr_xp:top() )
	self._lp_xp_curr:set_h( ch )
	
	
	
	
	self._lp_xp_gained:set_left( self._lp_curr_xp:left() )
	-- self._lp_xp_gained:set_bottom( self._lp_circle:center_y() - self._lp_xp_gained:h() )
	self._lp_xp_gained:set_h( ch )
	
	self._lp_xp_gain:set_left( self._lp_xp_gained:x() + w + 5  )
	-- self._lp_xp_gain:set_top( self._lp_xp_gained:y() )
	self._lp_xp_gain:set_h( gh )
	
	
	--[[
	self._lp_curr_xp:set_left( self._lp_circle:right() + 15 )
	self._lp_curr_xp:set_top( self._lp_circle:top() )
	self._lp_curr_xp:set_h( ch )
	
	self._lp_xp_gained:set_left( self._lp_curr_xp:left() )
	self._lp_xp_gained:set_top( self._lp_curr_xp:bottom() + 0 )
	self._lp_xp_gained:set_h( gh )
	]]
	
	
	if squeeze_more_pixels then
		lp_bg_circle:move( -20, 0 )
		self._lp_circle:move( -20, 0 )
		self._lp_text:move( -20, 0 )
		self._lp_curr_xp:move( -30, 0 )
		self._lp_xp_gained:move( -30, 0 )
		self._lp_next_level:move( -30, 0 )
		self._lp_skill_points:move( -30, 0 )
		self._lp_sp_info:move( -30, 0 )
	end
	
	self._box = BoxGuiObject:new( self._lp_backpanel, { sides = { 1, 1, 1, 1 } } )
	
	WalletGuiObject.set_wallet( self._foreground_layer_safe )
	
	-- self._lp_circle:set_layer(5)
	
	--- self._lp_test = self._lp_forepanel:rect( { layer=3 } )
	--[[
	local next_level_data = managers.experience:next_level_data()
	if( next_level_data ) then
		local current = next_level_data.current_points or 0
		local total = next_level_data.points or 0
		local xp_gained = managers.experience:xp_gained()
		
		self._lp_text:set_text( managers.experience:current_level() )
		self._lp_xp_curr:set_text( xp_gained )
		self._lp_xp_nl:set_text( total - current )
		
		if( total ~= 0 ) then
			self._lp_circle:set_color( Color( current / total, 1, 1 ) )
		end
	end
	]]
	
	-- self._package_backpanel = self._background_layer_safe:panel( { name="package_backpanel", w=self._background_layer_safe:w()/2, h=self._background_layer_safe:h()/2, y=140 } )

		--[[	
	self._package_forepanel = self._foreground_layer_safe:panel( { name="package_forepanel", w=self._foreground_layer_safe:w()/2, h=self._foreground_layer_safe:h()/2-70, y=70 } )
	self._package_forepanel:set_right( self._foreground_layer_safe:w() )
	self._package_forepanel:set_alpha( 0 )

	-- self._package_picture = self._package_forepanel:rect( { w=self._package_forepanel:w()*0.30, h=self._package_forepanel:w()*0.30, y=content_font_size+10, color=Color.white } )
	self._package_picture = self._package_forepanel:bitmap( { w=self._package_forepanel:w()*0.30, h=self._package_forepanel:w()*0.30, y=content_font_size+10, color=Color.white, texture="guis/textures/pd2/endscreen/test_icon_package" } )
	self._package_picture:move( self._package_forepanel:w() * 0.025, self._package_forepanel:w() * 0.025 )
	
	self._package_unlocked_text = self._package_forepanel:text( { name="package_unlocked", text=managers.localization:text( "menu_package_unlocked" ) .. ": ", align="left", vertical="top", h=content_font_size ,font_size=content_font_size, font=content_font, color=tweak_data.screen_colors.text } )
	self._package_upgrade_text = self._package_forepanel:text( { name="package_upgrade_text", text="", align="left", vertical="top", y=content_font_size+10, font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.text, wrap=true, word_wrap=true, w=self._package_forepanel:w()*0.65 } )
	self._package_upgrade_text:set_right( self._package_forepanel:w() )
	self._package_upgrade_text:set_h( self._package_forepanel:h()-self._package_upgrade_text:y() )
	
	self._package_items = self._package_forepanel:text( { name="package_items", text="", align="left", vertical="top", font_size=small_font_size, font=small_font, color=tweak_data.screen_colors.important_2 } )
]]

	self._package_forepanel = self._foreground_layer_safe:panel( { name="package_forepanel", w=self._foreground_layer_safe:w()/2-10, h=self._foreground_layer_safe:h()/2-70 - 10, y=70, alpha=0 } )
	self._package_forepanel:set_right( self._foreground_layer_safe:w() )
	self._package_forepanel:text( { name="title_text", font=content_font, font_size=content_font_size, text="", x=10, y=10 } )
	
	local package_box_panel = self._foreground_layer_safe:panel()
	package_box_panel:set_shape( self._package_forepanel:shape() )
	package_box_panel:set_layer( self._package_forepanel:layer() )
	self._package_box = BoxGuiObject:new( package_box_panel, { sides = { 1, 1, 1, 1 } } )
	
	-- self._package_forepanel:set_debug(true)
	self._package_items = {}
	
	-- self:stop_stage()
	self:clear_stage()
	if( self._data ) then
		self:start_experience_gain()
		-- self:animate_level_progress( self._lp_circle, self._data )
	end
	
	
	
	-- self._lp_circle:animate( anim_red, self._lp_test, true )
	-- self._lp_backpanel:set_debug(true)
	-- self._package_picture:set_debug(true)
	
	--[[
	------------------- Level Progress test
	local lp_backpanel = self._background_layer_safe:panel( { name="lp_backpanel", w=self._background_layer_safe:w()/2, h=self._background_layer_safe:h()/2, y=140 } )
	local lp_forepanel = self._foreground_layer_safe:panel( { name="lp_forepanel", w=self._foreground_layer_safe:w()/2, h=self._foreground_layer_safe:h()/2, y=140 } )
	
	lp_backpanel:set_layer( 10 )
	lp_forepanel:set_layer( 10 )
	
	lp_backpanel:set_left( self._lp_backpanel:right() )
	lp_forepanel:set_left( self._lp_forepanel:right() )
	
	
	lp_backpanel:move(-400,0)
	lp_forepanel:move(-400,0)
	self._lp_forepanel:text( { name="level_progress_text", text=managers.localization:text( "menu_level_progress" ), align="left", vertical="top", h=content_font_size ,font_size=content_font_size, font=content_font, color=Color( 163/255, 236/255, 255/255 ) } )
	
	local lp_circle = lp_forepanel:bitmap( { name="level_progress_circle", texture="guis/textures/pd2/endscreen/exp_ring", h=self._lp_backpanel:h()-content_font_size, w=self._lp_backpanel:h()-content_font_size, y=content_font_size, color=Color(0, 1, 1), render_template="VertexColorTexturedRadial", blend_mode="add" } )
	lp_circle:set_layer(5)
	
	local lp_test = lp_forepanel:rect( { layer=3 } )
	
	lp_circle:animate( anim_red, lp_test, false )
	-- lp_backpanel:set_debug(true)
	]]
	
	-- self._backdrop._panel:set_debug(true)
	-- self._backdrop._blackborder_workspace:set_debug(true)
end

function HUDStageEndScreen:hide()
	self._backdrop:hide()
end

function HUDStageEndScreen:show()
	self._backdrop:show()
end

function HUDStageEndScreen:update_layout()
	self._backdrop:_set_black_borders()
end

function HUDStageEndScreen:spawn_animation( o, delay, post_event )
	wait( delay or 0 )
	if post_event then
		managers.menu_component:post_event( post_event ) -- "count_1_finished")
		-- managers.menu_component:post_event( "menu_enter" )
	end
	over( 0.5, function( p ) o:set_alpha( p ) end )
end
	
function HUDStageEndScreen:destroy_animation( o, delay, speed )
	wait( delay or 0 )
	local start_alpha = o:alpha()
	over( 0.25 * (speed or 1), function(p) 
		o:set_alpha( math.lerp( start_alpha, 0, p ) ) 
		if o.children then
			for _, child in ipairs( o:children() ) do
				if child.set_color then
					child:set_color( math.lerp( child:color(), tweak_data.screen_colors.text, p ) )
				else
					for _, object in ipairs( child:children() ) do
						object:set_color( math.lerp( object:color(), tweak_data.screen_colors.text, p ) )
					end
				end
			end
		end
	end )
	o:parent():remove( o )
	o=nil
end
	
function HUDStageEndScreen:bonus_risk( panel, delay, bonus )
	local risk_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.risk, text=managers.localization:to_upper_text("menu_es_risk_bonus") } )
	local _, _, w, h = risk_text:text_rect()
	risk_text:set_size( w, h )
	
	panel:set_h( h )
	
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	
	--[[
	local risk_icons = { "risk_swat", "risk_fbi", "risk_death_squad" }
	local x = 5
	for i=1, math.min( difficulty_stars, #risk_icons ) do
		local texture, rect = tweak_data.hud_icons:get_icon_data( risk_icons[i] )
		local risk = panel:bitmap( { texture=texture, texture_rect=rect, x=w+x, color=tweak_data.screen_colors.risk, alpha=0 } )
		x = x + risk:w() + 4
		
		risk:animate( callback( self, self, "spawn_animation" ), delay + i*0.65 )
		panel:set_h( math.max( h, risk:bottom() ) )
	end]]
	
	panel:animate( callback( self, self, "spawn_animation" ), delay, "box_tick" )
	
	local sign_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.risk, text="+", alpha=0, align="right" } )
	sign_text:set_world_right( self._lp_xp_curr:world_left() )
	sign_text:animate( callback( self, self, "spawn_animation" ), delay, false )
	
	
	local value_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.risk, text=managers.money:add_decimal_marks_to_string( tostring(math.abs(bonus)) ), alpha=0 } )
	value_text:set_world_left( self._lp_xp_curr:world_left() )
	value_text:animate( callback( self, self, "spawn_animation" ), delay, false )
	
	return delay
end
--[[
function HUDStageEndScreen:bonus_swat( panel, delay )
	local texture, rect = tweak_data.hud_icons:get_icon_data( "risk_swat" )
	local risk = panel:bitmap( { texture=texture, texture_rect=rect, x=0, y=0, color=tweak_data.screen_colors.risk } )
	risk:set_center_y( math.round( panel:h()/2 ) )
	
	panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.risk, x=risk:right()+5, vertical="center", text=managers.localization:to_upper_text("menu_es_risk_bonus") } ):set_center_y( risk:center_y() )
	panel:animate( callback( self, self, "spawn_animation" ), delay )
end

function HUDStageEndScreen:bonus_fbi( panel, delay )
	local texture, rect = tweak_data.hud_icons:get_icon_data( "risk_fbi" )
	local risk = panel:bitmap( { texture=texture, texture_rect=rect, x=0, y=0, color=tweak_data.screen_colors.risk } )
	risk:set_center_y( math.round( panel:h()/2 ) )
	
	panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.risk, x=risk:right()+5, vertical="center", text=managers.localization:to_upper_text("menu_es_risk_bonus") } ):set_center_y( risk:center_y() )
	panel:animate( callback( self, self, "spawn_animation" ), delay  )
end

function HUDStageEndScreen:bonus_special( panel, delay )
	local texture, rect = tweak_data.hud_icons:get_icon_data( "risk_death_squad" )
	local risk = panel:bitmap( { texture=texture, texture_rect=rect, x=0, y=0, color=tweak_data.screen_colors.risk } )
	risk:set_center_y( math.round( panel:h()/2 ) )
	
	panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.risk, x=risk:right()+5, vertical="center", text=managers.localization:to_upper_text("menu_es_risk_bonus") } ):set_center_y( risk:center_y() )
	panel:animate( callback( self, self, "spawn_animation" ), delay )
end
]]

function HUDStageEndScreen:bonus_days( panel, delay, bonus )
	local text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.localization:to_upper_text("menu_es_day_bonus") } )
	local _, _, w, h = text:text_rect()
	panel:set_h( h )
	text:set_size( w, h )
	text:set_center_y( math.round(panel:h()/2) )
	
	panel:animate( callback( self, self, "spawn_animation" ), delay, "box_tick" )
	
	local sign_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text="+", alpha=0, align="right" } )
	sign_text:set_world_right( self._lp_xp_curr:world_left() )
	sign_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	
	local value_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.money:add_decimal_marks_to_string( tostring(math.abs(bonus)) ), alpha=0 } )
	value_text:set_world_left( self._lp_xp_curr:world_left() )
	value_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	return delay + 0.0
end

function HUDStageEndScreen:bonus_skill( panel, delay, bonus )
	local text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.localization:to_upper_text("menu_es_skill_bonus") } )
	local _, _, w, h = text:text_rect()
	panel:set_h( h )
	text:set_size( w, h )
	text:set_center_y( math.round(panel:h()/2) )
	
	panel:animate( callback( self, self, "spawn_animation" ), delay, "box_tick" )
	
	local sign_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text="+", alpha=0, align="right" } )
	sign_text:set_world_right( self._lp_xp_curr:world_left() )
	sign_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	
	local value_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.money:add_decimal_marks_to_string( tostring(math.abs(bonus)) ), alpha=0 } )
	value_text:set_world_left( self._lp_xp_curr:world_left() )
	value_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	return delay + 0.0
end

function HUDStageEndScreen:bonus_num_players( panel, delay, bonus )
	local text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.localization:to_upper_text("menu_es_alive_players_bonus") } )
	local _, _, w, h = text:text_rect()
	panel:set_h( h )
	text:set_size( w, h )
	text:set_center_y( math.round(panel:h()/2) )
	
	panel:animate( callback( self, self, "spawn_animation" ), delay, "box_tick" )
	
	local sign_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text="+", alpha=0, align="right" } )
	sign_text:set_world_right( self._lp_xp_curr:world_left() )
	sign_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	
	local value_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.money:add_decimal_marks_to_string( tostring(math.abs(bonus)) ), alpha=0 } )
	value_text:set_world_left( self._lp_xp_curr:world_left() )
	value_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	return delay + 0.0
end

function HUDStageEndScreen:bonus_failed( panel, delay, bonus )
	local text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.important_1, text=managers.localization:to_upper_text("menu_es_alive_failed_bonus") } )
	local _, _, w, h = text:text_rect()
	panel:set_h( h )
	text:set_size( w, h )
	text:set_center_y( math.round(panel:h()/2) )
	
	panel:animate( callback( self, self, "spawn_animation" ), delay, "box_tick" )
	
	local sign_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.important_1, text="-", alpha=0, align="right" } )
	sign_text:set_world_right( self._lp_xp_curr:world_left() )
	sign_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	
	local value_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.important_1, text=managers.money:add_decimal_marks_to_string( tostring(math.abs(bonus)) ), alpha=0 } )
	value_text:set_world_left( self._lp_xp_curr:world_left() )
	value_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	return delay + 0.0
end

function HUDStageEndScreen:bonus_low_level( panel, delay, bonus )
	local text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, 5, text=managers.localization:to_upper_text("menu_es_alive_low_level_bonus") } )
	local _, _, w, h = text:text_rect()
	panel:set_h( h )
	text:set_size( w, h )
	text:set_center_y( math.round(panel:h()/2) )
	
	panel:animate( callback( self, self, "spawn_animation" ), delay, "box_tick" )
	
	local sign_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text="-", alpha=0, align="right" } )
	sign_text:set_world_right( self._lp_xp_curr:world_left() )
	sign_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	
	local value_text = panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.money:add_decimal_marks_to_string( tostring(math.abs(bonus)) ), alpha=0 } )
	value_text:set_world_left( self._lp_xp_curr:world_left() )
	value_text:animate( callback( self, self, "spawn_animation" ), delay + 0.0, false )
	return delay + 0.0
end


function HUDStageEndScreen:start_experience_gain()
	self:reset_stage()
end

function HUDStageEndScreen:clear_stage()
	self._wait_t = nil
	self._csl = nil
	self._all_done = nil
	self._playing_sound = nil
	
	self._lp_text:hide()
	self._lp_circle:hide()
	self._lp_backpanel:child("bg_progress_circle"):hide()
	self._lp_forepanel:child("level_progress_text"):hide()

	self._lp_curr_xp:hide()
	self._lp_xp_gained:hide()
	self._lp_next_level:hide()
	self._lp_skill_points:hide()
	self._lp_sp_info:hide()
	
	self._lp_xp_gain:hide()
	self._lp_xp_curr:hide()
	self._lp_xp_nl:hide()
	self._lp_sp_gain:hide()
	
	self._lp_text:set_text( tostring( self._data and self._data.start_t.level or 0 ) )
	self:reset_skill_points()
	
	if self._background_layer_full:child("money_video") then
		self._background_layer_full:child("money_video"):stop()
		self._background_layer_full:remove( self._background_layer_full:child("money_video") )
	end
	if self._money_panel then
		self._money_panel:parent():remove( self._money_panel ) 
		self._money_panel = nil
	end
	
	WalletGuiObject.set_object_visible( "wallet_level_icon", false )
	WalletGuiObject.set_object_visible( "wallet_level_text", false )
	WalletGuiObject.set_object_visible( "wallet_money_icon", false )
	WalletGuiObject.set_object_visible( "wallet_money_text", false )
end

function HUDStageEndScreen:stop_stage()
	self:clear_stage()
	
	self._stage = 0
end

function HUDStageEndScreen:reset_stage()
	self:clear_stage()
	
	self._stage = 1
end

function HUDStageEndScreen:step_stage_up()
	self._stage = self._stage + 1
	-- Application:debug( "HUDStageEndScreen, Stepping Up", self._stage )
end

function HUDStageEndScreen:step_stage_down()
	self._stage = self._stage - 1
	-- Application:debug( "HUDStageEndScreen, Stepping Down", self._stage )
end

function HUDStageEndScreen:step_stage_to_start()
	self._stage = 1
end

function HUDStageEndScreen:step_stage_to_end()
	self._stage = #self.stages
end

function HUDStageEndScreen:_wait_for_video()
	local time = 0
	local video = self._background_layer_full:child( "money_video" )
	local length = video:length()
	local fade_t = 1
	local alpha = 0
	while video:loop_count() == 0 do
		local dt = coroutine.yield()
		time = time + dt
		video:set_alpha( math.min(time, 1) * 0.2 )
	end
	
	if( alive( video ) ) then
		video:stop()
		video:animate( callback( self, self, "destroy_animation" ), nil, 4 )
	end
end

function HUDStageEndScreen:create_money_counter( t, dt )
	local is_success = game_state_machine:current_state().is_success and game_state_machine:current_state():is_success()
	
	self._is_fail_video = not is_success
	if not (SystemInfo:platform() == Idstring( "X360" )) then
		if self._is_fail_video then
			local variant = math.random(2)
			local video = self._background_layer_full:video( { name="money_video", video = "movies/fail_stage"..tostring(variant), alpha=0, width = 1280, height = 720, blend_mode="add", loop=false } )
			video:animate( callback( self, self, "_wait_for_video" ), nil )
		else
			local variant = 0
			local video = self._background_layer_full:video( { name="money_video", video = "movies/money_count"..tostring(variant), alpha=0, width = 1280, height = 720, blend_mode="add", loop=true } )
			video:animate( callback( self, self, "spawn_animation" ), 1, false )
		end
	end
	
	self._money_panel = self._lp_forepanel:panel( { name="money_panel", x=10, y=10 } )
	self._money_panel:grow( -20, -20 )
	-- self._money_panel:set_debug(true)
	
	local stage_payout, job_payout, bag_payout, small_loot_payout, crew_payout = managers.money:get_payouts()
	
	local check_if_clear = function( data )
		for _, d in ipairs( data ) do
			if d[2] and d[2] > 0 then
				return false
			end
		end
		return true
	end
	
	self._money = {}
	self._money.income = {
		{"menu_es_cash_stage", math.round(stage_payout or 0)},
		{"menu_es_cash_job", math.round(job_payout or 0)},
		{"menu_cash_bonus_bags", math.round(bag_payout)}, -- managers.loot:get_secured_bonus_bags_amount() or 0,
		{"menu_cash_crew", math.round(crew_payout or 0)},
		{"hud_instant_cash", math.round(small_loot_payout or 0)},
		name_id=managers.localization:to_upper_text("menu_cash_income", {money=""} ),
	}
	if check_if_clear(self._money.income) then
		self._money.income = {}
	end
	
	
	self._money.costs = {
		-- {"menu_cash_assets", math.round(managers.assets:get_money_spent())},
		-- {"menu_cash_civ_kill", math.round(math.min(managers.money:get_civilian_deduction() * (managers.statistics:session_total_civilian_kills() or 0), managers.money:total() - managers.money:heist_spending()))},
		name_id=managers.localization:to_upper_text("menu_cash_costs", {money=""} ),
	}
	if check_if_clear(self._money.costs) then
		self._money.costs = {}
	end
	
	local spending_earned = managers.money:heist_spending()
	self._money.balance = {
		{"hud_offshore_account", math.round(managers.money:heist_offshore())},
		{"menu_cash_spending", math.round(spending_earned), spending_earned>0 and tweak_data.screen_colors.friend_color or tweak_data.screen_colors.pro_color},
		name_id=managers.localization:to_upper_text("menu_cash_balance", {money=""} ),
	}
	if check_if_clear(self._money.balance) then
		self._money.balance = {}
	end
	
	self._money_stage = { "income", "costs", "balance" }
	self._money_stage_index = 1
	self._money_index = 0
	self._money_text_y = 10
	self._start_count_money = false
	self._counting_money = false
	self._money_counting_amount = 0
-- 	self._prev_money_object = self._money_panel
	self._wait_t = t + 1
	self:step_stage_up()
	
	self._debug_m = nil
end

function HUDStageEndScreen:set_debug_m(name)
	do return end
	if self._debug_m ~= name then
		self._debug_m = name
		Application:debug(name)
	end
end

function HUDStageEndScreen:count_money( t, dt )
	local money_stage = self._money_stage[ self._money_stage_index ]
	
	if money_stage then
		local money_data = self._money[ money_stage ]
		if money_data then
			local money_specific = money_data[ self._money_index ]
			if (money_specific or self._money_index == 0) and money_data.name_id then
				if self._money_index == 0 then
					local text_object = self._money_panel:text( { x=0, y=self._money_text_y, text=money_data.name_id, font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
					managers.hud:make_fine_text( text_object )
					text_object:grow( 0, 2 )
					
					managers.menu_component:post_event("box_tick")
					self._money_text_y = text_object:bottom()
					self._wait_t = t + 0.65
					self._money_index = 1
					-- self:set_debug_m("new "..money_data.name_id)
				elseif self._start_count_money then
					local text_object = self._money_panel:text( { name="text"..tostring(self._money_stage_index)..tostring(self._money_index), x=self._money_panel:w()*0.5, y=self._money_text_y, text=managers.experience:cash_string( 0 ), font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size } )
					managers.hud:make_fine_text( text_object )
					
					local dir_object = self._money_panel:text( { name="dir"..tostring(self._money_stage_index)..tostring(self._money_index), y=self._money_text_y, text=(money_specific[2]<0 or money_stage=="costs") and "-" or "+", font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size } )
					managers.hud:make_fine_text( dir_object )
					dir_object:set_right( text_object:left() )
					dir_object:hide()
					
					self._wait_t = t + 0.45
					self._start_count_money = false
					self._counting_money = true
					self._money_counting_amount = 0
					-- self:set_debug_m("start "..tostring(self._money_stage_index)..tostring(self._money_index))
					self._set_count_first = true
					
				elseif self._counting_money then
					local text_object = self._money_panel:child( "text"..tostring(self._money_stage_index)..tostring(self._money_index) )
					local dir_object = self._money_panel:child( "dir"..tostring(self._money_stage_index)..tostring(self._money_index) )
					
					if self._set_count_first then
						self._set_count_first = nil
						managers.menu_component:post_event("count_1")
						dir_object:show()
					end
					
					self._money_counting_amount = math.round( math.step( self._money_counting_amount, money_specific[2], dt*math.max( 20000, money_specific[2]/1.5 ) ) )
					
					text_object:set_text( managers.experience:cash_string( math.abs(self._money_counting_amount) ) )
					managers.hud:make_fine_text( text_object )
					if self._money_counting_amount == money_specific[2] then
						self._counting_money = false
						self._money_index = self._money_index + 1
						self._money_text_y = text_object:bottom()
						self._wait_t = t + 0.45
						managers.menu_component:post_event("count_1_finished")
						
						text_object:set_color( money_specific[3] or tweak_data.screen_colors.text )
						dir_object:set_color( money_specific[3] or tweak_data.screen_colors.text )
						-- self:set_debug_m("stop "..tostring(self._money_stage_index)..tostring(self._money_index))
					else
						-- self:set_debug_m("count "..tostring(self._money_stage_index)..tostring(self._money_index))
					end
				elseif not money_specific[2] or money_specific[2] == 0 then
					self._money_index = self._money_index + 1
					-- self:set_debug_m("skip "..money_specific[1])
				else
					local text_object = self._money_panel:text( { x=10, y=self._money_text_y, text=managers.localization:to_upper_text(money_specific[1], {money=""}), font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size } )
					managers.hud:make_fine_text( text_object )
					
					managers.menu_component:post_event("box_tick")
					self._start_count_money = true
					-- self:set_debug_m("more "..money_specific[1])
				end
			else
				self._money_index = 0
				self._money_stage_index = self._money_stage_index + 1
				self._money_text_y = self._money_text_y + 15
				self._wait_t = t + ( money_data and 0 or 1)
				-- self:set_debug_m("next "..tostring(self._money_stage_index))
			end
			return
		end
	end
	WalletGuiObject.refresh()
	WalletGuiObject.set_object_visible( "wallet_money_icon", true )
	WalletGuiObject.set_object_visible( "wallet_money_text", true )
	managers.menu_component:show_endscreen_cash_summary()
	-- self:set_debug_m("done")
	self._wait_t = t + 1.25
	self:step_stage_up()
end

--[[
function HUDStageEndScreen:list_money_profits( t, dt )
	Application:debug("HUDStageEndScreen:list_money_profits")
	print( inspect( self._money.income ) )
	
	if self._money_index == 0 then
		local text_object = self._money_panel:text( { x=0, y=self._money_text_y, text="INCOME", font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
		managers.hud:make_fine_text( text_object )
		text_object:grow( 0, 5 )
		
		self._money_text_y = text_object:bottom()
	elseif self._money_index <= #self._money.income then
		local text_object = self._money_panel:text( { x=10, y=self._money_text_y, text=self._money.income[self._money_index][1], font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
		managers.hud:make_fine_text( text_object )
		
		self._money_text_y = text_object:bottom()
	else
		self._wait_t = t + 5
		self._money_text_y = self._money_text_y + 5
		self._money_index = 0
		self:step_stage_up()
	end
	
	self._money_index = self._money_index + 1
end

function HUDStageEndScreen:list_money_costs( t, dt )
	Application:debug("HUDStageEndScreen:list_money_costs")
	print( inspect( self._money.costs ) )
	
	if self._money_index == 0 then
		local text_object = self._money_panel:text( { x=0, y=self._money_text_y, text="COSTS", font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
		managers.hud:make_fine_text( text_object )
		text_object:grow( 0, 5 )
		
		self._money_text_y = text_object:bottom()
	elseif self._money_index <= #self._money.costs then
		local text_object = self._money_panel:text( { x=10, y=self._money_text_y, text=self._money.costs[self._money_index][1], font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
		managers.hud:make_fine_text( text_object )
		
		self._money_text_y = text_object:bottom()
	else
		self._wait_t = t + 1
		self._money_text_y = self._money_text_y + 5
		self._money_index = 0
		self:step_stage_up()
	end
	
	self._money_index = self._money_index + 1
end

function HUDStageEndScreen:list_money_balance( t, dt )
	Application:debug("HUDStageEndScreen:list_money_balance")
	print( inspect( self._money.balance ) )
	
	if self._money_index == 0 then
		local text_object = self._money_panel:text( { x=0, y=self._money_text_y, text="BALANCE", font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
		managers.hud:make_fine_text( text_object )
		text_object:grow( 0, 5 )
		
		self._money_text_y = text_object:bottom()
	elseif self._money_index <= #self._money.balance then
		local text_object = self._money_panel:text( { x=10, y=self._money_text_y, text=self._money.balance[self._money_index][1], font=tweak_data.menu.pd2_medium_font, font_size=tweak_data.menu.pd2_medium_font_size } )
		managers.hud:make_fine_text( text_object )
		
		self._money_text_y = text_object:bottom()
	else
		self._wait_t = t + 5
		self._money_text_y = self._money_text_y + 5
		self._money_index = 0
		self:step_stage_up()
	end
	
	self._money_index = self._money_index + 1
end
]]

function HUDStageEndScreen:hide_money( t, dt )
	Application:debug("HUDStageEndScreen:hide_money")
	
	if not self._is_fail_video then
		local video = self._background_layer_full:child("money_video")
		if video then
			video:animate( callback( self, self, "destroy_animation" ) )
		end
	end
	self._money_panel:animate( callback( self, self, "destroy_animation" ) )
	self._money_panel = nil
	
	self:step_stage_up()
end

function HUDStageEndScreen:stage_init( t, dt )
	local data = self._data
	
	self._lp_text:show()
	self._lp_circle:show()
	self._lp_backpanel:child("bg_progress_circle"):show()
	self._lp_forepanel:child("level_progress_text"):show()
	
	if( data.gained == 0 and data.start_t.current == data.start_t.total ) then
		self._lp_text:set_text( tostring( data.start_t.level ) )
		self._lp_circle:set_color( Color(1, 1, 1) )
		managers.menu_component:post_event("box_tick")
		
		self:step_stage_to_end()
		return
	end
	
	self._lp_circle:set_alpha( 0 )
	self._lp_backpanel:child("bg_progress_circle"):set_alpha( 0 )
	self._lp_text:set_alpha( 0 )
	
	if( data.gained == 0 ) then
		managers.menu_component:post_event("box_tick")
		self:step_stage_to_end()
		return
	end
	
	self._bonuses_panel = self._lp_forepanel:panel( { x=self._lp_curr_xp:x(), y=10 } )
	self._bonuses_panel:grow( -self._bonuses_panel:x(), -self._bonuses_panel:y() )
	--[[
	local last_stage = data.bonuses[3]
	local has_active_job = managers.job:has_active_job()
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local base_xp = last_stage and managers.experience:get_job_xp_by_stars( job_stars ) or managers.experience:get_stage_xp_by_stars( job_stars )
	local risk_xp = last_stage and managers.experience:get_current_job_xp_by_stars( job_stars ) or managers.experience:get_current_stage_xp_by_stars( job_stars )
	risk_xp = risk_xp - base_xp
	
	local unmodded_total_xp = data.bonuses.total
	local num_players_bonus = (data.bonuses[2] and tweak_data.experience_manager.alive_humans_multiplier[ data.bonuses[2] ] or 1) - 1
	local alive_bonus = unmodded_total_xp * num_players_bonus
	
	local failed_bonus = 0
	
	(not success and tweak_data.experience_manager.stage_failed_multiplier or 1) 
		]]
	
	local stage_text = managers.localization:to_upper_text( "menu_es_base_xp_stage" )
	local base_text = self._bonuses_panel:text( { name="base_text", font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=stage_text } )
	local xp_text = self._bonuses_panel:text( { name="xp_text", font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=managers.money:add_decimal_marks_to_string( tostring(data.bonuses.stage_xp) ) } )
	
	local _, _, tw, th = base_text:text_rect()
	base_text:set_h( th )
	
	xp_text:set_world_left( self._lp_xp_curr:world_left() )
	
	
	local delay = 0.80
	local y = math.round( base_text:bottom() )
	if data.bonuses.last_stage then
		local job_text = managers.localization:to_upper_text( "menu_es_base_xp_job" )
		local job_xp_fade_panel = self._bonuses_panel:panel( {alpha=0} )
		local base_text = job_xp_fade_panel:text( { name="base_text", font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, text=job_text, y=y } )
		local sign_text = job_xp_fade_panel:text( { name="sign_text", font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, y=y, text="+", align="right" } )
		local xp_text = job_xp_fade_panel:text( { name="xp_text", font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, color=tweak_data.screen_colors.text, y=y, text=managers.money:add_decimal_marks_to_string( tostring(data.bonuses.job_xp) ) } )
		
		local _, _, tw, th = base_text:text_rect()
		base_text:set_h( th )
		
		xp_text:set_world_left( self._lp_xp_curr:world_left() )
		sign_text:set_world_right( self._lp_xp_curr:world_left() )
		
		delay = 0.85+0.60
		y = math.round( base_text:bottom() )
		job_xp_fade_panel:animate( callback( self, self, "spawn_animation" ), 0.60, "box_tick" )
	end
	
	
	local bonuses_to_string_converter = { "bonus_risk", "bonus_low_level", "bonus_failed", "bonus_days", "bonus_num_players", "bonus_skill" }
	-- local bonuses_to_value_converter = { risk_xp, alive_bonus, 0, failed_bonus, 0 }
	
	
	if data.bonuses.rounding_error ~= 0 then
		Application:debug("GOT A ROUNDING ERROR IN EXPERIENCE GIVING:", data.bonuses.rounding_error)
	end
		
	local index = 2
	for i, func_name in ipairs( bonuses_to_string_converter ) do
		local bonus = data.bonuses[func_name] 
		if bonus and bonus~=0 then
			local panel = self._bonuses_panel:panel( { alpha=0, y=y } )
			delay = (callback( self, self, func_name )( panel, delay, bonus ) or delay) + 0.60
			
			y = y + panel:h()
			index = index + 1
		end
	end
	
	
	local sum_line = self._bonuses_panel:rect( { color=Color(0.0, 1, 1, 1), alpha=0.0, h=2 } )
	sum_line:set_y( y )
	-- sum_line:animate( callback( self, self, "spawn_animation" ), delay )

	self._lp_xp_gain:set_world_top( sum_line:world_top() )
	if SystemInfo:platform() == Idstring( "WIN32" ) then
		self._lp_xp_gain:move( 0, self._lp_xp_gain:h() )
	end
	self._lp_xp_gained:set_top( self._lp_xp_gain:top() )
	
	local sum_text = self._bonuses_panel:text( { font=tweak_data.menu.pd2_small_font, font_size=tweak_data.menu.pd2_small_font_size, text="= ", align="right", alpha=0 } )
	sum_text:set_world_righttop( self._lp_xp_gain:world_left(), self._lp_xp_gain:world_top() )
	sum_text:animate( callback( self, self, "spawn_animation" ), delay+1, "box_tick" )
	
	self._lp_circle:set_color( Color( (data.start_t.current / data.start_t.total), 1, 1 ) )
	self._wait_t = t + 1
	self._start_ramp_up_t = delay
	self._ramp_up_timer = 0
	
	-- managers.menu_component:post_event("menu_enter")
	managers.menu_component:post_event("box_tick")
	self:step_stage_up()
end

function HUDStageEndScreen:stage_spin_up( t, dt )
	local data = self._data
	
	if self._start_ramp_up_t then
		self._ramp_up_timer = math.min( self._ramp_up_timer + dt, self._start_ramp_up_t )
		local ratio = (self._ramp_up_timer/self._start_ramp_up_t)*(data.start_t.current / data.start_t.total)
		-- self._lp_circle:set_color( Color( ratio, 1, 1 ) )
		
		ratio = (self._ramp_up_timer/self._start_ramp_up_t)
		self._lp_circle:set_alpha( ratio )
		self._lp_backpanel:child("bg_progress_circle"):set_alpha( ratio * 0.6 )
		self._lp_text:set_alpha( ratio )
		
		if self._ramp_up_timer == self._start_ramp_up_t then
			self._static_current_xp = data.start_t.xp
			self._static_gained_xp = 0
			self._static_start_xp = data.start_t.current
			
			self._current_xp = self._static_current_xp
			self._gained_xp = self._static_gained_xp
			self._next_level_xp = data.start_t.total - data.start_t.current
			self._speed = 1
			self._wait_t = t + 2.4
			
			self._ramp_up_timer = nil
			self._start_ramp_up_t = nil
			
			
			ratio = 1
			self._lp_circle:set_alpha( ratio )
			self._lp_backpanel:child("bg_progress_circle"):set_alpha( ratio * 0.6 )
			self._lp_text:set_alpha( ratio )
		
			self._lp_text:stop()
			self._lp_text:set_font_size( tweak_data.menu.pd2_massive_font_size )
			self._lp_text:set_text( tostring( data.start_t.level ) )
			
			self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.start_t.xp ) ) ) )
			self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( 0 ) ) ) )
			self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.start_t.total - data.start_t.current ) ) ) )
			
			
			local clbk = callback( self, self, "spawn_animation" )
	
			
			self._lp_curr_xp:show()
			self._lp_xp_gained:show()
			self._lp_next_level:show()
			
			self._lp_xp_gain:show()
			self._lp_xp_curr:show()
			self._lp_xp_nl:show()
			
			
		-- 	managers.menu_component:post_event("box_tick")
			self._lp_curr_xp:animate(clbk, 0)
			self._lp_xp_gained:animate(clbk, 0)
			self._lp_next_level:animate(clbk, 0)
			
			self._lp_xp_gain:animate(clbk, 0)
			self._lp_xp_curr:animate(clbk, 0)
			self._lp_xp_nl:animate(clbk, 0)
			
			-- self._bonuses_panel:animate( callback( self, self, "destroy_animation" ), 1 )
			self:step_stage_up()
		end
	end
end

function HUDStageEndScreen:stage_show_all( t, dt )
	self._lp_curr_xp:show()
	self._lp_xp_gained:show()
	self._lp_next_level:show()
	
	self._lp_xp_gain:show()
	self._lp_xp_curr:show()
	self._lp_xp_nl:show()
	self:step_stage_up()
end

function HUDStageEndScreen:stage_spin_levels( t, dt )
	-- do print( inspect( self._data ) ) self._stage = 4 end
	local data = self._data
	
	
	if not self._playing_sound then
		self._playing_sound = true
		managers.menu_component:post_event("count_1")
	end
	self._csl = self._csl or 1		-- current spin level
	local current_level_data = data[ self._csl ]
	if current_level_data then
		local total_xp = current_level_data.total
		local xp_gained_frame = dt * self._speed * math.max( (total_xp*0.08), 450 )
		
		self._next_level_xp = self._next_level_xp - xp_gained_frame
		if self._next_level_xp <= 0 then
			xp_gained_frame = xp_gained_frame + self._next_level_xp
			
			self._next_level_xp = 0
		end
		
		self._current_xp = self._current_xp + xp_gained_frame
		self._gained_xp = self._gained_xp + xp_gained_frame
		
		self._speed = self._speed + dt * 1.55
		
		local ratio = 1 - ( self._next_level_xp / total_xp )
		
		self._lp_circle:set_color( Color( ratio, 1, 1 ) )
		
		
		if( self._next_level_xp == 0 ) then
			self._csl = self._csl + 1				-- next level
			if data[ self._csl ] then
				self._next_level_xp = data[ self._csl ].total
			else
				self._next_level_xp = data.end_t.total
			end
			-- self._current_xp = current_level_data.total
			-- self._gained_xp = 0
			-- self._next_level_xp = 0
			
			self._static_current_xp = self._static_current_xp + current_level_data.total - self._static_start_xp
			self._static_gained_xp = self._static_gained_xp + current_level_data.total - self._static_start_xp
			self._current_xp = self._static_current_xp
			self._gained_xp = self._static_gained_xp
			self._static_start_xp = 0
			
		-- print( inspect( current_level_data ) )
			
			self._speed = math.max( 1, self._speed * 0.55 )
			
			if self:level_up( current_level_data.level ) then
				self._wait_t = t + 1.4
				managers.menu_component:post_event("count_1_finished")
				self._playing_sound = nil
			else
				self._wait_t = t + 0.4
				managers.menu_component:post_event("count_1_finished")
				self._playing_sound = nil
			end
		end
		
		self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._current_xp ) ) ) )
		self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._gained_xp ) ) ) )
		self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._next_level_xp ) ) ) )
	else
		self._speed = math.max( 1.55, self._speed * 0.55 )
		self._top_speed = self._speed
		self:step_stage_up()
	end
	--[[
	for i = 1, #data do
		current_xp, gained_xp, speed = spin_func( self, o, data[i].current, data[i].total, data[i].total, current_xp, gained_xp, speed )
		
		local package_unlocked = tweak_data.upgrades.level_tree[ data[i].level ]
		
		self._lp_text:stop()
		self._lp_text:animate( ding_func, base_font_size, math.min( 1, (data[i].level/managers.experience:level_cap()) * ( package_unlocked and 10 or 1 ) ), data[i].level )
		last_level = data[i].level
		
		if( package_unlocked ) then
			self._package_forepanel:stop()
			self._package_forepanel:animate( package_func, package_unlocked )
		end
	end
	]]
end

function HUDStageEndScreen:stage_spin_slowdown( t, dt )
	local data = self._data
	local xp_gained_frame = dt * self._speed * math.max( (data.end_t.total*0.1), 450 )
	local total_xp = data.end_t.total - data.end_t.current
	
	-- self._top_speed = self._top_speed or self._speed
	self._next_level_xp = self._next_level_xp - xp_gained_frame
	if self._next_level_xp < total_xp then
		xp_gained_frame = xp_gained_frame + (self._next_level_xp-total_xp)
		
		self._next_level_xp = total_xp
		self:step_stage_up()
		managers.menu_component:post_event("count_1_finished")
	end
	
	self._current_xp = self._current_xp + xp_gained_frame
	self._gained_xp = self._gained_xp + xp_gained_frame
	
	-- self._speed = self._speed + dt * 1.5
	if data.end_t.current ~= 0 then
		self._top_speed = self._top_speed or 1
		-- print((data.end_t.total - self._next_level_xp) / data.end_t.current)
		local ex =(data.end_t.total - self._next_level_xp) / data.end_t.current
		self._speed = math.max( 1, self._top_speed / ((self._top_speed*2)^ex) )
		-- self._speed = math.step( self._speed, 1, ((data.end_t.total - self._next_level_xp) / data.end_t.current) * dt * 25 )
	end
	
	local ratio = 1 - ( self._next_level_xp / data.end_t.total )
	self._lp_circle:set_color( Color( ratio, 1, 1 ) )
	
	self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._current_xp ) ) ) )
	self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._gained_xp ) ) ) )
	self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._next_level_xp ) ) ) )
end

--[[
	local last_level = data.start_t.level
	for i = 1, #data do
		current_xp, gained_xp, speed = spin_func( self, o, data[i].current, data[i].total, data[i].total, current_xp, gained_xp, speed )
		
		local package_unlocked = tweak_data.upgrades.level_tree[ data[i].level ]
		
		self._lp_text:stop()
		self._lp_text:animate( ding_func, base_font_size, math.min( 1, (data[i].level/managers.experience:level_cap()) * ( package_unlocked and 10 or 1 ) ), data[i].level )
		last_level = data[i].level
		
		if( package_unlocked ) then
			self._package_forepanel:stop()
			self._package_forepanel:animate( package_func, package_unlocked )
		end
	end
	
	current_xp, gained_xp, speed = spin_func( self, o, (#data>0 and 0) or data.start_t.current, data.end_t.current, data.end_t.total, current_xp, gained_xp, speed, true )
	
	if( last_level ~= data.end_t.level ) then
		self._lp_text:stop()
		self._lp_text:animate( ding_func, base_font_size, (data.end_t.level/managers.experience:level_cap()), tostring( data.end_t.level ) )
		
		local package_unlocked = tweak_data.upgrades.level_tree[ data.end_t.level ]
		
		if( package_unlocked ) then
			self._package_forepanel:stop()
			self._package_forepanel:animate( package_func, package_unlocked )
		end
	else
		self._lp_text:set_text( tostring( data.end_t.level ) )
	end
	 
	self._lp_xp_curr:set_text( tostring( math.floor( data.end_t.xp ) ) )
	self._lp_xp_gain:set_text( tostring( math.floor( data.gained ) ) )
	self._lp_xp_nl:set_text( tostring( math.floor( data.end_t.total - data.end_t.current ) ) )
	
	
	if( self._done_clbk ) then
		self._done_clbk( true )
	end
]]

function HUDStageEndScreen:stage_end( t, dt )
	local data = self._data
	local ratio = ( data.end_t.current / data.end_t.total )
	
	self._static_current_xp = data.end_t.xp
	self._static_gained_xp = data.gained
	self._current_xp = self._static_current_xp
	self._gained_xp = self._static_gained_xp
			
	self._lp_circle:set_color( Color( ratio, 1, 1 ) )
	self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._current_xp ) ) ) )
	self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( self._gained_xp ) ) ) )
	self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.end_t.total - data.end_t.current ) ) ) )
	
	self._wait_t = t -- + 3
	self:step_stage_up()
end

function HUDStageEndScreen:stage_done( t, dt )
	if self._all_done then
		return
	end
	
	if self._done_clbk then
		WalletGuiObject.refresh()
		WalletGuiObject.set_object_visible( "wallet_level_icon", true )
		WalletGuiObject.set_object_visible( "wallet_level_text", true )
		self._done_clbk( true )
		self._all_done = true
	end
end

function HUDStageEndScreen:level_up( level )
	local level_text_func = function( o, ding_scale, level )
		local center_x, center_y = o:center()
		local size = tweak_data.menu.pd2_massive_font_size
		local ding_size = size * ( 1.0 + ding_scale )
		
		wait( 0.1 )
		-- over( 0.09, function(p) o:set_font_size( math.lerp( size, ding_size, p ) ) end )
		o:set_text( tostring( level ) )
		self:give_skill_points( 1 )
		-- wait( 0.05 )
		
		-- over( 0.19, function(p)
			-- o:set_font_size( math.lerp( ding_size, size, p ) )
			-- o:set_color( math.lerp( tweak_data.screen_colors.text, tweak_data.screen_colors.button_stage_2, math.sin( p * 180 ) ) )
		-- end )
		-- o:set_color(tweak_data.screen_colors.text)
		-- o:set_font_size(size)
	end
	
	local text_ding_func = function( o )
		local TOTAL_T = 0.4
		local t = TOTAL_T
		local mul = 1
		-- local c_x, c_y = o:center()
		local size = o:font_size()
		while t > 0 do
			local dt = coroutine.yield()
			t = t - dt
			local ratio = math.max( t/TOTAL_T, 0 )
			mul = mul + dt * 4.0
			o:set_font_size( size * mul )
			-- o:set_center( c_x, c_y )
			
			o:set_alpha( ratio ) 
			o:set_color( math.lerp( Color.white, tweak_data.screen_colors.button_stage_2, 1 - ratio ) )
		end
		o:parent():remove( o )
	end
	
	local circle_ding_func = function( o )
		wait( 0.15 )
		local TOTAL_T = 0.6
		local t = TOTAL_T
		local mul = 1
		local c_x, c_y = o:center()
		local size = o:w()
		while t > 0 do
			local dt = coroutine.yield()
			t = t - dt
			local ratio = math.max( t/TOTAL_T, 0 )
			mul = mul + dt * 0.75
			o:set_size( size * mul, size * mul )
			o:set_center( c_x, c_y )
			
			o:set_alpha( ratio ) 
			o:set_color( math.lerp( Color.white, tweak_data.screen_colors.button_stage_2, 1 - ratio ) )
		end
		o:parent():remove( o )
	end
	
	local package_func = function( o, data )
		local start_alpha = o:alpha()
		for _, item in ipairs( self._package_items ) do
			item:close()
		end
		self._package_items = {}
		wait( 0.6 )
		
		local new_items = {}
		
		if data.announcements then
			for i, announcement in ipairs( data.announcements ) do
				table.insert( new_items, { announcement=announcement } )
			end
		end
		
		if data.upgrades then
			for i, upgrade in ipairs( data.upgrades ) do
				table.insert( new_items, { upgrade=upgrade } )
			end
		end
		self._package_forepanel:child("title_text"):set_text( managers.localization:to_upper_text( "menu_es_package_unlocked_" .. (#new_items==1 and "singular" or "plural") ) )
		
		if #new_items > 2 then
			Application:error( "HUDStageEndScreen: Please, max 2 announcements+upgrades per level in tweak_data.level_tree, rest will not be shown in gui!" )
		end
		
		over( 0.42, function(p) o:set_alpha( math.cos( 652 * p ) * math.rand(0.4, 0.8) ) end )
		over( 0.04, function(p) o:set_alpha( math.step( o:alpha(), 1, p ) ) end )
		o:set_alpha( 1 )
		
		for i, item in ipairs( new_items ) do
			table.insert( self._package_items, HUDPackageUnlockedItem:new( o, i, item, self ) )
			wait( 0.24 )
		end
	end
	
	managers.menu_component:post_event("stinger_levelup")
	
	local ding_circle = self._lp_backpanel:bitmap( { texture="guis/textures/pd2/endscreen/exp_ring", w=self._lp_circle:w(), h=self._lp_circle:h(), x=self._lp_circle:x(), y=self._lp_circle:y(), color=Color.white, blend_mode="add", layer=0, rotation=360 } )
	ding_circle:animate( circle_ding_func )
	
	local ding_text = self._lp_forepanel:text( { w=self._lp_text:w(), h=self._lp_text:h(), x=self._lp_text:x(), y=self._lp_text:y(), color=Color.white, blend_mode="add", font_size=tweak_data.menu.pd2_massive_font_size, font=tweak_data.menu.pd2_massive_font, text=self._lp_text:text(), align="center", vertical="center", layer=0, rotation=360 } )
	ding_text:animate( text_ding_func )
	
	self._lp_circle:set_color( Color( 0, 1, 1 ) )
	
	self._lp_text:stop()
	self._lp_text:animate( level_text_func, 1.0, tostring( level ) )	-- (level/managers.experience:level_cap())
	
	local package_unlocked = tweak_data.upgrades.level_tree[ level ]
	if package_unlocked then
		self._package_forepanel:stop()
		self._package_forepanel:animate( package_func, package_unlocked )
	end
	
	return package_unlocked
end

function HUDStageEndScreen:reset_skill_points()
	self:give_skill_points( -self._num_skill_points_gained )
end

function HUDStageEndScreen:give_skill_points( points )
	self._num_skill_points_gained = self._num_skill_points_gained + points
	self._update_skill_points = true
end

function HUDStageEndScreen:stage_debug_loop( t, dt )
	self:reset_stage()
	self._wait_t = t + 3
end

HUDStageEndScreen.stages = { "create_money_counter", "count_money", "hide_money", "stage_init", "stage_spin_up", "stage_show_all", "stage_spin_levels", "stage_spin_slowdown", "stage_end", --[["stage_debug_loop",]] "stage_done" }
function HUDStageEndScreen:update( t, dt )
	if self._wait_t then
		if self._wait_t < t then
			self._wait_t = nil
		end
	elseif self._stage and self.stages[self._stage] then
		self[ self.stages[self._stage] ]( self, t, dt )
	else
		-- self:reset_stage()
	end
	
	if self._update_skill_points then
		self._update_skill_points = nil
		
		local skill_point_text_func = function( o, text )
			local center_x, center_y = o:center()
			local content_font_size = tweak_data.menu.pd2_small_font_size
			local start_font_size = o:font_size()
			o:set_rotation(360)
			o:set_color( tweak_data.screen_colors.text )
			
			over( 0.12, function(p) o:set_font_size( math.lerp( start_font_size, content_font_size*0.2, p ) ) end )
			over( 0.07, function(p) o:set_font_size( math.lerp( content_font_size*0.2, content_font_size*2, p ) ) end )
			o:set_text( text )
			
			over( 0.19, function(p)
				o:set_font_size( math.lerp( content_font_size*2, content_font_size, math.sin( p * 180 ) ) )
				o:set_color( math.lerp( tweak_data.screen_colors.text, tweak_data.screen_colors.button_stage_2, math.clamp( math.sin( p * 180 ), 0, 1 ) ) )
			end )
			o:set_font_size( content_font_size )
			o:set_color( tweak_data.screen_colors.text )
			o:set_rotation(0)
			
			local t = 0
			local dt = 0
			while( true ) do
				dt = coroutine.yield()
				t = (t + dt*90) % 180
				
				local color = math.lerp( tweak_data.screen_colors.text, tweak_data.screen_colors.resource, math.clamp( math.sin(t), 0, 1 ) )
				o:set_color( color )
			end
		end
		local animate_new_skillpoints = function(o)
			while true do
				over( 1, function(p) 
					o:set_alpha( math.lerp( 0.4, 0.85, math.sin(p*180) ) )
				end )
			end
		end
		
		self._lp_sp_gain:set_text( tostring( self._num_skill_points_gained ) )
		
		
		local skill_glow = self._lp_sp_gain:parent():child("skill_glow")
		if not skill_glow then
			skill_glow = self._lp_sp_gain:parent():bitmap( { name="skill_glow", texture="guis/textures/pd2/crimenet_marker_glow", w=self._lp_sp_gain:parent():w()*0.25, h=40, color=tweak_data.screen_colors.button_stage_3, layer=0, blend_mode="add", rotation=360 } )
		end	
		skill_glow:set_center_y( self._lp_skill_points:center_y() )
		skill_glow:set_center_x( self._lp_sp_gain:left() + 5 )
		skill_glow:stop()
		
		local visible = self._num_skill_points_gained > 0
		if visible then
			skill_glow:animate( animate_new_skillpoints )
		end
		-- self._lp_sp_gain:stop()
		-- local visible = self._num_skill_points_gained > 0
		-- if visible then
		-- 	self._lp_sp_gain:animate( skill_point_text_func, tostring( self._num_skill_points_gained ) )
		-- end
		skill_glow:set_visible( visible )
		self._lp_skill_points:set_visible( visible )
		self._lp_sp_gain:set_visible( visible )
		self._lp_sp_info:set_visible( visible )
	end
	--[[
	local spin_func = function( self, o, xp, end_xp, total_xp, current_xp, gained_xp, speed, breaks )
		local dt = 0
		local ratio = xp / total_xp
		local diff_xp = 0
		
		o:set_color( Color( ratio, 1, 1 ) )
		self._lp_xp_curr:set_text( math.floor( tostring( current_xp ) ) )
		self._lp_xp_gain:set_text( math.floor( tostring( gained_xp ) ) )
		self._lp_xp_nl:set_text( math.floor( tostring( total_xp - xp ) ) )
		
		while( true ) do
			dt = coroutine.yield()
			
			diff_xp = xp
			xp = math.min( xp + dt * total_xp * 0.20 * speed, end_xp )
			diff_xp = xp - diff_xp
			speed = speed + dt * 0.5
			
			if( breaks and end_xp ~= 0 ) then
				speed = math.lerp( speed, 1, xp/end_xp )
			end
			
			ratio = xp / total_xp
			gained_xp = gained_xp + diff_xp
			current_xp = current_xp + diff_xp
			
			o:set_color( Color( ratio, 1, 1 ) )
			self._lp_xp_curr:set_text( tostring( math.floor( current_xp ) ) )
			self._lp_xp_gain:set_text( tostring( math.floor( gained_xp ) ) )
			self._lp_xp_nl:set_text( tostring( math.floor( total_xp - xp ) ) )
			
			if( xp == end_xp ) then
				return current_xp, gained_xp, speed
			end
		end
	end
	
	
	local wait_func = function( self, o, wait_time )
		local dt = 0
		local time = wait_time
		while( true ) do
			if( self._skip ) then
				self._skip = nil
				return
			end
			dt = coroutine.yield()
			
			time = time - dt
			if( time <= 0 ) then
				return
			end
		end
	end
	
	local base_font_size = self._lp_text:font_size()
	local ding_func = function( o, font_size, ding_size, level )
		local center_x, center_y = o:center()
		local size = font_size
		local ding_size = size * ( 1.0 + ding_size )
		local color = o:color()
		
		over( 0.1, function(p) o:set_font_size( math.lerp( size, ding_size, p ) ) end )
		o:set_text( tostring( level ) )
		
		over( 0.18, function(p) 
			o:set_font_size( math.lerp( ding_size, size, p ) )
			o:set_color( math.lerp( color, tweak_data.screen_color_red, math.sin( p * 180 ) ) )
		end )
	end
	
	local package_func = function( o, data )
		local start_alpha = o:alpha()
		over( 0.04, function(p) o:set_alpha( math.lerp( start_alpha, 0, p ) ) end )
		
		if( data.name_id ) then	-- got something
			local item_string = ""
			
			if( data.announcements ) then -- got some announcements to make
				self._package_unlocked_text:set_text( utf8.to_upper( managers.localization:text( "menu_es_announcements" ) ) )
				self._package_upgrade_text:set_text( "" )
				self._package_picture:set_image( "guis/textures/pd2/mission_briefing/calendar_stamp" )
				
				self._package_unlocked_text:set_align( "center" )
				
				for _, string_id in ipairs( data.announcements ) do
					item_string = item_string .. managers.localization:text( string_id ) .. "\n"
				end
			elseif( data.upgrades ) then	-- got upgrades
				self._package_unlocked_text:set_text( utf8.to_upper( managers.localization:text( "menu_package_unlocked" ) .. ": '" .. managers.localization:text( data.name_id ) .. "'" ) )
				self._package_upgrade_text:set_text( utf8.to_upper(managers.localization:text( "menu_package_upgrade", {package=data.name_id} ) ) )
				
				self._package_unlocked_text:set_align( "left" )
				for _, id in ipairs( data.upgrades ) do
					local upgrade = tweak_data.upgrades.definitions[ id ]
					
					if( upgrade ) then
						local category = upgrade.category
						local upgrade_string = ""
						local localized = false
						
						if category == "weapon" then
							upgrade_string = managers.weapon_factory:get_weapon_name_by_factory_id( upgrade.factory_id )
							localized = true
						elseif category =="crafting" then
							upgrade_string = tweak_data.weapon[ upgrade.weapon_id ].name_id
						elseif category == "equipment" or category =="armor" or category == "ammo" or category == "what_is_this" then
							upgrade_string = upgrade.name_id
						elseif( category == "rep_upgrade" ) then
							upgrade_string = "menu_es_rep_upgrade"
						end
						
						if( not localized ) then
							item_string = item_string .. managers.localization:text( upgrade_string ) .. "\n"
						else
							item_string = item_string .. upgrade_string .. "\n"
						end
					else
						item_string = item_string .. "! " .. id .. " !" .. "\n"
					end
				end
				local first_upgrade = tweak_data.upgrades.definitions[ data.upgrades[1] ]
				
				if( first_upgrade and first_upgrade.category == "weapon" ) then
					self._package_picture:set_image( "guis/textures/pd2/blackmarket/icons/weapons/" .. first_upgrade.weapon_id )
				else
					self._package_picture:set_image( "guis/textures/pd2/endscreen/test_icon_package" )
				end
			end
			
			
			local w = self._package_picture:texture_width()
			local h = self._package_picture:texture_height()
			local sh = math.min( self._package_forepanel:w()*0.30, self._package_forepanel:w()*0.30/(w/h) )
			local sw = math.min( self._package_forepanel:w()*0.30, self._package_forepanel:w()*0.30*(w/h) )
			
			local cx, cy = self._package_picture:center()
			self._package_picture:set_size( sw, sh )
			self._package_picture:set_center( cx, cy )
			
			local _, _, _, h = self._package_upgrade_text:text_rect()
			self._package_items:set_top( h + self._package_upgrade_text:y() )
			self._package_items:set_left( self._package_upgrade_text:left() )
			self._package_items:set_text( item_string )
		else
			self._package_unlocked_text:set_text("")
			self._package_upgrade_text:set_text("")
			self._package_items:set_text("")
			self._package_picture:hide()
		end
		
		
		over( 0.08, function(p) o:set_alpha( math.lerp( 0, 1, p ) ) end )
	end
	
	if( data.gained == 0 and data.start_t.current == data.start_t.total ) then
		self._lp_text:set_text( tostring( data.start_t.level ) )
		self._lp_xp_gain:hide()
		self._lp_xp_curr:hide()
		self._lp_xp_nl:hide()
		
		self._lp_curr_xp:hide()
		self._lp_xp_gained:hide()
		self._lp_next_level:hide()
		
		o:set_color( Color(1,1,1) )
		if( self._done_clbk ) then
			self._done_clbk( true )
		end
		return
	end
	
	
		self._lp_xp_gain:show()
		self._lp_xp_curr:show()
		self._lp_xp_nl:show()
		
		self._lp_curr_xp:show()
		self._lp_xp_gained:show()
		self._lp_next_level:show()
		
		local current_xp = data.start_t.xp - data.start_t.current
		local gained_xp = 0
		local speed = 1.5
		self._lp_text:set_text( tostring( data.start_t.level ) )
		self._lp_xp_gain:hide()
		
		current_xp = data.start_t.xp
		gained_xp = 0
		speed = 1
		o:set_color( Color( data.start_t.current / data.start_t.total, 1, 1 ) )
		self._lp_xp_curr:set_text( tostring( math.floor( data.start_t.xp ) ) )
		self._lp_xp_gain:set_text( tostring( math.floor( gained_xp ) ) )
		self._lp_xp_nl:set_text( tostring( math.floor( data.start_t.total - data.start_t.current ) ) )
		self._lp_xp_gain:show()
		
		if( data.gained == 0 ) then
			if( self._done_clbk ) then
				self._done_clbk( true )
			end
			return
		end
		wait_func( self, o, 1.3 )
		
		local last_level = data.start_t.level
		for i = 1, #data do
			current_xp, gained_xp, speed = spin_func( self, o, data[i].current, data[i].total, data[i].total, current_xp, gained_xp, speed )
			
			local package_unlocked = tweak_data.upgrades.level_tree[ data[i].level ]
			
			self._lp_text:stop()
			self._lp_text:animate( ding_func, base_font_size, math.min( 1, (data[i].level/managers.experience:level_cap()) * ( package_unlocked and 10 or 1 ) ), data[i].level )
			last_level = data[i].level
			
			if( package_unlocked ) then
				self._package_forepanel:stop()
				self._package_forepanel:animate( package_func, package_unlocked )
			end
		end
		
		current_xp, gained_xp, speed = spin_func( self, o, (#data>0 and 0) or data.start_t.current, data.end_t.current, data.end_t.total, current_xp, gained_xp, speed, true )
		
		if( last_level ~= data.end_t.level ) then
			self._lp_text:stop()
			self._lp_text:animate( ding_func, base_font_size, (data.end_t.level/managers.experience:level_cap()), tostring( data.end_t.level ) )
			
			local package_unlocked = tweak_data.upgrades.level_tree[ data.end_t.level ]
			
			if( package_unlocked ) then
				self._package_forepanel:stop()
				self._package_forepanel:animate( package_func, package_unlocked )
			end
		else
			self._lp_text:set_text( tostring( data.end_t.level ) )
		end
		 
		self._lp_xp_curr:set_text( tostring( math.floor( data.end_t.xp ) ) )
		self._lp_xp_gain:set_text( tostring( math.floor( data.gained ) ) )
		self._lp_xp_nl:set_text( tostring( math.floor( data.end_t.total - data.end_t.current ) ) )
		
		
	if( self._done_clbk ) then
		self._done_clbk( true )
	end
	]]
end

function HUDStageEndScreen:set_continue_button_text( text )
	print( "HUDStageEndScreen:set_continue_button_text( text )", text )
	self._button_text = text
end

function HUDStageEndScreen:set_success( success, server_left )
	print( "HUDStageEndScreen:set_success( success, server_left )", success, server_left )
	self._success = success
	self._server_left = server_left
	
	local stage_status = success and utf8.to_upper( managers.localization:text("menu_success" ) ) or utf8.to_upper( managers.localization:text("menu_failed" ) )
	self._foreground_layer_safe:child("stage_text"):set_text( self._stage_name .. ": " .. stage_status )
	self._background_layer_full:child("stage_text"):set_text( self._stage_name .. ": " .. stage_status )
end

function HUDStageEndScreen:set_statistics( criminals_completed, success )
	print( "HUDStageEndScreen:set_statistics( criminals_completed, success )", criminals_completed, success )
	self._criminals_completed = criminals_completed
	self._success = success
	
	local stage_status = success and utf8.to_upper( managers.localization:text("menu_success" ) ) or utf8.to_upper( managers.localization:text("menu_failed" ) )
	self._foreground_layer_safe:child("stage_text"):set_text( self._stage_name .. ": " .. stage_status )
	self._background_layer_full:child("stage_text"):set_text( self._stage_name .. ": " .. stage_status )

	-- print( total_xp_bonus )
	-- self._end_xp = total_xp_bonus
	
	--[[
	time_played:set_text( string.upper( managers.statistics:session_time_played().." (H:M:S)" ) )
	completed_objectives:set_text( string.upper( "" ..comp_obj.. "%" ) )
	last_completed_challenge:set_text( string.upper( managers.challenges:get_last_comleted_title_text() ) )
	total_downed:set_text( string.upper( managers.statistics:total_downed() ) )
	favourite_weapon:set_text( string.upper(""..managers.statistics:session_favourite_weapon() ) )
	hit_accuracy:set_text( string.upper(managers.statistics:session_hit_accuracy().."%" ) )
	total_kills:set_text( string.upper( ""..managers.statistics:session_total_kills() ) )
	total_specials_kills:set_text( string.upper( ""..managers.statistics:session_total_specials_kills() ) )
	total_head_shots:set_text( string.upper(""..managers.statistics:session_total_head_shots() ) )
	completion_criminals_finished:set_text( string.upper(""..criminals_completed ) )
	completed_objectives_bonus:set_text( string.upper( "" ..comp_obj.. "%" ) )
	completed_civilians_killed_penalty:set_text( string.upper(""..total_civilian_kills ) )
	completion_criminals_finished_xp:set_text( managers.experience:cash_string( 0 ).._xp_postfix )
	completed_objectives_bonus_xp:set_text( managers.experience:cash_string( 0 ).._xp_postfix )
	completed_civilians_killed_penalty_xp:set_color( total_civilian_kills == 0 and Color.white or Color.red )
	completed_civilians_killed_penalty_xp:set_text( managers.experience:cash_string( 0 ).._xp_postfix )
	
	
	
	total_completion_bonus:set_text( managers.experience:cash_string( 0 ) .. xp_postfix )
	]]
end

function HUDStageEndScreen:animate_level_progress( o, data )
	local animate_func = function( o, self, data )
		local spin_func = function( self, o, xp, end_xp, total_xp, current_xp, gained_xp, speed, breaks )
			local dt = 0
			local ratio = xp / total_xp
			local diff_xp = 0
			
			o:set_color( Color( ratio, 1, 1 ) )
			self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( math.floor( tostring( current_xp ) ) ) )
			self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( math.floor( tostring( gained_xp ) ) ) )
			self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( math.floor( tostring( total_xp - xp ) ) ) )
			
			while( true ) do
				--[[
				if( self._skip ) then
					self._skip = nil
					o:set_color( Color( end_xp / total_xp, 1, 1 ) )
					self._lp_xp_curr:set_text( tostring( math.floor( current_xp + end_xp ) ) )
					self._lp_xp_gain:set_text( tostring( math.floor( gained_xp + end_xp ) ) )
					self._lp_xp_nl:set_text( tostring( math.floor( total_xp - end_xp ) ) )
					
					return nil
				end
				]]
				
				dt = coroutine.yield()
				
				diff_xp = xp
				xp = math.min( xp + dt * total_xp * 0.20 * speed, end_xp )
				diff_xp = xp - diff_xp
				speed = speed + dt * 0.5
				
				if( breaks and end_xp ~= 0 ) then
					speed = math.lerp( speed, 1, xp/end_xp )
				end
				
				ratio = xp / total_xp
				gained_xp = gained_xp + diff_xp
				current_xp = current_xp + diff_xp
				
				o:set_color( Color( ratio, 1, 1 ) )
				self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( current_xp ) ) ) )
				self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( gained_xp ) ) ) )
				self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( total_xp - xp ) ) ) )
				
				if( xp == end_xp ) then
					return current_xp, gained_xp, speed
				end
			end
		end
		
		
		local wait_func = function( self, o, wait_time )
			local dt = 0
			local time = wait_time
			while( true ) do
				if( self._skip ) then
					self._skip = nil
					return
				end
				dt = coroutine.yield()
				
				time = time - dt
				if( time <= 0 ) then
					return
				end
			end
		end
		
		local base_font_size = self._lp_text:font_size()
		local ding_func = function( o, font_size, ding_size, level )
			local center_x, center_y = o:center()
			local size = font_size
			local ding_size = size * ( 1.0 + ding_size )
			local color = o:color()
			
			over( 0.1, function(p) o:set_font_size( math.lerp( size, ding_size, p ) ) end )
			o:set_text( tostring( level ) )
			
			over( 0.18, function(p) 
				o:set_font_size( math.lerp( ding_size, size, p ) )
				o:set_color( math.lerp( color, tweak_data.screen_colors.important_2, math.sin( p * 180 ) ) )
			end )
		end
		
		local package_func = function( o, data )
			local start_alpha = o:alpha()
			over( 0.04, function(p) o:set_alpha( math.lerp( start_alpha, 0, p ) ) end )
			
			if( data.name_id ) then	-- got something
				local item_string = ""
				local items = {}
				
				
				if( data.announcements ) then -- got some announcements to make
					self._package_unlocked_text:set_text( utf8.to_upper( managers.localization:text( "menu_es_announcements" ) ) )
					self._package_upgrade_text:set_text( "" )
					self._package_picture:set_image( "guis/textures/pd2/mission_briefing/calendar_stamp" )
					
					self._package_unlocked_text:set_align( "center" )
					
					for _, string_id in ipairs( data.announcements ) do
						item_string = item_string .. managers.localization:text( string_id ) .. "\n"
					end
				elseif( data.upgrades ) then	-- got upgrades
					self._package_unlocked_text:set_text( utf8.to_upper( managers.localization:text( "menu_package_unlocked" ) .. ": '" .. managers.localization:text( data.name_id ) .. "'" ) )
					self._package_upgrade_text:set_text( utf8.to_upper(managers.localization:text( "menu_package_upgrade", {package=data.name_id} ) ) )
					
					self._package_unlocked_text:set_align( "left" )
					for _, id in ipairs( data.upgrades ) do
						local upgrade = tweak_data.upgrades.definitions[ id ]
						
						if( upgrade ) then
							local category = upgrade.category
							local upgrade_string = ""
							local localized = false
							
							if category == "weapon" then
								upgrade_string = managers.weapon_factory:get_weapon_name_by_factory_id( upgrade.factory_id )
								localized = true
							elseif category =="crafting" then
								upgrade_string = tweak_data.weapon[ upgrade.weapon_id ].name_id
							elseif category == "equipment" or category =="armor" or category == "ammo" or category == "what_is_this" then
								upgrade_string = upgrade.name_id
							elseif( category == "rep_upgrade" ) then
								upgrade_string = "menu_es_rep_upgrade"
							end
							
							if( not localized ) then
								item_string = item_string .. managers.localization:text( upgrade_string ) .. "\n"
							else
								item_string = item_string .. upgrade_string .. "\n"
							end
						else
							item_string = item_string .. "! " .. id .. " !" .. "\n"
						end
					end
					local first_upgrade = tweak_data.upgrades.definitions[ data.upgrades[1] ]
					
					if( first_upgrade and first_upgrade.category == "weapon" ) then
						self._package_picture:set_image( "guis/textures/pd2/blackmarket/icons/weapons/" .. first_upgrade.weapon_id )
					else
						self._package_picture:set_image( "guis/textures/pd2/endscreen/test_icon_package" )
					end
				end
				
				
				local w = self._package_picture:texture_width()
				local h = self._package_picture:texture_height()
				local sh = math.min( self._package_forepanel:w()*0.30, self._package_forepanel:w()*0.30/(w/h) )
				local sw = math.min( self._package_forepanel:w()*0.30, self._package_forepanel:w()*0.30*(w/h) )
				
				local cx, cy = self._package_picture:center()
				self._package_picture:set_size( sw, sh )
				self._package_picture:set_center( cx, cy )
				
				local _, _, _, h = self._package_upgrade_text:text_rect()
				self._package_items:set_top( h + self._package_upgrade_text:y() )
				self._package_items:set_left( self._package_upgrade_text:left() )
				self._package_items:set_text( item_string )
			else
				self._package_unlocked_text:set_text("")
				self._package_upgrade_text:set_text("")
				self._package_items:set_text("")
				self._package_picture:hide()
			end
			
			
			over( 0.08, function(p) o:set_alpha( math.lerp( 0, 1, p ) ) end )
		end
		
		if( data.gained == 0 and data.start_t.current == data.start_t.total ) then
			self._lp_text:set_text( tostring( data.start_t.level ) )
			self._lp_xp_gain:hide()
			self._lp_xp_curr:hide()
			self._lp_xp_nl:hide()
			
			self._lp_curr_xp:hide()
			self._lp_xp_gained:hide()
			self._lp_next_level:hide()
			
			o:set_color( Color(1,1,1) )
			if( self._done_clbk ) then
				self._done_clbk( true )
			end
			return
		end
		
			self._lp_xp_gain:show()
			self._lp_xp_curr:show()
			self._lp_xp_nl:show()
			
			self._lp_curr_xp:show()
			self._lp_xp_gained:show()
			self._lp_next_level:show()
	
		-- while( true ) do
			local current_xp = data.start_t.xp - data.start_t.current
			local gained_xp = 0 -- data.gained
			local speed = 1.5
			self._lp_text:set_text( tostring( data.start_t.level ) )
			self._lp_xp_gain:hide()
			-- current_xp, gained_xp, speed = spin_func( self, o, 0, data.start_t.current, data.start_t.total, current_xp, gained_xp, speed )
			
			current_xp = data.start_t.xp
			gained_xp = 0
			speed = 1
			o:set_color( Color( data.start_t.current / data.start_t.total, 1, 1 ) )
			self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.start_t.xp ) ) ) )
			self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( gained_xp ) ) ) )
			self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.start_t.total - data.start_t.current ) ) ) )
			self._lp_xp_gain:show()
			
			if( data.gained == 0 ) then
				if( self._done_clbk ) then
					self._done_clbk( true )
				end
				return
			end
			wait_func( self, o, 1.3 )
			
			local last_level = data.start_t.level
			for i = 1, #data do
				current_xp, gained_xp, speed = spin_func( self, o, data[i].current, data[i].total, data[i].total, current_xp, gained_xp, speed )
				
				local package_unlocked = tweak_data.upgrades.level_tree[ data[i].level ]
				
				self._lp_text:stop()
				self._lp_text:animate( ding_func, base_font_size, math.min( 1, (data[i].level/managers.experience:level_cap()) * ( package_unlocked and 10 or 1 ) ), data[i].level )
				last_level = data[i].level
				
				if( package_unlocked ) then
					self._package_forepanel:stop()
					self._package_forepanel:animate( package_func, package_unlocked )
				end
				-- self._lp_text:set_text(  )
			end
			
			current_xp, gained_xp, speed = spin_func( self, o, (#data>0 and 0) or data.start_t.current, data.end_t.current, data.end_t.total, current_xp, gained_xp, speed, true )
			
			if( last_level ~= data.end_t.level ) then
				self._lp_text:stop()
				self._lp_text:animate( ding_func, base_font_size, (data.end_t.level/managers.experience:level_cap()), tostring( data.end_t.level ) )
				
				local package_unlocked = tweak_data.upgrades.level_tree[ data.end_t.level ]
				
				if( package_unlocked ) then
					self._package_forepanel:stop()
					self._package_forepanel:animate( package_func, package_unlocked )
				end
			else
				self._lp_text:set_text( tostring( data.end_t.level ) )
			end
			 
			self._lp_xp_curr:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.end_t.xp ) ) ) )
			self._lp_xp_gain:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.gained ) ) ) )
			self._lp_xp_nl:set_text( managers.money:add_decimal_marks_to_string( tostring( math.floor( data.end_t.total - data.end_t.current ) ) ) )
		-- end
		
		if( self._done_clbk ) then
			self._done_clbk( true )
		end
	end
	
	o:animate( animate_func, self, data )
end

function HUDStageEndScreen:send_xp_data( data, done_clbk )
	self._data = data
	self._done_clbk = done_clbk
	
	print( "HUDStageEndScreen:send_xp_data", inspect( data ), done_clbk )
	
	self:start_experience_gain()
	-- self:animate_level_progress( self._lp_circle, data )
end


function HUDStageEndScreen:set_group_statistics( best_kills, best_kills_score,
										best_special_kills, best_special_kills_score, 
										best_accuracy, best_accuracy_score, 
										most_downs, most_downs_score, 
										total_kills, total_specials_kills, total_head_shots, group_accuracy, group_downs )

	print( "HUDStageEndScreen:set_group_statistics( best_kills, best_kills_score, best_special_kills, best_special_kills_score, best_accuracy, best_accuracy_score, most_downs, most_downs_score, total_kills, total_specials_kills, total_head_shots, group_accuracy, group_downs )", best_kills, best_kills_score, best_special_kills, best_special_kills_score, best_accuracy, best_accuracy_score, most_downs, most_downs_score, total_kills, total_specials_kills, total_head_shots, group_accuracy, group_downs )
	self._lot_of_stuff = {best_kills, best_kills_score,
										best_special_kills, best_special_kills_score, 
										best_accuracy, best_accuracy_score, 
										most_downs, most_downs_score, 
										total_kills, total_specials_kills, total_head_shots, group_accuracy, group_downs}
	
end


--[[
function present_completion_bonus( o, cb )
	local t = (10/30)
	
	local xp_postfix = _xp_postfix
	
	-------
	wait( 0.5 )
	-------
	
	local total_xp_bonus = 0
	local criminals_bonus_xp_counter = 0
	while criminals_bonus_xp_counter < _criminals_bonus_xp do
		local dt = coroutine.yield()
		local add_val = dt * 15 * math.max( 1, (_criminals_bonus_xp - criminals_bonus_xp_counter)/8 )
		criminals_bonus_xp_counter = criminals_bonus_xp_counter + add_val
		completion_criminals_finished_xp:set_text( managers.experience:cash_string( math.round(criminals_bonus_xp_counter) ).._xp_postfix )
		total_xp_bonus = total_xp_bonus + add_val
		total_completion_bonus:set_text( managers.experience:cash_string( math.round(total_xp_bonus) )..xp_postfix )
	end
	completion_criminals_finished_xp:set_text( managers.experience:cash_string( _criminals_bonus_xp ).._xp_postfix )
	
	-------
	wait( 0.5 )
	-------
	
	local objectives_bonus_xp_counter = 0
	while objectives_bonus_xp_counter < _objectives_bonus_xp do
		local dt = coroutine.yield()
		local add_val = dt * 15 * math.max( 1, (_objectives_bonus_xp - objectives_bonus_xp_counter)/8 )
		objectives_bonus_xp_counter = objectives_bonus_xp_counter + add_val
		completed_objectives_bonus_xp:set_text( managers.experience:cash_string( math.round(objectives_bonus_xp_counter) ).._xp_postfix )
		total_xp_bonus = total_xp_bonus + add_val
		total_completion_bonus:set_text(managers.experience:cash_string( math.round(total_xp_bonus) )..xp_postfix )
	end
	completed_objectives_bonus_xp:set_text( managers.experience:cash_string( _objectives_bonus_xp ).._xp_postfix )
	
	-------
	wait( 0.5 )
	-------
	
	local civilians_penalty_xp_counter = 0
	while civilians_penalty_xp_counter < _civilians_penalty_xp do
		local dt = coroutine.yield()
		local add_val = dt * 15 * math.max( 1, (_civilians_penalty_xp - civilians_penalty_xp_counter)/8 )
		civilians_penalty_xp_counter = civilians_penalty_xp_counter + add_val
		completed_civilians_killed_penalty_xp:set_text( managers.experience:cash_string( - math.round(civilians_penalty_xp_counter) ).._xp_postfix )
		total_xp_bonus = total_xp_bonus - add_val
		total_completion_bonus:set_text( managers.experience:cash_string( math.round(total_xp_bonus) )..xp_postfix )
	end
	completed_civilians_killed_penalty_xp:set_text( managers.experience:cash_string( - _civilians_penalty_xp ).._xp_postfix )
	total_completion_bonus:set_text( managers.experience:cash_string( _total_xp_bonus )..xp_postfix )
	
	-------
	wait( 0.1 )
	-------
	
	cb( _total_xp_bonus )
end
]]



function HUDStageEndScreen:reload()
	self._backdrop:close()
	self._backdrop = nil
	
	HUDStageEndScreen.init( self, self._hud, self._workspace )
end