-- Info:
-- Heat decays at 1 point per second


DramaTweakData = DramaTweakData or class()

function DramaTweakData:init()
	self:_create_table_structure() -- Leave this alone
	
	self.drama_actions = { criminal_dead = 0.2				-- When killed
												,criminal_disabled = 0.1		-- When arrested/bleedout
												,criminal_hurt = 0.5 				-- Multiplied by percentage of health lost
												}
	
	self.decay_period = 30			-- How long it takes for drama to decay from full to zero. (sec)
	
	self.max_dis = 6000			-- Distance at which we receive the minimum drama multiplier
	self.max_dis_mul = 0.5	-- Multiplier to drama at max_dis from enemy
	
	self.low = 0.1			-- Percentage considered to be a drama low
	self.peak = 0.95			-- // 													 drama peak
	self.assault_fade_end = 0.25							-- drama value at which the assault fade phase is allowed to end and begin regroup
end

-----------------------------------------------------------------------------------

function DramaTweakData:_create_table_structure()

end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------
