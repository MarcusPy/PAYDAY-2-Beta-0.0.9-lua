ListenerHolder = ListenerHolder or class()

-- key can be anything. You provide the same key when you want to remove the listener
-- clbk is a callback object
function ListenerHolder:add( key, clbk )
	if self._calling then
		self:_set_new( key, clbk )
	else
		self:_add( key, clbk )
	end
end

-----------------------------------------------------------------------------------

-- key is the same key you provided when you added the listener
function ListenerHolder:remove( key )
	if self._calling then	-- We are currently looping through the listeners and cannot remove elements
		self:_set_trash( key )
	else
		self:_remove( key )
	end
end

-----------------------------------------------------------------------------------

function ListenerHolder:call( ... )
	if self._listeners then
	
		self._calling = true	-- This flag means we are are iterating the self._listeners table and cannot append/remove elements
		
		for key, clbk in pairs( self._listeners ) do
			if self:_not_trash( key ) then
				clbk( ... )
			end
		end
		self._calling = nil
		
		-- Append listeners that were added while calling
		self:_append_new_additions()
		-- Remove listeners that were removed while calling
		self:_dispose_trash()
	end
end

-----------------------------------------------------------------------------------

function ListenerHolder:_remove( key )
	self._listeners[ key ] = nil
	if not next( self._listeners ) then
		self._listeners = nil
	end
end

-----------------------------------------------------------------------------------

function ListenerHolder:_add( key, clbk )
	self._listeners = self._listeners or {}
	self._listeners[ key ] = clbk
end

-----------------------------------------------------------------------------------

function ListenerHolder:_set_trash( key )
	self._trash = self._trash or {}
	self._trash[ key ] = true
	if self._additions then	-- If the item was added while calling, it needs to be removed from the additions table
		self._additions[ key ] = nil
	end 
end

-----------------------------------------------------------------------------------

function ListenerHolder:_set_new( key, clbk )
	self._additions = self._additions or {}
	self._additions[ key ] = clbk
	if self._trash then
		self._trash[ key ] = nil
	end 
end

-----------------------------------------------------------------------------------

function ListenerHolder:_append_new_additions()
	if self._additions then
		for key, clbk in pairs( self._additions ) do
			self:_add( key, clbk )
		end
		self._additions = nil
	end
end

-----------------------------------------------------------------------------------

function ListenerHolder:_dispose_trash()
	if self._trash then
		for key, _ in pairs( self._trash ) do
			self:_remove( key )
		end
		self._trash = nil
	end
end

-----------------------------------------------------------------------------------

function ListenerHolder:_not_trash( key )
	return not ( self._trash and self._trash[ key ] )
end

-----------------------------------------------------------------------------------
-----------------------------------------------------------------------------------