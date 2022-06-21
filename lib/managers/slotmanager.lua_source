core:module( "SlotManager" )

core:import( "CoreSlotManager" )
core:import( "CoreClass" )

SlotManager = SlotManager or class( CoreSlotManager.SlotManager )

function SlotManager:init()
	SlotManager.super.init( self )

	-- I've moved some slots and masks to CoreSlotManager, please look there before adding
	-- a slot that seems free, could be taken in Core. /Martin Waern, Sthlm

	local C_UnitManager = World:unit_manager()

-- 		0. Unit Trashcan.	Defined in Core
-- 		1. Statics.		Defined in Core

-- 		2. Players
	C_UnitManager:set_slot_infinite( 2 )

-- 		3. Player Husks
	C_UnitManager:set_slot_infinite( 3 )

-- 		4. Harmless Players	( mask off, neutralized )
	C_UnitManager:set_slot_infinite( 4 )

-- 		5. Harmless Player Husks ( mask off, neutralized )
	C_UnitManager:set_slot_infinite( 5 )
			
-- 		7. Cameras
	C_UnitManager:set_slot_infinite( 7 )
	
-- 		8. Shields
	C_UnitManager:set_slot_infinite( 8 )

-- 		9. Melee Weapons
	C_UnitManager:set_slot_infinite( 9 )

-- 		10. Mission elements (formerly known as hub elements).	Defined in Core
-- 		11. Dynamics.	Defined in Core
	
-- 		12. Enemy
	C_UnitManager:set_slot_infinite( 12 )

-- 		13. Ranged Weapons
	C_UnitManager:set_slot_infinite( 13 )
	
-- 		14. Use items (grenades, trip mines etc)
	C_UnitManager:set_slot_infinite( 14 )

-- 		15. AI blocker. ( Editor helper unit ).	Defined in Core

-- 		16. Criminal team AI ( harmless slot is 24 )
	C_UnitManager:set_slot_infinite( 16 )
	
-- 		17. Corpses
	C_UnitManager:set_slot_infinite( 17 )
	
-- 		18. Debris
	C_UnitManager:set_slot_limited( 18, 10 )
		
-- 		19. Editor Units.	Defined in Core

-- 		20. Pickups
	C_UnitManager:set_slot_infinite( 20 )

-- 		21. Civilians ( when free/active )
	C_UnitManager:set_slot_infinite( 21 )
	
-- 		22. Bound Hostages ( civilian and police ) handcuffed players are in slot 4
	C_UnitManager:set_slot_infinite( 22 )

-- 		23. Limited Pickups
	C_UnitManager:set_slot_limited( 23, 20 )

-- 		24. Harmless Criminal team AI ( mask off, neutralized )
	C_UnitManager:set_slot_infinite( 24 )

-- 		25. Sentry Gun
	C_UnitManager:set_slot_infinite( 25 )
	
-- 		26. Broken Sentry Gun
	C_UnitManager:set_slot_infinite( 26 )
	
-- 		29. Brush units.	Defined in Core
	
-- 		33. Crowd Enemy
	C_UnitManager:set_slot_infinite( 33 )
	
-- 		34. Crowd Ally
	C_UnitManager:set_slot_infinite( 34 )

-- 		35. Wires/Cables.	Defined in Core

-- 		36. Cover Units
	C_UnitManager:set_slot_infinite( 36 )

-- 		37. Bain
	C_UnitManager:set_slot_infinite( 37 )

-- 		38. AI Attention Object
	C_UnitManager:set_slot_infinite( 38 )
	
	
	
-- SLOTMASKS
	self._masks[ "all" ] = World:make_slot_mask( 1, 2, 3, 10, 11, 12, 19, 29, 33, 34, 35, 36, 37, 38 )
	self._masks[ "players" ] = World:make_slot_mask( 2, 3, 4 ,5 )
	self._masks[ "criminals" ] = World:make_slot_mask( 2, 3, 16, 25 )
	self._masks[ "criminals_no_deployables" ] = World:make_slot_mask( 2, 3, 16 )
	self._masks[ "all_criminals" ] = World:make_slot_mask( 2, 3, 4, 5, 16, 24, 25 )
	self._masks[ "raycastable_characters" ] = World:make_slot_mask( 3, 12, 16, 17, 25 )
	self._masks[ "harmless_criminals" ] = World:make_slot_mask( 4, 5, 24 )
	self._masks[ "harmless_player_criminals" ] = World:make_slot_mask( 4, 5 )
	self._masks[ "long_distance_interaction" ] = World:make_slot_mask( 3, 12, 16, 21 )
	self._masks[ "trip_mine_targets" ] = World:make_slot_mask( 12, 21, 22, 33 )
	self._masks[ "cameras" ] = World:make_slot_mask( 7 )
	self._masks[ "enemies" ] = World:make_slot_mask( 12, 33 )
	self._masks[ "persons" ] = World:make_slot_mask( 2, 3, 4, 5, 12, 16, 21, 22, 24, 33 )
	self._masks[ "crowd" ] = World:make_slot_mask( 33, 34 )
	self._masks[ "melee_equipment" ] = World:make_slot_mask( 8, 9 )					-- Handheld weapons and shields used in melee
	self._masks[ "body_area_damage" ] = World:make_slot_mask( 1, 11 )							-- Which units that want area damage. The unit extension receive damage from all body extensions (one for each affected body in ie an explosion).
	self._masks[ "unit_area_damage" ] = self._masks[ "players" ] + self._masks[ "enemies" ]		-- Which units that want area damage. The unit extension receive only one damage from all body extensions (once per ie explosion).
	self._masks[ "area_damage_blocker" ] = World:make_slot_mask( 1, 3, 8, 11 )						-- Rays against these units when determining if area damage is blocked.
	self._masks[ "bullet_impact_targets" ] = World:make_slot_mask( 1, 2, 8, 11, 12, 14, 16, 17, 18, 21, 22, 25, 26, 33, 34, 35 )
	self._masks[ "bullet_impact_targets_no_police" ] = World:make_slot_mask( 1, 2, 8, 11, 14, 16, 17, 25, 33, 34, 35 )
	self._masks[ "bullet_impact_targets_no_criminals" ] = World:make_slot_mask( 1, 8, 11, 12, 14, 17, 33, 34, 35 )
	self._masks[ "bullet_impact_targets_sentry_gun" ] = World:make_slot_mask( 1, 8, 11, 12, 14, 17, 22, 33, 34, 35 )
	self._masks[ "bullet_impact_targets_no_AI" ] = World:make_slot_mask( 1, 2, 8, 11, 14, 17, 33, 34, 35 )
	self._masks[ "bullet_impact_targets_shooting_death" ] = World:make_slot_mask( 1, 8, 11, 12, 14, 17, 21, 22, 33, 34, 35 )
	self._masks[ "bullet_blank_impact_targets" ] = World:make_slot_mask( 1, 8, 11, 35 )
	self._masks[ "bullet_physics_push" ] = World:make_slot_mask( 11, 18 )
	self._masks[ "AI_visibility" ] = World:make_slot_mask( 1, 11, 38 )	-- Units of slot 15 block AI rays but only exist in Editor mode
	self._masks[ "AI_visibility_sentry_gun" ] = World:make_slot_mask( 1, 11, 17, 38 )	-- Sentry gun does not see through corpses
	self._masks[ "AI_graph_obstacle_check" ] = World:make_slot_mask( 1, 11, 15 )	-- Used when generating navigation fields
	self._masks[ "player_autoaim" ] = World:make_slot_mask( 12 )	-- units in this slotmask will be affected by autohit statistic
	self._masks[ "cover" ] = World:make_slot_mask( 36 )
	self._masks[ "env_effect" ] = World:make_slot_mask( 1 )
	self._masks[ "world_geometry" ] = World:make_slot_mask( 1, 11 )
	self._masks[ "trip_mine_placeables" ] = World:make_slot_mask( 1 )
	self._masks[ "pickups" ] = World:make_slot_mask( 20, 23 )
	self._masks[ "flesh" ] = World:make_slot_mask( 12, 16, 17, 21, 22, 24, 33 )
	self._masks[ "footstep" ] = World:make_slot_mask( 1, 8, 11 )
	
-- Complete those inherrited from Core
	self._masks[ "editor_all" ] = self._masks[ "editor_all" ] + 36 + 38
	self._masks[ "statics_layer" ] = self._masks[ "statics_layer" ] + 36 + 38
end

CoreClass.override_class( CoreSlotManager.SlotManager, SlotManager )
