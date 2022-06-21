core:import( "CoreLocalizationManager" )
core:import( "CoreClass" )

LocalizationManager = LocalizationManager or class( CoreLocalizationManager.LocalizationManager )

function LocalizationManager:init()
	LocalizationManager.super.init( self )

	self:_setup_macros()
	Application:set_default_letter( 95 ) -- The unicode letter for "_"
end

function LocalizationManager:_setup_macros()
	--[[
	57344--> A
	57345--> B
	57346--> X
	57347--> Y
	57348--> BACK
	57349--> START
	57350--> L_STICK
	57351--> R_STICK
	57352--> TOP_L
	57353--> TOP_R
	57354--> BOT_L
	57355--> BOT_R
	]]

	-- Hardwired icons
	local btn_a = utf8.char(57344)
	local btn_b = utf8.char(57345)
	local btn_x = utf8.char(57346)
	local btn_y = utf8.char(57347)
	local btn_back = utf8.char(57348)
	local btn_start = utf8.char(57349)
	local stick_l = utf8.char(57350)
	local stick_r = utf8.char(57351)
	local btn_top_l = utf8.char(57352)
	local btn_top_r = utf8.char(57353)
	local btn_bottom_l = utf8.char(57354)
	local btn_bottom_r = utf8.char(57355)
	local btn_stick_l = utf8.char(57356)
	local btn_stick_r = utf8.char(57357)
	
	local btn_dpad_u = utf8.char(57358)
	local btn_dpad_d = utf8.char(57358)
	local btn_dpad_l = utf8.char(57358)
	local btn_dpad_r = utf8.char(57358)
	
	local btn_inv_new = utf8.char(57362)
	
	
	if SystemInfo:platform() ~= Idstring("PS3") then
		btn_top_l = utf8.char(57354)
		btn_bottom_l = utf8.char(57352)
		btn_top_r = utf8.char(57355)
		btn_bottom_r = utf8.char(57353)
	end

	-- Abstract icons
	local btn_accept = btn_a
	local btn_cancel = btn_b
	-- The following are just placeholder names since the game haven't
	-- been fully designed yet. They will need to change as we get a
	-- more complete design. We will also have to add a lot of buttons.
	local btn_attack = btn_a
	local btn_block = btn_b
	
	local btn_interact = btn_bottom_r
	local btn_primary = btn_top_r
	local btn_use_item = btn_bottom_l
	local btn_secondary = btn_top_l
	local btn_reload = btn_x
	local btn_jump = btn_a
	
	-- Swap the accept and cancel buttons if applicable
	-- The PS3 has it's buttons swapped on the Japanese SKU for example
	local swap_accept = false
	if SystemInfo:platform() == Idstring("PS3") and PS3:pad_cross_circle_inverted() then
		swap_accept = true
	end
	if swap_accept then
		btn_accept = btn_b
		btn_cancel = btn_a
	end
	

	-- We only want to use BTN_STICK_L and BTN_STICK_R on the PS3
	-- So for other platforms we overwrite those macros with
	-- the normal character
	if SystemInfo:platform() ~= Idstring("PS3") then
		btn_stick_r = stick_r
		btn_stick_l = stick_l
		-- Also fix asbtract icons here!
	end

	-- Hardwired macros
	self:set_default_macro("BTN_BACK", btn_back)
	self:set_default_macro("BTN_START", btn_start)
	self:set_default_macro("BTN_A", btn_a)
	self:set_default_macro("BTN_B", btn_b)
	self:set_default_macro("BTN_X", btn_x)
	self:set_default_macro("BTN_Y", btn_y)
	self:set_default_macro("BTN_TOP_L", btn_top_l)
	self:set_default_macro("BTN_TOP_R", btn_top_r)
	self:set_default_macro("BTN_BOTTOM_L", btn_bottom_l)
	self:set_default_macro("BTN_BOTTOM_R", btn_bottom_r)
	self:set_default_macro("BTN_STICK_L", btn_stick_l)
	self:set_default_macro("BTN_STICK_R", btn_stick_r)
	self:set_default_macro("STICK_L", stick_l)
	self:set_default_macro("STICK_R", stick_r)

	-- Abstract macros
	self:set_default_macro("BTN_INTERACT", btn_interact)
	self:set_default_macro("BTN_USE_ITEM", btn_use_item)
	self:set_default_macro("BTN_PRIMARY", btn_primary)
	self:set_default_macro("BTN_SECONDARY", btn_secondary)
	self:set_default_macro("BTN_RELOAD", btn_reload)
	self:set_default_macro("BTN_JUMP", btn_jump)
		
	self:set_default_macro("BTN_ACCEPT", btn_accept)
	self:set_default_macro("BTN_CANCEL", btn_cancel)
	-- And the placeholder gameplay buttons
	self:set_default_macro("BTN_ATTACK", btn_attack)
	self:set_default_macro("BTN_BLOCK", btn_block)
	
	self:set_default_macro("CONTINUE", btn_a)
	self:set_default_macro("BTN_GADGET", btn_dpad_u)
	
	self:set_default_macro("BTN_INV_NEW", btn_inv_new)
end

local is_PS3 = SystemInfo:platform() == Idstring("PS3")
function LocalizationManager:btn_macro( button, to_upper )
	if not managers.menu:is_pc_controller() then -- is_PS3 then
	 return
	end
	
	local type = managers.controller:get_default_wrapper_type()
	local text = "["..managers.controller:get_settings( type ):get_connection( button ):get_input_name_list()[1].."]"
	return to_upper and utf8.to_upper( text ) or text
end

function LocalizationManager:ids( file )
	return Localizer:ids( Idstring( file ) )
end

function LocalizationManager:to_upper_text( string_id, macros )
	return utf8.to_upper( self:text( string_id, macros ) )
end

function LocalizationManager:debug_file( file )
	local t = {}
	local ids_in_file = self:ids( file )
	for i,ids in ipairs( ids_in_file ) do
		local s = ids:s()
		local text = self:text( s, { BTN_INTERACT = self:btn_macro( "interact" ) } ) -- Could perhaps add more default macros here? 
		t[ s ] = text
	end
	-- print( "\nTotal", #ids_in_file )
	return t
end

CoreClass.override_class( CoreLocalizationManager.LocalizationManager, LocalizationManager )
