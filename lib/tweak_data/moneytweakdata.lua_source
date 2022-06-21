MoneyTweakData = MoneyTweakData or class()

function MoneyTweakData._create_value_table(min, max, table_size, round, curve ) -- return table linear
	local t = {}
	for i = 1, table_size do
		local v = math.lerp( min, max, math.pow((i-1)/(table_size-1), curve and curve or 1) )
		
		if(v>999) then
			v = v*0.001
			v = round and math.ceil( v ) or v
			v = v*1000
		elseif(v>99) then
			v = v*0.01
			v = round and math.ceil( v ) or v
			v = v*100
		elseif(v>9) then
			v = v*0.1
			v = round and math.ceil( v ) or v
			v = v*10
		else
			v = round and math.ceil( v ) or v
		end
		
		table.insert( t, v ) 
	end
	return t
end

-- DEBUG
function MoneyTweakData._test_curves( pay, bags, alive_players, diff, days)
	local v;
	local loot_bags = tweak_data.money_manager.bag_value_multiplier[pay] * tweak_data.money_manager.bag_values.default
	
	local diff_multiplier = 0
	if(diff>0) then
		diff_multiplier = tweak_data.money_manager.difficulty_multiplier[diff]
	end
	
	v = tweak_data.money_manager.stage_completion[pay] + tweak_data.money_manager.job_completion[pay] + loot_bags * bags
	v = v * days
	v = v + v * diff_multiplier
	v = v * tweak_data.money_manager.alive_humans_multiplier[alive_players]
	
	print(v, v * tweak_data.money_manager.offshore_rate)
end

function MoneyTweakData:init()
-- Gobals
	-- 5040000
	self.biggest_score = 5040000 -- 3600000 * 1.4 -- Thematic cash. Scaled up becasue of fourstores level small loot bug. Used to be 2.000.000, could be alot smaller for better money respect
	self.biggest_cashout = 250000 -- Cash -- Tweaked with 250.000
	self.offshore_rate = self.biggest_cashout / self.biggest_score
	-- 20.16 original relationship
	-- 16.8
	
	self.alive_players_max = 1.3
	self.cashout_without_player_alive = self.biggest_cashout / self.alive_players_max
	
	self.cut_difficulty = 8
	self.max_mission_bags = 6
	self.cut_lootbag_bonus = ((self.cashout_without_player_alive * 0.3))
	self.cut_lootbag_bonus = (self.cut_lootbag_bonus / self.max_mission_bags) / self.cut_difficulty
	
	self.max_days = 3
	self.cut_stage_complete = ((self.cashout_without_player_alive * 0.55))
	self.cut_stage_complete = (self.cut_stage_complete / self.cut_difficulty) * 0.7
	
	self.cut_job_complete = ((self.cashout_without_player_alive * 0.15))
	self.cut_job_complete = self.cut_job_complete / self.cut_difficulty
	
	self.bag_values = {} -- For sake of syncronization, these values will be multiplied with bag_value_multiplier
	self.bag_values.default = 150 -- Is this coke? where is coke?!
	self.bag_values.money = 450
	self.bag_values.gold = 600
	self.bag_values.diamonds = 125 -- To compensate for the fast smash and grabs
	self.bag_values.coke = 500
	self.bag_values.meth = 600
	self.bag_values.weapons = 700
	
	-- Lootbags
	self.bag_value_multiplier = self._create_value_table( ((self.cut_lootbag_bonus)/5)/self.offshore_rate/self.bag_values.default, (self.cut_lootbag_bonus)/self.offshore_rate/self.bag_values.default, 7, true, 0.85 ) -- Scaled with stars
	
	-- Money from completed job based on stars
	self.stage_completion = self._create_value_table( (self.cut_stage_complete/7)/self.offshore_rate, (self.cut_stage_complete/self.offshore_rate), 7, true, 1 ) -- { 2500, 5000, 7500, 10000, 12500, 15000, 20000, 30000, 40000, 50000 }
	self.job_completion = self._create_value_table( (self.cut_job_complete/7)/self.offshore_rate, self.cut_job_complete/self.offshore_rate, 7, true, 1 ) -- { 10000, 20000, 30000, 50000, 75000, 150000, 200000, 300000, 500000, 1000000 }
	
	self.level_limit = {}
	self.level_limit.low_cap_level = -1				-- Players <= this level will gain additional money from his level, using .low_cap_multiplier 
	self.level_limit.low_cap_multiplier = 0.75
	self.level_limit.pc_difference_multipliers = { 1, 0.9, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3, 0.2, 0.1 }	-- players above low_cap_level will gain money of the difference between his level and job level, from the different of pc
	
	self.stage_failed_multiplier = 0.1
	self.difficulty_multiplier = self._create_value_table( 2.5, self.cut_difficulty, 3, false, 1 ) -- This is an addition to the job/stage money( stage_money+stage_money*mul ), hard, overkill, overkill_146
	self.small_loot_difficulty_multiplier = self._create_value_table( 0, 0, 3, false, 1 )
	self.alive_humans_multiplier = self._create_value_table( 1, self.alive_players_max, 4, false, 1 )
	
	-- Selling value multipliers (a multiplier used to calculate money recieved when selling something)
	self.sell_weapon_multiplier = 0.25
	self.sell_mask_multiplier = 0.25
	
	self.killing_civilian_deduction = self._create_value_table( 2000, 20000, 10, true, 2 )
	
	
	

	self.global_value_multipliers = {}
	self.global_value_multipliers.normal = 1
	self.global_value_multipliers.superior = 1
	self.global_value_multipliers.exceptional = 1
	self.global_value_multipliers.infamous = 5
	self.global_value_multipliers.preorder = 1 / 1
	self.global_value_multipliers.overkill = 0.01
	
	self.global_value_bonus_multiplier = {}									-- for every extra of the same global value, the sell price is increased with this ( multiplier of value )
	self.global_value_bonus_multiplier.normal = 0						-- example. an infamous mask with infamous texture = 2 normal, 2 infamous = 1 bonus normal + 1 bonus infamous = sell value increased with 3 times its value ( 1 * 0 + 1 * 3 )
	self.global_value_bonus_multiplier.superior = 0.1				-- example 2. an infamous mask with all three parts infamous = 0 normal, 4 infamous = 0 bonus normal + 3 bonus infamous = sell value increased with 9 times its value ( 0 * 0 + 3 * 3 )
	self.global_value_bonus_multiplier.exceptional = 0.2		-- example 3. an infamous mask with all three parts normal = 3 normal, 1 infamous = 3 bonus normal + 0 bonus infamous = sell value increased with 0 times its value ( 3 * 0 + 0 * 3 )
	self.global_value_bonus_multiplier.infamous = 1
	self.global_value_bonus_multiplier.preorder = 0
	self.global_value_bonus_multiplier.overkill = 20
	
	
	local smallest_cashout = (self.stage_completion[1] + self.job_completion[1]) * self.offshore_rate
	
-- MASK COSTS
	local biggest_mask_cost = self.biggest_cashout * 100
	local biggest_mask_cost_deinfamous = math.round(biggest_mask_cost / self.global_value_multipliers.infamous)
	local biggest_mask_part_cost = math.round(smallest_cashout * 46)
	local smallest_mask_part_cost = math.round(smallest_cashout * 3.9) -- To put preoerder mask just below 25.000 on the crafting
	
-- WEAPON AND MOD COSTS
	local biggest_weapon_cost = math.round(self.biggest_cashout * 2.5)
	local smallest_weapon_cost = math.round(smallest_cashout * 6)
	
	local biggest_weapon_mod_cost = math.round(self.biggest_cashout * 1.3)
	local smallest_weapon_mod_cost = math.round(smallest_cashout * 7)
	
-- WEAPON TABLES
	self.weapon_cost = self._create_value_table( smallest_weapon_cost, biggest_weapon_cost, 40, true, 1.1 )
	self.modify_weapon_cost = self._create_value_table( smallest_weapon_mod_cost, biggest_weapon_mod_cost, 10, true, 1.2 )
	self.remove_weapon_mod_cost_multiplier = self._create_value_table( 1, 1, 10, true, 1 )
	
-- MASK TABLES
	self.masks = {} -- + material + pattern + color values
	self.masks.mask_value = self._create_value_table( smallest_mask_part_cost, smallest_mask_part_cost * 3, 10, true, 2 )
	self.masks.material_value = self._create_value_table( smallest_mask_part_cost * 0.9, biggest_mask_part_cost, 10, true, 1.2 )
	self.masks.pattern_value = self._create_value_table( smallest_mask_part_cost * 0.8, biggest_mask_part_cost, 10, true, 1.1 )
	self.masks.color_value = self._create_value_table( smallest_mask_part_cost * 0.7, biggest_mask_part_cost, 10, true, 1 )
	
	
	
-- ASSETS
	self.mission_asset_cost_by_pc = self._create_value_table( 1, 1, 10, true, 1 )
	self.mission_asset_cost_multiplier_by_pc = { 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0 }
	self.mission_asset_cost_multiplier_by_risk = { 0.5, 1.0, 2.0 }	
	self.mission_asset_cost_small = self._create_value_table( 2500, 15000, 10, true, 1 )
	self.mission_asset_cost_medium = self._create_value_table( 10000, 45000, 10, true, 1 )
	self.mission_asset_cost_large = self._create_value_table( 55000, 400000, 10, true, 1 )
	
-- SMALL LOOT
	-- self.small_loot_value_multiplier = self._create_value_table( (2.5/self.offshore_rate) * 1.66 , 5/self.offshore_rate, 10, true, 1.7 ) -- Scaled with stars
	self.small_loot_value_multiplier = { 100, 100, 100, 100, 100, 100, 100, 100, 100, 100 } -- Scaled with stars
	
-- Value is multiplied with self.small_loot_value_multiplier DONT HAVE IT BELOW 1!
	self.small_loot 							= {} 
	self.small_loot.money_bundle			 	= 10
	self.small_loot.diamondheist_vault_bust 	= 12 -- 50
	self.small_loot.diamondheist_vault_diamond 	= 15 -- 30
	self.small_loot.diamondheist_big_diamond 	= 15 -- 100
	self.small_loot.value_gold 					= 30 -- 400
	self.small_loot.gen_atm 					= 220 -- 1000
	self.small_loot.special_deposit_box			= 35
	
-- Bank deposit specials
	self.small_loot.vault_loot_gold				= 100
	self.small_loot.vault_loot_cash				= 50
	self.small_loot.vault_loot_coins			= 35
	self.small_loot.vault_loot_ring				= 15
	self.small_loot.vault_loot_jewels			= 25
	self.small_loot.vault_loot_macka			= 0.01
	
	self.skilltree = {}
	
-- RESPEC NOT USED CURRENTLY
	self.skilltree.respec = {}
	self.skilltree.respec.base_cost = 200		-- the base cost for respeccing
	self.skilltree.respec.profile_cost_increaser_multiplier = { 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 }		-- each time this profile respec, next respec is increase by this multiplier, up to max in table
	self.skilltree.respec.tier_cost = {}			-- the respeccing cost for each tier
	self.skilltree.respec.tier_cost[1] = 1500
	self.skilltree.respec.tier_cost[2] = 2000
	self.skilltree.respec.tier_cost[3] = 10000
	self.skilltree.respec.tier_cost[4] = 40000
	self.skilltree.respec.tier_cost[5] = 100000
	self.skilltree.respec.tier_cost[6] = 400000
	
-- Skillpoint costs
	self.skilltree.respec.base_point_cost = 500				-- base cost for spending a point
	self.skilltree.respec.point_tier_cost = self._create_value_table( 4000, self.biggest_cashout*0.45, 6, true, 1.1 )
	
	self.skilltree.respec.respec_refund_multiplier = 0.5
	
	self.skilltree.respec.point_cost = 0			-- the respeccing cost for each point
	self.skilltree.respec.point_multiplier_cost = 1.0 -- point cost 'raised to' with this ( points * point_cost )^point_multiplier_cost, this increases the point cost for each point placed
		-- macro for calulating point cost (127 is the current maximum of points that can be spent in one tree)
		-- s for i=1, 127 do print( "POINT " .. i .. ": " .. ((tweak_data.money_manager.skilltree.respec.point_cost*i)^tweak_data.money_manager.skilltree.respec.point_multiplier_cost)-((tweak_data.money_manager.skilltree.respec.point_cost*(i-1))^tweak_data.money_manager.skilltree.respec.point_multiplier_cost) ) end
	
	local loot_drop_value = 1500 -- / self.offshore_rate -- Offshore space
	self.loot_drop_cash = {}
	self.loot_drop_cash.cash10 = loot_drop_value * 1
	self.loot_drop_cash.cash20 = loot_drop_value * 2
	self.loot_drop_cash.cash30 = loot_drop_value * 3
	self.loot_drop_cash.cash40 = loot_drop_value * 4
	self.loot_drop_cash.cash50 = loot_drop_value * 5
	self.loot_drop_cash.cash60 = loot_drop_value * 6
	self.loot_drop_cash.cash70 = loot_drop_value * 8
	self.loot_drop_cash.cash80 = loot_drop_value * 12
	self.loot_drop_cash.cash90 = loot_drop_value * 24
	self.loot_drop_cash.cash100 = loot_drop_value * 48
	
	self.loot_drop_cash.cash_preorder = self.biggest_cashout / 10
end

