GroupAITweakData = GroupAITweakData or class()

function GroupAITweakData:init( tweak_data )
	self:_read_mission_preset( tweak_data ) -- Leave this alone
	self:_create_table_structure() -- Leave this alone
	
	self:_init_task_data()
	self:_init_chatter_data()
	self:_init_unit_categories()
	self:_init_enemy_spawn_groups()
	
	self._level_mod = "CS_normal"
end



-----------------------------------------------------------------------------

function GroupAITweakData:_init_chatter_data()
	self.enemy_chatter.aggressive = {
		radius = 1000,
		max_nr = 3,
		duration = { 1, 3 },
		interval = { 2, 5 },	-- global chatter cooldown
		group_min = 3,
		queue = "g90"
	}
	self.enemy_chatter.retreat = {
		radius = 700,
		max_nr = 2,
		duration = { 2, 4 },
		interval = { 0.75, 1.5 },
		group_min = 3,
		queue = "m01"
	}
	self.enemy_chatter.follow_me = {
		radius = 700,
		max_nr = 1,
		duration = { 5, 10 },
		interval = { 0.75, 1.5 },
		group_min = 2,
		queue = "mov"
	}
	self.enemy_chatter.clear = {
		radius = 700,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.75, 1.5 },
		group_min = 3,
		queue = "clr"
	}
	self.enemy_chatter.go_go = {
		radius = 700,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.75, 1.2 },
		group_min = 0,
		queue = "mov"
	}
	self.enemy_chatter.ready = {
		radius = 700,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.75, 1.2 },
		group_min = 3,
		queue = "rdy"
	}
	self.enemy_chatter.smoke = {
		radius = 0,
		max_nr = 1,
		duration = { 0, 0 },
		interval = { 0, 0 },
		group_min = 2,
		queue = "d01"
	}
	self.enemy_chatter.flash_grenade = {
		radius = 0,
		max_nr = 1,
		duration = { 0, 0 },
		interval = { 0, 0 },
		group_min = 2,
		queue = "d02"
	}
	self.enemy_chatter.incomming_tank = {
		radius = 1000,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.5, 1 },
		group_min = 0,
		queue = "bdz"
	}
	self.enemy_chatter.incomming_spooc = {
		radius = 1000,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.5, 1 },
		group_min = 0,
		queue = "clk"
	}
	self.enemy_chatter.incomming_shield = {
		radius = 1000,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.5, 1 },
		group_min = 0,
		queue = "shd"
	}
	self.enemy_chatter.incomming_taser = {
		radius = 1000,
		max_nr = 1,
		duration = { 60, 60 },
		interval = { 0.5, 1 },
		group_min = 0,
		queue = "tsr"
	}
	
end

-----------------------------------------------------------------------------

function GroupAITweakData:_init_unit_categories()
	local access_type_walk_only = { walk = true }
	local access_type_all = { walk = true, acrobatic = true }
	
	-- Notes: 
	-- max_amount applies only to boss units
	-- A unit name may appear in more than one category
	-- Mixing boss and non-boss units in the same category is confusing for GroupAI
	-- access defines what "preferred" spawn points the unit may be spawned from
	
	self.unit_categories = {
		security = {
			units = {
				Idstring( "units/payday2/characters/ene_security_1/ene_security_1" ),
				Idstring( "units/payday2/characters/ene_security_2/ene_security_2" ),
				Idstring( "units/payday2/characters/ene_security_3/ene_security_3" )
			},
			access = access_type_walk_only
		},
		cop = {
			units = {
				Idstring( "units/payday2/characters/ene_cop_1/ene_cop_1" ),
				Idstring( "units/payday2/characters/ene_cop_2/ene_cop_2" ),
				Idstring( "units/payday2/characters/ene_cop_4/ene_cop_4" )
			},
			access = access_type_walk_only
		},
		cop_special = {
			units = {
				Idstring( "units/payday2/characters/ene_cop_3/ene_cop_3" )
			},
			access = access_type_walk_only
		},
		fbi = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_1/ene_fbi_1" ),
				Idstring( "units/payday2/characters/ene_fbi_2/ene_fbi_2" ),
				Idstring( "units/payday2/characters/ene_fbi_3/ene_fbi_3" )
			},
			access = access_type_walk_only
		},
		fbi_special = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_3/ene_fbi_3" ) -- TEMP
			},
			access = access_type_all
		},
		swat = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_1/ene_swat_1" ),
				Idstring( "units/payday2/characters/ene_swat_2/ene_swat_2" )
			},
			access = access_type_all
		},
		swat_assault = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_1/ene_swat_1" )
			},
			access = access_type_all
		},
		swat_close = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_2/ene_swat_2" )
			},
			access = access_type_all
		},
		swat_kevlar = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_swat_1/ene_fbi_swat_1" ),
				Idstring( "units/payday2/characters/ene_fbi_swat_2/ene_fbi_swat_2" )
			},
			access = access_type_all
		},
		swat_kevlar_assault = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_swat_1/ene_fbi_swat_1" )
			},
			access = access_type_all
		},
		swat_kevlar_close = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_swat_2/ene_fbi_swat_2" )
			},
			access = access_type_all
		},
		swat_heavy = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_heavy_1/ene_swat_heavy_1" )
			},
			access = access_type_all
		},
		tank = {
			units = {
				Idstring( "units/payday2/characters/ene_bulldozer_1/ene_bulldozer_1" )
			},
			access = access_type_walk_only,
			max_amount = 1,
			special_type = "tank" -- If the unit is a boss, spcify its tweak table name here
		},
		shield = {
			units = {
				Idstring( "units/payday2/characters/ene_shield_1/ene_shield_1" )
			},
			access = access_type_walk_only,
			max_amount = 2,
			special_type = "shield" -- If the unit is a boss, spcify its tweak table name here
		},
		shield_small = {
			units = {
				Idstring( "units/payday2/characters/ene_shield_2/ene_shield_2" )
			},
			access = access_type_walk_only,
			max_amount = 2,
			special_type = "shield" -- If the unit is a boss, spcify its tweak table name here
		},
		spooc = {
			units = {
				Idstring( "units/payday2/characters/ene_spook_1/ene_spook_1" )
			},
			access = access_type_all,
			max_amount = 2,
			special_type = "spooc" -- If the unit is a boss, spcify its tweak table name here
		},
		taser = {
			units = {
				Idstring( "units/payday2/characters/ene_tazer_1/ene_tazer_1" )
			},
			access = access_type_all,
			max_amount = 2,
			special_type = "taser" -- If the unit is a boss, spcify its tweak table name here
		},
		sniper = {
			units = {
				Idstring( "units/payday2/characters/ene_sniper_1/ene_sniper_1" )
			},
			access = access_type_all
		},
		
		
-- NEW
-- GENERAL
	-- Security
		GEN_security = {
			units = {
				Idstring( "units/payday2/characters/ene_security_1/ene_security_1" ),
				Idstring( "units/payday2/characters/ene_security_2/ene_security_2" ),
				Idstring( "units/payday2/characters/ene_security_3/ene_security_3" )
			},
			access = access_type_walk_only
		},
	-- Sniper
		GEN_sniper = {
			units = {
				Idstring( "units/payday2/characters/ene_sniper_1/ene_sniper_1" )
			},
			access = access_type_all
		},
		
-- COPS/SWAT
	-- CS Cops
		CS_cop_C45_R870 = {
			units = {
				Idstring( "units/payday2/characters/ene_cop_1/ene_cop_1" ),
				Idstring( "units/payday2/characters/ene_cop_2/ene_cop_2" ),
				Idstring( "units/payday2/characters/ene_cop_4/ene_cop_4" )
			},
			access = access_type_walk_only
		},
		CS_cop_stealth_MP5 = {
			units = {
				Idstring( "units/payday2/characters/ene_cop_3/ene_cop_3" )
			},
			access = access_type_walk_only
		},
	-- CS Swat
		CS_swat_MP5 = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_1/ene_swat_1" )
			},
			access = access_type_all
		},
		CS_swat_R870 = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_2/ene_swat_2" )
			},
			access = access_type_all
		},
	-- CS Heavy
		CS_heavy_M4 = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_heavy_1/ene_swat_heavy_1" )
			},
			access = access_type_all
		},
		CS_heavy_M4_w = {
			units = {
				Idstring( "units/payday2/characters/ene_swat_heavy_1/ene_swat_heavy_1" )
			},
			access = access_type_walk_only
		},
		-- CS Taser
		CS_tazer = {
			units = {
				Idstring( "units/payday2/characters/ene_tazer_1/ene_tazer_1" )
			},
			access = access_type_all,
			max_amount = 3,
			special_type = "taser" -- If the unit is a boss, spcify its tweak table name here
		},
	-- CS Shield
		CS_shield = {
			units = {
				Idstring( "units/payday2/characters/ene_shield_2/ene_shield_2" )
			},
			access = access_type_walk_only,
			max_amount = 4,
			special_type = "shield" -- If the unit is a boss, spcify its tweak table name here
		},
		
-- FBI
	-- FBI Suit
		FBI_suit_C45_M4 = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_1/ene_fbi_1" ),
				Idstring( "units/payday2/characters/ene_fbi_2/ene_fbi_2" ),
			},
			access = access_type_all
		},
		FBI_suit_M4_MP5 = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_2/ene_fbi_2" ),
				Idstring( "units/payday2/characters/ene_fbi_3/ene_fbi_3" ),
			},
			access = access_type_all
		},
		FBI_suit_stealth_MP5 = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_3/ene_fbi_3" )
			},
			access = access_type_all
		},
	-- FBI Swat
		FBI_swat_M4 = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_swat_1/ene_fbi_swat_1" )
			},
			access = access_type_all
		},
		FBI_swat_R870 = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_swat_2/ene_fbi_swat_2" )
			},
			access = access_type_all
		},
	-- FBI Heavy
		FBI_heavy_G36 = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_heavy_1/ene_fbi_heavy_1" )
			},
			access = access_type_all
		},
		FBI_heavy_G36_w = {
			units = {
				Idstring( "units/payday2/characters/ene_fbi_heavy_1/ene_fbi_heavy_1" )
			},
			access = access_type_walk_only
		},
		-- FBI Spooc
		FBI_spooc = {
			units = {
				Idstring( "units/payday2/characters/ene_spook_1/ene_spook_1" )
			},
			max_amount = 6,
			access = access_type_all,
			special_type = "spooc"
		},
	-- Shield
		FBI_shield = {
			units = {
				Idstring( "units/payday2/characters/ene_shield_1/ene_shield_1" )
			},
			access = access_type_walk_only,
			max_amount = 4,
			special_type = "shield" -- If the unit is a boss, spcify its tweak table name here
		},
	-- FBI Tank
		FBI_tank = {
			units = {
				Idstring( "units/payday2/characters/ene_bulldozer_1/ene_bulldozer_1" )
			},
			access = access_type_walk_only,
			max_amount = 2,
			special_type = "tank" -- If the unit is a boss, spcify its tweak table name here
		}
	}
end

-----------------------------------------------------------------------------
--[[
TACTIC DESCRIPTIONS:
group:
	deathguard: If possessed by the leader. The whole group aggressively occupies areas with downed criminals.
	flank: If possessed by the leader. The group tries to find alternate attack positions instead of the closest one.
	ranged_fire: If possessed by the leader. The group will open fire for about 15 seconds from a nearby position during assault before pushing.
personal:
	shield: If possessed by any member. The member provides cover to other members who possess the "shield_cover" tactic.
	provide_coverfire: If possessed by any member. The member becomes aggressive when teammates are assaulted.
	provide_support: If possessed by any member. The member becomes aggressive when teammates become aggressive.
	shield_cover: If possessed by any member. The member takes cover behind other members who possess the "shield" tactic.
	smoke_grenade: If possessed by any member. A smoke grenade is detonated right before the group moves in to assault.
	flash_grenade: If possessed by any member. A flashbang grenade is detonated right before the group moves in to assault.
	murder: If possessed by any member. The member will shoot criminals who are in bleedout even when they are not aggressive.
	
	Proposed tactics
	
	Door Ambush group
	2-2 movement
	3-1 movement
	4 movement
	1-3 movement
	Front of player 
	Splinter of group position in between players
	Loot bag retrievers
	Hit loners
	Move up on player on Reload
	Objective blockers - block the path from player to exit
	Machine sabotage - turns off drills and removes c4/tripmines ULF: If this is the same guy, tripmine value goes down dramatically.
  Support fire group - Tries to suppress player and player exit out of room.
Breach group - if doors are ziptied or boarded
Smoke and retreat
Breach and clear - flashbang	
	
]]

function GroupAITweakData:_init_enemy_spawn_groups()
-- NEW
	-- TODO : Add flank and ranged_fire to groups, and debug
	-- TODO : Remove support and coverfire and debug
	
	-- CS
	local tactics_CS_cop = { "provide_coverfire", "provide_support", "ranged_fire" }
	local tactics_CS_cop_stealth = { "flank", "provide_coverfire", "provide_support" }
	
	local tactics_CS_swat_rifle = { "smoke_grenade", "charge", "provide_coverfire", "provide_support", "ranged_fire" }
	local tactics_CS_swat_shotgun = { "smoke_grenade", "charge", "provide_coverfire", "provide_support" }
	local tactics_CS_swat_heavy = { "smoke_grenade", "charge", "provide_coverfire", "provide_support", "shield_cover" }
	local tactics_CS_shield = { "charge", "provide_coverfire", "provide_support", "shield" }
	
	local tactics_CS_swat_rifle_flank = { "flank", "flash_grenade", "smoke_grenade", "charge", "provide_coverfire", "provide_support" }
	local tactics_CS_swat_shotgun_flank = { "flank", "flash_grenade", "smoke_grenade", "charge", "provide_coverfire", "provide_support" }
	local tactics_CS_swat_heavy_flank = { "flank", "flash_grenade", "smoke_grenade", "charge", "provide_coverfire", "provide_support", "shield_cover" }
	local tactics_CS_shield_flank = { "flank", "charge", "flash_grenade", "provide_coverfire", "provide_support", "shield" }
	
	local tactics_CS_tazer = { "flank", "charge", "flash_grenade", "provide_coverfire", "provide_support" }
	
	-- FBI
	local tactics_FBI_suit = { "provide_coverfire", "provide_support", "ranged_fire" }
	local tactics_FBI_suit_stealth = { "provide_coverfire", "provide_support" }
	
	local tactics_FBI_swat_rifle = { "smoke_grenade", "charge", "provide_coverfire", "provide_support", "ranged_fire" }
	local tactics_FBI_swat_shotgun = { "smoke_grenade", "charge", "provide_coverfire", "provide_support" }
	local tactics_FBI_heavy = { "smoke_grenade", "charge", "provide_coverfire", "provide_support", "shield_cover" }
	local tactics_FBI_shield = { "smoke_grenade", "charge", "provide_coverfire", "provide_support", "shield" }
	
	local tactics_FBI_swat_rifle_flank = { "flank", "smoke_grenade", "flash_grenade", "charge", "provide_coverfire", "provide_support" }
	local tactics_FBI_swat_shotgun_flank = { "flank", "smoke_grenade", "flash_grenade", "charge", "provide_coverfire", "provide_support" }
	local tactics_FBI_heavy_flank = { "flank", "smoke_grenade", "flash_grenade", "charge", "provide_coverfire", "provide_support", "shield_cover" }
	local tactics_FBI_shield_flank = { "flank", "smoke_grenade", "flash_grenade", "charge", "provide_coverfire", "provide_support", "shield" }
	
	local tactics_FBI_spooc = { "flank", "smoke_grenade", "flash_grenade", "charge", "provide_coverfire" }
	local tactics_FBI_tank = { "charge", "provide_coverfire", "provide_support" }
	
-- CS (COPS/SWAT)
	self.enemy_spawn_groups.CS_defend_a = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_cop_C45_R870", freq = 1, tactics = tactics_CS_cop, rank = 1 }
		}
	}
	self.enemy_spawn_groups.CS_defend_b = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_swat_MP5", freq = 1, amount_min = 1, tactics = tactics_CS_cop, rank = 1 }
		}
	}
	self.enemy_spawn_groups.CS_defend_c = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_heavy_M4", freq = 1, amount_min = 1, tactics = tactics_CS_cop, rank = 1 }
		}
	}
	
	-- Cops
	self.enemy_spawn_groups.CS_cops = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_cop_C45_R870", freq = 1, amount_min = 1, tactics = tactics_CS_cop, rank = 1 }
		}
	}
	self.enemy_spawn_groups.CS_stealth_a = {
		amount = { 2, 3 },
		spawn = {
			{ unit = "CS_cop_stealth_MP5", freq = 1, amount_min = 1, tactics = tactics_CS_cop_stealth, rank = 1 }
		}
	}
	-- CS Swats
	self.enemy_spawn_groups.CS_swats = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_swat_MP5", freq = 1, tactics = tactics_CS_swat_rifle, rank = 2 },
			{ unit = "CS_swat_R870", freq = 0.4, amount_max = 2, tactics = tactics_CS_swat_shotgun, rank = 1 },
			{ unit = "CS_swat_MP5", freq = 0.2, tactics = tactics_CS_swat_rifle_flank, rank = 3 },
		}
	}
	
	-- CS Heavys
	self.enemy_spawn_groups.CS_heavys = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_heavy_M4", freq = 1, tactics = tactics_CS_swat_rifle, rank = 1 },
			{ unit = "CS_heavy_M4", freq = 0.35, tactics = tactics_CS_swat_rifle_flank, rank = 2 },
		}
	}
	
	-- CS Shields
	self.enemy_spawn_groups.CS_shields = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "CS_shield", freq = 1, amount_min = 1, amount_max = 1, tactics = tactics_CS_shield, rank = 3 },
			{ unit = "CS_shield", freq = 0.1, amount_max = 1, tactics = tactics_CS_shield, rank = 2 },
			{ unit = "CS_heavy_M4_w", freq = 1, amount_min = 1, tactics = tactics_CS_swat_heavy, rank = 1 }
		}
	}
	
	-- CS Tazers
	self.enemy_spawn_groups.CS_tazers = {
		amount = { 2, 2 },
		spawn = {
			{ unit = "CS_tazer", freq = 1, amount_min = 2, amount_max = 2, tactics = tactics_CS_tazer, rank = 1 }
			-- { unit = "CS_heavy_M4", freq = 1, tactics = tactics_CS_tazer, rank = 2 }
		}
	}
	-- CS Tanks
	self.enemy_spawn_groups.CS_tanks = {
		amount = { 1, 1 },
		spawn = {
			{ unit = "FBI_tank", freq = 1, tactics = tactics_FBI_tank, rank = 1 }
		}
	}
	
-- FBI
	-- FBI Defend
	self.enemy_spawn_groups.FBI_defend_a = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_suit_C45_M4", freq = 1, amount_min = 1, tactics = tactics_FBI_suit, rank = 2 },
			{ unit = "CS_cop_C45_R870", freq = 1, tactics = tactics_FBI_suit, rank = 1 }
		}
	}
	self.enemy_spawn_groups.FBI_defend_b = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_suit_M4_MP5", freq = 1, amount_min = 1, tactics = tactics_FBI_suit, rank = 2 },
			{ unit = "FBI_swat_M4", freq = 1, tactics = tactics_FBI_suit, rank = 1 }
		}
	}
	self.enemy_spawn_groups.FBI_defend_c = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_swat_M4", freq = 1, tactics = tactics_FBI_suit, rank = 1 }
		}
	}
	self.enemy_spawn_groups.FBI_defend_d = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_heavy_G36", freq = 1, tactics = tactics_FBI_suit, rank = 1 }
		}
	}
	
	-- FBI Stealth
	self.enemy_spawn_groups.FBI_stealth_a = {
		amount = { 2, 3 },
		spawn = {
			{ unit = "FBI_suit_stealth_MP5", freq = 1, amount_min = 1, tactics = tactics_FBI_suit_stealth, rank = 1 }
		}
	}
	self.enemy_spawn_groups.FBI_stealth_b = {
		amount = { 2, 4 },
		spawn = {
			{ unit = "FBI_suit_stealth_MP5", freq = 1, amount_min = 1, tactics = tactics_FBI_suit_stealth, rank = 1 }
			-- { unit = "FBI_spooc", freq = 1, amount_min = 1, tactics = tactics_FBI_suit_stealth, rank = 1 }
		}
	}
	
	-- FBI Swats
	self.enemy_spawn_groups.FBI_swats = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_swat_M4", freq = 1, amount_min = 1, tactics = tactics_FBI_swat_rifle, rank = 2 },
			{ unit = "FBI_swat_M4", freq = 0.75, tactics = tactics_FBI_swat_rifle_flank, rank = 3 },
			{ unit = "FBI_swat_R870", freq = 0.5, amount_max = 2, tactics = tactics_FBI_swat_shotgun, rank = 1 }
		}
	}
	
	-- FBI Heavy
	self.enemy_spawn_groups.FBI_heavys = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_heavy_G36", freq = 1, tactics = tactics_FBI_swat_rifle, rank = 1 },
			{ unit = "FBI_heavy_G36", freq = 0.5, tactics = tactics_FBI_swat_rifle_flank, rank = 2 }
		}
	}
	
	-- FBI Shields
	self.enemy_spawn_groups.FBI_shields = {
		amount = { 3, 4 },
		spawn = {
			{ unit = "FBI_shield", freq = 0.3, amount_min = 1, amount_max = 2, tactics = tactics_FBI_shield_flank, rank = 3 },
			{ unit = "FBI_heavy_G36_w", freq = 1, amount_min = 1, tactics = tactics_FBI_heavy_flank, rank = 1 }
		}
	}
	
	-- FBI Spoocs
	self.enemy_spawn_groups.FBI_spoocs = {
		amount = { 2, 2 },
		spawn = {
			{ unit = "FBI_suit_stealth_MP5", freq = 1, amount_min = 1, tactics = tactics_FBI_suit_stealth, rank = 1 }
			-- { unit = "FBI_spooc", freq = 1, tactics = tactics_FBI_spooc, rank = 1 }
		}
	}
	
	-- FBI Tanks
	self.enemy_spawn_groups.FBI_tanks = {
		amount = { 1, 2 },
		spawn = {
			{ unit = "FBI_tank", freq = 1, tactics = tactics_FBI_tank, rank = 1 }
		}
	}
end

-----------------------------------------------------------------------------

function GroupAITweakData:_init_task_data()
	local is_console = SystemInfo:platform() ~= Idstring( "WIN32" )
		
	self.max_nr_simultaneous_boss_types = 0 -- TEMP -- how many different types of boss units are allowed to exist simultaneously in a level
	self.difficulty_curve_points = { 0.5 }
	self.optimal_trade_distance = { 0, 0 }
	self.bain_assault_praise_limits = { 1, 3 }
	
	------------ BESIEGE GROUP AI ------------
	self.besiege.regroup.duration = { 15, 15, 15 }				-- maximum duration of regroup phase. Can also expire due to low drama. (sec)
	
	self.besiege.assault.anticipation_duration = { { 30, 1 }, { 30, 1 }, { 45, 0.5 } }							-- Anticipation phase duration and its chance. (sec)
	self.besiege.assault.build_duration = 35					-- Build phase duration. Gets interrupted by drama peak. (sec)
	self.besiege.assault.sustain_duration_min = { 0, 80, 120 } 	-- Sustain phase duration. (sec)
	self.besiege.assault.sustain_duration_max = { 0, 80, 120 } 	-- Sustain phase duration. (sec)
	self.besiege.assault.sustain_duration_balance_mul = { 1, 1.1, 1.2, 1.3 } -- multiplier to sustain phase duration on 1 to 4 players
	self.besiege.assault.fade_duration = 5
	self.besiege.assault.delay = { 80, 70, 30 }  				-- Delay after the end of a regroup task when we are again allowed to assault depending on control level. (sec)
	self.besiege.assault.hostage_hesitation_delay = { 30, 30, 30 }
	
	if is_console then
		self.besiege.assault.force = { 0, 4, 6 }		-- max simultaneous # pigs participating in assault
		self.besiege.assault.force_pool = { 0, 60, 100 }			-- max # pigs that may be spawned during the entire assault phase
	else
		self.besiege.assault.force = { 0, 10, 12 }				-- max simultaneous # pigs participating in assault	
		self.besiege.assault.force_pool = { 0, 60, 100 }			-- max # pigs that may be spawned during the entire assault phase
	end
	
	self.besiege.assault.force_balance_mul = { 1, 1.1, 1.2, 1.3 } -- multiplier to "self.besiege.assault.force" on 1 to 4 players
	self.besiege.assault.force_pool_balance_mul = { 1, 1.1, 1.2, 1.3 } -- multiplier to "self.besiege.assault.force_pool" on 1 to 4 players
	
-- TODO : Create several versions to get a good mix on levels
	-- Normal
	--[[
	self.besiege.assault.groups = {
		CS_swats = { 0, 1, 0 },
		CS_heavys = { 0, 0, 1 },
		CS_shields = { 0, 0, 0.6 },
		CS_tazers = { 0, 0, 0.1 },
		CS_tanks = { 0, 0, 0.025 },
	}
	]]
	
	self.besiege.assault.groups = {
		CS_swats = { 0, 1, 0.7 },
		CS_heavys = { 0, 0, 0.5 },
		CS_shields = { 0, 0, 0.1 },
	}
	
	self.besiege.reenforce.interval = { 10, 20, 30 }		-- (sec)
	self.besiege.reenforce.groups = {
		CS_defend_a = { 1, 0.2, 0 },
		CS_defend_b = { 0, 1, 1 },
	}
	
	self.besiege.recon.interval = { 5, 5, 5 }				-- (sec)
	self.besiege.recon.force = { 2, 4, 6 }					-- max # pigs
	self.besiege.recon.interval_variation = 40 				-- random adjustment to the recon_interval. ex 0.5 means: +0% to +50% adjustment	
	self.besiege.recon.groups = {
		CS_stealth_a = { 1, 1, 0 },
		CS_swats = { 0, 1, 1 },
	}
	
-- TODO : reinforce intervals for the different modes
	
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//> GroupAI Data Initialized <\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")

end

function GroupAITweakData:_set_easy()
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//> Difficulty set to : Easy <\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	
end

function GroupAITweakData:_set_normal()
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------\\//> Difficulty set to : Normal <//\\-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	
	-- Normal
end

function GroupAITweakData:_set_hard()
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//> Difficulty set to : Hard <\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	
	-- Swat reinforcement
	self.besiege.assault.groups = {
		CS_swats = { 0, 1, 0 },
		CS_heavys = { 0, 0.2, 1 },
		CS_shields = { 0, 0.02, 1 },
		CS_tazers = { 0, 0.01, 0.2 },
		CS_tanks = { 0, 0.01, 0.1 },
		-- FBI_spoocs = { 0, 0, 0.05 },
	}
	
	self.besiege.reenforce.interval = { 10, 20, 30 }		-- (sec)
	self.besiege.reenforce.groups = {
		CS_defend_a = { 1, 0, 0 },
		CS_defend_b = { 2, 1, 0 },
		CS_defend_c = { 0, 0, 1 },
	}
	
	self.besiege.assault.delay = { 45, 35, 20 }
	
	self.besiege.recon.interval = { 5, 5, 5 }				-- (sec)
	self.besiege.recon.force = { 2, 4, 6 }					-- max # pigs
	self.besiege.recon.interval_variation = 40 				-- random adjustment to the recon_interval. ex 0.5 means: +0% to +50% adjustment	
	self.besiege.recon.groups = {
		CS_stealth_a = { 1, 0, 0 },
		CS_swats = { 0, 1, 1 },
		CS_tazers = { 0, 0, 0.2 },
		FBI_stealth_b = { 0, 0, 0.1 },
	}
	
	self.besiege.assault.force_balance_mul = { 1.5, 1.6, 1.7, 1.8 } -- multiplier to "self.besiege.assault.force" on 1 to 4 players
	-- self.besiege.assault.force_balance_mul = { 1.2, 1.4, 1.6, 1.8 } -- multiplier to "self.besiege.assault.force" on 1 to 4 players
	self.besiege.assault.force_pool_balance_mul = { 1.2, 1.4, 1.6, 1.8 } -- multiplier to "self.besiege.assault.force_pool" on 1 to 4 players
end

function GroupAITweakData:_set_overkill()
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//> Difficulty set to : Overkill <\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	
	-- FBI responds
	self.besiege.assault.groups = {
		FBI_swats = { 0, 1, 0.25 },
		FBI_heavys = { 0, 0.2, 1 },
		FBI_shields = { 0, 0.2, 1 },
		FBI_tanks = { 0, 0.01, 0.05 },
		-- FBI_spooc = { 0, 0.1, 0.3 },
		CS_tazers = { 0, 0.1, 0.2 },
	}
	self.besiege.reenforce.interval = { 10, 20, 30 }		-- (sec)
	self.besiege.reenforce.groups = {
		CS_defend_a = { 1, 0, 0 },
		CS_defend_b = { 2, 1, 0 },
		CS_defend_c = { 0, 0, 1 },
		FBI_defend_a = { 0, 1, 0 },
		FBI_defend_b = { 0, 0, 1 },
	}
	
	self.besiege.assault.delay = { 40, 30, 20 }
	
	self.besiege.recon.interval = { 5, 5, 5 }				-- (sec)
	self.besiege.recon.force = { 2, 4, 6 }					-- max # pigs
	self.besiege.recon.interval_variation = 40 				-- random adjustment to the recon_interval. ex 0.5 means: +0% to +50% adjustment	
	self.besiege.recon.groups = {
		FBI_stealth_a = { 1, 1, 0 },
		FBI_stealth_b = { 0, 0, 1 },
	}
	
	self.besiege.assault.force_balance_mul = { 1.8, 1.9, 2.0, 2.1 } -- multiplier to "self.besiege.assault.force" on 1 to 4 players
	self.besiege.assault.force_pool_balance_mul = { 1.6, 1.8, 2, 2.2 } -- multiplier to "self.besiege.assault.force_pool" on 1 to 4 players
end

function GroupAITweakData:_set_overkill_145()
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------//\\> Difficulty set to : Overkill_145 <//\\-------------")
	print("-------------\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//-------------")
	print("-------------//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\//\\-------------")
	
	-- FBI Case
	self.besiege.assault.groups = {
		FBI_swats = { 0, 1, 0 },
		FBI_heavys = { 0, 1, 1 },
		FBI_shields = { 0, 0.5, 1 },
		FBI_tanks = { 0, 0.1, 0.1 },
		-- FBI_spooc = { 0, 0.2, 0.4 },
		CS_tazers = { 0, 0.1, 0.2 },
	}
	self.besiege.reenforce.interval = { 10, 20, 30 }		-- (sec)
	self.besiege.reenforce.groups = {
		CS_defend_a = { 0.1, 0, 0 },
		FBI_defend_b = { 1, 1, 0 },
		FBI_defend_c = { 0, 1, 0 },
		FBI_defend_d = { 0, 0, 1 },
	}
	
	self.besiege.assault.delay = { 30, 20, 15 }
	
	self.besiege.recon.interval = { 5, 5, 5 }				-- (sec)
	self.besiege.recon.force = { 2, 4, 6 }					-- max # pigs
	self.besiege.recon.interval_variation = 40 				-- random adjustment to the recon_interval. ex 0.5 means: +0% to +50% adjustment	
	self.besiege.recon.groups = {
		FBI_stealth_a = { 1, 0.5, 0 },
		FBI_stealth_b = { 0, 1, 1 },
	}
	
	self.besiege.assault.force_balance_mul = { 1.8, 1.9, 2.0, 2.1 } -- multiplier to "self.besiege.assault.force" on 1 to 4 players
	self.besiege.assault.force_pool_balance_mul = { 2, 2.2, 2.4, 2.6 } -- multiplier to "self.besiege.assault.force_pool" on 1 to 4 players
end

-----------------------------------------------------------------------------
-- ************* NOTHING TO TWEAK BELOW THIS LINE *******************
-----------------------------------------------------------------------------















function GroupAITweakData:_read_mission_preset( tweak_data )
	if not Global.game_settings then
		return
	end
	local lvl_tweak_data = tweak_data.levels[ Global.game_settings.level_id ]
	self._mission_preset = lvl_tweak_data.group_ai_preset
end

-----------------------------------------------------------------------------------

function GroupAITweakData:_create_table_structure()
	self.enemy_chatter = {}
	self.enemy_spawn_groups = {}
	
	self.besiege = {
		regroup = {},
		assault = {
			force = {}
		},
		reenforce = {},
		recon = {},
		rescue = {}
	}
	
	self.street = {
		blockade = {
			force={}
		},
		assault = {
			force = {}
		},
		regroup = {},
		capture = {
			force = {}
		}
		--,assault = {}
		--,reenforce = {}
		--,recon = {}
	}
end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------