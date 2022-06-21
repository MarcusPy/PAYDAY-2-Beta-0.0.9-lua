-----------------------------------------------------------------------------
-- ************* BASE LOGIC *******************
-----------------------------------------------------------------------------

local mvec3_set = mvector3.set
local mvec3_set_z = mvector3.set_z
local mvec3_sub = mvector3.subtract
local mvec3_dir = mvector3.direction
local mvec3_dot = mvector3.dot
local mvec3_dis = mvector3.distance
local mvec3_dis_sq = mvector3.distance_sq

local tmp_vec1 = Vector3()
local tmp_vec2 = Vector3()

CopLogicBase = class()

CopLogicBase._AGGRESSIVE_ALERT_TYPES = {
	footstep = true,
	bullet = true,
	vo_cbt = true,
	vo_intimidate = true,
	vo_distress = true,
	aggression = true
}

CopLogicBase._DANGEROUS_ALERT_TYPES = {
	bullet = true,
	aggression = true
}

-----------------------------------------------------------------------------

function CopLogicBase.enter( data, new_logic_name, enter_params )
end

-----------------------------------------------------------------------------

function CopLogicBase.exit( data, new_logic_name, enter_params )
	--print( "[CopLogicBase.exit]", data.unit, data.name, new_logic_name )
	if data.internal_data then
		data.internal_data.exiting = true
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.action_data( data )
	return data.action_data
end

-----------------------------------------------------------------------------

function CopLogicBase.can_activate( data )
	return true
end

-----------------------------------------------------------------------------

function CopLogicBase.on_intimidated( data, amount, aggressor_unit )
end

-----------------------------------------------------------------------------

function CopLogicBase.on_tied( data, aggressor_unit )
end


-----------------------------------------------------------------------------

function CopLogicBase.on_criminal_neutralized( data, criminal_key )
end

-----------------------------------------------------------------------------

function CopLogicBase._set_attention_on_unit( data, attention_unit )
	local attention_data = { unit = attention_unit }
	data.unit:movement():set_attention( attention_data )
end

-----------------------------------------------------------------------------

function CopLogicBase._set_attention( data, attention_info, reaction )
	local attention_data = {
		unit = attention_info.unit,
		u_key = attention_info.u_key,
		handler = attention_info.handler,
		reaction = reaction or attention_info.reaction
	}
	data.unit:movement():set_attention( attention_data )
end

-----------------------------------------------------------------------------

function CopLogicBase._set_attention_on_pos( data, pos )
	local attention_data = { pos = pos }
	data.unit:movement():set_attention( attention_data )
end

-----------------------------------------------------------------------------

function CopLogicBase._reset_attention( data )
	data.unit:movement():set_attention()
end

-----------------------------------------------------------------------------

function CopLogicBase.is_available_for_assignment( data )
	return true
end

-----------------------------------------------------------------------------

function CopLogicBase.action_complete_clbk( data, action )
end

-----------------------------------------------------------------------------

function CopLogicBase.damage_clbk( data, result, attack_unit )
end

-----------------------------------------------------------------------------

function CopLogicBase.death_clbk( data, damage_info )
end

-----------------------------------------------------------------------------
-- alert_data: { alert_nature, epicenter, effective_radius, alerting_unit }
function CopLogicBase.on_alert( data, alert_data )
end

-----------------------------------------------------------------------------
-- Callback received from enemy manager when the safety status of an area changes
function CopLogicBase.on_area_safety( data, nav_seg, safe )
end

-----------------------------------------------------------------------------

function CopLogicBase.draw_reserved_positions( data )
	local my_pos = data.m_pos
	local my_data = data.internal_data
	local rsrv_pos = my_data.rsrv_pos
	-- Draw world positions
	if rsrv_pos.path then
		Application:draw_cylinder( rsrv_pos.path.pos, my_pos, 6, 0, 0.3, 0.3 )
	end
	if rsrv_pos.move_dest then
		Application:draw_cylinder( rsrv_pos.move_dest.pos, my_pos, 6, 0.3, 0.3, 0 )
	end
	if rsrv_pos.stand then
		Application:draw_cylinder( rsrv_pos.stand.pos, my_pos, 6, 0.3, 0, 0.3 )
	end
	--Draw positions reserved through covers
	if my_data.best_cover then
		local cover_pos = my_data.best_cover[1][5].pos
		Application:draw_cylinder( cover_pos, my_pos, 2, 0.2, 0.3, 0.6 )
		Application:draw_sphere( cover_pos, 10, 0.2, 0.3, 0.6 )
	end
	if my_data.nearest_cover then
		local cover_pos = my_data.nearest_cover[1][5].pos
		Application:draw_cylinder( cover_pos, my_pos, 2, 0.2, 0.6, 0.3 )
		Application:draw_sphere( cover_pos, 8, 0.2, 0.6, 0.3 )
	end
	if my_data.moving_to_cover then
		local cover_pos = my_data.moving_to_cover[1][5].pos
		Application:draw_cylinder( cover_pos, my_pos, 2, 0.3, 0.6, 0.2 )
		Application:draw_sphere( cover_pos, 8, 0.3, 0.6, 0.2 )
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.draw_reserved_covers( data )
	local my_pos = data.m_pos
	local my_data = data.internal_data
	if my_data.best_cover then
		local cover_pos = my_data.best_cover[1][5].pos
		Application:draw_cylinder( cover_pos, my_pos, 2, 0.2, 0.3, 0.6 )
		Application:draw_sphere( cover_pos, 10, 0.2, 0.3, 0.6 )
	end
	if my_data.nearest_cover then
		local cover_pos = my_data.nearest_cover[1][5].pos
		Application:draw_cylinder( cover_pos, my_pos, 2, 0.2, 0.6, 0.3 )
		Application:draw_sphere( cover_pos, 8, 0.2, 0.6, 0.3 )
	end
	if my_data.moving_to_cover then
		local cover_pos = my_data.moving_to_cover[1][5].pos
		Application:draw_cylinder( cover_pos, my_pos, 2, 0.3, 0.6, 0.2 )
		Application:draw_sphere( cover_pos, 8, 0.3, 0.6, 0.2 )
	end
end

-----------------------------------------------------------------------------

function CopLogicBase._exit( unit, state_name, params )
	unit:brain():set_logic( state_name, params )
end

-----------------------------------------------------------------------------

function CopLogicBase.on_detected_enemy_destroyed( data, enemy_unit )
end

-----------------------------------------------------------------------------

function CopLogicBase._can_move( data )
	return true
end

-----------------------------------------------------------------------------

function CopLogicBase._report_detections( enemies )
	local group = managers.groupai:state()
	for key, data in pairs( enemies ) do
		if data.verified and data.criminal_record then
			group:criminal_spotted( data.unit )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.on_importance( data )
end

-----------------------------------------------------------------------------

function CopLogicBase.queue_task( internal_data, id, func, data, exec_t, asap )
	--print( "CopLogicBase.queue_task", internal_data.unit, internal_data.unit:brain()._current_logic_name, id, asap )
	--Application:stack_dump()
	if internal_data.unit and internal_data ~= internal_data.unit:brain()._logic_data.internal_data then
		debug_pause( "[CopLogicBase.queue_task] Task queued from the wrong logic", internal_data.unit, id, func, data, exec_t, asap )
	end
	local qd_tasks = internal_data.queued_tasks
	if qd_tasks then
		if qd_tasks[ id ] then
			debug_pause( "[CopLogicBase.queue_task] Task queued twice", internal_data.unit, id, func, data, exec_t, asap )
		end
		qd_tasks[ id ] = true
	else
		internal_data.queued_tasks = { [id] = true }
	end
	managers.enemy:queue_task( id, func, data, exec_t, callback( CopLogicBase, CopLogicBase, "on_queued_task", internal_data ), asap )
end

-----------------------------------------------------------------------------

function CopLogicBase.cancel_queued_tasks( internal_data )
	--print( "CopLogicBase.cancel_queued_tasks", internal_data.unit, internal_data.unit:brain()._current_logic_name )
	local qd_tasks = internal_data.queued_tasks
	if qd_tasks then
		local e_manager = managers.enemy
		for id, _ in pairs( qd_tasks ) do
			e_manager:unqueue_task( id )
		end
		internal_data.queued_tasks = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.unqueue_task( internal_data, id )
	--print( "CopLogicBase.unqueue_task", internal_data.unit, id )
	managers.enemy:unqueue_task( id )
	internal_data.queued_tasks[ id ] = nil
	if not next( internal_data.queued_tasks ) then
		internal_data.queued_tasks = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.chk_unqueue_task( internal_data, id )
	if internal_data.queued_tasks and internal_data.queued_tasks[ id ] then
		--print( "CopLogicBase.chk_unqueue_task", internal_data.unit, id )
		managers.enemy:unqueue_task( id )
		internal_data.queued_tasks[ id ] = nil
		if not next( internal_data.queued_tasks ) then
			internal_data.queued_tasks = nil
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.on_queued_task( ignore_this, internal_data, id )
	if not ( internal_data.queued_tasks and internal_data.queued_tasks[ id ] ) then
		debug_pause( "[CopLogicBase.on_queued_task] the task is not queued", internal_data.unit, id )
		return
	end
	internal_data.queued_tasks[ id ] = nil
	if not next( internal_data.queued_tasks ) then
		internal_data.queued_tasks = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.add_delayed_clbk( internal_data, id, clbk, exec_t )
	if internal_data.unit and internal_data ~= internal_data.unit:brain()._logic_data.internal_data then
		debug_pause( "[CopLogicBase.add_delayed_clbk] Clbk added from the wrong logic", internal_data.unit, id, clbk, exec_t )
	end
	local clbks = internal_data.delayed_clbks
	if clbks then
		if clbks[ id ] then
			debug_pause( "[CopLogicBase.queue_task] Callback added twice", internal_data.unit, id, clbk, exec_t )
		end
		clbks[ id ] = true
	else
		internal_data.delayed_clbks = {[id] = true }
	end
	managers.enemy:add_delayed_clbk( id, clbk, exec_t )
end

-----------------------------------------------------------------------------

function CopLogicBase.cancel_delayed_clbks( internal_data )
	local clbks = internal_data.delayed_clbks
	if clbks then
		local e_manager = managers.enemy
		for id, _ in pairs( clbks ) do
			e_manager:remove_delayed_clbk( id )
		end
		internal_data.delayed_clbks = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.cancel_delayed_clbk( internal_data, id )
	if not ( internal_data.delayed_clbks and internal_data.delayed_clbks[ id ] ) then
		debug_pause( "[CopLogicBase.cancel_delayed_clbk] Tried to cancel inexistent clbk", internal_data.unit, id, internal_data.delayed_clbks and inspect( internal_data.delayed_clbks ) )
		return
	end
	managers.enemy:remove_delayed_clbk( id )
	internal_data.delayed_clbks[ id ] = nil
	if not next( internal_data.delayed_clbks ) then
		internal_data.delayed_clbks = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.chk_cancel_delayed_clbk( internal_data, id )
	if internal_data.delayed_clbks and internal_data.delayed_clbks[ id ] then
		managers.enemy:remove_delayed_clbk( id )
		internal_data.delayed_clbks[ id ] = nil
		if not next( internal_data.delayed_clbks ) then
			internal_data.delayed_clbks = nil
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.on_delayed_clbk( internal_data, id )
	if not ( internal_data.delayed_clbks and internal_data.delayed_clbks[ id ] ) then
		debug_pause( "[CopLogicBase.on_delayed_clbk] Callback not added", internal_data.unit, id, internal_data.delayed_clbks and inspect( internal_data.delayed_clbks ) )
	end
	internal_data.delayed_clbks[ id ] = nil
	if not next( internal_data.delayed_clbks ) then
		internal_data.delayed_clbks = nil
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.on_objective_unit_damaged( data, unit, attacker_unit )
end

-----------------------------------------------------------------------------

function CopLogicBase.on_objective_unit_destroyed( data, unit )
	if not alive( data.unit ) then
		debug_pause( "dead unit did not remove destroy listener", data.debug_name, inspect( data.objective ), data.name )
		return
	end
	data.objective.destroy_clbk_key = nil
	data.objective.death_clbk_key = nil
	managers.groupai:state():on_objective_failed( data.unit, data.objective )
end

-----------------------------------------------------------------------------

function CopLogicBase.on_new_objective( data, old_objective )
	if old_objective then
		if old_objective.follow_unit then
			if old_objective.destroy_clbk_key then
				old_objective.follow_unit:base():remove_destroy_listener( old_objective.destroy_clbk_key )
				old_objective.destroy_clbk_key = nil
			end
			if old_objective.death_clbk_key then
				old_objective.follow_unit:character_damage():remove_listener( old_objective.death_clbk_key )
				old_objective.death_clbk_key = nil
			end
		end
	end
	local new_objective = data.objective
	if new_objective then
		if ( new_objective.follow_unit ) and not new_objective.destroy_clbk_key then
			local ext_brain = data.unit:brain()
			local destroy_clbk_key = "objective_"..new_objective.type..tostring( data.unit:key() )
			new_objective.destroy_clbk_key = destroy_clbk_key
			new_objective.follow_unit:base():add_destroy_listener( destroy_clbk_key, callback( ext_brain, ext_brain, "on_objective_unit_destroyed" ) )
			
			if new_objective.follow_unit:character_damage() then
				new_objective.death_clbk_key = destroy_clbk_key
				new_objective.follow_unit:character_damage():add_listener( destroy_clbk_key, { "death", "hurt" }, callback( ext_brain, ext_brain, "on_objective_unit_damaged" ) )
			end
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.is_advancing( data )
end

-----------------------------------------------------------------------------

function CopLogicBase.anim_clbk(...)
end

-----------------------------------------------------------------------------

function CopLogicBase._upd_attention_obj_detection( data, min_reaction, max_reaction )
	--print( "[CopLogicBase._upd_attention_obj_detection]", data.unit:name():s(), data.t )
	local t = data.t
	local detected_obj = data.detected_attention_objects
	local my_data = data.internal_data
	local my_key = data.key
	local my_pos = data.unit:movement():m_head_pos()
	local my_access = data.SO_access
	local all_attention_objects = managers.groupai:state():get_AI_attention_objects_by_filter( data.SO_access_str )
	local my_head_fwd
	local my_tracker = data.unit:movement():nav_tracker()
	local chk_vis_func = my_tracker.check_visibility
	local is_detection_persistent = managers.groupai:state():is_detection_persistent()
	local delay = 1
	local player_importance_wgt = data.unit:in_slot( managers.slot:get_mask( "enemies" ) ) and {}	-- odd entries are keys, even entries are distances from human player criminals. applies only to enemies
	
-- Local function
	local _angle_chk = function ( attention_pos, dis, strictness )
		mvector3.direction( tmp_vec1, my_pos, attention_pos )
		my_head_fwd = my_head_fwd or data.unit:movement():m_head_rot():z()
		local angle = mvector3.angle( my_head_fwd, tmp_vec1 )
		
		local angle_max = math.lerp( 180, my_data.detection.angle_max, math.clamp( ( dis - 150 ) / 700, 0, 1 ) )
		
		--Application:draw_line( my_pos, my_pos + my_head_fwd * 1000, 0,1,0 )
		--Application:draw_line( my_pos, my_pos + tmp_vec1 * 1000, 0,1,0 )
		
		if angle * strictness < angle_max then	-- not outside my FOV
			return true
		end
	end
	
-- Local function	
	local _angle_and_dis_chk = function ( handler, settings, attention_pos )
		attention_pos = attention_pos or handler:get_detection_m_pos()
		local dis = mvector3.direction( tmp_vec1, my_pos, attention_pos )
		local dis_multiplier, angle_multiplier
		local max_dis = math.min( my_data.detection.dis_max, settings.max_range or my_data.detection.dis_max )
		if settings.detection and settings.detection.range_mul then
			max_dis = max_dis * settings.detection.range_mul
		end
		dis_multiplier = dis / max_dis
		
		if settings.uncover_range and my_data.detection.use_uncover_range and dis < settings.uncover_range then
			return -1, 0 -- angle=-1 means instant detection 
		end
		
		if dis_multiplier < 1 then	-- not too far
			if settings.notice_requires_FOV then
				my_head_fwd = my_head_fwd or data.unit:movement():m_head_rot():z()
				local angle = mvector3.angle( my_head_fwd, tmp_vec1 )
				
				local angle_max = math.lerp( 180, my_data.detection.angle_max, math.clamp( ( dis - 150 ) / 700, 0, 1 ) )
				angle_multiplier = angle / angle_max
				
				if angle_multiplier < 1 then	-- not outside my FOV
					return angle, dis_multiplier
				end
			else
				return 0, dis_multiplier
			end
		end
	end
	
-- Local function
	local _nearly_visible_chk = function ( attention_info, detect_pos ) -- this is only used for characters
		local near_pos = tmp_vec1
		if attention_info.verified_dis < 2000 and math.abs( detect_pos.z - my_pos.z ) < 300 then
			mvec3_set( near_pos, detect_pos )
			mvec3_set_z( near_pos, near_pos.z + 100 )
			local near_vis_ray = World:raycast( "ray", my_pos, near_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision", "report" )
			if near_vis_ray then
				local side_vec = tmp_vec1
				mvec3_set( side_vec, detect_pos )
				mvec3_sub( side_vec, my_pos )
				mvector3.cross( side_vec, side_vec, math.UP )
				mvector3.set_length( side_vec, 150 )
				mvector3.set( near_pos, detect_pos )
				mvector3.add( near_pos, side_vec )
				local near_vis_ray = World:raycast( "ray", my_pos, near_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision", "report" )
				if near_vis_ray then
					mvector3.multiply( side_vec, -2 )
					mvector3.add( near_pos, side_vec )
					near_vis_ray = World:raycast( "ray", my_pos, near_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision", "report" )
				end
			end
			
			if not near_vis_ray then
				--Application:draw_line( my_pos, near_pos, 0,0,1 )
				attention_info.nearly_visible = true
				attention_info.last_verified_pos = mvector3.copy( near_pos )
			end
		end
	end
	
-- Local function
	local _chk_record_pl_importance_wgt = function( attention_info )
		if not ( player_importance_wgt and attention_info.is_human_player ) then
			return
		end
		local weight = mvector3.direction( tmp_vec1, attention_info.m_head_pos, my_pos )
		local e_fwd
		if attention_info.is_husk_player then -- husk
			e_fwd = attention_info.unit:movement():detect_look_dir()
		else
			e_fwd = attention_info.unit:movement():m_head_rot():y()	-- local unit
		end
		local dot = mvector3.dot( e_fwd, tmp_vec1 )
		weight = weight * weight * ( 1 - dot )
		table.insert( player_importance_wgt, attention_info.u_key )
		table.insert( player_importance_wgt, weight )
	end
	
	-- acquire new
	for u_key, attention_info in pairs( all_attention_objects ) do
		if u_key ~= my_key and not detected_obj[ u_key ] then
			if not attention_info.nav_tracker or chk_vis_func( my_tracker, attention_info.nav_tracker ) then
				local settings = attention_info.handler:get_attention( my_access, min_reaction, max_reaction )
				--print( "[CopLogicBase._upd_attention_obj_detection] settings", settings and inspect( settings ) )
				
				if settings then
					local attention_pos = attention_info.handler:get_detection_m_pos()
					if _angle_and_dis_chk( attention_info.handler, settings, attention_pos ) then
						local vis_ray = World:raycast( "ray", my_pos, attention_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision" )
						--print( "[CopLogicBase._upd_attention_obj_detection] attention vis_ray", vis_ray and vis_ray.unit:name():s() )
						
						if not vis_ray or vis_ray.unit:key() == u_key then
							--Application:draw_line( my_pos, attention_pos, 0,0,1 )
							--Application:set_pause( true )
							--print( "attention acquired", attention_info.unit )
							
							detected_obj[ u_key ] = CopLogicBase._create_detected_attention_object_data( data, my_data, u_key, attention_info, settings )
						end
					end
				end
			end
		end
	end
	
	-- check if a new attention object is noticed and verify all detected
	for u_key, attention_info in pairs( detected_obj ) do
		if not data.important and attention_info.next_verify_t > t then
			delay = math.min( attention_info.next_verify_t - t, delay )
		else
			attention_info.next_verify_t = t + ( attention_info.identified and attention_info.verified and attention_info.settings.verification_interval or attention_info.settings.notice_interval or attention_info.settings.verification_interval )
			delay = math.min( delay, attention_info.settings.verification_interval )
			if not attention_info.identified then
				local noticable
				local angle, dis_multiplier = _angle_and_dis_chk( attention_info.handler, attention_info.settings )
				
				if angle then -- inside FOV
					local attention_pos = attention_info.handler:get_detection_m_pos()
					local vis_ray = World:raycast( "ray", my_pos, attention_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision" )
					if not vis_ray or vis_ray.unit:key() == u_key then
						noticable = true
					end
				end
				
				local delta_prog
				local dt = t - attention_info.prev_notice_chk_t
				
				if noticable then
					--print( "\nnoticeable" )
					if angle == -1 then -- instant detection
						delta_prog = 1
					else
						local min_delay = my_data.detection.delay[1]
						local max_delay = my_data.detection.delay[2]
						local angle_mul_mod = 0.25 * math.min( angle / my_data.detection.angle_max, 1 ) -- angle only plays 25% role
						local dis_mul_mod = 0.75 * dis_multiplier
						local notice_delay_mul = attention_info.settings.notice_delay_mul or 1
						if attention_info.settings.detection and attention_info.settings.detection.delay_mul then
							notice_delay_mul = notice_delay_mul * attention_info.settings.detection.delay_mul
						end
						local notice_delay_modified = math.lerp( min_delay, max_delay, dis_mul_mod + angle_mul_mod ) * notice_delay_mul
						delta_prog = notice_delay_modified > 0 and dt / notice_delay_modified or 1
						--[[if attention_info.unit == managers.player:player_unit() then
							print( "notice_delay_modified", notice_delay_modified, "max_delay", max_delay, "angle_mul_mod", angle_mul_mod, "dis_mul_mod", dis_mul_mod, "dis_multiplier", dis_multiplier, "angle_mul", angle / my_data.detection.angle_max, "dt", dt, "delta_prog", delta_prog )
						end]]
					end
				else
					delta_prog = dt * -0.125
					--print( "non-noticeable delta_prog", delta_prog )
				end
				
				attention_info.notice_progress = attention_info.notice_progress + delta_prog
				--print( "notice_progress", attention_info.notice_progress )
				if attention_info.notice_progress > 1 then
					--print( "GAME OVER" )
					attention_info.notice_progress = nil
					attention_info.prev_notice_chk_t = nil
					attention_info.identified = true
					attention_info.release_t = t + attention_info.settings.release_delay
					attention_info.identified_t = t
					noticable = true --identified
					
					data.logic.on_attention_obj_identified( data, u_key, attention_info )
				elseif attention_info.notice_progress < 0 then
					--print( "LOST" )
					CopLogicBase._destroy_detected_attention_object_data( data, attention_info ) -- notice_clbk gets called here
					noticable = false --lost
				else
					--print( "COUNTING", attention_info.notice_progress )
					noticable = attention_info.notice_progress
					attention_info.prev_notice_chk_t = t
					if attention_info.settings.reaction >= AIAttentionObject.REACT_SCARED then
						managers.groupai:state():on_criminal_suspicion_progress( attention_info.unit, data.unit, noticable )
					end
				end
				if noticable ~= false and attention_info.settings.notice_clbk then
					attention_info.settings.notice_clbk( data.unit, noticable )
				end
			end
			
			if attention_info.identified then
				delay = math.min( delay, attention_info.settings.verification_interval )
				attention_info.nearly_visible = nil
				local verified, vis_ray
				local attention_pos = attention_info.handler:get_detection_m_pos()
				local dis = mvector3.distance( data.m_pos, attention_info.m_pos )
				if dis < my_data.detection.dis_max * 1.2 and ( not attention_info.settings.max_range or dis < attention_info.settings.max_range * 1.2 ) then -- 20% tolerance
					local detect_pos
					if attention_info.is_husk_player and attention_info.unit:anim_data().crouch then -- husk player. aim where the head is on the client
						detect_pos = tmp_vec1
						mvector3.set( detect_pos, attention_info.m_pos )
						mvector3.add( detect_pos, tweak_data.player.stances.default.crouched.head.translation )
					else
						detect_pos = attention_pos
					end
					
					local in_FOV = not attention_info.settings.notice_requires_FOV or data.enemy_slotmask and attention_info.unit:in_slot( data.enemy_slotmask ) or _angle_chk( attention_pos, dis, 0.8 )
					if in_FOV then
						vis_ray = World:raycast( "ray", my_pos, detect_pos, "slot_mask", data.visibility_slotmask, "ray_type", "ai_vision" )
						--print( "[CopLogicBase._upd_attention_obj_detection] attention vis_ray", vis_ray and vis_ray.unit:name():s() )
						if not vis_ray or vis_ray.unit:key() == u_key then
							verified = true
						end
					end
					
					attention_info.verified = verified
				end
				
				attention_info.dis = dis
				
				if verified then
					attention_info.release_t = nil
					attention_info.verified_t = t
					mvector3.set( attention_info.verified_pos, attention_pos )
					attention_info.last_verified_pos = mvector3.copy( attention_pos )
					attention_info.verified_dis = dis
				elseif data.enemy_slotmask and attention_info.unit:in_slot( data.enemy_slotmask ) then -- special treatment for enemies
					if attention_info.criminal_record and attention_info.settings.reaction >= AIAttentionObject.REACT_COMBAT then
						if not is_detection_persistent and mvector3.distance( attention_pos, attention_info.criminal_record.pos ) > 700 then
							CopLogicBase._destroy_detected_attention_object_data( data, attention_info )
						else
							delay = math.min( 0.2, delay )	-- We have lost the enemy from sight. update more often
							attention_info.verified_pos = mvector3.copy( attention_info.criminal_record.pos )	-- All friendly units contribute to detecting enemies
							attention_info.verified_dis = dis
							if vis_ray and data.logic._chk_nearly_visible_chk_needed( data, attention_info, u_key ) then
								_nearly_visible_chk( attention_info, attention_pos )
							end
						end
					elseif attention_info.release_t and attention_info.release_t < t then
						CopLogicBase._destroy_detected_attention_object_data( data, attention_info )
					else
						attention_info.release_t = attention_info.release_t or t + attention_info.settings.release_delay
					end
				elseif attention_info.release_t and attention_info.release_t < t then
					CopLogicBase._destroy_detected_attention_object_data( data, attention_info )
				else
					attention_info.release_t = attention_info.release_t or t + attention_info.settings.release_delay
				end
			end
		end
		_chk_record_pl_importance_wgt( attention_info )
	end
	
	if player_importance_wgt then
		managers.groupai:state():set_importance_weight( data.key, player_importance_wgt )
	end
	return delay
end

-----------------------------------------------------------------------------

function CopLogicBase._create_detected_attention_object_data( data, my_data, u_key, attention_info, settings )
	local ext_brain = data.unit:brain()
	attention_info.handler:add_listener( "detect_"..tostring( data.key ), callback( ext_brain, ext_brain, "on_detected_attention_obj_modified" ) )
	
	local att_unit = attention_info.unit
	
	local m_pos = attention_info.handler:get_ground_m_pos()
	local m_head_pos = attention_info.handler:get_detection_m_pos()
	
	local is_local_player, is_husk_player, is_deployable, is_person, is_very_dangerous, nav_tracker, char_tweak
	if att_unit:base() then
		is_local_player = att_unit:base().is_local_player
		is_husk_player = att_unit:base().is_husk_player
		is_deployable = att_unit:base().sentry_gun
		is_person = att_unit:in_slot( managers.slot:get_mask( "persons" ) )
		if att_unit:base()._tweak_table then
			char_tweak = tweak_data.character[ att_unit:base()._tweak_table ]
		end
		is_very_dangerous = att_unit:base()._tweak_table == "taser" or att_unit:base()._tweak_table == "spooc"
	end

	local dis = mvector3.distance( data.unit:movement():m_head_pos(), m_head_pos )
	local new_entry = {
		settings = settings,
		unit = attention_info.unit,
		u_key = u_key,
		handler = attention_info.handler,
		next_verify_t = data.t + ( settings.notice_interval or settings.verification_interval ),
		prev_notice_chk_t = data.t,
		notice_progress = 0,
		m_pos = m_pos,
		m_head_pos = m_head_pos,
		nav_tracker = attention_info.nav_tracker,
		is_local_player = is_local_player,
		is_husk_player = is_husk_player,
		is_human_player = is_local_player or is_husk_player,
		is_deployable = is_deployable,
		is_person = is_person,
		is_very_dangerous = is_very_dangerous,
		reaction = settings.reaction,
		criminal_record = managers.groupai:state():criminal_record( u_key ),
		char_tweak = char_tweak,
		verified_pos = mvector3.copy( m_head_pos ),
		verified_dis = dis,
		dis = dis,
		verified = false,
		verified_t = false
	}
	
	return new_entry
end

-----------------------------------------------------------------------------

function CopLogicBase._destroy_detected_attention_object_data( data, attention_info )
	attention_info.handler:remove_listener( "detect_"..tostring( data.key ) )
	if attention_info.settings.notice_clbk then
		attention_info.settings.notice_clbk( data.unit, false )
	end
	
	if attention_info.settings.reaction >= AIAttentionObject.REACT_SCARED then
		managers.groupai:state():on_criminal_suspicion_progress( attention_info.unit, data.unit, nil )
	end
	
	if attention_info.uncover_progress then
		attention_info.unit:movement():on_suspicion( data.unit, false )
	end
	data.detected_attention_objects[ attention_info.u_key ] = nil
end

-----------------------------------------------------------------------------

function CopLogicBase._destroy_all_detected_attention_object_data( data )
	for u_key, attention_info in pairs( data.detected_attention_objects ) do
		attention_info.handler:remove_listener( "detect_"..tostring( data.key ) )
		if not attention_info.identified and attention_info.settings.notice_clbk then
			attention_info.settings.notice_clbk( data.unit, false )
		end
		
		if attention_info.settings.reaction >= AIAttentionObject.REACT_SCARED then
			managers.groupai:state():on_criminal_suspicion_progress( attention_info.unit, data.unit, nil )
		end
	
		if attention_info.uncover_progress then
			attention_info.unit:movement():on_suspicion( data.unit, false )
		end
	end
	data.detected_attention_objects = {}
end

-----------------------------------------------------------------------------

function CopLogicBase.on_detected_attention_obj_modified( data, modified_u_key )
	if data.logic.on_detected_attention_obj_modified_internal then
		data.logic.on_detected_attention_obj_modified_internal( data, modified_u_key )
	end
	
	local attention_info = data.detected_attention_objects[ modified_u_key ]
	if not attention_info then
		return
	end
	
	local new_settings = attention_info.handler:get_attention( data.SO_access )
	local old_settings = attention_info.settings
	if new_settings == old_settings then
		return -- no changes for me
	end
	
	local old_notice_clbk = not attention_info.identified and old_settings.notice_clbk
	
	if new_settings then -- replace old settings
		local switch_from_suspicious = new_settings.reaction >= AIAttentionObject.REACT_SCARED and attention_info.reaction <= AIAttentionObject.REACT_SUSPICIOUS
		
		attention_info.settings = new_settings
		attention_info.stare_expire_t = nil
		attention_info.pause_expire_t = nil
		if attention_info.uncover_progress then
			attention_info.uncover_progress = nil
			attention_info.unit:movement():on_suspicion( data.unit, false )
			managers.groupai:state():on_criminal_suspicion_progress( attention_info.unit, data.unit, nil )
		end
		if attention_info.identified then
			if switch_from_suspicious then
				attention_info.identified = false
				attention_info.notice_progress = attention_info.uncover_progress or 0
				attention_info.verified = nil -- these 2 lines force redetection of the attention_object
				attention_info.next_verify_t = 0
				attention_info.prev_notice_chk_t = TimerManager:game():time()
			end
		else
			if switch_from_suspicious then
				attention_info.next_verify_t = 0
				attention_info.notice_progress = 0
				attention_info.prev_notice_chk_t = TimerManager:game():time()
			end
		end
		attention_info.reaction = math.min( new_settings.reaction, attention_info.reaction ) -- this will need to be re-evaluated. drop the reaction to min to be on the safe side
	else -- no attention for this object
		CopLogicBase._destroy_detected_attention_object_data( data, attention_info )
		
		local my_data = data.internal_data
		if data.attention_obj and data.attention_obj.u_key == modified_u_key then
			CopLogicBase._set_attention_obj( data, nil, nil )
			
			if my_data then
				if my_data.firing or my_data.firing_on_client then
					data.unit:movement():set_allow_fire( false )
					my_data.firing = nil
					my_data.firing_on_client = nil
				end
			end
		end
		if my_data.arrest_targets then
			my_data.arrest_targets[ modified_u_key ] = nil
		end
	end
	
	if old_notice_clbk and not ( new_settings and new_settings.notice_clbk ) then
		old_notice_clbk( data.unit, false )
	end
	
	if old_settings.reaction >= AIAttentionObject.REACT_SCARED and not ( new_settings and new_settings.reaction >= AIAttentionObject.REACT_SCARED ) then
		managers.groupai:state():on_criminal_suspicion_progress( attention_info.unit, data.unit, nil )
	end
end

-----------------------------------------------------------------------------

function CopLogicBase._set_attention_obj( data, new_att_obj, new_reaction )
	
	local old_att_obj = data.attention_obj
	data.attention_obj = new_att_obj
	
	if new_att_obj then
		new_reaction = new_reaction or new_att_obj.settings.reaction
		new_att_obj.reaction = new_reaction
		local new_crim_rec = new_att_obj.criminal_record
		local is_same_obj
		local contact_chatter_time_ok
		
		if old_att_obj then -- we were engaged
			if old_att_obj.u_key == new_att_obj.u_key then
				is_same_obj = true
				
				contact_chatter_time_ok = new_crim_rec and data.t - new_crim_rec.det_t > 2
				
				if new_att_obj.stare_expire_t and data.t > new_att_obj.stare_expire_t then
					if new_att_obj.settings.pause then
						new_att_obj.stare_expire_t = nil
						new_att_obj.pause_expire_t = data.t + math.lerp( new_att_obj.settings.pause[1], new_att_obj.settings.pause[2], math.random() )
					end
				elseif new_att_obj.pause_expire_t and data.t > new_att_obj.pause_expire_t then
					if not new_att_obj.settings.attract_chance or math.random() < new_att_obj.settings.attract_chance then
						new_att_obj.pause_expire_t = nil
						new_att_obj.stare_expire_t = data.t + math.lerp( new_att_obj.settings.duration[1], new_att_obj.settings.duration[2], math.random() )
					else -- skip attraction this time
						debug_pause_unit( data.unit, "skipping attraction" )
						new_att_obj.pause_expire_t = data.t + math.lerp( new_att_obj.settings.pause[1], new_att_obj.settings.pause[2], math.random() )
					end
				end
			else
				if old_att_obj.criminal_record then
					managers.groupai:state():on_enemy_disengaging( data.unit, old_att_obj.u_key )
				end
				if new_crim_rec then
					managers.groupai:state():on_enemy_engaging( data.unit, new_att_obj.u_key )
				end
				contact_chatter_time_ok = new_crim_rec and data.t - new_crim_rec.det_t > 15
			end
		else	-- we were not engaged
			if new_crim_rec then
				managers.groupai:state():on_enemy_engaging( data.unit, new_att_obj.u_key )
			end
			contact_chatter_time_ok = new_crim_rec and data.t - new_crim_rec.det_t > 15
		end
		
		if not is_same_obj then
			if new_att_obj.settings.duration then
				new_att_obj.stare_expire_t = data.t + math.lerp( new_att_obj.settings.duration[1], new_att_obj.settings.duration[2], math.random() )
				new_att_obj.pause_expire_t = nil
			end
			new_att_obj.acquire_t = data.t
		end
		
		if new_reaction >= AIAttentionObject.REACT_SHOOT and
			new_att_obj.verified and
			contact_chatter_time_ok and
			( data.unit:anim_data().idle or data.unit:anim_data().move ) and
			new_att_obj.is_person and
			data.char_tweak.chatter.contact
		then
			data.unit:sound():say( "c01", true ) -- "I see him!"
		end
	elseif old_att_obj then	-- we were engaged
		if old_att_obj.criminal_record then
			managers.groupai:state():on_enemy_disengaging( data.unit, old_att_obj.u_key )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase._am_i_important_to_player( record, my_key )
	if record.important_enemies then
		for i, test_e_key in ipairs( record.important_enemies ) do
			if test_e_key == my_key then
				return true
			end
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.should_duck_on_alert( data, alert_data )
	if not ( data.important and data.char_tweak.allow_crouch and alert_data[1] ~= "voice" ) or data.unit:anim_data().crouch or data.unit:movement():chk_action_forbidden( "walk" ) then
		return
	end
	
	local lower_body_action = data.unit:movement()._active_actions[2]
	if lower_body_action and lower_body_action:type() == "walk" then 
		if not data.char_tweak.crouch_move then
			return
		end
	end
	return 
end

-----------------------------------------------------------------------------

function CopLogicBase._chk_nearly_visible_chk_needed( data, attention_info, u_key )
	return not attention_info.criminal_record or attention_info.is_human_player and CopLogicBase._am_i_important_to_player( attention_info.criminal_record, data.key )
end

-----------------------------------------------------------------------------
-- return values ( may_take_transition, is_interrupted )
function CopLogicBase.is_obstructed( data, objective, strictness, attention )
	local my_data = data.internal_data
	
	attention = attention or data.attention_obj
	
	-- to do: carry action_duration between logics so we don't have to forbid transition
	if not objective or objective.is_default or ( objective.in_place or not objective.nav_seg ) and not objective.action and not objective.action_duration then
		return true, false
	end
	
	if objective.interrupt_suppression and data.is_suppressed then
		return true, true -- we have sustained too much suppression. allow_transition & interrupt
	end
	
	strictness = strictness or 0
	
	if objective.interrupt_health then
		local health_ratio = data.unit:character_damage():health_ratio()
		if health_ratio < 1 and health_ratio * (1-strictness) < objective.interrupt_health then
			return true, true -- we have sustained too much dmg. allow_transition & interrupt
		end
	end
	
	if objective.interrupt_dis then
		if attention and attention.reaction >= AIAttentionObject.REACT_SURPRISED then
			if objective.interrupt_dis == -1 then
				return true, true
			elseif math.abs( attention.m_pos.z - data.m_pos.z ) < 250 then
				local enemy_dis
				if attention.verified then
					enemy_dis = attention.verified_dis * (1-strictness)
				else
					enemy_dis = 1.5 * mvector3.distance( attention.m_pos, data.m_pos ) * (1-strictness) -- non visible enemies must come closer to interrupt
				end
				if objective.interrupt_dis > enemy_dis then
					return true, true -- threat is too near. allow_transition & interrupt
				end
			end
			
			if objective.pos and math.abs( attention.m_pos.z - objective.pos.z ) < 250 then
				local enemy_dis = mvector3.distance( objective.pos, attention.m_pos ) * (1-strictness)
				if objective.interrupt_dis > enemy_dis then
					return true, true -- threat is too near. allow_transition & interrupt
				end
			end
		elseif objective.interrupt_dis == -1 and not data.unit:movement():cool() then
			return true, true
		end
	end
	
	return false, false -- there was a threat but was not not big enough. do not abandon objective
end

-----------------------------------------------------------------------------

function CopLogicBase._upd_suspicion( data, my_data, attention_obj )
	
	if not attention_obj.verified then
		if my_data.last_suspicion_t then
			my_data.last_suspicion_t = data.t
		end
		return
	end
	local _exit_func = function()
		--print( "uncovered!!!!!" )
		attention_obj.unit:movement():on_uncovered( data.unit )
		
		local reaction, state_name
		if attention_obj.dis < 2000 and attention_obj.verified then
			reaction = AIAttentionObject.REACT_ARREST
			state_name = "arrest"
		else
			reaction = AIAttentionObject.REACT_COMBAT
			state_name = "attack"
		end
		attention_obj.reaction = reaction
		local allow_trans, obj_failed = CopLogicBase.is_obstructed( data, data.objective, nil, attention_obj )
		if allow_trans then
			if obj_failed then
				--print( "obj failed" )
				managers.groupai:state():on_objective_failed( data.unit, data.objective )
				if my_data ~= data.internal_data then
					return true -- we already changed state
				end
			end
			CopLogicBase._exit( data.unit, state_name )
			return true
		end
	end
	
	local dis = attention_obj.dis
	local susp_settings = attention_obj.unit:base():suspicion_settings()
	if attention_obj.settings.uncover_range and dis < math.min( attention_obj.settings.max_range, attention_obj.settings.uncover_range * susp_settings.range_mul ) then
		--print( "came too close", dis, math.min( attention_obj.settings.max_range, attention_obj.settings.uncover_range * susp_settings.range_mul ) )
		attention_obj.unit:movement():on_suspicion( data.unit, true )
		managers.groupai:state():on_criminal_suspicion_progress( attention_obj.unit, data.unit, true )
		managers.groupai:state():criminal_spotted( attention_obj.unit )
		return _exit_func()
	elseif attention_obj.settings.suspicion_range and dis < math.min( attention_obj.settings.max_range, attention_obj.settings.suspicion_range * susp_settings.range_mul ) then
		--print( "detection in progress", dis, math.min( attention_obj.settings.max_range, attention_obj.settings.suspicion_range * susp_settings.range_mul ) )
		
		if my_data.last_suspicion_t and my_data.last_suspicion_u_key ~= attention_obj.u_key then
			my_data.last_suspicion_t = nil
		end
		
		if my_data.last_suspicion_t then
			local dt = ( data.t - my_data.last_suspicion_t )
			
			local range_max = ( attention_obj.settings.suspicion_range - ( attention_obj.settings.uncover_range or 0 ) ) * susp_settings.range_mul
			local range_min = ( attention_obj.settings.uncover_range or 0 ) * susp_settings.range_mul
			local mul = 1 - ( dis - range_min ) / ( range_max )
			
			local progress = dt * mul * susp_settings.buildup_mul / attention_obj.settings.suspicion_duration
			attention_obj.uncover_progress = ( attention_obj.uncover_progress or 0 )  + progress
			--print( "attention_obj.uncover_progress", attention_obj.uncover_progress, "progress", progress, "dis", dis, "mul", mul, "dt", dt )
			if attention_obj.uncover_progress < 1 then
				attention_obj.unit:movement():on_suspicion( data.unit, attention_obj.uncover_progress )
				managers.groupai:state():on_criminal_suspicion_progress( attention_obj.unit, data.unit, attention_obj.uncover_progress )
			else
				attention_obj.unit:movement():on_suspicion( data.unit, true )
				managers.groupai:state():on_criminal_suspicion_progress( attention_obj.unit, data.unit, true )
				managers.groupai:state():criminal_spotted( attention_obj.unit )
				return _exit_func()
			end
		else
			my_data.last_suspicion_u_key = attention_obj.u_key
			attention_obj.uncover_progress = 0
		end
		
		my_data.last_suspicion_t = data.t
	elseif attention_obj.uncover_progress then
		--print( "out of range", dis )
		if my_data.last_suspicion_t then
			local dt = data.t - my_data.last_suspicion_t
			attention_obj.uncover_progress = attention_obj.uncover_progress - dt
			if attention_obj.uncover_progress <= 0 then
				attention_obj.uncover_progress = nil
				my_data.last_suspicion_t = nil
				attention_obj.unit:movement():on_suspicion( data.unit, false )
			else
				attention_obj.unit:movement():on_suspicion( data.unit, attention_obj.uncover_progress )
			end
		else
			my_data.last_suspicion_t = data.t
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase._get_logic_state_from_reaction( data, reaction )
	if reaction == nil and data.attention_obj then
		reaction = data.attention_obj.reaction
	end
	
	local police_is_being_called = managers.groupai:state():chk_enemy_calling_in_area( managers.groupai:state():get_area_from_nav_seg_id( data.unit:movement():nav_tracker():nav_segment() ), data.key )
	if not reaction or reaction <= AIAttentionObject.REACT_SCARED then
		if not ( police_is_being_called or managers.groupai:state():is_police_called() or data.unit:movement():cool() ) then
			return "arrest" -- call the police
		elseif data.unit:movement():cool() then
		else
			return "idle"
		end
	elseif reaction == AIAttentionObject.REACT_ARREST then
		return "arrest"
	else
		if not ( data.char_tweak.no_arrest or police_is_being_called or managers.groupai:state():is_police_called() or data.unit:movement():cool() ) and not ( data.attention_obj and data.attention_obj.verified and data.attention_obj.dis < 1500 ) then
			return "arrest" -- call the police
		else
			return "attack"
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase._chk_call_the_police( data )
	if not CopLogicBase._can_arrest( data ) then
		return
	end
	local allow_trans, obj_failed = CopLogicBase.is_obstructed( data, data.objective, nil, nil )
	if allow_trans and data.logic.is_available_for_assignment( data ) and not ( data.is_converted or data.unit:movement():cool() or managers.groupai:state():is_police_called() ) and ( not data.attention_obj or not data.attention_obj.verified_t or data.t - data.attention_obj.verified_t > 6 or data.attention_obj.reaction <= AIAttentionObject.REACT_ARREST ) then
		if obj_failed then
			managers.groupai:state():on_objective_failed( data.unit, data.objective )
		end
		if ( not data.objective or data.objective.is_default ) and not managers.groupai:state():chk_enemy_calling_in_area( managers.groupai:state():get_area_from_nav_seg_id( data.unit:movement():nav_tracker():nav_segment() ), data.key ) then
			CopLogicBase._exit( data.unit, "arrest" )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.identify_attention_obj_instant( data, att_u_key )
	local att_obj_data = data.detected_attention_objects[ att_u_key ]
	local is_new = not att_obj_data
	if att_obj_data then
		mvector3.set( att_obj_data.verified_pos, att_obj_data.handler:get_detection_m_pos() )
		att_obj_data.verified_dis = mvector3.distance( att_obj_data.verified_pos, data.unit:movement():m_stand_pos() )
		if not att_obj_data.identified then
			att_obj_data.identified = true
			att_obj_data.identified_t = data.t
			att_obj_data.notice_progress = nil
			att_obj_data.prev_notice_chk_t = nil
			if att_obj_data.settings.notice_clbk then
				att_obj_data.settings.notice_clbk( data.unit, true )
			end
			
			data.logic.on_attention_obj_identified( data, att_u_key, att_obj_data )
		elseif att_obj_data.uncover_progress then
			att_obj_data.uncover_progress = nil
			att_obj_data.unit:movement():on_suspicion( data.unit, false )
		end
	else
		local attention_info = managers.groupai:state():get_AI_attention_objects_by_filter( data.SO_access_str )[ att_u_key ]
		if attention_info then
			local settings = attention_info.handler:get_attention( data.SO_access, nil, nil ) 
			if settings then
				att_obj_data = CopLogicBase._create_detected_attention_object_data( data, data.internal_data, att_u_key, attention_info, settings )
				att_obj_data.identified = true
				att_obj_data.identified_t = data.t
				att_obj_data.notice_progress = nil
				att_obj_data.prev_notice_chk_t = nil
				if att_obj_data.settings.notice_clbk then
					att_obj_data.settings.notice_clbk( data.unit, true )
				end
				data.detected_attention_objects[ att_u_key ] = att_obj_data
				
				data.logic.on_attention_obj_identified( data, att_u_key, att_obj_data )
			end
		end
	end
	
	if att_obj_data then
		managers.groupai:state():on_criminal_suspicion_progress( att_obj_data.unit, data.unit, true )
	end
	
	return att_obj_data, is_new
end

-----------------------------------------------------------------------------

function CopLogicBase.is_alert_aggressive( alert_type )
	return CopLogicBase._AGGRESSIVE_ALERT_TYPES[ alert_type ] 
end

-----------------------------------------------------------------------------

function CopLogicBase.is_alert_dangerous( alert_type )
	return CopLogicBase._DANGEROUS_ALERT_TYPES[ alert_type ] 
end

-----------------------------------------------------------------------------

function CopLogicBase._evaluate_reason_to_surrender( data, my_data, aggressor_unit )
	--print( "[CopLogicBase._evaluate_reason_to_surrender]" )
	local surrender_tweak = data.char_tweak.surrender
	if not surrender_tweak then
		return
	end
	
	local t = data.t
	if data.surrender_window then
		if t > data.surrender_window.window_expire_t then -- window of surrender has expired
			--print( "time-out surrender", data.surrender_window.expire_t - t )
			data.unit:brain():on_surrender_chance() -- re-new the timeout
			return -- time-out surrender
		end
	end
	
	local hold_chance = 1
	
	local surrender_chk = {}
	
	surrender_chk.health = function( health_surrender )
		local health_ratio = data.unit:character_damage():health_ratio()
		if health_ratio < 1 then
			local min_setting, max_setting
			for k, v in pairs( health_surrender ) do
				if not min_setting or min_setting.k > k then
					min_setting = { k = k, v = v }
				end
				if not max_setting or max_setting.k < k then
					max_setting = { k = k, v = v }
				end
			end
			if health_ratio < max_setting.k then
				hold_chance = hold_chance * ( 1 - math.lerp( min_setting.v, max_setting.v, math.max( 0, health_ratio - min_setting.k ) / ( max_setting.k - min_setting.k ) ) )
				--print( "health: hold_chance", hold_chance, "health_ratio", health_ratio, "min_setting", inspect( min_setting ), "max_setting", inspect( max_setting ) )
			end
		end
	end
	
	surrender_chk.aggressor_dis = function( agg_dis_surrender )
		local agg_dis = mvec3_dis( data.m_pos, aggressor_unit:movement():m_pos() )
		local min_setting, max_setting
		for k, v in pairs( agg_dis_surrender ) do
			if not min_setting or min_setting.k > k then
				min_setting = { k = k, v = v }
			end
			if not max_setting or max_setting.k < k then
				max_setting = { k = k, v = v }
			end
		end
		if agg_dis < max_setting.k then
			hold_chance = hold_chance * ( 1 - math.lerp( min_setting.v, max_setting.v, math.max( 0, agg_dis - min_setting.k ) / ( max_setting.k - min_setting.k ) ) )
			--print( "agg_dis: hold_chance", hold_chance, "agg_dis", agg_dis, "min_setting", inspect( min_setting ), "max_setting", inspect( max_setting ) )
		end
	end
	
	surrender_chk.weapon_down = function( weap_down_surrender )
		local anim_data = data.unit:anim_data()
		if anim_data.reload then
			hold_chance = hold_chance * ( 1 - weap_down_surrender )
			--print( "reload: hold_chance", hold_chance, "weap_down_surrender", weap_down_surrender )
		elseif anim_data.hurt then
			hold_chance = hold_chance * ( 1 - weap_down_surrender )
			--print( "hurt: hold_chance", hold_chance, "weap_down_surrender", weap_down_surrender )
		elseif data.unit:movement():stance_name() == "ntl" then
			hold_chance = hold_chance * ( 1 - weap_down_surrender )
			--print( "ntl: hold_chance", hold_chance, "weap_down_surrender", weap_down_surrender )
		end
		
		local ammo_max, ammo = data.unit:inventory():equipped_unit():base():ammo_info()
		if ammo == 0 then
			hold_chance = hold_chance * ( 1 - weap_down_surrender )
			--print( "no_ammo: hold_chance", hold_chance, "weap_down_surrender", weap_down_surrender )
		end
	end
	
	surrender_chk.flanked = function( flanked_surrender )
		local dis = mvec3_dir( tmp_vec1, data.m_pos, aggressor_unit:movement():m_pos() )
		if dis > 250 then
			local fwd = data.unit:movement():m_rot():y()
			local fwd_dot = mvec3_dot( fwd, tmp_vec1 )
			if fwd_dot < -0.5 then
				hold_chance = hold_chance * ( 1 - flanked_surrender )
				--print( "fwd_dot: hold_chance", hold_chance, "flanked_surrender", flanked_surrender )
			end
		end
	end
	
	surrender_chk.unaware_of_aggressor = function( unaware_of_aggressor_surrender )
		local att_info = data.detected_attention_objects[ aggressor_unit:key() ]
		if not att_info or not att_info.identified or t - att_info.identified_t < 1 then
			hold_chance = hold_chance * ( 1 - unaware_of_aggressor_surrender )
			--print( "unaware_of_aggressor: hold_chance", hold_chance, "unaware_of_aggressor_surrender", unaware_of_aggressor_surrender )
		end
	end
	
	surrender_chk.enemy_weap_cold = function( enemy_weap_cold_surrender )
		if not managers.groupai:state():enemy_weapons_hot() then
			hold_chance = hold_chance * ( 1 - enemy_weap_cold_surrender )
			--print( "enemy_weap_cold: hold_chance", hold_chance, "enemy_weap_cold_surrender", enemy_weap_cold_surrender )
		end
	end
	
	surrender_chk.isolated = function( isolated_surrender )
		if data.group and data.group.has_spawned and data.group.initial_size > 1 then
			local has_support
			local max_dis_sq = 850 * 850
			for u_key, u_data in pairs( data.group.units ) do
				if u_key ~= data.key then
					if mvec3_dis_sq( data.m_pos, u_data.m_pos ) < max_dis_sq then
						has_support = true
						break
					end
				end
				if not has_support then
					hold_chance = hold_chance * ( 1 - isolated_surrender )
					--print( "isolation: hold_chance", hold_chance, "isolated_surrender", isolated_surrender )
				end
			end
		end
	end
	
	surrender_chk.pants_down = function( pants_down_surrender )
		local not_cool_t = data.unit:movement():not_cool_t()
		if ( not not_cool_t or t - not_cool_t < 1.5 ) and not managers.groupai:state():enemy_weapons_hot() then
			hold_chance = hold_chance * ( 1 - pants_down_surrender )
			--print( "pants_down: hold_chance", hold_chance, "pants_down_surrender", pants_down_surrender )
		end
	end
	
	--print( "checking reasons" )
	for reason, reason_data in pairs( surrender_tweak.reasons ) do
		surrender_chk[ reason ]( reason_data )
	end
	
	if hold_chance >= 1 - ( surrender_tweak.significant_chance or 0 ) then
		--print( "no reason" )
		return 1
	end
	
	--print( "checking factors" )
	for factor, factor_data in pairs( surrender_tweak.factors ) do
		surrender_chk[ factor ]( factor_data )
	end
	
	if data.surrender_window then
		hold_chance = hold_chance * ( 1 - data.surrender_window.chance_mul )
		--print( "surrender_window: hold_chance", hold_chance, "data.surrender_window.chance_mul", data.surrender_window.chance_mul )
	end
	
	if surrender_tweak.violence_timeout then
		local violence_t = data.unit:character_damage():last_suppression_t()
		if violence_t then
			local violence_dt = t - violence_t
			if violence_dt < surrender_tweak.violence_timeout then
				hold_chance = hold_chance + ( 1 - hold_chance ) * ( 1 - violence_dt / surrender_tweak.violence_timeout )
				--print( "violence_timeout: hold_chance", hold_chance, "violence_dt", violence_dt, "violence_timeout", surrender_tweak.violence_timeout )
			end
		end
	end
	
	return hold_chance < 1 and hold_chance
end

-----------------------------------------------------------------------------

function CopLogicBase._can_arrest( data )
	return not ( data.char_tweak.no_arrest or data.objective and data.objective.no_arrest )
end

-----------------------------------------------------------------------------

function CopLogicBase.on_attention_obj_identified( data, attention_u_key, attention_info )
	if data.group then
		for u_key, u_data in pairs( data.group.units ) do
			if u_key ~= data.key then
				u_data.unit:brain():clbk_group_member_attention_identified( data.unit, attention_u_key )
			end
		end
	end
	
	if attention_info.settings.reaction >= AIAttentionObject.REACT_SCARED then
		managers.groupai:state():on_criminal_suspicion_progress( attention_info.unit, data.unit, true )
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.on_suppressed_state( data )
	if data.is_suppressed and data.objective then
		local allow_trans, interrupt = CopLogicBase.is_obstructed( data, data.objective, nil, nil )
		if interrupt then
			--debug_pause_unit( data.unit, "[CopLogicBase.on_suppressed_state] objective interrupted due to suppression", inspect( data.objective ) )
			managers.groupai:state():on_objective_failed( data.unit, data.objective )
		end
	end
end

-----------------------------------------------------------------------------

function CopLogicBase.chk_start_action_dodge( data, reason )
	--print( "[CopLogicBase.chk_start_action_dodge]", reason )
	if not ( data.char_tweak.dodge and data.char_tweak.dodge.occasions[ reason ] ) then
		--print( "no tweak" )
		return -- this character never dodges for this reason
	end
	
	-- 1. check if we have timed out the previous dodge
	if data.dodge_timeout_t and data.t < data.dodge_timeout_t or 
		data.dodge_chk_timeout_t and data.t < data.dodge_chk_timeout_t or 
		data.unit:movement():chk_action_forbidden( "walk" )
	then
		--print( "cant dodge atm", data.dodge_timeout_t and ( data.dodge_timeout_t - data.t), data.dodge_chk_timeout_t and ( data.dodge_chk_timeout_t - data.t ) )
		return -- cannot dodge atm
	end
	
	local dodge_tweak = data.char_tweak.dodge.occasions[ reason ]
	
	-- 2. roll chance to dodge
	data.dodge_chk_timeout_t = TimerManager:game():time() + math.lerp( dodge_tweak.check_timeout[1], dodge_tweak.check_timeout[2], math.random() )
	
	if dodge_tweak.chance == 0 or math.random() > dodge_tweak.chance then
		--print( "chance fail" )
		return
	end
	
	-- 3. pick random variation
	local rand_nr = math.random()
	local total_chance = 0
	local variation, variation_data
	for test_variation, test_variation_data in pairs( dodge_tweak.variations ) do
		total_chance = total_chance + test_variation_data.chance
		if test_variation_data.chance > 0 and total_chance >= rand_nr then
			variation = test_variation
			variation_data = test_variation_data
			break
		end
	end
	
	--print( "variation", variation, "variation_data", inspect( variation_data ) )
	
	-- 4. choose direction
	
	local dodge_dir = Vector3()
	local face_attention
	if data.attention_obj and data.attention_obj.reaction >= AIAttentionObject.REACT_COMBAT then
		mvec3_set( dodge_dir, data.attention_obj.m_pos )
		mvec3_sub( dodge_dir, data.m_pos )
		mvector3.set_z( dodge_dir, 0 )
		mvector3.normalize( dodge_dir )
		if mvector3.dot( data.unit:movement():m_fwd(), dodge_dir ) < 0 then
			--print( "attention is behind me" )
			return
		end
		--print( "attention dir", dodge_dir )
		mvector3.cross( dodge_dir, dodge_dir, math.UP )
		--print( "dodge_dir from attention", dodge_dir )
		face_attention = true
	else
		mvector3.random_orthogonal( dodge_dir, math.UP )
		--print( "dodge_dir random", dodge_dir )
	end
	
	local dodge_dir_reversed = false
	if math.random() < 0.5 then
		mvector3.negate( dodge_dir )
		--print( "negating", dodge_dir )
		dodge_dir_reversed = not dodge_dir_reversed
	end
	--Application:set_pause( true )
	--Application:draw_cylinder( data.m_pos, data.m_pos + dodge_dir * 300, 10, 1, 0, 0 )
	--Application:draw_rotation( data.m_pos, data.unit:movement():m_rot() )
	
	local prefered_space = 130
	local min_space = 90
	
	local ray_to_pos = tmp_vec1
	mvec3_set( ray_to_pos, dodge_dir )
	mvector3.multiply( ray_to_pos, 130 )
	mvector3.add( ray_to_pos, data.m_pos )
	local ray_params = {
		tracker_from = data.unit:movement():nav_tracker(),
		pos_to = ray_to_pos,
		trace = true
	}
	
	local ray_hit1 = managers.navigation:raycast( ray_params )
	local dis
	if ray_hit1 then -- we hit an obstacle on this side. check if there is more space to the other side
		--Application:draw_sphere( ray_params.trace[1], 5, 0, 0, 1 )
		dis = mvector3.distance( ray_params.trace[1], data.m_pos )
		--print( "ray_hit1 dis", dis )
		mvec3_set( ray_to_pos, dodge_dir )
		mvector3.multiply( ray_to_pos, -130 )
		mvector3.add( ray_to_pos, data.m_pos )
		ray_params.pos_to = ray_to_pos
		local ray_hit2 = managers.navigation:raycast( ray_params )
		if ray_hit2 then
			--Application:draw_sphere( data.m_pos, 4, 0, 0, 0.7 )
			local prev_dis = dis
			dis = mvector3.distance( ray_params.trace[1], data.m_pos )
			--print( "ray_hit2 dis", dis )
			if dis > prev_dis and dis > min_space then
				--print( "negating due to obstacle" )
				mvector3.negate( dodge_dir )
				dodge_dir_reversed = not dodge_dir_reversed
			end
		else
			--print( "negating due to obstacle" )
			mvector3.negate( dodge_dir )
			dis = nil
			dodge_dir_reversed = not dodge_dir_reversed
		end
	end
	
	if ray_hit1 and dis and dis < min_space then
		--print( "not enough space" )
		return
	end
	
	local dodge_side
	if face_attention then -- choose side that rotates us towards our attention
		dodge_side = dodge_dir_reversed and "l" or "r"
	else -- choose side closest to the wanted rotation
		local fwd_dot = mvec3_dot( dodge_dir, data.unit:movement():m_fwd() )
		local my_right = tmp_vec1
		mrotation.x( data.unit:movement():m_rot(), my_right )
		local right_dot = mvec3_dot( dodge_dir, my_right )
		--print( "fwd_dot", fwd_dot )
		--print( "right_dot", right_dot )
		if math.abs( fwd_dot ) > 0.7071067690849 then
			if fwd_dot > 0 then
				dodge_side = "fwd"
			else
				dodge_side = "bwd"
			end
		else
			if right_dot > 0 then
				dodge_side = "r"
			else
				dodge_side = "l"
			end
		end
	end
	
	--print( "dodge_side", dodge_side )
	
	local action_data = { 
		type = "dodge",
		body_part = 1,
		variation = variation,
		side = dodge_side,
		direction = dodge_dir,
		timeout = variation_data.timeout,
		speed = data.char_tweak.dodge.speed,
		blocks = {
			walk = -1,
			action = -1,
			act = -1,
			aim = -1,
			tase = -1,
			bleedout = -1,
			dodge = -1
		}
	}
	
	if variation ~= "side_step" then -- they can play hurts while side-stepping
		action_data.blocks.hurt = -1
		action_data.blocks.heavy_hurt = -1
	end
	
	local action = data.unit:movement():action_request( action_data )
	
	if action then
		local my_data = data.internal_data
		CopLogicAttack._cancel_cover_pathing( data, my_data )
		CopLogicAttack._cancel_charge( data, my_data )
		CopLogicAttack._cancel_expected_pos_path( data, my_data )
		CopLogicAttack._cancel_walking_to_cover( data, my_data, true )
		
		if my_data.rsrv_pos.stand then
			managers.navigation:unreserve_pos( my_data.rsrv_pos.stand )
			my_data.rsrv_pos.stand = nil
		end
	end 
	
	return action
end

-----------------------------------------------------------------------------

function CopLogicBase.chk_am_i_aimed_at( data, attention_obj, max_dot )
	if not attention_obj.is_person then
		return
	end
	local enemy_look_dir = tmp_vec1
	mrotation.y( attention_obj.unit:movement():m_head_rot(), enemy_look_dir )
	local enemy_vec = tmp_vec2
	mvec3_dir( enemy_vec, attention_obj.m_head_pos, data.unit:movement():m_com() )
	return mvec3_dot( enemy_vec, enemy_look_dir ) > max_dot
end

-----------------------------------------------------------------------------
-- reduce the radius if there are obstacles. this check is done on client side and no need to be repeated. [3] exists only on server side
function CopLogicBase._chk_alert_obstructed( my_listen_pos, alert_data )
	if alert_data[3] then
		local alert_epicenter
		if alert_data[1] == "bullet" then
			alert_epicenter = tmp_vec1
			mvector3.step( alert_epicenter, alert_data[2], alert_data[6], 50 )
		else
			alert_epicenter = alert_data[2]
		end
		local ray = World:raycast( "ray", my_listen_pos, alert_epicenter, "slot_mask", managers.slot:get_mask( "AI_visibility" ), "ray_type", "ai_vision", "report" )
		if ray then
			if alert_data[1] == "footstep" then -- footsteps do not propagate through walls
				return true
			end
			local my_dis_sq = mvector3.distance( my_listen_pos, alert_epicenter )
			local dampening = alert_data[1] == "bullet" and 0.5 or 0.25
			local effective_dis_sq = alert_data[3] * dampening
			effective_dis_sq = effective_dis_sq * effective_dis_sq
			if my_dis_sq > effective_dis_sq then
				return true
			end
		end
	end
end

-----------------------------------------------------------------------------
-----------------------------------------------------------------------------


