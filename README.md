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

local CustomName = "🔥 ᗪ爪几乂 ㄩㄥㄒ丨爪卂ㄒ乇 丂卩卂爪 🔥"

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

FeatureTab:CreateToggle({
    Name = "RGB RP Name",
    CurrentValue = true,
    Callback = function(state)
        RPNameEnabled = state

        local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

        -- REMOVE NAME
        if not state and remote then
            remote:FireServer("SetRPName", "")
        end
    end,
})

local FeatureTab = Window:CreateTab("Features", 4483362458)

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

FeatureTab:CreateButton({
    Name = "Rejoin Server",
    Callback = function()
        game:GetService("TeleportService"):Teleport(
            game.PlaceId,
            game.Players.LocalPlayer
        )
    end,
})

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
