-----------------------------------------------------------------------------
-- ************* GUARD LOGIC *******************
-----------------------------------------------------------------------------

CopLogicGuard = class( CopLogicIdle )

function CopLogicGuard.enter( data, new_logic_name, enter_params )
	CopLogicBase.enter( data, new_logic_name, enter_params )
	--print( "CopLogicGuard.enter" )
	data.unit:brain():cancel_all_pathing_searches()
	
	local objective = data.objective
	local guard_obj = objective.guard_obj
		
	local my_data = { unit = data.unit }
	my_data.detection = data.char_tweak.detection.guard
	
	my_data.rsrv_pos = {}
	
	my_data.guard_obj = guard_obj
	if guard_obj and guard_obj.type == "door" then
		CopLogicAttack._set_attention_on_pos( data, guard_obj.door.center + math.UP * 140 )
	else
		CopLogicBase._reset_attention( data )
	end
	
	if managers.groupai:state():is_nav_seg_safe( guard_obj.from_seg ) then
		--print( "GUARDED AREA WAS SAFE ON ENTRY" )
		my_data.from_seg_safe = true
	end
	
	local old_internal_data = data.internal_data
	if old_internal_data then--	Stuff inheritted from the previous state
		my_data.rsrv_pos = old_internal_data.rsrv_pos or my_data.rsrv_pos
			
		if old_internal_data.best_cover then	-- maintain the old cover
			my_data.best_cover = old_internal_data.best_cover
			managers.navigation:reserve_cover( my_data.best_cover[1], data.pos_rsrv_id ) -- logics unregister their covers on exit. we need to re-reserve.
		end
		if old_internal_data.nearest_cover then	-- maintain the old cover
			my_data.nearest_cover = old_internal_data.nearest_cover
			managers.navigation:reserve_cover( my_data.nearest_cover[1], data.pos_rsrv_id ) -- logics unregister their covers on exit. we need to re-reserve.
		end
	end
	
	my_data.need_turn_check = true	-- check if we need to turn to face the guard object
	--my_data.from_seg_safe = nil	-- check if the area we are guarding against is safe
	--my_data.aiming = nil	-- we are performing a shoot action without actualy firing... need to fix the name
	--my_data.turning = nil	-- we are performing a turn action
	
	data.internal_data = my_data
	
	my_data.detection_task_key = "CopLogicGuard._upd_enemy_detection"..tostring( data.unit:key() )
	CopLogicBase.queue_task( my_data, my_data.detection_task_key, CopLogicIdle._upd_enemy_detection, data, data.t + 1 )
	
	CopLogicTravel.reset_actions( data, my_data, old_internal_data, CopLogicTravel.allowed_transitional_actions)
	
	data.unit:movement():set_cool( false )
	if my_data ~= data.internal_data then
		return
	end
	data.unit:brain():set_attention_settings( { cbt = true } )
end

-----------------------------------------------------------------------------

function CopLogicGuard.update( data )
	local my_data = data.internal_data
	local guard_obj = my_data.guard_obj
	
	if my_data.need_turn_check then
		if not ( my_data.turning or data.unit:movement():chk_action_forbidden( "walk" ) ) then
			my_data.need_turn_check = CopLogicAttack._chk_request_action_turn_to_enemy( data, my_data, data.m_pos, guard_obj.door.center )
		end
	end
	
	if guard_obj.type == "door" then
		if not ( my_data.aiming or my_data.from_seg_safe ) then
			local shoot_action = {}
			shoot_action.type = "shoot"
			shoot_action.body_part = 3
			CopLogicAttack._set_attention_on_pos( data, guard_obj.door.center + math.UP * 140 )
			if data.unit:brain():action_request( shoot_action ) then
				my_data.aiming = true
			end
		end
	end

	if my_data.from_seg_safe and not ( my_data.need_turn_check or my_data.turning ) then
		CopLogicBase._exit( data.unit, "idle" )
	end
	
end

-----------------------------------------------------------------------------

function CopLogicGuard.action_complete_clbk( data, action )
	--print( "CopLogicGuard.action_complete_clbk", action:type() )
	local action_type = action:type()
	if action_type == "shoot" then
		data.internal_data.shooting = nil
	elseif action_type == "turn" then
		data.internal_data.turning = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicGuard.on_new_objective( data, old_objective )
	CopLogicIdle.on_new_objective( data, old_objective )
end

-----------------------------------------------------------------------------
-- Callback received from enemy manager when the safety status of an area changes
function CopLogicGuard.on_area_safety( data, nav_seg, safe, event )
	local objective = data.objective
	if objective.guard_obj.from_seg == nav_seg then	-- The area accross the door we are guarding is now safe. stop aiming
		local my_data = data.internal_data
		if safe then
			--print( "GUARDED AREA IS NOW SAFE" )
			if my_data.need_turn_check or my_data.turning then -- do not exit before we face the guard object. even if it is safe
				my_data.from_seg_safe = true
			else
				CopLogicBase._exit( data.unit, "idle" )
			end
			managers.groupai:state():on_objective_complete( data.unit, objective )
			--print( "new_objective", data.objective and data.objective.type )
		else
			my_data.from_seg_safe = nil
		end
	elseif nav_seg == data.unit:movement():nav_tracker():nav_segment() then
		if not safe then
			CopLogicBase._exit( data.unit, "attack" )
		end
	else
		if event.reason == "criminal" then
			local new_occupation = managers.groupai:state():verify_occupation_in_area( objective )
			if new_occupation then
				--print( "changing guard door" )
				new_occupation.type = "guard"
				local new_objective = { type = "investigate_area", nav_seg = objective.nav_seg, status = "in_progress", guard_obj = new_occupation, scan = true }
				data.unit:brain():set_objective( new_objective )
			end
		end
	end
end

-----------------------------------------------------------------------------
-----------------------------------------------------------------------------



