GameDirectionUnitElement = GameDirectionUnitElement or class( MissionElement )

function GameDirectionUnitElement:init( unit )	
	MissionElement.init( self, unit )
	
end

function GameDirectionUnitElement:update_selected( t, dt )	
	--[[if self._hed.distance ~= 0 then
		local brush = Draw:brush()
		brush:set_color( Color(0.15, 1.0,1.0, 1.0) )
		local pen = Draw:pen( Color(0.15, 0.5, 0.5, 0.5) )
		if not self._hed.in_front then
			brush:sphere( self._unit:position(), self._hed.distance, 4 )
			pen:sphere( self._unit:position(), self._hed.distance )
		else
			brush:half_sphere( self._unit:position(), self._hed.distance, -self._unit:rotation():y(), 4 )
			pen:half_sphere( self._unit:position(), self._hed.distance, -self._unit:rotation():y() )
		end
	end]]
end


function GameDirectionUnitElement:_build_panel( panel, panel_sizer )
	-- self:_create_panel()
		
	-- panel = panel or self._panel
	-- panel_sizer = panel_sizer or self._panel_sizer
end
