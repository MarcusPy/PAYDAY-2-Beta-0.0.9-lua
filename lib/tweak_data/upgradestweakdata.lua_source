UpgradesTweakData = UpgradesTweakData or class()

--[[function UpgradesTweakData:init()
	
	self.definitions = {}
	
end]]

function UpgradesTweakData:_init_pd2_values()
	self:_init_value_tables()
	
	--[[ R E P   U P G R A D E S ]]
	self.values.rep_upgrades = {}															-- same amount of indexes in both, creates rep_upgrade upgrades
	self.values.rep_upgrades.classes = { "rep_upgrade" }			-- identifier of the upgrade, the upgrade name will be: class .. i  (class is this value, i is index between 1 and 10) ("rep_upgrade1", "rep_upgrade2", etc)
	self.values.rep_upgrades.values = { 2 }										-- how many skill point is rewarded
	
	--[[ A R M O R ]]
	self.values.player.body_armor = { 1, 2, 3, 4, 5, 10 }
	self.values.player.armor_movement_penalty = { 0.96, 0.92, 0.88, 0.84, 0.8, 0.76 } -- A multiplier on movement speed
	
	
	--[[ D E F A U L T  U P G R A D E S ]]
	self.values.player.special_enemy_highlight						= { true }
	self.values.player.hostage_trade											= { true }
	self.values.player.sec_camera_highlight								= { true }
	
	
	--[[ D E P L O Y A B L E   V A L U E S ]]
	self.ammo_bag_base																		= 4
	self.ecm_jammer_base_battery_life											= 20
	self.ecm_jammer_base_low_battery_life									= 8					-- int, not float
	self.sentry_gun_base_ammo															= 150
	self.sentry_gun_base_armor														= 10
	self.doctor_bag_base																	= 2
	
	
	--[[ D L C  U P G R A D E S ]]
		-- PRE-ORDER
	self.values.player.crime_net_deal											= { 0.9 }
	
	--[[ M A S T E R M I N D ]]
		--[[ TIER 1 ]]
			-- TACTICIAN
	self.values.weapon.special_damage_taken_multiplier		= { 1.05 }	-- BASIC
	self.values.player.marked_enemy_extra_damage					= { true }	-- ACE. self.values.player.marked_enemy_damage_mul when active
	self.values.player.marked_enemy_damage_mul						= 1.15				-- ACE. this is read directly
			
			-- CABLE_GUY
	self.values.cable_tie.interact_speed_multiplier				= { 0.5 }		-- BASIC
	self.values.cable_tie.quantity												= { 8 }			-- ACE
	self.values.cable_tie.can_cable_tie_doors							= { true }	-- ACE
	
			-- COMBAT_MEDIC
	self.values.temporary.combat_medic_damage_multiplier	= { { 1.25, 10 }, { 1.25, 15 } }	-- BASIC. { value, time }, only first upgrade is in skilltree
	self.values.player.revive_health_boost								= { 1 }			-- ACE. defines which value in self.revive_health_multiplier to use
	self.revive_health_multiplier													= { 1.25 }		-- ACE. this is read directly
	
		--[[ TIER 2 ]]
			-- CONTROL_FREAK
	self.values.player.civ_harmless_bullets								= { true } -- BASIC
	self.values.player.civ_harmless_melee									= { true } -- BASIC
	self.values.player.civ_calming_alerts									= { true } -- BASIC
	self.values.player.civ_intimidation_mul								= { 10.00 }	-- ACE
	
			-- LEADERSHIP
	-- BASIC see, self.values.team.stamina.passive_multiplier rank 2
	self.values.team.pistol.recoil_multiplier							= { 0.75 }	-- ACE
	
			-- INSIDE_MAN
	self.values.player.assets_cost_multiplier							= { 0.5 } 	-- BASIC
	self.values.player.additional_assets									= { true }	-- ACE
	
		--[[ TIER 3 ]]
			-- TRIATHLETE
	self.values.player.stamina_multiplier									= { 2 }			-- BASIC
	self.values.team.stamina.multiplier										= { 1.5 }		-- ACE
	
			-- DOMINATOR
	self.values.player.intimidate_enemies									= { true }	-- BASIC
	self.values.player.intimidate_range_mul								= { 2 }			-- ACE
	self.values.player.intimidate_aura										= { 700 }		-- ACE. radius in cm
	
			-- STOCKHOLM_SYNDROME
	self.values.player.civilian_reviver										= { true }	-- BASIC
	self.values.player.civilian_gives_ammo								= { true }	-- ACE
	
		--[[ TIER 4 ]]
			-- BLACK_MARKETEER
	self.values.player.buy_cost_multiplier								= { 0.95, 0.8 } -- BASIC, ACE
	self.values.player.sell_cost_multiplier								= { 1.25 }		-- ACE
	
			-- MEDIC_2X
	self.values.doctor_bag.quantity												= { 1 }			-- BASIC
	self.values.doctor_bag.amount_increase								= { 2 }			-- ACE
	
			-- JOKER
	self.values.player.convert_enemies										= { true }	-- BASIC
	self.values.player.convert_enemies_max_minions				= { 1, 2 }	-- BASIC. only first upgrade is in skilltree
	self.values.player.convert_enemies_health_multiplier	= { 0.10 }	-- ACE. implemented as a damage reduction multiplier
	self.values.player.convert_enemies_damage_multiplier	= { 4.5 }		-- ACE
	
		--[[ TIER 5 ]]
			-- FAST_LEARNER
	self.values.player.xp_multiplier											= { 1.1 }		-- BASIC
	self.values.team.xp.multiplier												= { 1.1 }		-- ACE
		
			-- GUN_FIGHTER
	self.values.pistol.reload_speed_multiplier						= { 1.33 }	-- BASIC
	self.values.pistol.damage_multiplier									= { 1.25 }	-- ACE

			-- KILMER
	self.values.assault_rifle.reload_speed_multiplier			= { 1.33 }	-- BASIC
	self.values.assault_rifle.move_spread_multiplier			= { 0.5 }		-- ACE
	
		--[[ TIER 6 ]]
			-- PISTOL_MESSIAH
	self.values.player.pistol_revive_from_bleed_out				= { 1, 3 }	-- BASIC, ACE
	
			-- EQUILIBRIUM
	self.values.pistol.spread_multiplier									= { 0.9 }		-- BASIC
	self.values.pistol.swap_speed_multiplier							= { 1.5 }		-- BASIC
	self.values.pistol.fire_rate_multiplier								= { 2 }			-- ACE
	
			-- INSPIRE
	self.values.player.revive_interaction_speed_multiplier	= { 0.5 }	-- BASIC
	self.values.player.long_dis_revive										= { true }	-- ACE
	
		--[[ TIER BONUS ]]
	self.values.doctor_bag.interaction_speed_multiplier		= { 0.80 }					-- TIER 1
	self.values.team.stamina.passive_multiplier						= { 1.15, 1.30 }		-- TIER 2, Leadership BASIC
	self.values.player.passive_intimidate_range_mul				= { 1.25 }					-- TIER 3
	self.values.player.passive_convert_enemies_health_multiplier		= { 0.50 }	-- TIER 4. implemented as a damage reduction multiplier
	self.values.player.passive_convert_enemies_damage_multiplier		= { 1.05 }	-- TIER 4
	self.values.player.convert_enemies_interaction_speed_multiplier	= { 0.33 }	-- TIER 5
	self.values.player.empowered_intimidation_mul					= { 3	}							-- TIER 6
	self.values.player.passive_assets_cost_multiplier			= { 0.5 }						-- TIER 6
	
	
	--[[ E N F O R C E R ]]
		--[[ TIER 1 ]]
			-- OPPRESSOR
	self.values.player.suppression_multiplier							= { 1.25, 1.75 }	-- BASIC, ACE
	
			-- PACK_MULE
	self.values.carry.movement_speed_multiplier						= { 1.5 }		-- BASIC
	self.values.carry.throw_distance_multiplier						= { 1.5 }		-- ACE
	
			-- AMMO_RESERVOIR
	self.values.temporary.no_ammo_cost										= { {true, 5}, {true, 10} } -- BASIC, ACE. { value, time }
	
		--[[ TIER 2 ]]
			-- STEROIDS
	self.values.player.non_special_melee_multiplier				= { 2.0 }		-- BASIC
	self.values.player.melee_damage_multiplier						= { 2.0 }		-- ACE
	
			-- SHOW_OF_FORCE
	self.values.player.primary_weapon_when_downed					= { true }	-- BASIC
	self.values.player.armor_regen_timer_multiplier				= { 0.85 }	-- ACE
	
			-- UNDERDOG
	self.values.temporary.dmg_multiplier_outnumbered			= { { 1.1, 7 } }	-- BASIC. { value, time }
	self.values.temporary.dmg_dampener_outnumbered				= { { 0.85, 7 } }	-- ACE. { value, time }
	
		--[[ TIER 3 ]]
			-- BANDOLIERS
	self.values.player.extra_ammo_multiplier							= { 1.25 }		-- BASIC
	self.values.player.pick_up_ammo_multiplier						= { 1.75 }		-- ACE
	
			-- TOUGH_GUY
	self.values.player.damage_shake_multiplier						= { 0.5 }		-- BASIC
	self.values.player.bleed_out_health_multiplier				= { 1.25 }		-- ACE
	
			-- SHOTGUN_IMPACT
	self.values.shotgun.recoil_multiplier									= { 0.75 }	-- BASIC
	self.values.shotgun.damage_multiplier									= { 1.20 }	-- ACE
	
		--[[ TIER 4 ]]
			-- AMMO_2X
	self.values.ammo_bag.ammo_increase										= { 2 }			-- BASIC
	self.values.ammo_bag.quantity													= { 1 }			-- ACE
	
			-- SHOTGUN_CQB
	self.values.shotgun.reload_speed_multiplier						= { 1.5 }		-- BASIC
	self.values.shotgun.enter_steelsight_speed_multiplier	= { 2.25 }			-- ACE
	
			-- PORTABLE_SAW
	-- BASIC unlocks saw
	self.values.saw.extra_ammo_multiplier									= { 1.5 }			-- ACE. Will double amount of blades
	
		--[[ TIER 5 ]]
			-- SHADES
	self.values.player.flashbang_multiplier								= { 0.5, 0.25 }	-- BASIC, ACE
	
			-- FROM_THE_HIP
	self.values.shotgun.hip_fire_spread_multiplier				= { 0.80 }		-- BASIC
	self.values.pistol.hip_fire_spread_multiplier					= { 0.80 }		-- ACE
	self.values.assault_rifle.hip_fire_spread_multiplier	= { 0.80 }		-- ACE
	self.values.smg.hip_fire_spread_multiplier						= { 0.80 }		-- ACE
	self.values.saw.hip_fire_spread_multiplier						= { 0.80 }		-- ACE
	
			-- CARBON_BLADE
	self.values.player.saw_speed_multiplier								= { 0.9, 0.7 }	-- BASIC, ACE
	self.values.saw.lock_damage_multiplier								= { 1.2, 1.4 }	-- BASIC, ACE
	self.values.saw.enemy_slicer													= { true }				-- ACE. Less ammo usage when killing enemies
	
		--[[ TIER 6 ]]
			-- WOLVERINE
	self.values.player.melee_damage_health_ratio_multiplier	= { 5.0 }	-- BASIC. Additional damage ontop of base damage ( multiplier of base damage, 1 = x2 (1+1) damage, 5 = x6 (1+5) damage ect. )
	self.values.player.damage_health_ratio_multiplier			= { 1.0 }		-- ACE. Additional damage ontop of base damage
	self.player_damage_health_ratio_threshold							= 0.4 			-- ACE. when self.values.damage_health_ratio_multiplier starts to kick in (health of player 0-1)

			-- JUGGERNAUT
	-- BASIC unlocks armor level 7
	self.values.player.shield_knock												= { true }	-- ACE
	
			-- OVERKILL
	self.values.temporary.overkill_damage_multiplier			= { { 2, 6 } } -- BASIC { value, time }
	self.values.player.overkill_all_weapons								= { true }	-- ACE	
	
		--[[ TIER BONUS ]]
	self.values.player.passive_suppression_multiplier			= { 1.1, 1.2 }				-- TIER 1, TIER 3
	self.values.player.passive_health_multiplier					= { 1.1, 1.20, 1.40 }		-- TIER 2, TIER 4, TIER 6
	self.values.weapon.passive_damage_multiplier					= { 1.05 } 						-- TIER 5
	
	
	--[[ T E C H N I C I A N ]]
		--[[ TIER 1 ]]
			-- RIFLEMAN
	self.values.assault_rifle.enter_steelsight_speed_multiplier	= { 2 }	-- BASIC
	self.values.assault_rifle.zoom_increase								= { 1.0 }		-- ACE. increase in the tweak_data.weapon.stats.zoom
	
			-- MASTER_CRAFTSMAN
	self.values.player.crafting_weapon_multiplier					= { 0.90 }		-- BASIC
	self.values.player.crafting_mask_multiplier						= { 0.90 }		-- ACE
	
			-- TRIP_MINER
	self.values.trip_mine.quantity_1											= { 1 }			-- BASIC
	self.values.trip_mine.can_switch_on_off								= { true }	-- ACE

		--[[ TIER 2 ]]
			-- DRILL_EXPERT
	self.values.player.drill_speed_multiplier							= { 0.9, 0.75 }	-- BASIC, ACE
	
			-- TRIP_MINE_EXPERT
	self.values.player.trip_mine_deploy_time_multiplier		= { 0.80, 0.5 }	-- TRIP_MINE_EXPERT: BASIC, HARDWARE_EXPERT: BASIC
	self.values.trip_mine.sensor_toggle										= { true }	-- ACE
	
			-- HARDWARE_EXPERT
	-- BASIC uses self.values.player.trip_mine_deploy_time_multiplier, having one of them gives first value, having both gives second
	self.values.player.drill_fix_interaction_speed_multiplier	= { 0.75 }	-- BASIC
	self.values.player.drill_autorepair										= { 0.3 }		-- ACE
	self.values.player.sentry_gun_deploy_time_multiplier	= { 0.50 }	-- ACE
	
		--[[ TIER 3 ]]
			-- SENTRY_GUN
	-- BASIC unlocks sentry gun
	self.values.sentry_gun.armor_multiplier								= { 2 }		-- ACE
	
			-- SHARPSHOOTER
	self.values.weapon.single_spread_multiplier						= { 0.5 }		-- BASIC
	self.values.assault_rifle.recoil_multiplier						= { 0.75 }	-- ACE
	
			-- INSULATION
	self.values.player.taser_malfunction									= { true }	-- BASIC
	self.values.player.taser_self_shock										= { true }	-- ACE
	
		--[[ TIER 4 ]]
			-- SENTRY_TARGETING_PACKAGE
	self.values.sentry_gun.spread_multiplier							= { 0.5 }		-- BASIC
	self.values.sentry_gun.rot_speed_multiplier						= { 2 }		-- ACE
	
			-- DISCIPLINE
	self.values.player.interacting_damage_multiplier			= { 0.5 }		-- BASIC
	self.values.player.steelsight_when_downed							= { true }	-- ACE
	
			-- SILENT_DRILLING
	self.values.player.drill_alert_rad										= { 900 }		-- BASIC. in cm  (?)
	self.values.player.silent_drill												= { true }	-- ACE
	
		--[[ TIER 5 ]]
			-- SENTRY_2_0
	self.values.sentry_gun.extra_ammo_multiplier					= { 1.5, 2 }	-- BASIC. only first upgrade is in skilltree
	self.values.sentry_gun.shield													= { true }	-- ACE
	
			-- BLAST_RADIUS
	self.values.trip_mine.explosion_size_multiplier				= { 1.25, 1.75 }	-- BASIC, ACE
	
			-- SHAPED_CHARGE
	self.values.trip_mine.quantity_3											= { 3 }			-- BASIC. amount
	self.values.player.trip_mine_shaped_charge						= { true }	-- ACE
	
	
		--[[ TIER 6 ]]
			-- SENTRY_GUN_2X
	self.values.sentry_gun.quantity												= { 1 }			-- BASIC. amount
	self.values.sentry_gun.damage_multiplier							= { 2 }			-- ACE
	
			-- MAG_PLUS
	self.values.weapon.clip_ammo_increase									= { 5, 15 }	-- BASIC, ACE
	
			-- IRON_MAN
	self.values.player.armor_multiplier										= { 1.5 }		-- BASIC
	self.values.team.armor.regen_time_multiplier					= { 0.75 }		-- ACE
	
		--[[ TIER BONUS ]]
	self.values.player.passive_crafting_weapon_multiplier	= { 0.99, 0.97, 0.95 }	-- TIER 1, TIER 3, TIER 5
	self.values.player.passive_crafting_mask_multiplier		= { 0.99, 0.97, 0.95 }	-- TIER 1, TIER 3, TIER 5
	self.values.weapon.passive_recoil_multiplier					= { 0.95, 0.9 }					-- TIER 2, TIER 6
	self.values.weapon.passive_headshot_damage_multiplier	= { 1.25 }							-- TIER 4
	self.values.player.passive_armor_multiplier						= { 1.1, 1.25 }					-- TIER 6. only first upgrade is in skilltree
	self.values.team.armor.passive_regen_time_multiplier	= { 0.9 }							-- TIER 6
	
	
	--[[ G H O S T ]]
		--[[ TIER 1 ]]
			-- SCAVENGER
	self.values.player.small_loot_multiplier							= { 1.1, 1.3 }	-- BASIC, ACE
	
			-- SPRINTER
	self.values.player.stamina_regen_timer_multiplier			= { 0.75 }	-- BASIC
	self.values.player.stamina_regen_multiplier						= { 1.25 }	-- BASIC
	self.values.player.run_dodge_chance										= { 0.25 }	-- ACE
	self.values.player.run_speed_multiplier								= { 1.25 }	-- ACE
	
			-- CAT_BURGLAR
	self.values.player.fall_damage_multiplier							= { 0.5 }		-- BASIC
	self.values.player.fall_health_damage_multiplier			= { 0.0 }		-- ACE	-- THIS ONE IS LOCKED TO THE SKILL DESCRIPTION AND CANNOT BE CHANGED WITHOUT CHANGING THE DESRIPTION
	self.values.player.respawn_time_multiplier						= { 0.5 } 	-- ACE
	
		--[[ TIER 2 ]]
			-- CLEANER
	self.values.player.corpse_alarm_pager_bluff						= { true }	-- BASIC. tweak_data.player.alarm_pager[ has_upgrade and "bluff_success_chance_w_skill" or "bluff_success_chance" ]
	self.values.player.corpse_dispose											= { true }	-- ACE
	
			-- TRANSPORTER
	self.values.carry.interact_speed_multiplier						= { 0.6, 0.25 }	-- BASIC, ACE
	
			-- CHAMELEON
	self.values.player.suspicion_multiplier								= { 0.75 }	-- BASIC
	self.values.player.camouflage_bonus										= { 1.25 }	-- ACE. multiplier to player's detection delay
	
		--[[ TIER 3 ]]
			-- ASSASSIN
	self.values.player.walk_speed_multiplier							= { 1.25 }	-- BASIC
	self.values.player.crouch_speed_multiplier						= { 1.25 }	-- BASIC
	self.values.player.silent_kill												= { 400 }		-- ACE. in cm alert radius on dead and hurt enemies
	
			-- MARTIAL_ARTS
	self.values.player.melee_knockdown_mul								= { 1.5 }		-- BASIC
	self.values.player.damage_dampener										= { 0.5 }		-- ACE. multiplier
	
			-- SMG_MASTER
	self.values.smg.reload_speed_multiplier								= { 1.33 }	-- BASIC
	self.values.smg.recoil_multiplier											= { 0.75 }	-- ACE
	
		--[[ TIER 4 ]]
			-- NINE_LIVES
	self.values.player.additional_lives										= { 1, 3 }	-- BASIC, ACE, ACE NOT USED
	self.values.player.cheat_death_chance 								= { 0.1 }
	
			-- ECM_FEEDBACK
	self.values.ecm_jammer.can_activate_feedback					= { true }	-- BASIC
	self.values.ecm_jammer.feedback_duration_boost				= { 1.25 }	-- ACE
	
			-- HITMAN
	self.values.weapon.silencer_damage_multiplier					= { 1.1, 1.2 }	-- BASIC, ACE
	
		--[[ TIER 5 ]]
			-- ECM_BOOSTER
	self.values.ecm_jammer.duration_multiplier						= { 1.25 }	-- BASIC
	self.values.ecm_jammer.can_open_sec_doors							= { true }	-- ACE
	
			-- MAGIC_TOUCH
	self.values.player.pick_lock_easy											= { true }	-- BASIC
	self.values.player.pick_lock_easy_speed_multiplier		= { 0.75 }	-- BASIC. reduces speed for pick_lock_easy, pick_lock_easy_no_skill and pick_lock_hard_no_skill interactions
	self.values.player.pick_lock_hard											= { true }	-- ACE
			
			-- SILENCE_EXPERT
	self.values.weapon.silencer_recoil_multiplier					= { 0.5 }		-- BASIC
	self.values.weapon.silencer_spread_multiplier					= { 0.5 }		-- ACE
	self.values.weapon.silencer_enter_steelsight_speed_multiplier	= { 2.0 }	-- ACE
		
		--[[ TIER 6 ]]
			-- GOOD_LUCK_CHARM
	self.values.player.loot_drop_multiplier								= { 1.5, 3 }	-- BASIC, ACE
	
			-- ECM_2X
	self.values.ecm_jammer.quantity												= { 1, 3 }	-- BASIC, ACE
	self.values.ecm_jammer.duration_multiplier_2					= { 1.25 }	-- ACE
	self.values.ecm_jammer.feedback_duration_boost_2			= { 1.25 }	-- ACE
	
			-- MOVING_TARGET
	self.values.player.can_strafe_run											= { true }	-- BASIC
	self.values.player.can_free_run												= { true }	-- ACE
			
		--[[ TIER BONUS ]]
	self.values.ecm_jammer.affects_cameras								= { true }				-- TIER 1
	self.values.player.passive_dodge_chance								= { 0.05, 0.15 }	-- TIER 1, TIER 3
	self.values.weapon.passive_swap_speed_multiplier			= { 1.2, 2 }		-- TIER 2, TIER 5
	self.values.player.passive_suspicion_multiplier				= { 0.75 }				-- TIER 4
	self.values.player.passive_loot_drop_multiplier				= { 1.1 }				-- TIER 6
end

function UpgradesTweakData:init()

	-- weapon upgrades
	-- self.upgrades.weapon_id.clip_ammo_increase, defines an increased amount of bullets in mag
	-- self.upgrades.weapon_id.clip_amount_increase, defines an increased amount of mags
	-- self.upgrades.weapon_id.recoil_multiplier, defines a multiplier value to the recoil kick (less then 1 means less kick)
	-- self.upgrades.weapon_id.spread_multiplier, defines a multiplier value to the spread (less then 1 means less spread)
	-- self.upgrades.weapon_id.explosion_range_multiplier, defines a multiplier value to the explosion range (less then 1 means less range)


	self.level_tree = {}

							
	self.level_tree[  2 ] = { name_id = "Angst",
							upgrades = { "colt_1911", "mac10"  } }					
	self.level_tree[  4 ] = { name_id = "Angst", 
							upgrades = { "new_m4", "ak74" } }			
	self.level_tree[  6 ] = { name_id = "Angst",
							upgrades = { "new_raging_bull", "b92fs" } }							
	self.level_tree[ 8 ]  = { name_id = "Angst",
							upgrades = { "r870", "aug" } }							
	self.level_tree[ 10 ] = { name_id = "lvl_10", 
							announcements = { "menu_es_jobs_available" }, 	-- 2 star jobs unlocked
							upgrades = { "rep_upgrade1"} }			
							
	self.level_tree[ 12 ] = { name_id = "body_armor",
							upgrades = { "body_armor1", "body_armor2" } }	
	self.level_tree[ 13 ] = { name_id = "Angst", 
							upgrades = { "new_mp5", "serbu"} }											
	self.level_tree[ 16 ] = { name_id = "Angst", 
							upgrades = { "akm", "g36" } }								
	self.level_tree[ 19 ] = { name_id = "Angst",
							upgrades = { "olympic", "mp9"} } 
	self.level_tree[ 20 ] = { name_id = "lvl_20", 
							announcements = { "menu_es_jobs_available" }, -- 3 star jobs unlocked
							upgrades = { "rep_upgrade2" } }
	self.level_tree[ 21 ] = { name_id = "body_armor3",
							upgrades = { "body_armor3" } }	
							
						
						
	self.level_tree[ 26 ] = { name_id = "Angst", 
							upgrades = { "new_m14", "saiga" } }														
	self.level_tree[ 29 ] = { name_id = "Angst",
							upgrades = { "akmsu", "glock_18c" } }	
	self.level_tree[ 30 ] = { name_id = "lvl_30",
							announcements = { "menu_es_jobs_available" },	-- 4 star jobs unlocked
							upgrades = { "rep_upgrade3" } }
	self.level_tree[ 31 ] = { name_id = "body_armor4",
							upgrades = { "body_armor4" } }
							
	self.level_tree[ 33 ] = { name_id = "Angst", 
							upgrades = { "ak5" } }
	self.level_tree[ 36 ] = { name_id = "Angst", 
							upgrades = { "p90", "deagle"  } }
	self.level_tree[ 39 ] = { name_id = "Angst", 
							upgrades = { "m16", "huntsman"  } }
	self.level_tree[ 40 ] = { name_id = "lvl_40", 
							announcements = { "menu_es_jobs_available" },	-- 5 star jobs unlocked
							upgrades = { "rep_upgrade4" } } 
	self.level_tree[ 41 ] = { name_id = "body_armor5", 
							upgrades = { "body_armor5"  } }	
							
	self.level_tree[ 50 ] = { name_id = "lvl_50", 
							announcements = { "menu_es_jobs_available" }, 	-- 6 star jobs unlocked
							upgrades = { "rep_upgrade5" } }
	self.level_tree[ 60 ] = { name_id = "lvl_60", 
							announcements = { "menu_es_jobs_available" }, 	-- 7 star jobs unlocked
							upgrades = { "rep_upgrade6" } }
	self.level_tree[ 70 ] = { name_id = "lvl_70", 
							announcements = { "menu_es_jobs_available" }, 	-- overkill++
							upgrades = { "rep_upgrade7" } }
	self.level_tree[ 80 ] = { name_id = "lvl_80", 
							announcements = { "menu_es_jobs_available" }, 
							upgrades = { "rep_upgrade8" } }
	self.level_tree[ 90 ] = { name_id = "lvl_90", 
							announcements = { "menu_es_jobs_available" }, 
							upgrades = { "rep_upgrade9" } }
	self.level_tree[ 95 ] = { name_id = "menu_es_jobs_available", 
							announcements = { "menu_es_jobs_available" },
							upgrades = { "lucky_charm" } }
	self.level_tree[ 100 ] = { name_id = "lvl_100", 
							upgrades = { "rep_upgrade10" }}
							


-- 16:56:57	[app:script] Tried to aquire an upgrade that doesn't exist: shape_charge (aquire() lib/managers/upgradesmanager.lua:248)
-- 16:56:57	[app:script] Tried to aquire an upgrade that doesn't exist: body_armor6 (aquire() lib/managers/upgradesmanager.lua:248)
-- 16:56:57	[app:script] Tried to aquire an upgrade that doesn't exist: lucky_charm (aquire() lib/managers/upgradesmanager.lua:248)
	
	
	self:_init_pd2_values()					-- PD2 SKILL INITS HERE
	
	-- self.values = self.values or {}
	
	self:_init_values()
	
	self.steps = {}
	
	self.values.player = self.values.player or {}
	-- self.values.player.body_armor	 				= { 1, 2, 3, 4, 5, 6, 10 } --, 5, 6, 7, 8 }
	self.values.player.thick_skin	 													= { 2, 4, 6, 8, 10 } -- , 12, 14, 16 }
	-- self.values.player.body_armor	 				= { 2, 4, 6, 8, 10, 12, 14, 16 }
	 -- self.values.player.bleed_out_increase			= { 2, 4, 6, 8, 10, 12, 14, 16 }
	-- self.values.player.intimidation_multiplier		= { 1.08, 1.16, 1.24, 1.32, 1.40, 1.48, 1.56, 1.64 }
	-- self.values.player.extra_ammo_multiplier		= { 1.1 } -- { 1.1, 1.2, 1.3, 1.4, 1.5 } --, 1.48, 1.56, 1.64 }
	-- self.values.player.flashbang_multiplier 			= { 0.8, 0.4 }
	-- self.values.player.pick_up_ammo_multiplier		= { 1.5 } -- { 1.1, 1.2, 1.3, 1.4, 1.5 } --, 1.48, 1.56, 1.64 }
	-- self.values.player.primary_weapon_when_downed	= { true }
	self.values.player.primary_weapon_when_carrying					= { true }
	-- self.values.player.can_strafe_run							= { true }
	-- self.values.player.can_free_run								= { true }
	-- self.values.player.damage_shake_multiplier		= { 0.5 } -- Multiplier on camera shake when taking damage
	self.values.player.health_multiplier										= { 1.10 } -- 1.5
	-- self.values.player.revive_interaction_speed_multiplier = { 0.5 } 
	-- self.values.player.bleed_out_health_multiplier = { 1.1 } -- 2.0
	-- self.values.player.armor_regen_timer_multiplier	= { 0.8 } -- 0.75
	-- self.values.player.passive_armor_multiplier	= { 1.05, 1.1 } -- 1.1, 1.25
	-- self.values.player.armor_multiplier	= { 1.1 } -- 1.1, 1.25
	-- self.values.player.stamina_multiplier			= { 2}
	-- self.values.player.stamina_regen_multiplier						= { 1.25 }
	-- self.values.player.stamina_regen_timer_multiplier			= { 0.75 }
	-- self.values.player.run_dodge_chance										= { 0.25 }				-- sprint, chance, 0 = 0%, 1 = 100%
	-- self.values.player.run_speed_multiplier								= { 1.25 }				-- sprint
	-- self.values.player.passive_dodge_chance								= { 0.05, 0.15 }				-- chance, 0 = 0%, 1 = 100%
	-- self.values.player.fall_damage_multiplier							= { 0.5 }
	-- self.values.player.fall_health_damage_multiplier			= { 0.10 }
	-- self.values.player.interacting_damage_multiplier			= { 0.5 }
	-- self.values.player.steelsight_when_downed			= { true }
	-- self.values.player.corpse_alarm_pager_bluff = { true }
	-- self.values.player.corpse_dispose = { true }
	-- self.values.player.taser_malfunction = { true }
	-- self.values.player.taser_self_shock = { true }
	self.values.player.electrocution_resistance_multiplier 		= { 0.25 }
	-- self.values.player.silent_kill = { 400 } -- 4m alert radius on dead and hurt enemies
	-- .values.player.melee_knockdown_mul = { 1.5 }
	-- self.values.player.walk_speed_multiplier	= { 1.25 }
	-- self.values.player.crouch_speed_multiplier	= { 1.25 }
	-- self.values.player.xp_multiplier				= { 1.05 }
	self.values.player.passive_xp_multiplier									= { 1.05 }
	-- self.values.player.passive_health_multiplier	= { 1.1, 1.2, 1.3 } -- 2.0
	-- self.values.player.non_special_melee_multiplier	= { 2 }
	self.values.player.dye_pack_chance_multiplier							= { 0.5 }
	self.values.player.dye_pack_cash_loss_multiplier					= { 0.4 } -- Multiplied to tweak_data.dye.value_multiplier
	self.values.player.toolset																= { 0.95, 0.9, 0.85, 0.8 }
	-- self.values.player.suspicion_multiplier = { 0.75 }
	-- self.values.player.passive_suspicion_multiplier = { 0.75 }
	-- self.values.player.camouflage_bonus = { 1.25 } -- multiplier to player's detection delay
	self.values.player.uncover_progress_mul										= { 0.5 }
	self.values.player.uncover_progress_decay_mul							= { 1.5 }
	self.values.player.suppressed_multiplier									= { 0.5 }
	-- self.values.player.suppression_multiplier = { 1.5, 2 }
	-- self.values.player.suppression_multiplier2 = { 2 }
	-- self.values.player.passive_suppression_multiplier = { 1.1, 1.2 }
	-- self.values.player.special_enemy_highlight								= { true }
	-- self.values.player.damage_dampener = { 0.9 }
	-- self.values.player.marked_enemy_damage_mul = 1.1 -- this is read directly. not used through through the skill system
	-- self.values.player.marked_enemy_extra_damage = { true }
	-- self.values.player.civ_intimidation_mul = { 10.00 }
	-- self.values.player.civilian_reviver = { true }
	-- self.values.player.civilian_gives_ammo = { true }
	-- self.values.player.drill_alert_rad = { 900 }
	-- self.values.player.silent_drill = { true }
	-- self.values.player.trip_mine_shaped_charge = { true }
	-- self.values.player.small_loot_multiplier = { 1.3, 1.6 }
	-- self.values.player.intimidate_enemies = { true }
	self.values.player.intimidate_specials 										= { true }
	-- self.values.player.empowered_intimidation_mul = { 2	}
	self.values.player.intimidation_multiplier 								= { 1.25 }
	-- self.values.player.convert_enemies = { true }
	-- self.values.player.convert_enemies_health_multiplier = { 0.01 }					-- DOES NOT INCREASE COP HEALTH, BUT REDUCES THE DAMAGE HE TAKES, easier to implement
	-- self.values.player.convert_enemies_damage_multiplier = { 4.5 }
	-- self.values.player.convert_enemies_interaction_speed_multiplier = { 0.5 }
	-- self.values.player.passive_convert_enemies_health_multiplier = { 0.90 }	-- DOES NOT INCREASE COP HEALTH, BUT REDUCES THE DAMAGE HE TAKES, easier to implement
	-- self.values.player.passive_convert_enemies_damage_multiplier = { 1.05 }
	-- self.values.player.intimidate_range_mul = { 1.5 }
	-- self.values.player.passive_intimidate_range_mul = { 1.33 }
	-- self.values.player.intimidate_aura = { 600 } -- radius in cm
	-- self.values.player.hostage_trade = { true }
	-- self.values.player.drill_autorepair = { 0.15 }	-- chance that the auto-repair will take place
	-- self.values.player.cheat_death_chance 										= { 0.5 }			--     0 = 0%, 1 = 100%
	-- self.values.player.additional_lives 				= { 1, 3 }
	-- self.values.player.overkill_all_weapons 			= { true }
	-- self.values.player.drill_speed_multiplier = { 0.9, 0.75 }
	-- self.values.player.saw_speed_multiplier = { 0.75, 0.50 }
	-- self.values.player.drill_fix_interaction_speed_multiplier = { 0.75 }
	-- self.values.player.sentry_gun_deploy_time_multiplier = { 0.50 }
	-- self.values.player.trip_mine_deploy_time_multiplier 			= { 0.80, 0.5 }
	-- self.values.player.long_dis_revive 			= { true }
	self.values.player.morale_boost														= { true }
	-- self.player_damage_health_ratio_threshold										= 0.4 			-- when damage_health_ratio_multiplier starts to kick in
	-- self.values.player.damage_health_ratio_multiplier						= { 1.0 }
	-- self.values.player.melee_damage_health_ratio_multiplier			= { 5.0 }
	-- self.values.player.melee_damage_multiplier									= { 2.0 }
	-- self.values.player.respawn_time_multiplier 									= { 0.5 } 
	-- self.values.player.shield_knock = { true }
	-- self.values.player.pick_lock_easy = { true }
	-- self.values.player.pick_lock_hard = { true }
	-- self.values.player.pick_lock_easy_speed_multiplier = { 0.5 }
	-- self.values.player.loot_drop_multiplier = { 1.5, 3 }
	-- self.values.player.passive_loot_drop_multiplier = { 1.05 }
-- 	self.values.player.buy_cost_multiplier = { 0.9, 0.75 }
-- 	self.values.player.sell_cost_multiplier = { 1.9 }
	-- self.values.player.crafting_weapon_multiplier = { 0.8 }
	-- self.values.player.crafting_mask_multiplier = { 0.8 }
	-- self.values.player.passive_crafting_weapon_multiplier = { 0.99, 0.97, 0.95 }
	-- self.values.player.passive_crafting_mask_multiplier = { 0.99, 0.97, 0.95 }
	-- self.values.player.additional_assets = { true }
	-- self.values.player.assets_cost_multiplier = { 0.5 }
	-- self.values.player.passive_assets_cost_multiplier = { 0.92 }
	-- self.values.player.revive_health_boost = { 1 }				-- which value in self.revive_health_multiplier the revived player should use when boosting his health
	-- self.values.player.pistol_revive_from_bleed_out	= { 1, 3 }
	
	self.steps.player = {}
	self.steps.player.thick_skin = { nil, 8, 18, 27, 39 }
	self.steps.player.extra_ammo_multiplier = { nil, 7, 16, 24, 38 }
	self.steps.player.toolset = { nil, 7, 16, 38 }
	
	self.values.trip_mine = self.values.trip_mine or {}
	self.values.trip_mine.quantity										= { 1, 2, 3, 4, 5, 8 }
	-- self.values.trip_mine.quantity_1					= { 1 } 
	self.values.trip_mine.quantity_2									= { 2 }
	-- self.values.trip_mine.quantity_3					= { 2 }
		-- self.values.trip_mine.damage_multiplier						= { 1.3, 1.6 } -- , 1.39, 1.52 }
	-- self.values.trip_mine.can_switch_on_off			= { true }
	-- self.values.trip_mine.sensor_toggle					= { true }
	self.values.trip_mine.quantity_increase						= { 2 }
	-- self.values.trip_mine.explosion_size_multiplier = { 1.25, 1.5 }
	self.values.trip_mine.explode_timer_delay					= { 2 }		-- in seconds
	
	self.steps.trip_mine = {}
	self.steps.trip_mine.quantity = { 14, 22, 29, 36, 42, 47}
	self.steps.trip_mine.damage_multiplier = { 6, 32 }
	
	-- self.ammo_bag_base = 4
	self.values.ammo_bag = self.values.ammo_bag or {}
	-- self.values.ammo_bag.ammo_increase				= { 2 } -- , 3, 6 } -- { 50, 100, 150, 200, 250, 300, 350, 400 }
	-- self.values.ammo_bag.quantity					= { 1 }
	self.steps.ammo_bag = {}
	self.steps.ammo_bag.ammo_increase = { 10, 19, 30 }
	
	-- self.ecm_jammer_base_battery_life						= 20
	-- self.ecm_jammer_base_low_battery_life				= 8				-- int, no float
	self.values.ecm_jammer = self.values.ecm_jammer or {}
	-- self.values.ecm_jammer.can_activate_feedback	= { true }
	-- self.values.ecm_jammer.can_open_sec_doors	= { true }
	-- self.values.ecm_jammer.quantity = { 1, 3 }
	-- self.values.ecm_jammer.duration_multiplier = { 1.25 }			-- multiplies the battery_life
	-- self.values.ecm_jammer.duration_multiplier_2 = { 1.25 }			-- multiplies the battery_life
	-- self.values.ecm_jammer.affects_cameras = { true } -- prevents security cameras from sending alerts
	-- self.values.ecm_jammer.feedback_duration_boost = { 1.25 }
	-- self.values.ecm_jammer.feedback_duration_boost_2 = { 1.25 }
	
-- Technician DLC --
	self.values.sentry_gun = self.values.sentry_gun or {}
	-- self.values.sentry_gun.damage_multiplier = { 2 }
	-- self.values.sentry_gun.quantity 		= { 1 }
	-- self.values.sentry_gun.extra_ammo_multiplier = { 1.25, 1.75 }
	-- self.values.sentry_gun.armor_multiplier = { 1.5 }
	-- self.values.sentry_gun.spread_multiplier = { 0.6 }
	-- self.values.sentry_gun.rot_speed_multiplier = { 1.7 }
	-- self.values.sentry_gun.shield = { true }
	
	self.steps.sentry_gun = {}
	-- self.sentry_gun_base_ammo = 150
	-- self.sentry_gun_base_armor = 10
		-- self.values.sentry_gun.ammo_increase			= { 100, 150, 200, 250 }
		-- self.values.sentry_gun.armor_increase			= { 5, 10, 15, 20 }
		-- self.steps.sentry_gun.ammo_increase				= { 11, 19, 36, 45 }
		-- self.steps.sentry_gun.armor_increase			= { 10, 17, 24, 33 }
	
	-- self.doctor_bag_base							= 2
	self.values.doctor_bag = self.values.doctor_bag or {}
	-- self.values.doctor_bag.amount_increase			= { 2 } -- { 1, 2, 3 } -- , 4 }
	-- self.values.doctor_bag.quantity					= { 1 } -- , 4 }
	-- self.values.doctor_bag.interaction_speed_multiplier = { 0.80 }
	self.steps.doctor_bag = {}
	self.steps.doctor_bag.amount_increase = { 11, 19, 33 }
	
	self.values.extra_cable_tie = {}
	self.values.extra_cable_tie.quantity = { 1, 2, 3, 4 }
	self.steps.extra_cable_tie = {}
	self.steps.extra_cable_tie.quantity = { nil, 12, 23, 33 }
	
	--[[self.values.c45									= {}
	self.values.c45.clip_ammo_increase				= { 2, 4 } --, 6, 8 }
	self.values.c45.recoil_multiplier				= { 0.9, 0.8, 0.7, 0.6 } --, 0.65, 0.58, 0.51, 0.44 }
	self.values.c45.damage_multiplier				= { 1.1, 1.2, 1.3, 1.4 } -- , 1.40, 1.48, 1.56, 1.64 }
	self.steps.c45									= {}
	self.steps.c45.clip_ammo_increase				= { 20, 29 }
	self.steps.c45.recoil_multiplier				= { 16, 36, 41, 45 }
	self.steps.c45.damage_multiplier				= { 18, 25, 34, 43 }
	
	self.values.beretta92							= {}
	self.values.beretta92.clip_ammo_increase		= { 4, 6 } -- , 6, 8, 10, 12, 14, 16 }
	self.values.beretta92.recoil_multiplier			= { 0.9, 0.8, 0.7, 0.6 } -- , 0.65, 0.58, 0.51, 0.44 }
	self.values.beretta92.spread_multiplier			= { 0.9, 0.8 } -- , 0.7, 0.6 }
	self.steps.beretta92							= {}
	self.steps.beretta92.clip_ammo_increase			= { 3, 8 }
	self.steps.beretta92.recoil_multiplier			= { 3, 9, 21, 30 }
	self.steps.beretta92.spread_multiplier			= { 1, 9 }
	
	self.values.raging_bull							= {}
	self.values.raging_bull.spread_multiplier		= { 0.9, 0.8, 0.7, 0.6 } -- , 0.65, 0.58, 0.51, 0.44 }
	self.values.raging_bull.reload_speed_multiplier	= { 1.1, 1.2 } -- , 1.3, 1.4 }
	self.values.raging_bull.damage_multiplier		= { 1.1, 1.2, 1.3, 1.4 } -- , 1.40, 1.48, 1.56, 1.64 }
	self.steps.raging_bull							= {}
	self.steps.raging_bull.spread_multiplier		= { 10, 20, 34, 43 }
	self.steps.raging_bull.reload_speed_multiplier	= { 14, 26 }
	self.steps.raging_bull.damage_multiplier		= { 8, 17, 30, 46 }
	
	self.values.m4									= {}
	self.values.m4.clip_ammo_increase				= { 5, 10 } --, 6, 8, 10, 12, 14, 16 }
	self.values.m4.spread_multiplier				= { 0.9, 0.8, 0.7, 0.6 } -- , 0.65, 0.58, 0.51, 0.44 }
	self.values.m4.damage_multiplier				= { 1.1, 1.2 } -- , 1.39, 1.52 }
	self.steps.m4									= {}
	self.steps.m4.clip_ammo_increase				= { 3, 12 }
	self.steps.m4.spread_multiplier					= { 1, 5, 12, 16 }
	self.steps.m4.damage_multiplier					= { 1, 15 }
	
	self.values.m14									= {}
	self.values.m14.clip_ammo_increase				= { 2, 4 } -- , 6, 8 }
	self.values.m14.recoil_multiplier				= { 0.9, 0.8, 0.7, 0.6 } -- , 0.65, 0.58, 0.51, 0.44 }
	self.values.m14.spread_multiplier				= { 0.9, 0.8 } -- , 0.7, 0.6 }
	self.values.m14.damage_multiplier				= { 1.1, 1.2 } -- , 1.39, 1.52 }
	self.steps.m14									= {}
	self.steps.m14.clip_ammo_increase				= { 35, 45 }
	self.steps.m14.recoil_multiplier				= { 24, 37, 41, 48 }
	self.steps.m14.spread_multiplier				= { 19, 31 }
	self.steps.m14.damage_multiplier				= { 26, 43 }
		
	self.values.r870_shotgun						= {}
	self.values.r870_shotgun.clip_ammo_increase		= { 2, 4 } -- , 3, 4 }
	self.values.r870_shotgun.recoil_multiplier		= { 0.9, 0.8, 0.7, 0.6 } --, 0.65, 0.58, 0.51, 0.44 }
	self.values.r870_shotgun.damage_multiplier		= { 1.1, 1.2, 1.3, 1.4 } --, 1.40, 1.48, 1.56, 1.64 }
	self.steps.r870_shotgun							= {}
	self.steps.r870_shotgun.clip_ammo_increase		= { 18, 28 }
	self.steps.r870_shotgun.recoil_multiplier		= { 22, 36, 41, 48 }
	self.steps.r870_shotgun.damage_multiplier		= { 15, 25, 39, 44 }
	
	self.values.mossberg							= {}
	self.values.mossberg.clip_ammo_increase			= { 1, 2 }
	self.values.mossberg.reload_speed_multiplier	= { 1.1, 1.2 } -- { 1.07, 1.14, 1.21, 1.28, 1.35, 1.42, 1.49, 1.56 }
	self.values.mossberg.fire_rate_multiplier		= { 1.2, 1.3, 1.4, 1.6 } --, 1.40, 1.48, 1.56, 1.64 }
	self.values.mossberg.recoil_multiplier			= { 0.9, 0.8 }
	-- self.values.mossberg.melee_multiplier			= { 1.2, 1.4 }
	self.steps.mossberg								= {}
	self.steps.mossberg.clip_ammo_increase			= { 13, 38 }
	self.steps.mossberg.reload_speed_multiplier		= { 15, 40 }
	self.steps.mossberg.fire_rate_multiplier		= { 10, 23, 34, 44 }
	self.steps.mossberg.recoil_multiplier			= { 28, 46 }
	
	self.values.mp5										= {}
	self.values.mp5.recoil_multiplier					= { 0.85, 0.7 } -- , 0.7, 0.6 }
	self.values.mp5.spread_multiplier					= { 0.75, 0.5 } --, 0.7, 0.6 }
	self.values.mp5.reload_speed_multiplier				= { 1.1, 1.2, 1.3, 1.4 } --, 1.35, 1.42, 1.49, 1.56 }
	self.values.mp5.enter_steelsight_speed_multiplier	= { 1.5, 1.8 } -- , 1.3, 1.4 }
	self.steps.mp5										= {}
	self.steps.mp5.recoil_multiplier					= { 32, 40 }
	self.steps.mp5.spread_multiplier					= { 27, 45 }
	self.steps.mp5.reload_speed_multiplier				= { 31, 37, 42, 47 }
	self.steps.mp5.enter_steelsight_speed_multiplier	= { 23, 35 }
	
	self.values.mac11									= {}
	self.values.mac11.clip_ammo_increase				= { 6, 12, 18, 24 }
	self.values.mac11.recoil_multiplier					= { 0.9, 0.8, 0.7, 0.6 }
	self.values.mac11.enter_steelsight_speed_multiplier	= { 1.1, 1.2 }
	self.steps.mac11									= {}
	self.steps.mac11.recoil_multiplier					= { 6, 11, 21, 35 }
	self.steps.mac11.clip_ammo_increase					= { 9, 17, 32, 40 }
	self.steps.mac11.enter_steelsight_speed_multiplier	= { 14, 28 }
	
	self.values.hk21 = {}
	self.values.hk21.clip_ammo_increase				= { 20, 40, 60, 80 }
	self.values.hk21.recoil_multiplier				= { 0.9, 0.8 }
	self.values.hk21.damage_multiplier				= { 1.1, 1.2, 1.3, 1.4 }
	self.steps.hk21 = {}
	self.steps.hk21.clip_ammo_increase				= { 24, 37, 42, 48 }
	self.steps.hk21.recoil_multiplier					= { 31, 46 }
	self.steps.hk21.damage_multiplier					= { 27, 39, 44, 47}
	
-- Technician DLC -- 
	self.values.ak47 = {}
	self.values.ak47.spread_multiplier				= { 0.9, 0.8 }
	self.values.ak47.recoil_multiplier				= { 0.95, 0.9, 0.85, 0.8 }
	self.values.ak47.damage_multiplier				= { 1.1, 1.2, 1.25, 1.3 }
	self.values.ak47.clip_ammo_increase				= { 5, 10 }
	self.steps.ak47 = {}
	self.steps.ak47.spread_multiplier					= { 18, 40 }
	self.steps.ak47.recoil_multiplier					= { 12, 22, 41, 48 }
	self.steps.ak47.damage_multiplier					= { 15, 30, 37, 44 }
	self.steps.ak47.clip_ammo_increase				= { 27, 32 }
	
	self.values.glock = {}
	self.values.glock.recoil_multiplier					= { 0.9, 0.8 }
	self.values.glock.clip_ammo_increase				= { 5, 10, 15, 20 }
	self.values.glock.damage_multiplier					= { 1.1, 1.2 }
	self.values.glock.reload_speed_multiplier		= { 1.1, 1.2 }
	self.steps.glock = {}
	self.steps.glock.recoil_multiplier					= { 14, 26 }	
	self.steps.glock.clip_ammo_increase					= { 3, 20, 34, 43 }
	self.steps.glock.damage_multiplier					= { 8, 46 }
	self.steps.glock.reload_speed_multiplier		= { 4, 13 }
	
	self.values.m79 = {}
	self.values.m79.damage_multiplier						= { 1.1, 1.2, 1.3, 1.4 }
	self.values.m79.explosion_range_multiplier	= { 1.1, 1.2 }
	self.values.m79.clip_amount_increase				= { 1, 3 }
	self.steps.m79 = {}
	self.steps.m79.damage_multiplier						= { 31, 35, 42, 47 }
	self.steps.m79.explosion_range_multiplier		= { 23, 28 }
	self.steps.m79.clip_amount_increase					= { 25, 39 }
	]]
	
-- UpgradesManager / upgrade tree
	-- trees 1, 2, 3, 4
	-- tree 1 = assault
	-- tree 2 = sharpshooters
	-- tree 3 = support
	-- tree 4 = technician
	--[[
	self.itree_caps = {}
	table.insert( self.itree_caps, { step = 5, level = 8 } )
	table.insert( self.itree_caps, { step = 13, level = 20 } )
	table.insert( self.itree_caps, { step = 44, level = 88 } )
	table.insert( self.itree_caps, { step = 49, level = managers.dlc:has_dlc1() and 192 or 144 } )
	table.insert( self.itree_caps, { step = 50, level = managers.dlc:has_dlc1() and 193 or 145 } )
	
	
	
	
	self.tree_caps = {}
	for i,d in ipairs( self.itree_caps ) do
		self.tree_caps[ d.step ] = d.level	
	end
	
	self.trees = {}
	self.trees[ 1 ] = {
						name_id = "debug_upgrade_tree_assault",
						}
	self.trees[ 2 ] = {
						name_id = "debug_upgrade_tree_sharpshooter",
						}
	self.trees[ 3 ] = {
						name_id = "debug_upgrade_tree_support",
						}
	self.trees[ 4 ] = {
						name_id = "debug_upgrade_tree_technician",
						}
	]]

	self.definitions = {}
	
	self:_player_definitions()
	self:_trip_mine_definitions()
	self:_ecm_jammer_definitions()
	self:_ammo_bag_definitions()
	self:_doctor_bag_definitions()
	self:_cable_tie_definitions()
	self:_sentry_gun_definitions()
	self:_rep_definitions()
		
	--[[self:_c45_definitions()
	self:_beretta92_definitions()
	self:_raging_bull_definitions()
	self:_m4_definitions()
	self:_m14_definitions()
	self:_mp5_definitions()
	self:_mac11_definitions()
	self:_remington_definitions()
	self:_mossberg_definitions()
	self:_hk21_definitions()
	self:_ak47_definitions()
	self:_glock_definitions()
	self:_m79_definitions()]]
	
	-- New
	self:_olympic_definitions()
	self:_amcar_definitions()
	self:_m16_definitions()
	self:_new_m4_definitions()
	self:_glock_18c_definitions()
	self:_saiga_definitions()
	self:_akmsu_definitions()
	self:_ak74_definitions()
	self:_akm_definitions()
	self:_ak5_definitions()
	self:_aug_definitions()
	self:_g36_definitions()
	self:_p90_definitions()
	self:_new_m14_definitions()
	self:_mp9_definitions()
	self:_deagle_definitions()
	self:_new_mp5_definitions()
	self:_colt_1911_definitions()
	self:_mac10_definitions()
	self:_glock_17_definitions()
	self:_b92fs_definitions()
	self:_huntsman_definitions()
	self:_r870_definitions()
	self:_serbu_definitions()
	self:_new_raging_bull_definitions()
	self:_saw_definitions()
	
	self:_weapon_definitions()
	self:_pistol_definitions()
	self:_assault_rifle_definitions()
	self:_smg_definitions()
	self:_shotgun_definitions()
	
	self:_carry_definitions()
	-- self:_ammo_definitions()
	self:_team_definitions()
	self:_temporary_definitions()
	
	self:_shape_charge_definitions()
	
	self.definitions[ "lucky_charm" ]			= {
														category 		= "what_is_this",
														name_id			= "menu_lucky_charm",
														}
	
	-- Money 
	--[[self.definitions[ "money_bag1" ]	= {
														category 	= "money_multiplier",
														name_id		= "debug_upgrade_money_bag1",
														icon		= "equipment_money_bag",
														multiplier	= "money_bag1",
														unlock_lvl  = 7,
														prio		= "high",
														}
	self.definitions[ "money_bag2" ]	= {
														category 	= "money_multiplier",
														name_id		= "debug_upgrade_money_bag2",
														icon		= "equipment_money_bag",
														multiplier	= "money_bag2",
														unlock_lvl  = 10,
														prio		= "high",
														depends_on	= "money_bag1",
														}]]
	
	self.levels					= {}
		
	for name,upgrade in pairs( self.definitions ) do
		local unlock_lvl = upgrade.unlock_lvl or 1
		self.levels[ unlock_lvl ] = self.levels[ unlock_lvl ] or {}
		if upgrade.prio and upgrade.prio == "high" then
			table.insert( self.levels[ unlock_lvl ], 1, name )
		else
			table.insert( self.levels[ unlock_lvl ], name )
		end
	end
	
	self.progress				= { {}, {}, {}, {} }
		
	for name,upgrade in pairs( self.definitions ) do
		if upgrade.tree then
			if upgrade.step then
				if self.progress[ upgrade.tree ][ upgrade.step ] then
					Application:error( "upgrade collision",  upgrade.tree, upgrade.step, self.progress[ upgrade.tree ][ upgrade.step ], name  )
				end
				self.progress[ upgrade.tree ][ upgrade.step ] = name
				-- print( "HAD STEP", name, upgrade.step )
				--[[if self.progress[ upgrade.tree ][ upgrade.step ] then
					table.insert( self.progress[ upgrade.tree ], self.progress[ upgrade.tree ][ upgrade.step ] ) 
					self.progress[ upgrade.tree ][ upgrade.step ] = name
				end]]
			else
				print( name, upgrade.tree, "is in no step" )
			-- Should be predefined
				-- table.insert( self.progress[ upgrade.tree ], name )
			end 
		end
	end
	
	self.progress[ 1 ][ 49 ] = "mr_nice_guy"
	self.progress[ 2 ][ 49 ] = "mr_nice_guy"
	self.progress[ 3 ][ 49 ] = "mr_nice_guy"
	self.progress[ 4 ][ 49 ] = "mr_nice_guy"
end


function UpgradesTweakData:_init_value_tables()
	self.values = {}
	
	--[[ P L A Y E R ]]
	self.values.player = {}
	self.values.carry = {}
	
	
	--[[ D E P L O Y A B L E ]]
	self.values.trip_mine = {}
	self.values.ammo_bag = {}
	self.values.ecm_jammer = {}
	self.values.sentry_gun = {}
	self.values.doctor_bag = {}
	self.values.cable_tie = {}
	
	
	--[[ W E A P O N ]]
	self.values.weapon = {}
	self.values.pistol = {}
	self.values.assault_rifle = {}
	self.values.smg = {}
	self.values.shotgun = {}
	self.values.saw = {}
	
	
	--[[ T E M P O R A R Y ]]
	self.values.temporary = {}
	
	
	--[[ T E A M ]]
	self.values.team = {}
	self.values.team.weapon = {}
	self.values.team.pistol = {}
	self.values.team.xp = {}
	self.values.team.armor = {}
	self.values.team.stamina = {}
end


function UpgradesTweakData:_init_values()
	self.values.weapon = self.values.weapon or {}
	self.values.weapon.reload_speed_multiplier			= { 1 }
	self.values.weapon.damage_multiplier						= { 1 }
	-- self.values.weapon.clip_ammo_increase				= { 2, 6 }
	self.values.weapon.swap_speed_multiplier				= { 1.25 }
	-- self.values.weapon.passive_swap_speed_multiplier	= { 1.1, 1.35 }
	self.values.weapon.passive_reload_speed_multiplier = { 1.1 }
	-- .values.weapon.passive_recoil_multiplier = { 0.85, 0.6 }
	-- self.values.weapon.passive_headshot_damage_multiplier = { 1.25 }
	-- self.values.weapon.passive_damage_multiplier = { 1.25 } 
	-- self.values.weapon.special_damage_taken_multiplier = { 1.05 } 
	-- self.values.weapon.silencer_spread_multiplier = { 0.5 }
	-- self.values.weapon.silencer_recoil_multiplier = { 0.5 }
	-- self.values.weapon.silencer_damage_multiplier = { 1.1, 1.3 }
	-- self.values.weapon.silencer_enter_steelsight_speed_multiplier = { 2 }
	self.values.weapon.auto_spread_multiplier 			= { 1 } 
	-- self.values.weapon.single_spread_multiplier = { 0.5 } 
	self.values.weapon.spread_multiplier						= { 0.9 }
	self.values.weapon.fire_rate_multiplier					= { 2 }
	
		
	self.values.pistol = self.values.pistol or {}
	-- self.values.pistol.reload_speed_multiplier			= { 1.25 }
	-- self.values.pistol.damage_multiplier						= { 1.25 }
	-- self.values.pistol.spread_multiplier						= { 0.9 }
	-- self.values.pistol.swap_speed_multiplier				= { 1.25 }
	-- self.values.pistol.fire_rate_multiplier					= { 2 }
	self.values.pistol.exit_run_speed_multiplier 		= {	1.25 }
	-- self.values.pistol.hip_fire_spread_multiplier = { 0.5 }
	
	self.values.assault_rifle = self.values.assault_rifle or {}
	-- self.values.assault_rifle.reload_speed_multiplier							= { 1.25 }	-- SKILL: Fire and Move: PRO
	-- self.values.assault_rifle.recoil_multiplier										= { 0.75 }	-- SKILL: Rifleman: STD
	-- self.values.assault_rifle.move_spread_multiplier 							= { 0.5 }
	-- self.values.assault_rifle.hip_fire_spread_multiplier 					= { 0.5 }		-- SKILL: From the Hip: PRO
	-- self.values.assault_rifle.enter_steelsight_speed_multiplier		= { 2 }			-- SKILL: Sharpshooter: STD
	-- self.values.assault_rifle.zoom_increase												= { 1 }			-- SKILL: Rifleman: PRO			 increase in the tweak_data.weapon.stats.zoom		table
	
	self.values.smg = self.values.smg or {}
	-- self.values.smg.reload_speed_multiplier		= { 1.25 }	-- SKILL: SMG Training: STD
	-- self.values.smg.recoil_multiplier					= { 0.75 }	-- SKILL: SMG Training: PRO
	-- self.values.smg.hip_fire_spread_multiplier = { 0.5 }
		
	self.values.shotgun = self.values.shotgun or {}
	-- self.values.shotgun.reload_speed_multiplier			= { 1.5 }
	-- self.values.shotgun.enter_steelsight_speed_multiplier = { 2 }
	-- self.values.shotgun.recoil_multiplier				= { 0.75 }
	-- self.values.shotgun.damage_multiplier				= { 1.25 }
	-- self.values.shotgun.hip_fire_spread_multiplier 			= { 0.5 }		-- SKILL: From the Hip: STD
	
	self.values.carry = self.values.carry or{}
	-- self.values.carry.movement_speed_multiplier			= { 1.2 }
	-- self.values.carry.throw_distance_multiplier			= { 1.3 }
	-- self.values.carry.interact_speed_multiplier			= { 0.9, 0.5 } -- Transporter Skill Std and ace
	self.values.carry.catch_interaction_speed			= { 0.6, 0.1 } -- Transporter Skill Std and ace
	
	self.values.cable_tie = self.values.cable_tie or{}
	-- self.values.cable_tie.quantity						= { 8 }
	-- self.values.cable_tie.interact_speed_multiplier		= { 0.5 }
	-- self.values.cable_tie.can_cable_tie_doors			= { true }
	self.values.cable_tie.quantity_unlimited = { true }
	
	self.values.temporary = self.values.temporary or {}
	-- self.values.temporary.combat_medic_damage_multiplier					= { { 1.25, 10 }, { 1.25, 15 } } -- value, time
	self.values.temporary.combat_medic_enter_steelsight_speed_multiplier 	= { { 1.2, 15 } } -- value, time
	-- self.values.temporary.dmg_multiplier_outnumbered 	= { { 1.1, 7 } } -- value, time
	-- self.values.temporary.dmg_dampener_outnumbered 	= { { 0.9, 7 } } -- value, time
	-- self.values.temporary.overkill_damage_multiplier = { { 2, 6 } } -- value, time
	self.values.temporary.wolverine_health_regen = { { 0.001, 5 } } -- value, time
	self.values.temporary.pistol_revive_from_bleed_out	= { {true, 3} }
	-- self.values.temporary.no_ammo_cost	= { {true, 5}, {true, 15} }
	self.values.temporary.revive_health_boost	= { {true, 10} }
	-- self.revive_health_multiplier	= { 1.5 }			-- reads this value when temporary.revive_health_boost is active, because im not the one with the skill, I just gets the effect.
	
	
	
	--[[self.values.team											= {}
	self.values.team.pistol_suppression_recoil_multiplier		= { 0.2 }
	self.values.team.pistol_recoil_multiplier					= { 0.1 }]]
	
	self.values.team = self.values.team or {}
	self.values.team.pistol = self.values.team.pistol or {}
	-- self.values.team.pistol.suppression_recoil_multiplier		= { 0.75 }
	-- self.values.team.pistol.recoil_multiplier					= { 0.75 }
	self.values.team.weapon = self.values.team.weapon or {}
	self.values.team.weapon.suppression_recoil_multiplier		= { 0.75 }
	self.values.team.weapon.recoil_multiplier								= { 0.75 }
	self.values.team.xp = self.values.team.xp or {}
	-- self.values.team.xp.multiplier								= { 1.05 }
	self.values.team.armor = self.values.team.armor or {}
	-- self.values.team.armor.regen_time_multiplier			= { 0.8 }
	-- self.values.team.armor.passive_regen_time_multiplier			= { 0.95 }
	self.values.team.stamina = self.values.team.stamina or {}
	-- self.values.team.stamina.multiplier						= { 1.5 }
	-- self.values.team.stamina.passive_multiplier		= { 1.15, 1.30 }
	
	self.values.saw = self.values.saw or {}
	-- self.values.saw.extra_ammo_multiplier				= { 2 } -- Will double amount of blades
	self.values.saw.recoil_multiplier												= { 0.75 }
	self.values.saw.fire_rate_multiplier										= { 1.25, 1.50 }
	-- self.values.saw.lock_damage_multiplier		= { 1.25, 1.50 }
	-- self.values.saw.enemy_slicer						= { true } -- Less ammo usage when killing ehemies
	-- self.values.saw.hip_fire_spread_multiplier = { 0.5 }
end

-- Player ------------------------------------------------------------
function UpgradesTweakData:_player_definitions()
	self.definitions[ "body_armor1" ]			= {	
														category 		= "armor",
														armor_id		= "level_2",
														name_id			= "bm_armor_level_2",
														}
	self.definitions[ "body_armor2" ]			= {	
														category 		= "armor",
														armor_id		= "level_3",
														name_id			= "bm_armor_level_3",
														}
	self.definitions[ "body_armor3" ]			= {	
														category 		= "armor",
														armor_id		= "level_4",
														name_id			= "bm_armor_level_4",
														}
	self.definitions[ "body_armor4" ]			= {	
														category 		= "armor",
														armor_id		= "level_5",
														name_id			= "bm_armor_level_5",
														}
	self.definitions[ "body_armor5" ]			= {	
														category 		= "armor",
														armor_id		= "level_6",
														name_id			= "bm_armor_level_6",
														}
	self.definitions[ "body_armor6" ]			= {	
														category 		= "armor",
														armor_id		= "level_7",
														name_id			= "bm_armor_level_7",
														}

	--[[self.definitions[ "body_armor" ]			= {	
														category 		= "equipment",
														equipment_id	= "body_armor",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														-- name_id			= "debug_body_armor",
														name_id			= "debug_upgrade_body_armor1",
														icon			= "equipment_armor",
														image			= "upgrades_bodyarmor",
														image_slice		= "upgrades_bodyarmor_slice",
														unlock_lvl  	= 0,
														description_text_id = "body_armor",
														-- prio			= "high",
														-- aquire			=  { upgrade = "body_armor1" },
														slot			= 0,
														}

	-- Body armor increase
	for i,_ in ipairs( self.values.player.body_armor ) do
		local depends_on = (i-1) > 0 and "body_armor"..(i-1)
		local unlock_lvl = 3										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "body_armor"..i ] = {		
														incremental = true,
														category 	= "feature",
														name_id		= "debug_upgrade_body_armor"..i,
														title_id	= "debug_upgrade_body_armor",
														subtitle_id	= "debug_upgrade_body_armor_increase",
														icon		= "equipment_armor",
														image			= "upgrades_bodyarmor",
														image_slice		= "upgrades_bodyarmor_slice",
														description_text_id = "body_armor",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "player",
																		upgrade 	= "body_armor",
																		value 		= i
																		}
														}
	end
	
	-- THIS IS TEMP
	self.definitions[ "body_armor1" ].tree = 1
	self.definitions[ "body_armor1" ].step = 7
	self.definitions[ "body_armor2" ].tree = 2
	self.definitions[ "body_armor2" ].step = 11
	self.definitions[ "body_armor3" ].tree = 2
	self.definitions[ "body_armor3" ].step = 25
	self.definitions[ "body_armor4" ].tree = 3
	self.definitions[ "body_armor4" ].step = 4
	self.definitions[ "body_armor5" ].tree = 4
	self.definitions[ "body_armor5" ].step = 6]]
		
	-- Thick skin
	self.definitions[ "thick_skin" ]			= {	
														tree			= 2,
														step			= 2,
														category 		= "equipment",
														equipment_id	= "thick_skin",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														-- name_id			= "debug_thick_skin",
														title_id		= "debug_upgrade_player_upgrade",
														subtitle_id		= "debug_upgrade_thick_skin1",
														name_id			= "debug_upgrade_thick_skin1",
														icon			= "equipment_armor",
														image			= "upgrades_thugskin",
														image_slice		= "upgrades_thugskin_slice",
														description_text_id = "thick_skin",
														unlock_lvl  	= 0,
														-- prio			= "high",
														aquire			=  { upgrade = "thick_skin1" },
														slot			= 2,
														}
														
	-- Thick skin increase
	for i,_ in ipairs( self.values.player.thick_skin ) do
		local depends_on = (i-1) > 0 and "thick_skin"..(i-1)
		local unlock_lvl = 3										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "thick_skin"..i ] = {	
														tree		= 2,
														step		= self.steps.player.thick_skin[ i ],
														category 	= "feature",
														title_id	= "debug_upgrade_player_upgrade",
														subtitle_id	= "debug_upgrade_thick_skin"..i,
														name_id		= "debug_upgrade_thick_skin"..i,
														icon		= "equipment_thick_skin",
														image		= "upgrades_thugskin",
														image_slice	= "upgrades_thugskin_slice",
														description_text_id = "thick_skin",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "player",
																		upgrade 	= "thick_skin",
																		value 		= i
																		}
														}
	end
	
	
		
	--[[self.definitions[ "bleed_out_increase" ]			= {
														category 		= "equipment",
														equipment_id	= "bleed_out_increase",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_upgrade_bleed_out_increase1",
														icon			= "equipment_bleed_out",
														unlock_lvl  	= 7,
														prio			= "high",
														aquire			=  { upgrade = "bleed_out_increase1" },
														slot			= 3,
														}

	-- Bleed out time increase
	for i,_ in ipairs( self.values.player.bleed_out_increase ) do
		local depends_on = (i-1) > 0 and "bleed_out_increase"..(i-1)
		local unlock_lvl = 8										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "bleed_out_increase"..i ] = {	
														category 	= "feature",
														name_id		= "debug_upgrade_bleed_out_increase"..i,
														icon		= "equipment_bleed_out",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "player",
																		upgrade 	= "bleed_out_increase",
																		value 		= i
																		}
														}
	end]]
	
	--[[self.definitions[ "intimidation" ]			= {
														category 		= "equipment",
														equipment_id	= "intimidation",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_upgrade_intimidation_increase1",
														icon			= "interaction_intimidate",
														unlock_lvl  	= 11,
														prio			= "high",
														aquire			=  { upgrade = "intimidation_multiplier1" },
														slot			= 3,
														}

	-- Intimidation multiplier
	for i,_ in ipairs( self.values.player.intimidation_multiplier ) do
		local depends_on = (i-1) > 0 and "intimidation_multiplier"..(i-1)
		local unlock_lvl = 12										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "intimidation_multiplier"..i ] = {	
														category 	= "feature",
														name_id		= "debug_upgrade_intimidation_increase"..i,
														icon		= "interaction_intimidate",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "player",
																		upgrade 	= "intimidation_multiplier",
																		value 		= i
																		}
														}
	end]]
	
	self.definitions[ "extra_start_out_ammo" ]			= {
														tree			= 3,
														step			= 2,
														category 		= "equipment",
														equipment_id	= "extra_start_out_ammo",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_upgrade_extra_start_out_ammo1",
														title_id		= "debug_upgrade_player_upgrade",
														subtitle_id		= "debug_upgrade_extra_start_out_ammo1",
														icon			= "equipment_extra_start_out_ammo",
														image			= "upgrades_extrastartammo",
														image_slice		= "upgrades_extrastartammo_slice",
														description_text_id = "extra_ammo_multiplier",
														unlock_lvl  	= 13,
														prio			= "high",
														aquire			=  { upgrade = "extra_ammo_multiplier1" },
														slot			= 2,
														}

	-- Extra start out ammo multiplier
	for i,_ in ipairs( self.values.player.extra_ammo_multiplier ) do
		local depends_on = (i-1) > 0 and "extra_ammo_multiplier"..(i-1)
		local unlock_lvl = 14										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "extra_ammo_multiplier"..i ] = {
														tree		= 3,
														step		= self.steps.player.extra_ammo_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_extra_start_out_ammo"..i,
														title_id	= "debug_upgrade_player_upgrade",
														subtitle_id	= "debug_upgrade_extra_start_out_ammo"..i,
														icon		= "equipment_extra_start_out_ammo",
														image		= "upgrades_extrastartammo",
														image_slice	= "upgrades_extrastartammo_slice",
														description_text_id = "extra_ammo_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "player",
																		upgrade 	= "extra_ammo_multiplier",
																		value 		= i
																		}
														}
	end
		
	self.definitions[ "player_flashbang_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_flashbang_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "flashbang_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_flashbang_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_flashbang_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "flashbang_multiplier",
																		value 		= 2
																		}
														}
	
	self.definitions[ "player_pick_up_ammo_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_pick_up_ammo_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "pick_up_ammo_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_primary_weapon_when_downed" ] = {
														category 	= "feature",
														name_id		= "menu_player_primary_weapon_when_downed",
														upgrade		= { category 	= "player",
																		upgrade 	= "primary_weapon_when_downed",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_primary_weapon_when_carrying" ] = {
														category 	= "feature",
														name_id		= "menu_player_primary_weapon_when_carrying",
														upgrade		= { category 	= "player",
																		upgrade 	= "primary_weapon_when_carrying",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_pistol_revive_from_bleed_out_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_pistol_revive_from_bleed_out",
														upgrade		= { category 	= "player",
																		upgrade 	= "pistol_revive_from_bleed_out",
																		value 		= 1
																		}
														}
	self.definitions[ "player_pistol_revive_from_bleed_out_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_pistol_revive_from_bleed_out",
														upgrade		= { category 	= "player",
																		upgrade 	= "pistol_revive_from_bleed_out",
																		value 		= 2
																		}
														}
														
	self.definitions[ "player_pistol_revive_from_bleed_out_timer" ] = {
														category 	= "temporary",
														name_id		= "menu_player_pistol_revive_from_bleed_out_timer",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "pistol_revive_from_bleed_out",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_can_strafe_run" ] = {
														category 	= "feature",
														name_id		= "menu_player_can_strafe_run",
														upgrade		= { category 	= "player",
																		upgrade 	= "can_strafe_run",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_can_free_run" ] = {
														category 	= "feature",
														name_id		= "menu_player_can_free_run",
														upgrade		= { category 	= "player",
																		upgrade 	= "can_free_run",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_damage_shake_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_damage_shake_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "damage_shake_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_health_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "health_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_passive_health_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_health_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_health_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_health_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_passive_health_multiplier_3" ] = {
														category 	= "feature",
														name_id		= "menu_player_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_health_multiplier",
																		value 		= 3
																		}
														}
	self.definitions[ "player_bleed_out_health_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_bleed_out_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "bleed_out_health_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_revive_interaction_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_revive_interaction_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "revive_interaction_speed_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_shield_knock" ] = {
														category 	= "feature",
														name_id		= "menu_player_shield_knock",
														upgrade		= { category 	= "player",
																		upgrade 	= "shield_knock",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_steelsight_when_downed" ] = {
														category 	= "feature",
														name_id		= "menu_player_steelsight_when_downed",
														upgrade		= { category 	= "player",
																		upgrade 	= "steelsight_when_downed",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_armor_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_armor_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "armor_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_armor_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_passive_armor_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_armor_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_armor_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_passive_armor_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_armor_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_armor_regen_timer_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_armor_regen_timer_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "armor_regen_timer_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_wolverine_health_regen" ] = {
														category 	= "temporary",
														name_id		= "menu_player_wolverine_health_regen",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "wolverine_health_regen",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_stamina_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_stamina_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "stamina_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_stamina_regen_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_stamina_regen_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "stamina_regen_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_stamina_regen_timer_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_stamina_regen_timer_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "stamina_regen_timer_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_run_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_run_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "run_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_passive_dodge_chance_1" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_run_dodge_chance",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_dodge_chance",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_passive_dodge_chance_2" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_run_dodge_chance",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_dodge_chance",
																		value 		= 2
																		}
														}
														
	self.definitions[ "player_run_dodge_chance" ] = {
														category 	= "feature",
														name_id		= "menu_player_run_dodge_chance",
														upgrade		= { category 	= "player",
																		upgrade 	= "run_dodge_chance",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_walk_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_walk_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "walk_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_crouch_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_crouch_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "crouch_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_fall_damage_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_fall_damage_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "fall_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_fall_health_damage_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_fall_health_damage_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "fall_health_damage_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_interacting_damage_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_interacting_damage_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "interacting_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_damage_health_ratio_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_damage_health_ratio_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "damage_health_ratio_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_melee_damage_health_ratio_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_melee_damage_health_ratio_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "melee_damage_health_ratio_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_melee_damage_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_melee_damage_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "melee_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_respawn_time_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_respawn_time_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "respawn_time_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "passive_player_xp_multiplier" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_xp_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_xp_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_xp_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_xp_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "xp_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_non_special_melee_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_non_special_melee_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "non_special_melee_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_passive_suspicion_bonus" ] = {
														category 	= "feature",
														name_id		= "menu_player_passive_suspicion_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_suspicion_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_suspicion_bonus" ] = {
														category 	= "feature",
														name_id		= "menu_player_suspicion_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "suspicion_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_uncover_progress_mul" ] = {
														category 	= "feature",
														name_id		= "player_uncover_progress_mul",
														upgrade		= { category 	= "player",
																		upgrade 	= "uncover_progress_mul",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_uncover_progress_decay_mul" ] = {
														category 	= "feature",
														name_id		= "menu_player_uncover_progress_decay_mul",
														upgrade		= { category 	= "player",
																		upgrade 	= "uncover_progress_decay_mul",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_camouflage_bonus" ] = {
														category 	= "feature",
														name_id		= "menu_player_camouflage_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "camouflage_bonus",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_suppressed_bonus" ] = {
														category 	= "feature",
														name_id		= "menu_player_suppressed_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "suppressed_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_passive_suppression_bonus_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_suppression_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_suppression_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_passive_suppression_bonus_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_suppression_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_suppression_multiplier",
																		value 		= 2
																		}
														}
														
	self.definitions[ "player_suppression_bonus" ] = {
														category 	= "feature",
														name_id		= "menu_player_suppression_bonus",
														upgrade		= { category 	= "player",
																		upgrade 	= "suppression_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_civilian_reviver" ] = {
														category 	= "feature",
														name_id		= "menu_player_civilian_reviver",
														upgrade		= { category 	= "player",
																		upgrade 	= "civilian_reviver",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_overkill_damage_multiplier" ] = {
														category 	= "temporary",
														name_id		= "menu_player_overkill_damage_multiplier",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "overkill_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_overkill_all_weapons" ] = {
														category 	= "feature",
														name_id		= "menu_player_overkill_all_weapons",
														upgrade		= { category 	= "player",
																		upgrade 	= "overkill_all_weapons",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_damage_multiplier_outnumbered" ] = {
														category 	= "temporary",
														name_id		= "menu_player_dmg_mul_outnumbered",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "dmg_multiplier_outnumbered",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_damage_dampener_outnumbered" ] = {
														category 	= "temporary",
														name_id		= "menu_player_dmg_damp_outnumbered",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "dmg_dampener_outnumbered",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_corpse_alarm_pager_bluff" ] = {
														category 	= "feature",
														name_id		= "menu_player_pager_dis",
														upgrade		= { category 	= "player",
																		upgrade 	= "corpse_alarm_pager_bluff",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_corpse_dispose" ] = {
														category 	= "feature",
														name_id		= "menu_player_corpse_disp",
														upgrade		= { category 	= "player",
																		upgrade 	= "corpse_dispose",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_taser_malfunction" ] = {
														category 	= "feature",
														name_id		= "menu_player_taser_malf",
														upgrade		= { category 	= "player",
																		upgrade 	= "taser_malfunction",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_taser_self_shock" ] = {
														category 	= "feature",
														name_id		= "menu_player_taser_shock",
														upgrade		= { category 	= "player",
																		upgrade 	= "taser_self_shock",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_electrocution_resistance" ] = {
														category 	= "feature",
														name_id		= "menu_player_electrocution_resistance",
														upgrade		= { category 	= "player",
																		upgrade 	= "electrocution_resistance_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_silent_kill" ] = {
														category 	= "feature",
														name_id		= "menu_player_silent_kill",
														upgrade		= { category 	= "player",
																		upgrade 	= "silent_kill",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_melee_knockdown_mul" ] = {
														category 	= "feature",
														name_id		= "menu_player_melee_knockdown_mul",
														upgrade		= { category 	= "player",
																		upgrade 	= "melee_knockdown_mul",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_suppression_mul_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_suppression_mul_2",
														upgrade		= { category 	= "player",
																		upgrade 	= "suppression_multiplier",
																		value 		= 2
																		}
														}
	
	self.definitions[ "player_damage_dampener" ] = {
														category 	= "feature",
														name_id		= "menu_player_damage_dampener",
														upgrade		= { category 	= "player",
																		upgrade 	= "damage_dampener",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_marked_enemy_extra_damage" ] = {
														category 	= "feature",
														name_id		= "menu_player_marked_enemy_extra_damage",
														upgrade		= { category 	= "player",
																		upgrade 	= "marked_enemy_extra_damage",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_civ_intimidation_mul" ] = {
														category 	= "feature",
														name_id		= "menu_player_civ_intimidation_mul",
														upgrade		= { category 	= "player",
																		upgrade 	= "civ_intimidation_mul",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_civ_harmless_bullets" ] = {
														category 	= "feature",
														name_id		= "menu_player_civ_harmless_bullets",
														upgrade		= { category 	= "player",
																		upgrade 	= "civ_harmless_bullets",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_civ_harmless_melee" ] = {
														category 	= "feature",
														name_id		= "menu_player_civ_harmless_melee",
														upgrade		= { category 	= "player",
																		upgrade 	= "civ_harmless_melee",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_civ_calming_alerts" ] = {
														category 	= "feature",
														name_id		= "menu_player_civ_calming_alerts",
														upgrade		= { category 	= "player",
																		upgrade 	= "civ_calming_alerts",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_special_enemy_highlight" ] = {
														category 	= "feature",
														name_id		= "menu_player_special_enemy_highlight",
														upgrade		= { category 	= "player",
																		upgrade 	= "special_enemy_highlight",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_drill_alert" ] = {
														category 	= "feature",
														name_id		= "menu_player_drill_alert",
														upgrade		= { category 	= "player",
																		upgrade 	= "drill_alert_rad",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_silent_drill" ] = {
														category 	= "feature",
														name_id		= "menu_player_silent_drill",
														upgrade		= { category 	= "player",
																		upgrade 	= "silent_drill",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_drill_speed_multiplier1" ] = {
														category 	= "feature",
														name_id		= "menu_player_drill_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "drill_speed_multiplier",
																		value 		= 1
																		}
														}										
	self.definitions[ "player_drill_speed_multiplier2" ] = {
														category 	= "feature",
														name_id		= "menu_player_drill_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "drill_speed_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_saw_speed_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_saw_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "saw_speed_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_saw_speed_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_saw_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "saw_speed_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_drill_fix_interaction_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_drill_fix_interaction_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "drill_fix_interaction_speed_multiplier",
																		value 		= 1
																		}
														}

	self.definitions[ "player_dye_pack_chance_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_dye_pack_chance_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "dye_pack_chance_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_dye_pack_cash_loss_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_dye_pack_cash_loss_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "dye_pack_cash_loss_multiplier",
																		value 		= 1
																		}
														}					
														
	self.definitions[ "player_cheat_death_chance" ] = {
														category 	= "feature",
														name_id		= "menu_player_cheat_death_chance",
														upgrade		= { category 	= "player",
																		upgrade 	= "cheat_death_chance",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_additional_lives_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_additional_lives_1",
														upgrade		= { category 	= "player",
																		upgrade 	= "additional_lives",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_additional_lives_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_additional_lives_2",
														upgrade		= { category 	= "player",
																		upgrade 	= "additional_lives",
																		value 		= 2
																		}
														}
														
																								
	
	self.definitions[ "player_trip_mine_shaped_charge" ] = {
														category 	= "feature",
														name_id		= "menu_player_trip_mine_shaped_charge",
														upgrade		= { category 	= "player",
																		upgrade 	= "trip_mine_shaped_charge",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_small_loot_multiplier1" ] = {
														category 	= "feature",
														name_id		= "menu_player_small_loot_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "small_loot_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_small_loot_multiplier2" ] = {
														category 	= "feature",
														name_id		= "menu_player_small_loot_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "small_loot_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_intimidate_enemies" ] = {
														category 	= "feature",
														name_id		= "menu_player_intimidate_enemies",
														upgrade		= { category 	= "player",
																		upgrade 	= "intimidate_enemies",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_intimidate_specials" ] = {
														category 	= "feature",
														name_id		= "menu_player_intimidate_specials",
														upgrade		= { category 	= "player",
																		upgrade 	= "intimidate_specials",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_empowered_intimidation" ] = {
														category 	= "feature",
														name_id		= "menu_player_passive_empowered_intimidation",
														upgrade		= { category 	= "player",
																		upgrade 	= "empowered_intimidation_mul",
																		value 		= 1
																		}
														}
	self.definitions[ "player_intimidation_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_intimidation_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "intimidation_multiplier",
																		value 		= 1
																		}
														}
														
														
	self.definitions[ "player_sentry_gun_deploy_time_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_sentry_gun_deploy_time_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "sentry_gun_deploy_time_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_trip_mine_deploy_time_multiplier" ] = {
														category 	= "feature",
														incremental = true,
														name_id		= "menu_player_trip_mine_deploy_time_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "trip_mine_deploy_time_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_trip_mine_deploy_time_multiplier_2" ] = {
														category 	= "feature",
														incremental = true,
														name_id		= "menu_player_trip_mine_deploy_time_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "trip_mine_deploy_time_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_convert_enemies" ] = {
														category 	= "feature",
														name_id		= "menu_player_convert_enemies",
														upgrade		= { category 	= "player",
																		upgrade 	= "convert_enemies",
																		value 		= 1
																		}
														}
	self.definitions[ "player_convert_enemies_max_minions_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_convert_enemies_max_minions",
														upgrade		= { category 	= "player",
																		upgrade 	= "convert_enemies_max_minions",
																		value 		= 1
																		}
														}
	self.definitions[ "player_convert_enemies_max_minions_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_convert_enemies_max_minions",
														upgrade		= { category 	= "player",
																		upgrade 	= "convert_enemies_max_minions",
																		value 		= 2
																		}
														}
	self.definitions[ "player_convert_enemies_interaction_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_convert_enemies_interaction_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "convert_enemies_interaction_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_convert_enemies_health_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_convert_enemies_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "convert_enemies_health_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_convert_enemies_health_multiplier" ] = {	-- tier
														category 	= "feature",
														name_id		= "menu_player_passive_convert_enemies_health_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_convert_enemies_health_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_convert_enemies_damage_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_convert_enemies_damage_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "convert_enemies_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_convert_enemies_damage_multiplier" ] = {	-- tier
														category 	= "feature",
														name_id		= "menu_player_passive_convert_enemies_damage_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_convert_enemies_damage_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_passive_intimidate_range_mul" ] = {	-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_intimidate_range_mul",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_intimidate_range_mul",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_intimidate_range_mul" ] = {
														category 	= "feature",
														name_id		= "menu_player_intimidate_range_mul",
														upgrade		= { category 	= "player",
																		upgrade 	= "intimidate_range_mul",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_intimidate_aura" ] = {
														category 	= "feature",
														name_id		= "menu_player_intimidate_aura",
														upgrade		= { category 	= "player",
																		upgrade 	= "intimidate_aura",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_civilian_gives_ammo" ] = {
														category 	= "feature",
														name_id		= "menu_player_civilian_gives_ammo",
														upgrade		= { category 	= "player",
																		upgrade 	= "civilian_gives_ammo",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_drill_autorepair" ] = {
														category 	= "feature",
														name_id		= "menu_player_drill_autorepair",
														upgrade		= { category 	= "player",
																		upgrade 	= "drill_autorepair",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_hostage_trade" ] = {
														category 	= "feature",
														name_id		= "menu_player_hostage_trade",
														upgrade		= { category 	= "player",
																		upgrade 	= "hostage_trade",
																		value 		= 1
																		}
														}
	self.definitions[ "player_sec_camera_highlight" ] = {
														category 	= "feature",
														name_id		= "menu_player_sec_camera_highlight",
														upgrade		= { category 	= "player",
																		upgrade 	= "sec_camera_highlight",
																		value 		= 1
																		}
														}
	self.definitions[ "player_morale_boost" ] = {
														category 	= "feature",
														name_id		= "menu_player_morale_boost",
														upgrade		= { category 	= "player",
																		upgrade 	= "morale_boost",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_long_dis_revive" ] = {
														category 	= "feature",
														name_id		= "menu_player_long_dis_revive",
														upgrade		= { category 	= "player",
																		upgrade 	= "long_dis_revive",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_pick_lock_easy" ] = {
														category 	= "feature",
														name_id		= "menu_player_pick_lock_easy",
														upgrade		= { category 	= "player",
																		upgrade 	= "pick_lock_easy",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_pick_lock_hard" ] = {
														category 	= "feature",
														name_id		= "menu_player_pick_lock_hard",
														upgrade		= { category 	= "player",
																		upgrade 	= "pick_lock_hard",
																		value 		= 1
																		}
														}
	
	self.definitions[ "player_pick_lock_easy_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_pick_lock_easy_speed_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "pick_lock_easy_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "player_loot_drop_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_loot_drop_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "loot_drop_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_loot_drop_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_loot_drop_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "loot_drop_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_passive_loot_drop_multiplier" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_passive_loot_drop_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_loot_drop_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_buy_cost_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_buy_cost_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "buy_cost_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_buy_cost_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_player_buy_cost_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "buy_cost_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_crime_net_deal" ] = {
														category 	= "feature",
														name_id		= "menu_player_crime_net_deal",
														upgrade		= { category 	= "player",
																		upgrade 	= "crime_net_deal",
																		value 		= 1
																		}
														}
	self.definitions[ "player_sell_cost_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_player_sell_cost_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "sell_cost_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_crafting_weapon_multiplier" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_weapon_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "crafting_weapon_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_crafting_weapon_multiplier_1" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_weapon_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_crafting_weapon_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_crafting_weapon_multiplier_2" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_weapon_multiplier_2",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_crafting_weapon_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_passive_crafting_weapon_multiplier_3" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_weapon_multiplier_3",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_crafting_weapon_multiplier",
																		value 		= 3
																		}
														}
														
	self.definitions[ "player_crafting_mask_multiplier" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_mask_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "crafting_mask_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_crafting_mask_multiplier_1" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_mask_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_crafting_mask_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "player_passive_crafting_mask_multiplier_2" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_mask_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_crafting_mask_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "player_passive_crafting_mask_multiplier_3" ] = {		-- tier bonus
														category 	= "feature",
														name_id		= "menu_player_crafting_mask_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_crafting_mask_multiplier",
																		value 		= 3
																		}
														}
	self.definitions[ "player_additional_assets" ] = {
														category 	= "feature",
														name_id		= "menu_player_additional_assets",
														upgrade		= { category 	= "player",
																		upgrade 	= "additional_assets",
																		value 		= 1
																		}
														}
	self.definitions[ "player_assets_cost_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_player_assets_cost_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "assets_cost_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "passive_player_assets_cost_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_passive_player_assets_cost_multiplier",
														upgrade		= { category 	= "player",
																		upgrade 	= "passive_assets_cost_multiplier",
																		value 		= 1
																		}
														}

	self.definitions[ "player_revive_health_boost" ] = {
														category 	= "feature",
														name_id		= "menu_player_revive_health_boost",
														upgrade		= { category 	= "player",
																		upgrade 	= "revive_health_boost",
																		value 		= 1
																		}
														}
														
																									
														
	-- Toolset
	self.definitions[ "toolset" ]			= {	
														tree			= 4,
														step			= 1,
														category 		= "equipment",
														equipment_id	= "toolset",
														title_id		= "debug_upgrade_player_upgrade",
														subtitle_id		= "debug_upgrade_toolset1",
														name_id			= "debug_upgrade_toolset1",
														icon			= "equipment_toolset",
														image			= "upgrades_toolset",
														image_slice		= "upgrades_toolset_slice",
														description_text_id = "toolset",
														unlock_lvl  	= 0,
														-- prio			= "high",
														aquire			=  { upgrade = "toolset1" },
														slot			= 2,
														}
														
	-- Toolset increase
	for i,_ in ipairs( self.values.player.toolset ) do
		local depends_on = (i-1) > 0 and "toolset"..(i-1)
		local unlock_lvl = 3										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "toolset"..i ] = {	
														tree		= 4,
														step		= self.steps.player.toolset[ i ],
														category 	= "feature",
														title_id	= "debug_upgrade_player_upgrade",
														subtitle_id	= "debug_upgrade_toolset"..i,
														name_id		= "debug_upgrade_toolset"..i,
														icon		= "equipment_toolset",
														image		= "upgrades_toolset",
														image_slice	= "upgrades_toolset_slice",
														description_text_id = "toolset",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "player",
																		upgrade 	= "toolset",
																		value 		= i
																		}
														}
	end
	
		
		
		
end

-- TRIP MINE ------------------------------------------------------------
function UpgradesTweakData:_trip_mine_definitions()
	self.definitions[ "trip_mine" ]			= {
														tree			= 2,
														step			= 4,
														category 		= "equipment",
														equipment_id	= "trip_mine",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_trip_mine",
														title_id		= "debug_upgrade_new_equipment",
														subtitle_id		= "debug_trip_mine",
														icon			= "equipment_trip_mine",
														image			= "upgrades_tripmines",
														image_slice		= "upgrades_tripmines_slice",
														description_text_id = "trip_mine",
														unlock_lvl  	= 0,
														prio			= "high",
														slot			= 1,
														}

	self.definitions[ "trip_mine_can_switch_on_off" ] = {
													category 	= "feature",
													name_id		= "menu_trip_mine_can_switch_on_off",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "can_switch_on_off",
																	value 		= 1
																	}
													}
													
	self.definitions[ "trip_mine_sensor_toggle" ] = {
													category 	= "feature",
													name_id		= "menu_trip_mine_sensor_toggle",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "sensor_toggle",
																	value 		= 1
																	}
													}
								
	self.definitions[ "trip_mine_quantity_increase_1" ] = {
													category 	= "feature",
													name_id		= "menu_trip_mine_quantity_increase_1",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "quantity_1",
																	value 		= 1
																	}												
													}
	self.definitions[ "trip_mine_quantity_increase_2" ] = {
													category 	= "feature",
													name_id		= "menu_trip_mine_quantity_increase_1",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "quantity_2",
																	value 		= 1
																	}
													}
	self.definitions[ "trip_mine_quantity_increase_3" ] = {
													category 	= "feature",
													name_id		= "menu_trip_mine_quantity_increase_1",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "quantity_3",
																	value 		= 1
																	}
													}
													
	self.definitions[ "trip_mine_explosion_size_multiplier_1" ] = {
													category 	= "feature",
													incremental = true,
													name_id		= "menu_trip_mine_explosion_size_multiplier",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "explosion_size_multiplier",
																	value 		= 1
																	}
													}						
	self.definitions[ "trip_mine_explosion_size_multiplier_2" ] = {
													category 	= "feature",
													incremental = true,
													name_id		= "menu_trip_mine_explosion_size_multiplier",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "explosion_size_multiplier",
																	value 		= 2
																	}
													}	
	self.definitions[ "trip_mine_explode_timer_delay" ] = {
													category 	= "feature",
													incremental = true,
													name_id		= "menu_trip_mine_explode_timer_delay",
													upgrade		= { category 	= "trip_mine",
																	upgrade 	= "explode_timer_delay",
																	value 		= 1
																	}
													}
														
	--[[
	-- Quantity increase
	for i,_ in ipairs( self.values.trip_mine.quantity ) do
		local depends_on = (i-1) > 0 and "trip_mine_quantity"..(i-1) or "trip_mine"
		local unlock_lvl = 7										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "trip_mine_quantity"..i ] = {	
														tree		= 2,
														step		= self.steps.trip_mine.quantity[ i ],
														category 	= "equipment_upgrade",
														name_id		= "debug_upgrade_trip_mine_quantity"..i,
														title_id	= "debug_trip_mine",
														subtitle_id	= "debug_upgrade_amount_increase"..i,
														icon		= "equipment_trip_mine",
														image		= "upgrades_tripmines",
														image_slice	= "upgrades_tripmines_slice",
														description_text_id = "trip_mine_quantity",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "trip_mine",
																		upgrade 	= "quantity",
																		value 		= i
																		}
														}
	end
	
	-- Damage multiplier
	for i,_ in ipairs( self.values.trip_mine.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "trip_mine_damage"..(i-1) or "trip_mine"
		local unlock_lvl = 7										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "trip_mine_damage"..i ] = {	
														tree		= 2,
														step		= self.steps.trip_mine.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_trip_mine_damage"..i,
														title_id	= "debug_trip_mine",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "equipment_trip_mine",
														image		= "upgrades_tripmines",
														image_slice	= "upgrades_tripmines_slice",
														description_text_id = "trip_mine_damage",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "trip_mine",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end]]
end

-- TRIP MINE ------------------------------------------------------------
function UpgradesTweakData:_ecm_jammer_definitions()
	self.definitions[ "ecm_jammer" ]			= {
														category 		= "equipment",
														equipment_id	= "ecm_jammer",
														name_id			= "menu_equipment_ecm_jammer",
														slot			= 1,
														}

	self.definitions[ "ecm_jammer_can_activate_feedback" ] = {
													category 	= "feature",
													name_id		= "menu_ecm_jammer_can_activate_feedback",
													upgrade		= { category 	= "ecm_jammer",
																	upgrade 	= "can_activate_feedback",
																	value 		= 1
																	}
													}
	self.definitions[ "ecm_jammer_can_open_sec_doors" ] = {
													category 	= "feature",
													name_id		= "menu_ecm_jammer_can_open_sec_doors",
													upgrade		= { category 	= "ecm_jammer",
																	upgrade 	= "can_open_sec_doors",
																	value 		= 1
																	}
													}
													
	self.definitions[ "ecm_jammer_quantity_increase_1" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_quantity_1",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "quantity",
																		value 		= 1
																		}
														}
														
	self.definitions[ "ecm_jammer_quantity_increase_2" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_quantity_2",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "quantity",
																		value 		= 2
																		}
														}

	self.definitions[ "ecm_jammer_duration_multiplier" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_duration_multiplier",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "duration_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "ecm_jammer_duration_multiplier_2" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_duration_multiplier",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "duration_multiplier_2",
																		value 		= 1
																		}
														}
	
	self.definitions[ "ecm_jammer_affects_cameras" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_affects_cameras",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "affects_cameras",
																		value 		= 1
																		}
														}
	self.definitions[ "ecm_jammer_feedback_duration_boost" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_feedback_duration_boost",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "feedback_duration_boost",
																		value 		= 1
																		}
														}
	self.definitions[ "ecm_jammer_feedback_duration_boost_2" ] = {
														category 	= "equipment_upgrade",
														name_id		= "menu_ecm_jammer_feedback_duration_boost_2",
														upgrade		= { category 	= "ecm_jammer",
																		upgrade 	= "feedback_duration_boost_2",
																		value 		= 1
																		}
														}
	
end

-- AMMO BAG ------------------------------------------------------------
function UpgradesTweakData:_ammo_bag_definitions()
	self.definitions[ "ammo_bag" ]			= {
														tree			= 1,
														step			= 2,
														category 		= "equipment",
														equipment_id	= "ammo_bag",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_ammo_bag",
														title_id		= "debug_upgrade_new_equipment",
														subtitle_id		= "debug_ammo_bag",
														icon			= "equipment_ammo_bag",
														image			= "upgrades_ammobag",
														image_slice		= "upgrades_ammobag_slice",
														description_text_id = "ammo_bag",
														unlock_lvl  	= 0,
														prio			= "high",
														slot			= 1,
														}
	
	-- Quantity increase
	for i,_ in ipairs( self.values.ammo_bag.ammo_increase ) do
		local depends_on = (i-1) > 0 and "ammo_bag_ammo_increase"..(i-1) or "ammo_bag"
		local unlock_lvl = 11										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "ammo_bag_ammo_increase"..i ] = {	
														tree		= 1,
														step		= self.steps.ammo_bag.ammo_increase[ i ],
														category 	= "equipment_upgrade",
														name_id		= "debug_upgrade_ammo_bag_ammo_increase"..i,
														title_id	= "debug_ammo_bag",
														subtitle_id	= "debug_upgrade_amount_increase"..i,
														icon		= "equipment_ammo_bag",
														image		= "upgrades_ammobag",
														image_slice	= "upgrades_ammobag_slice",
														description_text_id = "ammo_bag_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "ammo_bag",
																		upgrade 	= "ammo_increase",
																		value 		= i
																		}
														}
	end
	
	self.definitions[ "ammo_bag_quantity" ] = {			
														category 	= "equipment_upgrade",
														name_id		= "menu_ammo_bag_quantity",
														upgrade		= { category 	= "ammo_bag",
																		upgrade 	= "quantity",
																		value 		= 1
																		}
														}
end

-- DOCTOR BAG ------------------------------------------------------------
function UpgradesTweakData:_doctor_bag_definitions()
	self.definitions[ "doctor_bag" ]			= {
														tree			= 3,
														step			= 5,
														category 		= "equipment",
														equipment_id	= "doctor_bag",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_doctor_bag",
														title_id		= "debug_upgrade_new_equipment",
														subtitle_id		= "debug_doctor_bag",
														icon			= "equipment_doctor_bag",
														image			= "upgrades_doctorbag",
														image_slice		= "upgrades_doctorbag_slice",
														description_text_id = "doctor_bag",
														unlock_lvl  	= 2,
														prio			= "high",
														slot			= 1,
														}
	
	-- Quantity increase
	for i,_ in ipairs( self.values.doctor_bag.amount_increase ) do
		local depends_on = (i-1) > 0 and "doctor_bag_amount_increase"..(i-1) or "doctor_bag"
		local unlock_lvl = 3										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "doctor_bag_amount_increase"..i ] = {
														tree		= 3,
														step		= self.steps.doctor_bag.amount_increase[ i ],
														category 	= "equipment_upgrade",
														name_id		= "debug_upgrade_doctor_bag_amount_increase"..i,
														title_id	= "debug_doctor_bag",
														subtitle_id	= "debug_upgrade_amount_increase"..i,
														icon		= "equipment_doctor_bag",
														image		= "upgrades_doctorbag",
														image_slice	= "upgrades_doctorbag_slice",
														description_text_id = "doctor_bag_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "doctor_bag",
																		upgrade 	= "amount_increase",
																		value 		= i
																		}
														}
	end
	
	self.definitions[ "doctor_bag_quantity" ] = {			
														category 	= "equipment_upgrade",
														name_id		= "menu_doctor_bag_quantity",
														upgrade		= { category 	= "doctor_bag",
																		upgrade 	= "quantity",
																		value 		= 1
																		}
														}
	self.definitions[ "passive_doctor_bag_interaction_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_passive_doctor_bag_interaction_speed_multiplier",
														upgrade		= { category 	= "doctor_bag",
																		upgrade 	= "interaction_speed_multiplier",
																		value 		= 1
																		}
														}
end

-- CABLE TIE ------------------------------------------------------------
function UpgradesTweakData:_cable_tie_definitions()
	self.definitions[ "cable_tie" ]			= {
														category 		= "equipment",
														equipment_id	= "cable_tie",
														name_id			= "debug_equipment_cable_tie",
														title_id		= "debug_equipment_cable_tie",
														icon			= "equipment_cable_ties",
														image			= "upgrades_extracableties",
														image_slice		= "upgrades_extracableties_slice",
														unlock_lvl  	= 0,
														prio			= "high",
														}
														
	self.definitions[ "extra_cable_tie" ]			= {
														tree			= 1,
														step			= 4,
														category 		= "equipment",
														equipment_id	= "extra_cable_tie",
														-- name_id			= "debug_equipment_extra_cable_tie",
														name_id			= "debug_upgrade_extra_cable_tie_quantity1",
														title_id		= "debug_equipment_cable_tie",
														subtitle_id		= "debug_upgrade_amount_increase1",
														icon			= "equipment_extra_cable_ties",
														image			= "upgrades_extracableties",
														image_slice		= "upgrades_extracableties_slice",
														description_text_id = "extra_cable_tie",
														unlock_lvl  	= 3,
														prio			= "high",
														aquire			=  { upgrade = "extra_cable_tie_quantity1" },
														slot			= 2,
														}
														
	-- Quantity increase
	for i,_ in ipairs( self.values.extra_cable_tie.quantity ) do
		local depends_on = (i-1) > 0 and "extra_cable_tie_quantity"..(i-1) or "extra_cable_tie"
		local unlock_lvl = 4										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "extra_cable_tie_quantity"..i ] = {
														tree		= 1,
														step		= self.steps.extra_cable_tie.quantity[ i ],
														category 	= "equipment_upgrade",
														name_id		= "debug_upgrade_extra_cable_tie_quantity"..i,
														title_id	= "debug_equipment_cable_tie",
														subtitle_id	= "debug_upgrade_amount_increase"..i,
														icon		= "equipment_extra_cable_ties",
														image		= "upgrades_extracableties",
														image_slice	= "upgrades_extracableties_slice",
														description_text_id = "extra_cable_tie",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "extra_cable_tie",
																		upgrade 	= "quantity",
																		value 		= i
																		}
														}
	end
														
	self.definitions[ "cable_tie_quantity" ] = {			
														category 	= "equipment_upgrade",
														name_id		= "menu_cable_tie_quantity",
														upgrade		= { category 	= "cable_tie",
																		upgrade 	= "quantity",
																		value 		= 1
																		}
														}
	
	self.definitions[ "cable_tie_interact_speed_multiplier" ] = {			
														category 	= "equipment_upgrade",
														name_id		= "menu_cable_tie_interact_speed_multiplier",
														upgrade		= { category 	= "cable_tie",
																		upgrade 	= "interact_speed_multiplier",
																		value 		= 1
																		}
														}
	
	self.definitions[ "cable_tie_can_cable_tie_doors" ] = {			
														category 	= "equipment_upgrade",
														name_id		= "menu_cable_tie_can_cable_tie_doors",
														upgrade		= { category 	= "cable_tie",
																		upgrade 	= "can_cable_tie_doors",
																		value 		= 1
																		}
														}
														
	self.definitions[ "cable_tie_quantity_unlimited" ] = {			
														category 	= "equipment_upgrade",
														name_id		= "menu_cable_tie_quantity_unlimited",
														upgrade		= { category 	= "cable_tie",
																		upgrade 	= "quantity_unlimited",
																		value 		= 1
																		}
														}
end


-- SENTRY GUN ------------------------------------------------------------
function UpgradesTweakData:_sentry_gun_definitions()
	self.definitions[ "sentry_gun" ]			= {
														tree			= 4,
														step			= 5,
														category 		= "equipment",
														equipment_id	= "sentry_gun",
														-- unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id			= "debug_sentry_gun",
														title_id		= "debug_upgrade_new_equipment",
														subtitle_id		= "debug_sentry_gun",
														icon			= "equipment_sentry",
														image			= "upgrades_sentry",
														image_slice		= "upgrades_sentry_slice",
														description_text_id = "sentry_gun",
														unlock_lvl  	= 0,
														prio			= "high",
														slot			= 1,
														}
	
	self.definitions[ "sentry_gun_quantity_increase" ] = {
													category 	= "feature",
													name_id		= "menu_sentry_gun_quantity_increase",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "quantity",
																	value 		= 1
																	}
													}
	self.definitions[ "sentry_gun_damage_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_sentry_gun_damage_multiplier",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "damage_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "sentry_gun_extra_ammo_multiplier_1" ] = {
													category 	= "feature",
													incremental = true,
													name_id		= "menu_sentry_gun_extra_ammo_multiplier",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "extra_ammo_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "sentry_gun_extra_ammo_multiplier_2" ] = {
													category 	= "feature",
													incremental = true,
													name_id		= "menu_sentry_gun_extra_ammo_multiplier",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "extra_ammo_multiplier",
																	value 		= 2
																	}
													}
	self.definitions[ "sentry_gun_armor_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_sentry_gun_armor_multiplier",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "armor_multiplier",
																	value 		= 1
																	}
													}
	
	self.definitions[ "sentry_gun_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_sentry_gun_spread_multiplier",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "spread_multiplier",
																	value 		= 1
																	}
													}
	
	self.definitions[ "sentry_gun_rot_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_sentry_gun_rot_speed_multiplier",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "rot_speed_multiplier",
																	value 		= 1
																	}
													}
	
	self.definitions[ "sentry_gun_shield" ] = {
													category 	= "feature",
													name_id		= "menu_sentry_gun_shield",
													upgrade		= { category 	= "sentry_gun",
																	upgrade 	= "shield",
																	value 		= 1
																	}
													}
	
	--[[
	for i,_ in ipairs( self.values.sentry_gun.ammo_increase ) do
		local depends_on = (i-1) > 0 and "sentry_gun_ammo_increase"..(i-1) or "sentry_gun"
		local unlock_lvl = 11										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "sentry_gun_ammo_increase"..i ] = {	
														tree		= 4,
														step		= self.steps.sentry_gun.ammo_increase[ i ],
														category 	= "equipment_upgrade",
														name_id		= "debug_upgrade_sentry_gun_ammo_increase"..i,
														title_id	= "debug_sentry_gun",
														subtitle_id	= "debug_upgrade_ammo_increase"..i,
														icon		= "equipment_sentry",
														image		= "upgrades_sentry",
														image_slice	= "upgrades_sentry_slice",
														description_text_id = "sentry_gun_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "sentry_gun",
																		upgrade 	= "ammo_increase",
																		value 		= i
																		}
														}
	end


	for i,_ in ipairs( self.values.sentry_gun.armor_increase ) do
		local depends_on = (i-1) > 0 and "sentry_gun_armor_increase"..(i-1) or "sentry_gun"
		local unlock_lvl = 11										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "sentry_gun_armor_increase"..i ] = {	
														tree		= 4,
														step		= self.steps.sentry_gun.armor_increase[ i ],
														category 	= "equipment_upgrade",
														name_id		= "name"..i,
														title_id	= "debug_sentry_gun",
														subtitle_id	= "debug_upgrade_armor_increase"..i,
														icon		= "equipment_sentry",
														image		= "upgrades_sentry",
														image_slice	= "upgrades_sentry_slice",
														description_text_id = "sentry_gun_armor_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "sentry_gun",
																		upgrade 	= "armor_increase",
																		value 		= i
																		}
														}
	end

]]
end

function UpgradesTweakData:_rep_definitions()
	local rep_upgrades = self.values.rep_upgrades
	
	for index, rep_class in ipairs( rep_upgrades.classes ) do
		for i=1, 10 do
			self.definitions[ rep_class..i ]	= {	
													category 	= "rep_upgrade",
													value=rep_upgrades.values[index],
													}
		end
	end
	
	--[[
	for i = 1, 10 do
		self.definitions[ "rep_upgrade"..i ]	= {	
												category 	= "rep_upgrade",
												value=2,
												}
	end
	]]
end

-- C45 ------------------------------------------------------------
function UpgradesTweakData:_c45_definitions()
	self.definitions[ "c45" ]			= {
														tree		= 1,
														step		= 13,
														category 	= "weapon",
														unit_name 	= Idstring( "units/weapons/c45/c45" ),
														name_id		= "debug_c45",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_c45_short",
														icon		= "c45",
														image		= "upgrades_45",
														image_slice	= "upgrades_45_slice",
														unlock_lvl  = 30,
														prio		= "high",
														description_text_id = "des_c45",
														}
	
	-- Ammo clip increase
	for i,_ in ipairs( self.values.c45.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "c45_mag"..(i-1) or "c45"
		local unlock_lvl = 31										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "c45_mag"..i ] = {	
														tree		= 1,
														step		= self.steps.c45.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_c45_mag"..i,
														title_id	= "debug_c45_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "c45",
														image		= "upgrades_45",
														image_slice	= "upgrades_45_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "c45",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.c45.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "c45_recoil"..(i-1) or "c45"
		local unlock_lvl = 31										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "c45_recoil"..i ] = {	
														tree		= 1,
														step		= self.steps.c45.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_c45_recoil"..i,
														title_id	= "debug_c45_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "c45",
														image		= "upgrades_45",
														image_slice	= "upgrades_45_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "c45",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end			
	
	-- Damage multiplier
	for i,_ in ipairs( self.values.c45.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "c45_damage"..(i-1) or "c45"
		local unlock_lvl = 31										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "c45_damage"..i ] = {	
														tree		= 1,
														step		= self.steps.c45.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_c45_damage"..i,
														title_id	= "debug_c45_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "c45",
														image		= "upgrades_45",
														image_slice	= "upgrades_45_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "c45",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end
end

-- BERETTA ------------------------------------------------------------
function UpgradesTweakData:_beretta92_definitions()
	self.definitions[ "beretta92" ]			= {
														category 	= "weapon",
														weapon_id	= "beretta92",		-- Referce to the tweakdata weapon id for this gun
														unit_name 	= Idstring( "units/weapons/beretta92/beretta92" ),
														name_id		= "debug_beretta92",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_beretta92_short",
														icon		= "beretta92",
														image		= "upgrades_m9sd",
														image_slice	= "upgrades_m9sd_slice",
														unlock_lvl  = 0,
														prio		= "high",
														description_text_id = "des_beretta92",
														}
	
	-- Ammo clip increase
	for i,_ in ipairs( self.values.beretta92.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "beretta_mag"..(i-1) or "beretta92"
		local unlock_lvl = 2										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "beretta_mag"..i ] = {	
														tree		= 1,
														step 		= self.steps.beretta92.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_beretta_mag"..i,
														title_id	= "debug_beretta92_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "beretta92",
														image		= "upgrades_m9sd",
														image_slice	= "upgrades_m9sd_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "beretta92",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.beretta92.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "beretta_recoil"..(i-1) or "beretta92"
		local unlock_lvl = 2										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "beretta_recoil"..i ] = {	
														tree		= 2,
														step		= self.steps.beretta92.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_beretta_recoil"..i,
														title_id	= "debug_beretta92_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "beretta92",
														image		= "upgrades_m9sd",
														image_slice	= "upgrades_m9sd_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "beretta92",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Spread multiplier
	for i,_ in ipairs( self.values.beretta92.spread_multiplier ) do
		local depends_on = (i-1) > 0 and "beretta_spread"..(i-1) or "beretta92"
		local unlock_lvl = 2										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "beretta_spread"..i ] = {	
														tree		= 3,
														step		= self.steps.beretta92.spread_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_beretta_spread"..i,
														title_id	= "debug_beretta92_short",
														subtitle_id	= "debug_upgrade_spread"..i,
														icon		= "beretta92",
														image		= "upgrades_m9sd",
														image_slice	= "upgrades_m9sd_slice",
														description_text_id = "spread_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "beretta92",
																		upgrade 	= "spread_multiplier",
																		value 		= i
																		}
														}
	end
end

-- RAGING BULL ------------------------------------------------------------
function UpgradesTweakData:_raging_bull_definitions()
	self.definitions[ "raging_bull" ]			= {
														tree		= 3,
														step		= 6,
														category 	= "weapon",
														weapon_id	= "raging_bull",
														unit_name 	= Idstring( "units/weapons/raging_bull/raging_bull" ),
														name_id		= "debug_raging_bull",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_raging_bull_short",
														icon		= "raging_bull",
														image		= "upgrades_ragingbull",
														image_slice = "upgrades_ragingbull_slice",
														unlock_lvl  = 60,
														prio		= "high",
														description_text_id = "des_raging_bull",
														}
														
	-- Spread multiplier
	for i,_ in ipairs( self.values.raging_bull.spread_multiplier ) do
		local depends_on = (i-1) > 0 and "raging_bull_spread"..(i-1)	-- First one should probably depend on beretta92 weapon when it is defined as upgrade
		local unlock_lvl = 61										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "raging_bull_spread"..i ] = {	
														tree		= 3,
														step		= self.steps.raging_bull.spread_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_raging_bull_spread"..i,
														title_id	= "debug_raging_bull_short",
														subtitle_id	= "debug_upgrade_spread"..i,
														icon		= "raging_bull",
														image		= "upgrades_ragingbull",
														image_slice = "upgrades_ragingbull_slice",
														description_text_id = "spread_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "raging_bull",
																		upgrade 	= "spread_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Reload speed multiplier
	for i,_ in ipairs( self.values.raging_bull.reload_speed_multiplier ) do
		local depends_on = (i-1) > 0 and "raging_bull_reload_speed"..(i-1) or "raging_bull"		
		local unlock_lvl = 61										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "raging_bull_reload_speed"..i ] = {
														tree		= 3,
														step		= self.steps.raging_bull.reload_speed_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_raging_bull_reload_speed"..i,
														title_id	= "debug_raging_bull_short",
														subtitle_id	= "debug_upgrade_reload_speed"..i,
														icon		= "raging_bull",
														image		= "upgrades_ragingbull",
														image_slice = "upgrades_ragingbull_slice",
														description_text_id = "reload_speed_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "raging_bull",
																		upgrade 	= "reload_speed_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Damage multiplier
	for i,_ in ipairs( self.values.raging_bull.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "raging_bull_damage"..(i-1) or "raging_bull"
		local unlock_lvl = 61										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "raging_bull_damage"..i ] = {	
														tree		= 3,
														step		= self.steps.raging_bull.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_raging_bull_damage"..i,
														title_id	= "debug_raging_bull_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "raging_bull",
														image		= "upgrades_ragingbull",
														image_slice = "upgrades_ragingbull_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "raging_bull",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end
end

function UpgradesTweakData:_olympic_definitions()
	self.definitions[ "olympic" ]			= {
														category 	= "weapon",
														weapon_id	= "olympic",
														factory_id  = "wpn_fps_smg_olympic",
														}
	--[[self.definitions[ "crafting_olympic" ]	= {
														category 	= "crafting",
														weapon_id	= "olympic",	
														}]]
end

function UpgradesTweakData:_amcar_definitions()
	self.definitions[ "amcar" ]			= {
														category 	= "weapon",
														weapon_id	= "amcar",
														factory_id  = "wpn_fps_ass_amcar",
														free		= true,
														}
end

function UpgradesTweakData:_m16_definitions()
	self.definitions[ "m16" ]			= {
														category 	= "weapon",
														weapon_id	= "m16",
														factory_id  = "wpn_fps_ass_m16",
														}
	--[[self.definitions[ "crafting_m16" ]	= {
														category 	= "crafting",
														weapon_id	= "m16",	
														}]]
end

function UpgradesTweakData:_new_m4_definitions()
	self.definitions[ "new_m4" ]			= {
														category 	= "weapon",
														weapon_id	= "new_m4",
														factory_id  = "wpn_fps_ass_m4",
														}
	--[[self.definitions[ "crafting_new_m4" ]	= {
														category 	= "crafting",
														weapon_id	= "new_m4",	
														}]]
end

function UpgradesTweakData:_glock_18c_definitions()
	self.definitions[ "glock_18c" ]			= {
														category 	= "weapon",
														weapon_id	= "glock_18c",
														factory_id  = "wpn_fps_pis_g18c",
														}
	--[[self.definitions[ "crafting_glock_18c" ]	= {
														category 	= "crafting",
														weapon_id	= "glock_18c",	
														}]]
end

function UpgradesTweakData:_saiga_definitions()
	self.definitions[ "saiga" ]			= {
														category 	= "weapon",
														weapon_id	= "saiga",
														factory_id  = "wpn_fps_shot_saiga",
														}
	--[[self.definitions[ "crafting_saiga" ]	= {
														category 	= "crafting",
														weapon_id	= "saiga",	
														}]]
end

function UpgradesTweakData:_akmsu_definitions()
	self.definitions[ "akmsu" ]			= {
														category 	= "weapon",
														weapon_id	= "akmsu",
														factory_id  = "wpn_fps_smg_akmsu",
														}
	--[[self.definitions[ "crafting_akmsu" ]	= {
														category 	= "crafting",
														weapon_id	= "akmsu",	
														}]]
end

function UpgradesTweakData:_ak74_definitions()
	self.definitions[ "ak74" ]			= {
														category 	= "weapon",
														weapon_id	= "ak74",
														factory_id  = "wpn_fps_ass_74",
														}
	--[[self.definitions[ "crafting_ak74" ]	= {
														category 	= "crafting",
														weapon_id	= "ak74",	
														}]]
end

function UpgradesTweakData:_akm_definitions()
	self.definitions[ "akm" ]			= {
														category 	= "weapon",
														weapon_id	= "akm",
														factory_id  = "wpn_fps_ass_akm",
														}
	--[[self.definitions[ "crafting_akm" ]	= {
														category 	= "crafting",
														weapon_id	= "akm",	
														}]]
end

function UpgradesTweakData:_ak5_definitions()
	self.definitions[ "ak5" ]			= {
														category 	= "weapon",
														weapon_id	= "ak5",
														factory_id  = "wpn_fps_ass_ak5",
														}
	--[[self.definitions[ "crafting_ak5" ]	= {
														category 	= "crafting",
														weapon_id	= "ak5",	
														}]]
end

function UpgradesTweakData:_aug_definitions()
	self.definitions[ "aug" ]			= {
														category 	= "weapon",
														weapon_id	= "aug",
														factory_id  = "wpn_fps_ass_aug",
														}
	--[[self.definitions[ "crafting_aug" ]	= {
														category 	= "crafting",
														weapon_id	= "aug",	
														}]]
end

function UpgradesTweakData:_g36_definitions()
	self.definitions[ "g36" ]			= {
														category 	= "weapon",
														weapon_id	= "g36",
														factory_id  = "wpn_fps_ass_g36",
														}
	--[[self.definitions[ "crafting_g36" ]	= {
														category 	= "crafting",
														weapon_id	= "g36",	
														}]]
end

function UpgradesTweakData:_p90_definitions()
	self.definitions[ "p90" ]			= {
														category 	= "weapon",
														weapon_id	= "p90",
														factory_id  = "wpn_fps_smg_p90",
														}
	--[[self.definitions[ "crafting_p90" ]	= {
														category 	= "crafting",
														weapon_id	= "p90",	
														}]]
end

function UpgradesTweakData:_new_m14_definitions()
	self.definitions[ "new_m14" ]			= {
														category 	= "weapon",
														weapon_id	= "new_m14",
														factory_id  = "wpn_fps_ass_m14",
														}
	--[[self.definitions[ "crafting_new_m14" ]	= {
														category 	= "crafting",
														weapon_id	= "new_m14",	
														}]]
end

function UpgradesTweakData:_mp9_definitions()
	self.definitions[ "mp9" ]			= {
														category 	= "weapon",
														weapon_id	= "mp9",
														factory_id  = "wpn_fps_smg_mp9",
														}
	--[[self.definitions[ "crafting_mp9" ]	= {
														category 	= "crafting",
														weapon_id	= "mp9",	
														}]]
end

function UpgradesTweakData:_deagle_definitions()
	self.definitions[ "deagle" ]			= {
														category 	= "weapon",
														weapon_id	= "deagle",
														factory_id  = "wpn_fps_pis_deagle",
														}
	--[[self.definitions[ "crafting_deagle" ]	= {
														category 	= "crafting",
														weapon_id	= "deagle",	
														}]]
end

function UpgradesTweakData:_new_mp5_definitions()
	self.definitions[ "new_mp5" ]			= {
														category 	= "weapon",
														weapon_id	= "new_mp5",
														factory_id  = "wpn_fps_smg_mp5",
														}
	--[[self.definitions[ "crafting_new_mp5" ]	= {
														category 	= "crafting",
														weapon_id	= "new_mp5",	
														}]]
end

function UpgradesTweakData:_colt_1911_definitions()
	self.definitions[ "colt_1911" ]			= {
														category 	= "weapon",
														weapon_id	= "colt_1911",
														factory_id  = "wpn_fps_pis_1911",
														}
	--[[self.definitions[ "crafting_colt_1911" ]	= {
														category 	= "crafting",
														weapon_id	= "colt_1911",	
														}]]
end

function UpgradesTweakData:_mac10_definitions()
	self.definitions[ "mac10" ]			= {
														category 	= "weapon",
														weapon_id	= "mac10",
														factory_id  = "wpn_fps_smg_mac10",
														}
	--[[self.definitions[ "crafting_mac10" ]	= {
														category 	= "crafting",
														weapon_id	= "mac10",	
														}]]
end

function UpgradesTweakData:_glock_17_definitions()
	self.definitions[ "glock_17" ]			= {
														category 	= "weapon",
														weapon_id	= "glock_17",
														factory_id  = "wpn_fps_pis_g17",
														free		= true,
														}
end

function UpgradesTweakData:_b92fs_definitions()
	self.definitions[ "b92fs" ]			= {
														category 	= "weapon",
														weapon_id	= "b92fs",
														factory_id  = "wpn_fps_pis_beretta",
														}
	--[[self.definitions[ "crafting_b92fs" ]	= {
														category 	= "crafting",
														weapon_id	= "b92fs",	
														}]]
end

function UpgradesTweakData:_huntsman_definitions()
	self.definitions[ "huntsman" ]			= {
														category 	= "weapon",
														weapon_id	= "huntsman",
														factory_id  = "wpn_fps_shot_huntsman",
														}
	--[[self.definitions[ "crafting_huntsman" ]	= {
														category 	= "crafting",
														weapon_id	= "huntsman",	
														}]]
end

function UpgradesTweakData:_r870_definitions()
	self.definitions[ "r870" ]			= {
														category 	= "weapon",
														weapon_id	= "r870",
														factory_id  = "wpn_fps_shot_r870",
														}
	--[[self.definitions[ "crafting_r870" ]	= {
														category 	= "crafting",
														weapon_id	= "r870",	
														}]]
end

function UpgradesTweakData:_serbu_definitions()
	self.definitions[ "serbu" ]			= {
														category 	= "weapon",
														weapon_id	= "serbu",
														factory_id  = "wpn_fps_shot_serbu",
														}
end

function UpgradesTweakData:_new_raging_bull_definitions()
	self.definitions[ "new_raging_bull" ]			= {
														category 	= "weapon",
														weapon_id	= "new_raging_bull",
														factory_id  = "wpn_fps_pis_rage",
														}
	--[[self.definitions[ "crafting_new_raging_bull" ]	= {
														category 	= "crafting",
														weapon_id	= "new_raging_bull",	
														}]]	
end

function UpgradesTweakData:_saw_definitions()
	self.definitions[ "saw" ]			= {
														category 	= "weapon",
														weapon_id	= "saw",
														factory_id  = "wpn_fps_saw",
														}														
	self.definitions[ "saw_extra_ammo_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_saw_extra_ammo_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "extra_ammo_multiplier",
																	value 		= 1
																	}
													}
	
	self.definitions[ "saw_enemy_slicer" ] = {
													category 	= "feature",
													name_id		= "menu_saw_enemy_slicer",
													upgrade		= { category 	= "saw",
																	upgrade 	= "enemy_slicer",
																	value 		= 1
																	}
													}
													
	self.definitions[ "saw_recoil_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_saw_recoil_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "recoil_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "saw_fire_rate_multiplier_1" ] = {
													category 	= "feature",
													name_id		= "menu_saw_fire_rate_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "fire_rate_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "saw_fire_rate_multiplier_2" ] = {
													category 	= "feature",
													name_id		= "menu_saw_fire_rate_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "fire_rate_multiplier",
																	value 		= 2
																	}
													}
	self.definitions[ "saw_lock_damage_multiplier_1" ] = {
													category 	= "feature",
													name_id		= "menu_lock_damage_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "lock_damage_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "saw_lock_damage_multiplier_2" ] = {
													category 	= "feature",
													name_id		= "menu_lock_damage_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "lock_damage_multiplier",
																	value 		= 2
																	}
													}
	self.definitions[ "saw_hip_fire_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_saw_hip_fire_spread_multiplier",
													upgrade		= { category 	= "saw",
																	upgrade 	= "hip_fire_spread_multiplier",
																	value 		= 1
																	}
													}
													
end

function UpgradesTweakData:_weapon_definitions()
	self.definitions[ "weapon_clip_ammo_increase_1" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_clip_ammo_increase_1",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "clip_ammo_increase",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_clip_ammo_increase_2" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_clip_ammo_increase_2",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "clip_ammo_increase",
																	value 		= 2
																	}
													}
	self.definitions[ "weapon_passive_swap_speed_multiplier_1" ] = {	-- tier bonus
													category 	= "feature",
													name_id		= "menu_weapon_swap_speed_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "passive_swap_speed_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_passive_swap_speed_multiplier_2" ] = {	-- tier bonus
													category 	= "feature",
													name_id		= "menu_weapon_swap_speed_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "passive_swap_speed_multiplier",
																	value 		= 2
																	}
													}
	self.definitions[ "weapon_swap_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_swap_speed_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "swap_speed_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_single_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_single_spread_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "single_spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_silencer_enter_steelsight_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_silencer_enter_steelsight_speed_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "silencer_enter_steelsight_speed_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_silencer_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_silencer_spread_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "silencer_spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_silencer_recoil_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_silencer_recoil_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "silencer_recoil_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_silencer_damage_multiplier_1" ] = {
													category 	= "feature",
													name_id		= "silencer_damage_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "silencer_damage_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_silencer_damage_multiplier_2" ] = {
													category 	= "feature",
													name_id		= "silencer_damage_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "silencer_damage_multiplier",
																	value 		= 2
																	}
													}
	self.definitions[ "weapon_passive_reload_speed_multiplier" ] = {	-- tier bonus
													category 	= "feature",
													name_id		= "menu_weapon_reload_speed",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "passive_reload_speed_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_passive_recoil_multiplier_1" ] = {	-- tier bonus
														category 	= "feature",
														name_id		= "menu_weapon_recoil_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "passive_recoil_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "weapon_passive_recoil_multiplier_2" ] = {	-- tier bonus
														category 	= "feature",
														name_id		= "menu_weapon_recoil_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "passive_recoil_multiplier",
																		value 		= 2
																		}
														}
	self.definitions[ "weapon_passive_headshot_damage_multiplier" ] = {	-- tier bonus
														category 	= "feature",
														name_id		= "menu_weapon_headshot_damage_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "passive_headshot_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "weapon_passive_damage_multiplier" ] = {	-- tier bonus
														category 	= "feature",
														name_id		= "menu_weapon_passive_damage_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "passive_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "weapon_special_damage_taken_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_weapon_special_damage_taken_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "special_damage_taken_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "weapon_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_spread_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "weapon_fire_rate_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_weapon_fire_rate_multiplier",
													upgrade		= { category 	= "weapon",
																	upgrade 	= "fire_rate_multiplier",
																	value 		= 1
																	}
													}
														
end

function UpgradesTweakData:_pistol_definitions()
	self.definitions[ "pistol_reload_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_pistol_reload_speed",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "reload_speed_multiplier",
																	value 		= 1
																	}
													}
													
	self.definitions[ "pistol_damage_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_pistol_damage_multiplier",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "damage_multiplier",
																	value 		= 1
																	}
													}
													
	self.definitions[ "pistol_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_pistol_spread_multiplier",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "pistol_fire_rate_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_pistol_fire_rate_multiplier",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "fire_rate_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "pistol_exit_run_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_exit_run_speed_multiplier",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "exit_run_speed_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "pistol_hip_fire_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_pistol_hip_fire_spread_multiplier",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "hip_fire_spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "pistol_swap_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_pistol_swap_speed_multiplier",
													upgrade		= { category 	= "pistol",
																	upgrade 	= "swap_speed_multiplier",
																	value 		= 1
																	}
													}
end

function UpgradesTweakData:_assault_rifle_definitions()
	self.definitions[ "assault_rifle_recoil_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_assault_rifle_recoil_multiplier",
														upgrade		= { category 	= "assault_rifle",
																		upgrade 	= "recoil_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "assault_rifle_enter_steelsight_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_assault_rifle_enter_steelsight_speed_multiplier",
														upgrade		= { category 	= "assault_rifle",
																		upgrade 	= "enter_steelsight_speed_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "assault_rifle_reload_speed_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_assault_rifle_reload_speed_multiplier",
													upgrade		= { category 	= "assault_rifle",
																	upgrade 	= "reload_speed_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "assault_rifle_move_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_assault_rifle_move_spread_multiplier",
													upgrade		= { category 	= "assault_rifle",
																	upgrade 	= "move_spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "assault_rifle_hip_fire_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_assault_rifle_hip_fire_spread_multiplier",
													upgrade		= { category 	= "assault_rifle",
																	upgrade 	= "hip_fire_spread_multiplier",
																	value 		= 1
																	}
													}
	self.definitions[ "assault_rifle_zoom_increase" ] = {
													category 	= "feature",
													name_id		= "menu_assault_rifle_zoom_increase",
													upgrade		= { category 	= "assault_rifle",
																	upgrade 	= "zoom_increase",
																	value 		= 1
																	}
													}
														
end

function UpgradesTweakData:_smg_definitions()
	self.definitions[ "smg_reload_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_reload_speed_multiplier",
														upgrade		= { category 	= "smg",
																		upgrade 	= "reload_speed_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "smg_recoil_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_smg_recoil_multiplier",
														upgrade		= { category 	= "smg",
																		upgrade 	= "recoil_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "smg_hip_fire_spread_multiplier" ] = {
													category 	= "feature",
													name_id		= "menu_smg_hip_fire_spread_multiplier",
													upgrade		= { category 	= "smg",
																	upgrade 	= "hip_fire_spread_multiplier",
																	value 		= 1
																	}
													}
end

function UpgradesTweakData:_shotgun_definitions()
	self.definitions[ "shotgun_recoil_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_shotgun_recoil_multiplier",
														upgrade		= { category 	= "shotgun",
																		upgrade 	= "recoil_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "shotgun_damage_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_shotgun_damage_multiplier",
														upgrade		= { category 	= "shotgun",
																		upgrade 	= "damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "shotgun_reload_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_shotgun_reload_speed_multiplier",
														upgrade		= { category 	= "shotgun",
																		upgrade 	= "reload_speed_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "shotgun_enter_steelsight_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_shotgun_enter_steelsight_speed_multiplier",
														upgrade		= { category 	= "shotgun",
																		upgrade 	= "enter_steelsight_speed_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "shotgun_hip_fire_spread_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_shotgun_hip_fire_spread_multiplier",
														upgrade		= { category 	= "shotgun",
																		upgrade 	= "hip_fire_spread_multiplier",
																		value 		= 1
																		}
														}
														
														
end

function UpgradesTweakData:_carry_definitions()
	self.definitions[ "carry_movement_speed_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_carry_movement_speed_multiplier",
														upgrade		= { category 	= "carry",
																		upgrade 	= "movement_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "carry_throw_distance_multiplier" ] = {
														category 	= "feature",
														name_id		= "menu_carry_throw_distance_multiplier",
														upgrade		= { category 	= "carry",
																		upgrade 	= "throw_distance_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "carry_interact_speed_multiplier_1" ] = {
														category 	= "feature",
														name_id		= "menu_carry_interact_speed_multiplierr",
														upgrade		= { category 	= "carry",
																		upgrade 	= "interact_speed_multiplier",
																		value 		= 1
																		}
														}								
	self.definitions[ "carry_catch_interaction_speed_1" ] = {
														category 	= "feature",
														name_id		= "menu_carry_catch_interaction_speed",
														upgrade		= { category 	= "carry",
																		upgrade 	= "catch_interaction_speed",
																		value 		= 1
																		}
														}
	self.definitions[ "carry_interact_speed_multiplier_2" ] = {
														category 	= "feature",
														name_id		= "menu_carry_interact_speed_multiplierr",
														upgrade		= { category 	= "carry",
																		upgrade 	= "interact_speed_multiplier",
																		value 		= 2
																		}
														}								
	self.definitions[ "carry_catch_interaction_speed_2" ] = {
														category 	= "feature",
														name_id		= "menu_carry_catch_interaction_speed",
														upgrade		= { category 	= "carry",
																		upgrade 	= "catch_interaction_speed",
																		value 		= 2
																		}
														}
																																					
end

function UpgradesTweakData:_team_definitions()
	self.definitions[ "team_pistol_suppression_recoil_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_pistol_suppression_recoil_multiplier",
														upgrade		= { category 	= "pistol",
																		upgrade 	= "suppression_recoil_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "team_pistol_recoil_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_pistol_recoil_multiplier",
														upgrade		= { category 	= "pistol",
																		upgrade 	= "recoil_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "team_weapon_suppression_recoil_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_weapon_suppression_recoil_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "suppression_recoil_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "team_weapon_recoil_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_weapon_recoil_multiplier",
														upgrade		= { category 	= "weapon",
																		upgrade 	= "recoil_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "team_xp_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_xp_multiplier",
														upgrade		= { category 	= "xp",
																		upgrade 	= "multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "team_armor_regen_time_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_armor_regen_time_multiplier",
														upgrade		= { category 	= "armor",
																		upgrade 	= "regen_time_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "team_passive_armor_regen_time_multiplier" ] = {	-- tier bonus
														category 	= "team",
														name_id		= "menu_team_armor_regen_time_multiplier",
														upgrade		= { category 	= "armor",
																		upgrade 	= "passive_regen_time_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "team_stamina_multiplier" ] = {
														category 	= "team",
														name_id		= "menu_team_stamina_multiplier",
														upgrade		= { category 	= "stamina",
																		upgrade 	= "multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "team_passive_stamina_multiplier_1" ] = {	-- tier bonus
														category 	= "team",
														name_id		= "menu_team_stamina_multiplier",
														upgrade		= { category 	= "stamina",
																		upgrade 	= "passive_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "team_passive_stamina_multiplier_2" ] = {	-- tier bonus
														category 	= "team",
														name_id		= "menu_team_stamina_multiplier",
														upgrade		= { category 	= "stamina",
																		upgrade 	= "passive_multiplier",
																		value 		= 2
																		}
														}
end

function UpgradesTweakData:_temporary_definitions()
	self.definitions[ "temporary_combat_medic_damage_multiplier1" ] = {
														incremental = true,
														category 	= "temporary",
														name_id		= "menu_temporary_combat_medic_damage_multiplier",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "combat_medic_damage_multiplier",
																		value 		= 1
																		}
														}
	self.definitions[ "temporary_combat_medic_damage_multiplier2" ] = {
														incremental = true,
														category 	= "temporary",
														name_id		= "menu_temporary_combat_medic_damage_multiplier",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "combat_medic_damage_multiplier",
																		value 		= 2
																		}
														}
	
	self.definitions[ "temporary_combat_medic_enter_steelsight_speed_multiplier" ] = {
														category 	= "temporary",
														name_id		= "menu_temporary_combat_medic_enter_steelsight_speed_multiplier",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "combat_medic_enter_steelsight_speed_multiplier",
																		value 		= 1
																		}
														}
														
	self.definitions[ "temporary_revive_health_boost" ] = {
														category 	= "temporary",
														name_id		= "menu_temporary_revive_health_boost",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "revive_health_boost",
																		value 		= 1
																		}
														}
														
	self.definitions[ "temporary_no_ammo_cost_1" ] = {
														category 	= "temporary",
														name_id		= "menu_temporary_no_ammo_cost_1",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "no_ammo_cost",
																		value 		= 1
																		}
														}
	self.definitions[ "temporary_no_ammo_cost_2" ] = {
														category 	= "temporary",
														name_id		= "menu_temporary_no_ammo_cost_2",
														upgrade		= { category 	= "temporary",
																		upgrade 	= "no_ammo_cost",
																		value 		= 2
																		}
														}
end

--[[function UpgradesTweakData:_ammo_definitions()
	self.definitions[ "armor_piercing_ammo" ] = {
														category 	= "ammo",
														name_id		= "menu_armor_piercing_ammo",
														}
														
	self.definitions[ "hollow_tip_ammo" ] = {
														category 	= "ammo",
														name_id		= "menu_hollow_tip_ammo",
														}
														
	self.definitions[ "incendiary_ammo" ] = {
														category 	= "ammo",
														name_id		= "menu_incendiary_ammo",
														}
end]]

function UpgradesTweakData:_shape_charge_definitions()
	self.definitions[ "shape_charge" ]			= {
														category 		= "equipment",
														equipment_id	= "shape_charge",
														name_id			= "menu_shape_charge",
														}
end

-- M4 ------------------------------------------------------------
function UpgradesTweakData:_m4_definitions()
																												
									------------------------------------------------------------													

	self.definitions[ "m4" ]			= {
														category 	= "weapon",
														weapon_id	= "m4",
														unit_name 	= Idstring( "units/weapons/m4_rifle/m4_rifle" ),
														name_id		= "debug_m4_rifle",
														title_id	= "debug_m4_rifle_short",
														icon		= "m4",
														image		= "upgrades_m4",
														image_slice	= "upgrades_m4_slice",
														unlock_lvl  = 0,
														prio		= "high",
														description_text_id = "des_m4",
														}
	
	-- Ammo clip increase
	for i,_ in ipairs( self.values.m4.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "m4_mag"..(i-1) or "m4"
		local unlock_lvl = 3										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m4_mag"..i ] = {	
														tree		= 3,
														step		= self.steps.m4.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m4_mag"..i,
														title_id	= "debug_m4_rifle_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "m4",
														image		= "upgrades_m4",
														image_slice	= "upgrades_m4_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m4",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	-- Spread multiplier
	for i,_ in ipairs( self.values.m4.spread_multiplier ) do
		local depends_on = (i-1) > 0 and "m4_spread"..(i-1) or "m4"
		local unlock_lvl = 4										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m4_spread"..i ] = {	
														tree		= 2,
														step		= self.steps.m4.spread_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m4_spread"..i,
														title_id	= "debug_m4_rifle_short",
														subtitle_id	= "debug_upgrade_spread"..i,
														icon		= "m4",
														image		= "upgrades_m4",
														image_slice	= "upgrades_m4_slice",
														description_text_id = "spread_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m4",
																		upgrade 	= "spread_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Damage multiplier
	for i,_ in ipairs( self.values.m4.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "m4_damage"..(i-1) or "m4"
		local unlock_lvl = 5										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m4_damage"..i ] = {	
														tree		= 1,
														step 		= self.steps.m4.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m4_damage"..i,
														title_id	= "debug_m4_rifle_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "m4",
														image		= "upgrades_m4",
														image_slice	= "upgrades_m4_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m4",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end
		
	-- self.definitions[ "m4_damage1" ].step = 1
	-- self.definitions[ "m4_damage2" ].step = 15
end

-- M14 ------------------------------------------------------------
function UpgradesTweakData:_m14_definitions()
	self.definitions[ "m14" ]			= {
														tree		= 2,
														step		= 17,
														category 	= "weapon",
														weapon_id	= "m14",
														unit_name 	= Idstring( "units/weapons/m14/m14" ),
														name_id		= "debug_m14",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_m14_short",
														icon		= "m14",
														image		= "upgrades_m14",
														image_slice	= "upgrades_m14_slice",
														unlock_lvl  = 101,
														prio		= "high",
														description_text_id = "des_m14",
														}
	
	-- Ammo clip increase
	for i,_ in ipairs( self.values.m14.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "m14_mag"..(i-1) or "m14"
		local unlock_lvl = 102										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m14_mag"..i ] = {	
														tree		= 2,
														step		= self.steps.m14.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m14_mag"..i,
														title_id	= "debug_m14_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "m14",
														image		= "upgrades_m14",
														image_slice	= "upgrades_m14_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m14",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	-- Spread multiplier
	for i,_ in ipairs( self.values.m14.spread_multiplier ) do
		local depends_on = (i-1) > 0 and "m14_spread"..(i-1) or "m14"
		local unlock_lvl = 102										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m14_spread"..i ] = {	
														tree		= 2,
														step		= self.steps.m14.spread_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m14_spread"..i,
														title_id	= "debug_m14_short",
														subtitle_id	= "debug_upgrade_spread"..i,
														icon		= "m14",
														image		= "upgrades_m14",
														image_slice	= "upgrades_m14_slice",
														description_text_id = "spread_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m14",
																		upgrade 	= "spread_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Damage multiplier
	for i,_ in ipairs( self.values.m14.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "m14_damage"..(i-1) or "m14"
		local unlock_lvl = 102										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m14_damage"..i ] = {	
														tree		= 2,
														step		= self.steps.m14.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m14_damage"..i,
														title_id	= "debug_m14_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "m14",
														image		= "upgrades_m14",
														image_slice	= "upgrades_m14_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m14",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.m14.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "m14_recoil"..(i-1) or "m14"		
		local unlock_lvl = 102										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "m14_recoil"..i ] = {	
														tree		= 2,
														step		= self.steps.m14.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m14_recoil"..i,
														title_id	= "debug_m14_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "m14",
														image		= "upgrades_m14",
														image_slice	= "upgrades_m14_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m14",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end		
end

-- MP5 ------------------------------------------------------------
function UpgradesTweakData:_mp5_definitions()
	self.definitions[ "mp5" ]			= {
														tree		= 3,
														step		= 21,
														category 	= "weapon",
														weapon_id	= "mp5",
														unit_name 	= Idstring( "units/weapons/mp5/mp5" ),
														name_id		= "debug_mp5",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_mp5_short",
														icon		= "mp5",
														image		= "upgrades_mp5",
														image_slice	= "upgrades_mp5_slice",
														unlock_lvl  = 6,
														prio		= "high",
														description_text_id = "des_mp5",
														}
	
	-- Spread multiplier
	for i,_ in ipairs( self.values.mp5.spread_multiplier ) do
		local depends_on = (i-1) > 0 and "mp5_spread"..(i-1) or "mp5"		
		local unlock_lvl = 7										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mp5_spread"..i ] = {	
														tree		= 3,
														step		= self.steps.mp5.spread_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mp5_spread"..i,
														title_id	= "debug_mp5_short",
														subtitle_id	= "debug_upgrade_spread"..i,
														icon		= "mp5",
														image		= "upgrades_mp5",
														image_slice	= "upgrades_mp5_slice",
														description_text_id = "spread_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mp5",
																		upgrade 	= "spread_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.mp5.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "mp5_recoil"..(i-1) or "mp5"		
		local unlock_lvl = 8										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mp5_recoil"..i ] = {	
														tree		= 3,
														step		= self.steps.mp5.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mp5_recoil"..i,
														title_id	= "debug_mp5_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "mp5",
														image		= "upgrades_mp5",
														image_slice	= "upgrades_mp5_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mp5",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end									
	
	-- Reload speed multiplier
	for i,_ in ipairs( self.values.mp5.reload_speed_multiplier ) do
		local depends_on = (i-1) > 0 and "mp5_reload_speed"..(i-1) or "mp5"		
		local unlock_lvl = 9										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mp5_reload_speed"..i ] = {	
														tree		= 3,
														step		= self.steps.mp5.reload_speed_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mp5_reload_speed"..i,
														title_id	= "debug_mp5_short",
														subtitle_id	= "debug_upgrade_reload_speed"..i,
														icon		= "mp5",
														image		= "upgrades_mp5",
														image_slice	= "upgrades_mp5_slice",
														description_text_id = "reload_speed_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mp5",
																		upgrade 	= "reload_speed_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Enter steelsight speed multiplier
	for i,_ in ipairs( self.values.mp5.enter_steelsight_speed_multiplier ) do
		local depends_on = (i-1) > 0 and "mp5_enter_steelsight_speed"..(i-1) or "mp5"		
		local unlock_lvl = 10										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mp5_enter_steelsight_speed"..i ] = {	
														tree		= 3,
														step		= self.steps.mp5.enter_steelsight_speed_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mp5_enter_steelsight_speed"..i,
														title_id	= "debug_mp5_short",
														subtitle_id	= "debug_upgrade_enter_steelsight_speed"..i,
														icon		= "mp5",
														image		= "upgrades_mp5",
														image_slice	= "upgrades_mp5_slice",
														description_text_id = "enter_steelsight_speed_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mp5",
																		upgrade 	= "enter_steelsight_speed_multiplier",
																		value 		= i
																		}
														}
	end
end

-- MAC 11 ------------------------------------------------------------
function UpgradesTweakData:_mac11_definitions()
	self.definitions[ "mac11" ]			= {
														tree		= 1,
														step		= 5,
														category 	= "weapon",
														weapon_id	= "mac11",
														unit_name 	= Idstring( "units/weapons/mac11/mac11" ),
														name_id		= "debug_mac11",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_mac11_short",
														icon		= "mac11",
														image		= "upgrades_mac10",
														image_slice	= "upgrades_mac10_slice",
														unlock_lvl  = 81,
														prio		= "high",
														description_text_id = "des_mac11",
														}
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.mac11.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "mac11_recoil"..(i-1) or "mac11"		
		local unlock_lvl = 82										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mac11_recoil"..i ] = {	
														tree		= 1,
														step		= self.steps.mac11.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mac11_recoil"..i,
														title_id	= "debug_mac11_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "mac11",
														image		= "upgrades_mac10",
														image_slice	= "upgrades_mac10_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mac11",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end									
	
	-- Enter steelsight speed multiplier
	for i,_ in ipairs( self.values.mac11.enter_steelsight_speed_multiplier ) do
		local depends_on = (i-1) > 0 and "mac11_enter_steelsight_speed"..(i-1) or "mac11"		
		local unlock_lvl = 82										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mac11_enter_steelsight_speed"..i ] = {	
														tree		= 1,
														step		= self.steps.mac11.enter_steelsight_speed_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mac11_enter_steelsight_speed"..i,
														title_id	= "debug_mac11_short",
														subtitle_id	= "debug_upgrade_enter_steelsight_speed"..i,
														icon		= "mac11",
														image		= "upgrades_mac10",
														image_slice	= "upgrades_mac10_slice",
														description_text_id = "enter_steelsight_speed_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mac11",
																		upgrade 	= "enter_steelsight_speed_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Ammo clip increase
	for i,_ in ipairs( self.values.mac11.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "mac11_mag"..(i-1) or 	"mac11"
		local unlock_lvl = 82										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mac11_mag"..i ] = {	
														tree		= 1,
														step		= self.steps.mac11.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mac11_mag"..i,
														title_id	= "debug_mac11_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "mac11",
														image		= "upgrades_mac10",
														image_slice	= "upgrades_mac10_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mac11",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
end

-- REMINGTON ------------------------------------------------------------
function UpgradesTweakData:_remington_definitions()
	self.definitions[ "r870_shotgun" ]	= {
														tree		= 3,
														step		= 13,
														category 	= "weapon",
														weapon_id	= "r870_shotgun",
														unit_name 	= Idstring( "units/weapons/r870_shotgun/r870_shotgun" ),
														name_id		= "debug_r870_shotgun",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_r870_shotgun_short",
														icon		= "r870_shotgun",
														image		= "upgrades_remington",
														image_slice	= "upgrades_remington_slice",
														unlock_lvl  = 1,
														prio		= "high",
														description_text_id = "des_r870_shotgun",
														}
														
	for i,_ in ipairs( self.values.r870_shotgun.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "remington_mag"..(i-1) or 	"r870_shotgun"
		local unlock_lvl = 2										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "remington_mag"..i ] = {	
														tree		= 3,
														step		= self.steps.r870_shotgun.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_remington_mag"..i,
														title_id	= "debug_r870_shotgun_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "r870_shotgun",
														image		= "upgrades_remington",
														image_slice	= "upgrades_remington_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "r870_shotgun",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	for i,_ in ipairs( self.values.r870_shotgun.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "remington_recoil"..(i-1) or "r870_shotgun"
		local unlock_lvl = 3										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "remington_recoil"..i ] = {	
														tree		= 3,
														step		= self.steps.r870_shotgun.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_remington_recoil"..i,
														title_id	= "debug_r870_shotgun_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "r870_shotgun",
														image		= "upgrades_remington",
														image_slice	= "upgrades_remington_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "r870_shotgun",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end
	
	for i,_ in ipairs( self.values.r870_shotgun.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "remington_damage"..(i-1) or "r870_shotgun"
		local unlock_lvl = 4										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "remington_damage"..i ] = {	
														tree		= 3,
														step		= self.steps.r870_shotgun.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_remington_damage"..i,
														title_id	= "debug_r870_shotgun_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "r870_shotgun",
														image		= "upgrades_remington",
														image_slice	= "upgrades_remington_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "r870_shotgun",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end
	
end

-- MOSSBERG ------------------------------------------------------------
function UpgradesTweakData:_mossberg_definitions()
	self.definitions[ "mossberg" ]	= {
														tree		= 2,
														step		= 7,
														category 	= "weapon",
														weapon_id	= "mossberg",
														unit_name 	= Idstring( "units/weapons/mossberg/mossberg" ),
														name_id		= "debug_mossberg",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_mossberg_short",
														icon		= "mossberg",
														image		= "upgrades_mossberg",
														image_slice	= "upgrades_mossberg_slice",
														unlock_lvl  = 120,
														prio		= "high",
														description_text_id = "des_mossberg",
														}
	
	-- Ammo clip increase			
	for i,_ in ipairs( self.values.mossberg.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "mossberg_mag"..(i-1) or 	"mossberg"
		local unlock_lvl = 121										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mossberg_mag"..i ] = {	
														tree		= 2,
														step		= self.steps.mossberg.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mossberg_mag"..i,
														title_id	= "debug_mossberg_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "mossberg",
														image		= "upgrades_mossberg",
														image_slice	= "upgrades_mossberg_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mossberg",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	-- Reload speed multiplier
	for i,_ in ipairs( self.values.mossberg.reload_speed_multiplier ) do
		local depends_on = (i-1) > 0 and "mossberg_reload_speed"..(i-1) or "mossberg"		
		local unlock_lvl = 121									-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mossberg_reload_speed"..i ] = {
														tree		= 2,
														step		= self.steps.mossberg.reload_speed_multiplier[ i ], 
														category 	= "feature",
														name_id		= "debug_upgrade_mossberg_reload_speed"..i,
														title_id	= "debug_mossberg_short",
														subtitle_id	= "debug_upgrade_reload_speed"..i,
														icon		= "mossberg",
														image		= "upgrades_mossberg",
														image_slice	= "upgrades_mossberg_slice",
														description_text_id = "reload_speed_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mossberg",
																		upgrade 	= "reload_speed_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Firerate multiplier
	for i,_ in ipairs( self.values.mossberg.fire_rate_multiplier ) do
		local depends_on = (i-1) > 0 and "mossberg_fire_rate_multiplier"..(i-1) or "mossberg"		
		local unlock_lvl = 121										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mossberg_fire_rate_multiplier"..i ] = {
														tree		= 2,
														step		= self.steps.mossberg.fire_rate_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mossberg_fire_rate"..i,
														title_id	= "debug_mossberg_short",
														subtitle_id	= "debug_upgrade_fire_rate"..i,
														icon		= "mossberg",
														image		= "upgrades_mossberg",
														image_slice	= "upgrades_mossberg_slice",
														description_text_id = "fire_rate_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mossberg",
																		upgrade 	= "fire_rate_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.mossberg.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "mossberg_recoil_multiplier"..(i-1) or "mossberg"		
		local unlock_lvl = 121										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mossberg_recoil_multiplier"..i ] = {
														tree		= 2,
														step		= self.steps.mossberg.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_mossberg_recoil_multiplier"..i,
														title_id	= "debug_mossberg_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "mossberg",
														image		= "upgrades_mossberg",
														image_slice	= "upgrades_mossberg_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mossberg",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Melee multiplier
	--[[for i,_ in ipairs( self.values.mossberg.melee_multiplier ) do
		local depends_on = (i-1) > 0 and "mossberg_melee_multiplier"..(i-1) or "mossberg"		
		local unlock_lvl = 121										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "mossberg_melee_multiplier"..i ] = {
														tree		= 2,
														category 	= "feature",
														name_id		= "debug_upgrade_mossberg_melee_multiplier"..i,
														title_id	= "debug_mossberg",
														subtitle_id	= "debug_upgrade_melee_multiplier"..i,
														icon		= "mossberg",
														image		= "upgrades_mossberg",
														image_slice	= "upgrades_mossberg_slice",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "mossberg",
																		upgrade 	= "melee_multiplier",
																		value 		= i
																		}
														}
	end]]
	
	
	
end

-- HK21 ------------------------------------------------------------
function UpgradesTweakData:_hk21_definitions()
	self.definitions[ "hk21" ]	= {
														tree		= 1,
														step		= 22,
														category 	= "weapon",
														weapon_id	= "hk21",
														unit_name 	= Idstring( "units/weapons/hk21/hk21" ),
														name_id		= "debug_hk21",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_hk21_short",
														icon		= "hk21",
														image		= "upgrades_hk21",
														image_slice	= "upgrades_hk21_slice",
														unlock_lvl  = 140,
														prio		= "high",
														description_text_id = "des_hk21",
														}

	-- Ammo clip increase			
	for i,_ in ipairs( self.values.hk21.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "hk21_mag"..(i-1) or "hk21"
		local unlock_lvl = 141										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "hk21_mag"..i ] = {	
														tree		= 1,
														step		= self.steps.hk21.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_hk21_mag"..i,
														title_id	= "debug_hk21_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "hk21",
														image		= "upgrades_hk21",
														image_slice	= "upgrades_hk21_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "hk21",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
	-- Recoil multiplier
	for i,_ in ipairs( self.values.hk21.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "hk21_recoil"..(i-1) or "hk21"		
		local unlock_lvl = 141										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "hk21_recoil"..i ] = {	
														tree		= 1,
														step		= self.steps.hk21.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_hk21_recoil"..i,
														title_id	= "debug_hk21_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "hk21",
														image		= "upgrades_hk21",
														image_slice	= "upgrades_hk21_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "hk21",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end
	
	-- Damage multiplier
	for i,_ in ipairs( self.values.hk21.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "hk21_damage"..(i-1) or "hk21"
		local unlock_lvl = 141										-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "hk21_damage"..i ] = {	
														tree		= 1,
														step		= self.steps.hk21.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_hk21_damage"..i,
														title_id	= "debug_hk21_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "hk21",
														image		= "upgrades_hk21",
														image_slice	= "upgrades_hk21_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "hk21",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end	
end	
	
	
	

-- AK47 ------------------------------------------------------------
function UpgradesTweakData:_ak47_definitions()
	self.definitions[ "ak47" ]	= {
														tree		= 4,
														step		= 9,
														category 	= "weapon",
														weapon_id	= "ak47",
														unit_name 	= Idstring( "units/weapons/ak47/ak" ),
														name_id		= "debug_ak47",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_ak47_short",
														icon		= "ak",
														image		= "upgrades_ak",
														image_slice	= "upgrades_ak_slice",
														unlock_lvl  = 0,
														prio		= "high",
														description_text_id = "des_ak47",
														}
														
														
	for i,_ in ipairs( self.values.ak47.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "ak47_damage"..(i-1) or "ak47"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "ak47_damage"..i ] = {	
														tree		= 4,
														step		= self.steps.ak47.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_ak47_damage"..i,
														title_id	= "debug_ak47_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "ak",
														image		= "upgrades_ak",
														image_slice	= "upgrades_ak_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "ak47",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end	
														

	for i,_ in ipairs( self.values.ak47.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "ak47_recoil"..(i-1) or "ak47"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "ak47_recoil"..i ] = {	
														tree		= 4,
														step		= self.steps.ak47.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_ak47_recoil"..i,
														title_id	= "debug_ak47_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "ak",
														image		= "upgrades_ak",
														image_slice	= "upgrades_ak_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "ak47",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end	



	for i,_ in ipairs( self.values.ak47.spread_multiplier ) do
		local depends_on = (i-1) > 0 and "ak47_spread"..(i-1) or "ak47"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "ak47_spread"..i ] = {	
														tree		= 4,
														step		= self.steps.ak47.spread_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_ak47_spread"..i,
														title_id	= "debug_ak47_short",
														subtitle_id	= "debug_upgrade_spread"..i,
														icon		= "ak",
														image		= "upgrades_ak",
														image_slice	= "upgrades_ak_slice",
														description_text_id = "spread_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "ak47",
																		upgrade 	= "spread_multiplier",
																		value 		= i
																		}
														}
	end





	for i,_ in ipairs( self.values.ak47.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "ak47_mag"..(i-1) or "ak47"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "ak47_mag"..i ] = {	
														tree		= 4,
														step		= self.steps.ak47.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_ak47_mag"..i,
														title_id	= "debug_ak47_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "ak",
														image		= "upgrades_ak",
														image_slice	= "upgrades_ak_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "ak47",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	
end
	

-- Glock ------------------------------------------------------------
function UpgradesTweakData:_glock_definitions()
	self.definitions[ "glock" ]	= {
														tree		= 4,
														step		= 2,
														category 	= "weapon",
														weapon_id	= "glock",
														unit_name 	= Idstring( "units/weapons/glock/glock" ),
														name_id		= "debug_glock",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_glock_short",
														icon		= "glock",
														image		= "upgrades_glock",
														image_slice	= "upgrades_glock_slice",
														unlock_lvl  = 0,
														prio		= "high",
														description_text_id = "des_glock",
														}




	for i,_ in ipairs( self.values.glock.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "glock_damage"..(i-1) or "glock"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "glock_damage"..i ] = {	
														tree		= 4,
														step		= self.steps.glock.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_glock_damage"..i,
														title_id	= "debug_glock_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "glock",
														image		= "upgrades_glock",
														image_slice	= "upgrades_glock_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "glock",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end	
														

	for i,_ in ipairs( self.values.glock.recoil_multiplier ) do
		local depends_on = (i-1) > 0 and "glock_recoil"..(i-1) or "glock"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "glock_recoil"..i ] = {	
														tree		= 4,
														step		= self.steps.glock.recoil_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_glock_recoil"..i,
														title_id	= "debug_glock_short",
														subtitle_id	= "debug_upgrade_recoil"..i,
														icon		= "glock",
														image		= "upgrades_glock",
														image_slice	= "upgrades_glock_slice",
														description_text_id = "recoil_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "glock",
																		upgrade 	= "recoil_multiplier",
																		value 		= i
																		}
														}
	end	





	for i,_ in ipairs( self.values.glock.clip_ammo_increase ) do
		local depends_on = (i-1) > 0 and "glock_mag"..(i-1) or "glock"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "glock_mag"..i ] = {	
														tree		= 4,
														step		= self.steps.glock.clip_ammo_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_glock_mag"..i,
														title_id	= "debug_glock_short",
														subtitle_id	= "debug_upgrade_mag"..i,
														icon		= "glock",
														image		= "upgrades_glock",
														image_slice	= "upgrades_glock_slice",
														description_text_id = "clip_ammo_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "glock",
																		upgrade 	= "clip_ammo_increase",
																		value 		= i
																		}
														}
	end
	



	for i,_ in ipairs( self.values.glock.reload_speed_multiplier ) do
		local depends_on = (i-1) > 0 and "glock_reload_speed"..(i-1) or "glock"		
		local unlock_lvl = 141									-- All will be unlocked here, but they depends on each others anyway
		local prio = i == 1 and "high"
		self.definitions[ "glock_reload_speed"..i ] = {
														tree		= 4,
														step		= self.steps.glock.reload_speed_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_glock_reload_speed"..i,
														title_id	= "debug_glock_short",
														subtitle_id	= "debug_upgrade_reload_speed"..i,
														icon		= "glock",
														image		= "upgrades_glock",
														image_slice	= "upgrades_glock_slice",
														description_text_id = "reload_speed_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "glock",
																		upgrade 	= "reload_speed_multiplier",
																		value 		= i
																		}
														}
	end



end


-- M79 ------------------------------------------------------------
function UpgradesTweakData:_m79_definitions()
	self.definitions[ "m79" ]	= {
														tree		= 4,
														step		= 21,
														category 	= "weapon",
														weapon_id	= "m79",
														unit_name 	= Idstring( "units/weapons/m79/m79" ),
														name_id		= "debug_m79",
														title_id	= "debug_upgrade_new_weapon",
														subtitle_id	= "debug_m79_short",
														icon		= "m79",
														image		= "upgrades_grenade",
														image_slice	= "upgrades_grenade_slice",
														unlock_lvl  = 0,
														prio		= "high",
														description_text_id = "des_m79",
														}
														
														
	for i,_ in ipairs( self.values.m79.damage_multiplier ) do
		local depends_on = (i-1) > 0 and "m79_damage"..(i-1) or "m79"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "m79_damage"..i ] = {	
														tree		= 4,
														step		= self.steps.m79.damage_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m79_damage"..i,
														title_id	= "debug_m79_short",
														subtitle_id	= "debug_upgrade_damage"..i,
														icon		= "m79",
														image		= "upgrades_grenade",
														image_slice	= "upgrades_grenade_slice",
														description_text_id = "damage_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m79",
																		upgrade 	= "damage_multiplier",
																		value 		= i
																		}
														}
	end	
							
							
							
	for i,_ in ipairs( self.values.m79.explosion_range_multiplier ) do
		local depends_on = (i-1) > 0 and "m79_expl_range"..(i-1) or "m79"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "m79_expl_range"..i ] = {	
														tree		= 4,
														step		= self.steps.m79.explosion_range_multiplier[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m79_expl_range"..i,
														title_id	= "debug_m79_short",
														subtitle_id	= "debug_upgrade_expl_range"..i,
														icon		= "m79",
														image		= "upgrades_grenade",
														image_slice	= "upgrades_grenade_slice",
														description_text_id = "explosion_range_multiplier",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m79",
																		upgrade 	= "explosion_range_multiplier",
																		value 		= i
																		}
														}
	end

					
	for i,_ in ipairs( self.values.m79.clip_amount_increase ) do
		local depends_on = (i-1) > 0 and "m79_clip_num"..(i-1) or "m79"
		local unlock_lvl = 141
		local prio = i == 1 and "high"
		self.definitions[ "m79_clip_num"..i ] = {	
														tree		= 4,
														step		= self.steps.m79.clip_amount_increase[ i ],
														category 	= "feature",
														name_id		= "debug_upgrade_m79_clip_num"..i,
														title_id	= "debug_m79_short",
														subtitle_id	= "debug_upgrade_clip_num"..i,
														icon		= "m79",
														image		= "upgrades_grenade",
														image_slice	= "upgrades_grenade_slice",
														description_text_id = "clip_amount_increase",
														depends_on	= depends_on,
														unlock_lvl	= unlock_lvl,
														prio		= prio,
														upgrade		= { category 	= "m79",
																		upgrade 	= "clip_amount_increase",
																		value 		= i
																		}
														}
	end
														
														
end
