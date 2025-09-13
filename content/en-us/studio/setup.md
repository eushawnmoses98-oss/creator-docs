-- LocalScript (place in StarterPlayerScripts)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Configuration
local colorEnergy = Color3.fromRGB(255, 0, 0) -- Change this to any color
local energyRadius = 5
local pulseSpeed = 2

-- Create the aura
local aura = Instance.new("Part")
aura.Name = "ColorEnergyAura"
aura.Size = Vector3.new(energyRadius*2, energyRadius*2, energyRadius*2)
aura.Transparency = 0.5
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = colorEnergy
aura.Parent = workspace

-- Update aura position
RunService.RenderStepped:Connect(function(time)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        -- Optional pulsing effect
        local scale = (math.sin(time * pulseSpeed) + 1)/2 + 0.5
        aura.Size = Vector3.new(energyRadius*2, energyRadius*2, energyRadius*2) * scale
        aura.Transparency = 0.5 - (scale/4)
    end
end)

-- Optional: Change color dynamically
local function setColor(newColor)
    colorEnergy = newColor
    aura.Color = colorEnergy
end

-- Example usage: setColor(Color3.fromRGB(0, 0, 255))
-- LocalScript (StarterPlayerScripts)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Color Energy Data
local ColorEnergy = {
    Red = {Color = Color3.fromRGB(255,0,0), DamageBoost = 1.2, SpeedBoost = 1},
    Blue = {Color = Color3.fromRGB(0,0,255), DamageBoost = 1, SpeedBoost = 1.2},
    Green = {Color = Color3.fromRGB(0,255,0), DamageBoost = 1, SpeedBoost = 1},
    -- Add more colors with abilities
}

local currentColor = "Red"
local energyAmount = 100
local maxEnergy = 100
local energyDrainRate = 5  -- per second when using ability

-- Create Aura
local aura = Instance.new("Part")
aura.Name = "ColorEnergyAura"
aura.Size = Vector3.new(5,5,5)
aura.Transparency = 0.5
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = ColorEnergy[currentColor].Color
aura.Parent = workspace

-- Function to switch color
local function switchColor(newColor)
    if ColorEnergy[newColor] then
        currentColor = newColor
        aura.Color = ColorEnergy[currentColor].Color
    end
end

-- Function to use energy ability
local function useEnergy(amount)
    if energyAmount >= amount then
        energyAmount = energyAmount - amount
        return true
    else
        return false
    end
end

-- Example: Recharge energy
local function rechargeEnergy(dt)
    energyAmount = math.min(maxEnergy, energyAmount + (10 * dt))
end

-- Aura and energy updates
RunService.RenderStepped:Connect(function(dt)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        local scale = (math.sin(tick() * 3) + 1)/2 + 0.5
        aura.Size = Vector3.new(5,5,5) * scale
        aura.Transparency = 0.5 - (scale/4)
    end
    rechargeEnergy(dt)
end)

-- Example key input for switching colors (optional)
local UserInputService = game:GetService("UserInputService")
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.One then
        switchColor("Red")
    elseif input.KeyCode == Enum.KeyCode.Two then
        switchColor("Blue")
    elseif input.KeyCode == Enum.KeyCode.Three then
        switchColor("Green")
    end
end)
-- LocalScript (StarterPlayerScripts)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- ===== Color Energy System =====
local ColorEnergyData = {
    Red = {Color = Color3.fromRGB(255,0,0), DamageBoost = 1.5, SpeedBoost = 1, Ability = "FireSlash"},
    Blue = {Color = Color3.fromRGB(0,0,255), DamageBoost = 1.2, SpeedBoost = 1.2, Ability = "WaterShield"},
    Green = {Color = Color3.fromRGB(0,255,0), DamageBoost = 1, SpeedBoost = 1, Ability = "HealingAura"},
    Silver = {Color = Color3.fromRGB(192,192,192), DamageBoost = 1.3, SpeedBoost = 1.1, Ability = "ReflectBarrier"},
    -- Add more colors based on story
}

local currentColor = "Red"
local energyAmount = 100
local maxEnergy = 100
local energyDrainRate = 20 -- per second when using ability

-- ===== Aura Setup =====
local aura = Instance.new("Part")
aura.Name = "ColorEnergyAura"
aura.Size = Vector3.new(5,5,5)
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = ColorEnergyData[currentColor].Color
aura.Transparency = 0.5
aura.Parent = workspace

-- ===== Functions =====
local function switchColor(newColor)
    if ColorEnergyData[newColor] then
        currentColor = newColor
        aura.Color = ColorEnergyData[currentColor].Color
        print("Switched to color:", currentColor)
    end
end

local function useAbility()
    if energyAmount >= energyDrainRate then
        energyAmount = energyAmount - energyDrainRate
        local ability = ColorEnergyData[currentColor].Ability
        print("Using ability:", ability)
        -- TODO: Implement ability effects (damage, shield, healing)
    else
        print("Not enough energy!")
    end
end

local function rechargeEnergy(dt)
    energyAmount = math.min(maxEnergy, energyAmount + (10 * dt))
end

-- ===== Aura Update =====
RunService.RenderStepped:Connect(function(dt)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        local pulse = (math.sin(tick() * 3) + 1)/2 + 0.5
        aura.Size = Vector3.new(5,5,5) * pulse
        aura.Transparency = 0.5 - (pulse/4)
    end
    rechargeEnergy(dt)
end)

-- ===== Input Controls =====
UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.One then
        switchColor("Red")
    elseif input.KeyCode == Enum.KeyCode.Two then
        switchColor("Blue")
    elseif input.KeyCode == Enum.KeyCode.Three then
        switchColor("Green")
    elseif input.KeyCode == Enum.KeyCode.Four then
        switchColor("Silver")
    elseif input.KeyCode == Enum.KeyCode.E then
        useAbility()
    end
end)
-- LocalScript (StarterPlayerScripts)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- ===== Color Energy Data =====
local ColorEnergyData = {
    Red = {Color = Color3.fromRGB(255,0,0), DamageBoost = 1.5, SpeedBoost = 1, Ability = "FireSlash", EnergyDrain = 20},
    Blue = {Color = Color3.fromRGB(0,0,255), DamageBoost = 1.2, SpeedBoost = 1.2, Ability = "WaterShield", EnergyDrain = 15},
    Green = {Color = Color3.fromRGB(0,255,0), DamageBoost = 1, SpeedBoost = 1, Ability = "HealingAura", EnergyDrain = 10},
    Silver = {Color = Color3.fromRGB(192,192,192), DamageBoost = 1.3, SpeedBoost = 1.1, Ability = "ReflectBarrier", EnergyDrain = 25},
    -- Add more colors if needed
}

local currentColor = "Red"
local maxEnergy = 100
local energyAmount = maxEnergy
local energyRechargeRate = 10 -- per second

-- ===== Aura Setup =====
local aura = Instance.new("Part")
aura.Name = "ColorEnergyAura"
aura.Size = Vector3.new(5,5,5)
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = ColorEnergyData[currentColor].Color
aura.Transparency = 0.5
aura.Parent = workspace

-- ===== Functions =====
local function switchColor(newColor)
    if ColorEnergyData[newColor] then
        currentColor = newColor
        aura.Color = ColorEnergyData[currentColor].Color
        print("Switched to color:", currentColor)
    end
end

local function useAbility()
    local data = ColorEnergyData[currentColor]
    if energyAmount >= data.EnergyDrain then
        energyAmount = energyAmount - data.EnergyDrain
        print("Using ability:", data.Ability)
        
        -- ===== Ability Effects =====
        if data.Ability == "FireSlash" then
            -- Example: Create a short red slash effect in front of player
            local slash = Instance.new("Part")
            slash.Size = Vector3.new(1,5,10)
            slash.CFrame = humanoidRootPart.CFrame * CFrame.new(0,0,-5)
            slash.Anchored = true
            slash.CanCollide = false
            slash.Material = Enum.Material.Neon
            slash.Color = data.Color
            slash.Parent = workspace
            game.Debris:AddItem(slash, 0.5)
            
        elseif data.Ability == "WaterShield" then
            -- Example: Create a blue shield around player
            local shield = Instance.new("Part")
            shield.Size = Vector3.new(6,6,6)
            shield.CFrame = humanoidRootPart.CFrame
            shield.Anchored = true
            shield.CanCollide = false
            shield.Material = Enum.Material.Neon
            shield.Color = data.Color
            shield.Transparency = 0.4
            shield.Parent = workspace
            game.Debris:AddItem(shield, 2)
            
        elseif data.Ability == "HealingAura" then
            -- Heal the player
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.Health = math.min(humanoid.MaxHealth, humanoid.Health + 30)
            end
            
        elseif data.Ability == "ReflectBarrier" then
            -- Example: temporary barrier effect
            local barrier = Instance.new("Part")
            barrier.Size = Vector3.new(7,7,7)
            barrier.CFrame = humanoidRootPart.CFrame
            barrier.Anchored = true
            barrier.CanCollide = false
            barrier.Material = Enum.Material.Neon
            barrier.Color = data.Color
            barrier.Transparency = 0.6
            barrier.Parent = workspace
            game.Debris:AddItem(barrier, 3)
        end
    else
        print("Not enough energy!")
    end
end

local function rechargeEnergy(dt)
    energyAmount = math.min(maxEnergy, energyAmount + energyRechargeRate * dt)
end

-- ===== Aura & Energy Update =====
RunService.RenderStepped:Connect(function(dt)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        local pulse = (math.sin(tick() * 3) + 1)/2 + 0.5
        aura.Size = Vector3.new(5,5,5) * pulse
        aura.Transparency = 0.5 - (pulse/4)
    end
    rechargeEnergy(dt)
end)

-- ===== Input Controls =====
UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.One then
        switchColor("Red")
    elseif input.KeyCode == Enum.KeyCode.Two then
        switchColor("Blue")
    elseif input.KeyCode == Enum.KeyCode.Three then
        switchColor("Green")
    elseif input.KeyCode == Enum.KeyCode.Four then
        switchColor("Silver")
    elseif input.KeyCode == Enum.KeyCode.E then
        useAbility()
    end
end)
-- LocalScript (StarterPlayerScripts)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- ===== Color Energy Data =====
local ColorEnergyData = {
    Red = {Color = Color3.fromRGB(255,0,0), DamageBoost = 1.5, SpeedBoost = 1, Ability = "FireSlash", EnergyDrain = 20, Level = 1, Exp = 0},
    Blue = {Color = Color3.fromRGB(0,0,255), DamageBoost = 1.2, SpeedBoost = 1.2, Ability = "WaterShield", EnergyDrain = 15, Level = 1, Exp = 0},
    Green = {Color = Color3.fromRGB(0,255,0), DamageBoost = 1, SpeedBoost = 1, Ability = "HealingAura", EnergyDrain = 10, Level = 1, Exp = 0},
    Silver = {Color = Color3.fromRGB(192,192,192), DamageBoost = 1.3, SpeedBoost = 1.1, Ability = "ReflectBarrier", EnergyDrain = 25, Level = 1, Exp = 0},
}

local currentColor = "Red"
local maxEnergy = 100
local energyAmount = maxEnergy
local energyRechargeRate = 10 -- per second

-- ===== Aura Setup =====
local aura = Instance.new("Part")
aura.Name = "ColorEnergyAura"
aura.Size = Vector3.new(5,5,5)
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = ColorEnergyData[currentColor].Color
aura.Transparency = 0.5
aura.Parent = workspace

-- ===== UI Setup =====
local PlayerGui = player:WaitForChild("PlayerGui")
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ColorEnergyUI"
screenGui.Parent = PlayerGui

local energyBar = Instance.new("Frame")
energyBar.Size = UDim2.new(0.3,0,0.03,0)
energyBar.Position = UDim2.new(0.35,0,0.9,0)
energyBar.BackgroundColor3 = Color3.fromRGB(50,50,50)
energyBar.BorderSizePixel = 2
energyBar.Parent = screenGui

local energyFill = Instance.new("Frame")
energyFill.Size = UDim2.new(1,0,1,0)
energyFill.BackgroundColor3 = ColorEnergyData[currentColor].Color
energyFill.BorderSizePixel = 0
energyFill.Parent = energyBar

local colorLabel = Instance.new("TextLabel")
colorLabel.Size = UDim2.new(0.3,0,0.03,0)
colorLabel.Position = UDim2.new(0.35,0,0.87,0)
colorLabel.BackgroundTransparency = 1
colorLabel.TextColor3 = ColorEnergyData[currentColor].Color
colorLabel.Text = currentColor.." Lv."..ColorEnergyData[currentColor].Level
colorLabel.Font = Enum.Font.SourceSansBold
colorLabel.TextScaled = true
colorLabel.Parent = screenGui

-- ===== Functions =====
local function switchColor(newColor)
    if ColorEnergyData[newColor] then
        currentColor = newColor
        aura.Color = ColorEnergyData[currentColor].Color
        energyFill.BackgroundColor3 = ColorEnergyData[currentColor].Color
        colorLabel.TextColor3 = ColorEnergyData[currentColor].Color
        colorLabel.Text = currentColor.." Lv."..ColorEnergyData[currentColor].Level
    end
end

local function gainExp(color, amount)
    local data = ColorEnergyData[color]
    data.Exp = data.Exp + amount
    if data.Exp >= data.Level * 100 then
        data.Exp = data.Exp - data.Level * 100
        data.Level = data.Level + 1
        print(color.." leveled up! Now Lv."..data.Level)
        colorLabel.Text = currentColor.." Lv."..ColorEnergyData[currentColor].Level
    end
end

local function useAbility()
    local data = ColorEnergyData[currentColor]
    if energyAmount >= data.EnergyDrain then
        energyAmount = energyAmount - data.EnergyDrain
        print("Using ability:", data.Ability)
        
        -- Ability effects (can expand with animations & combos)
        if data.Ability == "FireSlash" then
            local slash = Instance.new("Part")
            slash.Size = Vector3.new(1,5,10)
            slash.CFrame = humanoidRootPart.CFrame * CFrame.new(0,0,-5)
            slash.Anchored = true
            slash.CanCollide = false
            slash.Material = Enum.Material.Neon
            slash.Color = data.Color
            slash.Parent = workspace
            game.Debris:AddItem(slash, 0.5)
            
        elseif data.Ability == "WaterShield" then
            local shield = Instance.new("Part")
            shield.Size = Vector3.new(6,6,6)
            shield.CFrame = humanoidRootPart.CFrame
            shield.Anchored = true
            shield.CanCollide = false
            shield.Material = Enum.Material.Neon
            shield.Color = data.Color
            shield.Transparency = 0.4
            shield.Parent = workspace
            game.Debris:AddItem(shield, 2)
            
        elseif data.Ability == "HealingAura" then
            humanoid.Health = math.min(humanoid.MaxHealth, humanoid.Health + 30)
            
        elseif data.Ability == "ReflectBarrier" then
            local barrier = Instance.new("Part")
            barrier.Size = Vector3.new(7,7,7)
            barrier.CFrame = humanoidRootPart.CFrame
            barrier.Anchored = true
            barrier.CanCollide = false
            barrier.Material = Enum.Material.Neon
            barrier.Color = data.Color
            barrier.Transparency = 0.6
            barrier.Parent = workspace
            game.Debris:AddItem(barrier, 3)
        end
        
        -- Gain exp for using ability
        gainExp(currentColor, 20)
        
    else
        print("Not enough energy!")
    end
end

local function rechargeEnergy(dt)
    energyAmount = math.min(maxEnergy, energyAmount + energyRechargeRate * dt)
end

-- ===== Aura & Energy Update =====
RunService.RenderStepped:Connect(function(dt)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        local pulse = (math.sin(tick() * 3) + 1)/2 + 0.5
        aura.Size = Vector3.new(5,5,5) * pulse
        aura.Transparency = 0.5 - (pulse/4)
    end
    rechargeEnergy(dt)
    energyFill.Size = UDim2.new(energyAmount/maxEnergy,0,1,0)
end)

-- ===== Input Controls =====
UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.One then
        switchColor("Red")
    elseif input.KeyCode == Enum.KeyCode.Two then
        switchColor("Blue")
    elseif input.KeyCode == Enum.KeyCode.Three then
        switchColor("Green")
    elseif input.KeyCode == Enum.KeyCode.Four then
        switchColor("Silver")
    elseif input.KeyCode == Enum.KeyCode.E then
        useAbility()
    end
end)
-- LocalScript (StarterPlayerScripts)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- ===== Color Energy Data with Combos =====
local ColorEnergyData = {
    Red = {Color = Color3.fromRGB(255,0,0), Ability = "FireSlash", EnergyDrain = 20, Level=1, Exp=0},
    Blue = {Color = Color3.fromRGB(0,0,255), Ability = "WaterShield", EnergyDrain = 15, Level=1, Exp=0},
    Green = {Color = Color3.fromRGB(0,255,0), Ability = "HealingAura", EnergyDrain = 10, Level=1, Exp=0},
    Silver = {Color = Color3.fromRGB(192,192,192), Ability = "ReflectBarrier", EnergyDrain = 25, Level=1, Exp=0},
}

local currentColor = "Red"
local maxEnergy = 100
local energyAmount = maxEnergy
local energyRechargeRate = 10 -- per second

-- ===== Aura Setup =====
local aura = Instance.new("Part")
aura.Size = Vector3.new(5,5,5)
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = ColorEnergyData[currentColor].Color
aura.Transparency = 0.5
aura.Parent = workspace

-- ===== Combo System =====
local comboQueue = {}
local comboResetTime = 1.2 -- seconds to reset combo
local comboTimer = 0

local function addToCombo(ability)
    table.insert(comboQueue, ability)
    comboTimer = comboResetTime
end

local function executeCombo()
    if #comboQueue == 2 then
        if comboQueue[1] == "FireSlash" and comboQueue[2] == "WaterShield" then
            -- Example combo: Fire + Water = SteamBlast
            local effect = Instance.new("Part")
            effect.Size = Vector3.new(8,8,8)
            effect.CFrame = humanoidRootPart.CFrame * CFrame.new(0,0,-5)
            effect.Anchored = true
            effect.CanCollide = false
            effect.Material = Enum.Material.Neon
            effect.Color = Color3.fromRGB(255,128,0)
            effect.Parent = workspace
            game.Debris:AddItem(effect, 1)
            print("Combo Executed: SteamBlast!")
        end
    end
    comboQueue = {}
end

-- ===== Ability Usage =====
local function useAbility(hold=false)
    local data = ColorEnergyData[currentColor]
    if energyAmount >= data.EnergyDrain then
        if hold then
            -- Charged ability
            energyAmount = energyAmount - data.EnergyDrain*2
            print("Charged Ability:", data.Ability.." (Power Boost)")
        else
            energyAmount = energyAmount - data.EnergyDrain
            print("Using Ability:", data.Ability)
        end
        
        -- Add to combo system
        addToCombo(data.Ability)
        
        -- Simple ability visual
        local part = Instance.new("Part")
        part.Size = Vector3.new(1,5,10)
        part.CFrame = humanoidRootPart.CFrame * CFrame.new(0,0,-5)
        part.Anchored = true
        part.CanCollide = false
        part.Material = Enum.Material.Neon
        part.Color = data.Color
        part.Parent = workspace
        game.Debris:AddItem(part,0.5)
    else
        print("Not enough energy!")
    end
end

-- ===== Energy Recharge =====
local function rechargeEnergy(dt)
    energyAmount = math.min(maxEnergy, energyAmount + energyRechargeRate * dt)
end

-- ===== Aura & Energy Update =====
RunService.RenderStepped:Connect(function(dt)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        local pulse = (math.sin(tick()*3)+1)/2 +0.5
        aura.Size = Vector3.new(5,5,5) * pulse
        aura.Transparency = 0.5 - pulse/4
    end
    rechargeEnergy(dt)
    
    -- Combo Timer
    if comboTimer > 0 then
        comboTimer = comboTimer - dt
    else
        if #comboQueue > 0 then
            executeCombo()
        end
    end
end)

-- ===== Input Controls =====
local holdingKey = false
local holdStartTime = 0
local holdThreshold = 1.0 -- seconds to trigger charged ability

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.One then currentColor="Red"; aura.Color=ColorEnergyData.Red.Color end
    if input.KeyCode == Enum.KeyCode.Two then currentColor="Blue"; aura.Color=ColorEnergyData.Blue.Color end
    if input.KeyCode == Enum.KeyCode.Three then currentColor="Green"; aura.Color=ColorEnergyData.Green.Color end
    if input.KeyCode == Enum.KeyCode.Four then currentColor="Silver"; aura.Color=ColorEnergyData.Silver.Color end
    
    if input.KeyCode == Enum.KeyCode.E then
        holdingKey = true
        holdStartTime = tick()
    end
end)

UserInputService.InputEnded:Connect(function(input, processed)
    if input.KeyCode == Enum.KeyCode.E and holdingKey then
        holdingKey = false
        local heldTime = tick() - holdStartTime
        if heldTime >= holdThreshold then
            useAbility(true) -- Charged Ability
        else
            useAbility(false) -- Normal Ability
        end
    end
end)
-- LocalScript (StarterPlayerScripts)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- ===== Color Energy Data =====
local ColorEnergyData = {
    Red = {Color = Color3.fromRGB(255,0,0), Ability = "FireSlash", EnergyDrain = 20, Cooldown = 1.5},
    Blue = {Color = Color3.fromRGB(0,0,255), Ability = "WaterShield", EnergyDrain = 15, Cooldown = 2},
    Green = {Color = Color3.fromRGB(0,255,0), Ability = "HealingAura", EnergyDrain = 10, Cooldown = 3},
    Silver = {Color = Color3.fromRGB(192,192,192), Ability = "ReflectBarrier", EnergyDrain = 25, Cooldown = 4},
}

local currentColor = "Red"
local maxEnergy = 100
local energyAmount = maxEnergy
local energyRechargeRate = 10
local cooldownTimers = {} -- track cooldowns per ability

-- ===== Aura Setup =====
local aura = Instance.new("Part")
aura.Size = Vector3.new(5,5,5)
aura.Anchored = true
aura.CanCollide = false
aura.Material = Enum.Material.Neon
aura.Color = ColorEnergyData[currentColor].Color
aura.Transparency = 0.5
aura.Parent = Workspace

-- ===== Ability System =====
local function useAbility(charged)
    local data = ColorEnergyData[currentColor]
    if cooldownTimers[data.Ability] and cooldownTimers[data.Ability] > 0 then
        print(data.Ability.." is on cooldown!")
        return
    end
    
    if energyAmount >= data.EnergyDrain then
        energyAmount = energyAmount - data.EnergyDrain
        print("Used Ability:", data.Ability, charged and "(Charged)" or "")
        
        -- Ability visuals
        local effect = Instance.new("Part")
        effect.Size = Vector3.new(1,5,10)
        if charged then
            effect.Size = effect.Size * 1.5
        end
        effect.CFrame = humanoidRootPart.CFrame * CFrame.new(0,0,-5)
        effect.Anchored = true
        effect.CanCollide = false
        effect.Material = Enum.Material.Neon
        effect.Color = data.Color
        effect.Parent = Workspace
        game.Debris:AddItem(effect, 0.5)
        
        -- Cooldown start
        cooldownTimers[data.Ability] = data.Cooldown
    else
        print("Not enough energy!")
    end
end

-- ===== Combo System =====
local comboQueue = {}
local comboResetTime = 1.2
local comboTimer = 0

local function addToCombo(ability)
    table.insert(comboQueue, ability)
    comboTimer = comboResetTime
end

local function executeCombo()
    if #comboQueue == 2 then
        -- Example: Red + Blue = SteamBlast
        if comboQueue[1]=="FireSlash" and comboQueue[2]=="WaterShield" then
            print("Combo Activated: SteamBlast!")
            local effect = Instance.new("Part")
            effect.Size = Vector3.new(8,8,8)
            effect.CFrame = humanoidRootPart.CFrame * CFrame.new(0,0,-5)
            effect.Anchored = true
            effect.CanCollide = false
            effect.Material = Enum.Material.Neon
            effect.Color = Color3.fromRGB(255,128,0)
            effect.Parent = Workspace
            game.Debris:AddItem(effect,1)
        end
    end
    comboQueue = {}
end

-- ===== Input Controls =====
local holdingKey = false
local holdStartTime = 0
local holdThreshold = 1.0

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.One then currentColor="Red"; aura.Color=ColorEnergyData.Red.Color end
    if input.KeyCode == Enum.KeyCode.Two then currentColor="Blue"; aura.Color=ColorEnergyData.Blue.Color end
    if input.KeyCode == Enum.KeyCode.Three then currentColor="Green"; aura.Color=ColorEnergyData.Green.Color end
    if input.KeyCode == Enum.KeyCode.Four then currentColor="Silver"; aura.Color=ColorEnergyData.Silver.Color end
    
    if input.KeyCode == Enum.KeyCode.E then
        holdingKey = true
        holdStartTime = tick()
    end
end)

UserInputService.InputEnded:Connect(function(input, processed)
    if input.KeyCode==Enum.KeyCode.E and holdingKey then
        holdingKey = false
        local heldTime = tick() - holdStartTime
        local charged = heldTime >= holdThreshold
        useAbility(charged)
        addToCombo(ColorEnergyData[currentColor].Ability)
    end
end)

-- ===== Update Loop =====
RunService.RenderStepped:Connect(function(dt)
    if humanoidRootPart then
        aura.Position = humanoidRootPart.Position
        local pulse = (math.sin(tick()*3)+1)/2 +0.5
        aura.Size = Vector3.new(5,5,5)*pulse
        aura.Transparency = 0.5 - pulse/4
    end
    
    -- Recharge energy
    energyAmount = math.min(maxEnergy, energyAmount + energyRechargeRate*dt)
    
    -- Update cooldowns
    for ability,timer in pairs(cooldownTimers) do
        if timer>0 then
            cooldownTimers[ability]=math.max(0,timer-dt)
        end
    end
    
    -- Combo timer
    if comboTimer>0 then
        comboTimer = comboTimer - dt
    else
        if #comboQueue>0 then executeCombo() end
    end
end)

-- ===== PvE Enemy Example =====
local function spawnEnemy(position)
    local enemy = Instance.new("Part")
    enemy.Size = Vector3.new(4,6,4)
    enemy.Position = position
    enemy.Anchored = false
    enemy.Material = Enum.Material.SmoothPlastic
    enemy.Color = Color3.fromRGB(80,80,80)
    enemy.Name = "Enemy"
    enemy.Parent = Workspace
    
    local enemyHumanoid = Instance.new("Humanoid")
    enemyHumanoid.Health = 100
    enemyHumanoid.MaxHealth = 100
    enemyHumanoid.Parent = enemy
    
    return enemy
end

-- Example: spawn an enemy
spawnEnemy(humanoidRootPart.Position + Vector3.new(0,0,-15))

