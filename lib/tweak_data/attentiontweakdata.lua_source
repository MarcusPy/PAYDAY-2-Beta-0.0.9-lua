
--[[
reaction:
	REACT_IDLE -- glance when idling
	REACT_CURIOUS -- inspect, stare persistently
	REACT_SUSPICIOUS -- as in curious but if "suspicion_range" parameter is present, the observer may see through the disquise of the attention object
	REACT_SURPRISED -- stare and show amazement
	REACT_SCARED -- stare and show fright
	REACT_AIM -- aim at the object
	REACT_ARREST -- arrest logic ( aim, say "freeze!" and call reenforcements )
	REACT_DISARM -- walk towards the object and put handcuffs on it
	REACT_SHOOT -- shoot at the object until it's dead
	REACT_MELEE -- hit the object in melee
	REACT_COMBAT -- engage in combat behaviour treating the object as a threat
	REACT_SPECIAL_ATTACK -- tase, spooc

filter:
	all
	civilian
	law_enforcer
	gangster
	all_enemy
	criminal
	criminals_and_enemies
	civilians_enemies
	criminals_enemies_civilians
	
notice_clbk:
	clbk_attention_notice_sneak
	
self.settings.some_unique_name = {
	reaction = "REACT_CURIOUS", -- see above: REACT_*
	filter = "criminal",
	max_range = 5000, -- effective range. nil means infinite. (cm)
	notice_delay_mul = 2, -- delay multiplier applied to the observer's tweak_data.character[ "swat or whatever" ].detection[ "recon or whatever"].delay. nil means no multiplier.
	attract_chance = 0.3, -- chance that an observer will actually bother to look at the attention object for the duration period
	notice_interval = 0.5, -- how often observers check if they will notice the attention object
	verification_interval = 4, -- how often observers should verify the attention. (sec)
	release_delay = 1, -- how soon does the observer discard this attention when it hasn't been visible, in FOV & in range. Note: gets overriden for enemies in combat (sec) 
	duration = { 1.5, 2.5 }, -- after that long, the observer pauses or stops paying attention. {min, max} randomized linearly. (sec)
	pause = { 45, 90 }, -- after the duration has expired, this is how long the observer needs to wait before paying attention again. { min, max } randomized linearly. (sec)
	notice_clbk = "clbk_attention_notice_sneak", -- name of callback to be called while an observer is counting down to noticing the attention object
	notice_requires_FOV = false, -- does the object need to be seen in order to be noticed? Useful when simulating audible attention objects.
	suspicion_range = 350, -- at this distance the suspicion meter starts building
	suspicion_duration = 4, -- how long it takes for a player with suspicion multiplier 1 to be uncovered when standing at suspicion_range distance. (sec)
	turn_around_range = 250, -- at this distance the observer will want to keep the suspect in sight
	uncover_range = 170, -- at this distance the suspect is uncovered instantly
	weight_mul = 2, -- this value affects the observer's choice of attention.
}
]]

AttentionTweakData = AttentionTweakData or class()

function AttentionTweakData:init()
	self.settings = {}
	self.indexes = {}
	
	self:_init_player()
	self:_init_team_AI()
	self:_init_civilian()
	self:_init_enemy()
	self:_init_drill()
	self:_init_sentry_gun()
	self:_init_prop()
	self:_init_custom()
	
	self:_post_init()
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_player()
	
	self.settings.pl_team_cur_peaceful = { -- attracts Team AI when in mask-off mode
		reaction = "REACT_IDLE",
		filter = "criminal",
		max_range = 1000,
		notice_delay_mul = 1,
		verification_interval = 4,
		release_delay = 3,
		duration = { 2, 5 },
		pause = { 15, 25 },
		notice_requires_FOV = false,
	}
	
	self.settings.pl_team_idle_std = { -- attracts Team AI when in combat
		reaction = "REACT_IDLE",
		filter = "criminal",
		max_range = 1000,
		notice_delay_mul = 0,
		verification_interval = 4,
		release_delay = 3,
		duration = { 1.7, 2.5 },
		pause = { 45, 90 },
		notice_requires_FOV = false,
	}
	
	self.settings.pl_gangster_cur_peaceful = { -- attracts gangsters when in mask-off mode
		reaction = "REACT_SUSPICIOUS",
		filter = "gangster",
		max_range = 600,
		suspicion_range = 500,
		suspicion_duration = 5.5,
		turn_around_range = 250,
		uncover_range = 90,
		notice_delay_mul = 0.3,
		verification_interval = 0.02,
		release_delay = 2,
		notice_requires_FOV = true,
	}
	
	self.settings.pl_gangster_cbt = { -- attracts gangsters when in combat
		reaction = "REACT_COMBAT",
		filter = "gangster",
		verification_interval = 1,
		release_delay = 1,
		uncover_range = 550,
		notice_requires_FOV = true,
	}
	
	self.settings.pl_law_susp_peaceful = { -- attracts pigs when in mask-off mode
		reaction = "REACT_SUSPICIOUS",
		filter = "law_enforcer",
		max_range = 600,
		suspicion_range = 500,
		suspicion_duration = 4,
		turn_around_range = 250,
		uncover_range = 150,
		notice_delay_mul = 0.3,
		verification_interval = 0.02,
		release_delay = 2,
		notice_requires_FOV = true,
	}
	
	self.settings.pl_enemy_cur_peaceful = { -- attracts enemies when in mask-off mode
		reaction = "REACT_CURIOUS",
		filter = "all_enemy",
		max_range = 600,
		notice_delay_mul = 0.5,
		verification_interval = 2,
		release_delay = 1,
		duration = { 1.5, 3.5 },
		pause = { 25, 50 },
		notice_requires_FOV = true,
	}
	
	self.settings.pl_enemy_cbt = { -- attracts enemies when in combat
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		verification_interval = 1,
		notice_interval = 0.1,
		uncover_range = 550, 
		release_delay = 1,
		notice_clbk = "clbk_attention_notice_sneak",
		notice_requires_FOV = true,
	}
	
	self.settings.pl_enemy_cbt_crh = { -- attracts enemies when in mask-on mode and crouching and it is not whisper_mode
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		verification_interval = 0.1, -- high timing resolution required to enable the player to react
		uncover_range = 350, 
		release_delay = 1,
		notice_delay_mul = 2,
		notice_requires_FOV = true,
	}
	
	self.settings.pl_enemy_sneak = { -- attracts enemies when in mask-on mode and crouching and whisper_mode
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		verification_interval = 0.1, -- high timing resolution required to enable the player to react
		uncover_range = 350, 
		release_delay = 1,
		notice_delay_mul = 2,
		max_range = 1500,
		notice_clbk = "clbk_attention_notice_sneak",
		notice_requires_FOV = true,
	}
	
	self.settings.pl_civ_idle_peaceful = { -- attracts civilians when in mask-off mode
		reaction = "REACT_IDLE",
		filter = "civilian",
		max_range = 600,
		notice_delay_mul = 0,
		notice_interval = 0.5,
		attract_chance = 0.5,
		verification_interval = 2,
		release_delay = 3,
		duration = { 2, 15 },
		pause = { 10, 60 },
		notice_requires_FOV = true,
	}
	
	self.settings.pl_civ_sneak = { -- attracts civilians when in mask-on mode and crouching
		reaction = "REACT_COMBAT",
		filter = "civilian",
		verification_interval = 0.1, -- high timing resolution required to enable the player to react
		uncover_range = 200,
		release_delay = 1,
		notice_delay_mul = 3,
		max_range = 1500,
		notice_clbk = "clbk_attention_notice_sneak",
		notice_requires_FOV = true,
	}
	
	self.settings.pl_civ_cbt = { -- attracts civilians when in mask-on mode
		reaction = "REACT_COMBAT",
		filter = "civilian",
		verification_interval = 0.1,
		uncover_range = 550,
		release_delay = 1,
		notice_delay_mul = 1.5,
		notice_clbk = "clbk_attention_notice_sneak",
		notice_requires_FOV = true,
	}
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_team_AI()
	
	self.settings.team_team_idle = { -- attracts friendlies when peaceful
		reaction = "REACT_IDLE",
		filter = "criminal",
		max_range = 1000,
		verification_interval = 3,
		release_delay = 2,
		duration = { 1.5, 4 },
		pause = { 25, 40 },
		notice_requires_FOV = false, -- does the object need to be seen in order to be noticed?
	}
	
	self.settings.team_enemy_idle = { -- attracts friendlies when peaceful
		reaction = "REACT_IDLE",
		filter = "all_enemy",
		max_range = 550,
		verification_interval = 3,
		release_delay = 1,
		duration = { 1.5, 3 },
		pause = { 35, 60 },
		notice_requires_FOV = false, -- does the object need to be seen in order to be noticed?
	}
	
	self.settings.team_enemy_cbt = { -- attracts enemies when in combat
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		max_range = 20000,
		notice_interval = 1,
		verification_interval = 1.5,
		uncover_range = 400,
		release_delay = 1,
		notice_requires_FOV = true,
		weight_mul = 0.5 -- pay less attention to TeamAI than human players
	}
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_civilian()
	self.settings.civ_all_peaceful = { -- attracts other AI units while peaceful
		reaction = "REACT_IDLE",
		filter = "all",
		max_range = 2000,
		verification_interval = 3,
		release_delay = 2,
		duration = { 1.5, 6 },
		pause = { 35, 60 },
		notice_requires_FOV = true,
	}
	
	self.settings.civ_enemy_cbt = { -- attracts enemies while alive and scared and global enemy weapons cold
		reaction = "REACT_SCARED",
		filter = "all_enemy",
		max_range = 8000,
		uncover_range = 300, 
		notice_delay_mul = 0.6,
		verification_interval = 0.1,
		release_delay = 6,
		duration = { 3, 6 },
		notice_clbk = "clbk_attention_notice_corpse",
		notice_requires_FOV = true,
	}
	
	self.settings.civ_enemy_corpse_sneak = {
		reaction = "REACT_SCARED",
		filter = "civilians_enemies",
		max_range = 2500,
		uncover_range = 300, 
		notice_delay_mul = 0.05,
		verification_interval = 0.1,
		release_delay = 6,
		notice_requires_FOV = true,
	}
	
	self.settings.civ_civ_cbt = { -- attracts civilians while alive and scared and global enemy weapons cold
		reaction = "REACT_SCARED",
		filter = "civilian",
		uncover_range = 300, 
		notice_delay_mul = 0.05,
		verification_interval = 0.1,
		release_delay = 6,
		duration = { 3, 6 },
		notice_requires_FOV = true,
	}
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_enemy()
	
	self.settings.enemy_team_idle = {
		reaction = "REACT_IDLE",
		filter = "criminal",
		max_range = 2000,
		verification_interval = 3,
		release_delay = 1,
		duration = { 2, 4 },
		pause = { 9, 40 },
		notice_requires_FOV = false,
	}
	
	self.settings.enemy_team_cbt = {
		reaction = "REACT_COMBAT",
		filter = "criminal",
		max_range = 20000,
		notice_delay_mul = 0,
		notice_interval = 0.2,
		verification_interval = 0.75,
		release_delay = 2,
		notice_requires_FOV = false, -- teamAI does not receive alerts from enemies, therefore it is better to detect even outside FOV
	}
	
	self.settings.enemy_law_corpse_sneak = self.settings.civ_enemy_corpse_sneak
	
	self.settings.enemy_team_corpse_sneak = self.settings.civ_enemy_corpse_sneak
	
	self.settings.enemy_law_corpse_cbt = {
		reaction = "REACT_CURIOUS",
		filter = "law_enforcer",
		max_range = 800,
		notice_delay_mul = 0.1,
		verification_interval = 1.5,
		release_delay = 1,
		duration = { 2, 3 },
		pause = { 30, 120 },
		notice_requires_FOV = true,
	}
	
	self.settings.enemy_team_corpse_cbt = {
		reaction = "REACT_IDLE",
		filter = "criminal",
		max_range = 800,
		notice_delay_mul = 2,
		verification_interval = 1.5,
		release_delay = 0,
		duration = { 2, 3 },
		pause = { 50, 360 },
		notice_requires_FOV = true,
	}
	
	self.settings.enemy_enemy_cbt = { -- attracts other enemies when attention object is in combat and global enemy weapons cold
		reaction = "REACT_SCARED",
		filter = "all_enemy",
		max_range = 8000,
		uncover_range = 300, 
		notice_delay_mul = 0.5,
		verification_interval = 0.5,
		release_delay = 1,
		notice_requires_FOV = true,
	}
	
	self.settings.enemy_civ_cbt = { -- attracts civilians while alive and scared and global enemy weapons cold
		reaction = "REACT_SCARED",
		filter = "civilian",
		max_range = 8000,
		uncover_range = 300, 
		notice_delay_mul = 0.2,
		verification_interval = 0.5,
		release_delay = 6,
		duration = { 1.5, 3 },
		notice_requires_FOV = true,
	}
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_custom()
	
	self.settings.custom_void = {
		reaction = "REACT_IDLE",
		filter = "none",
		max_range = 2000,
		verification_interval = 10,
		release_delay = 10,
	}
	
	self.settings.custom_team_idle = {
		reaction = "REACT_IDLE",
		filter = "criminal",
		max_range = 2000,
		verification_interval = 3,
		release_delay = 1,
		duration = { 2, 4 },
		pause = { 9, 40 },
		notice_requires_FOV = false,
	}
	
	self.settings.custom_team_cbt = {
		reaction = "REACT_COMBAT",
		filter = "criminal",
		max_range = 20000,
		verification_interval = 1.5,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_team_shoot_const = {
		reaction = "REACT_SHOOT",
		filter = "criminal",
		max_range = 10000,
		verification_interval = 1.5,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_team_shoot_burst = {
		reaction = "REACT_SHOOT",
		filter = "criminal",
		max_range = 10000,
		verification_interval = 1.5,
		release_delay = 2,
		duration = { 2, 4 },
		notice_requires_FOV = false,
	}
	
	self.settings.custom_team_aim_const = {
		reaction = "REACT_AIM",
		filter = "criminal",
		max_range = 10000,
		verification_interval = 1.5,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_enemy_forest_survive_kruka = {
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		max_range = 20000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_enemy_suburbia_shootout = {
		reaction = "REACT_SHOOT",
		filter = "all_enemy",
		max_range = 12000,
		verification_interval = 2,
		release_delay = 5,
		turn_around_range = 15000,
		notice_requires_FOV = true,
		weight_mul = 0.5,
	}
	
	self.settings.custom_enemy_suburbia_shootout_cops = {
		reaction = "REACT_SHOOT",
		filter = "all_enemy",
		max_range = 2000,
		verification_interval = 2,
		release_delay = 5,
		turn_around_range = 15000,
		notice_requires_FOV = true,
	}
	
	self.settings.custom_enemy_china_store_vase_shoot = {
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		max_range = 1200,
		verification_interval = 2,
		release_delay = 3,
		turn_around_range = 500,
		notice_requires_FOV = true,
	}
	
	self.settings.custom_enemy_china_store_vase_melee = {
		reaction = "REACT_MELEE",
		filter = "all_enemy",
		max_range = 500,
		verification_interval = 5,
		release_delay = 10,
		pause = 10,
		turn_around_range = 250,
		notice_requires_FOV = true,
	}
	
		self.settings.custom_enemy_china_store_vase_aim = {
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		max_range = 500,
		verification_interval = 5,
		release_delay = 10,
		pause = 10,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_enemy_shoot_const = {
		reaction = "REACT_SHOOT",
		filter = "all_enemy",
		max_range = 10000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = true,
	}
	
	self.settings.custom_gangster_shoot_const = {
		reaction = "REACT_SHOOT",
		filter = "gangster",
		max_range = 10000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = true,
	}
	
	self.settings.custom_law_shoot_const = {
		reaction = "REACT_SHOOT",
		filter = "law_enforcer",
		max_range = 100000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_law_look_in_container = {
		reaction = "REACT_AIM",
		filter = "law_enforcer",
		max_range = 100000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_law_shoot_const_escape_vehicle = {
		reaction = "REACT_COMBAT",
		filter = "law_enforcer",
		max_range = 4500,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_law_shoot_const_container = {
		reaction = "REACT_SHOOT",
		filter = "law_enforcer",
		max_range = 2000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_gangsters_shoot_warehouse = {
		reaction = "REACT_COMBAT",
		filter = "gangster",
		max_range = 2000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	self.settings.custom_gangster_sniper_apartment_suspicous = {
		reaction = "REACT_SCARED",
		filter = "law_enforcer",
		max_range = 850,
		verification_interval = 1,
		release_delay = 6,
		notice_requires_FOV = true,
		uncover_range = 350, 
		notice_delay_mul = 0.1,
	}
	
	self.settings.custom_gangster_docks_idle = {
		reaction = "REACT_CURIOUS",
		filter = "gangster",
		max_range = 10000,
		verification_interval = 1,
		release_delay = 6,
		notice_requires_FOV = true,
	}
	
	self.settings.custom_enemy_civ_scared = { -- attracts other enemies when attention object is in combat and global enemy weapons cold
		reaction = "REACT_SCARED",
		filter = "civilians_enemies",
		verification_interval = 5,
		release_delay = 2,
		duration = { 2, 4 },
		notice_requires_FOV = true,
	}
	
	self.settings.custom_boat_gangster = {
		reaction = "REACT_COMBAT",
		filter = "gangster",
		max_range = 4000,
		verification_interval = 1,
		release_delay = 2,
		notice_requires_FOV = false,
	}
	
	
	self.settings.custom_law_cbt = { -- attracts enemies when in combat
		reaction = "REACT_COMBAT",
		filter = "law_enforcer",
		verification_interval = 1,
		uncover_range = 350, 
		release_delay = 1,
		notice_clbk = "clbk_attention_notice_sneak",
		notice_requires_FOV = true,
	}
	
	self.settings.custom_airport_window = {
		reaction = "REACT_CURIOUS",
		filter = "all_enemy",
		max_range = 1500,
		uncover_range = 100, 
		notice_delay_mul = 0.2,
		verification_interval = 1.5,
		release_delay = 6,
		duration = { 3, 6 },
		notice_requires_FOV = true,
	}

	self.settings.custom_look_at = {
		reaction = "REACT_IDLE",
		filter = "all_enemy",
		max_range = 15000,
		notice_delay_mul = 0.2,
		verification_interval = 1,
		release_delay = 3,
		notice_requires_FOV = false,
	}

	self.settings.custom_look_at_FOV = {
		reaction = "REACT_CURIOUS",
		filter = "all_enemy",
		max_range = 1500,
		notice_delay_mul = 0.2,
		verification_interval = 1.5,
		release_delay = 6,
		duration = { 3, 6 },
		notice_requires_FOV = true,
	}

	self.settings.custom_server_room = {
		reaction = "REACT_SCARED",
		filter = "all_enemy",
		max_range = 350,
		uncover_range = 100, 
		notice_delay_mul = 0.2,
		verification_interval = 1.5,
		release_delay = 6,
		duration = { 3, 6 },
		notice_requires_FOV = true,
	}
	
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_drill()
	self.settings.drill_civ_ene_ntl = { -- attracts civilians and enemies when enemy_weapons_cold
		reaction = "REACT_SCARED",
		filter = "civilians_enemies",
		verification_interval = 0.4,
		uncover_range = 200, 
		release_delay = 1,
		notice_requires_FOV = false,
	}
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_sentry_gun()
	self.settings.sentry_gun_enemy_cbt = { -- attracts enemies when in combat
		reaction = "REACT_COMBAT",
		filter = "all_enemy",
		verification_interval = 1.5,
		uncover_range = 300, 
		release_delay = 1,
	}
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_init_prop()
	self.settings.prop_civ_ene_ntl = { -- attracts civilians and enemies when enemy_weapons_cold
		reaction = "REACT_AIM",
		filter = "civilians_enemies",
		verification_interval = 0.4,
		uncover_range = 500, 
		release_delay = 1,
		notice_requires_FOV = true,
	}

	self.settings.prop_ene_ntl = { -- attracts enemies when enemy_weapons_cold
		reaction = "REACT_AIM",
		filter = "all_enemy",
		verification_interval = 0.4,
		uncover_range = 500, 
		release_delay = 1,
		notice_requires_FOV = true,
	}
	
	self.settings.broken_cam_ene_ntl = { -- attracts civilians and enemies when enemy_weapons_cold
		reaction = "REACT_AIM",
		filter = "law_enforcer",
		verification_interval = 0.4,
		uncover_range = 100, 
		suspicion_range = 1000,
		max_range = 1200,
		release_delay = 1,
		notice_requires_FOV = true,
	}
	
	self.settings.prop_law_scary = { -- attracts civilians and enemies when enemy_weapons_cold
		reaction = "REACT_SCARED",
		filter = "law_enforcer",
		verification_interval = 0.4,
		uncover_range = 300, 
		release_delay = 1,
		notice_requires_FOV = true,
	}

	self.settings.prop_state_civ_ene_ntl = { -- attracts civilians and enemies when enemy_weapons_cold
		reaction = "REACT_CURIOUS",
		filter = "civilians_enemies",
		verification_interval = 0.4,
		uncover_range = 200, 
		release_delay = 1,
		notice_requires_FOV = true,
	}
	
end

--------------------------------------------------------------------------------------

function AttentionTweakData:get_attention_index( setting_name )
	for i_setting, test_setting_name in ipairs( self.indexes ) do
		if setting_name == test_setting_name then
			return i_setting
		end
	end
end

--------------------------------------------------------------------------------------

function AttentionTweakData:get_attention_name( index )
	return self.indexes[ index ]
end

--------------------------------------------------------------------------------------

function AttentionTweakData:_post_init()
	for setting_name, setting in pairs( self.settings ) do
		table.insert( self.indexes, setting_name )
	end
end
