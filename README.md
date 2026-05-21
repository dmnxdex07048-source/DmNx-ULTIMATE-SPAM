--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
--[[
WARNING: Heads up! This script has not been verified by ScriptBlox.
Use at your own risk!

SAM V1 CLEAN  
Spam Engine + Tool Loader

]]

---------------- SERVICES ----------------
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TextChatService = game:GetService("TextChatService")

local lp = Players.LocalPlayer

---------------- STATE ----------------
local active = false
local index = 1
local patternIndex = 1
local waitTime = 0
local iyLoaded = false
local mikeyLoaded = false
local gazeLoaded = false
local redzLoaded = false

---------------- PHRASES ----------------
local phrases = {
"leave marde","TMKX FAN","TMKX telephone","Tmkx Tablet",
"Tmkx keyboard","Tmkx guitar","Tmkx brookhaven",
"Tmkx mera lun","SAM DADA BOL"
}

---------------- UI CLEANUP ----------------
pcall(function()
if game.CoreGui:FindFirstChild("SAM_V1") then
game.CoreGui.SAM_V1:Destroy()
end
end)

---------------- UI ----------------
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "SAM_V1"
gui.ResetOnSpawn = false

-- FLOAT TOGGLE
local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.fromScale(0.12, 0.05)
toggleBtn.Position = UDim2.fromScale(0.44, 0.02)
toggleBtn.Text = "SAM UI"
toggleBtn.BackgroundColor3 = Color3.fromRGB(0,0,0)
toggleBtn.TextColor3 = Color3.new(1,1,1)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextScaled = true
toggleBtn.BorderSizePixel = 0
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(0,12)

-- MAIN FRAME
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.fromScale(0.36, 0.7)
frame.Position = UDim2.fromScale(0.32, 0.15)
frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
Instance.new("UICorner", frame).CornerRadius = UDim.new(0,16)

-- HEADER
local header = Instance.new("TextLabel", frame)
header.Size = UDim2.fromScale(1, 0.1)
header.Text = "SAM V1"
header.BackgroundTransparency = 1
header.TextColor3 = Color3.fromRGB(255,255,255)
header.Font = Enum.Font.GothamBold
header.TextScaled = true

---------------- UI HELPERS ----------------
local function makeBtn(text, x, y, sx)
local b = Instance.new("TextButton", frame)
b.Size = UDim2.fromScale(sx or 0.42, 0.08)
b.Position = UDim2.fromScale(x, y)
b.Text = text
b.BackgroundColor3 = Color3.fromRGB(0,0,0)
b.TextColor3 = Color3.new(1,1,1)
b.Font = Enum.Font.GothamBold
b.TextScaled = true
b.BorderSizePixel = 0
Instance.new("UICorner", b).CornerRadius = UDim.new(0,12)
return b
end

local function makeBox(placeholder, x, y)
local t = Instance.new("TextBox", frame)
t.Size = UDim2.fromScale(0.9, 0.08)
t.Position = UDim2.fromScale(x, y)
t.PlaceholderText = placeholder
t.Text = ""
t.BackgroundColor3 = Color3.fromRGB(0,0,0)
t.TextColor3 = Color3.new(1,1,1)
t.Font = Enum.Font.Gotham
t.TextScaled = true
t.BorderSizePixel = 0
Instance.new("UICorner", t).CornerRadius = UDim.new(0,10)
return t
end

---------------- SPAM UI ----------------
local input = makeBox("enter target name", 0.05, 0.15)

local startBtn = makeBtn("START", 0.05, 0.26)
local stopBtn  = makeBtn("STOP",  0.53, 0.26)

---------------- DELAY ----------------
local delayLabel = Instance.new("TextLabel", frame)
delayLabel.Size = UDim2.fromScale(0.4, 0.06)
delayLabel.Position = UDim2.fromScale(0.05, 0.36)
delayLabel.Text = "Delay: 0"
delayLabel.BackgroundTransparency = 1
delayLabel.TextColor3 = Color3.new(1,1,1)
delayLabel.Font = Enum.Font.GothamBold
delayLabel.TextScaled = true

local plusBtn  = makeBtn("+", 0.48, 0.36, 0.18)
local minusBtn = makeBtn("-", 0.72, 0.36, 0.18)

local function updateDelayUI()
delayLabel.Text = "Delay: "..string.format("%.1f",waitTime)..""
end

plusBtn.MouseButton1Click:Connect(function()
waitTime = math.clamp(waitTime + 0.1, 0, 3)
updateDelayUI()
end)

minusBtn.MouseButton1Click:Connect(function()
waitTime = math.clamp(waitTime - 0.1, 0, 3)
updateDelayUI()
end)

---------------- STATUS ----------------
local status = Instance.new("TextLabel", frame)
status.Size = UDim2.fromScale(0.9, 0.06)
status.Position = UDim2.fromScale(0.05, 0.45)
status.Text = "Status: WAITING"
status.BackgroundTransparency = 1
status.TextColor3 = Color3.fromRGB(255,80,80)
status.Font = Enum.Font.GothamBold
status.TextScaled = true

---------------- TOOLS ----------------
local iyBtn    = makeBtn("CORE HUB", 0.05, 0.55)
local mikeyBtn = makeBtn("AUDIO PLAYER", 0.53, 0.55)
local gazeBtn  = makeBtn("EMOTE SYSTEM", 0.29, 0.65)
local redzBtn  = makeBtn("GAME HUB", 0.29, 0.75)

---------------- CHAT SEND ----------------
local function send(msg, pattern)
if not msg or msg == "" then return end

local final = msg  
if pattern then  
    local p = {"%","_","0"}  
    local s = p[patternIndex]  
    final = string.rep(s, math.floor(170/#s)) .. " " .. msg  
    patternIndex = patternIndex % #p + 1  
end  

pcall(function()  
    if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then  
        TextChatService.TextChannels.RBXGeneral:SendAsync(final)  
    else  
        ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer(final, "All")  
    end  
end)

end

---------------- SPAM LOOP ----------------
task.spawn(function()
while true do
if active then
send(input.Text.." "..phrases[index], true)
index = index % #phrases + 1
task.wait(waitTime)
end
task.wait(0.05)
end
end)

startBtn.MouseButton1Click:Connect(function()
active = true
status.Text = "Status: ACTIVE"
status.TextColor3 = Color3.fromRGB(80,255,120)
end)

stopBtn.MouseButton1Click:Connect(function()
active = false
status.Text = "Status: STOPPED"
status.TextColor3 = Color3.fromRGB(255,80,80)
end)

---------------- INFINITE YIELD ----------------
iyBtn.MouseButton1Click:Connect(function()
if iyLoaded then
iyBtn.Text = "HUB LOADED ✅"
return
end

iyLoaded = true  
iyBtn.Text = "LOADING..."  

task.spawn(function()  
    pcall(function()  
        loadstring(game:HttpGet(  
            "https://raw.githubusercontent.com/4Peek/Fractal/refs/heads/main/iy_modded.lua"  
        ))()  
    end)  
    iyBtn.Text = "HUB LOADED ✅"  
end)

end)

---------------- AUDIO PLAYER ----------------
mikeyBtn.MouseButton1Click:Connect(function()
if mikeyLoaded then
mikeyBtn.Text = "AUDIO LOADED ✅"
return
end

mikeyLoaded = true  
mikeyBtn.Text = "LOADING..."  

task.spawn(function()  
    pcall(function()  
        loadstring(game:HttpGet(  
            "https://raw.githubusercontent.com/Gaurav-0196/Mickey-Aud/refs/heads/main/MickeyMusic"  
        ))()  
    end)  
    mikeyBtn.Text = "AUDIO LOADED ✅"  
end)

end)

---------------- EMOTE SYSTEM ----------------
gazeBtn.MouseButton1Click:Connect(function()
if gazeLoaded then
gazeBtn.Text = "EMOTES LOADED ✅"
return
end

gazeLoaded = true  
gazeBtn.Text = "LOADING..."  

task.spawn(function()  
    pcall(function()  
        loadstring(game:HttpGet(  
            "https://raw.githubusercontent.com/Gazer-Ha/Gaze-stuff/refs/heads/main/Gaze%20emote"  
        ))()  
    end)  
    gazeBtn.Text = "EMOTES LOADED ✅"  
end)

end)

---------------- GAME HUB ----------------
redzBtn.MouseButton1Click:Connect(function()
if redzLoaded then
redzBtn.Text = "GAME HUB LOADED ✅"
return
end

redzLoaded = true  
redzBtn.Text = "LOADING..."  

task.spawn(function()  
    pcall(function()  
        loadstring(game:HttpGet(  
            "https://rawscripts.net/raw/Brookhaven-RP-Brookhaven-Redz-Hub-Cracked-77478"  
        ))()  
    end)  
    redzBtn.Text = "GAME HUB LOADED ✅"  
end)

end)

---------------- TOGGLE ----------------
toggleBtn.MouseButton1Click:Connect(function()
frame.Visible = not frame.Visible
end)

updateDelayUI()
print("✅ SAM V1 LOADED — CORE HUB + AUDIO + EMOTES ACTIVE")
