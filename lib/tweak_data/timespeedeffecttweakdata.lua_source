TimeSpeedEffectTweakData = TimeSpeedEffectTweakData or class()

function TimeSpeedEffectTweakData:init()
	self:_init_base_effects()
	self:_init_mission_effects()
end

--------------------------------------------------------------------------------------

function TimeSpeedEffectTweakData:_init_base_effects()
	self.mask_on = {
		speed = 0.2
		,fade_in_delay = 1.35
		,fade_in = 0.25
		,sustain = 5
		,fade_out = 0.8
		,timer = "pausable"
		--,sync = true
	}
	
	self.mask_on_player = { -- runs simultaneously with "mask_on" effect
		speed = 0.5
		,fade_in_delay = self.mask_on.fade_in_delay
		,fade_in = self.mask_on.fade_in
		,sustain = self.mask_on.sustain
		,fade_out = self.mask_on.fade_out
		,timer = self.mask_on.timer
		,affect_timer = Idstring( "player" )
		--,sync = true
	}
	
	self.downed = {
		speed = 0.3
		,fade_in = 0.25
		,sustain = 3
		,fade_out = 0.8
		,timer = "pausable"
		--,sync = true
	}
	
	self.downed_player = { -- runs simultaneously with "mask_on" effect
		speed = self.downed.speed
		,fade_in = self.downed.fade_in
		,sustain = self.downed.sustain
		,fade_out = self.downed.fade_out
		,timer = self.downed.timer
		,affect_timer = Idstring( "player" )
		--,sync = true
	}
	
end

--------------------------------------------------------------------------------------

function TimeSpeedEffectTweakData:_init_mission_effects()
	--[[
	self.mission_test_1 = {
		speed = 0.2
		,fade_in = 0.25
		,sustain = 5
		,fade_out = 0.8
		,timer = "pausable"
		,sync = true
	}
	]]
end
