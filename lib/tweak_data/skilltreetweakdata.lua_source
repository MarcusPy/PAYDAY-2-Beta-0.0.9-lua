SkillTreeTweakData = SkillTreeTweakData or class()

function SkillTreeTweakData:init()					-- IF ANY CHANGES IS MADE TO THE SKILLTREE, SkillTreeManager.VERSION NEEDS TO BE INCREASED
	-- Skills gives upgrades. 
	
	-- self.tier_unlocks = { 1, 4, 7, 10, 13, 16 } -- Points spent to unlock tiers
	self.tier_unlocks = { 1, 5, 10, 20, 30, 40 } -- Points spent to unlock tiers
	
	self.costs = { unlock_tree = 1, default = 1, pro = 3, hightier = 4, hightierpro = 8 } -- Points cost
	
	
	--[[ 
	------------------------------------------- UNUSED UPGRADES -------------------------------------------
	
		
		saw_fire_rate_multiplier_1
		saw_fire_rate_multiplier_2
		
		saw_recoil_multiplier
		
		passive_player_xp_multiplier
		
		player_wolverine_health_regen
		cable_tie_quantity_unlimited
		
		temporary_combat_medic_enter_steelsight_speed_multiplier
		team_pistol_suppression_recoil_multiplier
		team_pistol_recoil_multiplier
		
		weapon_passive_reload_speed_multiplier
		
		player_intimidate_specials
		
		player_intimidation_multiplier
		pistol_exit_run_speed_multiplier
		
		weapon_swap_speed_multiplier
		
		player_morale_boost
		
		player_pistol_revive_from_bleed_out
		
		
		
		player_passive_armor_multiplier_2
		
		trip_mine_explode_timer_delay
		
		sentry_gun_extra_ammo_multiplier_2
		
		player_health_multiplier
		
		player_electrocution_resistance
		ecm_jammer_quantity_increase_2
		
		"player_civ_harmless_bullets"
		"player_civ_harmless_melee"
		
		weapon_fire_rate_multiplier
		weapon_spread_multiplier
		
		trip_mine_quantity_increase_2
		
		"cable_tie_can_cable_tie_doors"
		
		"player_convert_enemies_max_minions_2", 
		
		"player_additional_lives_2"
	]]
	
	
	--[[ 
	---------------------- DEFAULT UPGRADES, PlayerManager:aquire_default_upgrades() ----------------------
		
		cable_tie
		player_special_enemy_highlight
		player_hostage_trade
		DISABLED IN PLAYER DAMAGE:	temporary_revive_health_boost		-- needs to be in default so that player A can activate it when player B (who got 'player_revive_health_boost' upgrade) revives player A
	]]
	
	
	--[[ 
	-------------------------------------- NON IMPLEMENTED UPGRADES ---------------------------------------
		
		
		player_uncover_progress_decay_mul
		player_uncover_progress_mul
		player_primary_weapon_when_carrying		-- should be removed from upgrade_tweak_data
	]]
	
	
	-- All skills					-- IF ANY CHANGES IS MADE TO THE SKILLTREE, SkillTreeManager.VERSION NEEDS TO BE INCREASED
	self.skills = {}
	
	self.skills[ "mastermind" ] = {									-- This is a skill that gives an upgrade
								name_id = "menu_mastermind",				-- Name of skill
								desc_id = "menu_mastermind_desc",		-- Description of skill
								icon_xy = { 2, 7 },
								[1] = { upgrades = { "doctor_bag", "passive_doctor_bag_interaction_speed_multiplier" }, cost = self.costs.unlock_tree, desc_id="menu_mastermind_tier_1" },
								[2] = { upgrades = { "team_passive_stamina_multiplier_1" }, desc_id="menu_mastermind_tier_2" },
								[3] = { upgrades = { "player_passive_intimidate_range_mul" }, desc_id="menu_mastermind_tier_3" },
								[4] = { upgrades = { "player_passive_convert_enemies_health_multiplier", "player_passive_convert_enemies_damage_multiplier" }, desc_id="menu_mastermind_tier_4" },
								[5] = { upgrades = { "player_convert_enemies_interaction_speed_multiplier" }, desc_id="menu_mastermind_tier_5" },
								[6] = { upgrades = { "player_passive_empowered_intimidation", "passive_player_assets_cost_multiplier" }, desc_id="menu_mastermind_tier_6" },
								}
	self.skills[ "enforcer" ] =  {									-- This is a skill that gives an upgrade
								name_id = "menu_enforcer",				-- Name of skill
								desc_id = "menu_enforcer_desc",		-- Description of skill
								icon_xy = { 1, 0 },
								[1] = { upgrades = { "ammo_bag", "player_passive_suppression_bonus_1" }, cost = self.costs.unlock_tree, desc_id="menu_menu_enforcer_tier_1" },
								[2] = { upgrades = { "player_passive_health_multiplier_1" }, desc_id="menu_menu_enforcer_tier_2" },
								[3] = { upgrades = { "player_passive_suppression_bonus_2" }, desc_id="menu_menu_enforcer_tier_3" },
								[4] = { upgrades = { "player_passive_health_multiplier_2" }, desc_id="menu_menu_enforcer_tier_4" },
								[5] = { upgrades = { "weapon_passive_damage_multiplier" }, desc_id="menu_menu_enforcer_tier_5" },
								[6] = { upgrades = { "player_passive_health_multiplier_3" }, desc_id="menu_menu_enforcer_tier_6" },	
								}
	self.skills[ "technician" ] =  {									-- This is a skill that gives an upgrade
								name_id = "menu_technician",				-- Name of skill
								desc_id = "menu_technician_desc",		-- Description of skill
								icon_xy = { 7, 4 },
								[1] = { upgrades = { "trip_mine", "player_passive_crafting_weapon_multiplier_1", "player_passive_crafting_mask_multiplier_1" }, cost = self.costs.unlock_tree, desc_id="menu_technician_tier_1" },
								[2] = { upgrades = { "weapon_passive_recoil_multiplier_1" }, desc_id="menu_technician_tier_2" },
								[3] = { upgrades = { "player_passive_crafting_weapon_multiplier_2", "player_passive_crafting_mask_multiplier_2" }, desc_id="menu_technician_tier_3" },
								[4] = { upgrades = { "weapon_passive_headshot_damage_multiplier" }, desc_id="menu_technician_tier_4" },
								[5] = { upgrades = { "player_passive_crafting_weapon_multiplier_3", "player_passive_crafting_mask_multiplier_3" }, desc_id="menu_technician_tier_5" },
								[6] = { upgrades = { "weapon_passive_recoil_multiplier_2", "player_passive_armor_multiplier_1", "team_passive_armor_regen_time_multiplier" }, desc_id="menu_technician_tier_6" },
								}
	self.skills[ "ghost" ] =  {									-- This is a skill that gives an upgrade
								name_id = "menu_ghost",				-- Name of skill
								desc_id = "menu_ghost_desc",		-- Description of skill
								icon_xy = { 1, 4 },
								[1] = { upgrades = { "ecm_jammer", "ecm_jammer_affects_cameras", "player_passive_dodge_chance_1" }, cost = self.costs.unlock_tree, desc_id="menu_ghost_tier_1" },
								[2] = { upgrades = { "weapon_passive_swap_speed_multiplier_1" }, desc_id="menu_ghost_tier_2" },
								[3] = { upgrades = { "player_passive_dodge_chance_2" }, desc_id="menu_ghost_tier_3" },
								[4] = { upgrades = { "player_passive_suspicion_bonus" }, desc_id="menu_ghost_tier_4" },
								[5] = { upgrades = { "weapon_passive_swap_speed_multiplier_2" }, desc_id="menu_ghost_tier_5" },
								[6] = { upgrades = { "player_passive_loot_drop_multiplier" }, desc_id="menu_ghost_tier_6" },
								}
	
	
	-- Mastermind Skills
	
	self.skills[ "black_marketeer" ] = {	-- tier 4
									name_id = "menu_black_marketeer",
									desc_id = "menu_black_marketeer_desc",		-- Description of skill
									icon_xy = { 4, 8 },
									[1] = { upgrades = { "player_buy_cost_multiplier_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_buy_cost_multiplier_2", "player_sell_cost_multiplier_1" }, cost = self.costs.hightierpro },
								}
	self.skills[ "gun_fighter" ] = {	-- tier 5
									name_id = "menu_gun_fighter",
									desc_id = "menu_gun_fighter_desc",		-- Description of skill
									icon_xy = { 0, 9 },
									[1] = { upgrades = { "pistol_reload_speed_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "pistol_damage_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "combat_medic" ] = {	-- tier 1
									name_id = "menu_combat_medic",
									desc_id = "menu_combat_medic_desc",		-- Description of skill
									icon_xy = { 5, 7 },
									[1] = { upgrades = { "temporary_combat_medic_damage_multiplier1" }, cost = self.costs.default },
									[2] = { upgrades = { "player_revive_health_boost" }, cost = self.costs.pro },
								}
	self.skills[ "control_freak" ] = {	-- tier 6
									name_id = "menu_control_freak",
									desc_id = "menu_control_freak_desc",		-- Description of skill
									icon_xy = { 6, 7 },
									[1] = { upgrades = { "player_civ_calming_alerts" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_civ_intimidation_mul" }, cost = self.costs.hightierpro },
								}
	self.skills[ "leadership" ] = {	-- tier 2
									name_id = "menu_leadership",
									desc_id = "menu_leadership_desc",		-- Description of skill
									icon_xy = { 7, 7 },
									[1] = { upgrades = { "team_pistol_recoil_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "team_weapon_recoil_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "inside_man" ] = {	-- tier 2
									name_id = "menu_inside_man",
									desc_id = "menu_inside_man_desc",		-- Description of skill
									icon_xy = { 0, 8 },
									[1] = { upgrades = { "player_assets_cost_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_additional_assets" }, cost = self.costs.pro },
								}
	self.skills[ "target_mark" ] = {
									name_id = "menu_target_mark",
									desc_id = "menu_target_mark_desc",		-- Description of skill
									icon_xy = { 3, 7 },
									[1] = { upgrades = {  }, cost = self.costs.default },
									[2] = { cost = self.costs.pro },
								}
	self.skills[ "dominator" ] = {	-- tier 3
									name_id = "menu_dominator",
									desc_id = "menu_dominator_desc",		-- Description of skill
									icon_xy = { 2, 8 },
									[1] = { upgrades = { "player_intimidate_enemies" }, cost = self.costs.default },
									[2] = { upgrades = { "player_intimidate_range_mul", "player_intimidate_aura" }, cost = self.costs.pro },
								}
	self.skills[ "fast_learner" ] = {	-- tier 2
									name_id = "menu_fast_learner",
									desc_id = "menu_fast_learner_desc",		-- Description of skill
									icon_xy = { 7, 8 },
									[1] = { upgrades = { "player_xp_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "team_xp_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "stockholm_syndrome" ] = {	-- tier 3
									name_id = "menu_stockholm_syndrome",
									desc_id = "menu_stockholm_syndrome_desc",		-- Description of skill
									icon_xy = { 3, 8 },
									[1] = { upgrades = { "player_civilian_reviver" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_civilian_gives_ammo" }, cost = self.costs.hightierpro },
								}
	self.skills[ "cable_guy" ] = {	-- tier 1
									name_id = "menu_cable_guy",
									desc_id = "menu_cable_guy_desc",		-- Description of skill
									icon_xy = { 4, 7 },
									[1] = { upgrades = { "cable_tie_interact_speed_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "cable_tie_quantity" }, cost = self.costs.pro },
								}
	self.skills[ "tactician" ] = {	-- tier 3
									name_id = "menu_tactician",
									desc_id = "menu_tactician_desc",		-- Description of skill
									icon_xy = { 3, 7 },
									[1] = { upgrades = { "player_corpse_alarm_pager_bluff" }, cost = self.costs.default },
									[2] = { upgrades = { "player_marked_enemy_extra_damage" }, cost = self.costs.pro },
								}
	self.skills[ "triathlete" ] = {	-- tier 1
									name_id = "menu_triathlete",
									desc_id = "menu_triathlete_desc",		-- Description of skill
									icon_xy = { 1, 8 },
									[1] = { upgrades = { "player_stamina_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "team_stamina_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "kilmer" ] = {	-- tier 5
									name_id = "menu_kilmer",
									desc_id = "menu_kilmer_desc",		-- Description of skill
									icon_xy = { 1, 9 },
									[1] = { upgrades = { "assault_rifle_reload_speed_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "assault_rifle_move_spread_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "equilibrium" ] = {	-- tier 5
									name_id = "menu_equilibrium",
									desc_id = "menu_equilibrium_desc",		-- Description of skill
									icon_xy = { 3, 9 },
									[1] = { upgrades = { "pistol_spread_multiplier", "pistol_swap_speed_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "pistol_fire_rate_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "negotiator" ] = {
									name_id = "menu_negotiator",
									desc_id = "menu_negotiator_desc",		-- Description of skill
									icon_xy = { 7, 8 },
									[1] = { cost = self.costs.hightier },
									[2] = { cost = self.costs.hightierpro },
								}
	self.skills[ "medic_2x" ] = {	-- tier 4
									name_id = "menu_medic_2x",
									desc_id = "menu_medic_2x_desc",		-- Description of skill
									icon_xy = { 5, 8 },
									[1] = { upgrades = { "doctor_bag_quantity" }, cost = self.costs.hightier },
									[2] = { upgrades = { "doctor_bag_amount_increase1" }, cost = self.costs.hightierpro },
								}
	self.skills[ "joker" ] = {	-- tier 4
									name_id = "menu_joker",
									desc_id = "menu_joker_desc",		-- Description of skill
									icon_xy = { 6, 8 },
									prerequisites = { "dominator" },
									[1] = { upgrades = { "player_convert_enemies", "player_convert_enemies_max_minions_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_convert_enemies_health_multiplier", "player_convert_enemies_damage_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "inspire" ] = {	-- tier 6
									name_id = "menu_inspire",
									desc_id = "menu_inspire_desc",		-- Description of skill
									icon_xy = { 4, 9 },
									[1] = { upgrades = { "player_revive_interaction_speed_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_long_dis_revive" }, cost = self.costs.hightierpro },
								}
	self.skills[ "pistol_messiah" ] = {	-- tier 6
									name_id = "menu_pistol_messiah",
									desc_id = "menu_pistol_messiah_desc",		-- Description of skill
									icon_xy = { 2, 9 },
									[1] = { upgrades = { "player_pistol_revive_from_bleed_out_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_pistol_revive_from_bleed_out_2" }, cost = self.costs.hightierpro },
										
								}
								
	-- Enforcer Skills
		
		
	self.skills[ "ammo_reservoir" ] = {	-- tier 1
									name_id = "menu_ammo_reservoir",
									desc_id = "menu_ammo_reservoir_desc",		-- Description of skill
									icon_xy = { 4, 5 },
									[1] = { upgrades = { "temporary_no_ammo_cost_1" }, cost = self.costs.default },
									[2] = { upgrades = { "temporary_no_ammo_cost_2" }, cost = self.costs.pro },
								}
	self.skills[ "demolition_man" ] = {
									name_id = "menu_demolition_man",
									desc_id = "menu_demolition_man_desc",		-- Description of skill
									icon_xy = { 4, 5 },
									[1] = { upgrades = { }, cost = self.costs.default },
									[2] = { upgrades = { }, cost = self.costs.pro },
								}
	self.skills[ "oppressor" ] = {	-- tier 1
									name_id = "menu_oppressor",
									desc_id = "menu_oppressor_desc",		-- Description of skill
									icon_xy = { 7, 0 },
									[1] = { upgrades = { "player_suppression_bonus" }, cost = self.costs.default },
									[2] = { upgrades = { "player_suppression_mul_2"}, cost = self.costs.pro },
								}
	self.skills[ "steroids" ] = {	-- tier 2
									name_id = "menu_steroids",
									desc_id = "menu_steroids_desc",		-- Description of skill
									icon_xy = { 4, 0 },
									[1] = { upgrades = { "player_non_special_melee_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_melee_damage_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "bandoliers" ] = {	-- tier 3
									name_id = "menu_bandoliers",
									desc_id = "menu_bandoliers_desc",		-- Description of skill
									icon_xy = { 3, 0 },
									[1] = { upgrades = { "extra_ammo_multiplier1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_pick_up_ammo_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "pack_mule" ] = {	-- tier 1
									name_id = "menu_pack_mule",
									desc_id = "menu_pack_mule_desc",		-- Description of skill
									icon_xy = { 6, 0 },
									[1] = { upgrades = { "carry_movement_speed_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "carry_throw_distance_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "shotgun_impact" ] = {	-- tier 3
									name_id = "menu_shotgun_impact",
									desc_id = "menu_shotgun_impact_desc",		-- Description of skill
									icon_xy = { 5, 0 },
									[1] = { upgrades = { "shotgun_recoil_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "shotgun_damage_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "portable_saw" ] = {	-- tier 4
									name_id = "menu_portable_saw",
									desc_id = "menu_portable_saw_desc",		-- Description of skill
									icon_xy = { 0, 1 },
									[1] = { upgrades = { "saw" }, cost = self.costs.hightier },
									[2] = { upgrades = { "saw_extra_ammo_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "tough_guy" ] = {	-- tier 3
									name_id = "menu_tough_guy",
									desc_id = "menu_tough_guy_desc",		-- Description of skill
									icon_xy = { 1, 1 },
									[1] = { upgrades = { "player_damage_shake_multiplier"}, cost = self.costs.default },
									[2] = { upgrades = { "player_bleed_out_health_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "underdog" ] = {	-- tier 2
									name_id = "menu_underdog",
									desc_id = "menu_underdog_desc",		-- Description of skill
									icon_xy = { 2, 1 },
									[1] = { upgrades = { "player_damage_multiplier_outnumbered" }, cost = self.costs.default },
									[2] = { upgrades = { "player_damage_dampener_outnumbered" }, cost = self.costs.pro },
								}
	self.skills[ "juggernaut" ] = {	-- tier 6
									name_id = "menu_juggernaut",
									desc_id = "menu_juggernaut_desc",		-- Description of skill
									icon_xy = { 3, 1 },
									[1] = { upgrades = { "body_armor6" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_shield_knock" }, cost = self.costs.hightierpro },  
								}
	self.skills[ "from_the_hip" ] = {	-- tier 5
									name_id = "menu_from_the_hip",
									desc_id = "menu_from_the_hip_desc",		-- Description of skill
									icon_xy = { 4, 1 },
									[1] = { upgrades = { "shotgun_hip_fire_spread_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "assault_rifle_hip_fire_spread_multiplier", "saw_hip_fire_spread_multiplier", "pistol_hip_fire_spread_multiplier", "smg_hip_fire_spread_multiplier" }, cost = self.costs.hightierpro },			-- add more primary weapons here, if they exists
								}
	self.skills[ "shotgun_cqb" ] = {	-- tier 4
									name_id = "menu_shotgun_cqb",
									desc_id = "menu_shotgun_cqb_desc",		-- Description of skill
									icon_xy = { 5, 1 },
									[1] = { upgrades = { "shotgun_reload_speed_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "shotgun_enter_steelsight_speed_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "shades" ] = {	-- tier 5
									name_id = "menu_shades",
									desc_id = "menu_shades_desc",		-- Description of skill
									icon_xy = { 6, 1 },
									[1] = { upgrades = { "player_flashbang_multiplier_1" }, cost = self.costs.default }, 
									[2] = { upgrades = { "player_flashbang_multiplier_2" }, cost = self.costs.pro },
								}
	self.skills[ "ammo_2x" ] = {	-- tier 4
									name_id = "menu_ammo_2x",
									desc_id = "menu_ammo_2x_desc",		-- Description of skill
									icon_xy = { 7, 1 },
									[1] = { upgrades = { "ammo_bag_quantity"  }, cost = self.costs.hightier },
									[2] = { upgrades = { "ammo_bag_ammo_increase1"  }, cost = self.costs.hightierpro }, 
								}
	self.skills[ "carbon_blade" ] = {	-- tier 5
									name_id = "menu_carbon_blade",
									desc_id = "menu_carbon_blade_desc",		-- Description of skill
									icon_xy = { 0, 2 },
									prerequisites = { "portable_saw" },
									[1] = { upgrades = { "player_saw_speed_multiplier_1", "saw_lock_damage_multiplier_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_saw_speed_multiplier_2", "saw_lock_damage_multiplier_2", "saw_enemy_slicer" }, cost = self.costs.hightierpro },
								}
	self.skills[ "show_of_force" ] = {	-- tier 2
									name_id = "menu_show_of_force",
									desc_id = "menu_show_of_force_desc",		-- Description of skill
									icon_xy = { 1, 2 },
									[1] = { upgrades = { "player_primary_weapon_when_downed" }, cost = self.costs.default },
									[2] = { upgrades = { "player_armor_regen_timer_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "wolverine" ] = {	-- tier 6
									name_id = "menu_wolverine",
									desc_id = "menu_wolverine_desc",		-- Description of skill
									icon_xy = { 2, 2 },
									[1] = { upgrades = { "player_melee_damage_health_ratio_multiplier"  }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_damage_health_ratio_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "overkill" ] = {	-- tier 6
									name_id = "menu_overkill",
									desc_id = "menu_overkill_desc",		-- Description of skill
									icon_xy = { 3, 2 },
									[1] = { upgrades = { "player_overkill_damage_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_overkill_all_weapons" }, cost = self.costs.hightierpro },
								}
								
	-- Technician Skills
	self.skills[ "mag_plus" ] = {	-- tier 6
									name_id = "menu_mag_plus",
									desc_id = "menu_mag_plus_desc",		-- Description of skill
									icon_xy = { 2, 0 },
									[1] = { upgrades = { "weapon_clip_ammo_increase_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "weapon_clip_ammo_increase_2" }, cost = self.costs.hightierpro },
								}
	self.skills[ "iron_man" ] = {	-- tier 6
									name_id = "menu_iron_man",
									desc_id = "menu_iron_man_desc",		-- Description of skill
									icon_xy = { 6, 4 },
									[1] = { upgrades = { "player_armor_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "team_armor_regen_time_multiplier" }, cost = self.costs.hightierpro },
								}
	
	self.skills[ "rifleman" ] = {	-- tier 1
									name_id = "menu_rifleman",
									desc_id = "menu_rifleman_desc",		-- Description of skill
									icon_xy = { 0, 5 },
									[1] = { upgrades = { "assault_rifle_enter_steelsight_speed_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "assault_rifle_zoom_increase" }, cost = self.costs.pro },
								}
	self.skills[ "blast_radius" ] = {	-- tier 5
									name_id = "menu_blast_radius",
									desc_id = "menu_blast_radius_desc",		-- Description of skill
									icon_xy = { 1, 5 },
									[1] = { upgrades = { "trip_mine_explosion_size_multiplier_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "trip_mine_explosion_size_multiplier_2" }, cost = self.costs.hightierpro },
								}
	self.skills[ "insulation" ] = {	-- tier 3
									name_id = "menu_insulation",
									desc_id = "menu_insulation_desc",		-- Description of skill
									icon_xy = { 3, 5 },
									[1] = { upgrades = { "player_taser_malfunction" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_taser_self_shock" }, cost = self.costs.hightierpro },
								}
	self.skills[ "hardware_expert" ] = {	-- tier 2
									name_id = "menu_hardware_expert",
									desc_id = "menu_hardware_expert_desc",		-- Description of skill
									icon_xy = { 5, 5 },
									[1] = { upgrades = { "player_drill_fix_interaction_speed_multiplier", "player_trip_mine_deploy_time_multiplier_2" }, cost = self.costs.default },
									[2] = { upgrades = { "player_drill_autorepair", "player_sentry_gun_deploy_time_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "trip_mine_expert" ] = {	-- tier 2
									name_id = "menu_trip_mine_expert",
									desc_id = "menu_trip_mine_expert_desc",		-- Description of skill
									icon_xy = { 4, 6 },
									[1] = { upgrades = { "trip_mine_can_switch_on_off" }, cost = self.costs.default },
									[2] = { upgrades = { "trip_mine_sensor_toggle" }, cost = self.costs.pro },
								}

	self.skills[ "sharpshooter" ] = {	-- tier 3
									name_id = "menu_sharpshooter",
									desc_id = "menu_sharpshooter_desc",		-- Description of skill
									icon_xy = { 6, 5 },
									[1] = { upgrades = { "weapon_single_spread_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "assault_rifle_recoil_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "sentry_gun" ] = {	-- tier 3
									name_id = "menu_sentry_gun",
									desc_id = "menu_sentry_gun_desc",		-- Description of skill
									icon_xy = { 7, 5 },
									[1] = { upgrades = { "sentry_gun" }, cost = self.costs.default },
									[2] = { upgrades = { "sentry_gun_armor_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "sentry_targeting_package" ] = {	-- tier 4
									name_id = "menu_sentry_targeting_package",
									desc_id = "menu_sentry_targeting_package_desc",		-- Description of skill
									icon_xy = { 1, 6 },
									prerequisites = { "sentry_gun" },
									[1] = { upgrades = { "sentry_gun_spread_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "sentry_gun_rot_speed_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "sentry_2_0" ] = {	-- tier 5
									name_id = "menu_sentry_2_0",
									desc_id = "menu_sentry_2_0_desc",		-- Description of skill
									icon_xy = { 5, 6 },
									prerequisites = { "sentry_gun" },
									[1] = { upgrades = { "sentry_gun_extra_ammo_multiplier_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "sentry_gun_shield" }, cost = self.costs.hightierpro },
								}
	self.skills[ "drill_expert" ] = {	-- tier 2
									name_id = "menu_drill_expert",
									desc_id = "menu_drill_expert_desc",		-- Description of skill
									icon_xy = { 3, 6 },
									[1] = { upgrades = { "player_drill_speed_multiplier1" }, cost = self.costs.default },
									[2] = { upgrades = { "player_drill_speed_multiplier2" }, cost = self.costs.pro },
								}
	self.skills[ "military_grade" ] = {
									name_id = "menu_military_grade",
									desc_id = "menu_military_grade_desc",		-- Description of skill
									icon_xy = { 4, 6 },
									[1] = { upgrades = { "trip_mine_quantity_increase_2" }, cost = self.costs.hightier },
									[2] = { upgrades = {  }, cost = self.costs.hightierpro },
								}
	self.skills[ "dye_pack_removal" ] = {
									name_id = "menu_dye_pack_removal",
									desc_id = "menu_dye_pack_removal_desc",		-- Description of skill
									icon_xy = { 0, 6 },
									[1] = { upgrades = { "player_dye_pack_chance_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_dye_pack_cash_loss_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "silent_drilling" ] = {	-- tier 4
									name_id = "menu_silent_drilling",
									desc_id = "menu_silent_drilling_desc",		-- Description of skill
									icon_xy = { 2, 6 },
									[1] = { upgrades = { "player_drill_alert" }, cost = self.costs.hightier},
									[2] = { upgrades = { "player_silent_drill" }, cost = self.costs.hightierpro },
								}
	self.skills[ "discipline" ] = {	-- tier 4
									name_id = "menu_discipline",
									desc_id = "menu_discipline_desc",		-- Description of skill
									icon_xy = { 6, 6 },
									[1] = { upgrades = { "player_interacting_damage_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_steelsight_when_downed" }, cost = self.costs.pro },
								}
	self.skills[ "trip_miner" ] = {	-- tier 1
									name_id = "menu_trip_miner",
									desc_id = "menu_trip_miner_desc",		-- Description of skill
									icon_xy = { 2, 5 },
									[1] = { upgrades = { "trip_mine_quantity_increase_1" }, cost = self.costs.default },
									[2] = { upgrades = { "player_trip_mine_deploy_time_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "shaped_charge" ] = {	-- tier 5
									name_id = "menu_shaped_charge",
									desc_id = "menu_shaped_charge_desc",		-- Description of skill
									icon_xy = { 0, 7 },
									[1] = { upgrades = { "trip_mine_quantity_increase_3" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_trip_mine_shaped_charge" }, cost = self.costs.hightierpro },
								}
	self.skills[ "master_craftsman" ] = {	-- tier 1
									name_id = "menu_master_craftsman",
									desc_id = "menu_master_craftsman_desc",		-- Description of skill
									icon_xy = { 1, 7 },
									[1] = { upgrades = { "player_crafting_weapon_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_crafting_mask_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "sentry_gun_2x" ] = {	-- tier 6
									name_id = "menu_sentry_gun_2x",
									desc_id = "menu_sentry_gun_2x_desc",		-- Description of skill
									icon_xy = { 7, 6 },
									prerequisites = { "sentry_gun" },
									[1] = { upgrades = { "sentry_gun_quantity_increase" }, cost = self.costs.hightier },
									[2] = { upgrades = { "sentry_gun_damage_multiplier" }, cost = self.costs.hightierpro },
								}
								
	-- Ghost Skills -- Thief Skills
	
	self.skills[ "ecm_booster" ] = {	-- tier 5
									name_id = "menu_ecm_booster",
									desc_id = "menu_ecm_booster_desc",		-- Description of skill
									icon_xy = { 6, 3 },
									[1] = { upgrades = { "ecm_jammer_duration_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "ecm_jammer_can_open_sec_doors" }, cost = self.costs.hightierpro },
								}
	self.skills[ "sprinter" ] = {	-- tier 1
									name_id = "menu_sprinter",
									desc_id = "menu_sprinter_desc",		-- Description of skill
									icon_xy = { 7, 3 },
									[1] = { upgrades = { "player_stamina_regen_timer_multiplier", "player_stamina_regen_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_run_dodge_chance", "player_run_speed_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "smg_training" ] = {
									name_id = "menu_smg_training",
									desc_id = "menu_smg_training_desc",		-- Description of skill
									icon_xy = { 3, 3 },
									[1] = { upgrades = { "smg_reload_speed_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "smg_recoil_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "smg_master" ] = {	-- tier 3
									name_id = "menu_smg_master",
									desc_id = "menu_smg_master_desc",		-- Description of skill
									icon_xy = { 3, 3 },
									[1] = { upgrades = { "smg_reload_speed_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "smg_recoil_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "transporter" ] = {	-- tier 2
									name_id = "menu_transporter",
									desc_id = "menu_transporter_desc",		-- Description of skill
									icon_xy = { 4, 3 },
									[1] = { upgrades = { "carry_interact_speed_multiplier_1" }, cost = self.costs.default },
									[2] = { upgrades = { "carry_interact_speed_multiplier_2" }, cost = self.costs.pro },
								}
	self.skills[ "cat_burglar" ] = {	-- tier 1
									name_id = "menu_cat_burglar",
									desc_id = "menu_cat_burglar_desc",		-- Description of skill
									icon_xy = { 0, 4 },
									[1] = { upgrades = { "player_fall_damage_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_fall_health_damage_multiplier", "player_respawn_time_multiplier" }, cost = self.costs.pro },
								}
	self.skills[ "chameleon" ] = {	-- tier 2
									name_id = "menu_chameleon",
									desc_id = "menu_chameleon_desc",		-- Description of skill
									icon_xy = { 5, 3 },
									[1] = { upgrades = { "player_suspicion_bonus" }, cost = self.costs.default },
									[2] = { upgrades = { "player_camouflage_bonus" }, cost = self.costs.pro },
									
								
								}
	self.skills[ "cleaner" ] = {	-- tier 2
									name_id = "menu_cleaner",
									desc_id = "menu_cleaner_desc",		-- Description of skill
									icon_xy = { 7, 2 },
									[1] = { upgrades = { "weapon_special_damage_taken_multiplier" }, cost = self.costs.default }, 
									[2] = { upgrades = { "player_corpse_dispose" }, cost = self.costs.pro },
								}
	self.skills[ "ecm_2x" ] = {	-- tier 6
									name_id = "menu_ecm_2x",
									desc_id = "menu_ecm_2x_desc",		-- Description of skill
									icon_xy = { 3, 4 },
									[1] = { upgrades = { "ecm_jammer_quantity_increase_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "ecm_jammer_duration_multiplier_2", "ecm_jammer_feedback_duration_boost_2" }, cost = self.costs.hightierpro },
								}
	self.skills[ "assassin" ] = {	-- tier 3
									name_id = "menu_assassin",
									desc_id = "menu_assassin_desc",		-- Description of skill
									icon_xy = { 0, 3 },
									[1] = { upgrades = { "player_walk_speed_multiplier", "player_crouch_speed_multiplier" }, cost = self.costs.default },
									[2] = { upgrades = { "player_silent_kill" }, cost = self.costs.pro },
								}
	self.skills[ "martial_arts" ] = {	-- tier 3
									name_id = "menu_martial_arts",
									desc_id = "menu_martial_arts_desc",		-- Description of skill
									icon_xy = { 1, 3 },
									[1] = { upgrades = { "player_melee_knockdown_mul" }, cost = self.costs.default },
									[2] = { upgrades = { "player_damage_dampener" }, cost = self.costs.pro },
									
								}
	self.skills[ "nine_lives" ] = {	-- tier 4
									name_id = "menu_nine_lives",
									desc_id = "menu_nine_lives_desc",		-- Description of skill
									icon_xy = { 5, 2 },
									[1] = { upgrades = { "player_additional_lives_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_cheat_death_chance" }, cost = self.costs.hightierpro },
								}
	self.skills[ "ecm_feedback" ] = {	-- tier 4
									name_id = "menu_ecm_feedback",
									desc_id = "menu_ecm_feedback_desc",		-- Description of skill
									icon_xy = { 6, 2 },
									[1] = { upgrades = { "ecm_jammer_can_activate_feedback" }, cost = self.costs.hightier },
									[2] = { upgrades = { "ecm_jammer_feedback_duration_boost" }, cost = self.costs.hightierpro },
								}
	self.skills[ "moving_target" ] = {	-- tier 6
									name_id = "menu_moving_target",
									desc_id = "menu_moving_target_desc",		-- Description of skill
									icon_xy = { 2, 4 },
									[1] = { upgrades = { "player_can_strafe_run" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_can_free_run" }, cost = self.costs.hightierpro },
								}
	self.skills[ "scavenger" ] = {	-- tier 1
									name_id = "menu_scavenger",
									desc_id = "menu_scavenger_desc",		-- Description of skill
									icon_xy = { 2, 3 },
									[1] = { upgrades = { "player_small_loot_multiplier1" }, cost = self.costs.default },
									[2] = { upgrades = { "player_small_loot_multiplier2" }, cost = self.costs.pro },
								}
	self.skills[ "hitman" ] = {	-- tier 4
									name_id = "menu_hitman",
									desc_id = "menu_hitman_desc",		-- Description of skill
									icon_xy = { 5, 9 },
									[1] = { upgrades = { "weapon_silencer_damage_multiplier_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "weapon_silencer_damage_multiplier_2" }, cost = self.costs.hightierpro },
								}
	self.skills[ "silence" ] = {
									name_id = "menu_silence",
									desc_id = "menu_silence_desc",		-- Description of skill
									icon_xy = { 4, 4 },
									[1] = { upgrades = { "weapon_silencer_recoil_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "weapon_silencer_enter_steelsight_speed_multiplier", "weapon_silencer_spread_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "silence_expert" ] = {	-- tier 5
									name_id = "menu_silence_expert",
									desc_id = "menu_silence_expert_desc",		-- Description of skill
									icon_xy = { 4, 4 },
									[1] = { upgrades = { "weapon_silencer_recoil_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "weapon_silencer_enter_steelsight_speed_multiplier", "weapon_silencer_spread_multiplier" }, cost = self.costs.hightierpro },
								}
	self.skills[ "good_luck_charm" ] = {	-- tier 6
									name_id = "menu_good_luck_charm",
									desc_id = "menu_good_luck_charm_desc",		-- Description of skill
									icon_xy = { 4, 2 },
									[1] = { upgrades = { "player_loot_drop_multiplier_1" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_loot_drop_multiplier_2" }, cost = self.costs.hightierpro },
								}
	self.skills[ "disguise" ] = {
									name_id = "menu_disguise",
									desc_id = "menu_disguise_desc",		-- Description of skill
									icon_xy = { 6, 4 },
									[1] = { cost = self.costs.hightier },
									[2] = { cost = self.costs.hightierpro },
								}
	self.skills[ "magic_touch" ] = {	-- tier 5
									name_id = "menu_magic_touch",
									desc_id = "menu_magic_touch_desc",		-- Description of skill
									icon_xy = { 5, 4 },
									[1] = { upgrades = { "player_pick_lock_easy", "player_pick_lock_easy_speed_multiplier" }, cost = self.costs.hightier },
									[2] = { upgrades = { "player_pick_lock_hard" }, cost = self.costs.hightierpro },
								}
													
	--[[self.skills[ "" ] = {
									name_id = "menu_",
									[1] = { cost = self.costs.default },
									[2] = { cost = self.costs.pro },
								}]]
	
	-- All trees
	self.trees = {}
	self.trees[ 1 ] = { name_id = "st_menu_mastermind",	skill = "mastermind",	background_texture = "guis/textures/pd2/skilltree/bg_mastermind", tiers = {} }
	self.trees[ 2 ] = { name_id = "st_menu_enforcer", 	skill = "enforcer", 	background_texture = "guis/textures/pd2/skilltree/bg_enforcer", tiers = {} }
	self.trees[ 3 ] = { name_id = "st_menu_technician",	skill = "technician",	background_texture = "guis/textures/pd2/skilltree/bg_technician", tiers = {} }
	self.trees[ 4 ] = { name_id = "st_menu_ghost", 			skill = "ghost", 			background_texture = "guis/textures/pd2/skilltree/bg_ghost", tiers = {} }
	
						-- IF ANY CHANGES IS MADE TO THE SKILLTREE, SkillTreeManager.VERSION NEEDS TO BE INCREASED
	-- Mastermind
	-- self.trees[ 1 ].tiers[ 1 ] = { "target_mark", "cable_guy", "combat_medic" }
	-- self.trees[ 1 ].tiers[ 2 ] = { "control_freak", "leadership", "gun_fighter" }
	-- self.trees[ 1 ].tiers[ 5 ] = { "fast_learner", "inspire", "kilmer" }
	-- self.trees[ 1 ].tiers[ 6 ] = { "negotiator", "equilibrium", "pistol_messiah" }
	
	self.trees[ 1 ].tiers[ 1 ] = { "cable_guy", "combat_medic", "triathlete" }
	self.trees[ 1 ].tiers[ 2 ] = { "inside_man", "fast_learner", "leadership" }
	self.trees[ 1 ].tiers[ 3 ] = { "tactician", "equilibrium",  "dominator" }
	self.trees[ 1 ].tiers[ 4 ] = { "stockholm_syndrome", "medic_2x", "joker"}
	self.trees[ 1 ].tiers[ 5 ] = { "black_marketeer", "gun_fighter", "kilmer" }
	self.trees[ 1 ].tiers[ 6 ] = { "control_freak", "pistol_messiah", "inspire" }
	
	-- Enforcer
	-- self.trees[ 2 ].tiers[ 1 ] = { "oppressor", "pack_mule", "demolition_man" }
	self.trees[ 2 ].tiers[ 1 ] = { "oppressor", "ammo_reservoir", "pack_mule" }
	self.trees[ 2 ].tiers[ 2 ] = { "show_of_force", "underdog", "steroids" }
	self.trees[ 2 ].tiers[ 3 ] = { "shotgun_impact", "shades", "tough_guy" }
	self.trees[ 2 ].tiers[ 4 ] = { "shotgun_cqb", "ammo_2x", "wolverine" }
	self.trees[ 2 ].tiers[ 5 ] = { "from_the_hip", "bandoliers", "portable_saw" }
	self.trees[ 2 ].tiers[ 6 ] = { "overkill", "juggernaut", "carbon_blade" }
	
	-- Technician
	--[[
	self.trees[ 3 ].tiers[ 1 ] = { "rifleman", "mag_plus", "trip_miner" }
	self.trees[ 3 ].tiers[ 2 ] = { "drill_expert", "blast_radius", "hardware_expert" }
	self.trees[ 3 ].tiers[ 3 ] = { "sharpshooter", "sentry_gun",  "insulation" }
	self.trees[ 3 ].tiers[ 4 ] = { "sentry_targeting_package", "shaped_charge", "silent_drilling" }
	self.trees[ 3 ].tiers[ 5 ] = { "military_grade", "sentry_2_0", "discipline" }
	self.trees[ 3 ].tiers[ 6 ] = { "sentry_gun_2x", "master_craftsman",  "dye_pack_removal" }
	]]
	self.trees[ 3 ].tiers[ 1 ] = { "rifleman", "trip_miner", "discipline" }
	self.trees[ 3 ].tiers[ 2 ] = { "sharpshooter", "trip_mine_expert", "hardware_expert" }
	self.trees[ 3 ].tiers[ 3 ] = { "sentry_gun","master_craftsman",  "drill_expert" }
	self.trees[ 3 ].tiers[ 4 ] = { "sentry_targeting_package", "blast_radius", "silent_drilling" }
	self.trees[ 3 ].tiers[ 5 ] = { "sentry_2_0", "shaped_charge", "insulation" }
	self.trees[ 3 ].tiers[ 6 ] = { "sentry_gun_2x", "mag_plus",  "iron_man" }
	
	-- Ghost -- Thief
	-- self.trees[ 4 ].tiers[ 3 ] = { "assassin", "martial_arts", "ecm_booster" }
	-- self.trees[ 4 ].tiers[ 4 ] = { "nine_lives", "ecm_feedback", "magic_touch" }
	-- self.trees[ 4 ].tiers[ 5 ] = { "moving_target", "ecm_2x", "silence" }
	-- self.trees[ 4 ].tiers[ 6 ] = { "good_luck_charm", "disguise", "smg_training" }
	self.trees[ 4 ].tiers[ 1 ] = { "scavenger", "sprinter", "cat_burglar" }
	self.trees[ 4 ].tiers[ 2 ] = { "transporter", "chameleon", "cleaner" }
	self.trees[ 4 ].tiers[ 3 ] = { "assassin", "martial_arts", "smg_master" }
	self.trees[ 4 ].tiers[ 4 ] = { "nine_lives", "ecm_2x", "hitman" }
	self.trees[ 4 ].tiers[ 5 ] = { "magic_touch", "ecm_booster", "silence_expert" }
	self.trees[ 4 ].tiers[ 6 ] = { "good_luck_charm", "ecm_feedback", "moving_target" }
	
end

