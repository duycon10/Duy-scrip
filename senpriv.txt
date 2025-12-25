-- -- âœ… Whitelist System (UserId-based, for script access)
-- local whitelist = {
--     [8338257076] = true,
--     [8567162226] = true,
--     [3499198980] = true,
--     [8440579671] = true,
--     [8469575498] = true, -- Added as requested
--     [8435731222] = true, -- New
--     [2548119720] = true, -- New
--     [8470796004] = true, -- Whitelisted as requested
-- }

-- local player = game.Players.LocalPlayer
-- if not whitelist[player.UserId] then
--     player:Kick("Message From Sen(UA): Huy putangina ka magbayad kamuna sakin")
--     return
-- end

-- -- Player name/UserId whitelist for "Auto Kill All"
-- local playerNameWhitelist = {}
-- local userIdWhitelist = {
--     [8338257076] = true,
--     [8567162226] = true,
--     [3499198980] = true,
--     [2259275501] = true,
--     [8469575498] = true,
--     [8558962930] = true, -- New
--     [2548119720] = true, -- New
--     [8470796004] = true, -- Whitelisted as requested
-- }

-- local function isPlayerNameWhitelisted(name)
--     for _, n in ipairs(playerNameWhitelist) do
--         if n:lower() == name:lower() then
--             return true
--         end
--     end
--     return false
-- end

-- local function isUserIdWhitelisted(userId)
--     return userIdWhitelist[userId] or false
-- end

-- -- Anti AFK/Anti Kick
-- task.spawn(function()
--     local vu = game:service'VirtualUser'
--     game:GetService('Players').LocalPlayer.Idled:Connect(function()
--         vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
--         task.wait(1)
--         vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
--     end)
-- end)

-- Load Elerium v2 UI Library
local library_url = "https://raw.githubusercontent.com/memejames/elerium-v2-ui-library/main/Library"
local success, library_code = pcall(function()
    return game:HttpGet(library_url, true)
end)

if not success or not library_code or #library_code == 0 then
    warn("âŒ Failed to load UI library.")
    return
end

local library
local loaded = pcall(function()
    library = loadstring(library_code)()
end)

if not loaded or not library then
    warn("âŒ UI library failed to load.")
    return
end

-- Create Main Window and Tabs
local newWindow = library:AddWindow("Sen â€¢ Paid Script", {
    main_color = Color3.fromRGB(30, 30, 30),
    min_size = Vector2.new(500, 300),
    can_resize = false,
    tabs_side = true,
})

local rebirthTab = newWindow:AddTab("Rebirth") -- Changed from Home to Rebirth
local autoStuffTab = newWindow:AddTab("Auto")
local killerTab = newWindow:AddTab("Killer")
local autoCrystalsTab = newWindow:AddTab("Crystals")
local autoChestTab = newWindow:AddTab("Chests")
local autoRockTab = newWindow:AddTab("Rocks") -- The Rocks tab
local lockTab = newWindow:AddTab("Lock")
local teleportTab = newWindow:AddTab("TP")
local creditsTab = newWindow:AddTab("Credits")
rebirthTab:Show() -- Show the new Rebirth tab by default

-- Rebirth Tab (formerly Home)
local targetRebirthCount = 0 -- Define here so it's accessible within the rebirthTab scope

rebirthTab:AddSwitch("Auto Rebirth (Infinite)", function(State)
    _G.AutoRebirthEnabled = State
    _G.TargetRebirthEnabled = false -- Turn off target rebirth if this is enabled
    task.spawn(function()
        while _G.AutoRebirthEnabled do
            game:GetService("ReplicatedStorage").rEvents.rebirthRemote:InvokeServer("rebirthRequest")
            wait(0.5)
        end
    end)
end, "Automatically rebirths infinitely.")

rebirthTab:AddTextBox("Target Rebirth Amount", function(txt)
    local num = tonumber(txt)
    if num and num >= 0 then
        targetRebirthCount = math.floor(num)
    else
        warn("Invalid target rebirth amount entered: " .. txt)
    end
end, {
    placeholder = "Enter target rebirth count",
    cleartext = false
})

rebirthTab:AddSwitch("Enable Target Rebirth", function(State)
    _G.TargetRebirthEnabled = State
    _G.AutoRebirthEnabled = false -- Turn off auto rebirth (infinite) if this is enabled
    task.spawn(function()
        local currentRebirths = player.leaderstats.Rebirths.Value -- Assuming leaderstats is the correct path to rebirths
        while _G.TargetRebirthEnabled and currentRebirths < targetRebirthCount do
            game:GetService("ReplicatedStorage").rEvents.rebirthRemote:InvokeServer("rebirthRequest")
            wait(0.5)
            currentRebirths = player.leaderstats.Rebirths.Value -- Update current rebirths
        end
        if _G.TargetRebirthEnabled and currentRebirths >= targetRebirthCount then
            _G.TargetRebirthEnabled = false -- Turn off once target is reached
            -- You might want to add a notification here that the target has been reached
        end
    end)
end, "Automatically rebirths until target amount is reached.")


-- Auto Stuff Tab
autoStuffTab:AddSwitch("Auto Weight", function(Value)
    _G.AutoWeight = Value
    if Value then
        local weightTool = player.Backpack:FindFirstChild("Weight")
        if weightTool then
            player.Character.Humanoid:EquipTool(weightTool)
        end
    else
        local equipped = player.Character and player.Character:FindFirstChild("Weight")
        if equipped then
            equipped.Parent = player.Backpack
        end
    end
    task.spawn(function()
        while _G.AutoWeight do
            if not _G.AutoWeight then break end
            player.muscleEvent:FireServer("rep")
            task.wait(0.1)
        end
    end)
end, "Do lifting automatically")

autoStuffTab:AddSwitch("Auto Pushups", function(Value)
    _G.AutoPushups = Value
    if Value then
        local tool = player.Backpack:FindFirstChild("Pushups")
        if tool then
            player.Character.Humanoid:EquipTool(tool)
        end
    else
        local equipped = player.Character and player.Character:FindFirstChild("Pushups")
        if equipped then
            equipped.Parent = player.Backpack
        end
    end
    task.spawn(function()
        while _G.AutoPushups do
            if not _G.AutoPushups then break end
            player.muscleEvent:FireServer("rep")
            task.wait(0.1)
        end
    end)
end, "Do pushups automatically")

autoStuffTab:AddSwitch("Auto Handstand", function(Value)
    _G.AutoHandstand = Value
    if Value then
        local tool = player.Backpack:FindFirstChild("Handstand")
        if tool then
            player.Character.Humanoid:EquipTool(tool)
        end
    else
        local equipped = player.Character and player.Character:FindFirstChild("Handstand")
        if equipped then
            equipped.Parent = player.Backpack
        end
        end
    task.spawn(function()
        while _G.AutoHandstand do
            if not _G.AutoHandstand then break end
            player.muscleEvent:FireServer("rep")
            task.wait(0.1)
        end
    end)
end, "Do handstands automatically")

autoStuffTab:AddSwitch("Auto Situps", function(Value)
    _G.AutoSitups = Value
    if Value then
        local tool = player.Backpack:FindFirstChild("Situps")
        if tool then
            player.Character.Humanoid:EquipTool(tool)
        end
    else
        local equipped = player.Character and player.Character:FindFirstChild("Situps")
        if equipped then
            equipped.Parent = player.Backpack
        end
    end
    task.spawn(function()
        while _G.AutoSitups do
            if not _G.AutoSitups then break end
            player.muscleEvent:FireServer("rep")
            task.wait(0.1)
        end
    end)
end, "Do situps automatically")

autoStuffTab:AddSwitch("Auto Speed Keypress", function(State)
    China = State
    local args = {[1] = "changeSize", [2] = 1}
    game:GetService("ReplicatedStorage").rEvents.changeSpeedSizeRemote:InvokeServer(unpack(args))
    task.spawn(function()
        while China do task.wait() end
    end)
end, "Automatically triggers the speed keypress event")

autoStuffTab:AddSwitch("Auto Turn Size 2", function(State)
    _G.AutoTurnSize2 = State
    local args = {[1] = "changeSize", [2] = 2}
    task.spawn(function()
        while _G.AutoTurnSize2 do
            game:GetService("ReplicatedStorage").rEvents.changeSpeedSizeRemote:InvokeServer(unpack(args))
            task.wait(0.1)
        end
    end)
end, "Automatically sets your size to 2 repeatedly")

autoStuffTab:AddSwitch("Auto join brawl", function(State)
    hot = State
    task.spawn(function()
        while hot do
            wait(2)
            game:GetService("ReplicatedStorage").rEvents.joinBrawlRemote:InvokeServer("joinBrawl")
        end
    end)
end, "Automatically joins brawls when enabled.")

autoStuffTab:AddButton("Turn Small", function()
    game:GetService("ReplicatedStorage").rEvents.changeSpeedSizeRemote:InvokeServer("changeSize",2)
end)

-- KILLER TAB
-- Whitelist management for "Auto Kill All"
killerTab:AddTextBox("Add Whitelist Name", function(txt)
    if txt and txt ~= "" then
        table.insert(playerNameWhitelist, txt)
    end
end, {placeholder = "Type player name to whitelist", cleartext = true})

killerTab:AddButton("Clear Whitelist", function()
    playerNameWhitelist = {}
end)

killerTab:AddSwitch("Auto Kill All (Evil Karma)", function(Value)
    _G.AutoKillAllEvilKarma = Value
    task.spawn(function()
        local lastPos = nil
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            lastPos = player.Character.HumanoidRootPart.CFrame
        end
        while _G.AutoKillAllEvilKarma do
            local punchTool = player.Backpack:FindFirstChild("Punch")
            if punchTool and not (player.Character and player.Character:FindFirstChild("Punch")) then
                player.Character.Humanoid:EquipTool(punchTool)
                task.wait(0.1)
            end
            for _, target in pairs(game.Players:GetPlayers()) do
                if target ~= player
                    and target.Character
                    and target.Character:FindFirstChild("HumanoidRootPart")
                    and player.Character
                    and player.Character:FindFirstChild("RightHand")
                    and not isPlayerNameWhitelisted(target.Name)
                    and not isUserIdWhitelisted(target.UserId)
                then
                    local inSafeZone = false
                    for _, part in ipairs(target.Character:GetChildren()) do
                        if part.Name:lower():find("safe") then
                            inSafeZone = true
                            break
                        end
                    end
                    if not inSafeZone then
                        target.Character.HumanoidRootPart.CFrame = player.Character.RightHand.CFrame * CFrame.new(0,0,-2)
                        player.muscleEvent:FireServer("punch", "rightHand")
                        local tool = player.Character:FindFirstChild("Punch")
                        if tool then tool:Activate() end
                    end
                end
            end
            if lastPos and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = lastPos
            end
            task.wait(0.2)
        end
    end)
end, "Magnet all players to your right hand and punch them for Evil Karma (auto farming)")

-- Target Player Auto Kill
local targetPlayerName = "TARGET_PLAYER_NAME_HERE" -- CHANGE THIS to the exact username of the player you want to target!

killerTab:AddTextBox("Target Name", function(txt)
    targetPlayerName = txt
end, {
    placeholder = "Enter exact player name",
    cleartext = true
})

killerTab:AddSwitch("Auto Kill Target Player", function(Value)
    _G.AutoKillTargetPlayer = Value
    if not Value then return end
    spawn(function()
        local lastPos = player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character.HumanoidRootPart.CFrame
        while _G.AutoKillTargetPlayer do
            -- Equip Punch tool if not equipped
            local punchTool = player.Backpack:FindFirstChild("Punch")
            if punchTool and not (player.Character and player.Character:FindFirstChild("Punch")) then
                player.Character.Humanoid:EquipTool(punchTool)
                wait(0.1)
            end

            -- Find the target player
            local foundTarget = nil
            for _, target in pairs(game.Players:GetPlayers()) do
                if target ~= player and target.Name:lower() == targetPlayerName:lower() and target.Character and target.Character:FindFirstChild("HumanoidRootPart") and player.Character and player.Character:FindFirstChild("RightHand") then
                    foundTarget = target
                    break
                end
            end

            -- If target found, kill them
            if foundTarget then
                -- Optional: check for safe zone parts
                local inSafeZone = false
                for _, part in ipairs(foundTarget.Character:GetChildren()) do
                    if part.Name:lower():find("safe") then
                        inSafeZone = true
                        break
                    end
                end
                if not inSafeZone then
                    foundTarget.Character.HumanoidRootPart.CFrame = player.Character.RightHand.CFrame * CFrame.new(0,0,-2)
                    player.muscleEvent:FireServer("punch", "rightHand")
                    local tool = player.Character:FindFirstChild("Punch")
                    if tool then tool:Activate() end
                end
            end

            -- Always return to your last position
            if lastPos and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = lastPos
            end

            wait(0.2)
        end
    end)
end, "Teleports a specific target to your right hand and punches them. Set the name above!")

-- Auto Crystals Tab with ON/OFF switch
local autoCrystalsEnabled = false

autoCrystalsTab:AddSwitch("Crystals: ON/OFF", function(state)
    autoCrystalsEnabled = state
end, "Toggle all Auto Crystals buttons ON/OFF")

autoCrystalsTab:AddButton("Blue", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Blue Crystal")
end)
autoCrystalsTab:AddButton("Green", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Green Crystal")
end)
autoCrystalsTab:AddButton("Mythical", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Mythical Crystal")
end)
autoCrystalsTab:AddButton("Frost", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Frost Crystal")
end)
autoCrystalsTab:AddButton("Inferno", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Inferno Crystal")
end)
autoCrystalsTab:AddButton("Legends", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Legends Crystal")
end)
autoCrystalsTab:AddButton("Elite", function()
    if not autoCrystalsEnabled then return end
    game:GetService("ReplicatedStorage").rEvents.openCrystalRemote:InvokeServer("openCrystal", "Muscle Elite Crystal")
end)

-- Auto Chest Tab
autoChestTab:AddSwitch("Auto Chests", function(State)
    tootooisgay = State
    task.spawn(function()
        while tootooisgay do
            task.wait()
            local chestNames = {"Magma Chest", "Mythical Chest", "Golden Chest", "Enchanted Chest", "Legends Chest"}
            for _, chest in ipairs(chestNames) do
                game:GetService("ReplicatedStorage").rEvents.checkChestRemote:InvokeServer(chest)
            end
        end
    end)
end, "Automatically collects all main chest types.")

-- AUTO ROCK TAB
autoRockTab:AddSwitch("Auto Punch", function(Value)
    _G.AutoPunch = Value
    _G.AutoFastPunch = false -- Turn off fast punch if regular punch is enabled
    if Value then
        task.spawn(function()
            while _G.AutoPunch do
                local punch = player.Backpack:FindFirstChild("Punch")
                if punch then
                    player.Character.Humanoid:EquipTool(punch)
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                task.wait(0.1)
            end
        end)
        task.spawn(function()
            while _G.AutoPunch do
                player.muscleEvent:FireServer("punch", "rightHand")
                player.muscleEvent:FireServer("punch", "leftHand")
                local punchTool = player.Character and player.Character:FindFirstChild("Punch")
                if punchTool then punchTool:Activate() end
                task.wait(0.1)
            end
        end)
    else
        local equipped = player.Character and player.Character:FindFirstChild("Punch")
        if equipped then
            equipped.Parent = player.Backpack
        end
    end
end, "Automatically punches.")

autoRockTab:AddSwitch("Auto Fast Punch", function(Value)
    _G.AutoFastPunch = Value
    _G.AutoPunch = false -- Turn off regular punch if fast punch is enabled
    if Value then
        task.spawn(function()
            while _G.AutoFastPunch do
                local punch = player.Backpack:FindFirstChild("Punch")
                if punch then
                    player.Character.Humanoid:EquipTool(punch)
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                -- No wait here, to make it fast
                player.muscleEvent:FireServer("punch", "rightHand")
                player.muscleEvent:FireServer("punch", "leftHand")
                local punchTool = player.Character and player.Character:FindFirstChild("Punch")
                if punchTool then punchTool:Activate() end
                task.wait() -- Small wait to prevent crashing, but still very fast
            end
        end)
    else
        local equipped = player.Character and player.Character:FindFirstChild("Punch")
        if equipped then
            equipped.Parent = player.Backpack
        end
    end
end, "Automatically punches very fast.")

local rockList = {
    {name="Legend", durability=1000000, label="Legend Gym Rock"},
    {name="King", durability=5000000, label="Muscle King Gym Rock"},
    {name="Jungle", durability=10000000, label="Ancient Jungle Rock"},
    {name="TinyRock", durability=0, label="Tiny Rock"},
    {name="PunchingRock", durability=10, label="Punching Rock"},
    {name="LargeRock", durability=100, label="Large Rock"},
    {name="GoldenRock", durability=5000, label="Golden Rock"},
    {name="FrozenRock", durability=150000, label="Frozen Rock"},
    {name="MythicalRock", durability=400000, label="Mythical Rock"},
    {name="EternalRock", durability=750000, label="Eternal Rock"},
}
for _, rock in ipairs(rockList) do
    autoRockTab:AddSwitch(rock.name, function(Value)
        selectrock = rock.label
        getgenv().autoFarm = Value
        task.spawn(function()
            while getgenv().autoFarm do
                task.wait()
                if not getgenv().autoFarm then break end
                if player.Durability.Value >= rock.durability then
                    for i, v in pairs(game:GetService("Workspace").machinesFolder:GetDescendants()) do
                        if v.Name == "neededDurability" and v.Value == rock.durability and player.Character:FindFirstChild("LeftHand") and player.Character:FindFirstChild("RightHand") then
                            firetouchinterest(v.Parent.Rock, player.Character.RightHand, 0)
                            firetouchinterest(v.Parent.Rock, player.Character.RightHand, 1)
                            firetouchinterest(v.Parent.Rock, player.Character.LeftHand, 0)
                            firetouchinterest(v.Parent.Rock, player.Character.LeftHand, 1)
                        end
                    end
                end
            end
        end)
    end)
end

-- Lock Position Tab
local lockPositionEnabled = false
local lastLockPosition = nil

lockTab:AddButton("Toggle Lock + AntiTP on Rebirth", function()
    lockPositionEnabled = not lockPositionEnabled

    if lockPositionEnabled then
        local char = player.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            lastLockPosition = char.HumanoidRootPart.CFrame
        end
        if not _G._LockCharConn then
            _G._LockCharConn = player.CharacterAdded:Connect(function(char)
                char:WaitForChild("HumanoidRootPart", 5)
                task.wait(0.2)
                if lastLockPosition and lockPositionEnabled then
                    char.HumanoidRootPart.CFrame = lastLockPosition
                end
            end)
        end
        if not _G._LockHeartbeat then
            _G._LockHeartbeat = game:GetService("RunService").Heartbeat:Connect(function()
                if not lockPositionEnabled then return end
                local char = player.Character
                if char and char:FindFirstChild("HumanoidRootPart") and lastLockPosition then
                    char.HumanoidRootPart.CFrame = lastLockPosition
                end
            end)
        end
    else
        if _G._LockCharConn then _G._LockCharConn:Disconnect() _G._LockCharConn = nil end
        if _G._LockHeartbeat then _G._LockHeartbeat:Disconnect() _G._LockHeartbeat = nil end
        lastLockPosition = nil
    end
end)

-- Teleport Tab
teleportTab:AddButton("Rejoin", function()
    local ts = game:GetService("TeleportService")
    ts:Teleport(game.PlaceId, player)
end)
teleportTab:AddButton("Legends gym", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(4298.60059, 1121.89404, -3898.68066)
    end
end)
teleportTab:AddButton("Mythical gym", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(2386.89038, 139.607956, 1094.26367)
    end
end)
teleportTab:AddButton("Frost gym", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(-2752.56543, 125.822533, -386.73703)
    end
end)
teleportTab:AddButton("Eternal gym", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(-6917.79248, 182.352829, -1336.63928)
    end
end)
teleportTab:AddButton("Tiny island", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(-4.25301933, 220.993713, 1963.60168)
    end
end)
teleportTab:AddButton("Brawl aura 1", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(985.910645, 163.795364, -7037.80615)
    end
end)
teleportTab:AddButton("Brawl aura 2", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(4466.75342, 334.973602, -8425.74512)
    end
end)
teleportTab:AddButton("Brawl aura 3", function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(-1901.87695, 251.895432, -5899.64795)
    end
end)

-- Credits Tab
creditsTab:AddLabel("Credits to a r q for ui liba")
creditsTab:AddLabel("Credits to DekuDimz#7960 aka me lol")
creditsTab:AddLabel("Note: Made by CokeLikeASaint_Sen")

-- Optional: Reload UI if needed
if Lib and Lib.Reload then Lib:Reload() end

-- Anti-AFK auto-respawn after 10 minutes (600s)
task.spawn(function()
    while true do
        wait(600)
        local char = player.Character
        if char and char:FindFirstChild("Head") then
            char.Head:Destroy()
        end
    end
end)