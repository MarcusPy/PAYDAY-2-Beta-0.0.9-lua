SyncUnitData = SyncUnitData or class()

function SyncUnitData:init( unit )
	self._unit = unit
end

function SyncUnitData:save( data )
	local state = { lights = {} }
	for _,light in ipairs( self._unit:get_objects_by_type( Idstring( "light" ) ) ) do
	
		local l = { 
				name 					= light:name(),
				enabled 				= light:enable(),
				far_range				= light:far_range(),
				near_range				= light:near_range(),
				color					= light:color(),
				spot_angle_start		= light:spot_angle_start(),
				spot_angle_end 			= light:spot_angle_end(),
				multiplier_nr			= light:multiplier(),				-- Sync multiplier values as numers instead of looking up a string
				specular_multiplier_nr	= light:specular_multiplier(),		-- Sync multiplier values as numers instead of looking up a string
				falloff_exponent		= light:falloff_exponent()
		}
	
		table.insert( state.lights, l )
	end
	state.projection_light = self._unit:unit_data().projection_light -- unit_data().projection_light is set when loading the texture in CoreWorldDefinition
	state.projection_textures = self._unit:unit_data().projection_textures -- unit_data().projection_textures is set when loading the texture in CoreWorldDefinition
	data.SyncUnitData = state
end

function SyncUnitData:load( data )
	local state = data.SyncUnitData
	self._unit:unit_data().unit_id = self._unit:editor_id()
	managers.worlddefinition:setup_lights( self._unit, state )
	managers.worlddefinition:setup_projection_light( self._unit, state )
end
