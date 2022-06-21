PlayerTweakData = PlayerTweakData or class()

function PlayerTweakData:_set_easy()
	self.damage.automatic_respawn_time = 150  -- time it takes to get respawned without trading (sec)
end

function PlayerTweakData:_set_normal()
	self.damage.automatic_respawn_time = 120  -- time it takes to get respawned without trading (sec)
end

function PlayerTweakData:_set_hard()
	self.damage.automatic_respawn_time = 220
	
	self.damage.DOWNED_TIME_DEC = 7							-- Amount of time decreased from bleed out and fatal timer every time you go down
	self.damage.DOWNED_TIME_MIN = 5						-- Minimum bleed out and fatal timer
end

function PlayerTweakData:_set_overkill()
	self.damage.DOWNED_TIME_DEC = 10							-- Amount of time decreased from bleed out and fatal timer every time you go down
	self.damage.DOWNED_TIME_MIN = 5						-- Minimum bleed out and fatal timer
end

function PlayerTweakData:_set_overkill_145()
	self.damage.DOWNED_TIME_DEC = 15							-- Amount of time decreased from bleed out and fatal timer every time you go down
	self.damage.DOWNED_TIME_MIN = 1						-- Minimum bleed out and fatal timer
	
	self.damage.REVIVE_HEALTH_STEPS = { 0.2 }
end

function PlayerTweakData:init()
	local is_console = SystemInfo:platform() ~= Idstring( "WIN32" )
	
	self.arrest = {
		arrest_timeout = 60 * 4, -- minimum delay between arrest attempts. (sec)
		aggression_timeout = 60 -- do not attempt to arrest if the player has performed aggression in this timeframe. (sec)
	}
	
	self.put_on_mask_time = 2
	
	self.damage = {}
	
	if(is_console) then
		self.damage.ARMOR_INIT = 5								-- Amount of armor, an non depleted step can be regenerated           -- Amount of armor before losing health (this can be generated)
	else
		self.damage.ARMOR_INIT = 2								-- Amount of armor, an non depleted step can be regenerated           -- Amount of armor before losing health (this can be generated)
	end
	
	self.damage.ARMOR_STEPS = 1									-- Amount of armor steps
	self.damage.ARMOR_DAMAGE_REDUCTION = 0.0		-- Amount of damage reduction on health the armor reduces
	self.damage.ARMOR_DAMAGE_REDUCTION_STEPS = { 1, 0.6, 0.7, 0.8, 0.9, 0.95, 0.96, 0.97, 0.98, 0.99 }
	
	self.damage.HEALTH_INIT = 20							-- The full health of the player unit in duel mode at level 1. (Absolute value)
	self.damage.LIVES_INIT = 4								-- How many lives the player have, you loose one life each time you go into bleedout, when at 0 you go automatically into custody. REMEMBER: This is _not_ how many times you can be downed/revived (its actually one less then that)

	if(is_console) then
		self.damage.REGENERATE_TIME = 2.0					-- Amount of time to pass before regenerate
	else
		self.damage.REGENERATE_TIME = 2.7					-- Amount of time to pass before regenerate
	end
	
	self.damage.REVIVE_HEALTH_STEPS = { 0.5 } -- , 0.5, 0.25 }
	
	self.damage.BLEED_OT_TIME = 40						-- Amount of time in bleed out state
	
	self.damage.TASED_TIME = 10					    	-- Duration of tased state
	self.damage.TASED_RECOVER_TIME = 1				-- How long it takes for a tased player to recover to standard movement state
	self.damage.BLEED_OUT_HEALTH_INIT = 10			-- Health before going into fatal when in bleed out	
	
	self.damage.DOWNED_TIME = 30							-- Total amount of time spent in bleed out and fatal (before death cam)
	self.damage.DOWNED_TIME_DEC = 5							-- Amount of time decreased from bleed out and fatal timer every time you go down
	self.damage.DOWNED_TIME_MIN = 10						-- Minimum bleed out and fatal timer
	self.damage.ARRESTED_TIME = 60						-- Total amount of time spent in arrested (before death cam)
	self.damage.INCAPACITATED_TIME = 30				-- Total amount of time spent in incapacitated (before death cam)
	
	self.damage.MIN_DAMAGE_INTERVAL = 0.35				-- How often the player can take damage. If the damage received is larger than the previous one, the timer is overriden. (sec)
	
	self.damage.respawn_time_penalty = 30  -- extra respawn delay per hostage murdered (sec)
	self.damage.base_respawn_time_penalty = 5 -- the respawn penalty if no hostages are killed (sec)
	
	self.fall_health_damage = 4
	self.fall_damage_alert_size = 250
	
	self.long_dis_interaction = {
		intimidate_strength = 0.5, -- shouting at a civilian once results in him staying calm for that percentage of his maximum intimidation time
		highlight_range = 3000,
		intimidate_range_enemies = 1000,
		intimidate_range_civilians = 1000
	}
	
-- TODO : Temporarily disabled, sortoff
	self.suppression = {
		max_value = 20, -- how long it takes for suppression to decay from full to zero. (sec)
		decay_start_delay = 1, -- how long it takes after a shot for suppression to start decaying. (sec)
		receive_mul = 10, -- multiplier aplied to all incomming suppression shots 
		spread_mul = 1.0, -- multiplier applied to weapon spread at full suppression. Linearly interpolated.
		autohit_chance_mul = 1.0, -- multiplier applied to weapon autohit at full suppression. Linearly interpolated.
		tolerance = 1 -- what value of suppression does the player need to build in order to start sufferring suppression penalties
	}
	
	self.suspicion = {
		max_value = 8, -- how long it takes for suspicion to decay from full to zero. (sec)
		range_mul = 1, -- multiplied to the attention range
		buildup_mul = 1 -- multiplied to the amount of buildup per second
	}
	
	self.alarm_pager = {
		ring_delay = { 2, 5 }, -- how long it takes before HQ calls the victim's radio. (sec)
		ring_reminder = { 4, 4 }, -- reminders are played with this interval. sec
		ring_duration = { 12, 12 }, -- how long does the operator wait before hanging up. (sec)
		bluff_success_chance = { 1, 1, 0 }, -- the bluff succes chance for zero, one, two... etc previous successful bluffs. The last number is min chance.
		bluff_success_chance_w_skill = { 1, 1, 1, 1, 0 } -- as above but with the cleaner skill.
	}
	
	self.TRANSITION_DURATION = 0.23
	
	self.stances = { default = { standard={ head={},shoulders={},vel_overshot={} }, crouched={ head={},shoulders={},vel_overshot={} }, steelsight={ shoulders={},vel_overshot={} } } }
	
	--	Standard stance
	self.stances.default.standard.head.translation = Vector3( 0, 0, 145 )
	self.stances.default.standard.head.rotation = Rotation()
	
	self.stances.default.standard.shakers = {}
	self.stances.default.standard.shakers.breathing = {}
	self.stances.default.standard.shakers.breathing.amplitude = 0.3
	
	self.stances.default.crouched.shakers = {}
	self.stances.default.crouched.shakers.breathing = {}
	self.stances.default.crouched.shakers.breathing.amplitude = 0.25
	
	self.stances.default.steelsight.shakers = {}
	self.stances.default.steelsight.shakers.breathing = {}
	self.stances.default.steelsight.shakers.breathing.amplitude = 0.025
	
	self.stances.default.mask_off = deep_clone( self.stances.default.standard )
	self.stances.default.mask_off.head.translation = Vector3( 0, 0, 160 ) -- Changing this is dangerous, clipping with world will happen if outside mover. 
	self.stances.default.clean = deep_clone( self.stances.default.mask_off )		
	
	local pivot_head_translation = Vector3(7,16,-0.5) -- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation( 0, 1, 0 ) -- camera:rotation():inverse() * pivot:rotation()
	local pivot_shoulder_translation = Vector3(8.40486, 33.2872, -0.76365) --Vector3(8.54165, 33.3334, -1.14068) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(0.151694, 0.644497, -1.1892) --  Rotation(-0.581886, 0.123524, -1.57262) -- camera:rotation():inverse() * pivot:rotation()
	
	self.stances.default.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.default.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.default.standard.vel_overshot.yaw_neg = 12
	self.stances.default.standard.vel_overshot.yaw_pos = -12
	self.stances.default.standard.vel_overshot.pitch_neg = -10
	self.stances.default.standard.vel_overshot.pitch_pos = 10
	self.stances.default.standard.vel_overshot.pivot = Vector3( 20, 50, 0 )
	self.stances.default.standard.FOV = 65
	
	-- Crouched stance
	self.stances.default.crouched.head.translation = Vector3( 0, 0, 75 )
	self.stances.default.crouched.head.rotation = Rotation()
	local pivot_head_translation = Vector3(6,16,-0.5)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(-1, 1, 0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.default.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.default.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.default.crouched.vel_overshot.yaw_neg = 12
	self.stances.default.crouched.vel_overshot.yaw_pos = -12
	self.stances.default.crouched.vel_overshot.pitch_neg = -10
	self.stances.default.crouched.vel_overshot.pitch_pos = 10
	self.stances.default.crouched.vel_overshot.pivot = Vector3( 20, 50, 0 )
	self.stances.default.crouched.FOV = self.stances.default.standard.FOV
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,7,0)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(0,0,0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.default.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.default.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.default.steelsight.vel_overshot.yaw_neg = 4
	self.stances.default.steelsight.vel_overshot.yaw_pos = -4
	self.stances.default.steelsight.vel_overshot.pitch_neg = -2
	self.stances.default.steelsight.vel_overshot.pitch_pos = 2
	self.stances.default.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.default.steelsight.zoom_fov = true
	self.stances.default.steelsight.FOV = self.stances.default.standard.FOV
	
	-- M4
	--[[
	self.stances.m4 = deep_clone( self.stances.default )
	self.stances.m4.steelsight.vel_overshot.yaw_neg = 12
	self.stances.m4.steelsight.vel_overshot.yaw_pos = -12
	self.stances.m4.steelsight.vel_overshot.pitch_neg = -12
	self.stances.m4.steelsight.vel_overshot.pitch_pos = 12
	-- END M4
	
	-- MP5
	self.stances.mp5 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.4541, 35.4915, -2.955)--10.5549, 35.3249, -2.91334) -- Vector3(7.54276, 25.5695, -2.58283) -- Vector3(8.60408, 31.5854, -3.89877) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-0.286163, 0.214701, 0.878467)---0.777848, 0.241479, 0.631354) -- Rotation(-0.643807, -0.756246, -2.07243) -- Rotation(-0.58189, -0.416477, -1.57262) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(6,17,-0.5)
	local pivot_head_rotation = Rotation( 0, -2, 0 )
	self.stances.mp5.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mp5.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mp5.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,10,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.mp5.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mp5.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mp5.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.mp5.steelsight.vel_overshot.yaw_neg = 10
	self.stances.mp5.steelsight.vel_overshot.yaw_pos = -10
	self.stances.mp5.steelsight.vel_overshot.pitch_neg = -10
	self.stances.mp5.steelsight.vel_overshot.pitch_pos = 10
	
	-- Crouched stance
	local pivot_head_translation = Vector3(5,17,-0.5)
	local pivot_head_rotation = Rotation(-1, -2, 0)
	self.stances.mp5.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mp5.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mp5.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END MP5
	
	-- MAC 11
	self.stances.mac11 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(7.48993, 40.865, -1.38648) -- 7.49773, 40.81, -1.446) -- Vector3(7.54276, 25.5695, -2.58283) -- Vector3(8.60408, 31.5854, -3.89877) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-0.752184, -0.37808, 2.04525) -- -1.07004, -0.594584, 1.7833) -- Rotation(-0.643807, -0.756246, -2.07243) -- Rotation(-0.58189, -0.416477, -1.57262) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(10,20,-1.7)
	local pivot_head_rotation = Rotation( 0, -1, 0 )
	self.stances.mac11.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mac11.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mac11.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,16,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.mac11.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mac11.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mac11.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.mac11.steelsight.vel_overshot.yaw_neg = 10
	self.stances.mac11.steelsight.vel_overshot.yaw_pos = -10
	self.stances.mac11.steelsight.vel_overshot.pitch_neg = -10
	self.stances.mac11.steelsight.vel_overshot.pitch_pos = 10
	
	-- Crouched stance
	local pivot_head_translation = Vector3(7,20,-1.7)
	local pivot_head_rotation = Rotation( -1, -1, 0)
	self.stances.mac11.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mac11.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mac11.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END MAC 11
	
	-- R870 Shotgun
	self.stances.r870_shotgun = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(0) -- Vector3(8.73742, 54.6102, -3.44525)  -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(0) -- Rotation(-1.35622, 0.649671, -0.183373) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,20,-2)
	local pivot_head_rotation = Rotation( -2, -2, 0 )
	self.stances.r870_shotgun.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.r870_shotgun.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.r870_shotgun.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,15,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.r870_shotgun.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.r870_shotgun.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.r870_shotgun.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.r870_shotgun.steelsight.vel_overshot.yaw_neg = 12
	self.stances.r870_shotgun.steelsight.vel_overshot.yaw_pos = -12
	self.stances.r870_shotgun.steelsight.vel_overshot.pitch_neg = -12
	self.stances.r870_shotgun.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,17,-2)
	local pivot_head_rotation = Rotation( -2, -2, 0 )
	self.stances.r870_shotgun.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.r870_shotgun.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.r870_shotgun.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END R870 Shotgun
	
	-- MOSSBERG Shotgun
	self.stances.mossberg = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.08536, 37.3747, -3.28995) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-0.835943, 0.75668, -0.12396) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance 10,18,-2  10,25,-2
	local pivot_head_translation = Vector3(6,15,-6)
	local pivot_head_rotation = Rotation( 0, 0, 2 )
	self.stances.mossberg.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mossberg.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mossberg.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,25,0)
	local pivot_head_rotation = Rotation(0,-2,0)
	self.stances.mossberg.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mossberg.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mossberg.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -15, 0)
	self.stances.mossberg.steelsight.vel_overshot.yaw_neg = 12
	self.stances.mossberg.steelsight.vel_overshot.yaw_pos = -12
	self.stances.mossberg.steelsight.vel_overshot.pitch_neg = -12
	self.stances.mossberg.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance  10,25,-2
	local pivot_head_translation = Vector3(4,20,-3)
	local pivot_head_rotation = Rotation( 1, 0, 0 )
	self.stances.mossberg.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mossberg.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mossberg.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END MOSSBERG Shotgun
	
	-- HAND GUN
	self.stances.beretta92 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.27388, 54.885, -1.42176)  -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-0.838946, 0.825787, -4.85158) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,29,-2)
	local pivot_head_rotation = Rotation( -2, -1, 0 )
	self.stances.beretta92.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.beretta92.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.beretta92.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.beretta92.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.beretta92.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.beretta92.steelsight.zoom_fov = false
	self.stances.beretta92.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.beretta92.steelsight.vel_overshot.yaw_neg = 12
	self.stances.beretta92.steelsight.vel_overshot.yaw_pos = -12
	self.stances.beretta92.steelsight.vel_overshot.pitch_neg = -12
	self.stances.beretta92.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(3,28,-2)
	local pivot_head_rotation = Rotation(-1, -1, 0)
	self.stances.beretta92.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.beretta92.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.beretta92.crouched.vel_overshot.pivot = pivot_shoulder_translation
	-- END HAND GUN
	
	-- .45 --------------------------------------------------------------------
	self.stances.c45 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.59479, 54.7405, -0.624365) -- Vector3(8.62354, 54.9134, -1.71595) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-0.807596, 1.47504, -1.76123) --Rotation(-1.51181, 0.508335, -5.25848) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,29,-2)
	local pivot_head_rotation = Rotation( -2, -1, 0 )
	self.stances.c45.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.c45.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.c45.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.c45.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.c45.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.c45.steelsight.zoom_fov = false
	self.stances.c45.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.c45.steelsight.vel_overshot.yaw_neg = 12
	self.stances.c45.steelsight.vel_overshot.yaw_pos = -12
	self.stances.c45.steelsight.vel_overshot.pitch_neg = -12
	self.stances.c45.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(3,28,-2)
	local pivot_head_rotation = Rotation(-1, -1, 0)
	self.stances.c45.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.c45.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.c45.crouched.vel_overshot.pivot = pivot_shoulder_translation
	-- END .45 --------------------------------------------------------------------
	
	-- Raging Bull --------------------------------------------------------------------
	self.stances.raging_bull = deep_clone( self.stances.default )
		
	local pivot_shoulder_translation = Vector3(9.36965, 58.7379, -1.8196) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-1.71169, 0.000728827, 8.77357e-005) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,35,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.raging_bull.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.raging_bull.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.raging_bull.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,28,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.raging_bull.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.raging_bull.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.raging_bull.steelsight.zoom_fov = false
	self.stances.raging_bull.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.raging_bull.steelsight.vel_overshot.yaw_neg = 12
	self.stances.raging_bull.steelsight.vel_overshot.yaw_pos = -12
	self.stances.raging_bull.steelsight.vel_overshot.pitch_neg = -12
	self.stances.raging_bull.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(3,33,-2)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.raging_bull.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.raging_bull.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.raging_bull.crouched.vel_overshot.pivot = pivot_shoulder_translation
	-- END Raging Bull --------------------------------------------------------------------
	
	-- M14 --------------------------------------------------------------------
	self.stances.m14 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.40396, 33.2866, -0.763476)
	local pivot_shoulder_rotation = Rotation(0.151441, 0.645593, -1.19106)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,16,-0.5)
	local pivot_head_rotation = Rotation( 0, 1, 0 )
	self.stances.m14.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m14.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m14.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,7,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.m14.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m14.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m14.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.m14.steelsight.vel_overshot.yaw_neg = 12
	self.stances.m14.steelsight.vel_overshot.yaw_pos = -12
	self.stances.m14.steelsight.vel_overshot.pitch_neg = -12
	self.stances.m14.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,16,-0.5)
	local pivot_head_rotation = Rotation(-1, 1, 0)
	self.stances.m14.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m14.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m14.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END M14 --------------------------------------------------------------------
	
	-- HK21 --------------------------------------------------------------------
	self.stances.hk21 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(17.7884, 25.4958, -5.87593)-- 18.2228, 26.969, -8.72573)
	local pivot_shoulder_rotation = Rotation(-15.9596, -1.5637, -1.19957)-- -15.9601, -1.56346, -1.2002)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,15,-3.5)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.hk21.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.hk21.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.hk21.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,7,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.hk21.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.hk21.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.hk21.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.hk21.steelsight.vel_overshot.yaw_neg = 18
	self.stances.hk21.steelsight.vel_overshot.yaw_pos = -18
	self.stances.hk21.steelsight.vel_overshot.pitch_neg = -16
	self.stances.hk21.steelsight.vel_overshot.pitch_pos = 16
		
	-- Crouched stance
	local pivot_head_translation = Vector3(6,16,-0.5)
	local pivot_head_rotation = Rotation(-1, 1, 0)
	self.stances.hk21.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.hk21.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.hk21.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END HK21 --------------------------------------------------------------------
	
	
		--------------------------------PLAYER TWEAK DATA--------------------------
	-- M79 --------------------------------------------------------------------
	self.stances.m79 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.58595, 40.5661, -2.41104)
	local pivot_shoulder_rotation = Rotation(0.143334, 1.04565, -1.1923)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5.2,16,-0.5)
	local pivot_head_rotation = Rotation( 0, 1, 0 )
	self.stances.m79.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m79.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m79.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,7,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.m79.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m79.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m79.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.m79.steelsight.vel_overshot.yaw_neg = 12
	self.stances.m79.steelsight.vel_overshot.yaw_pos = -12
	self.stances.m79.steelsight.vel_overshot.pitch_neg = -12
	self.stances.m79.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4.9,19,-0.5)
	local pivot_head_rotation = Rotation(-1, 1, 0)
	self.stances.m79.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m79.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m79.crouched.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END M79 --------------------------------------------------------------------
	

	-- AK47 --------------------------------------------------------------------
	self.stances.ak47 = deep_clone( self.stances.default )
	
	local pivot_head_translation = Vector3(6,15,-0.5) -- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation( 0, 0, 0 ) -- camera:rotation():inverse() * pivot:rotation()
	local pivot_shoulder_translation = Vector3(8.61463, 33.4594, -3.75638) --Vector3(8.54165, 33.3334, -1.14068) -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(0.15141, 0.645553, -1.19108) --  Rotation(-0.581886, 0.123524, -1.57262) -- camera:rotation():inverse() * pivot:rotation()
	
	self.stances.ak47.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak47.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak47.standard.vel_overshot.pivot = Vector3( 0, 50, 0 )
	
	-- Crouched stance
	self.stances.ak47.crouched.head.translation = Vector3( 0, 0, 75 )
	self.stances.ak47.crouched.head.rotation = Rotation()
	local pivot_head_translation = Vector3(3,15,-0.2)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(-1, 1, 0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.ak47.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak47.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()

	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,1,0)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(0,0,0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.ak47.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak47.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak47.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	
	-- END AK47 --------------------------------------------------------------------
	
	-- GLOCK --------------------------------------------------------------------
	self.stances.glock = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.73016, 54.6357, -2.2865)  -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(-0.807598, 1.47537, -1.76124) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,29,-2)
	local pivot_head_rotation = Rotation( -2, -1, 0 )
	self.stances.glock.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock.standard.vel_overshot.pivot = pivot_shoulder_translation
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.glock.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock.steelsight.FOV = self.stances.beretta92.standard.FOV
	self.stances.glock.steelsight.vel_overshot.pivot = pivot_shoulder_translation
	self.stances.glock.steelsight.vel_overshot.yaw_neg = 12
	self.stances.glock.steelsight.vel_overshot.yaw_pos = -12
	self.stances.glock.steelsight.vel_overshot.pitch_neg = -12
	self.stances.glock.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(3,28,-2)
	local pivot_head_rotation = Rotation(-1, -1, 0)
	self.stances.glock.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock.crouched.vel_overshot.pivot = pivot_shoulder_translation
	-- END GLOCK --------------------------------------------------------------------
	]]
	
	-- New weapon stances
	self:_init_new_stances()
		
	self.movement_state = {}
	
	self.movement_state.standard = {}
	self.movement_state.standard.movement = {speed={},jump_velocity={xy={}}}
	
	self.movement_state.standard.movement.speed.STANDARD_MAX = 350		-- Walk speed. ( cm/sec )
	self.movement_state.standard.movement.speed.RUNNING_MAX = 575
	self.movement_state.standard.movement.speed.CROUCHING_MAX = 225
	self.movement_state.standard.movement.speed.STEELSIGHT_MAX = 185
	self.movement_state.standard.movement.speed.INAIR_MAX = 185
	self.movement_state.standard.movement.jump_velocity.z = 470 -- absolute initial vertical velocity of the player unit when jumping
	self.movement_state.standard.movement.jump_velocity.xy.run = self.movement_state.standard.movement.speed.RUNNING_MAX * 1.0 -- horisontal speed boost when sprint-jumping
	self.movement_state.standard.movement.jump_velocity.xy.walk = self.movement_state.standard.movement.speed.STANDARD_MAX * 1.2 -- horisontal velocity when walk-jumping
	
	self.movement_state.interaction_delay = 1.5
	
	-- STAMINA
	self.movement_state.stamina = {}
	self.movement_state.stamina.STAMINA_INIT = 20							-- Max stamina
	self.movement_state.stamina.STAMINA_REGEN_RATE = 3				-- Rate of stamina regeneration, per second
	self.movement_state.stamina.STAMINA_DRAIN_RATE = 2				-- Amount of stamina that running drains per second
	self.movement_state.stamina.REGENERATE_TIME = 1						-- Time in sec, before starting to regenerate stamina
	self.movement_state.stamina.MIN_STAMINA_THRESHOLD = 4			-- Minimum amount of stamina required to start running / preforming a running jump
	
	self.movement_state.stamina.JUMP_STAMINA_DRAIN = 2				-- Amount of stamina a running jump drains
	
	
	-- CAMERA:
	self.camera = {}
	self.camera.MIN_SENSITIVITY = 0.3			-- Minimum sensitivity, multiplier that needs to be more then 0, of the camera movement when looking.
	self.camera.MAX_SENSITIVITY = 1.7			-- Maximum sensitivity, multiplier that needs to be more then 0, of the camera movement when looking.
	
end

function PlayerTweakData:_init_new_stances()
	-- new_m4 --------------------------------------------------------------------
	self.stances.new_m4 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7288, 23.0125, -1.8891) -- Vector3(8.73016, 54.6357, -2.2865)  -- ( pivot:position() - root:position() ):rotate_with( root:rotation():inverse() )
	local pivot_shoulder_rotation = Rotation(0.106607, -0.0844486, 0.629199) -- Rotation(-0.807598, 1.47537, -1.76124) -- camera:rotation():inverse() * pivot:rotation()
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,20,-0.5)
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 )
	self.stances.new_m4.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_m4.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_m4.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	self.stances.new_m4.steelsight.vel_overshot.pitch_neg = -17
	self.stances.new_m4.steelsight.vel_overshot.pitch_pos = 17
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,10,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.new_m4.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_m4.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_m4.steelsight.FOV = self.stances.new_m4.standard.FOV
	self.stances.new_m4.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-7,0)
	self.stances.new_m4.steelsight.vel_overshot.yaw_neg = 8
	self.stances.new_m4.steelsight.vel_overshot.yaw_pos = -8
	self.stances.new_m4.steelsight.vel_overshot.pitch_neg = -17
	self.stances.new_m4.steelsight.vel_overshot.pitch_pos = 17
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,16,-0.5)
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)
	self.stances.new_m4.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_m4.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_m4.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	-- END new_m4 --------------------------------------------------------------------
	
	-- GLOCK 18c --------------------------------------------------------------------
	self.stances.glock_18c = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.49051, 38.6474, -5.09399) -- Vector3(8.73016, 54.6357, -2.2865)
	local pivot_shoulder_rotation = Rotation(0.0999949, -0.687702, 0.630304) -- Rotation(-0.807598, 1.47537, -1.76124)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,36,-2)
	-- local pivot_head_rotation = Rotation( -2, -1, 0 )
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.glock_18c.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock_18c.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock_18c.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -35, 0 )
	self.stances.glock_18c.standard.vel_overshot.yaw_neg = 10
	self.stances.glock_18c.standard.vel_overshot.yaw_pos = -10
	self.stances.glock_18c.standard.vel_overshot.pitch_neg = -13
	self.stances.glock_18c.standard.vel_overshot.pitch_pos = 13
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,37,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.glock_18c.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock_18c.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock_18c.steelsight.FOV = self.stances.glock_18c.standard.FOV 
	self.stances.glock_18c.steelsight.zoom_fov = false
	self.stances.glock_18c.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -40, 0 )
	self.stances.glock_18c.steelsight.vel_overshot.yaw_neg = 8
	self.stances.glock_18c.steelsight.vel_overshot.yaw_pos = -8
	self.stances.glock_18c.steelsight.vel_overshot.pitch_neg = -8
	self.stances.glock_18c.steelsight.vel_overshot.pitch_pos = 8
	
	-- Crouched stance
	local pivot_head_translation = Vector3(3,32,-2)
	-- local pivot_head_rotation = Rotation(-1, -1, 0)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.glock_18c.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock_18c.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock_18c.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -30, 0 )
	-- END GLOCK 18c --------------------------------------------------------------------
	
	-- amcar --------------------------------------------------------------------
	self.stances.amcar = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7338, 23.0132, -1.4367) -- Vector3(8.47521, 33.3457, -0.8154) -- Vector3()
	local pivot_shoulder_rotation = Rotation(0.10663, -0.0844277, 0.629211) -- Rotation(0.151446, 0.645592, -1.19106)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,20,-0.5)
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 )
	self.stances.amcar.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.amcar.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.amcar.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	self.stances.amcar.standard.vel_overshot.yaw_neg = -6
	self.stances.amcar.standard.vel_overshot.yaw_pos = 6
	self.stances.amcar.standard.vel_overshot.pitch_neg = 5
	self.stances.amcar.standard.vel_overshot.pitch_pos = -5
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,14,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.amcar.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.amcar.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.amcar.steelsight.FOV = self.stances.amcar.standard.FOV
	self.stances.amcar.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-7,0)
	self.stances.amcar.steelsight.vel_overshot.yaw_neg = -2
	self.stances.amcar.steelsight.vel_overshot.yaw_pos = 4
	self.stances.amcar.steelsight.vel_overshot.pitch_neg = 5
	self.stances.amcar.steelsight.vel_overshot.pitch_pos = -5
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,20,-0.5)
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)
	self.stances.amcar.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.amcar.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.amcar.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	self.stances.amcar.crouched.vel_overshot.yaw_neg = -6
	self.stances.amcar.crouched.vel_overshot.yaw_pos = 6
	self.stances.amcar.crouched.vel_overshot.pitch_neg = 5
	self.stances.amcar.crouched.vel_overshot.pitch_pos = -5
	-- END amcar --------------------------------------------------------------------
	
	-- M16 --------------------------------------------------------------------
	self.stances.m16 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7353, 23.0139, -1.43553) -- Vector3(8.47532, 33.3458, -0.81541) -- Vector3()
	local pivot_shoulder_rotation = Rotation(0.106665, -0.0845104, 0.629231) -- Rotation(0.15141, -0.0359576, -1.19106)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,20,-0.5)
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 )
	self.stances.m16.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m16.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m16.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-15,0)
	self.stances.m16.standard.vel_overshot.yaw_neg = -6
	self.stances.m16.standard.vel_overshot.yaw_pos = 6
	self.stances.m16.standard.vel_overshot.pitch_neg = 10
	self.stances.m16.standard.vel_overshot.pitch_pos = -10
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,14,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.m16.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m16.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m16.steelsight.FOV = self.stances.m16.standard.FOV
	self.stances.m16.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-7,0)
	self.stances.m16.steelsight.vel_overshot.yaw_neg = -4
	self.stances.m16.steelsight.vel_overshot.yaw_pos = 4
	self.stances.m16.steelsight.vel_overshot.pitch_neg = 8
	self.stances.m16.steelsight.vel_overshot.pitch_pos = -8
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,16,-0.5)
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)
	self.stances.m16.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.m16.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.m16.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-15,0)
	self.stances.m16.crouched.vel_overshot.yaw_neg = -4
	self.stances.m16.crouched.vel_overshot.yaw_pos = 4
	self.stances.m16.crouched.vel_overshot.pitch_neg = 8
	self.stances.m16.crouched.vel_overshot.pitch_pos = -8
	-- END M16 --------------------------------------------------------------------
	
	-- Olympic --------------------------------------------------------------------
	self.stances.olympic = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7335, 23.013, -1.43651)
	local pivot_shoulder_rotation = Rotation(0.106626, -0.0833779, 0.62919)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,20,-0.5)
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 )
	self.stances.olympic.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.olympic.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.olympic.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-15,0)
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,14,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.olympic.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.olympic.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.olympic.steelsight.FOV = self.stances.olympic.standard.FOV
	self.stances.olympic.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-7,0)
	self.stances.olympic.steelsight.vel_overshot.yaw_neg = 8
	self.stances.olympic.steelsight.vel_overshot.yaw_pos = -8
	self.stances.olympic.steelsight.vel_overshot.pitch_neg = -17
	self.stances.olympic.steelsight.vel_overshot.pitch_pos = 17
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,17,-0.5)
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)
	self.stances.olympic.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.olympic.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.olympic.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-7,0)
	-- END olympic --------------------------------------------------------------------
	
	-- AK74 --------------------------------------------------------------------
	self.stances.ak74 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.6635, 40.3795, -4.93265)
	local pivot_shoulder_rotation = Rotation(0.10668, -0.0844958, 0.629228)
	
	-- Standard stance
	local pivot_head_translation = Vector3(9,36,-2) -- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 ) -- camera:rotation():inverse() * pivot:rotation()
		
	self.stances.ak74.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse()  ):rotate_with( pivot_head_rotation )
	self.stances.ak74.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak74.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.ak74.standard.vel_overshot.yaw_neg = -5
	self.stances.ak74.standard.vel_overshot.yaw_pos = 5
	self.stances.ak74.standard.vel_overshot.pitch_neg = 7
	self.stances.ak74.standard.vel_overshot.pitch_pos = -7

	
	-- Crouched stance
	-- self.stances.ak74.crouched.head.translation = Vector3( 0, 0, 75 )
	-- self.stances.ak74.crouched.head.rotation = Rotation()
	local pivot_head_translation = Vector3(6,36,-1)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.ak74.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak74.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak74.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.ak74.crouched.vel_overshot.yaw_neg = -4
	self.stances.ak74.crouched.vel_overshot.yaw_pos = 4
	self.stances.ak74.crouched.vel_overshot.pitch_neg = 5
	self.stances.ak74.crouched.vel_overshot.pitch_pos = -5
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.ak74.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak74.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak74.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.ak74.steelsight.vel_overshot.yaw_neg = -4
	self.stances.ak74.steelsight.vel_overshot.yaw_pos = 4
	self.stances.ak74.steelsight.vel_overshot.pitch_neg = 5
	self.stances.ak74.steelsight.vel_overshot.pitch_pos = -5
	
	-- END AK74 --------------------------------------------------------------------
	
	-- AKM --------------------------------------------------------------------
	self.stances.akm = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.6677, 40.3885, -4.94216)
	local pivot_shoulder_rotation = Rotation(0.10664, -0.0844285, 0.629212)
	
	-- Standard stance
	local pivot_head_translation = Vector3(9,36,-2) -- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 ) -- camera:rotation():inverse() * pivot:rotation()
		
	self.stances.akm.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.akm.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.akm.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.akm.standard.vel_overshot.yaw_neg = -5
	self.stances.akm.standard.vel_overshot.yaw_pos = 5
	self.stances.akm.standard.vel_overshot.pitch_neg = 7
	self.stances.akm.standard.vel_overshot.pitch_pos = -7
	
	-- Crouched stance
	-- self.stances.akm.crouched.head.translation = Vector3( 0, 0, 75 )
	-- self.stances.akm.crouched.head.rotation = Rotation()
	local pivot_head_translation = Vector3(6,36,-1)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.akm.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.akm.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.akm.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.akm.crouched.vel_overshot.yaw_neg = -4
	self.stances.akm.crouched.vel_overshot.yaw_pos = 4
	self.stances.akm.crouched.vel_overshot.pitch_neg = 5
	self.stances.akm.crouched.vel_overshot.pitch_pos = -5

	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.akm.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.akm.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.akm.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.akm.steelsight.vel_overshot.yaw_neg = -4
	self.stances.akm.steelsight.vel_overshot.yaw_pos = 4
	self.stances.akm.steelsight.vel_overshot.pitch_neg = 5
	self.stances.akm.steelsight.vel_overshot.pitch_pos = -5
	
	-- END AKM --------------------------------------------------------------------
	
	-- AKMSU --------------------------------------------------------------------
	self.stances.akmsu = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.6646, 40.3785, -4.67554)
	local pivot_shoulder_rotation = Rotation(0.10634, -0.0854686, 0.628928)
	
	-- Standard stance
	local pivot_head_translation = Vector3(8,36,-2) -- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 ) -- camera:rotation():inverse() * pivot:rotation()
		
	self.stances.akmsu.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.akmsu.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.akmsu.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.akmsu.standard.vel_overshot.yaw_neg = 10
	self.stances.akmsu.standard.vel_overshot.yaw_pos = -10
	self.stances.akmsu.standard.vel_overshot.pitch_neg = -20
	self.stances.akmsu.standard.vel_overshot.pitch_pos = 20
	
	-- Crouched stance
	-- self.stances.akmsu.crouched.head.translation = Vector3( 0, 0, 75 )
	-- self.stances.akmsu.crouched.head.rotation = Rotation()
	local pivot_head_translation = Vector3(6,36,-1)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.akmsu.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.akmsu.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.akmsu.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)

	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.akmsu.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.akmsu.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.akmsu.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.akmsu.steelsight.vel_overshot.yaw_neg = 10
	self.stances.akmsu.steelsight.vel_overshot.yaw_pos = -10
	self.stances.akmsu.steelsight.vel_overshot.pitch_neg = -20
	self.stances.akmsu.steelsight.vel_overshot.pitch_pos = 20
	
	-- END AKMSU --------------------------------------------------------------------
	
	-- SAIGA --------------------------------------------------------------------
	self.stances.saiga = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.663, 40.3754, -4.93619)
	local pivot_shoulder_rotation = Rotation(0.106614, -0.0844471, 0.629197)
	
	-- Standard stance
	local pivot_head_translation = Vector3(9,36,-2) -- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 ) -- camera:rotation():inverse() * pivot:rotation()
		
	self.stances.saiga.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.saiga.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.saiga.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	
	-- Crouched stance
	-- self.stances.saiga.crouched.head.translation = Vector3( 0, 0, 75 )
	-- self.stances.saiga.crouched.head.rotation = Rotation()
	local pivot_head_translation = Vector3(6,36,-1)-- ( pivot:position() - camera:position() ):rotate_with( camera:rotation():inverse() )
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)	-- camera:rotation():inverse() * pivot:rotation()
	self.stances.saiga.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.saiga.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.saiga.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)

	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,30,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.saiga.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.saiga.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.saiga.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.saiga.steelsight.vel_overshot.yaw_neg = 10
	self.stances.saiga.steelsight.vel_overshot.yaw_pos = -10
	self.stances.saiga.steelsight.vel_overshot.pitch_neg = -20
	self.stances.saiga.steelsight.vel_overshot.pitch_pos = 20
	
	-- END SAIGA --------------------------------------------------------------------
	
	-- AK5 --------------------------------------------------------------------
	self.stances.ak5 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7332, 15.6145, -2.75549)
	local pivot_shoulder_rotation = Rotation(0.106625, -0.430997, 0.629212)
	
	-- Standard stance
	local pivot_head_translation = Vector3(8,16,-2)
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 )
	self.stances.ak5.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak5.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak5.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -20, 0 )
	self.stances.ak5.standard.vel_overshot.yaw_neg = 10
	self.stances.ak5.standard.vel_overshot.yaw_pos = -10
	self.stances.ak5.standard.vel_overshot.pitch_neg = -10
	self.stances.ak5.standard.vel_overshot.pitch_pos = 10
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,18,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.ak5.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak5.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	-- self.stances.ak5.steelsight.FOV = self.stances.ak5.standard.FOV
	self.stances.ak5.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -10, 0 )
	self.stances.ak5.steelsight.vel_overshot.yaw_neg = 4
	self.stances.ak5.steelsight.vel_overshot.yaw_pos = -4
	self.stances.ak5.steelsight.vel_overshot.pitch_neg = -17
	self.stances.ak5.steelsight.vel_overshot.pitch_pos = 17
		
	-- Crouched stance
	local pivot_head_translation = Vector3(7,15,-0.5)
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)
	self.stances.ak5.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.ak5.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.ak5.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -30, 0 )
	-- END AK5 --------------------------------------------------------------------
	
	-- AUG
	self.stances.aug = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7046, 17.1499, -5.21193)
	local pivot_shoulder_rotation = Rotation(0.106362, -0.0855956, 0.628962)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,20,-1)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.aug.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.aug.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.aug.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -20, 0 )
	self.stances.aug.standard.vel_overshot.yaw_neg = 8
	self.stances.aug.standard.vel_overshot.yaw_pos = -8
	self.stances.aug.standard.vel_overshot.pitch_neg = -10
	self.stances.aug.standard.vel_overshot.pitch_pos = 10
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,17,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.aug.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.aug.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.aug.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -20, 0 )
	self.stances.aug.steelsight.vel_overshot.yaw_neg = 4
	self.stances.aug.steelsight.vel_overshot.yaw_pos = -4
	self.stances.aug.steelsight.vel_overshot.pitch_neg = -17
	self.stances.aug.steelsight.vel_overshot.pitch_pos = 17
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,18,0)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.aug.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.aug.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.aug.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -20, 0 )
	
	-- END AUG
	
	-- G36 --------------------------------------------------------------------
	self.stances.g36 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.5658, 24.8768, -1.07923)
	local pivot_shoulder_rotation = Rotation(-0.000313746, 0.000813967, -0.00027678)
	
	-- Standard stance
	local pivot_head_translation = Vector3(9,18,0)
	local pivot_head_rotation = Rotation( 0.4, 0.4, 0 )
	self.stances.g36.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.g36.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.g36.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -25, 0 )
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,17,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.g36.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.g36.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.g36.steelsight.FOV = self.stances.g36.standard.FOV
	self.stances.g36.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -10, 0 )
	self.stances.g36.steelsight.vel_overshot.yaw_neg = 5
	self.stances.g36.steelsight.vel_overshot.yaw_pos = -5
	self.stances.g36.steelsight.vel_overshot.pitch_neg = -17
	self.stances.g36.steelsight.vel_overshot.pitch_pos = 17
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,17,1)
	local pivot_head_rotation = Rotation(0.2, 0.2, 0)
	self.stances.g36.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.g36.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.g36.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -25, 0 )
	-- END G36 --------------------------------------------------------------------
	
	-- P90
	self.stances.p90 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(11.0002, 24.3293, -0.962641)
	local pivot_shoulder_rotation = Rotation(0.21279, 0.970214, 0.197877)
	
	-- Standard stance
	local pivot_head_translation = Vector3(7,23,0)
	local pivot_head_rotation = Rotation( 0, 0, -1 )
	self.stances.p90.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.p90.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.p90.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -25, 0 )
	self.stances.p90.standard.vel_overshot.yaw_neg = 10
	self.stances.p90.standard.vel_overshot.yaw_pos = -10
	self.stances.p90.standard.vel_overshot.pitch_neg = -15
	self.stances.p90.standard.vel_overshot.pitch_pos = 15
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,20,0)
	local pivot_head_rotation = Rotation(0,0,-0.5)
	self.stances.p90.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.p90.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.p90.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -15, 0 )
	self.stances.p90.steelsight.vel_overshot.yaw_neg = 5
	self.stances.p90.steelsight.vel_overshot.yaw_pos = -5
	self.stances.p90.steelsight.vel_overshot.pitch_neg = -17
	self.stances.p90.steelsight.vel_overshot.pitch_pos = 17
	
	-- Crouched stance
	local pivot_head_translation = Vector3(6,19,3)
	local pivot_head_rotation = Rotation( 0, 0, 0)
	self.stances.p90.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.p90.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.p90.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -25, 0 )
	-- END P90
	
	-- New M14 --------------------------------------------------------------------
	self.stances.new_m14 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7037, 23.0304, -4.44631)
	local pivot_shoulder_rotation = Rotation(0.10663, -0.0844432, 0.629197)
	
	-- Standard stance
	local pivot_head_translation = Vector3(6,18,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.new_m14.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_m14.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_m14.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-12,0)
	self.stances.new_m14.standard.vel_overshot.yaw_neg = -5
	self.stances.new_m14.standard.vel_overshot.yaw_pos = 5
	self.stances.new_m14.standard.vel_overshot.pitch_neg = 5
	self.stances.new_m14.standard.vel_overshot.pitch_pos = -5
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,10,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.new_m14.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_m14.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_m14.steelsight.vel_overshot.pivot = pivot_shoulder_translation  + Vector3(0,-5,0)
	self.stances.new_m14.steelsight.vel_overshot.yaw_neg = -4
	self.stances.new_m14.steelsight.vel_overshot.yaw_pos = 4
	self.stances.new_m14.steelsight.vel_overshot.pitch_neg = 3
	self.stances.new_m14.steelsight.vel_overshot.pitch_pos = -3
	
	-- Crouched stance
	local pivot_head_translation = Vector3(5,18,0)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.new_m14.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_m14.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_m14.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-12,0)
	self.stances.new_m14.crouched.vel_overshot.yaw_neg = -6
	self.stances.new_m14.crouched.vel_overshot.yaw_pos = 6
	self.stances.new_m14.crouched.vel_overshot.pitch_neg = 4
	self.stances.new_m14.crouched.vel_overshot.pitch_pos = -4
	-- END NEW M14 --------------------------------------------------------------------
	
	-- MP9
	self.stances.mp9 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.64427, 34.7702, -4.76617)
	local pivot_shoulder_rotation = Rotation(0.109998, 0.0123081, 0.630295)
	
	-- Standard stance
	local pivot_head_translation = Vector3(8,23,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.mp9.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mp9.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mp9.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-15,0)
	self.stances.mp9.standard.vel_overshot.yaw_neg = 10
	self.stances.mp9.standard.vel_overshot.yaw_pos = -10
	self.stances.mp9.standard.vel_overshot.pitch_neg = -5
	self.stances.mp9.standard.vel_overshot.pitch_pos = 5
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,22,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.mp9.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mp9.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mp9.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-5,0)
	self.stances.mp9.steelsight.vel_overshot.yaw_neg = 4
	self.stances.mp9.steelsight.vel_overshot.yaw_pos = -4
	self.stances.mp9.steelsight.vel_overshot.pitch_neg = -8
	self.stances.mp9.steelsight.vel_overshot.pitch_pos = 8
	
	-- Crouched stance
	local pivot_head_translation = Vector3(7,20,0)
	local pivot_head_rotation = Rotation( 0, 0, 0)
	self.stances.mp9.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mp9.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mp9.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-15,0)
	-- END MP9
	
	-- DEAGLE --------------------------------------------------------------------
	self.stances.deagle = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.51744, 40.6489, -2.66934)
	local pivot_shoulder_rotation = Rotation(0.100008, -0.687698, 0.630289)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,35,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.deagle.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.deagle.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.deagle.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -20, 0 )
	self.stances.deagle.standard.vel_overshot.yaw_neg = -5
	self.stances.deagle.standard.vel_overshot.yaw_pos = 5
	self.stances.deagle.standard.vel_overshot.pitch_neg = 4
	self.stances.deagle.standard.vel_overshot.pitch_pos = -4
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,35,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.deagle.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.deagle.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.deagle.steelsight.zoom_fov = false
	self.stances.deagle.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -30, 0 )
	self.stances.deagle.steelsight.vel_overshot.yaw_neg = -4
	self.stances.deagle.steelsight.vel_overshot.yaw_pos = 2
	self.stances.deagle.steelsight.vel_overshot.pitch_neg = 2
	self.stances.deagle.steelsight.vel_overshot.pitch_pos = -2
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,36,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0)
	self.stances.deagle.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.deagle.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.deagle.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -20, 0 )
	-- END DEAGLE --------------------------------------------------------------------
	
	-- NEW MP5
	self.stances.new_mp5 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7414, 18.4543, -3.29175)
	local pivot_shoulder_rotation = Rotation(0.106934, -0.220015, 0.629729)
	
	-- Standard stance
	local pivot_head_translation = Vector3(8,14,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.new_mp5.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_mp5.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_mp5.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-10,0)
	self.stances.new_mp5.standard.vel_overshot.yaw_neg = 10
	self.stances.new_mp5.standard.vel_overshot.yaw_pos = -10
	self.stances.new_mp5.standard.vel_overshot.pitch_neg = -5
	self.stances.new_mp5.standard.vel_overshot.pitch_pos = 5
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,10,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.new_mp5.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_mp5.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_mp5.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-5,0)
	self.stances.new_mp5.steelsight.vel_overshot.yaw_neg = 4
	self.stances.new_mp5.steelsight.vel_overshot.yaw_pos = -4
	self.stances.new_mp5.steelsight.vel_overshot.pitch_neg = -8
	self.stances.new_mp5.steelsight.vel_overshot.pitch_pos = 8
	
	-- Crouched stance
	local pivot_head_translation = Vector3(7,15,-0.5)
	local pivot_head_rotation = Rotation( 0, 0, 0)
	self.stances.new_mp5.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_mp5.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_mp5.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-10,0)
	-- END NEW MP5 --------------------------------------------------------------------
		
	-- COLT 1911 --------------------------------------------------------------------
	self.stances.colt_1911 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.51072, 41.1823, -3.19592)
	local pivot_shoulder_rotation = Rotation(0.0999825, -0.688529, 0.630296)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,38,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.colt_1911.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.colt_1911.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.colt_1911.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -30, 0 )
	self.stances.colt_1911.standard.vel_overshot.yaw_neg = 10
	self.stances.colt_1911.standard.vel_overshot.yaw_pos = -10
	self.stances.colt_1911.standard.vel_overshot.pitch_neg = -8
	self.stances.colt_1911.standard.vel_overshot.pitch_pos = 8
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,42,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.colt_1911.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.colt_1911.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.colt_1911.steelsight.zoom_fov = false
	self.stances.colt_1911.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -30, 0 )
	self.stances.colt_1911.steelsight.vel_overshot.yaw_neg = 8
	self.stances.colt_1911.steelsight.vel_overshot.yaw_pos = -8
	self.stances.colt_1911.steelsight.vel_overshot.pitch_neg = -12
	self.stances.colt_1911.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,35,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0)
	self.stances.colt_1911.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.colt_1911.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.colt_1911.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -20, 0 )
	-- END COLT 1911 --------------------------------------------------------------------
	
	-- MAC10
	self.stances.mac10 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.66929, 32.8934, -2.84433)
	local pivot_shoulder_rotation = Rotation(0.110003, -0.898575, 0.630293)
	

	-- Standard stance
	local pivot_head_translation = Vector3(8,25,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.mac10.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mac10.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mac10.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	self.stances.mac10.standard.vel_overshot.yaw_neg = 15
	self.stances.mac10.standard.vel_overshot.yaw_pos = -15
	self.stances.mac10.standard.vel_overshot.pitch_neg = -15
	self.stances.mac10.standard.vel_overshot.pitch_pos = 15
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,23,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.mac10.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mac10.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mac10.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-15,0)
	self.stances.mac10.steelsight.vel_overshot.yaw_neg = 15
	self.stances.mac10.steelsight.vel_overshot.yaw_pos = -15
	self.stances.mac10.steelsight.vel_overshot.pitch_neg = -18
	self.stances.mac10.steelsight.vel_overshot.pitch_pos = 18
	
	-- Crouched stance
	local pivot_head_translation = Vector3(7,20,0)
	local pivot_head_rotation = Rotation( 0, 0, 0)
	self.stances.mac10.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.mac10.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.mac10.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	-- END MAC10
	
	-- R870
	self.stances.r870 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7362, 12.88858, -4.29568)
	local pivot_shoulder_rotation = Rotation(0.106618, -0.0844415, 0.629205)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,15,-1)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.r870.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.r870.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.r870.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.r870.standard.vel_overshot.yaw_neg = -5
	self.stances.r870.standard.vel_overshot.yaw_pos = 3
	self.stances.r870.standard.vel_overshot.pitch_neg = 2
	self.stances.r870.standard.vel_overshot.pitch_pos = -3
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,15,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.r870.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.r870.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.r870.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	self.stances.r870.steelsight.vel_overshot.yaw_neg = -3
	self.stances.r870.steelsight.vel_overshot.yaw_pos = 3
	self.stances.r870.steelsight.vel_overshot.pitch_neg = 2
	self.stances.r870.steelsight.vel_overshot.pitch_pos = -2
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,17,-2)
	local pivot_head_rotation = Rotation( -2, -2, 0 )
	self.stances.r870.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.r870.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.r870.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	
	-- END R870
	
	-- GLOCK 17 --------------------------------------------------------------------
	self.stances.glock_17 = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.48582, 38.7727, -5.49358)
	local pivot_shoulder_rotation = Rotation(0.100007, -0.687692, 0.630291)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,34,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.glock_17.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock_17.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock_17.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -35, 0 )
	self.stances.glock_17.standard.vel_overshot.yaw_neg = 10
	self.stances.glock_17.standard.vel_overshot.yaw_pos = -10
	self.stances.glock_17.standard.vel_overshot.pitch_neg = -13
	self.stances.glock_17.standard.vel_overshot.pitch_pos = 13
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,35,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.glock_17.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock_17.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock_17.steelsight.FOV = self.stances.glock_17.standard.FOV
	self.stances.glock_17.steelsight.zoom_fov = false
	self.stances.glock_17.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -35, 0 )
	self.stances.glock_17.steelsight.vel_overshot.yaw_neg = 8
	self.stances.glock_17.steelsight.vel_overshot.yaw_pos = -8
	self.stances.glock_17.steelsight.vel_overshot.pitch_neg = -8
	self.stances.glock_17.steelsight.vel_overshot.pitch_pos = 8
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,32,-2)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.glock_17.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.glock_17.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.glock_17.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0, -35, 0 )
	-- END GLOCK 17 --------------------------------------------------------------------
	
	-- HUNTSMAN
	self.stances.huntsman = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.6562, 32.9715, -6.73279)
	local pivot_shoulder_rotation = Rotation(0.106667, -0.0844876, 0.629223)
	
	-- Standard stance
	local pivot_head_translation = Vector3(6,25,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.huntsman.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.huntsman.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.huntsman.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,20,-2)
	local pivot_head_rotation = Rotation(0,2,0)
	self.stances.huntsman.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.huntsman.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.huntsman.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-20,0)
	self.stances.huntsman.steelsight.vel_overshot.yaw_neg = 12
	self.stances.huntsman.steelsight.vel_overshot.yaw_pos = -12
	self.stances.huntsman.steelsight.vel_overshot.pitch_neg = -12
	self.stances.huntsman.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,17,-2)
	local pivot_head_rotation = Rotation( -2, -2, 0 )
	self.stances.huntsman.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.huntsman.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.huntsman.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3(0,-30,0)
	-- END HUNTSMAN
	
	-- BERETTA 92fs
	self.stances.b92fs = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(8.50075, 40.9227, -4.15328)
	local pivot_shoulder_rotation = Rotation(0.0994, -0.687851, 0.630047)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,34,-2)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.b92fs.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.b92fs.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.b92fs.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -36, 0 )
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,37,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.b92fs.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.b92fs.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.b92fs.steelsight.zoom_fov = false
	self.stances.b92fs.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -36, 0 )
	self.stances.b92fs.steelsight.vel_overshot.yaw_neg = 5
	self.stances.b92fs.steelsight.vel_overshot.yaw_pos = -5
	self.stances.b92fs.steelsight.vel_overshot.pitch_neg = -12
	self.stances.b92fs.steelsight.vel_overshot.pitch_pos = 12
	
	-- Crouched stance
	local pivot_head_translation = Vector3(3,32,-2)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.b92fs.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.b92fs.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.b92fs.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -36, 0 )
	-- END -- BERETTA 92fs
		
	-- New Raging Bull --------------------------------------------------------------------
	self.stances.new_raging_bull = deep_clone( self.stances.default )
		
	local pivot_shoulder_translation = Vector3(8.48834, 43.8612, -2.19366)
	local pivot_shoulder_rotation = Rotation(0.0999871, -0.68716, 0.629919)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,30,-1)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.new_raging_bull.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_raging_bull.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_raging_bull.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -10, 0 )
	self.stances.new_raging_bull.standard.vel_overshot.yaw_neg = -5
	self.stances.new_raging_bull.standard.vel_overshot.yaw_pos = 2
	self.stances.new_raging_bull.standard.vel_overshot.pitch_neg = 5
	self.stances.new_raging_bull.standard.vel_overshot.pitch_pos = -5
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,36,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.new_raging_bull.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_raging_bull.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_raging_bull.steelsight.zoom_fov = false
	self.stances.new_raging_bull.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -35, 0 )
	self.stances.new_raging_bull.steelsight.vel_overshot.yaw_neg = -5
	self.stances.new_raging_bull.steelsight.vel_overshot.yaw_pos = 2
	self.stances.new_raging_bull.steelsight.vel_overshot.pitch_neg = 5
	self.stances.new_raging_bull.steelsight.vel_overshot.pitch_pos = -5
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,33,-1)
	local pivot_head_rotation = Rotation(0, 0, 0)
	self.stances.new_raging_bull.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.new_raging_bull.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.new_raging_bull.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -30, 0 )
	-- END New  Raging Bull --------------------------------------------------------------------
		
	self:_init_saw()
	self:_init_serbu()
end

function PlayerTweakData:_init_saw()
	-- Saw
	self.stances.saw = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.7099, 18.4324, -4.38493)
	local pivot_shoulder_rotation = Rotation(0.106622, -0.218452, 0.629191)
	
	-- Standard stance
	local pivot_head_translation = Vector3(10,5,-5)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.saw.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.saw.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.saw.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -35, 0 )
	self.stances.saw.standard.vel_overshot.yaw_neg = -15
	self.stances.saw.standard.vel_overshot.yaw_pos = 15
	self.stances.saw.standard.vel_overshot.pitch_neg = 15
	self.stances.saw.standard.vel_overshot.pitch_pos = -15
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(10,10,-5)
	local pivot_head_rotation = Rotation(0,0,5)
	self.stances.saw.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.saw.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.saw.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -35, 0 )
	self.stances.saw.steelsight.zoom_fov = false
	self.stances.saw.steelsight.vel_overshot.yaw_neg = -15
	self.stances.saw.steelsight.vel_overshot.yaw_pos = 15
	self.stances.saw.steelsight.vel_overshot.pitch_neg = 15
	self.stances.saw.steelsight.vel_overshot.pitch_pos = -15
	
	-- Crouched stance
	local pivot_head_translation = Vector3(11,5,-2)
	local pivot_head_rotation = Rotation( 0, 0, 2)
	self.stances.saw.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.saw.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.saw.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -35, 0 )
end

function PlayerTweakData:_init_serbu()

	-- serbu
	self.stances.serbu = deep_clone( self.stances.default )
	
	local pivot_shoulder_translation = Vector3(10.71, 18.7035, -4.30218)
	local pivot_shoulder_rotation = Rotation(0.106838, -0.0844692, 0.629273)
	
	-- Standard stance
	local pivot_head_translation = Vector3(5,20,-1)
	local pivot_head_rotation = Rotation( 0, 0, 0 )
	self.stances.serbu.standard.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.serbu.standard.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.serbu.standard.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -20, 0 )
	self.stances.serbu.standard.vel_overshot.yaw_neg = 6
	self.stances.serbu.standard.vel_overshot.yaw_pos = -6
	self.stances.serbu.standard.vel_overshot.pitch_neg = -4
	self.stances.serbu.standard.vel_overshot.pitch_pos = 4
	
	-- Steelsight stance
	local pivot_head_translation = Vector3(0,20,0)
	local pivot_head_rotation = Rotation(0,0,0)
	self.stances.serbu.steelsight.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.serbu.steelsight.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.serbu.steelsight.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -10, 0 )
	self.stances.serbu.steelsight.vel_overshot.yaw_neg = 5
	self.stances.serbu.steelsight.vel_overshot.yaw_pos = -4
	self.stances.serbu.steelsight.vel_overshot.pitch_neg = -5
	self.stances.serbu.steelsight.vel_overshot.pitch_pos = 5
	
	-- Crouched stance
	local pivot_head_translation = Vector3(4,17,-2)
	local pivot_head_rotation = Rotation( -2, -2, 0 )
	self.stances.serbu.crouched.shoulders.translation = pivot_head_translation - pivot_shoulder_translation:rotate_with( pivot_shoulder_rotation:inverse() ):rotate_with( pivot_head_rotation )
	self.stances.serbu.crouched.shoulders.rotation = pivot_head_rotation * pivot_shoulder_rotation:inverse()
	self.stances.serbu.crouched.vel_overshot.pivot = pivot_shoulder_translation + Vector3( 0, -20, 0 )
	
	-- END serbu
	
end