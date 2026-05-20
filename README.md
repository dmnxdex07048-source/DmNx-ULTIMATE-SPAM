--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
-- // LOAD RAYFIELD IMMEDIATELY \\ --
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- // SETTINGS & STATE \\ --
local TargetName = "Enemy"
local SelectedSymbol = "@"
local SpamDelay = 2.0 
local IsSpamming = false
local CustomNameActive = true
local LastItem = ""
local ChatCounter = 0

local AntiAFK, AntiFling, AntiBang, NoSit, ModDetector = false, false, false, false, true

-- // RP NAME STYLE \\ --
local RP_NAME = "🚨💢 DmNx SPAM USER💢🚨"

-- // INITIAL EXECUTION MESSAGE \\ --
local function StartupMessage()
    local msg = "🚨 BEWARE ! DmNx Ji SPAM USER DETECTED 🚨 ! !"
    local ChatEvent = game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents")
    if ChatEvent then
        ChatEvent.SayMessageRequest:FireServer(msg, "All")
    else
        local channel = game:GetService("TextChatService"):FindFirstChild("TextChannels") and game:GetService("TextChatService").TextChannels:FindFirstChild("RBXGeneral")
        if channel then channel:SendAsync(msg) end
    end
end

-- // MOD WARNING UI \\ --
local function ShowModWarning(name)
    local sg = Instance.new("ScreenGui", game.CoreGui)
    local frame = Instance.new("Frame", sg)
    frame.Size = UDim2.new(1, 0, 0.3, 0)
    frame.Position = UDim2.new(0, 0, 0.35, 0)
    frame.BackgroundColor3 = Color3.new(0.5, 0, 0)
    frame.BackgroundTransparency = 0.3
    
    local txt = Instance.new("TextLabel", frame)
    txt.Size = UDim2.new(1, 0, 1, 0)
    txt.Text = "⚠️ YOUR PAPA JOINED: " .. name:upper() .. " ⚠️"
    txt.TextColor3 = Color3.new(1, 1, 1)
    txt.TextScaled = true
    txt.Font = Enum.Font.SourceSansBold
    
    task.delay(5, function() sg:Destroy() end)
end

-- // RGB NAME & BROOKHAVEN SYNC \\ --
task.spawn(function()
    while task.wait(0.1) do
        if not CustomNameActive then break end
        pcall(function()
            local char = game.Players.LocalPlayer.Character
            if char then
                local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")
                if remote then
                    remote:FireServer("SetRPName", RP_NAME)
                end
                
                local head = char:FindFirstChild("Head")
                if head then
                    for _, v in pairs(head:GetChildren()) do
                        if v:IsA("BillboardGui") then
                            local label = v:FindFirstChildOfClass("TextLabel")
                            if label then
                                label.Text = RP_NAME
                                label.Font = Enum.Font.RobotoMono
                                label.TextColor3 = Color3.fromHSV(tick() % 3 / 3, 1, 1)
                            end
                        end
                    end
                end
            end
        end)
    end
end)

-- // SMOOTH PHYSICS & ULTRA NO SIT \\ --
game:GetService("RunService").Heartbeat:Connect(function()
    local char = game.Players.LocalPlayer.Character
    if char then
        local hrp = char:FindFirstChild("HumanoidRootPart")
        local hum = char:FindFirstChildOfClass("Humanoid")
        
        if hum and NoSit then 
            hum.Sit = false
            if hum:GetState() == Enum.HumanoidStateType.Seated then
                hum:ChangeState(Enum.HumanoidStateType.Running)
            end
        end

        if hrp and (AntiFling or AntiBang) then
            for _, v in pairs(char:GetChildren()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
            
            for _, player in pairs(game.Players:GetPlayers()) do
                if player ~= game.Players.LocalPlayer and player.Character then
                    local otherHrp = player.Character:FindFirstChild("HumanoidRootPart")
                    local otherHum = player.Character:FindFirstChildOfClass("Humanoid")
                    
                    if otherHrp and otherHum then
                        local distance = (hrp.Position - otherHrp.Position).Magnitude
                        if distance < 3.8 then 
                            otherHrp.CFrame = CFrame.new(0, -1000, 0)
                            otherHum.Health = 0
                        end
                    end
                end
            end
        end
    end
end)

-- // MOD DETECTOR \\ --
game.Players.PlayerAdded:Connect(function(player)
    if ModDetector then
        if player:GetRankInGroup(4353493) >= 10 or player.UserId == 23204300 then 
            IsSpamming = false
            ShowModWarning(player.Name)
        end
    end
end)

-- // SPAM ENGINE \\ --
local function GetNewItem()
    -- Updated items list per your request
    local Items = {"ICE", "ROCKET", "LAPTOP", "RDP", "CANVAS", "BAG", "COLLEGE", "CVR", "CHOCO", "TOY", "SUN", "BANANA", "PAINT"}
    local chosen = Items[math.random(1, #Items)]
    while chosen == LastItem do chosen = Items[math.random(1, #Items)] end
    LastItem = chosen
    return chosen
end

local function SendSpam()
    task.spawn(function()
        while IsSpamming do
            ChatCounter = ChatCounter + 1
            local Message = ""
            local Noise = " " .. string.char(math.random(200, 250))
            
            if ChatCounter >= 7 then
                Message = "👑 **MADE BY DmNx PAPA** 👑" .. Noise
                ChatCounter = 0
            else
                local Item = GetNewItem()
                local SL = string.rep(SelectedSymbol, 35)
                Message = SL.."\n"..SL.."\n"..SL.."\n("..TargetName:upper()..") TMX MEH "..Item..Noise
            end
            
            local ChatEvent = game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents")
            if ChatEvent then
                ChatEvent.SayMessageRequest:FireServer(Message, "All")
            else
                local channel = game:GetService("TextChatService"):FindFirstChild("TextChannels") and game:GetService("TextChatService").TextChannels:FindFirstChild("RBXGeneral")
                if channel then channel:SendAsync(Message) end
            end
            task.wait(SpamDelay)
        end
    end)
end

-- // WINDOW SETUP \\ --
local Window = Rayfield:CreateWindow({
   Name = "SAM v2 | Made For H8 Xudai",
   LoadingTitle = "SAM v2",
   LoadingSubtitle = "By Sam Papa",
})

local MainTab = Window:CreateTab("Spammer", 4483362458)

MainTab:CreateInput({
   Name = "Target Name",
   PlaceholderText = "Enter Name",
   Callback = function(t) TargetName = t end,
})

MainTab:CreateDropdown({
   Name = "Symbol Menu",
   Options = {"@", "!", "$", "%", "*", "#", "_", "Ω", "Σ"},
   CurrentOption = {"@"},
   Callback = function(o) SelectedSymbol = o[1] end,
})

MainTab:CreateSlider({
   Name = "Delay",
   Range = {1.0, 5},
   Increment = 0.1,
   Suffix = "s",
   CurrentValue = 2.0, 
   Callback = function(v) SpamDelay = v end,
})

MainTab:CreateButton({
   Name = "Xudai Shuru",
   Callback = function() if not IsSpamming then IsSpamming = true SendSpam() end end,
})

MainTab:CreateButton({
   Name = "Xudai Roko",
   Callback = function() IsSpamming = false end,
})

local FeatureTab = Window:CreateTab("Features", 4483362458)
FeatureTab:CreateToggle({Name = "Mod Detector", CurrentValue = true, Callback = function(v) ModDetector = v end})
FeatureTab:CreateToggle({Name = "Ultra No Sit", Callback = function(v) NoSit = v end})
FeatureTab:CreateToggle({Name = "Anti-Fling (Kill Attacker)", Callback = function(v) AntiFling = v end})
FeatureTab:CreateToggle({Name = "Anti-Bang (Kill Attacker)", Callback = function(v) AntiBang = v end})

-- Reset Player Button
FeatureTab:CreateButton({
    Name = "Reset Player",
    Callback = function()
        local char = game.Players.LocalPlayer.Character
        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum then hum.Health = 0 end
        end
    end
})

-- Rejoin Server Button
FeatureTab:CreateButton({
    Name = "Rejoin Server",
    Callback = function()
        game:GetService("TeleportService"):Teleport(game.PlaceId, game.Players.LocalPlayer)
    end
})

FeatureTab:CreateButton({
    Name = "Reset RP Name",
    Callback = function()
        CustomNameActive = false
        local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")
        if remote then
            remote:FireServer("SetRPName", "") 
        end
        Rayfield:Notify({Title = "Name Reset", Content = "Custom RP Name disabled.", Duration = 3})
    end
})

local CreditTab = Window:CreateTab("Credits", 4483362458)
CreditTab:CreateLabel("DmNx PAPA 👑")

-- // EXECUTE \\ --
StartupMessage()
