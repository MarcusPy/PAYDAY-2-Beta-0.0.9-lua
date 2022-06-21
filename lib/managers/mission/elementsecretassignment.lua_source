core:import( "CoreMissionScriptElement" )

ElementSecretAssignment = ElementSecretAssignment or class( CoreMissionScriptElement.MissionScriptElement )

function ElementSecretAssignment:init( ... )
	ElementSecretAssignment.super.init( self, ... )
end

function ElementSecretAssignment:client_on_executed( ... )
	self:on_executed( ... )
end

function ElementSecretAssignment:on_executed( instigator )
	if not self._values.enabled then
		return
	end
	
	if self._values.assignment ~= "none" then
		managers.secret_assignment:set_assignment_enabled( self._values.assignment, self._values.set_enabled )
		--[[if self._values.state == "activate" then
			managers.objectives:activate_objective( self._values.objective )
		elseif self._values.state == "complete" then
			managers.objectives:complete_objective( self._values.objective )
		end]]
	elseif Application:editor() then
		managers.editor:output_error( 'Cant set enabled state on assignment '..self._values.assignment..' in element '..self._editor_name..'.' )
	end
	
	ElementSecretAssignment.super.on_executed( self, instigator )
end
