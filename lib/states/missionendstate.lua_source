require "lib/states/GameState"

MissionEndState = MissionEndState or class( GameState )
MissionEndState.GUI_ENDSCREEN 	= Idstring( "guis/victoryscreen/stage_endscreen" )

function MissionEndState:init( name, game_state_machine, setup )
	GameState.init( self, name, game_state_machine )
	self._type = ""
	
	self._completion_bonus_done = false
	self._continue_cb = callback( self, self, "_continue" )
	-- self._debug_continue_cb = callback( self, self, "debug_continue" )
	self._controller = nil
	self._continue_block_timer = 0
end

function MissionEndState:setup_controller()
	if( not self._controller ) then
		self._controller = managers.controller:create_controller( "victoryscreen", managers.controller:get_default_wrapper_index(), false )
		
		if( Network:is_server() ) then
			-- self._controller:add_trigger( "continue", self._continue_cb )
		end
		-- self._controller:add_trigger( "debug_continue", self._debug_continue_cb )
		
		self._controller:set_enabled( true )
	end
	
	-- local enabled = not managers.menu:active_menu()
	-- self._controller:set_enabled( enabled )
end

function MissionEndState:set_controller_enabled( enabled )
	if self._controller then
		-- self._controller:set_enabled( enabled )
	end
end

function MissionEndState:at_enter( old_state, params )
	managers.platform:set_presence( "Mission_end" )
	managers.platform:set_rich_presence( Global.game_settings.single_player and "SPEnd" or "MPEnd" )
	managers.hud:remove_updator( "point_of_no_return" )
	managers.hud:hide_stats_screen()
	
	-- managers.menu_component:set_active_components( { "endstate" } )	
	self._continue_block_timer = Application:time() + 1.5
	
	if Network:is_server() then
		managers.network.matchmake:set_server_joinable( false )
		if self._success then
			for peer_id,data in pairs( managers.player:get_all_synced_carry() ) do
				if not tweak_data.carry[ data.carry_id ].skip_exit_secure then
					print( "Secure loot for", peer_id, data.carry_id, data.value )
					managers.loot:secure( data.carry_id, data.value )
				end
			end
		end
	end

	local player = managers.player:player_unit()
	if player then
		player:camera():remove_sound_listener()
	end
	
	
	Application:debug( "1 second to managers.mission:pre_destroy()" )
	self._mission_destroy_t = Application:time() + 1.0
	if not self._success then
		managers.job:set_stage_success( false )
	end
	
	if self._success then
		print( "MissionEndState:at_enter", managers.job:on_last_stage() )
		managers.job:set_stage_success( true )
		
		if self._type=="victory" --[[or self._type=="gameover"]] then
			managers.money:on_mission_completed( params.num_winners )		-- does all things below
		end
			--[[
		if not managers.job:interupt_stage() then -- Skip money if escape level
			
			
			if managers.job:on_last_stage() then
			-- if managers.job:on_last_stage() and not managers.job:interupt_stage() then 	-- MAYBE FOR ESCAPE LAST
				self._money_earned = managers.money:on_job_completed( params.num_winners )
			else
				self._money_earned = managers.money:on_stage_completed( params.num_winners )
			end
			
			-- Award loose/smallloot cash
			local loose_cash = managers.loot:get_real_total_small_loot_value()
			managers.money:add_to_total( loose_cash )
			-- self._money_earned = self._money_earned + loose_cash
		
		end
			]]
	end
	
	if SystemInfo:platform() == Idstring( "WIN32" ) and managers.network.account:has_alienware() then
		LightFX:set_lamps( 0, 255, 0, 255 )
	end
	
	self._completion_bonus_done = self._completion_bonus_done or false
	self:setup_controller()

	if( not self._setup ) then
		self._setup = true
		managers.hud:load_hud( self.GUI_ENDSCREEN, false, true, false, {}, nil, nil, true )
	
		managers.menu:open_menu( "mission_end_menu", 1 )
		self._mission_end_menu = managers.menu:get_menu( "mission_end_menu" )
	end
		
	self._old_state = old_state
	
	-- managers.hud:hide( PlayerBase.PLAYER_INFO_HUD_PD2 ) -- THIS SHOULD BE HIDDEN BY STATES THAT NEEDS IT
	-- managers.hud:set_chat_output_state( "mission_end" )
-- 	managers.menu_component:pre_set_game_chat_leftbottom( 0, 0 )
	managers.menu_component:set_max_lines_game_chat( 7 )
	-- managers.hud:show( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN )
	-- managers.hud:show( PlayerBase.PLAYER_INFO_HUD )
	
	for _, component in ipairs( managers.hud:script( PlayerBase.PLAYER_INFO_HUD ).panel:children() ) do
		if component:name() == "title_mid_text" or
			component:name() == "present_mid_text" or
			component:name() == "present_mid_icon" then
			if not managers.hud._mid_text_presenting or managers.hud._mid_text_presenting.type ~= "challenge" then
				component:set_visible( false )
			end
		else
			component:set_visible( false )
		end
	end
	
	if not managers.hud._mid_text_presenting or managers.hud._mid_text_presenting.type ~= "challenge" then
		managers.hud:script( PlayerBase.PLAYER_INFO_HUD_FULLSCREEN ).present_background:set_visible( false )
	end

	-- managers.hud:show( self.GUI_SAFERECT )
	-- managers.hud:script( self.GUI_SAFERECT ):reset()
	-- managers.hud:show( self.GUI_FULLSCREEN )
	-- managers.hud:script( self.GUI_SAFERECT ):set_success( self._success, self._server_left )
	-- managers.hud:script( self.GUI_SAFERECT ):hide_legends()
	managers.hud:set_success_endscreen_hud( self._success, self._server_left )
	managers.hud:show_endscreen_hud()
	
	managers.groupai:state():set_AI_enabled( false )

	local player = managers.player:player_unit()
	if player then
		player:character_damage():set_invulnerable( true )
		player:character_damage():stop_heartbeat()
		player:base():set_stats_screen_visible( false )
		if player:movement():current_state():shooting() then
			player:movement():current_state()._equipped_unit:base():stop_shooting()
		end
		if player:movement():current_state()._interupt_action_interact then
			player:movement():current_state():_interupt_action_interact()
		end
	end
	self._sound_listener = SoundDevice:create_listener( "lobby_menu" )
	self._sound_listener:set_position( Vector3( 0, -50000, 0 ) )
	self._sound_listener:activate( true )
		
	if self._success then
		if params.personal_win then
			if managers.achievment:get_script_data( "last_man_standing" ) then
				managers.challenges:set_flag( "last_man_standing" )
			end

			if not managers.statistics:is_dropin() then
				if managers.achievment:get_script_data( "dodge_this_active" ) and not managers.achievment:get_script_data( "dodge_this_fail" ) and 
				   tweak_data:difficulty_to_index( Global.game_settings.difficulty ) >= 2 then
					managers.challenges:set_flag( "dodge_this" )
				end
				
				if not managers.achievment:get_script_data( "pacifist_fail" ) and Global.level_data.level_id == "suburbia" and
					tweak_data:difficulty_to_index( Global.game_settings.difficulty ) >= 3 then
					managers.challenges:set_flag( "pacifist" )
				end
				
				if not managers.achievment:get_script_data( "blow_out_fail" ) and Global.level_data.level_id == "secret_stash" then
					managers.challenges:set_flag( "blow_out" )
				end
				
				if tweak_data:difficulty_to_index( Global.game_settings.difficulty ) >= 4 then
					local noob_lubes = 0
					for _,data in ipairs( Global.player_manager.synced_bonuses ) do
						if data.upgrade == "welcome_to_the_gang" then
							noob_lubes = noob_lubes + 1
						end
					end
				
					if noob_lubes >= 3 then
						managers.challenges:set_flag( "noob_herder" )
					end
				end
				
				if not managers.achievment:get_script_data( "stand_together_fail" ) and
				   tweak_data:difficulty_to_index( Global.game_settings.difficulty ) >= 2 and
				   Global.level_data.level_id == "heat_street" then
					managers.challenges:set_flag( "stand_together" )
				end
			end
		elseif params.num_winners == 3 and not alive( managers.player:player_unit() ) then
			managers.challenges:set_flag( "left_for_dead" )
		end
	end
	
	self._criminals_completed = (self._success and params.num_winners) or 0
	managers.statistics:stop_session( { success = self._success } )
	managers.statistics:send_statistics()
	-- managers.hud:script( self.GUI_SAFERECT ):set_statistics( self._success and params.num_winners or 0, self._success )
	managers.hud:set_statistics_endscreen_hud( self._criminals_completed, self._success )
	
	managers.music:post_event( self._success and "resultscreen_win" or "resultscreen_lose" )
	
	managers.enemy:add_delayed_clbk( "play_finishing_sound", callback( self, self, "play_finishing_sound", self._success ), Application:time() + 2 )


-- 	self._bonuses = { false, false, false, false, false, total=0 }	-- risk, num_player_bonus, last_stage, failed, low_level
	
	if self._type=="victory" or self._type=="gameover" then
		local total_xp_bonus, bonuses = self:_get_xp_dissected( self._success, params and params.num_winners )
		self._bonuses = bonuses
		
		self:completion_bonus_done( total_xp_bonus )
	end
	--[[
	if self._success then
		-- local completed_objectives = managers.statistics:completed_objectives()
		-- local total_objectives = managers.objectives:total_objectives( Global.level_data and Global.level_data.level_id )
		-- local total_civilian_kills = managers.statistics:session_total_civilian_kills()
		
		-- local objectives_bonus_xp = math.round( total_objectives ~= 0 and ((completed_objectives/total_objectives) * tweak_data.experience_manager.total_objectives_finished) or 0 )
		-- local criminals_bonus_xp = self._criminals_completed * tweak_data.experience_manager.total_criminals_finished
		-- local civilians_penalty_xp = total_civilian_kills * tweak_data.experience_manager.civilians_killed
		
		local contract_xp = self:_get_contract_xp( self._success )
		local total_xp_bonus = contract_xp
		
		self._bonuses.total = total_xp_bonus
		
		-- total_xp_bonus = math.max( 0, total_xp_bonus - civilians_penalty_xp )
		local num_players_bonus = (params.num_winners and tweak_data.experience_manager.alive_humans_multiplier[ params.num_winners ] or 1)
		total_xp_bonus = total_xp_bonus * num_players_bonus
			self._bonuses[2] = params.num_winners > 1 and params.num_winners or false
		-- print( "TOTAL XP EARNED", total_xp_bonus )
		self:completion_bonus_done( total_xp_bonus )
	else
		self:completion_bonus_done( self:_get_contract_xp( self._success ) )
	end]]
	
	if Network:is_server() then
		managers.network:session():set_state( "game_end" )
	end
end

function MissionEndState:is_success()
	return self._success
end

-- Limits start based n player level.
-- Also decides if the difficulty multiplier should be less because of player level.
function MissionEndState:_get_xp_dissected( success, num_winners )
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	local player_stars = managers.experience:level_to_stars()
	local days_multiplier = managers.experience:get_current_job_day_multiplier()
	local is_level_limited = player_stars < job_and_difficulty_stars
	
	local total_stars = math.min( job_and_difficulty_stars, player_stars )
	local total_difficulty_stars = math.max( 0, total_stars - job_stars )
	local xp_multiplier = managers.experience:get_contract_difficulty_multiplier( total_difficulty_stars )
	
	total_stars = math.min( job_stars, total_stars )
	
	local contract_xp = 0
	local total_xp = 0
	
	local stage_xp_dissect = 0
	local job_xp_dissect = 0
	local level_limit_dissect = 0
	local risk_dissect = 0
	local failed_level_dissect = 0
	local alive_crew_dissect = 0
	local skill_dissect = 0
	local base_xp = 0
	local days_dissect = 0
	
	if success and has_active_job and managers.job:on_last_stage() then
		job_xp_dissect = managers.experience:get_job_xp_by_stars( job_stars )
		contract_xp = contract_xp + managers.experience:get_job_xp_by_stars( total_stars )
		
		level_limit_dissect = level_limit_dissect + job_xp_dissect
	end
	stage_xp_dissect = managers.experience:get_stage_xp_by_stars( job_stars )
	contract_xp = contract_xp + managers.experience:get_stage_xp_by_stars( total_stars )
	level_limit_dissect = level_limit_dissect + stage_xp_dissect
	
	base_xp = contract_xp
	contract_xp = contract_xp + math.round( contract_xp*xp_multiplier )
	risk_dissect = math.round( level_limit_dissect*managers.experience:get_contract_difficulty_multiplier( difficulty_stars ) )
	level_limit_dissect = math.round( level_limit_dissect + risk_dissect )
	
	
	
	if is_level_limited then
		local level_limit_tweak_data = tweak_data.experience_manager.level_limit
		
		if managers.experience:current_level() <= level_limit_tweak_data.low_cap_level then
			contract_xp = contract_xp + contract_xp * level_limit_tweak_data.low_cap_multiplier
			contract_xp = math.round( math.min( contract_xp, level_limit_dissect ) )
		else
			local diff_in_experience = level_limit_dissect - contract_xp
			local diff_in_stars = job_and_difficulty_stars - player_stars
			
			local tweak_multiplier = level_limit_tweak_data.pc_difference_multipliers[diff_in_stars] or 0
			
			contract_xp = contract_xp + diff_in_experience * tweak_multiplier
			contract_xp = math.round( math.min( contract_xp, level_limit_dissect ) )
		end
	end
	level_limit_dissect = contract_xp - level_limit_dissect
	
	
	
	if not success then
		failed_level_dissect = contract_xp
		contract_xp = math.round( contract_xp * (tweak_data.experience_manager.stage_failed_multiplier or 1) )
		failed_level_dissect = contract_xp - failed_level_dissect
	end
	total_xp = contract_xp
	
	if success then
		local num_players_bonus = (num_winners and tweak_data.experience_manager.alive_humans_multiplier[ num_winners ] or 1)
		
		alive_crew_dissect = math.round( contract_xp * num_players_bonus - contract_xp )
		-- contract_xp = math.round( contract_xp * num_players_bonus )
		-- alive_crew_dissect = contract_xp - alive_crew_dissect
		total_xp = total_xp + alive_crew_dissect
	end
	
	------------- SKILLS
	local multiplier = managers.player:upgrade_value( "player", "xp_multiplier", 1 )
	multiplier = multiplier * managers.player:upgrade_value( "player", "passive_xp_multiplier", 1 )
	multiplier = multiplier * managers.player:team_upgrade_value( "xp", "multiplier", 1 )
	
	skill_dissect = math.round( contract_xp * multiplier - contract_xp )
	-- contract_xp = 
	-- skill_dissect = contract_xp - skill_dissect
	total_xp = total_xp + skill_dissect
	
	
	------------- DAYS
	days_dissect = math.round( contract_xp * days_multiplier - contract_xp )
	total_xp = total_xp + days_dissect
	
	
	
	local dissection_table = {
		bonus_risk = math.round(risk_dissect),
		bonus_num_players = math.round(alive_crew_dissect),
		bonus_failed = math.round(failed_level_dissect),
		bonus_low_level = math.round(level_limit_dissect),
		bonus_skill = math.round(skill_dissect),
		bonus_days = math.round(days_dissect),
		
		stage_xp = math.round(stage_xp_dissect),
		job_xp = math.round(job_xp_dissect),
		base = math.round(base_xp),
		total = math.round(total_xp),
		last_stage = success and has_active_job and managers.job:on_last_stage(),
	}
	
	if Application:production_build() then
		local rounding_error = dissection_table.total - ( dissection_table.stage_xp + dissection_table.job_xp + dissection_table.bonus_risk + dissection_table.bonus_num_players + dissection_table.bonus_failed + dissection_table.bonus_low_level + dissection_table.bonus_skill + dissection_table.bonus_days )
		dissection_table.rounding_error = rounding_error
	else
		dissection_table.rounding_error = 0
	end
	
	return math.round(total_xp), dissection_table
end

function MissionEndState:_get_contract_xp( success )
	local has_active_job = managers.job:has_active_job()
	local job_and_difficulty_stars = has_active_job and managers.job:current_job_and_difficulty_stars() or 1
	local job_stars = has_active_job and managers.job:current_job_stars() or 1
	local difficulty_stars = job_and_difficulty_stars - job_stars
	local player_stars = managers.experience:level_to_stars()
	
	-- print( "job_and_difficulty_stars, player_stars", job_and_difficulty_stars, player_stars + 1 ) 
	local total_stars = math.min( job_and_difficulty_stars, player_stars + 1 )
		if total_stars < job_and_difficulty_stars then
			self._bonuses[5] = true
		end
	
	local total_difficulty_stars = math.max( 0, total_stars - job_stars )
	local xp_multiplier = managers.experience:get_contract_difficulty_multiplier( total_difficulty_stars )
	self._bonuses[1] = difficulty_stars > 0 and difficulty_stars or false
	
	total_stars = math.min( job_stars, total_stars )
	-- print( "total_stars", total_stars )
	
	-- print( "total_difficulty_stars, xp_multiplier", total_difficulty_stars, xp_multiplier )
		self._bonuses[3] = has_active_job and managers.job:on_last_stage() or false
	local contract_xp = 0
	if success and has_active_job and managers.job:on_last_stage() then
		contract_xp = contract_xp + managers.experience:get_job_xp_by_stars( total_stars )
		-- print( "AWARDED JOB COMPLETED EXPERIENCE", contract_xp )
	else
		contract_xp = contract_xp + managers.experience:get_stage_xp_by_stars( total_stars )
		-- print( "AWARDED STAGE COMPLETED EXPERIENCE", contract_xp )
	end
	
	contract_xp = contract_xp + (contract_xp*xp_multiplier)
	contract_xp = contract_xp * (not success and tweak_data.experience_manager.stage_failed_multiplier or 1) 
		if not success then
			self._bonuses[4] = true
		end
	-- print( "contract_xp", contract_xp )
	return contract_xp
end

function MissionEndState:set_continue_button_text()
	if self._completion_bonus_done then
		self:_set_continue_button_text()
	end
end

function MissionEndState:_set_continue_button_text()
	local text_id = "failed_disconnected_continue"
	local not_clickable = false
	if self._continue_block_timer and self._continue_block_timer > Application:time() then
		text_id = "menu_es_calculating_experience"
		not_clickable = true
	elseif managers.job:stage_success() and managers.job:on_last_stage() then
		text_id = "menu_victory_goto_payday"
	end
	
	local text = utf8.to_upper( managers.localization:text( text_id, { CONTINUE = managers.localization:btn_macro( "continue" ) } ) )
	
	
	managers.menu_component:set_endscreen_continue_button_text( text, not_clickable )
	-- managers.hud:set_continue_button_text_endscreen_hud( text )
	-- managers.hud:script( self.GUI_SAFERECT ):set_continue_button_text( text )
end

function MissionEndState:play_finishing_sound( success )
	if self._server_left then
		return
	end

	if managers.groupai:state():bain_state() then
		-- managers.dialog:queue_dialog( success and "Play_ban_g02x" or "Play_ban_g01x", {} )
	else
		-- managers.dialog:queue_dialog( success and "hos_03" or "hos_04", {} )
	end
end


function MissionEndState:completion_bonus_done( total_xp_bonus )
	-- managers.hud:script( self.GUI_SAFERECT ).bonus_panel:stop()
	self._total_xp_bonus = total_xp_bonus
	self._completion_bonus_done = false
	--[[if total_xp_bonus > 0 then
		managers.experience:add_points( total_xp_bonus, true )
	end
	self._completion_bonus_done = true
	managers.hud:script( self.GUI_SAFERECT ):show_legends()]]
end

function MissionEndState:at_exit( next_state )
	-- managers.hud:hide( self.GUI_SAFERECT )
	-- managers.hud:hide( self.GUI_FULLSCREEN )
	managers.briefing:stop_event( true )
	
	managers.hud:hide( self.GUI_ENDSCREEN )
	managers.menu_component:hide_game_chat_gui()
	
	
	self:_clear_controller()
	if not self._debug_continue and not Application:editor() then
		-- Should go to menu or load next chapter here
		
	-- managers.statistics:stop_session()
		managers.savefile:save_progress()
		
		if( Network:multiplayer() ) then
			self:_shut_down_network()
			-- managers.network:save()
			-- Global.load_to_lobby = true
			-- managers.network:prepare_stop_network()
			
			-- Network:set_multiplayer( false )
			-- managers.network:queue_stop_network()
		end
		
	-- self._sound_source:post_event( "menu_exit" )
	-- managers.menu:close_menu( "menu_pause" )
	
		local player = managers.player:player_unit()
		if player then
			player:camera():remove_sound_listener()
		end
		
		if self._sound_listener then
			self._sound_listener:delete()
			self._sound_listener = nil
		end
				
		if next_state:name() ~= "disconnected" then
			self:_load_start_menu( next_state )
			-- setup:load_start_menu()
		end
	else
		self._debug_continue = nil
		
		managers.groupai:state():set_AI_enabled( true )
		
		local player = managers.player:player_unit()
		if player then
			player:character_damage():set_invulnerable( false )
		end
	end
	
	managers.menu:close_menu( "mission_end_menu" )
end

-- Ovveridden in victory and gameover state to prevent from happening
function MissionEndState:_shut_down_network()
	Network:set_multiplayer( false )
	managers.network:queue_stop_network()
	managers.network.matchmake:destroy_game()
	managers.network.voice_chat:destroy_voice()
end

-- Ovveridden in victory and gameover state to prevent from happening
function MissionEndState:_load_start_menu( next_state )
	if next_state:name() == "disconnected" then
		return
	end
	
	if managers.dlc:is_trial() then
		Global.open_trial_buy = true
	end
	
	managers.job:deactivate_current_job()
	setup:load_start_menu()
end

function MissionEndState:on_statistics_result( 	best_kills_peer_id, best_kills_score, 
												best_special_kills_peer_id, best_special_kills_score, 
												best_accuracy_peer_id, best_accuracy_score, 
												most_downs_peer_id, most_downs_score, 
												total_kills, total_specials_kills, total_head_shots, group_accuracy, group_downs )
	
	print( "on_statistics_result begin" )
	if managers.network and managers.network:session() and managers.network:session():peer( best_kills_peer_id ) then
		local best_kills = managers.network:session():peer( best_kills_peer_id ):name()
		local best_special_kills = managers.network:session():peer( best_special_kills_peer_id ):name()
		local best_accuracy = managers.network:session():peer( best_accuracy_peer_id ):name()
		local most_downs = managers.network:session():peer( most_downs_peer_id ):name()
		-- managers.hud:script( self.GUI_SAFERECT ):
		
		local stage_cash_summary_string
		if self._success then
			local stage_payout, job_payout, bag_payout, small_loot_payout, crew_payout = managers.money:get_payouts()
			
		-- 	local stage_payout = 
			local bonus_bags = managers.loot:get_secured_bonus_bags_amount() + managers.loot:get_secured_mandatory_bags_amount()
			local bag_cash = bag_payout -- managers.loot:get_secured_bonus_bags_amount()
			local loose_cash = small_loot_payout or 0 -- managers.loot:get_real_total_small_loot_value() or 0
			local cleaner_cost = 0 -- math.min(managers.money:get_civilian_deduction() * (managers.statistics:session_total_civilian_kills() or 0), managers.money:total() - managers.money:heist_spending())
			local assets_cost =  0 -- managers.assets:get_money_spent()
			
			local current_total_money = managers.money:total()
			-- local total_cash = managers.money:heist_total()
			
			if job_payout > 0 then
				local job_string = managers.localization:text( "victory_stage_cash_summary_name_job", {stage_cash=managers.experience:cash_string(stage_payout), job_cash=managers.experience:cash_string(job_payout)} )
				stage_cash_summary_string = job_string
			else
				local stage_string = managers.localization:text( "victory_stage_cash_summary_name", {stage_cash=managers.experience:cash_string(stage_payout)} )
				stage_cash_summary_string = stage_string
			end
			
			
			if bonus_bags > 0 and bag_cash > 0 then
				stage_cash_summary_string = stage_cash_summary_string .. " " .. managers.localization:text( "victory_stage_cash_summary_name_bags", {bag_cash=managers.experience:cash_string(bag_cash), bag_amount=bonus_bags, bonus_bags=bonus_bags} )
			end
			
			if self._criminals_completed and crew_payout > 0 then
				stage_cash_summary_string = stage_cash_summary_string .. " " .. managers.localization:text( "victory_stage_cash_summary_name_crew", {winners=tostring(self._criminals_completed), crew_cash=managers.experience:cash_string(crew_payout)} )
			end
			
			if loose_cash > 0 then
				stage_cash_summary_string = stage_cash_summary_string .. " " .. managers.localization:text( "victory_stage_cash_summary_name_loose", {loose_cash=managers.experience:cash_string(loose_cash)} )
			end
			
			
			stage_cash_summary_string = stage_cash_summary_string .. "\n"
			
			if cleaner_cost > 0 then
				stage_cash_summary_string = stage_cash_summary_string .. managers.localization:text( "victory_stage_cash_summary_name_civ_kill", {civ_killed_cash=managers.experience:cash_string(cleaner_cost)} ) .. " "
			end
			
			if assets_cost > 0 then
				stage_cash_summary_string = stage_cash_summary_string .. managers.localization:text( "victory_stage_cash_summary_name_assets", {asset_cash=managers.experience:cash_string(assets_cost)} ) .. " "
			end
			
			if cleaner_cost > 0 or assets_cost > 0 then
				stage_cash_summary_string = stage_cash_summary_string .. "\n"
			end
			
			stage_cash_summary_string = stage_cash_summary_string .. "\n"
			
			local offshore_string = managers.localization:text( "victory_stage_cash_summary_name_offshore", {offshore=managers.localization:text("hud_offshore_account"), cash=managers.experience:cash_string(managers.money:heist_offshore())} )
			local spending_string = managers.localization:text( "victory_stage_cash_summary_name_spending", {cash="##"..managers.experience:cash_string(managers.money:heist_spending()).."##"} )
			-- local total_string = managers.localization:text( "victory_stage_cash_summary_name_total", {total_cash=managers.experience:cash_string(total_cash)} )
			
			stage_cash_summary_string = stage_cash_summary_string .. offshore_string .. "\n"
			stage_cash_summary_string = stage_cash_summary_string .. spending_string .. "\n"
			-- stage_cash_summary_string = stage_cash_summary_string .. total_string
		else
			stage_cash_summary_string = managers.localization:text( "failed_summary_name" )
		end
		
		self._statistics_data = {
			best_killer=managers.localization:text( "victory_best_killer_name", { PLAYER_NAME=best_kills, SCORE=best_kills_score } ),
			-- best_kills_score=best_kills_score,
			best_special=managers.localization:text( "victory_best_special_name", { PLAYER_NAME=best_special_kills, SCORE=best_special_kills_score } ),
			-- best_special_kills_score=best_special_kills_score, 
			best_accuracy=managers.localization:text( "victory_best_accuracy_name", { PLAYER_NAME=best_accuracy, SCORE=best_accuracy_score } ), 
			-- best_accuracy_score=best_accuracy_score, 
			most_downs=managers.localization:text( "victory_most_downs_name", { PLAYER_NAME=most_downs, SCORE=most_downs_score } ), 
			-- most_downs_score=most_downs_score, 
			total_kills=total_kills, 
			total_specials_kills=total_specials_kills, 
			total_head_shots=total_head_shots, 
			group_hit_accuracy=group_accuracy.."%", 
			group_total_downed=group_downs,
			
			stage_cash_summary = stage_cash_summary_string
		}
		--[[
		managers.hud:set_group_statistics_endscreen_hud( 	best_kills, best_kills_score,
																		best_special_kills, best_special_kills_score, 
																		best_accuracy, best_accuracy_score, 
																		most_downs, most_downs_score, 
																		total_kills, total_specials_kills, total_head_shots, group_accuracy, group_downs )
																		]]
	end
	print( "on_statistics_result end" )
																	
	if Network:is_server() and self._success then
		if not managers.achievment:get_script_data( "cant_touch_fail" ) and
		   tweak_data:difficulty_to_index( Global.game_settings.difficulty ) >= 4 and
		   Global.level_data.level_id == "heat_street" and group_accuracy >= 60 then
			managers.challenges:set_flag( "cant_touch" )
			managers.network:session():send_to_peers( "award_achievment", "cant_touch" )
		end
	end
end

function MissionEndState:_continue_blocked()
	local in_focus = managers.menu:active_menu() == self._mission_end_menu
	if not in_focus then
		return true
	end
	
	if managers.hud:showing_stats_screen() then
		return true
	end
	
	if managers.system_menu:is_active() then
		return true
	end
	
	if not self._completion_bonus_done then
		return true
	end
	
	if managers.menu_component:input_focus() == 1 then
		return true
	end
	
	if self._continue_block_timer and self._continue_block_timer > Application:time() then
		return true
	end
	
	return false
end

function MissionEndState:_continue()
	self:continue()
end

function MissionEndState:continue()
	if self:_continue_blocked() then
		return
	end
	--[[if managers.system_menu:is_active() then
		return
	end
	
	if not self._completion_bonus_done then
		return
	end]]
	
	if managers.job:stage_success() and managers.job:on_last_stage() then
		Application:debug( managers.job:stage_success() , managers.job:on_last_stage(), managers.job:is_job_finished() )
		self:_clear_controller()
		managers.menu_component:close_stage_endscreen_gui()
		self:gsm():change_state_by_name( "ingame_lobby_menu" )
	elseif( self._old_state ) then
		self:_clear_controller()
		self:gsm():change_state_by_name( "empty" )
	else
		Application:error( "Trying to continue from victory screen, but I have no state to goto" )
	end
end

function MissionEndState:_clear_controller()
	if not self._controller then
		return
	end
	
	if( Network:is_server() ) then
		-- self._controller:remove_trigger( "continue", self._continue_cb )
	end
	-- self._controller:remove_trigger( "debug_continue", self._debug_continue_cb )
	self._controller:set_enabled( false )
	self._controller:destroy()
	self._controller = nil
end

function MissionEndState:debug_continue()
	if not self._success then
		return
	end
	if not self._completion_bonus_done then
		return
	end
	if( self._old_state ) then
		self._debug_continue = true
		self:_clear_controller()
		self:gsm():change_state_by_name( self._old_state:name() )
	end
end

function MissionEndState:set_completion_bonus_done( done )
	self._completion_bonus_done = done
	self:_set_continue_button_text()
end

function MissionEndState:update( t, dt )
	-- managers.hud:script( self.GUI_SAFERECT ):update( t, dt )
	managers.hud:update_endscreen_hud( t, dt )
	
	if self._mission_destroy_t and self._mission_destroy_t <= Application:time() then
		Application:debug( "managers.mission:pre_destroy()" )
		managers.mission:pre_destroy()
		self._mission_destroy_t = nil
	end
	
	if self._total_xp_bonus then
		if self._total_xp_bonus > 0 then
			-- THIS IS THE ONLY PLACE ALLOWED TO GIVE EXP IN PAYDAY2
			local data = managers.experience:give_experience( self._total_xp_bonus ) -- add_points( self._total_xp_bonus, true )
			data.bonuses = self._bonuses
			managers.hud:send_xp_data_endscreen_hud( data, callback( self, self, "set_completion_bonus_done" ) )
			
			if SystemInfo:platform() == Idstring( "WIN32" ) and Global.level_data.level_id then
				local stats = {}
				stats[ Global.game_settings.difficulty .. "_" .. Global.level_data.level_id .. "_" .. "exp" ] = { type = "int", value = self._total_xp_bonus }
				managers.network.account:publish_statistics( stats )
			end
		else
			self:set_completion_bonus_done( true )
		end
		self._total_xp_bonus = nil
		-- self._completion_bonus_done = true
		-- managers.hud:script( self.GUI_SAFERECT ):show_legends()
	end
	
	if self._continue_block_timer and self._continue_block_timer <= t then
		self._continue_block_timer = nil
		self:_set_continue_button_text()
	end
	
	local in_focus = managers.menu:active_menu() == self._mission_end_menu
	
	if( in_focus and not self._in_focus ) then		-- when the menu component is created or recreated
		self:_set_continue_button_text()
		self._statistics_feeded = nil
	end
	
	if( not self._statistics_feeded and self._statistics_data ) then
		self._statistics_data.success = self._success
		self._statistics_data.criminals_finished = self._criminals_completed
		managers.menu_component:feed_endscreen_statistics( self._statistics_data )
		self._statistics_feeded = true
	end
	self._in_focus = in_focus
	
	--[[if self._controller and in_focus then
		local btn_stats_screen_press = not self._stats_screen and self._controller:get_input_bool( "stats_screen" )
		local btn_stats_screen_release = self._stats_screen and self._controller:get_input_released( "stats_screen" )
						
		if btn_stats_screen_press then
			self._stats_screen = true
			self._mission_end_menu.input._accept_input = false
			managers.hud:show_stats_screen()
		elseif btn_stats_screen_release then
			self._stats_screen = false
			self._mission_end_menu.input._accept_input = true
			managers.hud:hide_stats_screen()
		end
	elseif self._stats_screen then
		self._stats_screen = false
		managers.hud:hide_stats_screen()
		if self._controller and not in_focus then
			self._mission_end_menu.input._accept_input = true
		end
	end]]
end

function MissionEndState:game_ended( )
	return true
end

function MissionEndState:on_server_left()
	IngameCleanState.on_server_left( self )
end

function MissionEndState:on_kicked()
	IngameCleanState.on_kicked( self )
end

function MissionEndState:on_disconnected()
	IngameCleanState.on_disconnected( self )
end