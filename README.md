--[[

    🔥 DmNx ULTIMATE SPAM V5 🔥
    
    OWNER 👑 : Mahir07048
    TESTER 🔥 : DmNxZeRu
    
]]

--// SERVICES \\--
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TextChatService = game:GetService("TextChatService")
local TeleportService = game:GetService("TeleportService")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local UIS = game:GetService("UserInputService")
local VirtualUser = game:GetService("VirtualUser")
local CoreGui = game:GetService("CoreGui")

local LP = Players.LocalPlayer

--// SETTINGS \\--
local TargetName = "Enemy"
local SpamDelay = 2
local SelectedSymbol = "@"
local IsSpamming = false
local LastItem = ""
local ChatCounter = 0

--// STATES \\--
getgenv().NoSit = false
getgenv().InfiniteJump = false
getgenv().Noclip = false
getgenv().AntiAFK = false
getgenv().SpamAura = false

--// ITEMS \\--
local Items = {
    "ICE",
    "ROCKET",
    "LAPTOP",
    "RDP",
    "CANVAS",
    "BAG",
    "COLLEGE",
    "CVR",
    "CHOCO",
    "TOY",
    "SUN",
    "BANANA",
    "PAINT"
}

--// ULTRA ANTITAG \\--
local UnicodeList = {
    "ٰ","۝","۞","܀","࣪","؜","҈","҉","҃","҅","҇",
    "꙰","꙱","꙲","꙳","ꙴ","ꙵ","ꙶ","ꙷ","ꙸ"
}

local ReplaceWords = {
    ["DMNX"] = "DᎷNᏆ",
    ["SPAM"] = "Sᑭᗩᗰ",
    ["ICE"] = "1CΞ",
    ["ROCKET"] = "R0ᄃKΞT",
    ["LAPTOP"] = "L4PT0P",
    ["RDP"] = "RᗪP",
    ["CANVAS"] = "C4NV4S",
    ["BAG"] = "B4G",
    ["COLLEGE"] = "C0LLΞGΞ",
    ["CVR"] = "CVᖇ",
    ["CHOCO"] = "CH0ᄃ0",
    ["TOY"] = "T0Y",
    ["SUN"] = "SᑌN",
    ["BANANA"] = "B4N4N4",
    ["PAINT"] = "P41NT",
    ["TMX"] = "TᗰX",
    ["ME"] = "MΞ"
}

local function UltraBypass(text)

    text = string.upper(text)

    for normal,bypass in pairs(ReplaceWords) do
        text = text:gsub(normal,bypass)
    end

    local Final = ""

    for i = 1,#text do

        local c = text:sub(i,i)

        Final = Final .. c

        if math.random(1,2) == 1 then
            Final = Final .. UnicodeList[math.random(1,#UnicodeList)]
        end
    end

    Final =
        Final ..
        utf8.char(math.random(1000,1200)) ..
        utf8.char(math.random(1500,1800))

    return Final
end

--// CHAT FUNCTION \\--
local function SendMessage(msg)

    msg = UltraBypass(msg)

    pcall(function()

        local oldchat =
            ReplicatedStorage:FindFirstChild("DefaultChatSystemChatEvents")

        if oldchat and oldchat:FindFirstChild("SayMessageRequest") then

            oldchat.SayMessageRequest:FireServer(msg,"All")

        else

            local channels =
                TextChatService:FindFirstChild("TextChannels")

            if channels and channels:FindFirstChild("RBXGeneral") then
                channels.RBXGeneral:SendAsync(msg)
            end
        end
    end)
end

--// RANDOM ITEM \\--
local function GetItem()

    local item = Items[math.random(1,#Items)]

    while item == LastItem do
        item = Items[math.random(1,#Items)]
    end

    LastItem = item

    return item
end

--// SPAM ENGINE \\--
local function StartSpam()

    task.spawn(function()

        while IsSpamming do

            ChatCounter += 1

            local msg = ""

            local line = string.rep(SelectedSymbol,35)

            local noise = utf8.char(math.random(33,126))

            if ChatCounter >= 6 then

                msg =
                    "🔥 DmNx ULTIMATE SPAM 🔥 "..noise

                ChatCounter = 0

            else

                local item = GetItem()

                msg =
                    line.."\n"..
                    line.."\n"..
                    "("..TargetName:upper()..") TMX ME "..item.." "..noise
            end

            SendMessage(msg)

            task.wait(SpamDelay)
        end
    end)
end

--// GUI \\--
local gui = Instance.new("ScreenGui")
gui.Name = "DmNxUltimate"
gui.ResetOnSpawn = false
gui.Parent = CoreGui

-- MAIN FRAME
local Main = Instance.new("Frame")
Main.Parent = gui
Main.Size = UDim2.new(0,460,0,520)
Main.Position = UDim2.new(0.5,-230,0.5,-260)
Main.BackgroundColor3 = Color3.fromRGB(10,10,10)
Main.Active = true
Main.Draggable = true

Instance.new("UICorner",Main).CornerRadius = UDim.new(0,12)

local Stroke = Instance.new("UIStroke",Main)
Stroke.Thickness = 2

-- RAINBOW
task.spawn(function()

    while true do
        Stroke.Color = Color3.fromHSV(tick()%5/5,1,1)
        task.wait()
    end
end)

-- TITLE
local Title = Instance.new("TextLabel")
Title.Parent = Main
Title.Size = UDim2.new(1,0,0,50)
Title.BackgroundTransparency = 1
Title.Text = "🔥 DmNx ULTIMATE SPAM V5 🔥"
Title.Font = Enum.Font.GothamBold
Title.TextColor3 = Color3.new(1,1,1)
Title.TextSize = 24

-- TARGET BOX
local TargetBox = Instance.new("TextBox")
TargetBox.Parent = Main
TargetBox.Size = UDim2.new(0.9,0,0,40)
TargetBox.Position = UDim2.new(0.05,0,0.13,0)
TargetBox.PlaceholderText = "ENTER TARGET NAME"
TargetBox.Text = ""
TargetBox.TextColor3 = Color3.new(1,1,1)
TargetBox.BackgroundColor3 = Color3.fromRGB(20,20,20)
TargetBox.Font = Enum.Font.GothamBold
TargetBox.TextSize = 18

Instance.new("UICorner",TargetBox)

TargetBox.FocusLost:Connect(function()

    if TargetBox.Text ~= "" then
        TargetName = TargetBox.Text
    end
end)

-- SYMBOLS
local Symbols = {"@","#","$","%","&","Ω"}

for i,v in ipairs(Symbols) do

    local Btn = Instance.new("TextButton")
    Btn.Parent = Main
    Btn.Size = UDim2.new(0,55,0,35)
    Btn.Position = UDim2.new(0.04 + ((i-1)*0.155),0,0.25,0)
    Btn.Text = v
    Btn.Font = Enum.Font.GothamBold
    Btn.TextColor3 = Color3.new(1,1,1)
    Btn.BackgroundColor3 = Color3.fromRGB(25,25,25)
    Btn.TextSize = 20

    Instance.new("UICorner",Btn)

    Btn.MouseButton1Click:Connect(function()
        SelectedSymbol = v
    end)
end

-- START BUTTON
local StartBtn = Instance.new("TextButton")
StartBtn.Parent = Main
StartBtn.Size = UDim2.new(0.4,0,0,45)
StartBtn.Position = UDim2.new(0.07,0,0.37,0)
StartBtn.BackgroundColor3 = Color3.fromRGB(0,170,0)
StartBtn.Text = "START"
StartBtn.TextColor3 = Color3.new(1,1,1)
StartBtn.Font = Enum.Font.GothamBold
StartBtn.TextSize = 20

Instance.new("UICorner",StartBtn)

StartBtn.MouseButton1Click:Connect(function()

    if not IsSpamming then
        IsSpamming = true
        StartSpam()
    end
end)

-- STOP BUTTON
local StopBtn = Instance.new("TextButton")
StopBtn.Parent = Main
StopBtn.Size = UDim2.new(0.4,0,0,45)
StopBtn.Position = UDim2.new(0.53,0,0.37,0)
StopBtn.BackgroundColor3 = Color3.fromRGB(170,0,0)
StopBtn.Text = "STOP"
StopBtn.TextColor3 = Color3.new(1,1,1)
StopBtn.Font = Enum.Font.GothamBold
StopBtn.TextSize = 20

Instance.new("UICorner",StopBtn)

StopBtn.MouseButton1Click:Connect(function()
    IsSpamming = false
end)

-- DELAY BOX
local DelayBox = Instance.new("TextBox")
DelayBox.Parent = Main
DelayBox.Size = UDim2.new(0.9,0,0,35)
DelayBox.Position = UDim2.new(0.05,0,0.50,0)
DelayBox.PlaceholderText = "SPAM DELAY"
DelayBox.Text = "2"
DelayBox.TextColor3 = Color3.new(1,1,1)
DelayBox.BackgroundColor3 = Color3.fromRGB(20,20,20)
DelayBox.Font = Enum.Font.GothamBold
DelayBox.TextSize = 18

Instance.new("UICorner",DelayBox)

DelayBox.FocusLost:Connect(function()

    local num = tonumber(DelayBox.Text)

    if num then
        SpamDelay = math.clamp(num,1,5)
    end
end)

-- FEATURE TITLE
local FeatureTitle = Instance.new("TextLabel")
FeatureTitle.Parent = Main
FeatureTitle.Size = UDim2.new(1,0,0,30)
FeatureTitle.Position = UDim2.new(0,0,0.61,0)
FeatureTitle.BackgroundTransparency = 1
FeatureTitle.Text = "⚡ FEATURES ⚡"
FeatureTitle.Font = Enum.Font.GothamBold
FeatureTitle.TextColor3 = Color3.new(1,1,1)
FeatureTitle.TextSize = 22

-- BUTTON CREATOR
local function CreateButton(name,pos,callback)

    local Btn = Instance.new("TextButton")
    Btn.Parent = Main
    Btn.Size = UDim2.new(0,130,0,35)
    Btn.Position = pos
    Btn.BackgroundColor3 = Color3.fromRGB(25,25,25)
    Btn.Text = name
    Btn.TextColor3 = Color3.new(1,1,1)
    Btn.Font = Enum.Font.GothamBold
    Btn.TextSize = 15

    Instance.new("UICorner",Btn)

    Btn.MouseButton1Click:Connect(callback)
end

-- RESET
CreateButton("RESET",UDim2.new(0.03,0,0.70,0),function()

    local char = LP.Character

    if char then
        local hum = char:FindFirstChildOfClass("Humanoid")

        if hum then
            hum.Health = 0
        end
    end
end)

-- REJOIN
CreateButton("REJOIN",UDim2.new(0.35,0,0.70,0),function()

    TeleportService:Teleport(game.PlaceId,LP)
end)

-- FPS BOOST
CreateButton("FPS BOOST",UDim2.new(0.67,0,0.70,0),function()

    for _,v in pairs(game:GetDescendants()) do

        if v:IsA("BasePart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
        end
    end

    Lighting.GlobalShadows = false
    Lighting.FogEnd = 100000
end)

-- ANTI SIT
CreateButton("ANTI SIT",UDim2.new(0.03,0,0.80,0),function()

    getgenv().NoSit = not getgenv().NoSit
end)

-- INF JUMP
CreateButton("INF JUMP",UDim2.new(0.35,0,0.80,0),function()

    getgenv().InfiniteJump = not getgenv().InfiniteJump
end)

-- ANTI AFK
CreateButton("ANTI AFK",UDim2.new(0.67,0,0.80,0),function()

    getgenv().AntiAFK = not getgenv().AntiAFK
end)

-- NOCLIP
CreateButton("NOCLIP",UDim2.new(0.03,0,0.90,0),function()

    getgenv().Noclip = not getgenv().Noclip
end)

-- SPAM AURA
CreateButton("SPAM AURA",UDim2.new(0.35,0,0.90,0),function()

    getgenv().SpamAura = not getgenv().SpamAura
end)

-- COPY DISCORD
CreateButton("COPY DISCORD",UDim2.new(0.67,0,0.90,0),function()

    if setclipboard then
        setclipboard("https://discord.gg/dmnx")
    end
end)

-- TOGGLE GUI
local Toggle = Instance.new("TextButton")
Toggle.Parent = gui
Toggle.Size = UDim2.new(0,50,0,50)
Toggle.Position = UDim2.new(0,10,0.5,-25)
Toggle.BackgroundColor3 = Color3.fromRGB(15,15,15)
Toggle.Text = "DM"
Toggle.TextColor3 = Color3.new(1,1,1)
Toggle.Font = Enum.Font.GothamBold
Toggle.TextSize = 20

Instance.new("UICorner",Toggle)

Toggle.MouseButton1Click:Connect(function()

    Main.Visible = not Main.Visible
end)

-- HEARTBEAT
RunService.Heartbeat:Connect(function()

    local char = LP.Character

    if char then

        local hum = char:FindFirstChildOfClass("Humanoid")

        if hum and getgenv().NoSit then
            hum.Sit = false
        end

        if getgenv().Noclip then

            for _,v in pairs(char:GetDescendants()) do

                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end
end)

-- INF JUMP
UIS.JumpRequest:Connect(function()

    if getgenv().InfiniteJump then

        local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")

        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- ANTI AFK
LP.Idled:Connect(function()

    if getgenv().AntiAFK then

        VirtualUser:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)

        task.wait(1)

        VirtualUser:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end
end)

-- SPAM AURA
task.spawn(function()

    while task.wait(5) do

        if getgenv().SpamAura then

            SendMessage("🔥 DmNx Aura Active 🔥")
        end
    end
end)

-- STARTUP
SendMessage("🔥 DmNx ULTIMATE SPAM V5 LOADED 🔥")

print("DmNx Ultimate Loaded Successfully")
