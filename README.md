local Teams = game:GetService("Teams")

local gangNames = {"Red Gang","Blue Gang","Green Gang","Cops"}

for _,name in pairs(gangNames) do
	if not Teams:FindFirstChild(name) then
		local team = Instance.new("Team")
		team.Name = name
		team.AutoAssignable = false
		team.Parent = Teams
	end
endlocal player = game.Players.LocalPlayer
local bar = script.Parent:WaitForChild("HealthBar")

local function hook(char)
	local hum = char:WaitForChild("Humanoid")

	hum.HealthChanged:Connect(function()
		bar.Size = UDim2.new(hum.Health / hum.MaxHealth,0,1,0)
	end)
end

player.CharacterAdded:Connect(hook)
if player.Character then
	hook(player.Character)
endlocal tool = script.Parent
local cooldown = false

tool.Activated:Connect(function()
	if cooldown then return end
	cooldown = true

	local char = tool.Parent
	local player = game.Players:GetPlayerFromCharacter(char)
	if not player then return end

	local sound = tool.Handle:FindFirstChild("ShotSound")
	if sound then sound:Play() end

	local rayParams = RaycastParams.new()
	rayParams.FilterDescendantsInstances = {char}
	rayParams.FilterType = Enum.RaycastFilterType.Blacklist

	local result = workspace:Raycast(
		tool.Handle.Position,
		char.HumanoidRootPart.CFrame.LookVector * 200,
		rayParams
	)

	if result and result.Instance then
		local model = result.Instance:FindFirstAncestorOfClass("Model")
		if model and model:FindFirstChild("Humanoid") then
			model.Humanoid:TakeDamage(15)
		end
	end

	task.wait(0.25)
	cooldown = false
end)local MPS = game:GetService("MarketplaceService")
local Players = game:GetService("Players")

local LAUNCHER_PRODUCT_ID = 123456789 -- replace

MPS.ProcessReceipt = function(receipt)
	local player = Players:GetPlayerByUserId(receipt.PlayerId)
	if not player then return Enum.ProductPurchaseDecision.NotProcessedYet end

	local launcher = game.ServerStorage.Launcher:Clone()
	launcher.Parent = player.Backpack

	return Enum.ProductPurchaseDecision.PurchaseGranted
endlocal Teams = game:GetService("Teams")
local jail = workspace:WaitForChild("JailSpawn")

game.Players.PlayerAdded:Connect(function(player)

	player.CharacterAdded:Connect(function(char)
		local hum = char:WaitForChild("Humanoid")

		hum.Touched:Connect(function(hit)
			local otherChar = hit.Parent
			local otherHum = otherChar and otherChar:FindFirstChild("Humanoid")

			if not otherHum then return end

			local cop = game.Players:GetPlayerFromCharacter(char)
			local criminal = game.Players:GetPlayerFromCharacter(otherChar)

			if not cop or not criminal then return end
			if cop.Team.Name ~= "Cops" then return end

			local root = otherChar:FindFirstChild("HumanoidRootPart")
			if not root then return endlocal Teams = game:GetService("Teams")
local jail = workspace:WaitForChild("JailSpawn")

game.Players.PlayerAdded:Connect(function(player)

	player.CharacterAdded:Connect(function(char)
		local hum = char:WaitForChild("Humanoid")

		hum.Touched:Connect(function(hit)
			local otherChar = hit.Parent
			local otherHum = otherChar and otherChar:FindFirstChild("Humanoid")

			if not otherHum then return end

			local cop = game.Players:GetPlayerFromCharacter(char)
			local criminal = game.Players:GetPlayerFromCharacter(otherChar)

			if not cop or not criminal then return end
			if cop.Team.Name ~= "Cops" then return end

			local root = otherChar:FindFirstChild("HumanoidRootPart")
			if not root then return end

			root.CFrame = jail.CFrame + Vector3.new(0,3,0)

			task.delay(13,function()
				if criminal.Character and criminal.Character:FindFirstChild("HumanoidRootPart") then
					criminal.Character.HumanoidRootPart.CFrame = workspace.SpawnLocation.CFrame + Vector3.new(0,3,0)
				end
			end)
		end)
	end)
end)
			root.CFrame = jail.CFrame + Vector3.new(0,3,0)

			task.delay(13,function()
				if criminal.Character and criminal.Character:FindFirstChild("HumanoidRootPart") then
					criminal.Character.HumanoidRootPart.CFrame = workspace.SpawnLocation.CFrame + Vector3.new(0,3,0)
				end
			end)
		end)
	end)
end)
