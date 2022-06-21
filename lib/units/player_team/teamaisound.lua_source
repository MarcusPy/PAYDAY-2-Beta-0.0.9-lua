TeamAISound = TeamAISound or class( PlayerSound )

function TeamAISound:init( unit )
	self._unit = unit
	unit:base():post_init()	--	This is the last extension of the player unit. It is thus its job to tell the base that all extensions have been initialized. Question: what happens if another unit has this class as an extension in a different order?
	
	local ss = unit:sound_source( )
	ss:set_switch( "robber", tweak_data.character[ unit:base()._tweak_table ].speech_prefix )
	ss:set_switch( "int_ext", "third" )
end

---------------------------------------------------------------

function TeamAISound:set_voice( voice )
	local ss = self._unit:sound_source( )
	ss:set_switch( "robber", voice )
	ss:set_switch( "int_ext", "third" )
end

---------------------------------------------------------------
---------------------------------------------------------------