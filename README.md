--// DmNx ULTIMATE SPAM //--
--// OWNER : DmNx Ji //--
--// TESTER : DmNxZeru //--

--// LOAD RAYFIELD //--
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

--// SETTINGS //--
local TargetName = "Enemy"
local SelectedSymbol = "@"
local SpamDelay = 2
local IsSpamming = false
local LastItem = ""
local ChatCounter = 0

-- CUSTOM RP NAME INPUT
MainTab:CreateInput({
    Name = "Custom RP Name",
    PlaceholderText = "Enter RP Name",
    RemoveTextAfterFocusLost = false,
    Callback = function(text)

        if text and text ~= "" then
            CustomName = text
        end
    end,
})
MainTab:CreateInput({
    Name = "Target Name",

--// ITEMS //--
local Items = {
    "ㄒ爪Ҝ乂 ME ICE",
    "ㄒ爪Ҝ乂 ME ROCKET",
    "ㄒ爪Ҝ乂 ME LAPTOP",
    "ㄒ爪Ҝ乂 ME RDP",
    "ㄒ爪Ҝ乂 ME CANVAS",
    " ㄒ爪Ҝ乂 ME BAG",
    "ㄒ爪Ҝ乂 ME COLLEGE",
    "ㄒ爪Ҝ乂 ME CVR",
    "ㄒ爪Ҝ乂 ME CHOCO",
    "ㄒ爪Ҝ乂 ME TOY",
    "ㄒ爪Ҝ乂 ME SUN",
    "ㄒ爪Ҝ乂 ME BANANA",
    "ㄒ爪Ҝ乂 ME PAINT"
}

--// CHAT FUNCTION //--
local function SendMessage(msg)
    local ChatEvent = game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents")

    if ChatEvent then
        ChatEvent.SayMessageRequest:FireServer(msg, "All")
    else
        local channel = game:GetService("TextChatService"):FindFirstChild("TextChannels")
        if channel and channel:FindFirstChild("RBXGeneral") then
            channel.RBXGeneral:SendAsync(msg)
        end
    end
end

--// RANDOM ITEM //--
local function GetItem()
    local item = Items[math.random(1, #Items)]

    while item == LastItem do
        item = Items[math.random(1, #Items)]
    end

    LastItem = item
    return item
end

--// RP NAME ENABLE //--
local RPNameEnabled = true

--// RGB RP NAME //--
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()

            -- STOP NAME IF DISABLED
            if not RPNameEnabled then
                return
            end

            local char = game.Players.LocalPlayer.Character

            if char then
                local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

                if remote then
                    remote:FireServer("SetRPName", CustomName)
                end

                local head = char:FindFirstChild("Head")

                if head then
                    for _,v in pairs(head:GetChildren()) do
                        if v:IsA("BillboardGui") then
                            local txt = v:FindFirstChildOfClass("TextLabel")

                            if txt then
                                txt.Text = CustomName
                                txt.TextColor3 = Color3.fromHSV(tick()%5/5,1,1)
                                txt.Font = Enum.Font.RobotoMono
                            end
                        end
                    end
                end
            end
        end)
    end
end)

--// SPAM ENGINE //--
local function StartSpam()
    task.spawn(function()
        while IsSpamming do
            ChatCounter = ChatCounter + 1

            local Noise = " " .. string.char(math.random(200,250))
            local Message = ""

            if ChatCounter >= 7 then
                Message = "👑 MADE BY DmNx Ji😛🔥" .. Noise
                ChatCounter = 0
            else
                local item = GetItem()
                local line = string.rep(SelectedSymbol, 35)

                Message =
                    line.."\n"..
                    line.."\n"..
                    line.."\n"..
                    "("..TargetName:upper()..") "..item..Noise
            end

            SendMessage(Message)
            task.wait(SpamDelay)
        end
    end)
end

--// WINDOW //--
local Window = Rayfield:CreateWindow({
    Name = "DmNx ULTIMATE SPAM",
    LoadingTitle = "DmNx ULTIMATE SPAM",
    LoadingSubtitle = "By DmNx",
    ConfigurationSaving = {
        Enabled = false,
    },
    Discord = {
        Enabled = false,
    },
    KeySystem = false,
})

--// MAIN TAB //--
local MainTab = Window:CreateTab("Main", 4483362458)

MainTab:CreateInput({
    Name = "Target Name",
    PlaceholderText = "Enter Target",
    RemoveTextAfterFocusLost = false,
    Callback = function(text)
        TargetName = text
    end,
})

MainTab:CreateDropdown({
    Name = "Spam Symbol",
    Options = {"@", "#", "$", "%", "&", "*", "Ω", "Σ"},
    CurrentOption = {"@"},
    Callback = function(option)
        SelectedSymbol = option[1]
    end,
})

MainTab:CreateSlider({
    Name = "Spam Delay",
    Range = {1,5},
    Increment = 0.1,
    Suffix = " Seconds",
    CurrentValue = 2,
    Callback = function(value)
        SpamDelay = value
    end,
})

MainTab:CreateButton({
    Name = "START SPAM",
    Callback = function()
        if not IsSpamming then
            IsSpamming = true
            StartSpam()
        end
    end,
})

MainTab:CreateButton({
    Name = "STOP SPAM",
    Callback = function()
        IsSpamming = false
    end,
})

--// FEATURES TAB //--

local FeatureTab = Window:CreateTab("Features", 4483362458)

-- RESET CHARACTER
FeatureTab:CreateButton({
    Name = "Reset Character",
    Callback = function()
        local char = game.Players.LocalPlayer.Character

        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")

            if hum then
                hum.Health = 0
            end
        end
    end,
})

-- REJOIN
FeatureTab:CreateButton({
    Name = "Rejoin Server",
    Callback = function()
        game:GetService("TeleportService"):Teleport(
            game.PlaceId,
            game.Players.LocalPlayer
        )
    end,
})

-- ANTI SIT
FeatureTab:CreateToggle({
    Name = "Anti Sit",
    CurrentValue = false,
    Callback = function(state)
        getgenv().NoSit = state
    end,
})

game:GetService("RunService").Heartbeat:Connect(function()
    if getgenv().NoSit then
        local char = game.Players.LocalPlayer.Character

        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")

            if hum then
                hum.Sit = false
            end
        end
    end
end)

-- RGB RP NAME
FeatureTab:CreateToggle({
    Name = "RGB RP Name",
    CurrentValue = true,
    Callback = function(state)
        RPNameEnabled = state

        local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

        if not state and remote then
            remote:FireServer("SetRPName", "")
        end
    end,
})

-- WALKSPEED
FeatureTab:CreateSlider({
    Name = "WalkSpeed",
    Range = {16, 100},
    Increment = 1,
    Suffix = " WS",
    CurrentValue = 16,
    Callback = function(v)
        local hum = game.Players.LocalPlayer.Character and
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")

        if hum then
            hum.WalkSpeed = v
        end
    end,
})

-- JUMPPOWER
FeatureTab:CreateSlider({
    Name = "JumpPower",
    Range = {50, 200},
    Increment = 5,
    Suffix = " JP",
    CurrentValue = 50,
    Callback = function(v)
        local hum = game.Players.LocalPlayer.Character and
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")

        if hum then
            hum.JumpPower = v
        end
    end,
})

-- INFINITE JUMP
getgenv().InfiniteJump = false

FeatureTab:CreateToggle({
    Name = "Infinite Jump",
    CurrentValue = false,
    Callback = function(v)
        getgenv().InfiniteJump = v
    end,
})

game:GetService("UserInputService").JumpRequest:Connect(function()
    if getgenv().InfiniteJump then
        local hum = game.Players.LocalPlayer.Character and
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")

        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- NOCLIP
getgenv().Noclip = false

FeatureTab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Callback = function(v)
        getgenv().Noclip = v
    end,
})

game:GetService("RunService").Stepped:Connect(function()
    if getgenv().Noclip then
        local char = game.Players.LocalPlayer.Character

        if char then
            for _,v in pairs(char:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.CanCollide = false
                end
            end
        end
    end
end)

-- FULLBRIGHT
FeatureTab:CreateToggle({
    Name = "FullBright",
    CurrentValue = false,
    Callback = function(v)

        if v then
            game:GetService("Lighting").Brightness = 5
            game:GetService("Lighting").ClockTime = 14
            game:GetService("Lighting").FogEnd = 100000
            game:GetService("Lighting").GlobalShadows = false
        else
            game:GetService("Lighting").Brightness = 2
            game:GetService("Lighting").GlobalShadows = true
        end
    end,
})

-- FLY
getgenv().Fly = false

FeatureTab:CreateToggle({
    Name = "Fly",
    CurrentValue = false,
    Callback = function(v)
        getgenv().Fly = v

        local player = game.Players.LocalPlayer
        local char = player.Character
        if not char then return end

        local hrp = char:FindFirstChild("HumanoidRootPart")
        if not hrp then return end

        if v then
            local bv = Instance.new("BodyVelocity")
            bv.Name = "DmNxFly"
            bv.MaxForce = Vector3.new(999999,999999,999999)
            bv.Velocity = Vector3.new(0,0,0)
            bv.Parent = hrp

            task.spawn(function()
                while getgenv().Fly and hrp do
                    task.wait()
                    bv.Velocity = workspace.CurrentCamera.CFrame.LookVector * 60
                end

                if bv then
                    bv:Destroy()
                end
            end)
        else
            local fly = hrp:FindFirstChild("DmNxFly")
            if fly then
                fly:Destroy()
            end
        end
    end,
})

-- ANTI AFK
FeatureTab:CreateButton({
    Name = "Anti AFK",
    Callback = function()
        local vu = game:GetService("VirtualUser")

        game.Players.LocalPlayer.Idled:Connect(function()
            vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
            task.wait(1)
            vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        end)

        Rayfield:Notify({
            Title = "DmNx ULTIMATE SPAM",
            Content = "Anti AFK Enabled",
            Duration = 3,
        })
    end,
})

-- FPS BOOST
FeatureTab:CreateButton({
    Name = "FPS Boost",
    Callback = function()
        for _,v in pairs(game:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Material = Enum.Material.Plastic
                v.Reflectance = 0
            end
        end

        game:GetService("Lighting").GlobalShadows = false

        Rayfield:Notify({
            Title = "DmNx ULTIMATE SPAM",
            Content = "FPS Boost Enabled",
            Duration = 3,
        })
    end,
})

-- COPY DISCORD
FeatureTab:CreateButton({
    Name = "Copy Discord Invite",
    Callback = function()
        if setclipboard then
            setclipboard("https://discord.gg/dwZHZBVje")
        end

        Rayfield:Notify({
            Title = "DmNx",
            Content = "Discord Link Copied",
            Duration = 3,
        })
    end,
})

--// CREDITS TAB //--
local CreditTab = Window:CreateTab("Credits", 4483362458)

CreditTab:CreateLabel("OWNER - DmNx Ji")
CreditTab:CreateLabel("TESTER - DmNxZeru")
CreditTab:CreateLabel("USER - YOU🎀")

--// STARTUP MESSAGE //--
SendMessage("🔥 DmNx ULTIMATE SPAM LOADED 🔥")

Rayfield:Notify({
    Title = "DmNx ULTIMATE SPAM",
    Content = "Loaded Successfully!",
    Duration = 5,
    Image = 4483362458,
})
