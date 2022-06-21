SkillTreeManager = SkillTreeManager or class()
SkillTreeManager.VERSION = 4

function SkillTreeManager:init()
	self:_setup()
end

function SkillTreeManager:_setup( reset )
	if not Global.skilltree_manager or reset then
		Global.skilltree_manager = {}
		Global.skilltree_manager.points = 0 -- managers.experience:current_level()
		Global.skilltree_manager.VERSION = SkillTreeManager.VERSION
		Global.skilltree_manager.reset_message = false
		Global.skilltree_manager.times_respeced = 1
		
		self._global = Global.skilltree_manager
		
		self._global.trees = {}
		for tree,data in pairs( tweak_data.skilltree.trees ) do
			self:_create_tree_data( tree ) 
			-- self._global.trees[ tree ] = { unlocked = false, points_spent = 0 }	
		end
		
		self._global.skills = {}
		for skill_id,data in pairs( tweak_data.skilltree.skills ) do
			self._global.skills[ skill_id ] = { unlocked = 0, total = #data }	
		end
	end
	
	self._global = Global.skilltree_manager
end

function SkillTreeManager:_create_tree_data( tree_id )
	self._global.trees[ tree_id ] = { unlocked = false, points_spent = 0 }
end

function SkillTreeManager:unlock_tree( tree )
	if self._global.trees[ tree ].unlocked then
		Application:error( "Tree", tree, "allready unlocked" )
		return
	end
	
	local skill_id = tweak_data.skilltree.trees[ tree ].skill
	local to_unlock = managers.skilltree:next_skill_step( skill_id )
	local skill = tweak_data.skilltree.skills[ skill_id ][ to_unlock ]
	local points = skill.cost or tweak_data.skilltree.costs.unlock_tree
	
	if( not skill or not skill.cost ) then
		print( "[SkillTreeManager:unlock_tree] skill tree: \"" .. tostring(skill_id) .. "\" is missing cost!" )
	end
	
	if points > self:points() then
		return
	end
	
	self._global.trees[ tree ].unlocked = true
	self:_spend_points( tree, nil, points )
	
	-- self._global.skills[ skill_id ]
	-- self._global.skills[ skill_id ].unlocked = to_unlock
	 
	-- skill = tweak_data.skilltree.skills[ skill_id ][ self._global.skills[ skill_id ].unlocked ]
	-- local skill = tweak_data.skilltree.skills[ skill_id ]
	-- self:_aquire_skill( skill, skill_id )
	-- self:_on_skill_unlocked( tree, skill_id )
end

function SkillTreeManager:_spend_points( tree, tier, points )
	local pre_unlocked_tier = self:current_max_tier( tree )
	managers.money:on_skillpoint_spent( tree, tier, points )
	
	self._global.points = self._global.points - points
	self._global.trees[ tree ].points_spent = self._global.trees[ tree ].points_spent + points
	
	local post_unlocked_tier = self:current_max_tier( tree )
	
	-- Check unlocked tiers
	if post_unlocked_tier > pre_unlocked_tier then
		self:_on_tier_unlocked( tree, post_unlocked_tier )
	end
	
	self:_on_points_spent( tree, points )
end

function SkillTreeManager:points()
	return self._global.points
end

function SkillTreeManager:points_spent( tree )
	return self._global.trees[ tree ].points_spent
end

function SkillTreeManager:current_max_tier( tree )
	for tier,point in ipairs( tweak_data.skilltree.tier_unlocks ) do
		if self._global.trees[ tree ].points_spent < point then
			return tier-1
		end 
	end
	return #tweak_data.skilltree.tier_unlocks
end

function SkillTreeManager:skill_completed( skill_id )
	return self._global.skills[ skill_id ].unlocked == self._global.skills[ skill_id ].total
end

function SkillTreeManager:skill_step( skill_id )
	return self._global.skills[ skill_id ].unlocked
end

function SkillTreeManager:next_skill_step( skill_id )
	return self._global.skills[ skill_id ].unlocked + 1
end

function SkillTreeManager:next_skill_step_data( skill_id )
	return tweak_data.skilltree.skills[ skill_id ][ self._global.skills[ skill_id ].unlocked ]
end

function SkillTreeManager:skill_unlocked( tree, skill_id )
	if not tree then
		for tree_id,_ in pairs( tweak_data.skilltree.trees ) do
			-- print( "CALL", tree_id )
			if self:skill_unlocked( tree_id, skill_id ) then
				return true
			end
		end
		return false
	end
	
	for tier,data in pairs( tweak_data.skilltree.trees[ tree ].tiers ) do
		for _,skill in ipairs( data ) do
			if skill == skill_id then
				return self:tier_unlocked( tree, tier )
			end
		end
	end
end

function SkillTreeManager:unlock( tree, skill_id )
	if not self._global.trees[ tree ].unlocked then
		Application:error( "Cannot unlock skill", skill_id, "in tree", tree, ". Tree is locked" )
		return
	end
	
	if self._global.skills[ skill_id ].unlocked >= self._global.skills[ skill_id ].total then
		Application:error( "No more steps to unlock in skill", skill_id )
		return
	end	
	-- local points = skill_id.cost
	-- self._global.points = self._global.points - points
	
	local talent = tweak_data.skilltree.skills[ skill_id ]
	
				-- to block unlocking if prerequisites not met
	local prerequisites = talent.prerequisites or {}
	
	for _, prerequisite in ipairs( prerequisites ) do
		local unlocked = managers.skilltree:skill_step( prerequisite )
		if( unlocked and unlocked == 0 ) then   -- if( self:skill_unlocked( tree, prerequisite ) == false ) then		-- not managers.upgrades:aquired( prerequisite )
			return
		end
	end
	
	
	local to_unlock = managers.skilltree:next_skill_step( skill_id ) -- self._global.skills[ skill_id ].unlocked + 1
	local skill = talent[ to_unlock ]
	local points = skill.cost
	if points > self:points() then
		-- print( "not enough points" )
		return
	end
	
	self._global.skills[ skill_id ].unlocked = to_unlock
	
	local tier = nil
	for i, tier_skills in ipairs( tweak_data.skilltree.trees[ tree ].tiers ) do
		if table.contains( tier_skills, skill_id ) then
			tier = i
			break
		end
	end
	
	self:_spend_points( tree, tier, points )
	-- self:_unlock( tree, skill_id )
	self:_aquire_skill( skill, skill_id )
	self:_on_skill_unlocked( tree, skill_id )
end

function SkillTreeManager:_on_tier_unlocked( tree, tier )
	local skill_id = tweak_data.skilltree.trees[ tree ].skill
	local to_unlock = managers.skilltree:next_skill_step( skill_id )
	
	while( to_unlock <= tier ) do
		local skill = tweak_data.skilltree.skills[ skill_id ][ to_unlock ]
		if( not skill ) then
			print( "SkillTreeManager:_on_tier_unlocked: No tier upgrade at tier", tier, "for tree", tree )
			break;
		end
		self._global.skills[ skill_id ].unlocked = to_unlock
		
		self:_aquire_skill( skill, skill_id )
		self:_on_skill_unlocked( tree, skill_id )
		
		to_unlock = managers.skilltree:next_skill_step( skill_id )
	end
	--[[
	tweak_data.skilltree.skills[  ][ tier ]
	
	local points = skill.cost or tweak_data.skilltree.costs.unlock_tree
	
	self:_aquire_skill( skill, skill_id )
	self:_on_skill_unlocked( tree, skill_id )
	]]
	managers.menu_component:on_tier_unlocked( tree, tier )
end

function SkillTreeManager:_on_skill_unlocked( tree, skill_id )
	managers.menu_component:on_skill_unlocked( tree, skill_id )
end

function SkillTreeManager:_on_points_spent( tree, points )
	self:_check_achievements()	
	managers.menu_component:on_points_spent( tree, points )
end

function SkillTreeManager:_check_achievements()
	for _,data in pairs( self._global.trees ) do
		if data.points_spent < tweak_data.achievement.im_a_healer_tank_damage_dealer then
			return
		end
	end
	managers.achievment:award( "im_a_healer_tank_damage_dealer" )
end

function SkillTreeManager:level_up()
	self:_aquire_points( 1 )
end

function SkillTreeManager:rep_upgrade( upgrade, id )
	self:_aquire_points( (upgrade and upgrade.value) or 2 )
end

function SkillTreeManager:_aquire_points( points )
	self._global.points = self._global.points + points 
end

function SkillTreeManager:tier_unlocked( tree, tier )
	local required_points = tweak_data.skilltree.tier_unlocks[ tier ]
	return self._global.trees[ tree ].points_spent >= required_points
end

function SkillTreeManager:tree_unlocked( tree )
	return self._global.trees[ tree ].unlocked
end

function SkillTreeManager:_unlock( tree, skill_id )
	--[[if not tweak_data.skills.definitions[ skill_id ] then
		Application:error( "Tried to aquire an upgrade that doesn't exist: " .. (skill_id or "nil") .. "" )
		return
	end

	if self._global.aquired[ skill_id ] then
		Application:error( "Tried to aquire an upgrade that has allready been aquired: "..skill_id.."" )
		return
	end]]
		
	-- self._global.aquired[ id ] = true
	
	local skill = tweak_data.skills.definitions[ skill_id ]
	-- self._global.skills[ skill_id ]
	self:_aquire_skill( skill, skill_id )
end

function SkillTreeManager:_aquire_skill( skill, skill_id )
	-- print( "SkillTreeManager:_aquire_skill", skill, inspect( skill.upgrades ), skill_id )
	if skill.upgrades then
		for _,upgrade in ipairs( skill.upgrades ) do
			managers.upgrades:aquire( upgrade )
		end
	end
	--[[if upgrade.category == "weapon" then
		self:_aquire_weapon( upgrade, id )
	elseif upgrade.category == "feature" then
		self:_aquire_feature( upgrade, id )
	elseif upgrade.category == "outfit" then
		self:_aquire_outfit( upgrade, id )
	elseif upgrade.category == "weapon_upgrade" then
		self:_aquire_weapon_upgrade( upgrade, id )
	elseif upgrade.category == "money_multiplier" then
		self:_aquire_money_upgrade( upgrade, id )
	elseif upgrade.category == "equipment" then
		self:_aquire_equipment( upgrade, id )
	elseif upgrade.category == "equipment_upgrade" then
		self:_aquire_equipment_upgrade( upgrade, id )
	end]]
end

function SkillTreeManager:_unaquire_skill( skill_id )
	-- print( "SkillTreeManager:_unaquire_skill", skill_id )
	local progress_data = self._global.skills[ skill_id ]
	local skill_data = tweak_data.skilltree.skills[ skill_id ]
	
	-- print( "progress_data", inspect( progress_data ) )
	for i = progress_data.unlocked, 1, -1 do
		local step_data = skill_data[ i ]
		local upgrades = step_data.upgrades
		if upgrades then
			for i = #upgrades, 1, -1 do
				local upgrade = upgrades[ i ]
				-- print( "Should unaquire upgrade", upgrade )
				managers.upgrades:unaquire( upgrade )
			end
		end
	end
	
	progress_data.unlocked = 0
end

function SkillTreeManager:on_respec_tree( tree, forced_respec_multiplier )
	local points_spent = self._global.trees[ tree ].points_spent
	self._global.trees[ tree ].points_spent = 0
	self._global.trees[ tree ].unlocked = false
	print( "points_spent", points_spent, "give back" )
	
	managers.money:on_respec_skilltree( tree, forced_respec_multiplier )
	local tree_data = tweak_data.skilltree.trees[ tree ]
	
	for i = #tree_data.tiers, 1, -1 do
		local tier = tree_data.tiers[ i ]
		for _,skill in ipairs( tier ) do
			self:_unaquire_skill( skill )
		end
	end
	
	self:_unaquire_skill( tree_data.skill )
 	
 	self:_aquire_points( points_spent )
end

--[[function SkillTreeManager:is_locked( step )
	local level = managers.experience:current_level()
	for i,d in ipairs( tweak_data.upgrades.itree_caps ) do
		if level < d.level then
			return step >= d.step
		end
	end
	return false
end

function SkillTreeManager:get_level_from_step( step )
	for i,d in ipairs( tweak_data.upgrades.itree_caps ) do
		if step == d.step then
			return d.level
		end
	end
	return 0
end

function SkillTreeManager:progress_by_tree( tree )
	return self._global.progress[ tree ]
end


function SkillTreeManager:aquired_by_category( category )
	local t = {}
	for name,_ in pairs( self._global.aquired ) do
		if tweak_data.upgrades.definitions[ name ].category == category then
			table.insert( t, name )
		end
	end
	return t
end

function SkillTreeManager:aquired_features()
	return self:aquired_by_category( "feature" )
end

function SkillTreeManager:aquired_outfits()
	return self:aquired_by_category( "outfit" )
end

function SkillTreeManager:aquired_weapons()
	return self:aquired_by_category( "weapon" )
end

function SkillTreeManager:print_aquired_tree()
	local tree = {}
	
	for name, data in pairs( self._global.aquired ) do
		tree[ data.level ] = { name = name }
	end
	
	for i,data in pairs( tree ) do
		print( self:name( data.name ) )
	end
end]]

function SkillTreeManager:analyze()
	
end

function SkillTreeManager:tree_stats()
	
end

function SkillTreeManager:increase_times_respeced( increase )
	self._global.times_respeced = math.clamp( self._global.times_respeced + increase, 1, #tweak_data.money_manager.skilltree.respec.profile_cost_increaser_multiplier )
end

function SkillTreeManager:get_times_respeced()
	return self._global.times_respeced
end

function SkillTreeManager:reset_skilltrees()
	for tree_id, tree_data in pairs( self._global.trees ) do
		self:on_respec_tree( tree_id, 1 )
	end
	self._global.VERSION = SkillTreeManager.VERSION
	self._global.reset_message = true
	self._global.times_respeced = 1
end

function SkillTreeManager:check_reset_message()
	local show_reset_message = self._global.reset_message and true or false
	
	if show_reset_message then
		managers.menu:show_skilltree_reseted()
		self._global.reset_message = false
		MenuCallbackHandler:save_progress()
	end
end

			-- pd2 alpha final specific
function SkillTreeManager:get_tree_progress( tree )
	if type(tree) ~= "number" then
		local string_to_number = { mastermind=1, enforcer=2, technician=3, ghost=4 }
		tree = string_to_number[ tree ]
	end
	
	local td = tweak_data.skilltree.trees[tree]
	local skill_id = td.skill
	
	local step = managers.skilltree:next_skill_step( skill_id )
	local unlocked = managers.skilltree:skill_unlocked( nil, skill_id )
	local completed = managers.skilltree:skill_completed( skill_id )
	
	local progress = (step > 1) and 1 or 0
	local num_skills = 1
	
	if progress > 0 then
		for _, tier in ipairs( td.tiers ) do
			for _, skill_id in ipairs( tier ) do
				step = managers.skilltree:next_skill_step( skill_id )
				unlocked = managers.skilltree:skill_unlocked( nil, skill_id )
				completed = managers.skilltree:skill_completed( skill_id )
				
				num_skills = num_skills + 2
				progress = progress + ( step>1 and 1 or 0 ) + ( completed and 1 or 0 )
			end
		end
	end
	
	return progress, num_skills -- math.round( progress / num_skills * 100 )
end

function SkillTreeManager:get_most_progressed_tree()
	local max_tree = 1
	local max_points = 0
	
	for tree, _ in ipairs( tweak_data.skilltree.trees ) do
		local points = self:get_tree_progress( tree )
		
		if points > max_points then
			max_tree = tree
			max_points = points
		end
	end
	return max_tree
end
------------------------------------------------------------------

function SkillTreeManager:save( data )
	local state = {
		points = self._global.points,
		trees = self._global.trees,
		skills = self._global.skills,
		
		VERSION = self._global.VERSION or 0,
		reset_message = self._global.reset_message,
		times_respeced = self._global.times_respeced or 1,
	}
		
	data.SkillTreeManager = state
end

function SkillTreeManager:load( data, version )
	local state = data.SkillTreeManager
	-- print( "SkillTreeManager:load", inspect( state ), version, ": points when loaded", self._global.points )
	
	local points_aquired_during_load = self._global.points -- Basicly points aquired from level tree upgrade. Uses this value in _verify_loaded_data to see if we allready has accounted for it
	
	if state then
		self._global.points			= state.points
		-- self._global.trees			= state.trees
		for tree_id,tree_data in pairs( state.trees ) do
			self._global.trees[ tree_id ] = tree_data
		end
		for skill_id,skill_data in pairs( state.skills ) do
			if self._global.skills[ skill_id ] then
				self._global.skills[ skill_id ].unlocked = skill_data.unlocked
			end
		end
		-- self._global.skills			= state.skills
			
		self:_verify_loaded_data( points_aquired_during_load )
		
		
		self._global.VERSION = state.VERSION
		self._global.reset_message = state.reset_message
		self._global.times_respeced = state.times_respeced
		
		if not self._global.VERSION or self._global.VERSION < SkillTreeManager.VERSION then
			managers.savefile:add_load_done_callback( callback( self, self, "reset_skilltrees" ) )
		end
	end
end


function SkillTreeManager:_verify_loaded_data( points_aquired_during_load )
	
	-- Checks retroactivle added points from upgrades with skill points
	local level_points = managers.experience:current_level()
	local assumed_points = level_points + points_aquired_during_load
	
	local points = self._global.points
	for tree_id,data in pairs( clone( self._global.trees ) ) do
		points = points + data.points_spent
	end
	
	-- print( "assumed_points", assumed_points, points )
	if assumed_points > points then
		-- print( "Retroactive aquired upgrades from level tree with skill points, add them" )
		self._global.points = self._global.points + (assumed_points - points)
	end

	-- HANDLE DLC, skill trees might have been removed if DLC has been uninstalled
	
	for tree_id,data in pairs( clone( self._global.trees ) ) do
		 if	not tweak_data.skilltree.trees[ tree_id ] then
		 	print( "[SkillTreeManager:_verify_loaded_data] Tree doesn't exists", tree_id, ", removing loaded data." )
		 	self._global.trees[ tree_id ] = nil
		 end
	end
	
	for tree_id,tree_data in pairs( self._global.trees ) do
		if tree_data.unlocked then
			if not tweak_data.skilltree.trees[ tree_id ].dlc then -- or has_dlc( tree[ tree_id ].dlc )
				local skill_id = tweak_data.skilltree.trees[ tree_id ].skill
				local skill = tweak_data.skilltree.skills[ skill_id ]
				
				local skill_data = self._global.skills[ skill_id ]
				for i = 1, skill_data.unlocked do
					self:_aquire_skill( skill[i], skill_id )
				end
				
				for tier,skills in pairs( tweak_data.skilltree.trees[ tree_id ].tiers ) do
					for _,skill_id in ipairs( skills ) do
						-- print( "load skill", skill_id )
						local skill = tweak_data.skilltree.skills[ skill_id ]
						local skill_data = self._global.skills[ skill_id ]
						for i = 1, skill_data.unlocked do
							self:_aquire_skill( skill[i], skill_id )
						end
					end
				end
			end
		end
	end
end

----------------------------------------

function SkillTreeManager:debug()
	managers.debug:set_enabled( true )
	managers.debug:set_systems_enabled( true, { "gui" } )
	local gui = managers.debug._system_list[ "gui" ]
	gui:clear()
	
	local j = 1
	
	local add_func = function( skill_id )
		local skill = tweak_data.skilltree.skills[ skill_id ]
		local skill_data = self._global.skills[ skill_id ]
		for i = 1, skill_data.unlocked do
			local sub_skill = skill[ i ]
			
			local type = i == 1 and "STD" or "PRO"
			
			if sub_skill.upgrades then
				for _,upgrade in ipairs( sub_skill.upgrades ) do
					local value = managers.upgrades:get_value( upgrade )
					if value then
					
						if managers.upgrades:get_category( upgrade ) == "temporary" then
							local u = managers.upgrades:get_upgrade_upgrade( upgrade )
							local index = j
							gui:set_func( j, function()
									if managers.player:has_activate_temporary_upgrade( u.category, u.upgrade ) then
										gui:set_color( index, 0, 1, 0 )
									else
										if math.mod( index, 2 ) == 0 then
											gui:set_color( index, 0.75, 0.75, 0.75, 0.5 )
										else
											gui:set_color( index, 1, 1, 1, 0.5 )
										end			
									end
									return skill_id.." "..type.." - "..upgrade..":    "..tostring(value)	
								end	)
								
						else
							
							gui:set_func( j, function() return skill_id.." "..type.." - "..upgrade..":    "..tostring(value) end )
							if math.mod( j, 2 ) == 0 then
								gui:set_color( j, 0.75, 0.75, 0.75 )
							else
								gui:set_color( j, 1, 1, 1 )
							end
						end
						
						j = j + 1
					end
				end
			else
				gui:set_func( j, function() return skill_id.." "..type.."" end )
				j = j + 1
			end
		end
	end
	
	for tree_id,tree_data in pairs( self._global.trees ) do
		if tree_data.unlocked then
			if not tweak_data.skilltree.trees[ tree_id ].dlc then -- or has_dlc( tree[ tree_id ].dlc )
				local skill_id = tweak_data.skilltree.trees[ tree_id ].skill
				add_func( skill_id )
				
				for tier,skills in pairs( tweak_data.skilltree.trees[ tree_id ].tiers ) do
					for _,skill_id in ipairs( skills ) do
						add_func( skill_id )
					end
				end
			end
		end
	end
end

----------------------------------------

-- Not used, the idea was for respec of points
--[[function SkillTreeManager:reset_points()
	self._global.points = managers.experience:current_level()
 
	for skill_id,skill_data in pairs( self._global.skills ) do
 		local skill = tweak_data.skilltree.skills[ skill_id ]
 		for i = 1, skill_data.unlocked do
 			-- print( "for skill", skill_id )
 			self:_unaquire_skill( skill[ i ], skill_id )
 		end
 	end
 	
 	self:reset()
end]]

function SkillTreeManager:reset()
	Global.skilltree_manager = nil
	self:_setup()
end

