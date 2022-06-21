core:import( "CoreMissionScriptElement" )

ElementGameDirection = ElementGameDirection or class( CoreMissionScriptElement.MissionScriptElement )

function ElementGameDirection:init( ... )
	ElementGameDirection.super.init( self, ... )
end

function ElementGameDirection:on_executed( instigator )
	if not self._values.enabled then
		return
	end
	
	-- Add direction or rotation to ai manager here ( self._values.rotation or self._values.rotation:y() )
	managers.groupai:state():set_mission_fwd_vector( self._values.rotation:y(), self._values.position )
	ElementGameDirection.super.on_executed( self, instigator )
end
