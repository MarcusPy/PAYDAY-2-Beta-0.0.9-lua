core:import( "CoreAiLayer" )
core:import( "CoreHeatmapLayer" )

-- Stone Cold
require "lib/units/editor/SpawnEnemyGroupElement"
require "lib/units/editor/EnemyPreferedElement"
require "lib/units/editor/AIGraphElement"
require "lib/units/editor/WaypointElement"
require "lib/units/editor/SpawnCivilianElement"
require "lib/units/editor/SpawnCivilianGroupElement"
require "lib/units/editor/LookAtTrigger"
require "lib/units/editor/MissionEnd"
require "lib/units/editor/ObjectiveElement"
require "lib/units/editor/ConsoleCommandElement"
require "lib/units/editor/DialogueElement"
require "lib/units/editor/HeatElement"
require "lib/units/editor/HintElement"
require "lib/units/editor/MoneyElement"
require "lib/units/editor/AiGlobalEventElement"
require "lib/units/editor/EquipmentElement"
require "lib/units/editor/AreaMinPoliceForceUnitElement"
require "lib/units/editor/PlayerStateUnitElement"
require "lib/units/editor/ActionMessageUnitElement"
require "lib/units/editor/GameDirectionElement"
require "lib/units/editor/PressureElement"
require "lib/units/editor/DangerZoneElement"
require "lib/units/editor/ScenarioEventElement"
require "lib/units/editor/KillzoneElement"
require "lib/units/editor/SpecialObjectiveElement"
require "lib/units/editor/SpecialObjectiveTriggerElement"
require "lib/units/editor/SpecialObjectiveGroupElement"
require "lib/units/editor/SecretAssignmentElement"
require "lib/units/editor/PlayerStateTriggerUnitElement"
require "lib/units/editor/DifficultyElement"
require "lib/units/editor/BlurZoneElement"
require "lib/units/editor/AIRemoveElement"
require "lib/units/editor/FlashlightElement"
require "lib/units/editor/TeammateCommentElement"
require "lib/units/editor/FakeAssaultStateElement"
require "lib/units/editor/WhisperStateElement"
require "lib/units/editor/DifficultyLevelCheckElement"
require "lib/units/editor/AwardAchievmentElement"
require "lib/units/editor/PlayerNumberCheckElement"
require "lib/units/editor/PointOfNoReturnElement"
require "lib/units/editor/FadeToBlackElement"
require "lib/units/editor/AlertTriggerElement"
require "lib/units/editor/FeedbackElement"
require "lib/units/editor/ExplosionElement"
require "lib/units/editor/FilterElement"
require "lib/units/editor/DisableUnitElement"
require "lib/units/editor/EnableUnitElement"
require "lib/units/editor/SmokeGrenadeElement"
require "lib/units/editor/SetOutlineElement"
require "lib/units/editor/DisableShoutElement"
require "lib/units/editor/ExplosionDamageElement"
require "lib/units/editor/SequenceCharacterElement"
require "lib/units/editor/PlayerStyleElement"
require "lib/units/editor/DropinStateElement"
require "lib/units/editor/BainStateElement"
require "lib/units/editor/BlackscreenVariantElement"
require "lib/units/editor/AccessCameraElement"
require "lib/units/editor/AIAttentionElement"
require "lib/units/editor/MissionFilterElement"
require "lib/units/editor/AIAreaElement"
require "lib/units/editor/SecurityCameraElement"
require "lib/units/editor/CarryElement"
require "lib/units/editor/LootBagElement"
require "lib/units/editor/JobValueElement"
require "lib/units/editor/JobStageAlternativeElement"
require "lib/units/editor/NavObstacleElement"
require "lib/units/editor/LootSecuredTriggerElement"
require "lib/units/editor/MandatoryBagsElement"
require "lib/units/editor/AssetTriggerElement"
require "lib/units/editor/SpawnDeployableElement"
require "lib/units/editor/InventoryDummyElement"
require "lib/units/editor/FilterProfileElement"
require "lib/units/editor/FleePointElement"

-- Old Fortress
require "lib/units/editor/SpawnPlayerElement"
require "lib/units/editor/EnemyDummyTrigger"
require "lib/units/editor/SpawnEnemyElement"

-- FFS COMPIIIILE!

WorldEditor = WorldEditor or class( CoreEditor )

function WorldEditor:init( game_state_machine )
	WorldEditor.super.init( self, game_state_machine )
	
	Network:set_multiplayer( true )
	managers.network:host_game()
end

function WorldEditor:_init_mission_difficulties()
	self._mission_difficulties = { "easy", "normal", "hard", "overkill", "overkill_145" }
	self._mission_difficulty = "normal"
end

function WorldEditor:_init_mission_players()
	self._mission_players = { 1, 2, 3, 4 }
	self._mission_player = 1
end

-- Called from CoreEditor when adding layers. This function allows the project to add own layers.
function WorldEditor:_project_init_layer_classes()
	self:add_layer( "Ai",		CoreAiLayer.AiLayer )
	self:add_layer( "Heatmap",	CoreHeatmapLayer.HeatmapLayer )
end

-- Called from CoreEditor when initiating slot masks. This function allows the project to set up own slotmasks as well as add to others
function WorldEditor:_project_init_slot_masks()
	self._go_through_units_before_simulaton_mask = self._go_through_units_before_simulaton_mask + 15
end

-- Called from CoreEditor when a simulation is started. This function will allow the project to do
-- project specific things at start of the simulation.
function WorldEditor:project_run_simulation( with_mission )
	Global.game_settings.difficulty = self._mission_difficulty
	managers.network:host_game()
	managers.statistics:start_session( { from_beginning = true } ) -- Mayby only a test
	managers.player:on_simulation_started()	-- remove player respawn data
	managers.navigation:on_simulation_started()	-- removes dummy units. registers covers
	managers.groupai:on_simulation_started()	-- starts heat decay
		
	if not with_mission then
		managers.player:set_player_state( "standard" )
		managers.groupai:state():set_AI_enabled( false )
		managers.network:register_spawn_point( -1, { position = self._vp:camera():position(), rotation = Rotation:yaw_pitch_roll( self._vp:camera():rotation():yaw(), 0, 0 ) } )	
		-- managers.player:spawn_players( self._vp:camera():position(), Rotation:yaw_pitch_roll( 0, 0, 0 ) )
	end
	managers.network:game():on_load_complete()
	managers.network:game():spawn_players()
	
	managers.mission:set_mission_filter( self:layer( "Level Settings" ):get_mission_filter() )
end

function WorldEditor:_project_check_unit( unit )
	if unit:unit_data().secret_assignment_id then
		managers.secret_assignment:register_unit( unit )
	end
end

-- Called from CoreEditor when the simulation is stopped. This function will allow the project to clean for example
-- managers, to be ready for a new simulation.
function WorldEditor:project_stop_simulation()
	managers.hud:on_simulation_ended()
	managers.hud:clear_waypoints()
	managers.network:unregister_all_spawn_points()
	managers.menu:close_menu( "menu_pause" )
	managers.objectives:reset()
	setup:freeflight():disable()
	
	managers.groupai:on_simulation_ended()	-- stops group AI
	managers.enemy:on_simulation_ended() -- resets enemy data
	managers.navigation:on_simulation_ended() -- unregisters covers
	managers.dialog:on_simulation_ended() -- clear character assigned names
	managers.hint:on_simulation_ended() -- reset counters
	-- managers.experience:reset()
	managers.player:soft_reset()
	managers.secret_assignment:reset()
	managers.statistics:stop_session() -- Mayby only a test
	managers.environment_controller:set_blurzone( 0 ) -- Stop blur zones
	managers.music:stop() -- Stop muisc again, end assault restarts it
	managers.game_play_central:on_simulation_ended()	-- Reset flashlight state for example
	managers.criminals:on_simulation_ended()
	managers.loot:on_simulation_ended()
	managers.assets:on_simulation_ended()
end

-- Called from CoreEditor when a simulation is stopped. This function will allow the project to clean away units
-- that might have been created during the simualtion.
function WorldEditor:project_clear_units()
	managers.groupai:state():set_AI_enabled( false ) -- remove AI characters
	
	local units = World:find_units_quick( "all", World:make_slot_mask( 0, 2, 4, 5, 6, 8, 9, 11, 12, 13, 14, 15, 16, 17, 18, 20, 21, 22, 23, 24, 25 ) ) -- , 38 ) )
	for _,unit in ipairs( units ) do
		World:delete_unit( unit )
	end
end

-- Called from CoreEditor when a simulation is stopped. If any project specific layer needs to
-- be cleared (and then to be recreated) it should be done here. Typically by calling the clear function
-- on the layer( self._layers[ "Dynamics" ]:clear() ).
function WorldEditor:project_clear_layers()
end

-- Called from CoreEditor when a simulation is stopped. If any project specific layers has been cleared in
-- project_clear_layers, this is where it should be recreated. Typically by calling the load function on 
-- the layer( self._layers[ "Dynamics" ]:load( self._world_holder, Vector3( 0,0,0 ) ) ).
function WorldEditor:project_recreate_layers()
end

