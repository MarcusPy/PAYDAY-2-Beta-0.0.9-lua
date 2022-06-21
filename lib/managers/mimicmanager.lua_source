--[[	Mimic Manager
	The Mimic Manager will be taking request from
	mimic units to find NPC's willing to help do
	the mimic thats active on the mimic unit. 
	
	It will keep track of all the mimic units, 
	sorting them. This will for example do so
	that the player are able to, by asking
	the Mimic Manager, know what units he 
	will be able to call for NPC's to mimic him.
	Like aiming with a ranged weapon at a giant's
	head will activate the giant's ranged mimic
	(the giant being a mimic unit), allowing NPC's
	to aim at the head aswell, fireing when the
	player does so.
	
	The Mimic manager will have a crucial job
	to be able to dynamicly activating and/or
	deactivating mimic units. Allowing NPC's
	to handle a mimic unit without the player.
	Ofcourse, they will not be able to do so
	without the player having done something
	before, like when the player leap attacks
	a giant, activating that giants leap/climb
	mimic, the Mimic Manager may be allowed
	to activate some other giants further away,
	but not allowing the NPC's to complete (ie kill)
	before the player and those NPC's on his
	mimic unit giant have completed the mimic.
	
	some more stuff... will update in time, if something is missing...
	
	
	-----------------
	public methods:
		add_mimic_unit( mimic_unit, trigger_map )
			trigger_map:
				interaction
				*ranged
				*attack
		request_npcs( mimic_unit, number_of_npc_to_request )
		get_mimic_map( mimic_map ) ex. player ranged wanting mimic units with 'ranged' mimic types
]]--

MimicManager = MimicManager or class()

function MimicManager:init()
	self._mimic_map = {}
	
	self._mimic_map.interaction = {}
	self._mimic_map.ranged = {}
	self._mimic_map.attack = {}
	
	self._unit_map = {}
	self._mission_map = {}
	
	self._call_to_arms_list = {}
end

function MimicManager:add_mimic_unit( unit, type_list )
	local unit_key = unit:key()
	self._unit_map[ unit_key ] = unit
	
	for _, key in ipairs( type_list ) do
		assert( self._mimic_map[ key ], "No mimic type with name " .. tostring(key) .. " in MimicManager." )
		self._mimic_map[ key ][ unit_key ] = unit
	end
end

function MimicManager:activate_all()

	for k, v in pairs( self._mimic_map.interaction ) do
		v:mimic():activate_mimic( "interaction" )
	end
	for k, v in pairs( self._mimic_map.ranged ) do
		v:mimic():activate_mimic( "ranged" )
	end
	for k, v in pairs( self._mimic_map.attack ) do
		v:mimic():activate_mimic( "attack" )
	end
end

function MimicManager:request_npcs( unit, num )
	table.insert( self._call_to_arms_list, {unit, num } )
end

function MimicManager:get_mimic_map( mimic_map )
	return self._mimic_map[ mimic_map ]
end

function MimicManager:receive_mimic_types( unit_key )
	if( self._unit_map[ unit_key ] ) then
		return self._unit_map[ unit_key ]:mimic():receive_mimic_types()
	end
	
	return nil
end

function MimicManager:register_mission_callback( mission_condition, clbk, unit_key, mimic_type )
	
end

function MimicManager:update( t, dt )
	if( #self._call_to_arms_list > 0 ) then
		local all_called = true
		
		for k, params in ipairs( self._call_to_arms_list ) do
			if( params ) then
				local unit = params[1]
				local num = params[2]
				
				local units = unit:find_units_quick( "sphere", unit:position(), 12000, 12 )
				local return_list = {}
				
				for i=1, math.min( num, #units ) do
					if( alive( units[i] ) and units[i]:mimic_data() ) then
						table.insert( return_list, units[i] )
					end
				end
				
				unit:mimic():return_requested_npcs( return_list )
				
				self._call_to_arms_list[ k ] = false
				all_called = false
				break
			end
		end
		
		if( all_called ) then
			self._call_to_arms_list = {}
		end
	end
end

