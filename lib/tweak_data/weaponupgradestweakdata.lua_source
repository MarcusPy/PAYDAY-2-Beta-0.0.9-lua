WeaponUpgradesTweakData = WeaponUpgradesTweakData or class()

function WeaponUpgradesTweakData:init()
	self.upgrades = {}
	self.upgrades.scope1 = {}
	self.upgrades.scope1.name_id = "bm_wu_scope1"
		
	self.upgrades.scope2 = {}
	self.upgrades.scope2.name_id = "bm_wu_scope2"
	
	self.upgrades.scope3 = {}
	self.upgrades.scope3.name_id = "bm_wu_scope3"
	
	self.upgrades.barrel1 = {}
	self.upgrades.barrel1.name_id = "bm_wu_barrel1"
	
	self.upgrades.barrel2 = {}
	self.upgrades.barrel2.name_id = "bm_wu_barrel2"
	
	self.upgrades.barrel3 = {}
	self.upgrades.barrel3.name_id = "bm_wu_barrel3"
	
	self.upgrades.grip1 = {}
	self.upgrades.grip1.name_id = "bm_wu_grip1"
	
	self.upgrades.grip2 = {}
	self.upgrades.grip2.name_id = "bm_wu_grip2"
	
	self.upgrades.grip3 = {}
	self.upgrades.grip3.name_id = "bm_wu_grip3"
	
	self.upgrades.m14_scope1 = {}
	self.upgrades.m14_scope1.name_id = "bm_wu_scope1"
	self.upgrades.m14_scope1.visual_upgrade = "m14_spread2"
	
	self.upgrades.m14_scope2 = {}
	self.upgrades.m14_scope2.name_id = "bm_wu_scope2"
	self.upgrades.m14_scope2.visual_upgrade = "m14_spread1"
		
	self.upgrades.m4_scope1 = {}
	self.upgrades.m4_scope1.name_id = "bm_wu_scope1"
	self.upgrades.m4_scope1.visual_upgrade = "m4_spread4"
		
	self.upgrades.m4_barrel1 = {}
	self.upgrades.m4_barrel1.name_id = "bm_wu_barrel1"
	self.upgrades.m4_barrel1.visual_upgrade = "m4_spread2"
	
	self.upgrades.hk21_grip1 = {}
	self.upgrades.hk21_grip1.name_id = "bm_wu_grip1"
	self.upgrades.hk21_grip1.visual_upgrade = "hk21_mag4"
	
	self.upgrades.hk21_barrel1 = {}
	self.upgrades.hk21_barrel1.name_id = "bm_wu_barrel1"
	self.upgrades.hk21_barrel1.visual_upgrade = "hk21_recoil1"
	
	self.upgrades.hk21_scope1 = {}
	self.upgrades.hk21_scope1.name_id = "bm_wu_scope1"
	self.upgrades.hk21_scope1.visual_upgrade = "hk21_recoil2"
		
	self.weapon = {}
	
	self.weapon.m4 = {}
	self.weapon.m4.scopes = { "m4_scope1", "scope2", "scope3" }
	self.weapon.m4.barrels = { "m4_barrel1", "barrel2" }
	self.weapon.m4.grips = { "grip1", "grip2" }
	
	self.weapon.m14 = {}
	self.weapon.m14.scopes = { "m14_scope1", "m14_scope2", "scope3" }
	self.weapon.m14.barrels = { "barrel1", "barrel2" }
	self.weapon.m14.grips = { "grip2" }
	
	self.weapon.raging_bull = {}
	self.weapon.raging_bull.scopes = { "scope3" }
	self.weapon.raging_bull.barrels = { "barrel1" }
	self.weapon.raging_bull.grips = { "grip1", "grip2" }
	
	self.weapon.hk21 = {}
	self.weapon.hk21.scopes = { "hk21_scope1" }
	self.weapon.hk21.barrels = { "hk21_barrel1" }
	self.weapon.hk21.grips = { "hk21_grip1" }
	
	-- TEST DATABASE FOR UPGRADES
	--[[self.test = {}
	
	-- UNLOCKED SHOULD NOT BE PER WEAPON
	for weapon,_ in pairs( self.weapon ) do
		self.test[ weapon ] = {}
		for upgrades,data in pairs( self.weapon[ weapon ] ) do
			for _,upgrade in ipairs( data ) do
				self.test[ weapon ][ upgrade ] = { unlocked = true, owned = true, attached = false }
			end
		end
	end
		
	self.test.m4.m4_scope1.attached = false
	self.test.m4.scope2.owned = false
	-- self.test.m4.barrel1.owned = false
	self.test.m4.scope3.unlocked = false
	self.test.m4.scope3.owned = false
	self.test.m4.grip1.unlocked = false
	self.test.m4.grip1.owned = false
	
	self.test.m14.m14_scope1.attached = true
	
	self.test.m14.m14_scope2.owned = false
	self.test.m14.barrel1.owned = false
	
	self.test.m14.scope3.unlocked = false
	self.test.m14.scope3.owned = false
	
	self.test.raging_bull.grip1.unlocked = false
	self.test.raging_bull.grip1.owned = false]]
end
-----------------------------------------------------------------------------------