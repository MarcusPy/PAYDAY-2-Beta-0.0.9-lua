HUDHint = HUDHint or class()

function HUDHint:init( hud )
	self._hud_panel = hud.panel
	
	if self._hud_panel:child( "hint_panel" ) then	
		self._hud_panel:remove( self._hud_panel:child( "hint_panel" ) )
	end
			
	self._hint_panel = self._hud_panel:panel( { visible = false, name = "hint_panel", h = 30, y = 0, valign = {1/3.2,0}, layer = 3 } )
	local y = self._hud_panel:h()/3.2
	self._hint_panel:set_center_y( y )
	-- local y = self._hud_panel:center_y()/2.5
	
	-- self._hint_panel:set_debug ( true )
	
	local marker = self._hint_panel:rect( { name = "marker", visible = true, color = Color.white:with_alpha( 0.75 ), layer = 2, h = 30, w = 12 } )
	marker:set_center_y( self._hint_panel:h()/2 )
	
	local clip_panel = self._hint_panel:panel( { name = "clip_panel" } )
	
	clip_panel:rect( { name = "bg", visible = true, color = Color.black:with_alpha( 0.25 ) } )
	
	clip_panel:text( { name = "hint_text", text = "", font_size = 28, font = tweak_data.hud.medium_font_noshadow, color = Color.white, align = "center", vertical = "center", layer = 1, wrap = false, word_wrap = false } ) 
	-- clip_panel:text( { name = "hint_shadow_text", text = "", font_size = 28, font = tweak_data.hud.medium_font, color = Color.black, align = "center", vertical = "center", layer = 0, wrap = false, word_wrap = false, y = 1, x = 1 } )
end

function HUDHint:show( params )
	local text = params.text
	
	-- self._hint_panel:child( "hint_text" ):set_text( string.upper( text ) )
	-- self._hint_panel:child( "hint_shadow_text" ):set_text( string.upper( text ) )
		
	local clip_panel = self._hint_panel:child( "clip_panel" )
	clip_panel:child( "hint_text" ):set_text( utf8.to_upper( "" ) )
	-- clip_panel:child( "hint_shadow_text" ):set_text( string.upper( "" ) )
	-- Presentation
	-- hud.hint_text:set_text( string.upper( text ) )
	-- hud.hint_shadow_text:set_text( string.upper( text ) )
	
	self._stop = false
	self._hint_panel:stop()
	
	self._hint_panel:animate( callback( self, self, "_animate_show" ), callback( self, self, "show_done" ), params.time or 3, utf8.to_upper( text ) )
end

function HUDHint:stop()
	self._stop = true
end

function HUDHint:_animate_show( hint_panel, done_cb, seconds, text )
	local clip_panel = hint_panel:child( "clip_panel" )
	local hint_text = clip_panel:child( "hint_text" )
	-- hint_text:set_text( text ) -- string.upper( text ) )
	-- hint_panel:child( "hint_shadow_text" ):set_text( text ) -- string.upper( text ) )
	
	hint_panel:set_visible( true )
	hint_panel:set_alpha( 1 )
	
	-- clip_panel:set_w( 0 )
	hint_text:set_text( text )
	
	local offset = 32
	local _,_,w,h = hint_text:text_rect()
	hint_text:set_w( w + offset )
	clip_panel:set_w( w + offset )
	clip_panel:set_center_x( clip_panel:parent():w()/2 )
	clip_panel:set_w( 0 )
	local target_w = w + offset
	local w = 0
		
	local marker = hint_panel:child( "marker" )
	marker:set_visible( true )
	
	local t = seconds
	local forever = t == -1
	local st = 1
	local cs = 0.0
	
	local speed = 600
	
	-- Presenting
	local presenting = true
	while presenting do -- t > 0 do
		local dt = coroutine.yield()
		-- t = t - dt
		
		-- hint_text:set_w( math.lerp( hint_text:parent():w(), 0, t/seconds ) )
		-- hint_text:set_center_x( hint_text:parent():w()/2 )
		
		-- hint_text:set_text( text )
		
		-- st = st - dt
		-- local w = math.lerp( hint_panel:parent():w(), 0, math.clamp( st/1, 0, 1 ) )
		-- local w = math.lerp( target_w, 0, math.clamp( st/1, 0, 1 ) )
		-- local w = math.lerp( target_w, 0, math.clamp( st/1, 0, 1 ) )
		w = math.clamp( w + dt * speed, 0, target_w )
		
		presenting = w ~= target_w
		-- print( w )  
		-- marker:set_visible( w ~= target_w )
		-- print( w )
		clip_panel:set_w( w )
		-- hint_panel:set_w( w )
		-- hint_panel:set_w( math.lerp( hint_panel:parent():w(), hint_panel:parent():w()/2, t/seconds ) )
		-- hint_panel:set_w( hint_panel:parent():w()/1.75 )
		hint_text:set_text( "" )
		hint_text:set_text( text )
		-- hint_text:set_w( hint_text:w() )
		-- hint_text:set_x( math.rand( 0.01 ) )
		
		marker:set_alpha( (1 + math.sin( Application:time() * 800 ))/2 )
		marker:set_right( clip_panel:right() )
		
		--[[if t < 0.5 then
			hint_panel:set_alpha( t/0.5 )
			
			w = math.clamp( w + dt - 500, 0, target_w )
			clip_panel:set_w( w )
		end]]
	end
	
	-- Wait
	while (t > 0 or forever ) and not self._stop do
		local dt = coroutine.yield()
		t = t - dt
		
		marker:set_alpha( (1 + math.sin( Application:time() * 800 ))/2 )
		marker:set_right( clip_panel:right() )
	end
	
	self._stop = false
	
	-- Removing
	--[[local t = 0.5
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		
		hint_panel:set_alpha( t/0.5 )
	end]]
	
	local removing = true
	while removing do
		local dt = coroutine.yield()
		
		w = math.clamp( w - dt * speed * 2, 0, target_w )
		clip_panel:set_w( w )
		removing = w ~= 0
				
		marker:set_alpha( (1 + math.sin( Application:time() * 800 ))/2 )
		marker:set_right( clip_panel:right() )
	end
	
	hint_panel:set_visible( false )
	done_cb()

	--[[local split = {}
	for i = 1, string.len( text ) do
		table.insert( split, string.sub( text, i, i ) )
	end
	local hint = ""
	-- print( inspect( split ) )
	hint_panel:set_visible( true )
	hint_panel:set_alpha( 1 )
	local hint_text = hint_panel:child( "hint_text" )
	local hint_shadow_text = hint_panel:child( "hint_shadow_text" )
	local t = seconds
	local cs = 0.0
	while t > 0 do
		local dt = coroutine.yield()
		t = t - dt
		if t < 0.5 then
			hint_panel:set_alpha( t/0.5 )
		end
		
		cs = cs - dt
		if cs < 0 and #split > 0 then
			hint = hint..table.remove( split, 1 )
			-- hint_text:set_text( string.upper( hint.."" ) )
			-- hint_shadow_text:set_text( string.upper( hint.."" ) )
			hint_text:set_text( hint.."" )
			hint_shadow_text:set_text( hint.."" )
			cs = 0.025
		end
	end
	hint_panel:set_visible( false )
	done_cb()]]
end
  

function HUDHint:show_done()
	-- print( "HUDHint:show_done()" )
end
