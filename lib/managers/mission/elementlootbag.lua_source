core:import( "CoreMissionScriptElement" )

ElementLootBag = ElementLootBag or class( CoreMissionScriptElement.MissionScriptElement )

function ElementLootBag:init( ... )
	ElementLootBag.super.init( self, ... )
	
	self._triggers = {}
end

function ElementLootBag:client_on_executed( ... )
	-- self:on_executed( ... )
end

function ElementLootBag:on_executed( instigator )
	if not ( self._values.enabled ) then
		return
	end
	
	-- print( "ElementLootBag:on_executed" )
	
	local unit
	if self._values.carry_id ~= "none" then
		-- unit_name = tweak_data.carry[ self._values.carry_id ].unit or unit_name
		local dir = self._values.spawn_dir * self._values.push_multiplier
		-- print( "self._values.carry_id", self._values.carry_id, dir )
		-- local value = tweak_data.carry[ self._values.carry_id ].default_value or tweak_data.money_manager.bag_value
		local value = managers.money:get_bag_value( self._values.carry_id )
		unit = managers.player:server_drop_carry( self._values.carry_id, value, true, false, 1, self._values.position, self._values.rotation, dir, 0 )
	elseif self._values.from_respawn then
		local loot = managers.loot:get_respawn()
		if loot then
			-- loot.carry_id, loot.value
			local dir = self._values.spawn_dir * self._values.push_multiplier 
			unit = managers.player:server_drop_carry( loot.carry_id, loot.value, true, false, 1, self._values.position, self._values.rotation, dir, 0 )
		else
			print( "NO MORE LOOT TO RESPAWN" )
		end
	else
		local loot = managers.loot:get_distribute()
		if loot then
			-- loot.carry_id, loot.value
			local dir = self._values.spawn_dir * self._values.push_multiplier 
			unit = managers.player:server_drop_carry( loot.carry_id, loot.value, true, false, 1, self._values.position, self._values.rotation, dir, 0 )
		else
			print( "NO MORE LOOT TO DISTRIBUTE" )
		end
	end
	
	if alive( unit ) then
		self:_check_triggers( "spawn", unit )
		unit:carry_data():set_mission_element( self )
	end
	
	ElementLootBag.super.on_executed( self, instigator )
end

function ElementLootBag:add_trigger( id, type, callback )
	self._triggers[ type ] = self._triggers[ type ] or {}
	self._triggers[ type ][ id ] = { callback = callback }
	-- self._triggers[ id ] = { amount = amount, callback = callback } 
end

function ElementLootBag:_check_triggers( type, instigator )
	-- print( "ElementCounter:_check_triggers", type )
	if not self._triggers[ type ] then
		return
	end
		
	for id,cb_data in pairs( self._triggers[ type ] ) do
		cb_data.callback( instigator )
		-- self:remove_trigger( id ) -- Really?
	end
end

function ElementLootBag:trigger( type, instigator )
	-- print( "ElementLootBag:trigger", type, instigator )
	self:_check_triggers( type, instigator )
end

--------------------------------------------------------------------------------------

ElementLootBagTrigger = ElementLootBagTrigger or class( CoreMissionScriptElement.MissionScriptElement )

function ElementLootBagTrigger:init( ... )
	ElementLootBagTrigger.super.init( self, ... )
end

function ElementLootBagTrigger:on_script_activated()
	for _,id in ipairs( self._values.elements ) do
		local element = self:get_mission_element( id )
		element:add_trigger( self._id, self._values.trigger_type, callback( self, self, "on_executed" ) )
	end
end

function ElementLootBagTrigger:client_on_executed( ... )
	-- self:on_executed( ... )
end

function ElementLootBagTrigger:on_executed( instigator )
	if not ( self._values.enabled ) then
		return
	end
	
	-- print( "ElementLootBagTrigger:on_executed", instigator )
	
	ElementLootBagTrigger.super.on_executed( self, instigator )
end
