# RobloxDatastoreWrapper
Pretty much a simpler version of datastore2, needs documentation. dm risknight on discord if you want to make documentation for it.


Edit defaultData under the wrapper to create a default data format.

simple script using it:
```
local ss = game:GetService("ServerStorage") -- where you put the module
local players = game:GetService("Players")
local dsw = require(ss:WaitForChild("datastoring"))
local dd = require(rs:WaitForChild("defaultData"))

local function setupLeaderstats(p: Player)
	local leaderstats = Instance.new("Folder")
	leaderstats.Name = "leaderstats"
	leaderstats.Parent = p

	for i,v in pairs(dd["leaderstats"]) do
		local newInstance = Instance.new("IntValue")
		newInstance.Name = i
		newInstance.Value = dsw.getValue(p, i) or v
		newInstance.Parent = leaderstats

		newInstance.Changed:Connect(function()
			dsw.setValue(p, i, newInstance.Value)
		end)
	end
end

players.PlayerAdded:Connect(function(p)
	dsw.initPlayer(p)
	
	task.wait(2)
	setupLeaderstats(p)
end)

players.PlayerRemoving:Connect(function(p)
	dsw.save(p)
end)

game:BindToClose(function()
	for _,player in pairs(players:GetPlayers()) do
		dsw.save(player)
	end
end)
```
