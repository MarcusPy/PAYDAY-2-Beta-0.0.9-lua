core:import( "CoreMissionScriptElement" )

ElementScenarioEvent = ElementScenarioEvent or class( CoreMissionScriptElement.MissionScriptElement )

function ElementScenarioEvent:init( ... )
	ElementScenarioEvent.super.init( self, ... )
	self._network_execute = true
end

function ElementScenarioEvent:client_on_executed( ... )
	-- self:on_executed( ... )
end

function ElementScenarioEvent:on_executed( instigator )
	if not self._values.enabled then
		return
	end
		
	ElementScenarioEvent.super.on_executed( self, instigator )
end

function ElementScenarioEvent:operation_add()
	-- Add scenario event here (use self._id as key, self._values.amount and self._values.task )
	local event_decriptor = {
		amount = self._values.amount
		,task_type = self._values.task
		,element = self
		,pos = self._values.position
		,rot = self._values.rotation
		,base_chance = self._values.base_chance or 1-- Might be nil from old data
		,chance_inc = self._values.chance_inc or 0-- Might be nil from old data
	}
	-- NOTE: The event_decriptor table gets writen to by the group ai state!! 
	managers.groupai:state():add_spawn_event( self._id, event_decriptor )
end

function ElementScenarioEvent:operation_remove()
	-- Remove scenario event here (use self._id as key)
	managers.groupai:state():remove_spawn_event( self._id )
end
