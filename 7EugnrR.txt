--[[ Deobfuscated by MLRTAKEN | Full Source Leak ]]

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local StarterGui = game:GetService("StarterGui")
local Lighting = game:GetService("Lighting")

local hwid = game:GetService("RbxAnalyticsService"):GetClientId()
local CheckHWID = loadstring(game:HttpGet("https://raw.githubusercontent.com/CAXAP26BKyCH/whitelist/refs/heads/main/booster"))().CheckHWID
if not CheckHWID(hwid) then
    LocalPlayer:Kick("U are not whitlist!!!")
    return
end

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/CAXAP26BKyCH/library/refs/heads/main/Uilibrary"))()
local Window = Library:AddWindow("Doca V1 (Server Booster Version)", {
    main_color = Color3.fromRGB(255, 0, 0),
    min_size = Vector2.new(500, 600),
    can_resize = true
})

local MainTab = Window:AddTab("Main")
local LocalPlayersFolder = MainTab:AddFolder("Local Players")
LocalPlayersFolder:AddSwitch("Auto Eat Protein Egg Every 30 Minutes", function(state)
    getgenv().autoEatProteinEggActive = state
    task.spawn(function()
        while getgenv().autoEatProteinEggActive and LocalPlayer.Character do
            local egg = LocalPlayer.Backpack:FindFirstChild("Protein Egg") or LocalPlayer.Character:FindFirstChild("Protein Egg")
            if egg then
                egg.Parent = LocalPlayer.Character
                ReplicatedStorage.muscleEvent:FireServer("rep")
            end
            task.wait(1800)
        end
    end)
end)
LocalPlayersFolder:AddSwitch("Auto Eat Protein Egg Every 1 hour", function(state)
    getgenv().autoEatProteinEggHourly = state
    task.spawn(function()
        while getgenv().autoEatProteinEggHourly and LocalPlayer.Character do
            local egg = LocalPlayer.Backpack:FindFirstChild("Protein Egg") or LocalPlayer.Character:FindFirstChild("Protein Egg")
            if egg then
                egg.Parent = LocalPlayer.Character
                ReplicatedStorage.muscleEvent:FireServer("rep")
            end
            task.wait(3600)
        end
    end)
end)
local MiscFolder = MainTab:AddFolder("Misc")
MiscFolder:AddSwitch("Auto Farm (Equip Any tool)", function(state)
    getgenv().autoFarm = state
    task.spawn(function()
        while getgenv().autoFarm and LocalPlayer.Character do
            for _, tool in pairs(LocalPlayer.Backpack:GetChildren()) do
                if tool:IsA("Tool") then
                    tool.Parent = LocalPlayer.Character
                    ReplicatedStorage.muscleEvent:FireServer("rep")
                    task.wait(0.1)
                end
            end
            task.wait()
        end
    end)
end)
MiscFolder:AddLabel("---Script Hub---")
MiscFolder:AddButton("Permanent ShiftLock", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/MiniNoobie/ShiftLockx/main/Shiftlock-MiniNoobie"))()
end)
MiscFolder:AddLabel("---Time---")
MiscFolder:AddButton("Night", function()
    Lighting.ClockTime = 0
end)
MiscFolder:AddButton("Morning", function()
    Lighting.ClockTime = 6
end)
MiscFolder:AddButton("Day", function()
    Lighting.ClockTime = 12
end)
MiscFolder:AddLabel("----Farming----")
local AutoBrawlFolder = MainTab:AddFolder("Auto Brawl")
AutoBrawlFolder:AddSwitch("Auto Win Brawl", function(state)
    getgenv().autoWinBrawl = state
    task.spawn(function()
        while getgenv().autoWinBrawl and LocalPlayer.Character do
            ReplicatedStorage.rEvents.joinBrawl:FireServer("Win")
            task.wait(1)
        end
    end)
end)
AutoBrawlFolder:AddSwitch("Auto Join Brawl (For Farming)", function(state)
    getgenv().autoBrawlFarm = state
    task.spawn(function()
        while getgenv().autoBrawlFarm and LocalPlayer.Character do
            ReplicatedStorage.rEvents.joinBrawl:FireServer("Farm")
            task.wait(1)
        end
    end)
end)
local OpStuffFolder = MainTab:AddFolder("Op stuff (for farm)")
OpStuffFolder:AddSwitch("Anti Knockback", function(state)
    getgenv().antiKnockback = state
    LocalPlayer.CharacterAdded:Connect(function(char)
        if getgenv().antiKnockback then
            char.HumanoidRootPart.Anchored = true
            task.wait(0.1)
            char.HumanoidRootPart.Anchored = false
        end
    end)
end)
OpStuffFolder:AddSwitch("Auto Pushups with Rock (10M) and Auto Punch", function(state)
    getgenv().autoPushups10M = state
    task.spawn(function()
        while getgenv().autoPushups10M and LocalPlayer.Character do
            local punch = LocalPlayer.Backpack:FindFirstChild("Pushups") or LocalPlayer.Character:FindFirstChild("Pushups")
            if punch then
                punch.Parent = LocalPlayer.Character
                ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                local rock = Workspace.machinesFolder:FindFirstChild("King Rock")
                if rock and LocalPlayer:FindFirstChild("Durability") and LocalPlayer.Durability.Value >= 10000000 then
                    firetouchinterest(rock.Rock, LocalPlayer.Character.RightHand, 0)
                    firetouchinterest(rock.Rock, LocalPlayer.Character.LeftHand, 0)
                    task.wait()
                    firetouchinterest(rock.Rock, LocalPlayer.Character.RightHand, 1)
                    firetouchinterest(rock.Rock, LocalPlayer.Character.LeftHand, 1)
                end
            end
            task.wait(0.1)
        end
    end)
end)
OpStuffFolder:AddSwitch("Free AutoLift Gamepass", function(state)
    getgenv().autoLiftGamepass = state
    task.spawn(function()
        while getgenv().autoLiftGamepass and LocalPlayer.Character do
            ReplicatedStorage.rEvents.autoLiftGamepass:FireServer("Enable")
            task.wait(1)
        end
    end)
end)
OpStuffFolder:AddSwitch("Walk on Water", function(state)
    getgenv().walkOnWater = state
    if state then
        local parts = {
            {name = "Part_Side_", size = Vector3.new(1000, 1, 1000), position = Vector3.new(0, 0, 0)},
            {name = "Part_LeftRight_", size = Vector3.new(1000, 1, 1000), position = Vector3.new(0, 0, 0)},
            {name = "Part_UpLeft_", size = Vector3.new(1000, 1, 1000), position = Vector3.new(0, 0, 0)},
            {name = "Part_UpRight_", size = Vector3.new(1000, 1, 1000), position = Vector3.new(0, 0, 0)}
        }
        for i, partData in ipairs(parts) do
            local part = Instance.new("Part")
            part.Name = partData.name .. i
            part.Size = partData.size
            part.Position = partData.position
            part.Anchored = true
            part.Transparency = 0.5
            part.CanCollide = true
            part.Parent = Workspace
        end
    else
        for _, part in ipairs(Workspace:GetChildren()) do
            if part.Name:match("Part_Side_") or part.Name:match("Part_LeftRight_") or part.Name:match("Part_UpLeft_") or part.Name:match("Part_UpRight_") then
                part:Destroy()
            end
        end
    end
end)
OpStuffFolder:AddButton("Remove Ad Portal", function()
    for _, portal in pairs(Workspace:GetDescendants()) do
        if portal.Name:match("AdPortal") then
            portal:Destroy()
        end
    end
end)

local KillingTab = Window:AddTab("Killing")
KillingTab:AddSwitch("Auto Equip Punch", function(state)
    getgenv().autoEquipPunch = state
    task.spawn(function()
        while getgenv().autoEquipPunch and LocalPlayer.Character do
            local punch = LocalPlayer.Backpack:FindFirstChild("Punch") or LocalPlayer.Character:FindFirstChild("Punch")
            if punch then
                punch.Parent = LocalPlayer.Character
            else
                LocalPlayer.Character.Humanoid:UnequipTools()
                local backpackPunch = LocalPlayer.Backpack:FindFirstChild("Punch")
                if backpackPunch then
                    backpackPunch.Parent = LocalPlayer.Character
                end
            end
            task.wait(0.5)
        end
    end)
end)
KillingTab:AddSwitch("Auto Punch {With Movement}", function(state)
    getgenv().autoPunchMove = state
    task.spawn(function()
        while getgenv().autoPunchMove and LocalPlayer.Character do
            local punch = LocalPlayer.Character:FindFirstChild("Punch")
            if punch then
                ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                LocalPlayer.Character.HumanoidRootPart.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame + LocalPlayer.Character.Humanoid.MoveDirection * 0.5
            end
            task.wait(0.1)
        end
    end)
end)
KillingTab:AddSwitch("Auto Punch", function(state)
    getgenv().autoPunch = state
    task.spawn(function()
        while getgenv().autoPunch and LocalPlayer.Character do
            local punch = LocalPlayer.Character:FindFirstChild("Punch")
            if punch then
                ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
            end
            task.wait(0.1)
        end
    end)
end)
KillingTab:AddSwitch("Unlock Fast Punch", function(state)
    getgenv().fastPunch = state
    task.spawn(function()
        while getgenv().fastPunch and LocalPlayer.Character do
            local punch = LocalPlayer.Character:FindFirstChild("Punch")
            if punch then
                ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
            end
            task.wait(0.05)
        end
    end)
end)
KillingTab:AddTextBox("Whitelist Player", function(name)
    getgenv().whitelist = getgenv().whitelist or {}
    table.insert(getgenv().whitelist, name)
end)
KillingTab:AddButton("Clear Whitelist", function()
    getgenv().whitelist = {}
end)
KillingTab:AddSwitch("Auto Kill", function(state)
    getgenv().autokill = state
    task.spawn(function()
        while getgenv().autokill and LocalPlayer.Character do
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and not table.find(getgenv().whitelist or {}, player.Name) then
                    if player.Character and player.Character.HumanoidRootPart then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position)
                        ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                        ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                    end
                end
            end
            task.wait(0.1)
        end
    end)
end)
KillingTab:AddSwitch("Auto Kill Players", function(state)
    getgenv().autoKillPlayers = state
    local function onPlayerAdded(player)
        if getgenv().autoKillPlayers and player ~= LocalPlayer and not table.find(getgenv().whitelist or {}, player.Name) then
            task.spawn(function()
                while getgenv().autoKillPlayers and player.Character and LocalPlayer.Character do
                    if player.Character.HumanoidRootPart then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position)
                        ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                        ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                    end
                    task.wait(0.1)
                end
            end)
        end
    end
    Players.PlayerAdded:Connect(onPlayerAdded)
    Players.PlayerRemoving:Connect(function(player)
        if getgenv().autoKillPlayers then
            onPlayerAdded(player)
        end
    end)
end)
KillingTab:AddDropdown("Players", function()
    local playerNames = {}
    for _, player in ipairs(Players:GetPlayers()) do
        table.insert(playerNames, player.Name)
    end
    return playerNames
end)
KillingTab:AddTextBox("Kill Player", function(name)
    local target = Players:FindFirstChild(name)
    if target and target.Character and LocalPlayer.Character then
        task.spawn(function()
            while target.Character and LocalPlayer.Character do
                LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(target.Character.HumanoidRootPart.Position)
                ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                task.wait(0.1)
            end
        end)
    else
        warn("Player not found: " .. name)
    end
end)
KillingTab:AddLabel("---------------")
KillingTab:AddTextBox("View Player", function(name)
    local target = Players:FindFirstChild(name)
    if target and target.Character then
        Workspace.CurrentCamera.CameraSubject = target.Character.Humanoid
    else
        warn("Player not found: " .. name)
    end
end)
KillingTab:AddButton("Unview Player", function()
    if LocalPlayer.Character then
        Workspace.CurrentCamera.CameraSubject = LocalPlayer.Character.Humanoid
    end
end)

local StatsTab = Window:AddTab("Stats")
local StatsFolder = StatsTab:AddFolder("Stats")
StatsFolder:AddButton("Show Kills Gui", function()
    local leaderstats = LocalPlayer:FindFirstChild("leaderstats")
    if leaderstats then
        local killsLabel = StatsFolder:AddLabel("Kills: " .. (leaderstats.Kills and leaderstats.Kills.Value or 0))
        local strengthLabel = StatsFolder:AddLabel("Strength: " .. (leaderstats.Strength and leaderstats.Strength.Value or 0))
        local durabilityLabel = StatsFolder:AddLabel("Durability: " .. (leaderstats.Durability and leaderstats.Durability.Value or 0))
        if leaderstats.Kills then
            leaderstats.Kills.Changed:Connect(function(value)
                killsLabel.Text = "Kills: " .. value
            end)
        end
        if leaderstats.Strength then
            leaderstats.Strength.Changed:Connect(function(value)
                strengthLabel.Text = "Strength: " .. math.ceil(value)
            end)
        end
        if leaderstats.Durability then
            leaderstats.Durability.Changed:Connect(function(value)
                durabilityLabel.Text = "Durability: " .. value
            end)
        end
        LocalPlayer.CharacterAdded:Connect(function()
            killsLabel:Destroy()
            strengthLabel:Destroy()
            durabilityLabel:Destroy()
        end)
        LocalPlayer.CharacterRemoving:Connect(function()
            killsLabel:Destroy()
            strengthLabel:Destroy()
            durabilityLabel:Destroy()
        end)
    end
end)
StatsTab:AddFolder("Stats Gained")
Players.PlayerAdded:Connect(function() end)
Players.PlayerRemoving:Connect(function() end)

local FarmPlusTab = Window:AddTab("Farm++")
local AutoGymFolder = FarmPlusTab:AddFolder("Auto Gym")
AutoGymFolder:AddLabel("Jungle Gym")
local jungleGymMachines = {
    ["Jungle Bench"] = "autoJungleBench",
    ["Jungle Bar Lift"] = "autoJungleBarLift",
    ["Jungle Boulder"] = "autoJungleBoulder",
    ["Jungle Squat"] = "autoJungleSquat"
}
for machineName, varName in pairs(jungleGymMachines) do
    AutoGymFolder:AddSwitch("Auto " .. machineName, function(state)
        getgenv()[varName] = state
        task.spawn(function()
            while getgenv()[varName] and LocalPlayer.Character do
                local machine = Workspace.machinesFolder:FindFirstChild(machineName)
                if machine then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(machine.interactSeat.Position)
                    ReplicatedStorage.rEvents.machineInteractRemote:InvokeServer("useMachine", machine)
                    ReplicatedStorage.muscleEvent:FireServer("rep")
                end
                task.wait(0.1)
            end
        end)
    end)
end)
local AutoEquipFolder = FarmPlusTab:AddFolder("Auto Equip Weight Tools")
local toolSwitches = {
    ["Auto Handstands"] = "Handstands",
    ["Auto Weight"] = "Weight",
    ["Auto Pushups"] = "Pushups",
    ["Auto Situps"] = "Situps"
}
for label, toolName in pairs(toolSwitches) do
    AutoEquipFolder:AddSwitch(label, function(state)
        getgenv()[label] = state
        task.spawn(function()
            while getgenv()[label] and LocalPlayer.Character do
                local tool = LocalPlayer.Backpack:FindFirstChild(toolName) or LocalPlayer.Character:FindFirstChild(toolName)
                if tool then
                    tool.Parent = LocalPlayer.Character
                    ReplicatedStorage.muscleEvent:FireServer("rep")
                end
                task.wait(0.5)
            end
        end)
    end)
end)

local EggsTab = Window:AddTab("Eggs")
EggsTab:AddTextBox("Select Pet", function(petName)
    getgenv().selectedPet = petName
end)
EggsTab:AddSwitch("Auto Buy Pet", function(state)
    getgenv().autoBuyPet = state
    task.spawn(function()
        while getgenv().autoBuyPet and LocalPlayer.Character do
            if getgenv().selectedPet then
                ReplicatedStorage.rEvents.equipPetEvent:FireServer("unequipPet")
                task.wait(0.1)
                ReplicatedStorage.rEvents.buyPetEvent:FireServer(getgenv().selectedPet)
            end
            task.wait(1)
        end
    end)
end)
local crystals = {
    "Blue Crystal", "Green Crystal", "Frozen Crystal", "Mythical Crystal", "Inferno Crystal",
    "Legends Crystal", "Dark Nebula Crystal", "Muscle Elite Crystal", "Galaxy Oracle Crystal",
    "Battle Legends Crystal", "Sky Eclipse Crystal", "Jungle Crystal"
}
EggsTab:AddDropdown("Select Crystal", crystals, function(crystal)
    getgenv().crystal = crystal
end)
EggsTab:AddSwitch("Auto Hatch Crystal", function(state)
    getgenv().autoHatchCrystal = state
    task.spawn(function()
        while getgenv().autoHatchCrystal and LocalPlayer.Character do
            if getgenv().crystal then
                ReplicatedStorage.rEvents.openCrystalRemote:InvokeServer("openCrystal", getgenv().crystal)
            end
            task.wait(1)
        end
    end)
end)

local PlayersTab = Window:AddTab("Players")
PlayersTab:AddTextBox("Walkspeed", function(value)
    getgenv().ws = tonumber(value)
end)
PlayersTab:AddSwitch("Set Walkspeed", function(state)
    getgenv().setws = state
    task.spawn(function()
        while getgenv().setws and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") do
            local ws = getgenv().ws or 16
            LocalPlayer.Character.Humanoid.WalkSpeed = ws
            task.wait()
        end
    end)
end)
PlayersTab:AddTextBox("JumpPower", function(value)
    getgenv().jumpPower = tonumber(value)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = getgenv().jumpPower or 50
    end
end)
PlayersTab:AddTextBox("HipHeight", function(value)
    getgenv().hipHeight = tonumber(value)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.HipHeight = getgenv().hipHeight or 0
    end
end)
PlayersTab:AddTextBox("Max Zoom Distance", function(value)
    LocalPlayer.CameraMaxZoomDistance = tonumber(value) or 128
end)
PlayersTab:AddLabel("--------")
PlayersTab:AddSwitch("Lock Client Position", function(state)
    getgenv().lockPosition = state
    if state then
        getgenv().lockedPos = LocalPlayer.Character.HumanoidRootPart.CFrame
        getgenv().lockConnection = RunService.Heartbeat:Connect(function()
            if LocalPlayer.Character and LocalPlayer.Character.HumanoidRootPart then
                LocalPlayer.Character.HumanoidRootPart.CFrame = getgenv().lockedPos
            end
        end)
    else
        if getgenv().lockConnection then
            getgenv().lockConnection:Disconnect()
        end
    end
end)
PlayersTab:AddButton("Remove Punch", function()
    local punch = LocalPlayer.Backpack:FindFirstChild("Punch") or LocalPlayer.Character:FindFirstChild("Punch")
    if punch then
        punch:Destroy()
    end
end)
PlayersTab:AddButton("Recover Punch", function()
    ReplicatedStorage.rEvents.giveTool:FireServer("Punch")
end)
PlayersTab:AddSwitch("Infinite Jump", function(state)
    getgenv().infiniteJump = state
    game:GetService("UserInputService").JumpRequest:Connect(function()
        if getgenv().infiniteJump and LocalPlayer.Character then
            LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end)
end)
PlayersTab:AddSwitch("Noclip", function(state)
    getgenv().noclip = state
    if state then
        StarterGui:SetCore("SendNotification", {Title = "Player", Text = "Noclip enabled", Duration = 5})
        getgenv().noclipConnection = RunService.Stepped:Connect(function()
            if LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        StarterGui:SetCore("SendNotification", {Title = "Player", Text = "Noclip disabled", Duration = 5})
        if getgenv().noclipConnection then
            getgenv().noclipConnection:Disconnect()
        end
    end
end)
PlayersTab:AddButton("Anti AFK", function()
    game:GetService("VirtualUser").CaptureController:ClickButton2(Vector2.new())
    game:GetService("VirtualUser").Idled:Connect(function()
        game:GetService("VirtualUser").CaptureController:ClickButton2(Vector2.new())
    end)
end)
PlayersTab:AddButton("Anti Lag", function()
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("BasePart") and not v.Parent:IsA("Model") then
            v.Material = Enum.Material.SmoothPlastic
            v.Reflectance = 0
        end
    end
end)
PlayersTab:AddButton("ChatSpy", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/CAXAP26BKyCH/ChatSpy/main/ChatSpy"))()
end)

local CreditsTab = Window:AddTab("Credits")
CreditsTab:AddLabel("This Script made by Doca")
CreditsTab:AddLabel("Roblox: Xx_GPWArka")
CreditsTab:AddLabel("Discord: itsdocas_60003")
CreditsTab:AddButton("Copy Discord Invite link", function()
    setclipboard("https://discord.gg/itsdocas_60003")
end)

local BoostTab = Window:AddTab("Boost Tab")
BoostTab:AddSwitch("Auto Kill Good Karma", function(state)
    getgenv().autoKillGoodKarma = state
    task.spawn(function()
        while getgenv().autoKillGoodKarma and LocalPlayer.Character do
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Karma") and player.Character.Karma.Value > 0 then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position)
                    ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                    ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                end
            end
            task.wait(0.1)
        end
    end)
end)
BoostTab:AddSwitch("Auto Kill Evil Karma", function(state)
    getgenv().autoKillEvilKarma = state
    task.spawn(function()
        while getgenv().autoKillEvilKarma and LocalPlayer.Character do
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Karma") and player.Character.Karma.Value < 0 then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position)
                    ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                    ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                end
            end
            task.wait(0.1)
        end
    end)
end)
BoostTab:AddLabel("Ring Aura")
BoostTab:AddTextBox("Whitelist Player", function(name)
    getgenv().whitelist = getgenv().whitelist or {}
    table.insert(getgenv().whitelist, name)
end)
BoostTab:AddButton("Clear Whitelist", function()
    getgenv().whitelist = {}
end)
BoostTab:AddTextBox("Ring Aura Radius", function(value)
    getgenv().ringAuraRadius = tonumber(value) or 10
end)
BoostTab:AddSwitch("Ring Aura", function(state)
    getgenv().ringAura = state
    task.spawn(function()
        while getgenv().ringAura and LocalPlayer.Character do
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and not table.find(getgenv().whitelist or {}, player.Name) and player.Character and player.Character.HumanoidRootPart then
                    local distance = (LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
                    if distance <= (getgenv().ringAuraRadius or 10) then
                        ReplicatedStorage.muscleEvent:FireServer("punch", "rightHand")
                        ReplicatedStorage.muscleEvent:FireServer("punch", "leftHand")
                    end
                end
            end
            task.wait(0.1)
        end
    end)
end)
BoostTab:AddLabel("Fast Rebirths")
BoostTab:AddSwitch("Fast Rebirths | Required New Packs |", function(state)
    getgenv().fastRebirths = state
    task.spawn(function()
        while getgenv().fastRebirths and LocalPlayer.Character do
            ReplicatedStorage.rEvents.rebirthRemote:InvokeServer("rebirthRequest")
            task.wait(0.1)
        end
    end)
end)
BoostTab:AddSwitch("Fast Gain", function(state)
    getgenv().fastGain = state
    task.spawn(function()
        while getgenv().fastGain and LocalPlayer.Character do
            ReplicatedStorage.muscleEvent:FireServer("rep")
            task.wait(0.05)
        end
    end)
end)
BoostTab:AddSwitch("Hide Frames", function(state)
    getgenv().hideFrames = state
    task.spawn(function()
        while getgenv().hideFrames do
            for _, frame in pairs(game:GetService("CoreGui"):GetDescendants()) do
                if frame:IsA("Frame") then
                    frame.Visible = false
                end
            end
            task.wait(0.1)
        end
    end)
end)

LocalPlayer.CharacterAdded:Connect(function(char)
    if getgenv().setws and char:FindFirstChild("Humanoid") then
        char.Humanoid.WalkSpeed = getgenv().ws or 16
    end
    if getgenv().jumpPower and char:FindFirstChild("Humanoid") then
        char.Humanoid.JumpPower = getgenv().jumpPower or 50
    end
    if getgenv().hipHeight and char:FindFirstChild("Humanoid") then
        char.Humanoid.HipHeight = getgenv().hipHeight or 0
    end
end)