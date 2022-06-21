MoneyManager = MoneyManager or class()
--[[ MoneyManager.PATH = "gamedata/hints"
MoneyManager.FILE_EXTENSION = "hint"
MoneyManager.FULL_PATH = MoneyManager.PATH .. "." .. MoneyManager.FILE_EXTENSION ]]

function MoneyManager:init()
	self:_setup()
end

function MoneyManager:_setup()
	if not Global.money_manager then
		Global.money_manager = {}
		Global.money_manager.total = 0 -- Total spendable amount
		Global.money_manager.total_collected = 0 -- Total aquired without any costs or deductions
		Global.money_manager.offshore = 0
		Global.money_manager.total_spent = 0 
	end
	self._global = Global.money_manager
	self._heist_total = 0
	self._heist_spending = 0
	self._heist_offshore = 0
	self._active_multipliers = {}
	
	self._stage_payout = 0
	self._job_payout = 0
	self._bag_payout = 0
	self._small_loot_payout = 0
	self._crew_payout = 0
	
	self._cash_tousand_separator = managers.localization:text( "cash_tousand_separator" )
	self._cash_sign = managers.localization:text( "cash_sign" )
end

function MoneyManager:total_string_no_currency()
	local total = math.round( self._global.total )
	total = tostring( total )
	local reverse = string.reverse( total )
	local s = ""
	for i = 1, string.len( reverse ) do
		s = s..string.sub( reverse, i, i )..((math.mod( i, 3 ) == 0 and (i ~= string.len( reverse )) and self._cash_tousand_separator) or "")
	end
	return string.reverse( s )
end

function MoneyManager:total_string()
	local total = math.round( self._global.total )
	total = tostring( total )
	local reverse = string.reverse( total )
	local s = ""
	for i = 1, string.len( reverse ) do
		s = s..string.sub( reverse, i, i )..((math.mod( i, 3 ) == 0 and (i ~= string.len( reverse )) and self._cash_tousand_separator) or "")
	end
	return self._cash_sign..string.reverse( s )
end

function MoneyManager:total_collected_string_no_currency()
	local total = math.round( self._global.total_collected )
	total = tostring( total )
	local reverse = string.reverse( total )
	local s = ""
	for i = 1, string.len( reverse ) do
		s = s..string.sub( reverse, i, i )..((math.mod( i, 3 ) == 0 and (i ~= string.len( reverse )) and self._cash_tousand_separator) or "")
	end
	return string.reverse( s )
end

function MoneyManager:total_collected_string()
	local total = math.round( self._global.total_collected )
	total = tostring( total )
	local reverse = string.reverse( total )
	local s = ""
	for i = 1, string.len( reverse ) do
		s = s..string.sub( reverse, i, i )..((math.mod( i, 3 ) == 0 and (i ~= string.len( reverse )) and self._cash_tousand_separator) or "")
	end
	return self._cash_sign..string.reverse( s )
end

function MoneyManager:add_decimal_marks_to_string( string )
	local total = string
	local reverse = string.reverse( total )
	local s = ""
	for i = 1, string.len( reverse ) do
		s = s..string.sub( reverse, i, i )..((math.mod( i, 3 ) == 0 and (i ~= string.len( reverse )) and self._cash_tousand_separator) or "")
	end
	return string.reverse( s )
end

function MoneyManager:use_multiplier( multiplier )
	if not tweak_data.money_manager.multipliers[ multiplier ] then
		Application:error( "Unknown multiplier \"" .. tostring( multiplier ) .. " in money manager." )
		return
	end
	
	self._active_multipliers[ multiplier ] = tweak_data.money_manager.multipliers[ multiplier ]
end

function MoneyManager:remove_multiplier( multiplier )
	if not tweak_data.money_manager.multipliers[ multiplier ] then
		Application:error( "Unknown multiplier \"" .. tostring( multiplier ) .. " in money manager." )
		return
	end
	
	self._active_multipliers[ multiplier ] = nil
end

function MoneyManager:perform_action( action )
	if true then
		return
	end
	--[[if not tweak_data.money_manager.actions[ action ] then
		Application:error( "Unknown action \"" .. tostring( action ) .. " in money manager." )
		return
	end
	
	self:_add( tweak_data.money_manager.actions[ action ] )]]
end

function MoneyManager:perform_action_interact( name )
	if true then
		return
	end
	--[[if not tweak_data.money_manager.actions.interact[ name:key() ] then
		return
	end
	
	self:_add( tweak_data.money_manager.actions.interact[ name:key() ] )]]
end

function MoneyManager:perform_action_money_wrap( amount )
	self:_add( amount )
end

------------------------------------------------------------------
function MoneyManager:get_civilian_deduction()
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	return math.round( tweak_data.money_manager.killing_civilian_deduction[ job_and_difficulty_stars ] )
end

function MoneyManager:civilian_killed()
	local deduct_amount = self:get_civilian_deduction()
	
	-- local text = "Deducting "..managers.experience:cash_string( deduct_amount ).." from your account in fencing costs." -- managers.localization:text( equipment.text_id )	
	local text = managers.localization:text( "hud_civilian_killed_message", { AMOUNT = managers.experience:cash_string( deduct_amount ) } )
	local title = managers.localization:text( "hud_civilian_killed_title" )
	managers.hud:present_mid_text( { text = text, title = title, time = 4 } )
	
	self:_deduct_from_total( deduct_amount )
end

------------------------------------------------------------------

-- ( (stage_white_stars + ( stage_white_stars * diff_multiplier )) + (job_white_stars + ( job_white_stars * diff_multiplier )) + (bonus_bags_value + ( bonus_bags_value * diff_multiplier ) ) ) * alive_players + ( small_loot )

-- simplified
-- ( (stage_white_stars + job_white_stars + bonus_bags_value) * (diff_multiplier+1) ) * alive_players + ( small_loot_value * diff_multiplier )

--[[
function MoneyManager.mon( sws, jws, bbv, dm, ap, sl )
	return ( ( sws + ( sws * dm ) ) + ( jws + ( jws * dm ) ) + ( bbv + ( bbv * dm ) ) ) * ap + sl
	,( ( sws + jws + bbv ) * (dm+1) ) * ap + sl
end]]

function MoneyManager:on_mission_completed( num_winners )
	if managers.job:interupt_stage() then -- Skip money if escape level
		return
	end
	
	local stage_value, job_value, bag_value, small_value, crew_value, total_payout = self:get_real_job_money_values( num_winners )
	
	self:_set_stage_payout( stage_value )
	self:_set_job_payout( job_value )
	self:_set_bag_payout( bag_value )
	self:_set_small_loot_payout( small_value )
	self:_set_crew_payout( crew_value )
	
	self:_add_to_total( total_payout )
end

function MoneyManager:get_real_job_money_values( num_winners )
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	local player_stars = managers.experience:level_to_stars()
	local is_level_limited = player_stars < job_and_difficulty_stars
	
	local total_stars = math.min( job_and_difficulty_stars, player_stars )
	local total_difficulty_stars = math.max( 0, total_stars - job_stars )
	local money_multiplier = managers.money:get_contract_difficulty_multiplier( total_difficulty_stars ) or 0
	local small_loot_multiplier = managers.money:get_small_loot_difficulty_multiplier( total_difficulty_stars ) or 0
	
	total_stars = math.min( job_stars, total_stars )	-- 1-7
	
	local secured_bags = managers.loot:get_secured_bonus_bags_value()
	local real_small_value = math.round( managers.loot:get_real_total_small_loot_value() )
	
	local stage_value = self:get_stage_payout_by_stars( total_stars ) or 0	-- stage
	local job_value = 0
	local bag_value = 0
	local crew_value = 0
	local small_value = real_small_value
	
	if managers.job:on_last_stage() then
		job_value = self:get_job_payout_by_stars( total_stars ) or 0	-- job
		bag_value = secured_bags * tweak_data.money_manager.bag_value_multiplier[ total_stars ]	-- bag
	end
	
	stage_value = math.round( stage_value + ( stage_value * money_multiplier ) )
	job_value = math.round( job_value + ( job_value * money_multiplier ) )
	bag_value =math.round(  bag_value + ( bag_value * money_multiplier ) )
	small_value = math.round( small_value + ( small_value * small_loot_multiplier ) )
	
	local total_payout = math.round( stage_value + job_value + bag_value + small_value )	-- adding it up
	-- total_payout = math.round( total_payout + ( total_payout * money_multiplier ) )	-- add multiply by risk
	
	
	-- total_payout = total_payout + small_value
	
	if is_level_limited then
		local level_limit_tweak_data = tweak_data.money_manager.level_limit
		local player_pc_payout = total_payout
		local potential_pc_payout = 0
		
		
		-- calculate potential max payout for job
		local money_multiplier = managers.money:get_contract_difficulty_multiplier( difficulty_stars )
		local small_loot_multiplier = managers.money:get_small_loot_difficulty_multiplier( difficulty_stars )
		local new_stage_value = self:get_stage_payout_by_stars( job_stars ) or 0	-- stage
		local new_job_value = 0
		local new_bag_value = 0
		local new_small_value = real_small_value
		
		if managers.job:on_last_stage() then
			new_job_value = self:get_job_payout_by_stars( job_stars ) or 0	-- job
			new_bag_value = secured_bags * tweak_data.money_manager.bag_value_multiplier[ job_stars ]	-- bag
		end
		potential_pc_payout = math.round( new_stage_value + new_job_value + new_bag_value )	-- adding it up
		potential_pc_payout = math.round( potential_pc_payout + ( potential_pc_payout * money_multiplier ) )	-- add multiply by risk
		potential_pc_payout = potential_pc_payout + ( new_small_value + ( new_small_value * small_loot_multiplier ) )
		
		if managers.experience:current_level() <= level_limit_tweak_data.low_cap_level then
			player_pc_payout = player_pc_payout + player_pc_payout * level_limit_tweak_data.low_cap_multiplier
			player_pc_payout = math.round( math.min( player_pc_payout, potential_pc_payout ) )
			
			
			stage_value = stage_value + stage_value * level_limit_tweak_data.low_cap_multiplier
			stage_value = math.round( math.min( stage_value, new_stage_value ) )
			
			job_value = job_value + job_value * level_limit_tweak_data.low_cap_multiplier
			job_value = math.round( math.min( job_value, new_job_value ) )
			
			bag_value = bag_value + bag_value * level_limit_tweak_data.low_cap_multiplier
			bag_value = math.round( math.min( bag_value, new_bag_value ) )
			
			small_value = small_value + small_value * level_limit_tweak_data.low_cap_multiplier
			small_value = math.round( math.min( small_value, new_small_value ) )
		else
			local diff_in_money = potential_pc_payout - player_pc_payout
			local diff_in_stars = job_and_difficulty_stars - player_stars
			local tweak_multiplier = level_limit_tweak_data.pc_difference_multipliers[diff_in_stars] or 0
			
			player_pc_payout = player_pc_payout + diff_in_money * tweak_multiplier
			player_pc_payout = math.round( math.min( player_pc_payout, potential_pc_payout ) )
			
			do
				new_stage_value = new_stage_value + new_stage_value * money_multiplier
				local diff_in_money = new_stage_value - stage_value
				stage_value = stage_value + diff_in_money * tweak_multiplier
				stage_value = math.round( math.min( stage_value, new_stage_value ) )
			end
			
			do
				new_job_value = new_job_value + new_job_value * money_multiplier
				local diff_in_money = new_job_value - job_value
				job_value = job_value + diff_in_money * tweak_multiplier
				job_value = math.round( math.min( job_value, new_job_value ) )
			end
			
			do
				new_bag_value = new_bag_value + new_bag_value * money_multiplier
				local diff_in_money = new_bag_value - bag_value
				bag_value = bag_value + diff_in_money * tweak_multiplier
				bag_value = math.round( math.min( bag_value, new_bag_value ) )
			end
			
			do
				new_small_value = new_small_value + new_small_value * small_loot_multiplier
				local diff_in_money = new_small_value - small_value
				small_value = small_value + diff_in_money * tweak_multiplier
				small_value = math.round( math.min( small_value, new_small_value ) )
			end
			
			local collected_value = stage_value + job_value + bag_value + small_value
			if player_pc_payout ~= collected_value then
				local rounded_error_value = player_pc_payout - collected_value
				small_value = small_value - rounded_error_value
				
				print( "ROUNDING ERROR DETECTED", rounded_error_value )
			end
		end
		
		total_payout = player_pc_payout
	end
	
	total_payout = math.round(total_payout)
	crew_value = total_payout
	total_payout = math.round( total_payout * ( tweak_data.money_manager.alive_humans_multiplier[ num_winners ] or 1 ) )	-- multiply by alive players
	crew_value = math.round( total_payout - crew_value )	-- get alive bonus
	
	-- total_payout = math.round( total_payout + (small_value + small_value * money_multiplier) )	-- add loose cash
	
	return stage_value, job_value, bag_value, small_value, crew_value, total_payout
end

function MoneyManager:on_stage_completed( num_winners )
	if not managers.job:has_active_job() then
		return 0
	end
	
	-- local stars = managers.job:current_job_and_difficulty_stars()
	-- local amount = tweak_data.money_manager.stage_completion[ stars ]
	local stage_value = self:_get_contract_money( false, nil )
	local amount = stage_value
	amount = math.round( amount * tweak_data.money_manager.alive_humans_multiplier[ num_winners ] )
	
	self:_add_to_total( amount )
	self:_set_stage_payout( stage_value )
	self:_set_crew_payout( amount - stage_value )
	return amount
end

function MoneyManager:on_job_completed( num_winners )	-- OLD
	if not managers.job:has_active_job() then
		return 0
	end
	
	-- local stars = managers.job:current_job_and_difficulty_stars()
	-- local amount = tweak_data.money_manager.job_completion[ stars ]
	local stage_value = self:_get_contract_money( false, nil )		-- for day/stage
	local job_value = self:_get_contract_money( true, nil )	-- for contract/job
	local bag_value = self:get_secured_bonus_bags_money() -- managers.loot:get_secured_bonus_bags_amount() * ( tweak_data.money_manager.bag_value * self:get_job_bag_value() )
	
	local amount = stage_value + job_value -- + ( managers.loot:get_secured_bonus_bags_amount() or 0 )
	amount = amount + bag_value
	amount = math.round( amount * tweak_data.money_manager.alive_humans_multiplier[ num_winners ] )
	
	self:_add_to_total( amount )
	
	self:_set_stage_payout( stage_value )
	self:_set_job_payout( job_value )
	self:_set_bag_payout( bag_value )
	self:_set_crew_payout( amount - stage_value - job_value - bag_value )
	
	return amount
end

function MoneyManager:get_secured_bonus_bags_money()
--[[
	local stage_value, job_value, bag_value, small_value, crew_value, total_payout = self:get_real_job_money_values( 0 )
	return bag_value
	]]
	
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	local player_stars = managers.experience:level_to_stars()
	local is_level_limited = player_stars < job_and_difficulty_stars
	
	local total_stars = job_and_difficulty_stars --  math.min( job_and_difficulty_stars, player_stars )
	local total_difficulty_stars = math.max( 0, total_stars - job_stars )
	local money_multiplier = managers.money:get_contract_difficulty_multiplier( total_difficulty_stars ) or 0
	
	total_stars = math.min( job_stars, total_stars )	-- 1-7
	
	local secured_bags = managers.loot:get_secured_bonus_bags_value()
	local bag_value = 0
	
	bag_value = secured_bags * tweak_data.money_manager.bag_value_multiplier[ total_stars ]	-- bag
	bag_value = math.round( bag_value ) -- math.round( bag_value + ( bag_value * money_multiplier ) )
	
	--[[
	if is_level_limited then
		local level_limit_tweak_data = tweak_data.money_manager.level_limit
		local money_multiplier = managers.money:get_contract_difficulty_multiplier( difficulty_stars )
		local new_bag_value = secured_bags * tweak_data.money_manager.bag_value_multiplier[ job_stars ]	-- bag
		
		if managers.experience:current_level() <= level_limit_tweak_data.low_cap_level then
			bag_value = bag_value + bag_value * level_limit_tweak_data.low_cap_multiplier
			bag_value = math.round( math.min( bag_value, new_bag_value ) )
		else
			local diff_in_stars = job_and_difficulty_stars - player_stars
			local tweak_multiplier = level_limit_tweak_data.pc_difference_multipliers[diff_in_stars] or 0
			
			new_bag_value = new_bag_value + new_bag_value * money_multiplier
			local diff_in_money = new_bag_value - bag_value
			bag_value = bag_value + diff_in_money * tweak_multiplier
			bag_value = math.round( math.min( bag_value, new_bag_value ) )
		end
	end]]
	
	return bag_value
end

function MoneyManager:_get_contract_money( is_job, success )
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	local player_stars = managers.experience:level_to_stars()
	
	-- print( "job_and_difficulty_stars, player_stars", job_and_difficulty_stars, player_stars + 1 ) 
	local total_stars = math.min( job_and_difficulty_stars, player_stars + 1 )
	
	local total_difficulty_stars = math.max( 0, total_stars - job_stars )
	local money_multiplier = self:get_contract_difficulty_multiplier( total_difficulty_stars )
	
	total_stars = math.min( job_stars, total_stars )
	-- print( "total_stars", total_stars )
	
	-- print( "total_difficulty_stars, money_multiplier", total_difficulty_stars, money_multiplier )
	
	local contract_money = 0
	if is_job then
		contract_money = contract_money + self:get_job_payout_by_stars( total_stars )
		-- print( "AWARDED JOB COMPLETED MONEY", contract_money )
	else
		contract_money = contract_money + self:get_stage_payout_by_stars( total_stars )
		-- print( "AWARDED STAGE COMPLETED MONEY", contract_money )
	end
	
	contract_money = contract_money + (contract_money*money_multiplier)
	contract_money = contract_money -- * (not success and tweak_data.experience_manager.stage_failed_multiplier or 1) 
	-- print( "contract_money", contract_money )
	return math.round( contract_money )
end

function MoneyManager:get_job_bag_value()
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	return tweak_data.money_manager.bag_value_multiplier[ job_and_difficulty_stars ]
end

function MoneyManager:get_bag_value( carry_id )
	local carry_data = tweak_data.carry[ carry_id ]
	local bag_value_id = carry_data.bag_value or "default"
	local value = tweak_data.money_manager.bag_values[ bag_value_id ]
	return math.round( value )
end

-------------------------------------------------------------

function MoneyManager:debug_job_completed( stars )
	local amount = tweak_data.money_manager.job_completion[ stars ]
	self:_add_to_total( amount )
end

function MoneyManager:get_job_payout_by_stars( stars )
	local amount = tweak_data.money_manager.job_completion[ stars ]
	return amount
end

function MoneyManager:get_stage_payout_by_stars( stars )
	local amount = tweak_data.money_manager.stage_completion[ stars ]
	return amount
end

function MoneyManager:get_small_loot_difficulty_multiplier( stars ) -- stars are 0 - 3
	local multiplier = tweak_data.money_manager.small_loot_difficulty_multiplier[ stars ]
	
	return multiplier or 0
end

function MoneyManager:get_contract_difficulty_multiplier( stars ) -- stars are 0 - 3
	local multiplier = tweak_data.money_manager.difficulty_multiplier[ stars ]
	
	return multiplier or 0
end

function MoneyManager:get_potential_payout_from_current_stage()
	if not managers.job:has_active_job() then
		return 0
	end
	
	local job_and_difficulty_stars = managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	local money_multiplier = managers.money:get_contract_difficulty_multiplier( difficulty_stars )
	
	local amount = managers.money:get_stage_payout_by_stars( job_stars )
	if managers.job:on_last_stage() then
		amount = amount + managers.money:get_job_payout_by_stars( job_stars )
	end
	
	amount = amount + amount * money_multiplier
	return amount
end

------------------------------------------------------------------

function MoneyManager:can_afford_weapon( weapon_id )
	return self._global.total >= self:get_weapon_price_modified( weapon_id )
end

function MoneyManager:get_weapon_price( weapon_id )
	local pc = self:_get_weapon_pc( weapon_id )
	if not tweak_data.money_manager.weapon_cost[ pc ] then
		pc = 1
	end
	return tweak_data.money_manager.weapon_cost[ pc ]
end

function MoneyManager:get_weapon_price_modified( weapon_id )
	local pc = self:_get_weapon_pc( weapon_id )
	if not tweak_data.money_manager.weapon_cost[ pc ] then
		pc = 1
	end
	return math.round( tweak_data.money_manager.weapon_cost[ pc ] * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "crime_net_deal", 1 ) )
end

function MoneyManager:get_weapon_slot_sell_value( category, slot )
	local crafted_item = managers.blackmarket:get_crafted_category_slot( category, slot )
	if not crafted_item then
		return 0
	end
	local weapon_id = crafted_item.weapon_id
	local blueprint = crafted_item.blueprint
	
	local base_value = self:get_weapon_price( weapon_id )
	local parts_value = 0
	
	--[[
	if blueprint then
		local stats
		for _, id in ipairs( blueprint ) do
			stats = tweak_data.weapon.factory.parts[id].stats
			if stats then
				parts_value = parts_value + (tweak_data.weapon.stats.value[math.min( stats.value or 1, #tweak_data.weapon.stats.value )] * base_value)
			end
		end
	end]]
	
	-- print( base_value, parts_value )
	return math.round( (base_value + parts_value) * tweak_data.money_manager.sell_weapon_multiplier * managers.player:upgrade_value( "player", "sell_cost_multiplier", 1 ) )
end

function MoneyManager:get_weapon_sell_value( weapon_id )
	return math.round( self:get_weapon_price( weapon_id ) * tweak_data.money_manager.sell_weapon_multiplier * managers.player:upgrade_value( "player", "sell_cost_multiplier", 1 ) )
end

function MoneyManager:_get_weapon_pc( weapon_id )
	local weapon_level
	for level, level_data in pairs( tweak_data.upgrades.level_tree ) do
		for _, upgrade in ipairs( level_data.upgrades ) do
			if( upgrade == weapon_id ) then
				weapon_level = level
				break
			end
		end
	end
	
	if not weapon_level then
		Application:error( "DIDN'T FIND LEVEL FOR", weapon_id )
		weapon_level = 1
	end
	local pc = math.ceil( weapon_level )
	return pc
end

function MoneyManager:on_buy_weapon_platform( weapon_id )
	local amount = self:get_weapon_price_modified( weapon_id )
	-- print( "MoneyManager:on_buy_weapon_platform", amount )
	self:_deduct_from_total( amount )
end

function MoneyManager:on_sell_weapon( category, slot ) -- weapon_id )
	local amount = self:get_weapon_slot_sell_value( category, slot )
	-- local amount = self:get_weapon_sell_value( weapon_id )
	-- print( "MoneyManager:on_sell_weapon", amount )
	self:_add_to_total( amount, { no_offshore = true } )
end


function MoneyManager:get_weapon_part_sell_value( part_id )
	local part = tweak_data.weapon.factory.parts[ part_id ]
	local mod_price = 1000
	if part then
		local pc_value = self:_get_pc_entry( part )
		
		if pc_value then
			local star_value = math.ceil( pc_value/10 )
			mod_price = tweak_data.money_manager.modify_weapon_cost[ star_value ]
		end
		
		local stats_value = part.stats
		stats_value = stats_value and stats_value.value or 1
		mod_price = mod_price * tweak_data.weapon.stats.value[ math.clamp( stats_value, 1, #tweak_data.weapon.stats.value ) ]
	end
	return math.round( mod_price * tweak_data.money_manager.sell_weapon_multiplier * managers.player:upgrade_value( "player", "sell_cost_multiplier", 1 ) )
end

function MoneyManager:on_sell_weapon_part( part_id )
	local amount = self:get_weapon_part_sell_value( part_id )
	Application:debug( "value of removed weapon part", amount )
	self:_add_to_total( amount, { no_offshore = true } )
end

function MoneyManager:on_sell_weapon_slot( category, slot )
	local amount = self:get_weapon_slot_sell_value( category, slot )
	self._add_to_total( amount, { no_offshore = true } )
end

------------------------------------------------------------------

function MoneyManager:can_afford_mission_asset( asset_id )
	return self._global.total >= self:get_mission_asset_cost_by_id( asset_id )
end

function MoneyManager:on_buy_mission_asset( asset_id )
	local amount = self:get_mission_asset_cost_by_id( asset_id )
	
	
	-- print( "MoneyManager:on_buy_mission_asset", amount )
	self:_deduct_from_total( amount )
	return amount
end

------------------------------------------------------------------

function MoneyManager:can_afford_spend_skillpoint( tree, tier, points )
	return self._global.total >= self:get_skillpoint_cost( tree, tier, points )
end
function MoneyManager:can_afford_respec_skilltree( tree )
	return true -- self._global.total >= self:get_skilltree_tree_respec_cost( tree )
end

function MoneyManager:on_skillpoint_spent( tree, tier, points )
	local amount = self:get_skillpoint_cost( tree, tier, points )
	
	self:_deduct_from_total( amount )
end

function MoneyManager:on_respec_skilltree( tree, forced_respec_multiplier )
	local amount = self:get_skilltree_tree_respec_cost( tree, forced_respec_multiplier )
	-- managers.skilltree:increase_times_respeced( 1 )
	-- print( "MoneyManager:on_respec_skilltree", amount )
	-- self:_deduct_from_total( amount )
	self:_add_to_total( amount, { no_offshore = true } )
end

------------------------------------------------------------------

function MoneyManager:refund_weapon_part( weapon_id, part_id, global_value )
	local pc_value = tweak_data.blackmarket.weapon_mods[part_id].value or 1 	-- Changed system to used value instead of PC
	local mod_price = tweak_data.money_manager.modify_weapon_cost[ pc_value ] -- Changed system to used value instead of PC
	
	local global_value_multiplier = tweak_data.money_manager.global_value_multipliers[ global_value or "normal" ]
	-- local amount = self:get_weapon_modify_price( weapon_id, part_id, global_value )
	
	self:_add_to_total( math.round(mod_price*global_value_multiplier), { no_offshore = true } )
end

function MoneyManager:get_weapon_modify_price( weapon_id, part_id, global_value )
	local star_value
	
	-- local pc_value = self:_get_pc_entry( tweak_data.weapon.factory.parts[ part_id ] )
	-- local mod_price = tweak_data.money_manager.modify_weapon_cost[ (pc_value and math.ceil( pc_value/10 ) or 1) ]
	
	local pc_value = tweak_data.blackmarket.weapon_mods[part_id].value or 1 -- Changed system to used value instead of PC
	local mod_price = tweak_data.money_manager.modify_weapon_cost[ pc_value ] -- Changed system to used value instead of PC
	
	local global_value_multiplier = tweak_data.money_manager.global_value_multipliers[ global_value or "normal" ]
	local crafting_multiplier = managers.player:upgrade_value( "player", "passive_crafting_weapon_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "crafting_weapon_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "crime_net_deal", 1 )
	
	
	local total_price = mod_price * crafting_multiplier * global_value_multiplier
	-- print( "weapon_price", weapon_price, "mod_price", mod_price, "total_price", total_price, "crafting_multiplier", crafting_multiplier )
	return math.round( total_price )
end

function MoneyManager:can_afford_weapon_modification( weapon_id, part_id, global_value )
	return self._global.total >= self:get_weapon_modify_price( weapon_id, part_id, global_value )
end

function MoneyManager:on_buy_weapon_modification( weapon_id, part_id, global_value )
	local amount = self:get_weapon_modify_price( weapon_id, part_id, global_value )
	-- print( "MoneyManager:on_buy_weapon_modification", amount )
	self:_deduct_from_total( amount )
end

------------------------------------------------------------------

function MoneyManager:_get_pc_entry( entry )
	if not entry then
		Application:error( "MoneyManager:_get_pc_entry. No entry" )
		return 5
	end
	local pcs = entry.pcs
	local pc_value
	if not pcs then
		-- print( "no pcs for", entry )
		local pc = entry.pc
		if not pc then
			-- print( "no pc for entry", inspect( entry ) )
		else
			pc_value = pc
		end
	else
		pc_value = pcs[ 1 ] 
		for _,pcv in ipairs( pcs ) do
			pc_value = math.min( pc_value, pcv )
		end
	end
	return pc_value 
end

function MoneyManager:get_mask_part_price_modified( category, id, global_value )
	local mask_part_price = self:get_mask_part_price( category, id, global_value )
	local crafting_multiplier = managers.player:upgrade_value( "player", "passive_crafting_mask_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "crafting_mask_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "crime_net_deal", 1 )
	
	local total_price = mask_part_price * crafting_multiplier
	return math.round( total_price )
end

function MoneyManager:get_mask_crafting_price_modified( mask_id, global_value, blueprint )
	local mask_price = self:get_mask_crafting_price( mask_id, global_value, blueprint )
	local crafting_multiplier = managers.player:upgrade_value( "player", "passive_crafting_mask_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "crafting_mask_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 )
	crafting_multiplier = crafting_multiplier * managers.player:upgrade_value( "player", "crime_net_deal", 1 )
	
	local total_price = mask_price * crafting_multiplier
	return math.round( total_price )
end

function MoneyManager:get_mask_part_price( category, id, global_value )
	local part_pc = self:_get_pc_entry( tweak_data.blackmarket[category][id] ) or 0
	local star_value = (part_pc == 0 and part_pc) or math.ceil( part_pc / 10 )
	
	local part_name_converter = { textures="pattern", materials="material", colors="color" }
	-- local part_name_converter = { pattern="textures", material="materials", color="colors" }
	
	local gv_multiplier = tweak_data.money_manager.global_value_multipliers[global_value]
	
	local value = tweak_data.blackmarket[category][ id ].value or 1 -- Changed system to used value instead of PC
	
	local pv = tweak_data.money_manager.masks[part_name_converter[category] .. "_value"][value] or 0
	
	return math.round( pv * gv_multiplier )
end

function MoneyManager:get_mask_crafting_price( mask_id, global_value, blueprint )
	local bonus_global_values = { normal=0, superior=0, exceptional=0, infamous=0}
	
	-- local pc_value = self:_get_pc_entry( tweak_data.blackmarket.masks[ mask_id ] )
	local pc_value = tweak_data.blackmarket.masks[ mask_id ].value or 1 -- Changed system to used value instead of PC
	
	local star_value = pc_value and math.ceil( pc_value ) or 1
	
	local base_value = tweak_data.money_manager.masks.mask_value[ star_value ] * tweak_data.money_manager.global_value_multipliers[global_value]
	local parts_value = 0
	local part_name_converter = { pattern="textures", material="materials", color="colors" }
	
	bonus_global_values[ global_value ] = (bonus_global_values[ global_value ] or 0) + 1
	if not blueprint then
		blueprint = managers.blackmarket:get_default_mask_blueprint()
	end
	for id, data in pairs( blueprint ) do
		local part_pc = self:_get_pc_entry( tweak_data.blackmarket[part_name_converter[id]][data.id] ) or 0
		
		-- star_value = (part_pc == 0 and part_pc) or math.ceil( part_pc / 10 )
		star_value = tweak_data.blackmarket[part_name_converter[id]][ data.id ].value or 1 -- Changed system to used value instead of PC
		
		local gv_multiplier = tweak_data.money_manager.global_value_multipliers[data.global_value]
		bonus_global_values[ data.global_value ] = (bonus_global_values[ data.global_value ] or 0) + 1
		
		local pv = tweak_data.money_manager.masks[id .. "_value"][star_value] or 0
		parts_value = parts_value + pv * gv_multiplier
	end
	
	return math.round(base_value + parts_value), bonus_global_values
end

function MoneyManager:get_mask_sell_value( mask_id, global_value, blueprint )
	local sell_value, bonuses = self:get_mask_crafting_price( mask_id, global_value, blueprint )
	
	local bonus_multiplier
	for gv, amount in pairs( bonuses ) do
		bonus_multiplier = tweak_data.money_manager.global_value_bonus_multiplier[ gv ] * math.max( amount-1, 0 )
		sell_value = sell_value + ( sell_value * bonus_multiplier )
	end
	
	return math.round( sell_value * tweak_data.money_manager.sell_mask_multiplier * managers.player:upgrade_value( "player", "sell_cost_multiplier", 1 ) )
end

function MoneyManager:get_mask_slot_sell_value( slot )
	local mask = managers.blackmarket:get_crafted_category_slot( "masks", slot )
	if not mask then
		return 0
	end
	return math.round( self:get_mask_sell_value( mask.mask_id, mask.global_value, mask.blueprint ) )
end

function MoneyManager:can_afford_mask_crafting( mask_id, global_value, blueprint )
	return self._global.total >= self:get_mask_crafting_price_modified( mask_id, global_value, blueprint )
end

function MoneyManager:on_buy_mask( mask_id, global_value, blueprint )
	local amount = self:get_mask_crafting_price_modified( mask_id, global_value, blueprint )
	self:_deduct_from_total( amount )
end

function MoneyManager:on_sell_mask( mask_id, global_value, blueprint )
	local amount = self:get_mask_sell_value( mask_id, global_value, blueprint )
	self:_add_to_total( amount, { no_offshore = true } )
end

function MoneyManager:on_loot_drop_cash( value_id )
	local amount = tweak_data.money_manager.loot_drop_cash[ value_id ] or 100
	self:_add_to_total( amount, { no_offshore = true } )
end


------------------------------------------------------------------
function MoneyManager:get_skillpoint_cost( tree, tier, points )
	local respec_tweak_data = tweak_data.money_manager.skilltree.respec
	
	local exp_cost = 0
	--[[
	local points_spent = managers.skilltree:points_spent( tree )
	local exp_cost = respec_tweak_data.base_point_cost + (points_spent * respec_tweak_data.point_cost) ^ respec_tweak_data.point_multiplier_cost
	]]
	
	local tier_cost = (not tier and 0) or respec_tweak_data.point_tier_cost[tier] * points
	local cost = respec_tweak_data.base_point_cost + tier_cost
	
	return math.round( cost )
end

function MoneyManager:get_skilltree_tree_respec_cost( tree, forced_respec_multiplier )
	local respec_tweak_data = tweak_data.money_manager.skilltree.respec
	local value = respec_tweak_data.base_point_cost
	for id, tier in ipairs( tweak_data.skilltree.trees[tree].tiers ) do
		for _, skill_id in ipairs( tier ) do
			local step = managers.skilltree:skill_step( skill_id )
			if step > 0 then
				local skill_tweak_data = tweak_data.skilltree.skills[ skill_id ]
				for i=1, step do
					value = value + respec_tweak_data.base_point_cost + skill_tweak_data[i].cost * respec_tweak_data.point_tier_cost[id]
				end
			end
		end
	end
	
	return math.round( value * (forced_respec_multiplier or respec_tweak_data.respec_refund_multiplier) )
	--[[
	local respec_tweak_data = tweak_data.money_manager.skilltree.respec
	local cost = respec_tweak_data.base_cost
	
	local points_spent = managers.skilltree:points_spent( tree )
	local max_tier = managers.skilltree:current_max_tier( tree )
	
	cost = cost + (points_spent * respec_tweak_data.point_cost) ^ respec_tweak_data.point_multiplier_cost
	
	for i=1, max_tier do
		local tier_cost = respec_tweak_data.tier_cost[i] or 0
		
		cost = cost + tier_cost
	end
	
	local times_respeced = managers.skilltree:get_times_respeced()
	return math.round( cost * tweak_data.money_manager.skilltree.respec.profile_cost_increaser_multiplier[times_respeced] )
	]]
end

------------------------------------------------------------------

function MoneyManager:get_mission_asset_cost()
	local stars = managers.job:has_active_job() and managers.job:current_job_and_difficulty_stars() or 1
	return math.round( tweak_data.money_manager.mission_asset_cost[ stars ] * managers.player:upgrade_value( "player", "assets_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "passive_assets_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "crime_net_deal", 1 ) )
end

function MoneyManager:get_mission_asset_cost_by_stars( stars )
	return math.round( tweak_data.money_manager.mission_asset_cost[ stars ] * managers.player:upgrade_value( "player", "assets_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "passive_assets_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "crime_net_deal", 1 ) )
end

function MoneyManager:get_mission_asset_cost_by_id( id )
	local asset_tweak_data = managers.assets:get_asset_tweak_data_by_id( id )
	local value = asset_tweak_data and asset_tweak_data.money_lock or 0
	
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	
	local pc_multiplier = tweak_data.money_manager.mission_asset_cost_multiplier_by_pc[job_stars] or 0
	local risk_multiplier = difficulty_stars>0 and tweak_data.money_manager.mission_asset_cost_multiplier_by_risk[difficulty_stars] or 0
	
	
	value = value + value * pc_multiplier + value * risk_multiplier
	return math.round( value* managers.player:upgrade_value( "player", "assets_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "passive_assets_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "buy_cost_multiplier", 1 ) * managers.player:upgrade_value( "player", "crime_net_deal", 1 ) )
end
------------------------------------------------------------------

--[[function MoneyManager:calculate_end_score( multipliers )
	local player_alive = managers.player:player_unit() and true or false
	if not player_alive then
		return 0
	end
	
	multipliers = multipliers or {}
	local end_score = self._heist_total
	for _,multiplier in ipairs( multipliers ) do
		if not tweak_data.money_manager.end_multipliers[ multiplier ] then
			Application:error( "Unknown end multiplier \"" .. tostring( multiplier ) .. "\" in money manager." )
		end
		end_score = end_score * tweak_data.money_manager.end_multipliers[ multiplier ]
	end
	
	-- self._global.total = self._global.total + end_score
	self:_add_to_total( end_score )
	return math.round( end_score )
end]]

function MoneyManager:total()
	return self._global.total
end

function MoneyManager:total_collected()
	return self._global.total_collected
end

function MoneyManager:offshore()
	return self._global.offshore
end

function MoneyManager:total_spent()
	return self._global.total_spent
end

function MoneyManager:add_to_total( amount )
	amount = math.round( amount )
	print( "MoneyManager:add_to_total", amount )
	self:_add_to_total( amount )
end

function MoneyManager:_add_to_total( amount, params )
	local no_offshore = params and params.no_offshore
	local offshore = math.round( (no_offshore and 0) or amount * (1 - tweak_data.money_manager.offshore_rate) )
	local spending_cash = math.round( (no_offshore and amount) or amount * tweak_data.money_manager.offshore_rate )
	
	local rounding_error = math.round( amount - (offshore + spending_cash) )
	spending_cash = spending_cash + rounding_error
	
	self._global.total = self._global.total + spending_cash
	self._global.total_collected = self._global.total_collected + math.round( amount )
	self._global.offshore = self._global.offshore + offshore
	self:_on_total_changed( amount, spending_cash, offshore ) 
end

function MoneyManager:deduct_from_total( amount )
	amount = math.round( amount )
	print( "MoneyManager:deduct_from_total", amount )
	self:_deduct_from_total( amount )
end

function MoneyManager:_deduct_from_total( amount )
	amount = math.min( amount, self._global.total )
	self._global.total = math.max( 0, self._global.total - amount )
	self._global.total_spent = self._global.total_spent + amount 
	self:_on_total_changed( -amount, -amount, 0 )
end

function MoneyManager:_on_total_changed( amount, spending_cash, offshore )
	Application:debug( "MoneyManager:_on_total_changed:", "amount", amount, "total", self._global.total )
	-- self._heist_total = self._heist_total + spending_cash
	
	self._heist_total = self._heist_total + amount
	self._heist_offshore = self._heist_offshore + offshore
	
	if offshore and offshore > 0 then
		self._heist_spending = self._heist_spending + spending_cash
	end
	
	if self._global.total >= tweak_data.achievement.going_places then
		managers.achievment:award( "going_places" )
	end
	
	-- if (self._global.total_collected-self._global.total) >= tweak_data.achievement.spend_money_to_make_money then
	if self._global.total_spent >= tweak_data.achievement.spend_money_to_make_money then
		managers.achievment:award( "spend_money_to_make_money" )
	end
end

function MoneyManager:heist_total()
	return self._heist_total or 0
end

function MoneyManager:heist_spending()
	return self._heist_spending or 0
end

function MoneyManager:heist_offshore()
	return self._heist_offshore or 0
end

function MoneyManager:get_payouts()
	return self._stage_payout, self._job_payout, self._bag_payout, self._small_loot_payout, self._crew_payout
end

function MoneyManager:_set_stage_payout( amount )
	self._stage_payout = amount
end

function MoneyManager:_set_job_payout( amount )
	self._job_payout = amount
end

function MoneyManager:_set_bag_payout( amount )
	self._bag_payout = amount
end

function MoneyManager:_set_small_loot_payout( amount )
	self._small_loot_payout = amount
end

function MoneyManager:_set_crew_payout( amount )
	self._crew_payout = amount
end

function MoneyManager:_add( amount )
	amount = self:_check_multipliers( amount )
	self._heist_total = self._heist_total + amount
	self:_present( amount )
	-- managers.experience:add_points( amount )
end

function MoneyManager:_check_multipliers( amount )
	for _,multiplier in pairs( self._active_multipliers ) do
		amount = amount * multiplier
	end
	return math.round( amount )
end

function MoneyManager:_present( amount )
	-- print( "Recieved money:", amount, "Total now:", self._heist_total )
	
	local s_amount = tostring( amount )
	local reverse = string.reverse( s_amount )
	local present = ""
	for i = 1, string.len( reverse ) do
		present = present..string.sub( reverse, i, i )..((math.mod( i, 3 ) == 0 and (i ~= string.len( reverse )) and ",") or "")
	end
	
	-- Specify this in tweak data later
	local event = "money_collect_small"
	if amount > 999 then
		event = "money_collect_large"
	elseif amount > 101 then
		event = "money_collect_medium"
	end
	
	-- managers.hud:present_text( "$"..string.reverse( present ), 0.75 )
	
	--managers.hud:present_text( { text = "$"..string.reverse( present ), time = 0.75, event = event } )
end

-- Returns a sorted ipairs with all actions
function MoneyManager:actions()
	local t = {}
	for action,_ in pairs( tweak_data.money_manager.actions ) do
		table.insert( t, action )
	end
	table.sort ( t )
	return t
end

-- Returns a sorted ipairs with all multipliers
function MoneyManager:multipliers()
	local t = {}
	for multiplier,_ in pairs( tweak_data.money_manager.multipliers ) do
		table.insert( t, multiplier )
	end
	table.sort ( t )
	return t
end

function MoneyManager:reset()
	Global.money_manager = nil
	self:_setup()
end

function MoneyManager:save( data )
	local state = {	
				total = self._global.total,
				total_collected = self._global.total_collected,
				offshore = self._global.offshore,
				total_spent = self._global.total_spent,
				-- active_multipliers = self._active_multipliers
	}
	data.MoneyManager = state
end

function MoneyManager:load( data )
	local state = data.MoneyManager
	self._global.total = state.total
	self._global.total_collected = state.total_collected or 0
	self._global.offshore = state.offshore or 0
	self._global.total_spent = state.total_spent or 0
	
	--[[for multiplier,_ in pairs( state.active_multipliers ) do
		self:use_multiplier( multiplier )
	end]]
end