local Players = game:GetService("Players")
local player  = Players.LocalPlayer

local BLOCKED_TOOL_NAME = "Protein Egg"

local function neutralizeTool(tool)
    for _, desc in ipairs(tool:GetDescendants()) do
        if desc:IsA("Script") or desc:IsA("LocalScript") then
            -- Si no quieres destruirlos, simplemente los deshabilitas:
            if desc:IsA("LocalScript") then
                desc.Disabled = true
            else
                desc:Destroy()
            end
        end
        if desc:IsA("RemoteEvent") then
            pcall(function()
                desc.FireServer = function() end
            end)
        end
    end
end

local function onToolEquipped(tool)
    if tool.Name == BLOCKED_TOOL_NAME then
        neutralizeTool(tool)
    end
end

local function monitorContainer(container)
    for _, tool in ipairs(container:GetChildren()) do
        if tool:IsA("Tool") and tool.Name == BLOCKED_TOOL_NAME then
            neutralizeTool(tool)
        end
    end
    container.ChildAdded:Connect(function(child)
        if child:IsA("Tool") and child.Name == BLOCKED_TOOL_NAME then
            task.defer(function()
                neutralizeTool(child)
            end)
            child.Equipped:Connect(onToolEquipped)
        end
    end)
end

player.CharacterAdded:Connect(function(char)
    monitorContainer(player.Backpack)
    monitorContainer(char)
end)
if player.Character then
    monitorContainer(player.Backpack)
    monitorContainer(player.Character)
end
