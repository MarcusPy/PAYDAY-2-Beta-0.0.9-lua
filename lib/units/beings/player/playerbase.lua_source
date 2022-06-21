PlayerBase = PlayerBase or class( UnitBase )
PlayerBase.PLAYER_HUD = Idstring( "guis/player_hud" )
PlayerBase.PLAYER_INFO_HUD_FULLSCREEN = Idstring( "guis/player_info_hud_fullscreen" )
PlayerBase.PLAYER_INFO_HUD = Idstring( "guis/player_info_hud" )
PlayerBase.PLAYER_DOWNED_HUD = Idstring( "guis/player_downed_hud" )
PlayerBase.PLAYER_CUSTODY_HUD = Idstring( "guis/spectator_mode" )

PlayerBase.PLAYER_INFO_HUD_PD2 = Idstring( "guis/player_info_hud_pd2" )
PlayerBase.PLAYER_INFO_HUD_FULLSCREEN_PD2 = Idstring( "guis/player_info_hud_fullscreen_pd2" )

function PlayerBase:init( unit )
	UnitBase.init( self, unit, false )
	
	self._unit = unit
	
	self:_setup_suspicion_and_detection_data()
	self:_setup_hud()
	
	self._id = managers.player:player_id( self._unit )
	self._rumble_pos_callback = callback( self, self, "get_rumble_position" )
	self:_setup_controller()
	
	self._unit:set_extension_update_enabled( Idstring( "base" ), false )
	
	self._stats_screen_visible = false
	
	managers.game_play_central:restart_portal_effects()
	
	self:_chk_set_unit_upgrades()
end

-----------------------------------------------------------------------------------

function PlayerBase:update( unit, t, dt )
	if self._wanted_controller_enabled_t then
		if self._wanted_controller_enabled_t <= 0 then
			if self._wanted_controller_enabled then
				self._controller:set_enabled( true )
				self._wanted_controller_enabled = nil
				self._wanted_controller_enabled_t = nil
			end
			self._unit:set_extension_update_enabled( Idstring( "base" ), false )
		else
			self._wanted_controller_enabled_t = self._wanted_controller_enabled_t - 1
		end
	end
end

-----------------------------------------------------------------------------------

function PlayerBase:_setup_suspicion_and_detection_data()
	if not Network:is_server() then
		return
	end
	
	self._suspicion_settings = deep_clone( tweak_data.player.suspicion )
	self._suspicion_settings.multipliers = {}
	self._suspicion_settings.init_buildup_mul = self._suspicion_settings.buildup_mul
	self._suspicion_settings.init_range_mul = self._suspicion_settings.range_mul
	
	self._detection_settings = {}
	self._detection_settings.multipliers = {}
	self._detection_settings.init_delay_mul = 1
	self._detection_settings.init_range_mul = 1
end

-----------------------------------------------------------------------------------

function PlayerBase:_chk_set_unit_upgrades()
	local multiplier = 1
	if managers.player:has_category_upgrade( "player", "suspicion_multiplier" ) then
		multiplier = multiplier * managers.player:upgrade_value( "player", "suspicion_multiplier", 1 )
	end
	if managers.player:has_category_upgrade( "player", "passive_suspicion_multiplier" ) then
		multiplier = multiplier * managers.player:upgrade_value( "player", "passive_suspicion_multiplier", 1 )
	end	
	if multiplier and multiplier ~= 1 then
		self:set_suspicion_multiplier( "upgrade", multiplier )
	end
	
	managers.environment_controller:set_flashbang_multiplier( managers.player:upgrade_value( "player", "flashbang_multiplier" ) )
	
	local is_client = Network:is_client()
	
	if is_client then
		if managers.player:has_category_upgrade( "player", "suspicion_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "suspicion_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "suspicion_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "passive_suspicion_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "passive_suspicion_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "passive_suspicion_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "silent_kill" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "silent_kill", 1 )
		end
		if managers.player:has_category_upgrade( "player", "crouch_speed_multiplier" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "crouch_speed_multiplier", 1 )
		end
		if managers.player:has_category_upgrade( "player", "run_speed_multiplier" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "run_speed_multiplier", 1 )
		end
		if managers.player:has_category_upgrade( "player", "silent_drill" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "silent_drill", 1 )
		end
		if managers.player:has_category_upgrade( "player", "drill_alert_rad" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "drill_alert_rad", 1 )
		end
		if managers.player:has_category_upgrade( "player", "drill_autorepair" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "drill_autorepair", 1 )
		end
		if managers.player:has_category_upgrade( "player", "intimidate_enemies" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "intimidate_enemies", 1 )
		end
		if managers.player:has_category_upgrade( "player", "intimidate_specials" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "intimidate_specials", 1 )
		end
		if managers.player:has_category_upgrade( "player", "empowered_intimidation_mul" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "empowered_intimidation_mul", 1 )
		end
		if managers.player:has_category_upgrade( "player", "intimidation_multiplier" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "intimidation_multiplier", 1 )
		end
		if managers.player:has_category_upgrade( "player", "intimidate_aura" ) then
			local level = managers.player:upgrade_level( "player", "intimidate_aura" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "intimidate_aura", level )
		end
		if managers.player:has_category_upgrade( "player", "uncover_progress_mul" ) then
			local level = managers.player:upgrade_level( "player", "uncover_progress_mul" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "uncover_progress_mul", level )
		end
		if managers.player:has_category_upgrade( "player", "uncover_progress_decay_mul" ) then
			local level = managers.player:upgrade_level( "player", "uncover_progress_decay_mul" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "uncover_progress_decay_mul", level )
		end
		if managers.player:has_category_upgrade( "player", "civilian_gives_ammo" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "civilian_gives_ammo", 1 )
		end
		if managers.player:has_category_upgrade( "player", "drill_speed_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "drill_speed_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "drill_speed_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "saw_speed_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "saw_speed_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "saw_speed_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "camouflage_bonus" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "camouflage_bonus", 1 )
		end
		if managers.player:has_category_upgrade( "sentry_gun", "spread_multiplier" ) then
			managers.network:session():send_to_host( "sync_upgrade", "sentry_gun", "spread_multiplier", 1 )
		end
		if managers.player:has_category_upgrade( "sentry_gun", "rot_speed_multiplier" ) then
			managers.network:session():send_to_host( "sync_upgrade", "sentry_gun", "rot_speed_multiplier", 1 )
		end
		if managers.player:has_category_upgrade( "sentry_gun", "shield" ) then
			managers.network:session():send_to_host( "sync_upgrade", "sentry_gun", "shield", 1 )
		end
		if managers.player:has_category_upgrade( "ecm_jammer", "affects_cameras" ) then
			managers.network:session():send_to_host( "sync_upgrade", "ecm_jammer", "affects_cameras", 1 )
		end
		if managers.player:has_category_upgrade( "ecm_jammer", "feedback_duration_boost" ) then
			managers.network:session():send_to_host( "sync_upgrade", "ecm_jammer", "feedback_duration_boost", 1 )
		end
		if managers.player:has_category_upgrade( "ecm_jammer", "feedback_duration_boost_2" ) then
			managers.network:session():send_to_host( "sync_upgrade", "ecm_jammer", "feedback_duration_boost_2", 1 )
		end
		if managers.player:has_category_upgrade( "player", "convert_enemies_health_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "convert_enemies_health_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "convert_enemies_health_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "passive_convert_enemies_health_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "passive_convert_enemies_health_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "passive_convert_enemies_health_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "convert_enemies_damage_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "convert_enemies_damage_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "convert_enemies_damage_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "passive_convert_enemies_damage_multiplier" ) then
			local level = managers.player:upgrade_level( "player", "passive_convert_enemies_damage_multiplier" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "passive_convert_enemies_damage_multiplier", level )
		end
		if managers.player:has_category_upgrade( "player", "corpse_alarm_pager_bluff" ) then
			local level = managers.player:upgrade_level( "player", "corpse_alarm_pager_bluff" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "corpse_alarm_pager_bluff", level )
		end
		if managers.player:has_category_upgrade( "player", "civ_harmless_bullets" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "civ_harmless_bullets", 1 )
		end
		if managers.player:has_category_upgrade( "player", "civ_harmless_melee" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "civ_harmless_melee", 1 )
		end
		if managers.player:has_category_upgrade( "player", "civ_calming_alerts" ) then
			managers.network:session():send_to_host( "sync_upgrade", "player", "civ_calming_alerts", 1 )
		end
		if managers.player:has_category_upgrade( "player", "convert_enemies_max_minions" ) then
			local level = managers.player:upgrade_level( "player", "convert_enemies_max_minions" )
			managers.network:session():send_to_host( "sync_upgrade", "player", "convert_enemies_max_minions", level )
		end
	end
end

-----------------------------------------------------------------------------------

function PlayerBase:stats_screen_visible()
	return self._stats_screen_visible
end

function PlayerBase:set_stats_screen_visible( visible )
	self._stats_screen_visible = visible
	if self._stats_screen_visible then
		managers.hud:show_stats_screen()
	else
		managers.hud:hide_stats_screen()
	end
end

-----------------------------------------------------------------------------------


function PlayerBase:set_enabled( enabled )
	self._unit:set_extension_update_enabled( Idstring( "movement" ), enabled )
end

function PlayerBase:set_visible( visible )
	self._unit:set_visible( visible )
	self._unit:camera():camera_unit():set_visible( visible )
	if visible then
		self._unit:inventory():show_equipped_unit()
	else
		self._unit:inventory():hide_equipped_unit()
	end
end

-----------------------------------------------------------------------------------
function PlayerBase:_setup_hud()
	-- Loading of player huds are also done in waiting for players state
	if( not managers.hud:exists( self.PLAYER_HUD ) ) then
		managers.hud:load_hud( self.PLAYER_HUD, false, false, true, {} )
	end
	if( not managers.hud:exists( self.PLAYER_INFO_HUD_FULLSCREEN ) ) then
		managers.hud:load_hud( self.PLAYER_INFO_HUD_FULLSCREEN, false, false, false, {} )
	end
	if( not managers.hud:exists( self.PLAYER_INFO_HUD ) ) then
		managers.hud:load_hud( self.PLAYER_INFO_HUD, false, false, true, {} )
	end
	if( not managers.hud:exists( self.PLAYER_DOWNED_HUD ) ) then
		managers.hud:load_hud( self.PLAYER_DOWNED_HUD, false, false, true, {} )
	end
	-- managers.hud:_player_hud_layout()
	-- managers.hud:show( self.PLAYER_HUD )
	
	-- Add some test items
	-- managers.upgrades:aquire( "trip_mine", true )
	-- self._unit:equipment():add_equipment( { equipment = "trip_mine", amount = 4 } )
	-- self._unit:equipment():add_equipment( { equipment = "trip_mine" } )
	-- self._unit:equipment():add_equipment( { equipment = "ammo_bag", amount = 1 } )
	--- self._unit:equipment():add_equipment( { equipment = "flash_grenade", amount = 4 } )
	-- self._unit:equipment():add_equipment( { equipment = "smoke_grenade", amount = 4 } )
	-- self._unit:equipment():add_equipment( { equipment = "frag_grenade", amount = 4 } )
	-- self._unit:equipment():add_equipment( { equipment = "armour", amount = 8, icon = "equipment_armor" } )
	-- self._unit:equipment():add_equipment( { equipment = "grenade", amount = 3, icon = "guis/textures/equipment_thermite" } )
	
	-- self._unit:equipment():add_special( { name = "money_bag", silent = true } )
	-- managers.player:add_special( { name = "money_bag", silent = true } )
	-- self._unit:equipment():add_special( { name = "cable_tie", silent = true, amount = 2 } )
	
	-- Add test mugshots
	-- managers.hud:add_mugshot( { name = "SHADOWKILLER77" } )
	-- managers.hud:add_mugshot( { name = "FUTURECATCAR", weapon_icon = "guis/textures/weapon_beretta92" } )
	-- managers.hud:add_mugshot( { name = "1337K1NG", weapon_icon = "guis/textures/weapon_r870_shotgun" } )
end

-----------------------------------------------------------------------------------
--	This is going away once we fix a way to equip the player from the mission script
function PlayerBase:_equip_default_weapon()
end

-----------------------------------------------------------------------------------

function PlayerBase:post_init()
	self._unit:movement():post_init()
	self:_equip_default_weapon()
	if self._unit:movement():nav_tracker() then
		managers.groupai:state():register_criminal( self._unit )
	else
		self._unregistered = true
	end
	self._unit:character_damage():post_init()
	
	if Network:is_server() then
		local suspicion_mul, max_index = managers.blackmarket:get_suspicion_of_peer( Global.local_member:peer() )
		self:set_suspicion_multiplier( "equipment", suspicion_mul )
		self:set_detection_multiplier( "equipment", 1/suspicion_mul )
	end
	
end

-----------------------------------------------------------------------------------

function PlayerBase:_setup_controller()
	self._controller = managers.controller:create_controller( "player_" .. tostring( self._id ), nil, false )
	managers.rumble:register_controller( self._controller, self._rumble_pos_callback )
end

-----------------------------------------------------------------------------------

function PlayerBase:id()
	return self._id
end

-----------------------------------------------------------------------------------

function PlayerBase:nick_name()
	-- Should return a nick name really
	return Global.local_member:peer():name()
end

-----------------------------------------------------------------------------------

function PlayerBase:set_controller_enabled( enabled )
	if not self._controller then
		return
	end
	if not enabled then
		self._controller:set_enabled( false )
	end	
	self._wanted_controller_enabled = enabled
	
	if self._wanted_controller_enabled then
		self._wanted_controller_enabled_t = 1 -- messured in frames
		self._unit:set_extension_update_enabled( Idstring( "base" ), true )
	end
end

function PlayerBase:controller()
	return self._controller
end

-----------------------------------------------------------------------------------

function PlayerBase:anim_data_clbk_footstep( foot )
	
	local obj = self._unit:orientation_object()
	local proj_dir = math.UP
	local proj_from = obj:position()
	local proj_to = proj_from - proj_dir * 30
	
	--[[ No effects for this, yet, /Martin 100316	
	local walk_type = self._unit:anim_data().legs_state_jog and "_run" or "_walk"
 	local redir_name, pos, norm = World:pick_decal_effect( Idstring( "player_footstep" .. walk_type ), proj_from, proj_to, managers.slot:get_mask( "surface_move" ))
		
	local normal = norm or proj_dir
	local side_vec = self._unit:rotation():x():with_z( 0 )
	local up_vec = normal
	local fwd_vec = normal:cross( side_vec )
	local rot = Rotation( side_vec, fwd_vec, up_vec )
	
	if redir_name ~= Idstring( "" ) then
		World:effect_manager():spawn( { effect = redir_name, position = pos, rotation = rot } )
	else
		--	Benoit will rather that we do not play an effect if the mesh has no material specified
		--World:effect_manager():spawn( { effect = Idstring( "effects/particles/character/player/footstep_sand" .. walk_type ), position = proj_from, rotation = rot } )
	end]]
	
	local material_name, pos, norm = World:pick_decal_material( proj_from, proj_to, managers.slot:get_mask( "surface_move" ) )	
	self._unit:sound():play_footstep( foot, material_name )
end

-----------------------------------------------------------------------------------

function PlayerBase:get_rumble_position()
	return self._unit:position() + math.UP * 100
end

-----------------------------------------------------------------------------------

function PlayerBase:replenish()
	-- Replenish weapons
	for id,weapon in pairs( self._unit:inventory():available_selections() ) do
		if alive( weapon.unit ) then
			weapon.unit:base():replenish()
			-- managers.hud:set_weapon_ammo_by_unit( weapon.unit )
			managers.hud:set_ammo_amount( id, weapon.unit:base():ammo_info() )
		end
	end
	-- managers.hud:set_ammo_amount( self._unit:inventory():equipped_unit():base():selection_index(), self._unit:inventory():equipped_unit():base():ammo_info() )
	
	-- Replenish health
	self._unit:character_damage():replenish()
end

-------------------------------------------------------------------------------

function PlayerBase:suspicion_settings()
	return self._suspicion_settings
end

-------------------------------------------------------------------------------

function PlayerBase:detection_settings()
	return self._detection_settings
end

-------------------------------------------------------------------------------
-- buildup multiplier
function PlayerBase:set_suspicion_multiplier( reason, multiplier )
	if not Network:is_server() then
		return
	end
	
	self._suspicion_settings.multipliers[ reason ] = multiplier
	
	local buildup_mul = self._suspicion_settings.init_buildup_mul
	local range_mul = self._suspicion_settings.init_range_mul
	for reason, mul in pairs( self._suspicion_settings.multipliers ) do
		buildup_mul = buildup_mul * mul
		--range_mul = range_mul * mul -- It doesn't work too well to shrink the range. the player does not get enough hint
	end
	self._suspicion_settings.buildup_mul = buildup_mul
	self._suspicion_settings.range_mul = range_mul
end

-------------------------------------------------------------------------------
-- delay multiplier
function PlayerBase:set_detection_multiplier( reason, multiplier )
	if not Network:is_server() then
		return
	end
	
	self._detection_settings.multipliers[ reason ] = multiplier
	
	local delay_mul = self._detection_settings.init_delay_mul
	local range_mul = self._detection_settings.init_range_mul
	for reason, mul in pairs( self._detection_settings.multipliers ) do
		delay_mul = delay_mul * mul
		range_mul = range_mul * mul
	end
	self._detection_settings.delay_mul = delay_mul
	self._detection_settings.range_mul = range_mul
end

-------------------------------------------------------------------------------

function PlayerBase:arrest_settings()
	return tweak_data.player.arrest
end

-----------------------------------------------------------------------------------

function PlayerBase:_unregister()
	if not self._unregistered then
		self._unit:movement():attention_handler():set_attention( nil )
		
		managers.groupai:state():unregister_criminal( self._unit )
		self._unregistered = true
	end
end

-----------------------------------------------------------------------------------

function PlayerBase:pre_destroy( unit )
	self:_unregister()
	UnitBase.pre_destroy( self, unit )
	managers.player:player_destroyed( self._id )
	if self._controller then
		managers.rumble:unregister_controller( self._controller, self._rumble_pos_callback )
		self._controller:destroy()
		self._controller = nil
	end
	if managers.hud:alive( self.PLAYER_HUD ) then
		managers.hud:clear_weapons()
		managers.hud:hide( self.PLAYER_HUD )
		-- managers.hud:clear_mugshots()
	end
	self:set_stats_screen_visible( false )
	if Global.local_member then
		Global.local_member:set_unit( nil )
	end
	unit:movement():pre_destroy( unit )
	unit:inventory():pre_destroy( unit )
	unit:character_damage():pre_destroy()
end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------