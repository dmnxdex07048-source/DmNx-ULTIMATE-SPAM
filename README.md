--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
-- Services
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TextChatService = game:GetService("TextChatService")

-- State
local running = false
local interval = 1.1 
local currentMode = "ALL"
local glowStrokes = {}

-- UI Setup
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ZxzSpamV1_GlowEdition"
screenGui.IgnoreGuiInset = true 
screenGui.ResetOnSpawn = false
screenGui.DisplayOrder = 999999 

local success, err = pcall(function() screenGui.Parent = CoreGui end)
if not success then screenGui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui") end

-- Helper for Glowing Style
local function applyGlowStyle(obj, radius, thickness)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius or 10)
    corner.Parent = obj
    
    local stroke = Instance.new("UIStroke")
    stroke.Thickness = thickness or 2.5
    stroke.Color = Color3.fromRGB(255, 255, 255)
    stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
    stroke.Parent = obj
    
    table.insert(glowStrokes, stroke)
    return stroke
end

-- Main Frame
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 380, 0, 260) -- Adjusted width for 6 buttons
main.Position = UDim2.new(0.5, -190, 0.4, -130)
main.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
main.Active = true
main.Draggable = true
main.Parent = screenGui
applyGlowStyle(main, 12, 2)

-- Ninja Icon
local icon = Instance.new("ImageLabel")
icon.Size = UDim2.new(0, 60, 0, 60)
icon.Position = UDim2.new(0.5, -30, 0, -30)
icon.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
icon.Image = "rbxassetid://10821590453"
icon.Parent = main
applyGlowStyle(icon, 12, 2.5)

-- Banner Title
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 50)
title.Position = UDim2.new(0, 0, 0, 10)
title.BackgroundTransparency = 1
title.Text = "DmNx SPAM V1 🔥"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextSize = 24
title.Font = Enum.Font.GothamBold
title.Parent = main

-- Target Input
local targetInput = Instance.new("TextBox")
targetInput.Size = UDim2.new(0.9, 0, 0, 45)
targetInput.Position = UDim2.new(0.05, 0, 0.25, 0)
targetInput.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
targetInput.PlaceholderText = "Target Name Here..."
targetInput.Text = ""
targetInput.TextColor3 = Color3.new(1, 1, 1)
targetInput.Font = Enum.Font.GothamBold
targetInput.Parent = main
applyGlowStyle(targetInput, 8, 1.5)

-- Mode Selection Frame
local modeFrame = Instance.new("Frame")
modeFrame.Size = UDim2.new(0.96, 0, 0, 45)
modeFrame.Position = UDim2.new(0.02, 0, 0.46, 0)
modeFrame.BackgroundTransparency = 1
modeFrame.Parent = main

local function createGlowBtn(text, mode, xPos)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.15, 0, 0.9, 0) -- Adjusted width for 6 buttons
    btn.Position = UDim2.new(xPos, 0, 0, 0)
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    btn.Text = text
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    btn.Parent = modeFrame
    applyGlowStyle(btn, 6, 2.5)

    btn.MouseButton1Click:Connect(function()
        currentMode = mode
        for _, v in pairs(modeFrame:GetChildren()) do
            if v:IsA("TextButton") then v.BackgroundColor3 = Color3.fromRGB(30, 30, 30) end
        end
        btn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
    end)
    return btn
end

-- Layout for 6 Buttons
createGlowBtn("^~^", "UP", 0)
createGlowBtn("(ㄒ爪Ҝ乂)", "MAGI", 0.16)
createGlowBtn("#", "ANGEL", 0.32)
createGlowBtn("@", "AT", 0.48)
createGlowBtn("@_@", "AT_UNDERSCORE", 0.64)
createGlowBtn("ALL", "ALL", 0.82).BackgroundColor3 = Color3.fromRGB(80, 80, 80)

-- Main Toggle
local toggleSpam = Instance.new("TextButton")
toggleSpam.Size = UDim2.new(0.9, 0, 0, 50)
toggleSpam.Position = UDim2.new(0.05, 0, 0.75, 0)
toggleSpam.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
toggleSpam.Text = "START SPAMMING"
toggleSpam.TextColor3 = Color3.new(1, 1, 1)
toggleSpam.Font = Enum.Font.GothamBold
toggleSpam.TextSize = 20
toggleSpam.Parent = main
applyGlowStyle(toggleSpam, 10, 2)

-- AE Toggle Button
local aeBtn = Instance.new("TextButton")
aeBtn.Size = UDim2.new(0, 55, 0, 55)
aeBtn.Position = UDim2.new(0, 15, 0.5, -27)
aeBtn.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
aeBtn.Text = "AE"
aeBtn.TextColor3 = Color3.new(1, 1, 1)
aeBtn.Font = Enum.Font.GothamBold
aeBtn.Parent = screenGui
applyGlowStyle(aeBtn, 15, 4)

aeBtn.MouseButton1Click:Connect(function() main.Visible = not main.Visible end)

-- Rainbow & Pulsing Animation
task.spawn(function()
    local h = 0
    while true do
        h = h + 0.01
        local color = Color3.fromHSV(h % 1, 0.8, 1)
        local transparency = 0.2 + (math.sin(tick() * 5) * 0.3)
        for _, stroke in pairs(glowStrokes) do
            stroke.Color = color
            stroke.Transparency = transparency
        end
        task.wait(0.03)
    end
end)

-- Chat Function
local function sendChat(msg)
    if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
        local channel = TextChatService.TextChannels.RBXGeneral
        if channel then channel:SendAsync(msg) end
    else
        local remote = ReplicatedStorage:FindFirstChild("SayMessageRequest", true)
        if remote then remote:FireServer(msg, "All") end
    end
end

-- Spam Logic
toggleSpam.MouseButton1Click:Connect(function()
    running = not running
    if running then
        toggleSpam.Text = "STOP SPAMMING"
        toggleSpam.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
        task.spawn(function()
            local index = 1
            while running do
                local target = targetInput.Text ~= "" and targetInput.Text or "Player"
                local msgs = {
                    UP = "(^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^^~^ (" .. target .. ") ㄒ爪Ҝ乂 爪乇丨ㄖᐯ乇几 😒)",
                     = "(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂) 🔥 (" .. target .. ")",
                    乃Ҝㄥ = "################################################################################################################################### (" .. target .. ") H8TERS XUDA😛🔥",
                    AT = "(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂)(ㄒ爪Ҝ乂) (" .. target .. ") ㄒ爪Ҝ乂 爪乇丨 千卂几 🗿",
                    AT_UNDERSCORE = "@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@@_@   (" .. ㄒ爪Ҝ乂 爪乇丨 乃卄ㄖﾌ卩ㄩ尺丨( target .. ")"
                }
                local list = {msgs.UP, msgs.ㄒ爪Ҝ乂, msgs.乃Ҝㄥ, msgs.AT, msgs.AT_UNDERSCORE}
                local toSend = (currentMode == "ALL") and list[index] or msgs[currentMode]
                if currentMode == "ALL" then index = (index % #list) + 1 end
                pcall(function() sendChat(toSend) end)
                task.wait(interval)
            end
        end)
    else
        toggleSpam.Text = "START SPAMMING"
        toggleSpam.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    end
end)

-- Brookhaven Announcement on Load
task.spawn(function()
    task.wait(0.5)
    sendChat("—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—_—·–—__—— Khel Khatam Beta DmNx v1 Loaded 🔥🌪️")
end)
