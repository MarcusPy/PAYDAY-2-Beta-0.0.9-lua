-----------------------------------------------------------------------------
-- ************* INTIMIDATED LOGIC *******************
-----------------------------------------------------------------------------

local tmp_vec1 = Vector3()


CopLogicIntimidated = class( CopLogicBase )

function CopLogicIntimidated.enter( data, new_logic_name, enter_params )
	CopLogicBase.enter( data, new_logic_name, enter_params )
	--print( "CopLogicIntimidated.enter" )
	data.unit:brain():cancel_all_pathing_searches()
	
	local old_internal_data = data.internal_data
	local my_data = { unit = data.unit }
	data.internal_data = my_data
	
	my_data.detection = data.char_tweak.detection.combat
	
	my_data.rsrv_pos = {}
	
	if data.attention_obj then
		CopLogicBase._set_attention_obj( data, nil, nil )
	end
	
	if old_internal_data then --	Stuff inheritted from the previous state
		my_data.rsrv_pos = old_internal_data.rsrv_pos or my_data.rsrv_pos
		
		if old_internal_data.nearest_cover then	-- maintain the old cover
			my_data.nearest_cover = old_internal_data.nearest_cover
			managers.navigation:reserve_cover( my_data.nearest_cover[1], data.pos_rsrv_id ) -- logics unregister their covers on exit. we need to re-reserve.
		end
	end
	
	my_data.surrender_break_t = data.char_tweak.surrender_break_time and data.t + math.random( data.char_tweak.surrender_break_time[1], data.char_tweak.surrender_break_time[2], math.random() )
	
	if data.unit:anim_data().hands_tied then
		CopLogicIntimidated._do_tied( data, nil )
	else
		data.unit:brain():set_update_enabled_state( true )
	end
	
	data.unit:movement():set_allow_fire( false )
	
	if data.objective then	-- just in case
		managers.groupai:state():on_objective_failed( data.unit, data.objective )
	end
	
	if managers.groupai:state():rescue_state() then
		CopLogicIntimidated._add_delayed_rescue_SO( data, my_data )
	end
	
	managers.groupai:state():add_to_surrendered( data.unit, callback( CopLogicIntimidated, CopLogicIntimidated, "queued_update", data ) )
	my_data.surrender_clbk_registered = true
	data.unit:sound():say( "s01x", true )
	
	data.unit:movement():set_cool( false )
	if my_data ~= data.internal_data then
		return
	end
	
	data.unit:brain():set_attention_settings( { peaceful = true } )
	
	managers.groupai:state():register_rescueable_hostage( data.unit, nil )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.exit( data, new_logic_name, enter_params )
	CopLogicBase.exit( data, new_logic_name, enter_params )
	
	local my_data = data.internal_data
	CopLogicIntimidated._unregister_rescue_SO( data, my_data )
	
	if new_logic_name ~= "inactive" then
		data.unit:base():set_slot( data.unit, 12 )	-- Enemy
	end
	
	
	if my_data.nearest_cover then	-- Do not delete this value. The new logic might want to re-reserve it
		managers.navigation:release_cover( my_data.nearest_cover[1] )
	end
	
	if new_logic_name ~= "inactive" then
		data.unit:brain():set_update_enabled_state( true )
	end
	
	if my_data.tied then
		managers.groupai:state():on_enemy_untied( data.unit:key() )
	end
	
	managers.groupai:state():unregister_rescueable_hostage( data.key )
	
	CopLogicIntimidated._unregister_harassment_SO( data, my_data )
	
	-- This breaks so cops cannot be traded by clients
--	if data.unit:interaction() then
--		data.unit:interaction():set_active( false, true )
--	end
	
	if my_data.surrender_clbk_registered then
		managers.groupai:state():remove_from_surrendered( data.unit )
	end
	
	if my_data.is_hostage then
		managers.groupai:state():on_hostage_state( false, data.key, true )
	end
	
	data.unit:interaction():set_active( false, true, false )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.death_clbk( data, damage_info )
	CopLogicIntimidated.super.death_clbk( data, damage_info )

--	managers.groupai:state():hostage_killed( damage_info.attacker_unit )
end

-----------------------------------------------------------------------------
--	This function is queued in the enemy manager
function CopLogicIntimidated.queued_update( rubbish, data )
	--print( "CopLogicIntimidated.queued_update", data.unit:name(), data.unit:key() )
	local my_data = data.internal_data
	
	CopLogicIntimidated._update_enemy_detection( data, my_data )
	
	if my_data ~= data.internal_data then
		return
	end
	
	managers.groupai:state():add_to_surrendered( data.unit, callback( CopLogicIntimidated, CopLogicIntimidated, "queued_update", data ) ) -- this task gets removed by default. we need to add it every time
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._update_enemy_detection( data, my_data )
	local robbers = managers.groupai:state():all_criminals()
	local my_tracker = data.unit:movement():nav_tracker()
	local chk_vis_func = my_tracker.check_visibility
	
	local fight = not my_data.tied
	
	if not my_data.surrender_break_t or data.t < my_data.surrender_break_t then
		for u_key, u_data in pairs( robbers ) do
			if not u_data.is_deployable and chk_vis_func( my_tracker, u_data.tracker ) then
				local crim_unit = u_data.unit
				local crim_pos = u_data.m_pos
				local dis = mvector3.direction( tmp_vec1, data.m_pos, crim_pos )
				if dis < tweak_data.player.long_dis_interaction.intimidate_range_enemies * tweak_data.upgrades.values.player.intimidate_range_mul[1] * 1.05 then -- he is close
					local crim_fwd = crim_unit:movement():m_head_rot():y()
					mvector3.set_z( crim_fwd, 0 )
					mvector3.normalize( crim_fwd )
					if mvector3.dot( crim_fwd, tmp_vec1 ) < -0.2 then -- he has not turned his back on me
						local vis_ray = World:raycast( "ray", data.unit:movement():m_head_pos(), u_data.m_det_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision", "report" )
						if not vis_ray then -- he has line of sight to me
							fight = nil
							break
						end
					end
				end
			end
		end
	end
	
	if fight then
		my_data.surrender_clbk_registered = nil
		
		local new_action = { type = "act", variant = "idle", body_part = 1 }
		data.unit:brain():action_request( new_action )
		data.unit:brain():set_logic( "idle" )
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.action_complete_clbk( data, action )
	local my_data = data.internal_data
	local action_type = action:type()
	if action_type == "act" then
		if my_data.being_harassed then
			my_data.being_harassed = nil
			
			CopLogicIntimidated._add_delayed_rescue_SO( data, my_data )
		elseif my_data.act_action then
			my_data.act_action = nil
		end
	end
	data.unit:brain():set_update_enabled_state( true )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.update( data )
	if data.unit:anim_data().surrender then
		return -- we are already in a surrender anim. all good
	end
	
	if not data.unit:movement():chk_action_forbidden( "walk" ) then
		CopLogicIntimidated._start_action_hands_up( data )
		data.unit:brain():set_update_enabled_state( false )
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.can_activate()
	return false
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_intimidated( data, amount, aggressor_unit ) 
	local my_data = data.internal_data
	
	if not my_data.tied then
		
		my_data.surrender_break_t = data.char_tweak.surrender_break_time and data.t + math.random( data.char_tweak.surrender_break_time[1], data.char_tweak.surrender_break_time[2], math.random() )
		
		local anim_data = data.unit:anim_data()
		local anim, blocks
		if anim_data.hands_up then
			anim = "hands_back"
			blocks = { light_hurt = -1, hurt = -1, heavy_hurt = -1, action = -1, walk = -1 }
		elseif anim_data.hands_back then
			anim = "tied"
			blocks = { light_hurt = -1, hurt = -1, heavy_hurt = -1, hurt_sick = -1, action = -1, walk = -1 }
		else
			anim = "hands_up"
			blocks = { light_hurt = -1, hurt = -1, heavy_hurt = -1, action = -1, walk = -1 }
		end
		local action_data = { type = "act", body_part = 1, variant = anim, clamp_to_graph = true, blocks = blocks }
		local act_action = data.unit:brain():action_request( action_data )
		
		--[[if act_action and anim == "tied" and ( not managers.groupai:state():occasional_event_info( "cop_harassment" ) or math.random() < 1 ) then
			CopLogicIntimidated._register_harassment_SO( data, my_data )
		end]]
		
		--[[
		if data.unit:anim_data().hands_back then
			data.unit:interaction():set_active( true, true )
		end
		]]
		if data.unit:anim_data().hands_tied then
			CopLogicIntimidated._do_tied( data, aggressor_unit )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._register_harassment_SO( data, my_data )
	local objective_pos = data.unit:position() - data.unit:rotation():y() * 100
	local objective_rot = data.unit:rotation()
	local objective = { 
		type = "act",
		pos = objective_pos,
		rot = objective_rot,
		interrupt_dis = 700,
		interrupt_health = 0.85,
		stance = "hos",
		scan = true,
		nav_seg = data.unit:movement():nav_tracker():nav_segment(),
		action_start_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "on_harassment_SO_action_start", data ),
		fail_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "on_harassment_SO_failed", data ),
		action = { 
			type = "act",
			variant = "kick_fwd",
			body_part = 1,
			blocks = {
				action = -1,
				walk = -1
			}
		}
	}
	
	local so_descriptor = { 
		objective = objective,
		base_chance = 1,
		chance_inc = 0,
		interval = 10,
		search_dis_sq = 1500*1500,
		search_pos = mvector3.copy( data.m_pos ),
		usage_amount = 1,
		AI_group = "friendlies",
		admin_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "on_harassment_SO_administered", data )
	}
	
	local so_id = "harass"..tostring( data.unit:key() )
	my_data.harassment_SO_id = so_id
	managers.groupai:state():add_special_objective( so_id, so_descriptor )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_harassment_SO_administered( ignore_this, data, receiver_unit )
	local my_data = data.internal_data
	my_data.harassment_SO_id = nil
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_harassment_SO_action_start( ignore_this, data, receiver_unit )
	local my_data = data.internal_data
	local action = { 
		type = "act",
		variant = "harassed_kicked_from_behind",
		body_part = 1,
		blocks = {
			action = -1,
			walk = -1,
			light_hurt = -1,
			hurt = -1,
			heavy_hurt = -1
		}
	}
	my_data.being_harassed = data.unit:movement():action_request( action )
	managers.groupai:state():on_occasional_event( "cop_harassment" )
	
	CopLogicIntimidated._unregister_rescue_SO( data, my_data )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_harassment_SO_failed( ignore_this, data, receiver_unit )
	local my_data = data.internal_data
	if my_data.being_harassed then
		local action_data = { type = "act", body_part = 1, variant = "tied", blocks = { light_hurt = -1, hurt = -1, heavy_hurt = -1, walk = -1 } }
		data.unit:brain():action_request( action_data )
		my_data.being_harassed = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._unregister_harassment_SO( data, my_data )
	local my_data = data.internal_data
	if my_data.harassment_SO_id then
		managers.groupai:state():remove_special_objective( my_data.harassment_SO_id )
		my_data.harassment_SO_id = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._do_tied( data, aggressor_unit )
	local my_data = data.internal_data

	if my_data.surrender_clbk_registered then
		managers.groupai:state():remove_from_surrendered( data.unit )
		my_data.surrender_clbk_registered = nil
	end
	my_data.tied = true
	data.unit:inventory():destroy_all_items()
	data.unit:brain():set_update_enabled_state( false )
	
	if my_data.update_task_key then
		managers.enemy:unqueue_task( my_data.update_task_key )
		my_data.update_task_key = nil
	end
	
	local rsrv_pos = my_data.rsrv_pos
	if rsrv_pos.stand then
		managers.navigation:unreserve_pos( rsrv_pos.stand )
		rsrv_pos.stand = nil
	end
	managers.groupai:state():on_enemy_tied( data.unit:key() )
	data.unit:base():set_slot( data.unit, 22 )	--22. Bound Hostages ( civilian and police ) handcuffed players are in slot 4
	
	data.unit:interaction():set_tweak_data( "hostage_convert" )
	data.unit:interaction():set_active( true, true, false )
	
	my_data.is_hostage = true
	managers.groupai:state():on_hostage_state( true, data.key, true )
	
	if aggressor_unit then
		data.unit:character_damage():drop_pickup() -- Steal his things
		data.unit:character_damage():set_pickup( nil )
			
		if aggressor_unit == managers.player:player_unit() then -- Its me, the server
			managers.statistics:tied( { name = data.unit:base()._tweak_table } )
		else -- It was one of the clients
			aggressor_unit:network():send_to_unit( { "statistics_tied", data.unit:base()._tweak_table } )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_detected_enemy_destroyed( data, enemy_unit )
	CopLogicIdle.on_detected_enemy_destroyed( data, enemy_unit )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_criminal_neutralized( data, criminal_key )
	CopLogicIdle.on_criminal_neutralized( data, criminal_key )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_alert( data, alert_data )
	--print( "[CopLogicIntimidated.on_alert]", data.unit, inspect( alert_data ) )
	local alert_unit = alert_data[5]
	
	if alert_unit and alert_unit:in_slot( data.enemy_slotmask ) then
		local att_obj_data, is_new = CopLogicBase.identify_attention_obj_instant( data, alert_unit:key() )
		
		if not att_obj_data then -- not interested in detecting this attention object
			return
		end
		
		local alert_type = alert_data[1]
		
		if alert_type == "bullet" or alert_type == "aggression" then
			att_obj_data.alert_t = TimerManager:game():time()
		end
		
		if att_obj_data.criminal_record then
			managers.groupai:state():criminal_spotted( alert_unit )
			if alert_type == "bullet" or alert_type == "aggression" then
				managers.groupai:state():report_aggression( alert_unit )
			end
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.is_available_for_assignment( data, objective )
	if objective and objective.forced then
		return true
	end
	return false
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._add_delayed_rescue_SO( data, my_data )
--	print( "[CopLogicIntimidated._add_delayed_rescue_SO]", data.unit:key() )
	if data.char_tweak.flee_type == "hide" or (data.unit:unit_data() and data.unit:unit_data().not_rescued) then -- this one does not get rescued
	elseif my_data.delayed_clbks and my_data.delayed_clbks[ my_data.delayed_rescue_SO_id ] then	-- the SO has not been registered with GroupAI yet. reschedule
		--print( "rescheduling" )
		managers.enemy:reschedule_delayed_clbk( my_data.delayed_rescue_SO_id, TimerManager:game():time() + 10 )
	else
		if my_data.rescuer then
			--print( "notifying rescuer" )
			local objective = my_data.rescuer:brain():objective()
			local rescuer = my_data.rescuer
			my_data.rescuer = nil -- to avoid loop-back
			managers.groupai:state():on_objective_failed( rescuer, objective )
			
		elseif my_data.rescue_SO_id then	-- the SO has been registered with GroupAI. remove from GroupAI and schedule new addition time
			--print( "removing SO" )
			managers.groupai:state():remove_special_objective( my_data.rescue_SO_id )
			my_data.rescue_SO_id = nil
		end
		--print( "adding delayed_rescue_SO" )
		my_data.delayed_rescue_SO_id = "rescue"..tostring( data.unit:key() )
		CopLogicBase.add_delayed_clbk( my_data, my_data.delayed_rescue_SO_id, callback( CopLogicIntimidated, CopLogicIntimidated, "register_rescue_SO", data ), TimerManager:game():time() + 10 )
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.register_rescue_SO( ignore_this, data )
--	print( "[CopLogicIntimidated.register_rescue_SO]", data.unit:key() )
	local my_data = data.internal_data
	CopLogicBase.on_delayed_clbk( my_data, my_data.delayed_rescue_SO_id )
	my_data.delayed_rescue_SO_id = nil
	local my_tracker = data.unit:movement():nav_tracker()
	local objective_pos = my_tracker:field_position()
	local followup_objective = {
		type = "act",
		stance = "hos",
		scan = true,
		action = { 
			type = "act",
			variant = "idle",
			body_part = 1,
			blocks = {
				action = -1,
				walk = -1
			}
		},
		action_duration = tweak_data.interaction.free.timer,
	}
	local objective = { 
		type = "act",
		follow_unit = data.unit,
		pos = mvector3.copy( objective_pos ),
		destroy_clbk_key = false,	-- this gets filled later on
		interrupt_dis = 700,
		interrupt_health = 0.85,
		stance = "hos",
		scan = true,
		nav_seg = data.unit:movement():nav_tracker():nav_segment(),
		fail_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "on_rescue_SO_failed", data ),
		complete_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "on_rescue_SO_completed", data ),
		action = { 
			type = "act",
			variant = "untie",
			body_part = 1,
			blocks = {
				action = -1,
				walk = -1
			}
		},
		action_duration = tweak_data.interaction.free.timer,
		followup_objective = followup_objective
	}
	
	local so_descriptor = { 
		objective = objective,
		base_chance = 1,
		chance_inc = 0,
		interval = 10,
		search_dis_sq = 1000*1000,
		search_pos = mvector3.copy( data.m_pos ),
		usage_amount = 1,
		AI_group = "enemies",
		admin_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "on_rescue_SO_administered", data ),
		verification_clbk = callback( CopLogicIntimidated, CopLogicIntimidated, "rescue_SO_verification" )
	}
	
	local so_id = "rescue"..tostring( data.unit:key() )
	my_data.rescue_SO_id = so_id
	managers.groupai:state():add_special_objective( so_id, so_descriptor )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._unregister_rescue_SO( data, my_data )
--	print( "[CopLogicIntimidated._unregister_rescue_SO]", data.key )
	if my_data.rescuer then
--		print( "rescuer notified" )
		local rescuer = my_data.rescuer
		my_data.rescuer = nil
		managers.groupai:state():on_objective_failed( rescuer, rescuer:brain():objective() )
	elseif my_data.rescue_SO_id then
--		print( "SO unregistered" )
		managers.groupai:state():remove_special_objective( my_data.rescue_SO_id )
		my_data.rescue_SO_id = nil
	elseif my_data.delayed_rescue_SO_id then
--		print( "delayed SO removed" )
		CopLogicBase.chk_cancel_delayed_clbk( my_data, my_data.delayed_rescue_SO_id )
	end
end

-----------------------------------------------------------------------------
-- a pig is now comming to our rescue
function CopLogicIntimidated.on_rescue_SO_administered( ignore_this, data, receiver_unit )
--	print( "[CopLogicIntimidated.on_rescue_SO_administered]", data and inspect( data ), data.key, receiver_unit:key() )
	local my_data = data.internal_data
	my_data.rescuer = receiver_unit
	my_data.rescue_SO_id = nil
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.rescue_SO_verification( ignore_this, unit )
--	print( "[CopLogicIntimidated.rescue_SO_verification]", data and inspect( data ), unit)
	return tweak_data.character[ unit:base()._tweak_table ].rescue_hostages
end

-----------------------------------------------------------------------------
-- the pig that was coming to our rescue failed his objective
function CopLogicIntimidated.on_rescue_SO_failed( ignore_this, data )
--	print( "[CopLogicIntimidated.on_rescue_SO_failed]", data and inspect( data ), data.key, data.internal_data.rescuer and data.internal_data.rescuer:key() )
	local my_data = data.internal_data
	if my_data.rescuer then
		my_data.rescuer = nil	-- this pig failed to rescue us. re-register the SO immediately
		my_data.delayed_rescue_SO_id = "rescue"..tostring( data.unit:key() )
		CopLogicBase.add_delayed_clbk( my_data, my_data.delayed_rescue_SO_id, callback( CopLogicIntimidated, CopLogicIntimidated, "register_rescue_SO", data ), TimerManager:game():time() + 10 )
	end
end

-----------------------------------------------------------------------------
-- we have been cleared to fight again
function CopLogicIntimidated.on_rescue_SO_completed( ignore_this, data, good_pig )
--	print( "[CopLogicIntimidated.on_rescue_SO_completed]", data and inspect( data ), data.key, good_pig )
	--Application:stack_dump()


	if not data.unit:inventory():equipped_unit() then
		if data.unit:inventory():num_selections() <= 0 then
			local weap_name = data.unit:base():default_weapon_name()
			if weap_name then
				data.unit:inventory():add_unit_by_name( weap_name, true, true )
			end
		else
			data.unit:inventory():equip_selection(1, true)
		end
	end
	
	if data.unit:anim_data().hands_tied then
		local new_action = { type = "act", variant = "stand", body_part = 1 }
		data.unit:brain():action_request( new_action )
	end
	
	CopLogicBase._exit( data.unit, "idle" )
	
	--good_pig:sound():say( "_h01x_sin", true )
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.on_rescue_allowed_state( data, state )
	if state then
		if not data.unit:anim_data().move then
			CopLogicIntimidated._add_delayed_rescue_SO( data, data.internal_data )
		end
	else
		CopLogicIntimidated._unregister_rescue_SO( data, data.internal_data )
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated.anim_clbk( data, event_type )
	local my_data = data.internal_data
	if event_type == "harass_end" then
		if my_data.being_harassed then
			my_data.being_harassed = nil
			
			CopLogicIntimidated._add_delayed_rescue_SO( data, data.internal_data )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicIntimidated._start_action_hands_up( data )
	local my_data = data.internal_data
	local action_data = { type = "act", body_part = 1, variant = "hands_up", clamp_to_graph = true, blocks = { light_hurt = -1, hurt = -1, heavy_hurt = -1, walk = -1 } }
	my_data.act_action = data.unit:brain():action_request( action_data )
end

-----------------------------------------------------------------------------
-----------------------------------------------------------------------------



