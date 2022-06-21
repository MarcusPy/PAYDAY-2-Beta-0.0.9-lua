--This extention stores all animation flags

PlayerAnimationData = PlayerAnimationData or class()

function PlayerAnimationData:init( unit )
	self._unit = unit
end

-----------------------------------------------------------------------------------

function PlayerAnimationData:anim_clbk_footstep_l( unit )
	self._footstep = 'l'
	unit:base():anim_data_clbk_footstep( "left" )
	--cat_print( "george", "L", self._unit:anim_state_machine():segment_state( Idstring( "base" ) ) )
end

-----------------------------------------------------------------------------------

function PlayerAnimationData:anim_clbk_footstep_r( unit )
	self._footstep = 'r'
	unit:base():anim_data_clbk_footstep( "right" )
	--cat_print( "george", "R", self._unit:anim_state_machine():segment_state( Idstring( "base" ) ) )
end

-----------------------------------------------------------------------------------

function PlayerAnimationData:anim_clbk_startfoot_l( unit )
	self._footstep = 'l'
	--cat_print( "george", "L start", self._unit:anim_state_machine():segment_state( Idstring( "base" ) ) )
end

-----------------------------------------------------------------------------------

function PlayerAnimationData:anim_clbk_startfoot_r( unit )
	self._footstep = 'r'
	--cat_print( "george", "R start", self._unit:anim_state_machine():segment_state( Idstring( "base" ) ) )
end

-----------------------------------------------------------------------------------

-- Returns the 'r' if right foot is down, and vice versa. nil if the unit has not taken any footsteps
function PlayerAnimationData:foot()
	return self._footstep
end

-----------------------------------------------------------------------------------

function PlayerAnimationData:anim_clbk_upper_body_empty( unit )
	unit:anim_state_machine():stop_segment( Idstring( "upper_body" ) )
end

-----------------------------------------------------------------------------------

function PlayerAnimationData:anim_clbk_base_empty( unit )
	unit:anim_state_machine():stop_segment( Idstring( "base" ) )
end

-----------------------------------------------------------------------------------
-- This call means we have blended completely into our death pose
function PlayerAnimationData:anim_clbk_death_exit( unit )
	unit:movement():on_death_exit()
	unit:base():on_death_exit()
	if unit:inventory() then
		unit:inventory():on_death_exit()
	end
end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------