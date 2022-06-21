require "lib/managers/NavFieldBuilder"

local mvec3_n_equal = mvector3.not_equal
local mvec3_set = mvector3.set
local mvec3_set_st = mvector3.set_static
local mvec3_set_z = mvector3.set_z
local mvec3_sub = mvector3.subtract
local mvec3_norm = mvector3.normalize
local mvec3_dir = mvector3.direction
local mvec3_add = mvector3.add
local mvec3_mul = mvector3.multiply
local mvec3_div = mvector3.divide
local mvec3_lerp = mvector3.lerp
local mvec3_cpy = mvector3.copy
local mvec3_set_l = mvector3.set_length
local mvec3_dot = mvector3.dot
local mvec3_cross = mvector3.cross
local mvec3_dis = mvector3.distance
local mvec3_rot = mvector3.rotate_with

local math_abs = math.abs
local math_max = math.max
local math_clamp = math.clamp
local math_ceil = math.ceil
local math_floor = math.floor

local temp_vec1 = Vector3()
local temp_vec2 = Vector3()

NavigationManager = NavigationManager or class()

NavigationManager.nav_states = { "allow_access", "forbid_access" }

NavigationManager.COVER_RESERVED = 4
NavigationManager.COVER_RESERVATION = 5

NavigationManager.ACCESS_FLAGS_VERSION = 1

NavigationManager.ACCESS_FLAGS = { -- Max 30 flags
	"civ_male",
	"civ_female",
	"gangster",
	"security",
	"security_patrol",
	"cop",
	"fbi",
	"swat",
	"murky",
	"sniper",
	"spooc",
	"shield",
	"tank",
	"taser",
	"teamAI1",
	"teamAI2",
	"teamAI3",
	"teamAI4",
	"SO_ID1",
	"SO_ID2",
	"SO_ID3",
	
	"pistol",
	"rifle",
	
	"ntl",
	"hos",
	
	"run",
	"fumble",
	"sprint",
	"crawl",
	"climb",
	--"jump",
	--"repel",
}

NavigationManager.ACCESS_FLAGS_OLD = {}

function NavigationManager:init()
	
	self._debug = SystemInfo:platform() == Idstring("WIN32") and Application:production_build()
	self._builder = NavFieldBuilder:new()
	
	self._get_room_height_at_pos = self._builder._get_room_height_at_pos
	self._check_room_overlap_bool = self._builder._check_room_overlap_bool
	
	self._door_access_types = self._builder._door_access_types
	
	self._opposite_side_str = self._builder._opposite_side_str
	self._perp_pos_dir_str_map = self._builder._perp_pos_dir_str_map
	self._perp_neg_dir_str_map = self._builder._perp_neg_dir_str_map
	self._dim_str_map = self._builder._dim_str_map
	self._perp_dim_str_map = self._builder._perp_dim_str_map
	self._neg_dir_str_map = self._builder._neg_dir_str_map
	self._x_dir_str_map = self._builder._x_dir_str_map
	self._dir_str_to_vec = self._builder._dir_str_to_vec
	
	self._geog_segment_size = self._builder._geog_segment_size
	self._grid_size = self._builder._grid_size
	
	self._rooms = {}
	self._room_doors = {}
	self._geog_segments = {}
	self._nr_geog_segments = nil
	self._visibility_groups = {}
	self._nav_segments = {}
	
	self._draw_enabled = false
	
	self._coarse_searches = {}
	
	self._covers = {}
	
	self._next_pos_rsrv_expiry = false -- the nearest position reservation expiry time
	
	if Application:editor() then
		self._nav_links = {} -- all registered nav_links
	end
	self._quad_field = World:quad_field()
	self._quad_field:set_nav_link_filter( NavigationManager.ACCESS_FLAGS )
	
	self._pos_rsrv_filters = {}	-- index = reservation id, value = state
	
	self._obstacles = {}
	if self._debug then
		self._pos_reservations = {}
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_init_draw_data()
	local data = {}

	local duration = 10
	data.duration = duration
	local brush = {}
	brush.door = Draw:brush( Color(0.1,0,1,1), duration )
	brush.room_diag = Draw:brush( Color(1,0.5,0.5,0), duration )
	brush.room_diag_disabled = Draw:brush( Color(1,1,0,0), duration )
	brush.room_border = Draw:brush( Color(1,0.5,0.5,0.5), duration )
	brush.coarse_graph = Draw:brush( Color(0.2,0.05,0.2,0.9) )
	brush.vis_graph_rooms = Draw:brush( Color(0.6,0.5,0.2,0.9), duration )
	brush.vis_graph_node = Draw:brush( Color(1,0.6,0,0.9), duration )
	brush.vis_graph_links = Draw:brush( Color(0.2,0.8,0.1,0.6), duration )
	data.brush = brush
	
	local offsets = {
		Vector3(-1,-1),
		Vector3(-1,1),
		Vector3(1,-1),
		Vector3(1,1)
	}
	data.offsets = offsets
	
	data.next_draw_i_room = 1
	data.next_draw_i_door = 1
	data.next_draw_i_coarse = 1
	data.next_draw_i_vis = 1
	
	self._draw_data = data
end

-----------------------------------------------------------------------------------

function NavigationManager:update( t, dt )
	
	if self._debug then
		self._builder:update( t, dt )
		
		if self._draw_enabled then
			local options = self._draw_enabled
			local data = self._draw_data
			local progress = math.clamp( ( t - data.start_t ) / ( data.duration * 0.5 ), 0, 1 )
			if options.quads then
				self:_draw_rooms( progress )
			end
			if options.doors then
				self:_draw_doors( progress )
			end
			if options.blockers then
				self:_draw_nav_blockers()
			end
			if options.vis_graph then
				self:_draw_visibility_groups( progress )
			end
			if options.coarse_graph then
				self:_draw_coarse_graph()
			end
			if options.covers then
				self:_draw_covers()
			end
			if options.pos_rsrv then
				self:_draw_pos_reservations( t )
			end
			
			if progress == 1 then
				self._draw_data.start_t = t
			end
		end
	end
	
	self:_commence_coarce_searches( t )
	
	--self:_draw_geographic_segments()
	--self:_draw_covers()
	--self:_draw_anim_nav_links()
	
	--[[local cam = managers.editor:camera()
	local fwd = cam:rotation():y()
	local pos_from = cam:position()
	local pos_to = pos_from + fwd * 10000
	local ray = World:raycast( "ray", pos_from, pos_to, "slot_mask", self._builder._slotmask, "ray_type", "vis_graph" )
	if ray then
		Application:draw_sphere( ray.position, 30, 1,0,0)
	end]]
end

-----------------------------------------------------------------------------------


function NavigationManager:_draw_pos_reservations( t )
	local to_remove = {}
	for key, res in pairs( self._pos_reservations ) do
		local entry = res[1]
		
		if entry.expire_t and entry.expire_t < t then
			table.insert( to_remove, key )
		end
	
		if not entry.expire_t then
			Application:draw_sphere( entry.position, entry.radius,  0,0,0 )
			
			if res.unit then
				Application:draw_cylinder( entry.position, res.unit:movement():m_pos(),  3,  0,0,0 )
			end
		else
			Application:draw_sphere( entry.position, entry.radius,  0.3,0.3,0.3 )
		end
	end
	
	for _, key in ipairs( to_remove ) do
		self._pos_reservations[key] = nil
	end
end



function NavigationManager:get_save_data()
	local save_data = {}
	
	if not self._builder._building then
		-- clean up runtime data
		for i_room, room in ipairs( self._rooms ) do
			room.covers = nil
			room.nav_links = nil
		end
		
		for seg_id, nav_seg in pairs( self._nav_segments ) do
			nav_seg.disabled = nil
			if nav_seg.neighbours then
				for other_nav_seg, door_list in pairs( nav_seg.neighbours ) do
					for i_door, door_id in ipairs( door_list ) do
						if type( door_id ) == "table" then -- nav_link
							door_list[ i_door ] = nil
							if not next( door_list ) then
								nav_seg.neighbours[ other_nav_seg ] = nil
								break
							end
						end
					end
				end
			end
		end
		
		for i_seg, g_seg in ipairs( self._geog_segments ) do
			g_seg.rsrv_pos = nil
		end
		
		-- ...and save
		
		local grid_size = self._grid_size
		
		save_data.room_borders_x_pos = {}
		save_data.room_borders_x_neg = {}
		save_data.room_borders_y_pos = {}
		save_data.room_borders_y_neg = {}
		
		save_data.room_heights_xp_yp = {}
		save_data.room_heights_xp_yn = {}
		save_data.room_heights_xn_yp = {}
		save_data.room_heights_xn_yn = {}
		
		save_data.room_vis_groups = {}
		
		local t_ins = table.insert
		for i_room, room in ipairs( self._rooms ) do
			t_ins( save_data.room_borders_x_pos, room.borders.x_pos / grid_size )
			t_ins( save_data.room_borders_x_neg, room.borders.x_neg / grid_size )
			t_ins( save_data.room_borders_y_pos, room.borders.y_pos / grid_size )
			t_ins( save_data.room_borders_y_neg, room.borders.y_neg / grid_size )
			
			t_ins( save_data.room_heights_xp_yp, room.height.xp_yp )
			t_ins( save_data.room_heights_xp_yn, room.height.xp_yn )
			t_ins( save_data.room_heights_xn_yp, room.height.xn_yp )
			t_ins( save_data.room_heights_xn_yn, room.height.xn_yn )
			
			t_ins( save_data.room_vis_groups, room.vis_group )
		end
		
		save_data.door_low_pos = {}
		save_data.door_high_pos = {}
		save_data.door_low_rooms = {}
		save_data.door_high_rooms = {}
		
		for i_door, door in ipairs( self._room_doors ) do
			t_ins( save_data.door_low_pos, Vector3( door.pos.x / grid_size, door.pos.y / grid_size, door.pos.z ) )
			t_ins( save_data.door_high_pos, Vector3( door.pos1.x / grid_size, door.pos1.y / grid_size, door.pos1.z ) )
			t_ins( save_data.door_low_rooms, door.rooms[1] )
			t_ins( save_data.door_high_rooms, door.rooms[2] )
		end
		
		--save_data.rooms = self._rooms
		--save_data.doors = self._room_doors
		
		--save_data.geog_segments = self._geog_segments
		--save_data.geog_segments_offset = self._geog_segment_offset
		--save_data.nr_geog_segments = self._nr_geog_segments
		
		save_data.vis_groups = self._visibility_groups
		save_data.nav_segments = self._nav_segments
		save_data.helper_blockers = self._builder._helper_blockers
		
		save_data.version = NavFieldBuilder._VERSION
	end
	return ScriptSerializer:to_generic_xml( save_data )
	
end

-----------------------------------------------------------------------------------

function NavigationManager:set_load_data( data )
	if data.version == NavFieldBuilder._VERSION then
		local t_ins = table.insert
		
		local grid_size = self._grid_size
		local allow_debug_info = self._debug
		
		self._rooms = {}
		local nr_rooms = #data.room_borders_x_pos
		local i_room = 1
		while i_room <= nr_rooms do
			local room = {
				borders = {
					x_pos = data.room_borders_x_pos[ i_room ] * grid_size,
					x_neg = data.room_borders_x_neg[ i_room ] * grid_size,
					y_pos = data.room_borders_y_pos[ i_room ] * grid_size,
					y_neg = data.room_borders_y_neg[ i_room ] * grid_size
				},
				height = {
					xp_yp = data.room_heights_xp_yp[ i_room ],
					xp_yn = data.room_heights_xp_yn[ i_room ],
					xn_yp = data.room_heights_xn_yp[ i_room ],
					xn_yn = data.room_heights_xn_yn[ i_room ]
				},
				vis_group = data.room_vis_groups[ i_room ]
			}
			if allow_debug_info then
				room.doors = {
					x_pos = {},
					x_neg = {},
					y_pos = {},
					y_neg = {}
				}
			end
			
			t_ins( self._rooms, room )
			i_room = i_room + 1
		end
		
		Application:check_termination()
		
		local mvec3_set_x = mvector3.set_x
		local mvec3_set_y = mvector3.set_y
		self._room_doors = {}
		local nr_doors = #data.door_low_rooms
		local i_door = 1
		while i_door <= nr_doors do
			local door = {
				pos = data.door_low_pos[ i_door ],
				pos1 = data.door_high_pos[ i_door ],
				rooms = {
					data.door_low_rooms[ i_door ],
					data.door_high_rooms[ i_door ]
				}
			}
			mvec3_set_x( door.pos, door.pos.x * grid_size )
			mvec3_set_y( door.pos, door.pos.y * grid_size )
			mvec3_set_x( door.pos1, door.pos1.x * grid_size )
			mvec3_set_y( door.pos1, door.pos1.y * grid_size )
			
			t_ins( self._room_doors, door )
			
			if allow_debug_info then
				local door_dimention = door.pos.y == door.pos1.y and "x" or "y"
				t_ins( self._rooms[ door.rooms[1] ].doors[ door_dimention.."_pos" ], i_door )
				t_ins( self._rooms[ door.rooms[2] ].doors[ door_dimention.."_neg" ], i_door )
			end
			
			i_door = i_door + 1
		end
		
		Application:check_termination()
		
		if data.vis_groups and next( data.vis_groups ) then
			self:_reconstruct_geographic_segments()
		end
		
		--self._rooms = data.rooms 
		--self._room_doors = data.doors
		
		--self._geog_segments = data.geog_segments
		--self._geog_segment_offset = data.geog_segments_offset
		--self._nr_geog_segments = data.nr_geog_segments
		self._nav_segments = data.nav_segments
		
		self._visibility_groups = data.vis_groups
		
		if allow_debug_info then
			local helper_blockers = data.helper_blockers	-- Only load helper units in editor mode
			
			local builder_data = { _rooms = self._rooms,
										_room_doors = self._room_doors,
										_geog_segments = self._geog_segments,
										_geog_segment_offset = self._geog_segment_offset,
										_nr_geog_segments = self._nr_geog_segments,
										_visibility_groups = self._visibility_groups,
										_helper_blockers = helper_blockers,
										_nav_segments = self._nav_segments }
										
			self._builder:set_field_data( builder_data )
		end
		
		Application:check_termination()
		
		if self:is_data_ready() then
			self:send_nav_field_to_engine()
			if allow_debug_info then
				self:_complete_nav_field_for_debug()
				self:set_debug_draw_state( nil )
			else
				self:_strip_nav_field_for_gameplay()
			end
		end
	else
		Application:error( "! Error in NavigationManager:set_load_data( data ). The NavField in this level needs to be re-built using the latest version of the NavFieldBuilder." )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_reconstruct_geographic_segments()
	local tab_ins = table.insert
	local m_ceil = math.ceil
	
	local segments = {}
	self._geog_segments = segments
	
	local seg_size = self._geog_segment_size
	
	local level_limit_x_pos = -1000000
	local level_limit_x_neg = 1000000
	local level_limit_y_pos = -1000000
	local level_limit_y_neg = 1000000
	
	--	measure the size of the level
	for i_room, room in ipairs( self._rooms ) do
		local borders = room.borders
		if borders.x_pos > level_limit_x_pos then
			level_limit_x_pos = borders.x_pos
		end
		if borders.x_neg < level_limit_x_neg then
			level_limit_x_neg = borders.x_neg
		end
		if borders.y_pos > level_limit_y_pos then
			level_limit_y_pos = borders.y_pos
		end
		if borders.y_neg < level_limit_y_neg then
			level_limit_y_neg = borders.y_neg
		end
	end
	
	local safety_margin = 0
	--	Add a safety margin
	level_limit_x_pos = level_limit_x_pos + safety_margin
	level_limit_x_neg = level_limit_x_neg - safety_margin
	level_limit_y_pos = level_limit_y_pos	+ safety_margin
	level_limit_y_neg = level_limit_y_neg	- safety_margin
	
	--print( "level_limit_x_neg", level_limit_x_neg, "level_limit_x_pos", level_limit_x_pos, "level_limit_y_neg", level_limit_y_neg, "level_limit_y_pos", level_limit_y_pos )
	
	--	Set the botton left corner of the segment map
	self._geog_segment_offset = Vector3( level_limit_x_neg, level_limit_y_neg, 2000 )
	local seg_offset = self._geog_segment_offset
	
	--print( "seg_offset", seg_offset )
	
	--	Save the segement table dimentions 
	local nr_seg_x = m_ceil( ( level_limit_x_pos - level_limit_x_neg ) / seg_size )
	local nr_seg_y = m_ceil( ( level_limit_y_pos - level_limit_y_neg ) / seg_size )
	self._nr_geog_segments = { x = nr_seg_x, y = nr_seg_y }
	
	--print( "nr_seg_x", nr_seg_x, "nr_seg_y", nr_seg_y )
	
	--local max_nr_rooms_per_seg = 0
	--	Create indexed segment data tables	
	local i_seg = 1
	while( i_seg ) <= nr_seg_x * nr_seg_y do
		local segment = {}
		local seg_borders = self:_calculate_geographic_segment_borders( i_seg )
		
		local nr_rooms = 0
		for i_room, room in ipairs( self._rooms ) do
			local room_borders = room.borders
			if NavFieldBuilder._check_room_overlap_bool( seg_borders, room_borders ) then
				--	The segments intersects with the room. Add the room index to the list of rooms of the segment
				segment.rooms = segment.rooms or {}
				segment.rooms[ i_room ] = true
				--room.geog_segments[ i_seg ] = true
				nr_rooms = nr_rooms + 1
			end
			
		end
		
		--max_nr_rooms_per_seg = math.max( nr_rooms, max_nr_rooms_per_seg )
		if next( segment ) then
			segments[ i_seg ] = segment
		end
		i_seg = i_seg + 1
	end
	
	-- Now that we have indexed all the segments remove the empty ones
	i_seg = nr_seg_x * nr_seg_y
	while i_seg > 0 do
		if segments[ i_seg ] == false then
			segments[ i_seg ] = nil
		end
		i_seg = i_seg -1
	end

	--print( "max_nr_rooms_per_seg", max_nr_rooms_per_seg )
	--print( "done" )	
end

-----------------------------------------------------------------------------------

function NavigationManager:_calculate_geographic_segment_borders( i_seg )
	local seg_borders = {}
	
	local nr_seg_x = self._nr_geog_segments.x
	local seg_offset = self._geog_segment_offset
	local seg_size = self._geog_segment_size
	local grid_coorids = { 1 + (i_seg-1)%nr_seg_x, math.ceil( i_seg / nr_seg_x ) }
	seg_borders.x_pos = seg_offset.x + grid_coorids[1] * seg_size
	seg_borders.x_neg = seg_borders.x_pos - seg_size
	seg_borders.y_pos = seg_offset.y + grid_coorids[2] * seg_size
	seg_borders.y_neg = seg_borders.y_pos - seg_size
		
	return seg_borders
end

-----------------------------------------------------------------------------------

function NavigationManager:clear()
	self._builder:clear()
	self:_clear()
end

-----------------------------------------------------------------------------------

function NavigationManager:_clear()
	self:_unregister_cover_units()
	self._rooms = {}
	self._room_doors = {}
	self._geog_segments = {}
	self._nr_geog_segments = nil
	self._visibility_groups = {}
	self._nav_segments = {}
	
	self._debug_geographic_segments = {}
	self._debug_vis_groups = {}
	
	self._coarse_searches = {}
end

-----------------------------------------------------------------------------------

function NavigationManager:is_data_ready()
	return self._nr_geog_segments and true or false
end

-----------------------------------------------------------------------------------
-- build_settings format: { { position = Vector3, id = any_object, color = Color } }
function NavigationManager:build_nav_segments( build_settings, complete_clbk )
	--print( " NavigationManager:build_nav_segments()", inspect( build_settings ), complete_clbk )
	local draw_options = self._draw_enabled
	self:_clear()
	self:set_debug_draw_state( false )
	self._build_complete_clbk = complete_clbk
	self._builder:build_nav_segments( build_settings, callback( self, self, "build_complete_clbk", draw_options) )
end

-----------------------------------------------------------------------------------

function NavigationManager:build_complete_clbk( draw_options )
	self:_refresh_data_from_builder()
	if self:is_data_ready() then
		self:send_nav_field_to_engine()
	end
	
	self:set_debug_draw_state( draw_options )
	
	if self._build_complete_clbk then
		self._build_complete_clbk()
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_refresh_data_from_builder()
	self._rooms = self._builder._rooms
	self._room_doors = self._builder._room_doors
	self._geog_segments = self._builder._geog_segments
	self._geog_segment_offset = self._builder._geog_segment_offset
	self._nr_geog_segments = self._builder._nr_geog_segments
	self._visibility_groups = self._builder._visibility_groups
	self._nav_segments = self._builder._nav_segments
end

-----------------------------------------------------------------------------------
-- available states: forbid_access, allow_access
function NavigationManager:set_nav_segment_state( id, state )
	if not self._nav_segments[ id ] then
		debug_pause( "[NavigationManager:set_nav_segment_state] inexistent nav_segment", id )
		return
	end
	local wanted_state = state == "allow_access" and true or false
	local cur_state = self._quad_field:is_nav_segment_enabled( id )
	
	local seg_disabled_state
	if not wanted_state then
		seg_disabled_state = true
	end
	self._nav_segments[ id ].disabled = seg_disabled_state
	
	if wanted_state ~= cur_state then
		self._quad_field:set_nav_segment_enabled( id, wanted_state )
		managers.groupai:state():on_nav_segment_state_change( id, wanted_state )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:delete_nav_segment( id )
	--print( " NavigationManager:delete_nav_segment()", id )
	local draw_options = self._draw_enabled
	self:set_debug_draw_state( false )
	self._builder:delete_segment( id )
	self:_refresh_data_from_builder()
	self:set_debug_draw_state( draw_options )
end

-----------------------------------------------------------------------------------
-- complete_clbk is a callback object
function NavigationManager:build_visibility_graph( complete_clbk, all_visible, neg_filter, pos_filter, ray_dis  )
	--print( "[NavigationManager:build_visibility_graph] all_visible", all_visible, "pos_filter", inspect( pos_filter ), "neg_filter", inspect( neg_filter ), "ray_dis", ray_dis )
	local draw_options = self._draw_enabled
	self:set_debug_draw_state( false )
	self._build_complete_clbk = complete_clbk
	self._builder:build_visibility_graph( callback( self, self, "build_complete_clbk", draw_options ), all_visible, ray_dis, pos_filter, neg_filter )
end

-----------------------------------------------------------------------------------
-- options = { quads = true, doors = true, vis_graph = true, coarse_graph = true, blockers = true, covers = true }
function NavigationManager:set_debug_draw_state( options )
	if options and not self._draw_enabled then
		self:_init_draw_data()
		self._draw_data.start_t = TimerManager:game():time()
	end
	self._draw_enabled = options
end

-----------------------------------------------------------------------------------

function NavigationManager:set_selected_segment( unit )
	--print( "[NavigationManager:set_selected_segment]", unit and unit:unit_data().unit_id )
	self._selected_segment = unit and unit:unit_data().unit_id
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_rooms( progress )
	local selected_seg = self._selected_segment
	local room_mask
	if selected_seg and self._nav_segments[ selected_seg ] and next( self._nav_segments[ selected_seg ].vis_groups ) then
		room_mask = {}
		for _, i_vis_group in ipairs( self._nav_segments[ selected_seg ].vis_groups ) do
			local vis_group_rooms = self._visibility_groups[ i_vis_group ].rooms
			for i_room, _ in pairs( vis_group_rooms ) do
				room_mask[ i_room ] = true
			end
		end
	end
	local data = self._draw_data
	local rooms = self._rooms
	local nr_rooms = #rooms
	local i_room = data.next_draw_i_room
	local wanted_index = math.clamp( math.ceil( nr_rooms * progress ), 1, nr_rooms )
	while wanted_index >= i_room and i_room <= nr_rooms do
		local room = rooms[ i_room ]
		if ( not room_mask or room_mask[ i_room ] ) and not room.removed then
			self:_draw_room( room )
		end
		i_room = i_room + 1
	end
	if progress == 1 then
		data.next_draw_i_room = 1
	else
		data.next_draw_i_room = i_room
	end
	
	local obstructed_positions = self._quad_field:get_obstructed_quads()
	if obstructed_positions then
		for _, pos in ipairs( obstructed_positions ) do
			Application:draw_cylinder( pos, pos + math.UP * 5000, 12.5, 0.7, 0.2, 0 )
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_nav_blockers()
	if self._builder._helper_blockers then
		local mvec3_set = mvector3.set
		local mvec3_rot = mvector3.rotate_with
		local mvec3_add = mvector3.add
		local obj_name = Idstring( "help_blocker" )
		local nav_segments = self._builder._nav_segments
		local registered_blockers = self._builder._helper_blockers
		local all_blockers = World:find_units_quick( "all", 15 )
		for _, blocker_unit in ipairs( all_blockers ) do
			local id = blocker_unit:unit_data().unit_id
			if registered_blockers[ id ] then
				local draw_pos = blocker_unit:get_object( obj_name ):position()
				local nav_segment = registered_blockers[ id ]
				if nav_segments and nav_segments[ nav_segment ] and self._selected_segment == nav_segment then
					Application:draw_sphere( draw_pos, 30, 0,0,1 )
					Application:draw_cylinder( draw_pos, nav_segments[ nav_segment ].pos, 2, 0,0.3,0.6 )
				end
			end
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_room( room, instant )
	local draw, brushes, offsets
	
	if instant then
		offsets = {
			Vector3(-1,-1),
			Vector3(-1,1),
			Vector3(1,-1),
			Vector3(1,1)
		}
	else
		draw = self._draw_data
		brushes = draw and draw.brush
		offsets = draw and draw.offsets
	end
	
	local dir_vec_map = self._dir_str_to_vec
	local borders = room.borders
	local height = room.height
	
	local my_center = self._builder:_calculate_room_center( room )
	
	local xp_yp_draw = Vector3( borders.x_pos + offsets[1].x, borders.y_pos + offsets[1].y, height.xp_yp )
	local xp_yn_draw = Vector3( borders.x_pos + offsets[2].x, borders.y_neg + offsets[2].y, height.xp_yn )
	local xn_yp_draw = Vector3( borders.x_neg + offsets[3].x, borders.y_pos + offsets[3].y, height.xn_yp )
	local xn_yn_draw = Vector3( borders.x_neg + offsets[4].x, borders.y_neg + offsets[4].y, height.xn_yn )
	
	
	if instant then
		Application:draw_line( xp_yp_draw, xp_yn_draw, 0.5, 0.5, 0.5 )
		Application:draw_line( xn_yp_draw, xn_yn_draw, 0.5, 0.5, 0.5 )
		Application:draw_line( xp_yp_draw, xn_yp_draw, 0.5, 0.5, 0.5 )
		Application:draw_line( xp_yn_draw, xn_yn_draw, 0.5, 0.5, 0.5 )
		
		Application:draw_line( xp_yp_draw, xn_yn_draw, 0.5, 0.5, 0 )
		Application:draw_line( xn_yp_draw, xp_yn_draw, 0.5, 0.5, 0 )
	else
		--	Draw borders
		local brush = brushes.room_border
		brush:line( xp_yp_draw, xp_yn_draw )
		brush:line( xn_yp_draw, xn_yn_draw )
		brush:line( xp_yp_draw, xn_yp_draw )
		brush:line( xp_yn_draw, xn_yn_draw )
		
		-- Draw diagonals
		
		local nsi = room.vis_group and self:get_nav_seg_from_i_vis_group( room.vis_group )
		local ns = nsi and self._nav_segments[nsi]
		
		if ns and ns.disabled then
			brush = brushes.room_diag_disabled
		else
			brush = brushes.room_diag
		end
		
		brush:line( xp_yp_draw, xn_yn_draw )
		brush:line( xn_yp_draw, xp_yn_draw )
	end
	local expansion = room.expansion
	
	if expansion then
		for dir_str, side_expansion in pairs( expansion ) do
			for obstacle_type, obstacle_segments in pairs( side_expansion ) do
				local color, rad
				if obstacle_type == "walls" then
					rad = 3
					color = Vector3(1,0,0)
				elseif obstacle_type == "spaces" then
					rad = 2.2
					color = Vector3(0,1,0)
				elseif obstacle_type == "stairs" then
					rad = 1,8
					color = Vector3(1,0.4,0)
				elseif obstacle_type == "cliffs" then
					rad = 1,6
					color = Vector3(0.2,0.1,0)
				else
					rad = 1
					color = Vector3(0.5,0.5,0.5)
				end
				for i_obs_seg, obstacle_segment in pairs( obstacle_segments ) do
					Application:draw_cone( obstacle_segment[1], obstacle_segment[2], rad, color.x, color.y, color.z )
					--print( dir_str.." "..obstacle_type.." "..tostring(i_obs_seg), obstacle_segment[1], obstacle_segment[2] )
				end
			end
		end
	end
	
	if room.expansion_segments then
		for dir_str, seg_list in pairs( room.expansion_segments ) do
			local color, rad
			if self._neg_dir_str_map[ dir_str ] then
				rad = 3.5
				color = Vector3(0.5,0.5,0.5)
			else
				rad = 4
				color = Vector3(1,1,1)
			end
			for i_seg, seg in pairs( seg_list ) do
				Application:draw_cylinder( seg[1], seg[2], rad, color.x, color.y, color.z )
				--print( dir_str.." exp_seg "..tostring(i_seg), seg[1], seg[2] )
			end
		end
	end
	
	if room.neighbours then
		for side, neighbour_list in pairs( room.neighbours ) do
			local color, rad
			if self._neg_dir_str_map[ side ] then
				rad = 3.2
				color = Vector3(0,0.5,0.5)
			else
				rad = 4
				color = Vector3(0,1,1)
			end
			for i_neighbour, neighbour_data in pairs( neighbour_list ) do
				Application:draw_cylinder( neighbour_data.overlap[1], neighbour_data.overlap[2], rad, color.x, color.y, color.z )
				Application:draw_line( my_center, ( neighbour_data.overlap[1] + neighbour_data.overlap[2] ) * 0.5, color.x, color.y, color.z )
				--print( dir_str.." exp_seg "..tostring(i_seg), seg[1], seg[2] )
			end
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_doors( progress )
	local selected_seg = self._selected_segment
	local room_mask
	if selected_seg and self._nav_segments[ selected_seg ] and next( self._nav_segments[ selected_seg ].vis_groups ) then
		room_mask = {}
		for _, i_vis_group in ipairs( self._nav_segments[ selected_seg ].vis_groups ) do
			local vis_group_rooms = self._visibility_groups[ i_vis_group ].rooms
			for i_room, _ in pairs( vis_group_rooms ) do
				room_mask[ i_room ] = true
			end
		end
	end
	local data = self._draw_data
	local doors = self._room_doors
	local nr_doors = #doors
	local i_door = data.next_draw_i_door
	local wanted_index = math.clamp( math.ceil( nr_doors * progress ), 1, nr_doors )
	while wanted_index >= i_door and i_door <= nr_doors do
		local door = doors[ i_door ]
		if not room_mask or room_mask[ door.rooms[ 1 ] ] or room_mask[ door.rooms[ 2 ] ] then
			self:_draw_door( door )
		end
		i_door = i_door + 1
	end
	if progress == 1 then
		data.next_draw_i_door = 1
	else
		data.next_draw_i_door = i_door
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_door( door )
	local brush = self._draw_data.brush.door
	brush:cylinder( door.pos, door.pos1, 2 )
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_anim_nav_links()
	local brush = Draw:brush( Color( 0.2, 0.8, 0.2, 0.1 ) )
	local brush_fwd = Draw:brush( Color( 0.3, 0.1, 0.9, 0.1 ) )
	for i_room, room in ipairs( self._rooms ) do
		if room.nav_links then
			for _, nav_link in ipairs( room.nav_links ) do
				local start_pos = nav_link.element:value( "position" )
				brush:cone( nav_link.element:nav_link_end_pos(), start_pos, 20 )
				if nav_link.element:value( "align_rotation" ) then
					brush_fwd:cylinder( start_pos + nav_link.element:value( "rotation" ):y() * 100, start_pos, 2 )
				end
			end
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_covers()
	local reserved = self.COVER_RESERVED
	for i_cover, cover in ipairs( self._covers ) do
		local draw_pos = cover[1]
		Application:draw_rotation( draw_pos, Rotation( cover[2], math.UP ) )
		if cover[ reserved ] then
			Application:draw_sphere( draw_pos, 18, 0, 0, 0 )
		end
		local tracker = cover[3]
		if tracker:lost() then
			--local field_pos = tracker:field_position()
			local placed_pos = tracker:position()
			Application:draw_sphere( placed_pos, 20, 1, 0, 0 )
			Application:draw_line( placed_pos, draw_pos, 1, 0, 0 )
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:cover_info()
	local reserved = self.COVER_RESERVED
	for i_cover, cover in ipairs( self._covers ) do
		if cover[ reserved ] then
			print( "cover", i_cover, "reserved", cover[ reserved ], "times" )
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_geographic_segments()
	if not next( self._geog_segments ) then
		return
	end
	
	local seg_rad = 3
	local seg_color = Vector3( 0.8, 0.2, 0.1 )
	local room_rad = 2
	local room_color = Vector3( 1, 1, 1 )
	
	for i_seg, segment in pairs( self._geog_segments ) do
		local borders = self:_calculate_geographic_segment_borders( i_seg )
		local height = 300
		local top_right = Vector3( borders.x_pos, borders.y_pos, height )
		local top_left = Vector3( borders.x_neg, borders.y_pos, height )
		local bottom_right = Vector3( borders.x_pos, borders.y_neg, height )
		local bottom_left = Vector3( borders.x_neg, borders.y_neg, height )
		
		Application:draw_cylinder( top_right, top_left, seg_rad, seg_color.x, seg_color.y, seg_color.z )
		Application:draw_cylinder( top_left, bottom_left, seg_rad, seg_color.x, seg_color.y, seg_color.z )
		Application:draw_cylinder( bottom_left, bottom_right, seg_rad, seg_color.x, seg_color.y, seg_color.z )
		Application:draw_cylinder( bottom_right, top_right, seg_rad, seg_color.x, seg_color.y, seg_color.z )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_visibility_groups( progress )
	local selected_seg = self._selected_segment
	if not ( selected_seg and self._nav_segments[ selected_seg ] ) then
		return
	end
	
	local selected_vis_groups = self._nav_segments[ selected_seg ].vis_groups
	local nr_vis_groups = #selected_vis_groups
	if nr_vis_groups == 0 then
		return
	end

	local all_vis_groups = self._visibility_groups
	local all_rooms = self._rooms
	local builder = self._builder
	
	local draw_data = self._draw_data
	local brush_node = draw_data.brush.vis_graph_node
	local brush_rooms = draw_data.brush.vis_graph_rooms
	local brush_links = draw_data.brush.vis_graph_links
	
	local i_vis_group = draw_data.next_draw_i_vis
	local wanted_index = math.clamp( math.floor( nr_vis_groups * progress ), 0, nr_vis_groups )
	
	while wanted_index > 0 and wanted_index >= i_vis_group do
		local vis_group = all_vis_groups[ selected_vis_groups[ i_vis_group ] ]
		--print( "drawing", i_vis_group, "of", nr_vis_groups, Application:time() )
		brush_node:sphere( vis_group.pos, 30 )
		for i_vis_room, _ in pairs( vis_group.rooms ) do
			local room_c = builder:_calculate_room_center( all_rooms[ i_vis_room ] )
			brush_rooms:line( vis_group.pos, room_c )
		end
		for i_neigh_group, _ in pairs( vis_group.vis_groups ) do
			local neigh_group = all_vis_groups[ i_neigh_group ]
			brush_links:cylinder( vis_group.pos, neigh_group.pos, 2 )
			if neigh_group.seg ~= selected_seg then
				brush_links:sphere( neigh_group.pos, 20 )
			end
		end
		i_vis_group = i_vis_group + 1
	end
	if progress == 1 then
		draw_data.next_draw_i_vis = 1
	else
		draw_data.next_draw_i_vis = i_vis_group
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_draw_coarse_graph()
	local all_nav_segments = self._nav_segments
	local all_doors = self._room_doors
	local all_vis_groups = self._visibility_groups
	
	local brush = self._draw_data.brush.coarse_graph
	
	for seg_id, seg_data in pairs( all_nav_segments ) do
		local neighbours = seg_data.neighbours
		for neigh_i_seg, door_list in pairs( neighbours ) do
			brush:cone( all_nav_segments[ neigh_i_seg ].pos, seg_data.pos, 12 )
		end
	end
	
end

-----------------------------------------------------------------------------------

function NavigationManager:get_nav_segments_in_direction( start_nav_seg_id, fwd )
	local mvec3_set = mvector3.set
	local mvec3_dot = mvector3.dot
	local mvec3_sub = mvector3.subtract
	
	local all_nav_segs = self._nav_segments
	local start_nav_seg = all_nav_segs[ start_nav_seg_id ]
	local start_pos = start_nav_seg.pos
	
	local quad_field = self._quad_field
	local to_search = {} -- list of nav segments we should examine & expand
	local discovered = {}	-- list of nav segments we have encountered
	discovered[ start_nav_seg_id ] = true
	local found = {}	-- list of nav segments we have approved
	-- find all first neighbours in the wanted direction
	local search_vec = temp_vec1
	local immediate_neighbours = start_nav_seg.neighbours
	for neighbour_id, _ in pairs( immediate_neighbours ) do
		discovered[ neighbour_id ] = true
		local neighbour_seg = all_nav_segs[ neighbour_id ]
		if not neighbour_seg.disabled then
			-- calculate vector from segment position to start pos
			mvec3_set( search_vec, neighbour_seg.pos )
			mvec3_sub( search_vec, start_pos )
			local neighbour_dot = mvec3_dot( fwd, search_vec )
			if neighbour_dot > 0 then
				table.insert( to_search, neighbour_id )
				found[ neighbour_id ] = true
			end
		end
	end
	
	-- expand the approved first neighbours and their neighbours
	while #to_search ~= 0 do
		local search_nav_seg_id = table.remove( to_search )
		local my_neighbours = all_nav_segs[ search_nav_seg_id ].neighbours
		for neighbour_id, _ in pairs( my_neighbours ) do
			if not discovered[ neighbour_id ] then
				discovered[ neighbour_id ] = true
				if not all_nav_segs[ neighbour_id ].disabled then
					-- calculate vector from segment position to start pos
					table.insert( to_search, neighbour_id )
					found[ neighbour_id ] = true
				end
			end
		end
	end
	
	return next( found ) and found
end

-----------------------------------------------------------------------------------

function NavigationManager:find_random_position_in_segment( seg_id )
	return self._quad_field:random_position_in_nav_segment( seg_id )
end

-----------------------------------------------------------------------------------
--	cover_table = { position, forward, nav_tracker, reserved, pos_reservation_data }
function NavigationManager:register_cover_units()
	if not self:is_data_ready() then
		return
	end
	local rooms = self._rooms
	local covers = {}
	local cover_data = managers.worlddefinition:get_cover_data()
	local t_ins = table.insert
	if cover_data then
		
		local _register_cover = function( pos, fwd )
			local nav_tracker = self._quad_field:create_nav_tracker( pos, true )
			
			local cover = { nav_tracker:field_position(), fwd, nav_tracker }
			if self._debug then
				t_ins( covers, cover )
			end
			
			local location_script_data = self._quad_field:get_script_data( nav_tracker, true ) -- location, allow_create
			if not location_script_data.covers then
				location_script_data.covers = {}
			end
			t_ins( location_script_data.covers, cover )
		end
		
		if cover_data.positions then
			local rotations = cover_data.rotations
			for i, pos in ipairs( cover_data.positions ) do
				local fwd = Rotation( rotations[i] ):y()
				_register_cover( pos, fwd )
			end
		else
			for _, cover_desc in ipairs( cover_data ) do
				local pos = cover_desc[1]
				local fwd = Rotation( cover_desc[2] ):y()
				_register_cover( pos, fwd )
			end
		end
	else
		local all_cover_units = World:find_units_quick( "all", managers.slot:get_mask( "cover" ) )
		for i, unit in ipairs( all_cover_units ) do
			local pos = unit:position()
			local fwd = unit:rotation():y()
			local nav_tracker = self._quad_field:create_nav_tracker( pos, true )
			local cover = { nav_tracker:field_position(), fwd, nav_tracker, true }
			
			if self._debug then
				t_ins( covers, cover )
			end
			local location_script_data = self._quad_field:get_script_data( nav_tracker ) -- location, allow_create
			if not location_script_data.covers then
				location_script_data.covers = {}
			end
			t_ins( location_script_data.covers, cover )
			
			self:_safe_remove_unit( unit )
		end
	end
	self._covers = covers
end

-----------------------------------------------------------------------------------

function NavigationManager:_unregister_cover_units()
	for i_cover, cover in ipairs( self._covers ) do
		self._quad_field:destroy_nav_tracker( cover[3] )
	end
	self._covers = {}
end

-----------------------------------------------------------------------------------

function NavigationManager:_safe_remove_unit( unit )
	if Application:editor() then
		if unit:unit_data().continent then
			unit:unit_data().continent:remove_unit( unit )
		end
	end
	
	unit:set_slot( 0 )
end

-----------------------------------------------------------------------------------

function NavigationManager:remove_AI_blocker_units()
	local all_units = World:find_units_quick( "all", 15 )
	for i, unit in pairs( all_units ) do
		self:_safe_remove_unit( unit )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:register_anim_nav_link( element )
	--print( "[NavigationManager:register_anim_nav_link] id:", element._id )
	if element:nav_link() then
		if Application:editor() then
			managers.editor:output_error( "[NavigationManager:register_anim_nav_link] Navigation link is already registered id: "..tostring(element._id) )
		else
			Application:error( "[NavigationManager:register_anim_nav_link] Navigation link is already registered", element._id )
		end
		return
	end
	
	if self._nav_links then
		self._nav_links[ element ] = true
	end
	
	local nav_link = self._quad_field:add_nav_link( element:value( "position" ), element:nav_link_end_pos(), { element = element }, element:nav_link_access(), element:value( "base_chance" ), element._id )
	
	--print( "nav_link", nav_link )
	element:set_nav_link( nav_link )
	
	local start_nav_seg_id = nav_link:start_nav_segment()
	local end_nav_seg_id = nav_link:end_nav_segment()
	if start_nav_seg_id ~= end_nav_seg_id then
		local start_nav_seg = self._nav_segments[ start_nav_seg_id ]
		local start_nav_seg_neighbours = start_nav_seg.neighbours
		if start_nav_seg_neighbours[ end_nav_seg_id ] then
			table.insert( start_nav_seg_neighbours[ end_nav_seg_id ], nav_link )
		else
			start_nav_seg_neighbours[ end_nav_seg_id ] = { nav_link }
			managers.groupai:state():on_nav_seg_neighbour_state( start_nav_seg_id, end_nav_seg_id, true )
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:unregister_anim_nav_link( element )
	local nav_link = element:nav_link()
	--print( "[NavigationManager:unregister_anim_nav_link] id:", element._id, " nav_link ", nav_link )
	if not nav_link then
		return
	end
	
	if self._nav_links then
		self._nav_links[ element ] = nil
	end
	
	local start_nav_seg_id = nav_link:start_nav_segment()
	local end_nav_seg_id = nav_link:end_nav_segment()
	local start_nav_seg = self._nav_segments[ start_nav_seg_id ]
	local start_nav_seg_neighbours = start_nav_seg.neighbours
	
	if start_nav_seg_neighbours[ end_nav_seg_id ] then
		for i_door, door_id in pairs( start_nav_seg_neighbours[ end_nav_seg_id ] ) do -- remove the nav link from nav seg neighbours
			if door_id == nav_link then
				if #start_nav_seg_neighbours[ end_nav_seg_id ] == 1 then
					start_nav_seg_neighbours[ end_nav_seg_id ] = nil
					managers.groupai:state():on_nav_seg_neighbour_state( start_nav_seg_id, end_nav_seg_id, false )
				else
					table.remove( start_nav_seg_neighbours[ end_nav_seg_id ], i_door )
				end
				break
			end
		end
	end
	
	managers.groupai:state():on_nav_link_unregistered( element._id )
	
	element:set_nav_link()
	self._quad_field:remove_nav_link( element._id )
end

-----------------------------------------------------------------------------------

function NavigationManager:reserve_cover( cover, filter )
	local reserved = cover[ self.COVER_RESERVED ]
	if reserved then
		cover[ self.COVER_RESERVED ] = reserved + 1
	else
		cover[ self.COVER_RESERVED ] = 1
		local reservation = { position = cover[1], radius = 60, filter = filter }
		cover[ self.COVER_RESERVATION ] = reservation
		self:add_pos_reservation( reservation )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:release_cover( cover )
	local reserved = cover[ self.COVER_RESERVED ]
	if reserved == 1 then
		cover[ self.COVER_RESERVED ] = nil
		self:unreserve_pos( cover[ self.COVER_RESERVATION ] )
	else
		cover[ self.COVER_RESERVED ] = reserved - 1
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_near_pos_1( near_pos, threat_pos, max_near_dis, min_threat_dis, allow_fwd )
	local search_params = {
		near_pos = near_pos,
		threat_pos = threat_pos,
		forbid_fwd = not allow_fwd,
		max_distance = max_near_dis,
		min_threat_distance = min_threat_dis,
		variation_z = 250
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_away_from_pos( near_pos, threat_pos, nav_seg_id )
	if type( nav_seg_id ) == "table" then
		nav_seg_id = self._convert_nav_seg_map_to_vec( nav_seg_id )
	end
	
	local search_params = {
		near_pos = near_pos,
		threat_pos = threat_pos,
		forbid_fwd = true,
		in_nav_seg = nav_seg_id,
		optimal_threat_dis = mvec3_dis( near_pos, threat_pos ) + 100
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager._convert_nav_seg_map_to_vec( nav_seg_map )
	local nav_seg_vec = {}
	for nav_seg, _ in pairs( nav_seg_map ) do
		table.insert( nav_seg_vec, nav_seg )
	end
	return nav_seg_vec
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_in_nav_seg_1( nav_seg_id )
	if type( nav_seg_id ) == "table" then
		nav_seg_id = self._convert_nav_seg_map_to_vec( nav_seg_id )
	end
	
	local search_params = {
		in_nav_seg = nav_seg_id
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_in_nav_seg_2( nav_seg_id, defend_pos, defend_dir )
	if type( nav_seg_id ) == "table" then
		nav_seg_id = self._convert_nav_seg_map_to_vec( nav_seg_id )
	end
	
	local search_params = {
		near_pos = defend_pos,
		in_nav_seg = nav_seg_id,
		threat_dir = defend_dir
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_in_nav_seg_3( nav_seg_id, max_near_dis, near_pos, threat_pos )
	if type( nav_seg_id ) == "table" then
		nav_seg_id = self._convert_nav_seg_map_to_vec( nav_seg_id )
	end
	
	local search_params = {
		near_pos = near_pos,
		threat_pos = threat_pos,
		in_nav_seg = nav_seg_id,
		max_distance = max_near_dis
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_from_threat( nav_seg_id, optimal_threat_dis, near_pos, threat_pos )
	if type( nav_seg_id ) == "table" then
		nav_seg_id = self._convert_nav_seg_map_to_vec( nav_seg_id )
	end
	
	local search_params = {
		near_pos = near_pos,
		threat_pos = threat_pos,
		in_nav_seg = nav_seg_id,
		optimal_threat_dis = optimal_threat_dis
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager:find_cover_in_cone_from_threat_pos_1( threat_pos, furthest_pos, near_pos, search_from_pos, angle, min_dis, nav_seg, optimal_threat_dis, rsrv_filter )
	if type( nav_seg ) == "table" then
		nav_seg = self._convert_nav_seg_map_to_vec( nav_seg )
	end
	
	local search_params = {
		near_pos = near_pos,
		threat_pos = threat_pos,
		search_start_pos = search_from_pos,
		min_threat_distance = min_dis,
		cone_angle = angle,
		cone_base = furthest_pos,
		variation_z = 250,
		in_nav_seg = nav_seg,
		optimal_threat_dis = optimal_threat_dis,
		rsrv_filter = rsrv_filter 
	}
	return self._quad_field:find_cover( search_params )
end

-----------------------------------------------------------------------------------

function NavigationManager:find_walls_accross_tracker( from_tracker, accross_vec, angle, nr_rays )
	angle = angle or 180
	local center_pos = from_tracker:field_position()
	
	nr_rays = math.max( 2, nr_rays or 4 )
	local rot_step = angle / ( nr_rays - 1 )
	local rot_offset = ( math.random() * 2 - 1 ) * angle * 0.5
	local ray_rot = Rotation( -angle * 0.5 + rot_offset - rot_step )
	local vec_to = Vector3( accross_vec.x, accross_vec.y )
	mvec3_rot( vec_to, ray_rot )
	local pos_to = Vector3()
	
	mrotation.set_yaw_pitch_roll( ray_rot, rot_step, 0, 0 )
	local tracker_from, pos_from
	if from_tracker:lost() then
		pos_from = center_pos
	else
		tracker_from = from_tracker
	end
	
	local ray_params = {
		tracker_from = tracker_from,
		pos_from = pos_from,
		pos_to = pos_to,
		trace = true
	}
	
	local ray_results = {}
	
	local i_ray = 1
	while i_ray <= nr_rays do
		mvec3_rot( vec_to, ray_rot )
		mvec3_set( pos_to, vec_to )
		mvec3_add( pos_to, center_pos )
		local hit = self:raycast( ray_params )
		if hit then
			table.insert( ray_results, { ray_params.trace[1], true } )
		else
			table.insert( ray_results, { ray_params.trace[1] } )
		end
		i_ray = i_ray + 1
	end
	
	return #ray_results > 0 and ray_results
end

-----------------------------------------------------------------------------------

function NavigationManager:find_segment_doors( from_seg_id, approve_clbk )
	local all_doors = self._room_doors
	local all_nav_segs = self._nav_segments
	local from_seg = all_nav_segs[ from_seg_id ]
	local found_doors = {}
	
	for neighbour_seg_id, door_list in pairs( from_seg.neighbours ) do
		if not all_nav_segs[ neighbour_seg_id ].disabled and ( not approve_clbk or approve_clbk( neighbour_seg_id ) ) then
			for _, i_door in ipairs( door_list ) do
				if type( i_door ) == "number" then
					table.insert( found_doors,  all_doors[ i_door ] )
				end
			end
		end
	end

	--[[local duration = 3
	local brush_door = Draw:brush( Color( 0.2, 1, 0, 0 ), duration )
	print( "NavigationManager:_find_segment_doors() found_doors:", #found_doors, "searched:", table.size( searched ) )
	for i, door in ipairs( found_doors ) do
		brush_door:sphere( door.center, 40 )
	end]]
	
	return found_doors 
	
	
	--[[local segment_neighbours = self._quad_field:nav_segment_neighbours( from_seg_id, false )
	
	if approve_clbk then
		local i_neighbour = #segment_neighbours
		while i_neighbour > 0 do
			i_neighbour = i_neighbour - 1
			if not approve_clbk( segment_neighbours[ i_neighbour ] ) then
				segment_neighbours[ i_neighbour ] = segment_neighbours[ #segment_neighbours ]
				table.remove( segment_neighbours )
			end
		end
	end
	
	local segment_doors = self._quad_field:nav_segment_doors( from_seg_id, segment_neighbours )
	return segment_doors]]
end









































-----------------------------------------------------------------------------------

function NavigationManager:_commence_coarce_searches( t )
	local search_data = self._coarse_searches[ 1 ]
	if not search_data then
		return
	end
	
	table.remove( self._coarse_searches, 1 )
	
	local result = self:_execute_coarce_search( search_data )
	search_data.results_callback( result )
end

-----------------------------------------------------------------------------------

function NavigationManager:_execute_coarce_search( search_data )
	local search_id = search_data.id
	
	local i = 0
	while true do
		if i == 500 then
			debug_pause( "[NavigationManager:_execute_coarce_search] endless loop", inspect( search_data ) )
			return false
		else
			i = i + 1
		end
		--Application:set_pause( true )
		-- Get the vis_group at the top of the search list
		local next_search_seg = search_data.seg_to_search[ #search_data.seg_to_search ]
		local next_search_i_seg = next_search_seg.i_seg
		
		table.remove( search_data.seg_to_search )
		
		local all_nav_segments = self._nav_segments
		--[[for seg_id, _ in pairs( search_data.seg_searched ) do
			Application:draw_sphere( all_nav_segments[ seg_id ].pos, 90, 0,1,0 )
		end]]
		local neighbours = all_nav_segments[ next_search_i_seg ].neighbours
		--print( "next_search_i_seg", next_search_i_seg, "neighbours", inspect( neighbours ) )
		if neighbours[ search_data.end_i_seg ] then	-- Check if we have reached the destination group
			local entry_found
			for _, i_door in ipairs( neighbours[ search_data.end_i_seg ] ) do
				if type( i_door ) == "number" then
					entry_found = true
					break
				elseif TimerManager:game():time() > i_door:delay_time() and i_door:check_access( search_data.access_pos, search_data.access_neg ) then	-- nav link
					entry_found = true
					break
				end
			end
			if entry_found then
				--print( "Destination found amongst neighbours" )
				local i_seg = next_search_i_seg
				local this_seg = next_search_seg
				local prev_seg = search_data.end_i_seg
				local path = { { search_data.end_i_seg, search_data.to_pos } }
				--print( "added to path", search_data.end_i_seg )
				table.insert( path, 1, { next_search_i_seg, next_search_seg.pos } )
				--print( "added to path", next_search_i_seg )
				local searched = search_data.seg_searched
				while this_seg.from do
					--	Find out what the entry position of the last group was
					i_seg = this_seg.from
					this_seg = searched[ i_seg ]
					table.insert( path, 1, { i_seg, this_seg.pos } )
					--print( "added to path", i_seg )
				end
				return path
			end
		end
		
		--print( "Expanding neighbours" )
		local to_pos = search_data.to_pos
		--Application:draw_sphere( to_pos, 100, 1, 0, 0 )
		local new_segments = self:_sort_nav_segs_after_pos( to_pos, next_search_i_seg, search_data.discovered_seg, search_data.verify_clbk, search_data.access_pos, search_data.access_neg )
	
		--	add groups to my prio list and sort
		if new_segments then
			local to_search = search_data.seg_to_search
			for i_seg, seg_data in pairs( new_segments ) do
				--print( "found segment", inspect( seg_data ) )
				local new_seg_weight = seg_data.weight
				local search_index = #to_search
				while search_index > 0 and to_search[ search_index ].weight < new_seg_weight do	--	Sorted from least to most important
					search_index = search_index - 1
				end
				table.insert( to_search, search_index + 1, seg_data )
			end
		end
		
		--[[print( "to search:" )
		for _, seg_data in ipairs( search_data.seg_to_search ) do
			print( seg_data.i_seg )
			Application:draw_sphere( all_nav_segments[ seg_data.i_seg ].pos, 80, 0,1,1 )
		end]]
	
		local nr_seg_to_search = #search_data.seg_to_search
		if nr_seg_to_search == 0 then
			--cat_print( "george", "!		exhausted field without finding the destination nav_segment!" )
			return false
		else
			search_data.seg_searched[ next_search_i_seg ] = next_search_seg
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_sort_nav_segs_after_pos( to_pos, i_seg, ignore_seg, verify_clbk, access_pos, access_neg )
	local all_segs = self._nav_segments
	local all_doors = self._room_doors
	local all_rooms = self._rooms
	
	local seg = all_segs[ i_seg ]
	local neighbours = seg.neighbours
	local found_segs
	for neighbour_seg_id, door_list in pairs( neighbours ) do
		if not ( ignore_seg[ neighbour_seg_id ] or all_segs[ neighbour_seg_id ].disabled ) and ( not verify_clbk or verify_clbk( neighbour_seg_id ) ) then
			for _, i_door in ipairs( door_list ) do
				if type( i_door ) == "number" then
					local door = all_doors[ i_door ]
					--print( "checking door", i_door )
					local door_pos = door.center
					local weight = mvec3_dis( door_pos, to_pos )
					if found_segs then
						if found_segs[ neighbour_seg_id ] then
							if found_segs[ neighbour_seg_id ].weight > weight then
								--print( "overwriting previous entry pos" )
								found_segs[ neighbour_seg_id ] = { weight=weight, from=i_seg, i_seg = neighbour_seg_id, pos = door_pos }
							end
						else
							--print( "adding neighbour to found list" )
							found_segs[ neighbour_seg_id ] = { weight=weight, from=i_seg, i_seg = neighbour_seg_id, pos = door_pos }
							ignore_seg[ neighbour_seg_id ] = true
						end
					else
						--print( "first found neighbour" )
						found_segs = {}
						found_segs[ neighbour_seg_id ] = { weight=weight, from=i_seg, i_seg = neighbour_seg_id, pos = door_pos }
						ignore_seg[ neighbour_seg_id ] = true
					end
				else	-- nav link
					if TimerManager:game():time() > i_door:delay_time() and i_door:check_access( access_pos, access_neg ) then
						local end_pos = i_door:script_data().element:nav_link_end_pos()
						local my_weight = mvec3_dis( end_pos, to_pos )
						if found_segs then
							if found_segs[ neighbour_seg_id ] then
								if found_segs[ neighbour_seg_id ].weight > my_weight then
									--print( "overwriting previous entry pos" )
									found_segs[ neighbour_seg_id ] = { weight=my_weight, from=i_seg, i_seg = neighbour_seg_id, pos = end_pos }
								end
							else
								--print( "adding neighbour to found list" )
								found_segs[ neighbour_seg_id ] = { weight=my_weight, from=i_seg, i_seg = neighbour_seg_id, pos = end_pos }
								ignore_seg[ neighbour_seg_id ] = true
							end
						else
							--print( "first found neighbour" )
							found_segs = {}
							found_segs[ neighbour_seg_id ] = { weight=my_weight, from=i_seg, i_seg = neighbour_seg_id, pos = end_pos }
							ignore_seg[ neighbour_seg_id ] = true
						end
					--else
						--print( "neighbour unsuitable nav_link", TimerManager:game():time() > i_door:delay_time(), i_door:check_access( access_pos, access_neg ), access_pos, inspect( self:convert_access_filter_to_table( access_pos ) ), inspect( self:convert_access_filter_to_table( i_door:script_data().element:nav_link_access() ) ) )
					end
				end
			end
		--else
			--print( "neighbour unsuitable", ignore_seg[ neighbour_seg_id ], all_segs[ neighbour_seg_id ].disabled, not verify_clbk or verify_clbk( neighbour_seg_id ) )
		end
	end

	return found_segs
end

-----------------------------------------------------------------------------------
--params = { pos_from = Vector3(), tracker_from = tracker, pos_to = Vector3(), tracker_to = tracker, allow_entry = bool, trace = bool }
function NavigationManager:raycast( params )
	local res = self._quad_field:test_walkability( params )
	return res
end

-----------------------------------------------------------------------------------

--	Determine if a given position is inside a room
function NavigationManager._is_pos_in_room( pos, borders, height )
	--return math.abs( center.z - pos.z ) <= self._room_height and pos.x >= borders.x_neg and pos.x <= borders.x_pos and pos.y >= borders.y_neg and pos.y <= borders.y_pos
	if pos.x >= borders.x_neg and pos.x <= borders.x_pos and pos.y >= borders.y_neg and pos.y <= borders.y_pos then
		local z = NavFieldBuilder._get_room_height_at_pos( height, borders, pos )
		--Application:draw_cylinder( pos, pos:with_z( z ), 5, 1,0,0)
		--Application:draw_sphere( pos, 8, 1,0,0)
		if math.abs( z - pos.z ) < 100 then
			return true
		end
	end
end

-----------------------------------------------------------------------------------

--	Determine if a given position is inside a room
function NavigationManager._is_pos_in_room_xy( pos, borders )
	--return math.abs( center.z - pos.z ) <= self._room_height and pos.x >= borders.x_neg and pos.x <= borders.x_pos and pos.y >= borders.y_neg and pos.y <= borders.y_pos
	if pos.x >= borders.x_neg and pos.x <= borders.x_pos and pos.y >= borders.y_neg and pos.y <= borders.y_pos then
		return true
	end
end

-----------------------------------------------------------------------------------
--	params = { pos_from = Vector3(),	from_tracker = tracker, from_room = i_room, pos_to = Vector3(), to_tracker = tracker, 
--						to_room = i_room, access = { "walk"... }, results_clbk_obj = table, results_clbk_func = string, id = string, prio = number }
-- Highest prio # goes first
function NavigationManager:search_pos_to_pos( params )
	--[[self._debug_searches = self._debug_searches or {}
	if not params.id then
		debug_pause( "[NavigationManager:search_pos_to_pos] no id:", params.id )
	end
	if self._debug_searches[ params.id ] then
		debug_pause( "[NavigationManager:search_pos_to_pos] duplicate search:", params.id )
		return
	end
	self._debug_searches[ params.id ] = params.result_clbk
	params.result_clbk = callback( self, self, "detailed_pathing_results_clbk", params.id )]]
	self._quad_field:detailed_search( params )
end

-----------------------------------------------------------------------------------
--[[
function NavigationManager:detailed_pathing_results_clbk( search_id, path )
	self._debug_searches = self._debug_searches or {}
	
	local entry = self._debug_searches[ search_id ]
	if not entry then
		debug_pause( "[NavigationManager:detailed_pathing_results_clbk] search was not started. id:", search_id )
		return
	end
	entry( path )
	self._debug_searches[ search_id ] = nil
end
]]
-----------------------------------------------------------------------------------
--	params = { from_pos = Vector3(), from_tracker = nav_tracker, to_pos = Vector3(), to_tracker = nav_tracker, 
--						access = { "walk"... }, results_clbk = callback(), id = string }
function NavigationManager:search_coarse( params )
	local pos_to, start_i_seg, end_i_seg, access_pos, access_neg
	if params.from_seg then
		start_i_seg = params.from_seg
	elseif params.from_tracker then
		start_i_seg = params.from_tracker:nav_segment()
	end
	
	if params.to_seg then
		end_i_seg = params.to_seg
	elseif params.to_tracker then
		end_i_seg = params.to_tracker:nav_segment()
	end
	
	pos_to = params.to_pos or self._nav_segments[ end_i_seg ].pos

	if start_i_seg == end_i_seg then
		if params.results_clbk then
			params.results_clbk( { { start_i_seg }, { end_i_seg, mvec3_cpy( pos_to ) } } )
			return
		else
			return { { start_i_seg }, { end_i_seg, mvec3_cpy( pos_to ) } }
		end
	end
	
	access_pos = self._quad_field:convert_nav_link_flag_to_bitmask( params.access_pos )
	if params.access_neg then
		access_neg = self._quad_field:convert_nav_link_flag_to_bitmask( params.access_neg )
	else
		access_neg = 0
	end
	local new_search_data = {
		id = params.id,
		to_pos = mvec3_cpy( pos_to ),
		start_i_seg = start_i_seg,
		end_i_seg = end_i_seg,
		seg_searched = {},
		discovered_seg = { [ start_i_seg ] = true },
		seg_to_search = { { i_seg = start_i_seg } },
		results_callback = params.results_clbk,
		verify_clbk = params.verify_clbk,
		access_pos = access_pos,
		access_neg = access_neg
	}
	
	if params.results_clbk then
		table.insert( self._coarse_searches, new_search_data )
	else
		local result = self:_execute_coarce_search( new_search_data )
		return result
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:cancel_pathing_search( search_id )
	--[[self._debug_searches = self._debug_searches or {}
	if not self._debug_searches[ search_id ] then
		debug_pause( "[NavigationManager:cancel_pathing_search] search was not started. ID:", search_id )
		return
	end
	self._debug_searches[ search_id ] = nil]]
	self._quad_field:abort_detailed_search( search_id )
end

-----------------------------------------------------------------------------------

function NavigationManager:cancel_coarse_search( search_id )
	for i, search_data in ipairs( self._coarse_searches ) do
		if search_id == search_data.id then
			table.remove( self._coarse_searches, i )
			return
		end
	end
end

-----------------------------------------------------------------------------------
--[[
function NavigationManager:test_search()
	print( " " )
	print( "---------------------------------------------------------------" )
	print( "[NavigationManager:test_search]" )
	print( "---------------------------------------------------------------" )
	print( " " )
	
	Application:set_pause( true )
	
	
	local params = {}
	params.pos_from = Vector3(1571.45, 6667.5, 5.0001)
	params.pos_to = Vector3(430.943, 1634.18, 5.00009)
	params.id = "test_random"
	params.result_clbk = callback( self, self, "_test_search_res_clbk", params.id )
	params.nav_segs = { 6, 9, 5 }
	
	self._test_path = nil
	self:search_pos_to_pos( params )
	
end

-----------------------------------------------------------------------------------

function NavigationManager:_test_search_res_clbk( search_id, path )
	print( "[NavigationManager:_test_search_res_clbk]", search_id, inspect( path ) )
	self._test_path = path
end
]]
-----------------------------------------------------------------------------------
--[[
function NavigationManager:random_coarse_search()
	print( " " )
	print( "---------------------------------------------------------------" )
	print( "NavigationManager:random_coarse_search()" )
	print( "---------------------------------------------------------------" )
	print( " " )
	local nr_segs = 0
	local segs_indexed = {}
	for id, seg_data in pairs( self._nav_segments ) do
		nr_segs = nr_segs + 1
		table.insert( segs_indexed, id )
	end
	local lycky_index = math.random( #segs_indexed )
	local from_seg = segs_indexed[ lycky_index ]
	table.remove( segs_indexed, lycky_index )
	
	lycky_index = math.random( nr_segs - 1 )
	local to_seg = segs_indexed[ lycky_index ]
	
	local params = {}
	params.from_seg = from_seg
	params.to_seg = to_seg
	params.access_pos = "teamAI1"
	params.results_clbk = callback( self, self, "_test_coarse_res_clbk" )
	params.id = "test_random"
	print( "from_seg", params.from_seg, "to_seg", params.to_seg )
	self:search_coarse( params )
end

-----------------------------------------------------------------------------------

function NavigationManager:_test_coarse_res_clbk( path )
	if path then
		print( "pathing successful!!!" )
		local pos_path = {}
		for i, nav_point in ipairs( path ) do
			print( nav_point[2] )
			table.insert( pos_path, nav_point[2] )
		end
		self:draw_path( pos_path )
		Application:set_pause( true )
		for i,k in pairs( path ) do
			print( i, unpack( k ) )
		end
	else
		debug_pause( "pathing failed!!!" )
	end
	
end]]

-----------------------------------------------------------------------------------

function NavigationManager:print_rect_info()
	local camera = setup:freeflight()._camera_object
	--local camera = managers.viewport:get_current_camera()
	local cam_pos = camera:position()
	local cam_fwd = camera:rotation():y() * 20000
	local cam_look_ray = World:raycast( "ray", cam_pos, cam_pos + cam_fwd )
	if cam_look_ray then
		local look_pos = cam_look_ray.position
		local nav_tracker = self._quad_field:create_nav_tracker( look_pos, true )
		local nav_seg_id = nav_tracker:nav_segment()
		print( "nav_segment:", nav_seg_id, self._nav_segments[ nav_seg_id ].disabled and "disabled" )
		self._quad_field:destroy_nav_tracker( nav_tracker )
		if managers.groupai:state()._area_data then
			local areas_text = ""
			for area_id, area in pairs( managers.groupai:state()._area_data ) do
				if area.nav_segs[ nav_seg_id ] then
					areas_text = areas_text..tostring(area_id).." "
				end
			end
			print( "areas: "..areas_text )
		end 
	else
		print( "camera ray missed" )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:draw_path( path, color_link, color_node, duration )
	if path then
		color_node = color_node and Color( unpack( color_node ) ) or Color( 0.2, math.random(), math.random(), math.random() )
		color_link = color_link and Color( unpack( color_link ) ) or Color( 0.1, math.random(), math.random(), math.random() )
		local brush_node = Draw:brush( color_node, duration )
		local brush_link = Draw:brush( color_link, duration )
		brush_node:sphere( path[1], 15 )
		for i = 2, #path do
			if path[ i ].x then
				brush_node:sphere( path[ i ], 8 )
				if path[ i - 1 ].x then
					brush_link:cylinder( path[ i ], path[ i - 1 ], 5 )
				else
					brush_link:cylinder( path[ i ], CopActionWalk._nav_point_pos( path[ i - 1 ] ), 5 )
				end
			else
				local start_pos = CopActionWalk._nav_point_pos( path[ i ] )
				local end_pos = start_pos + path[ i ].element:value( "rotation" ):y() * 100
				brush_node:sphere( start_pos, 8 )
				brush_node:sphere( end_pos, 8 )
				brush_link:cone( end_pos, start_pos, 30 )
				if path[ i - 1 ].x then
					brush_link:cylinder( start_pos, path[ i - 1 ], 5 )
				else
					brush_link:cylinder( start_pos, CopActionWalk._nav_point_pos( path[ i - 1 ] ), 5 )
				end
			end
		end
	end
end

























-----------------------------------------------------------------------------------

function NavigationManager:create_nav_tracker( start_pos, allow_disabled )
	return self._quad_field:create_nav_tracker( start_pos, allow_disabled )
end

-----------------------------------------------------------------------------------

function NavigationManager:destroy_nav_tracker( nav_tracker )
	if alive( nav_tracker ) then
		self._quad_field:destroy_nav_tracker( nav_tracker )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:get_nav_seg_from_i_room( i_room )
	return self._visibility_groups[ self._rooms[ i_room ].vis_group ].seg
end

-----------------------------------------------------------------------------------

function NavigationManager:get_nav_seg_from_i_vis_group( i_group )
	return self._visibility_groups[ i_group ].seg
end

-----------------------------------------------------------------------------------

function NavigationManager:get_nav_seg_from_pos( pos, allow_disabled )
	return self._quad_field:find_nav_segment( pos, allow_disabled )
end

-----------------------------------------------------------------------------------

function NavigationManager:get_nav_seg_neighbours( seg_id )
	return self._nav_segments[ seg_id ].neighbours
end

-----------------------------------------------------------------------------------

function NavigationManager:on_game_started()
	self:register_cover_units()
	self:remove_AI_blocker_units()
end

-----------------------------------------------------------------------------------

function NavigationManager:on_simulation_started()
	if self:is_data_ready() then
		self:register_cover_units()
	end
	self:remove_AI_blocker_units()
end

-----------------------------------------------------------------------------------
--[[
function NavigationManager:test_step_clbk( data, test_pos )
	print( "test_verify_clbk", inspect( data ), test_pos )
	
	local step_vec = data.step_vec
	local step_mul = data.step_mul
	
	mvector3.set( test_pos, step_vec )
	mvector3.multiply( test_pos, step_mul )
	mvector3.add( test_pos, data.start_pos )
	
	local params = { pos_from = data.start_pos,
										pos_to = test_pos,
										allow_entry = false	
									}

	local blocked = self:raycast( params )
	if blocked then	-- we must stop testing in this direction
		Application:draw_cylinder( test_pos, blocked, 3, 0.3,0,0 )
	else
		params.pos_from = test_pos
		params.pos_to = data.fwd_pos
		blocked = self:raycast( params )
		if blocked then
			Application:draw_cylinder( test_pos, blocked, 3, 1,0,0 )
		else
			params.pos_to = data.bwd_pos
			blocked = self:raycast( params )
			if blocked then
				Application:draw_cylinder( test_pos, blocked, 3, 0.5,0,0 )
			end
		end
	end
	
	if blocked then
		print( "blocked" )
		if data.block then
			return false
		end
		data.block = true
		if step_mul > 0 then
			data.step_mul = -step_mul
		else
			data.step_mul = -step_mul + 1
		end
		return self:test_step_clbk( data, test_pos )
	elseif data.block then
		data.step_mul = step_mul + math.sign( step_mul )
	else
		if step_mul > 0 then
			data.step_mul = -step_mul
		else
			data.step_mul = -step_mul + 1
		end
	end
	return true
end

-----------------------------------------------------------------------------------

function NavigationManager:test_reserve_nav_pos()
	local player = managers.player:player_unit()
	local pos = player:position()
	local fwd = player:camera():forward():with_z( 0 ):normalized()
	local right = fwd:cross( math.UP ):normalized()
	local step_vec = right * 65
	local fwd_pos = pos + fwd * 1000
	local params = {}
			params.pos_from = pos
			params.pos_to = fwd_pos
			params.allow_entry = false
	local hit_pos = self:raycast( params )
	if hit_pos then
		fwd_pos = hit_pos
	end
	local bwd_pos = pos - fwd * 1000
	params.pos_to = bwd_pos
	hit_pos = self:raycast( params )
	if hit_pos then
		bwd_pos = hit_pos
	end
	local data = { start_pos = pos,
								fwd_pos = fwd_pos, 
								bwd_pos = bwd_pos, 
								step_vec = step_vec,
								step_mul = 1
								}
								
	local step_clbk = callback( self, NavigationManager, "test_step_clbk", data )
	
	self:reserve_pos( TimerManager:game():time(), 100, pos, step_clbk )
end]]

-----------------------------------------------------------------------------------
--[[
 How to reserve a world position: 
 start_t: the game time where your reservation starts taking effect
 duration: how long after start_t the reservation expires. 
 						nil and false means the reservation applies until cancelled with unreserve_pos( entry )
 pos: the world position you want to reserve. can be mutable
 step_clbk: a callback object that is called in case a reservation attempt fails.
 						to designate a new reservation attempt position mutate the Vector3() parameter supplied ( that previous attempt pos )
 						return false/nil if you want to terminate the search.
 						if step_clbk is not supplied and the first attempt fails reserve_pos() returns nil
 On success, reserve_pos() reserves a position and returns that position and an entry table that contains the reservation data.
 
 How to unreserve a world position: 
 call unreserve_pos() with the return value of reserve_pos() as a parameter
]]

function NavigationManager:reserve_pos( start_t, duration, pos, step_clbk, radius, filter )
	local entry = {
		position = mvec3_cpy( pos ),
		radius = radius,
		start_t = start_t,
		expire_t = start_t and duration and start_t + duration,
		filter = filter
	}
	
	local quad_field = self._quad_field
	local free_chk_func = quad_field.is_position_unreserved
	local search_pos = entry.position
	
	local found_pos
	repeat
		--print( "search_pos", search_pos )
		if free_chk_func( quad_field, entry ) then
			--Application:draw_sphere( search_pos, 30, 0, 0, 1 )
			--print( "position approved" )
			self:add_pos_reservation( entry )
			
			return entry
		elseif not step_clbk or not step_clbk( search_pos ) then
			--Application:draw_sphere( search_pos, 30, 1, 0, 0 )
			return
		end
	until nil
end

-----------------------------------------------------------------------------------

function NavigationManager:is_pos_free( desc )
	return self._quad_field:is_position_unreserved( desc )
end

-----------------------------------------------------------------------------------

function NavigationManager:add_pos_reservation( desc )
	if self._debug and not desc.filter then
		print( "[NavigationManager:add_pos_reservation] No filter added" )
		Application:stack_dump()
	end

	--print( "[add_pos_reservation]", desc, inspect( desc ) )
	--Application:stack_dump()
	desc.id = self._quad_field:add_position_reservation( desc )
	
	
	
	if self._debug then
		self._pos_reservations[desc.id] = { desc }
		
		if desc.filter then
			for u_key, u_data in pairs( managers.enemy:all_enemies() ) do
				if u_data.unit:movement():pos_rsrv_id() == desc.filter then
					self._pos_reservations[desc.id].unit = u_data.unit
					return
				end	
			end
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:unreserve_pos( entry )
	--print( "[unreserve_pos]", entry, inspect( entry ) )
	--Application:stack_dump()
	
	if self._debug then
		self._pos_reservations[entry.id] = nil
	end
	
	self._quad_field:remove_position_reservation( entry.id )
	entry.id = nil
end

-----------------------------------------------------------------------------------

function NavigationManager:move_pos_rsrv( desc )
	if self._debug then
		self._pos_reservations[desc.id].position = desc.position
	end
	
	self._quad_field:move_position_reservation( desc.id, desc.position )
end

-----------------------------------------------------------------------------------

function NavigationManager:on_simulation_ended()
	if self._nav_links then
		local nav_links = clone( self._nav_links )
		for element, _ in pairs( nav_links ) do
			self:unregister_anim_nav_link( element )
		end
	end
	-- remove nav link data
	for nav_seg_id, nav_seg in pairs( self._nav_segments ) do
		for neighbour_nav_seg_id, door_list in pairs( nav_seg.neighbours ) do
			for i = #door_list, 1, -1 do
				if type( door_list[ i ] ) ~= "number" then
					table.remove( door_list, i )
				end
			end
			if not next( door_list ) then
				nav_seg.neighbours[ neighbour_nav_seg_id ] = nil
			end
		end
	end
	
	self:_unregister_cover_units()
	
	local rooms = self._rooms
	local i_room = #rooms
	while i_room > 0 do
		local room = rooms[ i_room ]
		if room.added then
			table.remove( rooms, i_room )
		else
			room.removed = nil
			room.covers = nil
			--room.nav_links = nil
		end
		i_room = i_room - 1
	end
	
	for i, obs_data in ipairs( self._obstacles ) do
		self._quad_field:remove_obstacle( obs_data.id )
	end
	self._obstacles = {}
end

-----------------------------------------------------------------------------------

function NavigationManager:nav_field_sanity_check()
	print( "[NavigationManager:nav_field_sanity_check]" )
	local all_rooms = self._rooms
	local all_vis_groups = self._visibility_groups
	local all_segments = self._nav_segments
	
	for i_room, room in ipairs( all_rooms ) do
		local i_vis_group = room.vis_group
		local vis_group = all_vis_groups[ i_vis_group ]
		if not vis_group then
			print( "room", i_room, "belongs to inexistent vis_group", i_vis_group )
		end
	end
	
	for i_vis_group, vis_group in ipairs( all_vis_groups ) do
		local rooms = vis_group.rooms
		for i_room, _ in pairs( rooms ) do
			local room = all_rooms[ i_room ]
			if not room then
				print( "vis_group", i_vis_group, "owns inexistent room", i_room )
			elseif room.vis_group ~= i_vis_group then
				print( "vis_group", i_vis_group, "owns room", i_room, "that points to vis_group", room.vis_group )
			end
		end
		local nav_seg_id = vis_group.seg
		local nav_seg = all_segments[ nav_seg_id ]
		if not nav_seg then
			print( "vis_group", i_vis_group, "belongs to inexistent nav_seg", nav_seg_id )
		end
	end
	
	for nav_seg_id, nav_seg in pairs( all_segments ) do
		local neighbours = nav_seg.neighbours
		for neighbour_nav_seg_id, door_list in pairs( neighbours ) do
			local neighbour_nav_seg = all_segments[ neighbour_nav_seg_id ]
			if neighbour_nav_seg then
				local reverse_check
				for _, door_id in ipairs( door_list ) do -- iterate all doors and exclude nav_links ( they are one-directional. nothing to verify )
					if type( door_id ) == "number" then
						reverse_check = false
						local neighbour_door_list = neighbour_nav_seg.neighbours[ nav_seg_id ]
						if neighbour_door_list then
							for _, neighbour_door_id in ipairs( neighbour_door_list ) do
								if type( neighbour_door_id ) == "number" then
									reverse_check = true
									break
								end
							end
						end
					end
					if reverse_check then
						break 
					end
				end
				if reverse_check == false then
					print( "nav_segment", nav_seg_id, "has neighbour nav_segment", neighbour_nav_seg_id, "but not vice versa", inspect( neighbours ), inspect( neighbour_nav_seg.neighbours ) )
				end
			else
				print( "nav_segment", nav_seg_id, "has inexistent neighbour nav_segment ", neighbour_nav_seg_id )
			end
		end
	end
	
	print( "nav_field_sanity_check complete" )
end

-----------------------------------------------------------------------------------

function NavigationManager:send_nav_field_to_engine()
	local t_ins = table.insert
	local send_data = {}
	send_data.rooms = self._rooms
	send_data.doors = self._room_doors
	send_data.nav_segments = self._nav_segments
	
	send_data.quad_grid_size = self._grid_size
	send_data.sector_grid_offset = self._geog_segment_offset
	send_data.sector_grid_size = self._geog_segment_size
	send_data.sector_max_x = self._nr_geog_segments.x
	send_data.sector_max_y = self._nr_geog_segments.y
	
	local vis_groups = {}
	send_data.visibility_groups = vis_groups
	for i_vis_group, vis_group in ipairs( self._visibility_groups ) do
		local new_vis_group = { seg = vis_group.seg }
		local rooms = {}
		for i_room, _ in pairs( vis_group.rooms ) do
			t_ins( rooms, i_room )
		end
		new_vis_group.rooms = rooms
		local visible_groups = {}
		for i_visible_group, _ in pairs( vis_group.vis_groups ) do
			t_ins( visible_groups, i_visible_group )
		end
		new_vis_group.vis_groups = visible_groups
		t_ins( vis_groups, new_vis_group )
	end
	
	local nav_sectors = {}
	send_data.nav_sectors = nav_sectors
	for sector_id, sector in pairs( self._geog_segments ) do
		local rooms = {}
		local new_sector = { rooms = rooms }
		for i_room, _ in pairs( sector.rooms ) do
			t_ins( rooms, i_room )
		end
		nav_sectors[ sector_id ] = new_sector
	end
	
	local nav_field = World:quad_field()
	nav_field:set_navfield( send_data, callback( self, self, "clbk_navfield" ) )
end

-----------------------------------------------------------------------------------

function NavigationManager:_strip_nav_field_for_gameplay()
	-- strip doors
	local all_doors = self._room_doors
	local all_rooms = self._rooms
	local i_door = #all_doors
	while i_door ~= 0 do
		local door = all_doors[ i_door ]
		local seg_1 = self:get_nav_seg_from_i_room( door.rooms[1] )
		local seg_2 = self:get_nav_seg_from_i_room( door.rooms[2] )
		if seg_1 == seg_2 then
			-- internal door. delete
			all_doors[ i_door ] = nil
		else
			-- coarse graph door. strip
			local stripped_door = { center = door.pos }
			mvector3.lerp( stripped_door.center, door.pos, door.pos1, 0.5 )
			all_doors[ i_door ] = stripped_door
		end
		
		i_door = i_door - 1
	end
	
	for nav_seg_id, nav_seg in pairs( self._nav_segments ) do
		nav_seg.rooms = nil
		nav_seg.vis_groups = nil
	end
	
	self._rooms = {}
	self._geog_segments = {}
	self._geog_segment_offset = nil
	self._visibility_groups = {}
	self._helper_blockers = nil
	self._builder = nil
	self._covers = {}
end

-----------------------------------------------------------------------------------

function NavigationManager:_complete_nav_field_for_debug()
	for i_door, door in ipairs( self._room_doors ) do
		door.center = Vector3()
		mvec3_lerp( door.center, door.pos, door.pos1, 0.5 )
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:get_pos_reservation_id()
	local i = 1
	local filters = self._pos_rsrv_filters
	while filters[ i ] do
		i = i +1
	end
	
	filters[ i ] = true
	return i
end

-----------------------------------------------------------------------------------

function NavigationManager:release_pos_reservation_id( id )
	self._pos_rsrv_filters[ id ] = nil
end

-----------------------------------------------------------------------------------

function NavigationManager:convert_nav_link_maneuverability_to_SO_access( maneuverability )
	local t_ins = table.insert
	local nav_link_filter = {}
	if maneuverability <= 7 then
		t_ins( nav_link_filter, "security_patrol" )
	end
	if maneuverability <= 4 then
		t_ins( nav_link_filter, "spooc" )
	end
	if maneuverability <= 3 then
		t_ins( nav_link_filter, "murky" )
		t_ins( nav_link_filter, "sniper" )
		t_ins( nav_link_filter, "taser" )
		t_ins( nav_link_filter, "teamAI1" )
		t_ins( nav_link_filter, "teamAI2" )
		t_ins( nav_link_filter, "teamAI3" )
		t_ins( nav_link_filter, "teamAI4" )
	end
	if maneuverability <= 2 then
		t_ins( nav_link_filter, "fbi" )
		t_ins( nav_link_filter, "swat" )
		t_ins( nav_link_filter, "gangster" )
	end
	if maneuverability <= 1 then
		t_ins( nav_link_filter, "security" )
		t_ins( nav_link_filter, "cop" )
		t_ins( nav_link_filter, "tank" )
		t_ins( nav_link_filter, "shield" )
	end
	if maneuverability <= 0 then
		t_ins( nav_link_filter, "civ_male" )
		t_ins( nav_link_filter, "SO_ID1" )
		t_ins( nav_link_filter, "SO_ID2" )
		t_ins( nav_link_filter, "SO_ID3" )
	end
	local access_filter = self:convert_access_filter_to_number( nav_link_filter )
	print( "[NavigationManager:convert_nav_link_maneuverability_to_SO_access] maneuverability", maneuverability, "nav_link_filter", inspect( nav_link_filter ), "access_filter", access_filter )
	return access_filter
end

-----------------------------------------------------------------------------------

function NavigationManager:convert_SO_AI_group_to_access( ai_group_name )
	local ai_group_filter
	if ai_group_name == "friendlies" then
		ai_group_filter = { "teamAI1", "teamAI2", "teamAI3", "teamAI4" }
	elseif ai_group_name == "enemies" then
		ai_group_filter = { "cop", "security_patrol", "shield", "tank", "security", "gangster", "swat", "fbi", "taser", "sniper", "murky", "spooc" }
	elseif ai_group_name == "civilians" then
		ai_group_filter = { "civ_male", "civ_female", "SO_ID1", "SO_ID2", "SO_ID3" }
	elseif ai_group_name == "chavez" or ai_group_name == "bank_manager_old_man" or ai_group_name == "escort_guy_1" or ai_group_name == "escort_guy_2" or ai_group_name == "escort_guy_3" or ai_group_name == "escort_guy_5" then
		ai_group_filter = { "SO_ID1" }
	elseif ai_group_name == "escort_guy_4" then
		ai_group_filter = { "SO_ID2" }
	else
		debug_pause( "[NavigationManager:convert_SO_AI_group_to_access] Unknown SO AI group name", ai_group_name )
		return 0
	end
	local access_filter = self:convert_access_filter_to_number( ai_group_filter )
	--print( "[NavigationManager:convert_SO_ai_group_to_access] ai_group_name", ai_group_name, "ai_group_filter", inspect( ai_group_filter ), "access_filter", access_filter )
	return access_filter
end

-----------------------------------------------------------------------------------

function NavigationManager:convert_access_filter_to_number( access_filter )
	return self._quad_field:convert_access_filter_to_number( access_filter )
end

-----------------------------------------------------------------------------------

function NavigationManager:convert_access_filter_to_string( access_filter )
	return self._quad_field:convert_access_filter_to_string( access_filter )
end

-----------------------------------------------------------------------------------

function NavigationManager:convert_access_filter_to_table( access_filter )
	return self._quad_field:convert_access_filter_to_table( access_filter )
end

-----------------------------------------------------------------------------------

function NavigationManager:convert_access_flag( access_flag )
	return self._quad_field:convert_nav_link_flag_to_bitmask( access_flag )
end

-----------------------------------------------------------------------------------

function NavigationManager:check_access( access_filter, pos, neg )
	return self._quad_field:check_access_bitmask( access_filter, pos, neg )
end

-----------------------------------------------------------------------------------

function NavigationManager:upgrade_access_filter( access_filter_bitmask_old, version )
	local old_translation = self.ACCESS_FLAGS_OLD[ version ]
	self._quad_field:set_nav_link_filter( old_translation )	-- fake old filter translator
	local access_filter_table_old = self._quad_field:convert_nav_link_filter( access_filter_bitmask_old )	-- convert to string table
	self._quad_field:set_nav_link_filter( self.ACCESS_FLAGS )	-- set the new filter translator
	local access_filter_bitmask_new = self._quad_field:convert_nav_link_filter( access_filter_table_old )	-- convert the string table to a bitmask
	return access_filter_bitmask_new	-- return the new bitmask
end

-----------------------------------------------------------------------------------

function NavigationManager:get_nav_seg_metadata( nav_seg_id )
	return self._nav_segments[ nav_seg_id ]
end

-----------------------------------------------------------------------------------

function NavigationManager:set_location_ID( nav_seg_id, location_id )
	self:_set_nav_seg_metadata( nav_seg_id, "location_id", location_id )
end

-----------------------------------------------------------------------------------

function NavigationManager:set_suspicion_multiplier( nav_seg_id, suspicion_mul )
	self:_set_nav_seg_metadata( nav_seg_id, "suspicion_mul", suspicion_mul )
end

-----------------------------------------------------------------------------------

function NavigationManager:set_detection_multiplier( nav_seg_id, detection_mul )
	self:_set_nav_seg_metadata( nav_seg_id, "detection_mul", detection_mul )
end

-----------------------------------------------------------------------------------

function NavigationManager:_set_nav_seg_metadata( nav_seg_id, param_name, param_value )
	if self._nav_segments then
		local nav_seg = self._nav_segments[ nav_seg_id ]
		if nav_seg then
			nav_seg[ param_name ] = param_value
		end
	end
	
	self._builder:set_nav_seg_metadata( nav_seg_id, param_name, param_value )
end

-----------------------------------------------------------------------------------

function NavigationManager:add_obstacle( obstacle_unit, obstacle_obj_name )
	--print( "[NavigationManager:add_obstacle]", obstacle_unit, obstacle_obj_name )
	local obstacle_obj = obstacle_unit:get_object( obstacle_obj_name )
	local id = self._quad_field:add_obstacle( obstacle_obj )
	
	table.insert( self._obstacles, { unit = obstacle_unit, obstacle_obj_name = obstacle_obj_name, id = id } )
end

-----------------------------------------------------------------------------------

function NavigationManager:remove_obstacle( obstacle_unit, obstacle_obj_name )
	--print( "[NavigationManager:remove_obstacle]", obstacle_unit, obstacle_obj_name )
	local obstacle_obj = obstacle_unit:get_object( obstacle_obj_name )
	self._quad_field:remove_obstacle( obstacle_obj )
	
	for i, obs_data in ipairs( self._obstacles ) do
		if not alive( obs_data.unit ) then
			debug_pause( "[NavigationManager:remove_obstacle] obstacle destroyed before being removed" )
		elseif obs_data.unit:key() == obstacle_unit:key() and obs_data.obstacle_obj_name == obstacle_obj_name then
			self._obstacles[ i ] = self._obstacles[ #self._obstacles ]
			table.remove( self._obstacles )
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:clbk_navfield( event_name, args )
	--debug_pause( "[NavigationManager:clbk_navfield]", event_name, inspect( args ) )
	if event_name == "add_nav_seg_neighbours" then
		for nav_seg_id, add_neighbours in pairs( args ) do
			local nav_seg = self._nav_segments[ nav_seg_id ]
			for _, other_nav_seg_id in ipairs( add_neighbours ) do
				if nav_seg.disabled_neighbours[ other_nav_seg_id ] then
					nav_seg.neighbours[ other_nav_seg_id ] = nav_seg.disabled_neighbours[ other_nav_seg_id ]
					nav_seg.disabled_neighbours[ other_nav_seg_id ] = nil
				end
			end
		end
		for nav_seg_id, add_neighbours in pairs( args ) do
			managers.groupai:state():on_nav_seg_neighbours_state( nav_seg_id, args, true )
		end
	elseif event_name == "remove_nav_seg_neighbours" then
		for nav_seg_id, rem_neighbours in pairs( args ) do
			local nav_seg = self._nav_segments[ nav_seg_id ]
			for _, other_nav_seg_id in ipairs( rem_neighbours ) do
				nav_seg.disabled_neighbours = nav_seg.disabled_neighbours or {}
				nav_seg.disabled_neighbours[ other_nav_seg_id ] = nav_seg.neighbours[ other_nav_seg_id ]
				nav_seg.neighbours[ other_nav_seg_id ] = nil
			end
		end
		for nav_seg_id, add_neighbours in pairs( args ) do
			managers.groupai:state():on_nav_seg_neighbours_state( nav_seg_id, args, false )
		end
	elseif event_name == "invalidated_script_data" then
		if type( args ) == "table" then
			for i, k in pairs( args ) do
				--print( "entry", i, "\n", inspect( k ) )
				if k.covers then
					for i_cover, cover in ipairs( k.covers ) do
						cover[3]:move( cover[1] ) -- force update of the tracker
						local location_script_data = self._quad_field:get_script_data( cover[3], true ) -- location, allow_create
						if not location_script_data.covers then
							location_script_data.covers = {}
						end
						table.insert( location_script_data.covers, cover )
					end
				end
			end
		end
	elseif event_name == "add_quads" then -- PRODUCTION only
		if not self._debug then
			return
		end
		for i, k in pairs( args ) do
			--print( "entry", i, "\n", inspect( k.borders ) )
			self:_add_quad_by_data( k )
		end
	elseif event_name == "remove_quads" then -- PRODUCTION only
		if not self._debug then
			return
		end
		for i, k in pairs( args ) do
			--print( "entry", i, "\n", inspect( k.borders ) )
			self:_remove_quad_by_data( k )
		end
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_remove_quad_by_data( quad_data )
	--print( "[NavigationManager:_remove_quad_by_data]\n", inspect( quad_data ) )
	local room_c = self._builder:_calculate_room_center( quad_data, true )
	local closest_dis, closest_i_room
	for i_room, room in ipairs( self._rooms ) do
		local test_room_c = self._builder:_calculate_room_center( room )
		local my_dis = mvector3.distance_sq( test_room_c, room_c )
		if not closest_dis or my_dis < closest_dis then
			closest_i_room = i_room
			closest_dis = my_dis
		end
	end
	
	if closest_i_room then
		self._rooms[ closest_i_room ].added = nil
		self._rooms[ closest_i_room ].removed = true
	end
end

-----------------------------------------------------------------------------------

function NavigationManager:_add_quad_by_data( quad_data )
	quad_data.added = true
	table.insert( self._rooms, quad_data )
end

-----------------------------------------------------------------------------------

function NavigationManager:destroy()
	self._quad_field:clear_all()
end


-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------