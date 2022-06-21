SineSpline = SineSpline or class()

function SineSpline:init( position_table, nr_subseg, curviness, first_control_point, last_control_point )
	self._segments = position_table
	self._manual_first_control_point = first_control_point
	self._manual_last_control_point = last_control_point
	
	self._curviness = curviness
	self._nr_subseg = nr_subseg		--	NR subsegments per segment
	
	self._control_points = {}
	
	if first_control_point then
		self._control_points[ 1 ] = { p2 = first_control_point }
	end
	
	if last_control_point then
		self._control_points[ #position_table ] = { p1 = last_control_point }
	end
end

------------------------------------------------------------------------------------------

function SineSpline:prepare_walk_data( backward )	
	if #self._segments > 1 then
		self._mvec3_1 = Vector3()		--	MUTABLE
		self._mvec3_2 = Vector3()		--	MUTABLE
	
		if backward then
			local nr_seg = #self._segments
			if not self._control_points[ nr_seg - 1 ] then
				self:_extract_control_points_at_index( nr_seg - 1 )
			end
			if not self._control_points[ nr_seg ] then
				self:_extract_control_points_at_index( nr_seg )
			end
			
			local next_subseg = self._segments[ nr_seg ]
			local cur_subseg = self:_position_at_time_on_segment( ( self._nr_subseg - 1 ) / self._nr_subseg, self._segments[ nr_seg - 1 ], self._segments[ nr_seg ], self._control_points[ nr_seg ].p1, self._control_points[ nr_seg - 1 ].p2 )	
			local subseg_len = mvector3.distance( cur_subseg, next_subseg )
			
			local playtime_data = {}
			playtime_data.seg_i = nr_seg - 1		--	The index of the spline point we last visited
			playtime_data.subseg_i = self._nr_subseg - 1		--	The index of the subsegment we are travelling along
			playtime_data.subseg_start = cur_subseg
			playtime_data.subseg_end = next_subseg
			playtime_data.subseg_dis = subseg_len		--	How far along the subsegment we have travelled. ( cm )
			playtime_data.subseg_len = subseg_len		--	The length of the current subsegment. ( cm )
			
			self._playtime_data = playtime_data
		else
			if not self._control_points[ 2 ] then
				self:_extract_control_points_at_index( 2 )
			end
			if not self._control_points[ 1 ] then
				self:_extract_control_points_at_index( 1 )
			end
			
			local cur_subseg = self._segments[ 1 ]
			local next_subseg = self:_position_at_time_on_segment( 1 / self._nr_subseg, cur_subseg, self._segments[ 2 ], self._control_points[ 2 ].p1, self._control_points[ 1 ].p2 )
			local subseg_len = mvector3.distance( cur_subseg, next_subseg )
			
			local playtime_data = {}
			playtime_data.seg_i = 1		--	The index of the spline point we last visited
			playtime_data.subseg_i = 1		--	The index of the subsegment we are travelling along
			playtime_data.subseg_start = cur_subseg
			playtime_data.subseg_end = next_subseg
			playtime_data.subseg_dis = 0		--	How far along the subsegment we have travelled. ( cm )
			playtime_data.subseg_len = subseg_len		--	The length of the current subsegment. ( cm )
			
			self._playtime_data = playtime_data
		end
	end
end

------------------------------------------------------------------------------------------

function SineSpline:delete_walk_data()
	self._mvec3_1 = nil
	self._mvec3_2 = nil
	
	self._control_points = {}
	
	self._playtime_data = nil
end

------------------------------------------------------------------------------------------

function SineSpline:_position_at_time_on_segment( seg_t, pos_start, pos_end, p1, p2 )
	mvector3.lerp( self._mvec3_1, pos_start, p2, seg_t )
	mvector3.lerp( self._mvec3_2, p1, pos_end, seg_t )
			
	local xpo = ( math.sin( ( seg_t * 2 - 1 ) * 90 ) + 1 ) * self._curviness
	return math.lerp( self._mvec3_1, self._mvec3_2, xpo )
end

------------------------------------------------------------------------------------------

-- Move along the spline "delta_dis" cm and return the result position
function SineSpline:walk( delta_dis )
	local result_pos

	if #self._segments > 1 then
		local segments = self._segments
		local play_data = self._playtime_data
		
		--	Travel along the subsegment by the delta value
		local new_subseg_dis = play_data.subseg_dis + delta_dis
		
		if new_subseg_dis < 0 then		--	This is the segment / subsegment advancement code if we are travelling backwards
			local undershot = new_subseg_dis
			repeat
				if play_data.subseg_i == 1 then
					if play_data.seg_i == 1 then			--	We have reached the end of the spline! return the position of the last segment and our overshoot
						play_data.subseg_dis = 0
						return segments[ 1 ], undershot
					else		--Advance segment
						play_data.seg_i = play_data.seg_i - 1
						--print( "Backing segment", play_data.seg_i )
						if not self._control_points[ play_data.seg_i ] then
							self:_extract_control_points_at_index( play_data.seg_i )	
						end
						
						play_data.subseg_i = self._nr_subseg
						
						play_data.subseg_start = self:_position_at_time_on_segment( ( self._nr_subseg - 1 ) / self._nr_subseg, segments[ play_data.seg_i ], segments[ play_data.seg_i + 1 ], self._control_points[ play_data.seg_i + 1 ].p1, self._control_points[ play_data.seg_i ].p2 )
						play_data.subseg_end = segments[ play_data.seg_i + 1 ]
						play_data.subseg_len = mvector3.distance( play_data.subseg_start, play_data.subseg_end )
						new_subseg_dis = play_data.subseg_len + undershot
					end
				else
					play_data.subseg_i = play_data.subseg_i - 1
					--print( "Backing subsegment", play_data.subseg_i )
					play_data.subseg_end = play_data.subseg_start
					play_data.subseg_start = self:_position_at_time_on_segment( play_data.subseg_i / self._nr_subseg, segments[ play_data.seg_i ], segments[ play_data.seg_i + 1 ], self._control_points[ play_data.seg_i + 1 ].p1, self._control_points[ play_data.seg_i ].p2 )
					play_data.subseg_len = mvector3.distance( play_data.subseg_start, play_data.subseg_end )
					new_subseg_dis = play_data.subseg_len + undershot
				end
			until new_subseg_dis > 0
			
			--	Now "new_subseg_dis" is the length along the subsegment we need we need to travel to find our current position
			play_data.subseg_dis = new_subseg_dis
		
			return math.lerp( play_data.subseg_start, play_data.subseg_end, play_data.subseg_dis / play_data.subseg_len ), play_data.subseg_i == 1 and undershot
			
		else
			--	Check if we have overshot the end of the subsegment, in which case we need to advance subsegment and perhaps also segment
			while play_data.subseg_len < new_subseg_dis do
				local overshot = new_subseg_dis - play_data.subseg_len
				if play_data.subseg_i == self._nr_subseg then		--	This was the last subsegment in the segment. We need to advance segment or terminate
					if play_data.seg_i == #segments - 1 then			--	We have reached the end of the spline! return the position of the last segment and our overshoot
						play_data.subseg_dis = play_data.subseg_len
						return segments[ #segments ], overshot
					else		--Advance segment
						play_data.seg_i = play_data.seg_i + 1
						--print( "Advancing segment", play_data.seg_i )
						if not self._control_points[ play_data.seg_i + 1 ] then
							self:_extract_control_points_at_index( play_data.seg_i + 1 )	
						end
						
						play_data.subseg_i = 1
						
						play_data.subseg_start = segments[ play_data.seg_i ]
						play_data.subseg_end = self:_position_at_time_on_segment( 1 / self._nr_subseg, segments[ play_data.seg_i ], segments[ play_data.seg_i + 1 ], self._control_points[ play_data.seg_i + 1 ].p1, self._control_points[ play_data.seg_i ].p2 )
						play_data.subseg_len = mvector3.distance( play_data.subseg_start, play_data.subseg_end )
						new_subseg_dis = overshot
					end
				else
					play_data.subseg_i = play_data.subseg_i + 1
					--print( "Advancing subsegment", play_data.subseg_i )
					play_data.subseg_start = play_data.subseg_end
					play_data.subseg_end = self:_position_at_time_on_segment( play_data.subseg_i / self._nr_subseg, segments[ play_data.seg_i ], segments[ play_data.seg_i + 1 ], self._control_points[ play_data.seg_i + 1 ].p1, self._control_points[ play_data.seg_i ].p2 )
					play_data.subseg_len = mvector3.distance( play_data.subseg_start, play_data.subseg_end )
					new_subseg_dis = overshot
				end
			end
		end
		
		--	Now "new_subseg_dis" is the length along the subsegment we need we need to travel to find our current position
		play_data.subseg_dis = new_subseg_dis
		
		return math.lerp( play_data.subseg_start, play_data.subseg_end, play_data.subseg_dis / play_data.subseg_len ), play_data.subseg_i == self._nr_subseg and play_data.subseg_len - play_data.subseg_dis
	else
		return self._segments[ 1 ]
	end
end

------------------------------------------------------------------------------------------

function SineSpline:_extract_control_points_at_index( index )	
	local segments = self._segments
	local control_points = self._control_points
	local pos = segments[ index ]

	local segment_control_points = {}
	
	if index == #segments then
		if control_points[ #segments - 1 ] then
			local last_seg = pos - segments[ #segments - 1 ]
			local last_vec = ( control_points[ #segments - 1 ].p2 or segments[ 1 ] ) - segments[ #segments - 1 ]
			local last_angle = last_vec:angle( last_seg )
			local last_rot = last_seg:cross( last_vec )
			last_rot = Rotation( last_rot, 180 - 2 * last_angle )
			local w_vec = pos + last_vec:rotate_with( last_rot )
			segment_control_points.p1 = w_vec
		else
			segment_control_points.p1 = pos
		end
	elseif index == 1 then
		if control_points[ 2 ] then
			local first_vec = control_points[ 2 ].p1 - segments[ 2 ]
			local first_seg = segments[ 2 ] - segments[ 1 ]
			local first_angle = first_vec:angle( first_seg )
			local first_rot = first_seg:cross( first_vec )
			first_rot = Rotation( first_rot, 180 - 2 * first_angle )
			local w_vec = segments[ 1 ] + first_vec:rotate_with( first_rot )
			segment_control_points.p2 = w_vec
		else
			segment_control_points.p2 = pos
		end
	else
		local tan_seg = segments[ index + 1 ] - segments[ index - 1 ]
		mvector3.set_length( tan_seg, mvector3.distance( pos, segments[ index - 1 ] ) * self._curviness )
		segment_control_points.p1 = pos - tan_seg
		mvector3.set_length( tan_seg, mvector3.distance( pos, segments[ index + 1 ] ) * self._curviness )
		segment_control_points.p2 = pos + tan_seg
	end
	
	self._control_points[ index ] = segment_control_points
end

------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------