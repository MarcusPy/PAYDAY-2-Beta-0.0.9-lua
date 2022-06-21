core:import( "CoreEvent" )
--This files contains gui animate functions

--------------------------------------------------------------------------------

--[[function HUDManager:_animate_interaction_complete( bitmap, circle )
	local TOTAL_T = 0.6
	local t = TOTAL_T
	local mul = 1
	local c_x, c_y = bitmap:center()
	local size = bitmap:w()
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		mul = mul + dt * 0.75
		bitmap:set_size( size * mul, size * mul )
		bitmap:set_center( c_x, c_y )
		bitmap:set_color( bitmap:color():with_alpha( math.max( t/TOTAL_T, 0 ) ) )
		
		circle._circle:set_size( size * mul, size * mul )
		circle._circle:set_center( c_x, c_y )
		circle:set_current( 64 * (1 - t/TOTAL_T) )
		circle:set_color( circle:color():with_alpha( math.max( t/TOTAL_T, 0 ) ) )  
	end
	bitmap:parent():remove( bitmap )
	print( "done" )
end]]

--------------------------------------------------------------------------------

--[[function HUDManager:_animate_assault( assault_panel )
	local corner = assault_panel:child( "corner" )
	local corner2 = assault_panel:child( "corner2" )
	local assault_title = assault_panel:child( "assault_title" )
	while true do
		local a = 0.25 + (math.sin( Application:time()*750 )+1)/4
		-- print( "a", a, 1 - a )
		corner:set_color( corner:color():with_alpha( a ) )
		corner2:set_color( corner2:color():with_alpha( 0.25 ) )
		assault_title:set_color( Color( a, 0, 0.8-a ) )
		-- o:set_color( o:color():with_alpha( 0.5 + (math.sin( Application:time()*750 )+1)/4 ) )
		coroutine.yield()
	end
end]]

--------------------------------------------------------------------------------

--[[function HUDManager:_animate_show_stats_left_panel( left_panel )
	-- left_panel:set_x( -left_panel:w() )
	local start_x = left_panel:x()
	local TOTAL_T = 0.33 * (start_x/-left_panel:w())
	local t = 0
	-- print( "TOTAL_T", TOTAL_T )
	while t < TOTAL_T do
	-- while left_panel:x() ~= 0 do
		local dt = coroutine.yield()
		t = t + dt
		left_panel:set_x( math.lerp( start_x, 0, t/TOTAL_T ) )
		-- self._test_circle:set_current( 10 *( t/2) )
	end
	left_panel:set_x( 0 )
	print( "done" )
end

function HUDManager:_animate_hide_stats_left_panel( left_panel )
	-- left_panel:set_x( -left_panel:w() )
	local start_x = left_panel:x()
	local TOTAL_T = 0.33 * (1 - start_x/-left_panel:w())
	local t = 0
	while t < TOTAL_T do
	-- while left_panel:x() ~= 0 do
		local dt = coroutine.yield()
		t = t + dt
		left_panel:set_x( math.lerp( start_x, -left_panel:w(), t/TOTAL_T ) )
		-- self._test_circle:set_current( 10 *( t/2) )
	end
	left_panel:set_x( -left_panel:w() )
	print( "done" )
end]]

--[[function HUDManager:_animate_weapon_switch( target, source, ammo_total, ammo_total_clone, ammo_total_secondary, ammo_total_secondary_clone, ammo_pad_secondary, CENTER_X_OBJ, CENTER_Y_OBJ )
	local TOTAL_T = 0.4
	local t = TOTAL_T
	local MAX_DIST = 20
	local PAD_SECONDARY_MAX_DIST = 30
	local MAX_SIZE = 64
	ammo_total:set_color( ammo_total:color():with_alpha( 0 ) )
	ammo_total_secondary:set_color( ammo_total_secondary:color():with_alpha( 0 ) )
	local ammo_pad_secondary_right = ammo_pad_secondary:right()
	-- local ammo_pad_secondary_hide = ammo_pad_secondary_right - 10
	local ammo_total_secondary_right = ammo_total_secondary:right()
	-- CENTER_X = math.round( CENTER_X )
	-- CENTER_Y = math.round( CENTER_Y )
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		target:set_center_y(  CENTER_Y_OBJ:center_y() + MAX_DIST * -math.sin( (180/TOTAL_T) * t ) )
		source:set_center_y(  CENTER_Y_OBJ:center_y() + MAX_DIST * math.sin( (180/TOTAL_T) * t ) )
		local secondary_offset = PAD_SECONDARY_MAX_DIST * -math.sin( (180/TOTAL_T) * t )
		ammo_pad_secondary:set_right( ammo_pad_secondary_right + secondary_offset )
		ammo_total_secondary:set_right( ammo_total_secondary_right + secondary_offset )
		ammo_total_secondary_clone:set_right( ammo_total_secondary_right + secondary_offset )
		if t < (TOTAL_T/2) then -- After half the time, start fading and scaling away the source (old weapon)
			local a = t/(TOTAL_T/2) -- 1 -> 0
			source:child( "weapon" ):set_color( source:child( "weapon" ):color():with_alpha( a ) )
			local size = (MAX_SIZE/2) * (1-a)
			source:set_size( MAX_SIZE - size, MAX_SIZE - size )
			ammo_total:set_color( ammo_total:color():with_alpha( 1-a ) )
			-- ammo_total_secondary:set_color( ammo_total_secondary:color():with_alpha( (1-a)*0.75 ) )
			-- ammo_total:set_color( ammo_total:color():with_alpha( 1 ) )
			-- ammo_total_clone:set_color( ammo_total_clone:color():with_alpha( 0 ) )
			
			ammo_total_secondary:set_color( ammo_total_secondary:color():with_alpha( (1)*0.75 ) )
			ammo_total_secondary_clone:set_color( ammo_total_secondary_clone:color():with_alpha( 0 ) )
		else  -- First half the time, start fading and scaling up the target (new weapon)
			local a = (TOTAL_T - t)/(TOTAL_T/2) -- 0 -> 1
			target:child( "weapon" ):set_color( target:child( "weapon" ):color():with_alpha( a ) )
			local size = (MAX_SIZE/2) * a
			target:set_size( (MAX_SIZE/2) + size, (MAX_SIZE/2) + size )
			
			ammo_total_clone:set_color( ammo_total_clone:color():with_alpha( 1-a ) )
			-- ammo_total_secondary_clone:set_color( ammo_total_secondary_clone:color():with_alpha( (1-a)*0.75 ) )
		end
		target:set_center_x( CENTER_X_OBJ:center_x() )
		source:set_center_x( CENTER_X_OBJ:center_x() )
	end
	target:set_size( MAX_SIZE, MAX_SIZE )
	source:set_size( MAX_SIZE/2, MAX_SIZE/2 )
	target:set_center_y( CENTER_Y_OBJ:center_y() )
	source:set_center_y( CENTER_Y_OBJ:center_y() )
	target:set_center_x( CENTER_X_OBJ:center_x() )
	source:set_center_x( CENTER_X_OBJ:center_x() )
	target:child( "weapon" ):set_color( target:child( "weapon" ):color():with_alpha( 1 ) )
	source:child( "weapon" ):set_color( source:child( "weapon" ):color():with_alpha( 0 ) )
		
	ammo_pad_secondary:set_right( ammo_pad_secondary_right )
	ammo_total_secondary:set_right( ammo_total_secondary_right )
	ammo_total_secondary_clone:set_right( ammo_total_secondary_right )
	
	ammo_total_clone:parent():remove( ammo_total_clone )
	ammo_total_secondary_clone:parent():remove( ammo_total_secondary_clone )
	-- print( "done", "CENTER_Y", CENTER_Y, "CENTER_X", CENTER_X )
end]]

function HUDManager:_animate_test_circle()
	local t = 2
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		-- self._test_circle:set_current( 10 *( t/2) )
	end
	print( "done" )
end

function HUDManager:_animate_ammo_test( panel )
	local t = 3
	panel:set_alpha( 1 )
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		if t < 2 then
			panel:set_alpha( t/2 )
		end
		-- self._test_circle:set_current( 10 *( t/2) )
	end
	panel:set_alpha( 0 )
end 

--[[
function HUDManager:_animate_activate_objective( objectives_panel )
	local objectives_pad_panel = objectives_panel:child( "objectives_pad_panel" )
	local objective_start_new = objectives_pad_panel:child( "objective_start_new" )
	local objective_mid_new = objectives_pad_panel:child( "objective_mid_new" )
	local objective_end_new = objectives_pad_panel:child( "objective_end_new" )
	local TOTAL_T = 5
	local t = TOTAL_T
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		local a = (math.sin( t * 45 * 10 )) -- t / TOTAL_T
		objective_start_new:set_color( objective_start_new:color():with_alpha( a ) )
		objective_mid_new:set_color( objective_mid_new:color():with_alpha( a ) )
		objective_end_new:set_color( objective_end_new:color():with_alpha( a ) )
	end
	objective_start_new:set_color( objective_start_new:color():with_alpha( 0 ) )
	objective_mid_new:set_color( objective_mid_new:color():with_alpha( 0 ) )
	objective_end_new:set_color( objective_end_new:color():with_alpha( 0 ) )
	-- print( "done" )
end
]]

--[[
function HUDManager:_animate_state_switch( weapon, weapon_panel1, weapon_panel2, TARGET_Y, is_player, box_ai_bg, box_bg )
	local TOTAL_T = 0.25
	local t = TOTAL_T
	local weapon_y = weapon:y()
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		local a = t/TOTAL_T
		box_ai_bg:set_color( box_ai_bg:color():with_alpha( is_player and (t) or (1-t) ) )
		box_bg:set_color( box_bg:color():with_alpha( is_player and (1-t) or (t) ) )
		
		local y = math.lerp( TARGET_Y, weapon_y, t/TOTAL_T )
		weapon:set_y( y )
		weapon_panel1:set_y( y )
		weapon_panel2:set_y( y )
	end
	weapon:set_y( TARGET_Y )
	weapon_panel1:set_y( TARGET_Y )
	weapon_panel2:set_y( TARGET_Y )
	box_ai_bg:set_color( box_ai_bg:color():with_alpha( is_player and 0 or 1 ) )
	box_bg:set_color( box_bg:color():with_alpha( is_player and 1 or 0 ) )
	print( "_animate_state_switch done" )
end]]
