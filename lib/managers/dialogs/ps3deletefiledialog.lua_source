core:module( "SystemMenuManager" )

require "lib/managers/dialogs/DeleteFileDialog"

PS3DeleteFileDialog = PS3DeleteFileDialog or class( DeleteFileDialog )

function PS3DeleteFileDialog:show()
	self._manager:event_dialog_shown( self )

	--PS3:display_delete_save()		-- This has been removed in the engine. Replace it when a new function is available.

	self:done_callback( true )

	return true
end
