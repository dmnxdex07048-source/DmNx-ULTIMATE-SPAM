--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
	MODIFIED: AURA OF DmNx Ji, Devil Vibe UI, Cinematic Intro, Custom Input Fields.
]]
local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer

--------------------------------------------------
-- SETTINGS & VARIABLES
--------------------------------------------------
local saying = false
local targetName = "TMX"
local delayTime = 1
local pattern = "_"

--------------------------------------------------
-- RO MESSAGE LIST (KYA RE DmNx PAPA BOL)
--------------------------------------------------
local roMessages = {
	"TMX MEH FIRE 🔥","FYTER BNEGA?🤣","BCHE LEWIS ON TOP👑","TMX MEH Rocket 🚀",
	"TMX MEH electricity ⚡","TMX meh SURF 😆","Leave marde 🤣","LEWIS ON TOP BOL 🔥",
	"TMX MEH BOOK 📚","Pil gya itni jaldi 🤣","Itna lallu fyter🤧","Cvr bye LEWIS👑",
	"Tmx meh petroleum","Dffn?😔","Bhag ja bache😹","Tmx meh dino 😈",
	"TMX MEH MAJDOOR","TMX MEH SCRIPT","TMX MEH TREE🌴","TMX MEH CLIP",
	"Tmx Allu🥔","TMX MEH GOAT","TMX MEH BLAZE","TMX MEH SALT",
	"TMX MEH ROD","TMX MEH UNIVERSE","TMX MEH SOFA",
	"TMX MEH KEYBOARD","TMX MEH SNIPER","LEWIS ON TOP👑",
	"TMX MEH MONITOR","TMX MEH TABLE","TMX MEH GALAXLY",
	"TMX MEH MUSHROOM","TMX MEH STONE","TMX MEH BOT",
	"TMX MEH TAB","TMX MEH CYLINDER","TMX MEH KING",
	"TMX MEH VOID","TMX MEH REAPER","TMX MEH GOD",
	"TMX MEH MASTER","TMX MEH NOVA","TMX MEH BEAST",
	"TMX MEH LEGEND","TMX MEH GHOST","TMX MEH NINJA",
	"TMX MEH STAR","TMX MEH MOON","TMX MEH NEON",
	"TMX MEH OMEGA","TMX MEH STICK","TMX MEH PAPER",
	"TMX MEH STYLE","TMX MEH ALPHA","TMX MEH FISH"
}

--------------------------------------------------
-- GENERATE LINE
--------------------------------------------------
local function generateLine()
	local result = ""
	while #result < 150 do
		result = result .. pattern
	end
	return string.sub(result, 1, 150)
end

--------------------------------------------------
-- SPAM SYSTEM
--------------------------------------------------
local function startROSpam()
	if saying then return end
	saying = true

	task.spawn(function()
		while saying do
			local msgText = roMessages[math.random(1, #roMessages)]
			local msg = generateLine().."\n"..targetName.."\n"..msgText
			
			-- Send message via modern TextChatService
			pcall(function()
				TextChatService.TextChannels.RBXGeneral:SendAsync(msg)
			end)
			
			task.wait(delayTime)
		end
	end)
end

local function stopAll()
	saying = false
end

--------------------------------------------------
-- GUI CREATION (DEVIL VIBE)
--------------------------------------------------
local gui = Instance.new("ScreenGui")
gui.Name = "DevilLewis"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

-- Try to parent to CoreGui for safety, fallback to PlayerGui
local success = pcall(function() gui.Parent = CoreGui end)
if not success then gui.Parent = player:WaitForChild("PlayerGui") end

-- CINEMATIC INTRO
local introFrame = Instance.new("Frame", gui)
introFrame.Size = UDim2.new(1, 0, 1, 0)
introFrame.BackgroundColor3 = Color3.fromRGB(5, 0, 0)
introFrame.BorderSizePixel = 0
introFrame.ZIndex = 100

local introText = Instance.new("TextLabel", introFrame)
introText.Size = UDim2.new(1, 0, 1, 0)
introText.BackgroundTransparency = 1
introText.Text = "L E W I S   I S   H E R E . . ."
introText.Font = Enum.Font.Creepster
introText.TextSize = 45
introText.TextColor3 = Color3.fromRGB(180, 0, 0)
introText.TextTransparency = 1
introText.ZIndex = 101

-- MAIN SQUARE PANEL
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 0, 0, 0) -- Starts at 0 for pop-up tween
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 12, 12)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.ClipsDescendants = true
mainFrame.Active = true
mainFrame.Draggable = true

local mainCorner = Instance.new("UICorner", mainFrame)
mainCorner.CornerRadius = UDim.new(0, 12)

local mainStroke = Instance.new("UIStroke", mainFrame)
mainStroke.Color = Color3.fromRGB(150, 0, 0)
mainStroke.Thickness = 2
mainStroke.Transparency = 0.2

-- TITLE
local titleText = Instance.new("TextLabel", mainFrame)
titleText.Size = UDim2.new(1, 0, 0, 40)
titleText.BackgroundTransparency = 1
titleText.Text = "L E W I S"
titleText.Font = Enum.Font.GothamBlack
titleText.TextSize = 22
titleText.TextColor3 = Color3.fromRGB(200, 20, 20)

-- UI LAYOUT (For organized stacking)
local layout = Instance.new("UIListLayout", mainFrame)
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 10)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- Helper function to create TextBoxes
local function createInput(placeholder, text, order)
	local container = Instance.new("Frame", mainFrame)
	container.Size = UDim2.new(0.9, 0, 0, 35)
	container.BackgroundColor3 = Color3.fromRGB(25, 20, 20)
	container.LayoutOrder = order
	
	local corner = Instance.new("UICorner", container)
	corner.CornerRadius = UDim.new(0, 6)
	
	local stroke = Instance.new("UIStroke", container)
	stroke.Color = Color3.fromRGB(80, 10, 10)
	stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
	
	local input = Instance.new("TextBox", container)
	input.Size = UDim2.new(1, -20, 1, 0)
	input.Position = UDim2.new(0, 10, 0, 0)
	input.BackgroundTransparency = 1
	input.Text = text
	input.PlaceholderText = placeholder
	input.Font = Enum.Font.GothamSemibold
	input.TextSize = 14
	input.TextColor3 = Color3.fromRGB(220, 220, 220)
	input.PlaceholderColor3 = Color3.fromRGB(100, 100, 100)
	input.TextXAlignment = Enum.TextXAlignment.Left
	
	-- Hover Tween for Inputs
	container.MouseEnter:Connect(function()
		TweenService:Create(stroke, TweenInfo.new(0.3), {Color = Color3.fromRGB(180, 0, 0)}):Play()
	end)
	container.MouseLeave:Connect(function()
		TweenService:Create(stroke, TweenInfo.new(0.3), {Color = Color3.fromRGB(80, 10, 10)}):Play()
	end)
	
	return input
end

-- Helper function to create Buttons
local function createButton(text, color, order)
	local btn = Instance.new("TextButton", mainFrame)
	btn.Size = UDim2.new(0.9, 0, 0, 40)
	btn.BackgroundColor3 = color
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 15
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.LayoutOrder = order
	btn.AutoButtonColor = false
	
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 6)
	
	-- Button Animations
	local darkenColor = Color3.new(color.R * 0.7, color.G * 0.7, color.B * 0.7)
	local lightenColor = Color3.new(math.min(color.R * 1.2, 1), math.min(color.G * 1.2, 1), math.min(color.B * 1.2, 1))

	btn.MouseEnter:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = lightenColor}):Play()
	end)
	btn.MouseLeave:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = color}):Play()
	end)
	btn.MouseButton1Down:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.1), {BackgroundColor3 = darkenColor}):Play()
	end)
	btn.MouseButton1Up:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.1), {BackgroundColor3 = lightenColor}):Play()
	end)
	
	return btn
end

-- Create the empty title spacer for UIListLayout
local titleSpacer = Instance.new("Frame", mainFrame)
titleSpacer.Size = UDim2.new(1, 0, 0, 30)
titleSpacer.BackgroundTransparency = 1
titleSpacer.LayoutOrder = 0

-- Inputs
local targetInput = createInput("Target Name (e.g. TMX)", targetName, 1)
local delayInput = createInput("Delay (e.g. 1)", tostring(delayTime), 2)
local patternInput = createInput("Pattern (e.g. _)", pattern, 3)

-- Buttons
local startBtn = createButton("START SPAM", Color3.fromRGB(120, 10, 10), 4)
local stopBtn = createButton("STOP", Color3.fromRGB(40, 40, 40), 5)

-- TOGGLE BUTTON (Small Devil Icon)
local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.new(0, 45, 0, 45)
toggleBtn.Position = UDim2.new(0, 20, 0.5, -22)
toggleBtn.BackgroundColor3 = Color3.fromRGB(15, 12, 12)
toggleBtn.Text = "😈"
toggleBtn.TextSize = 25
toggleBtn.BorderSizePixel = 0
toggleBtn.Visible = false
toggleBtn.Draggable = true

local toggleCorner = Instance.new("UICorner", toggleBtn)
toggleCorner.CornerRadius = UDim.new(0, 8)
local toggleStroke = Instance.new("UIStroke", toggleBtn)
toggleStroke.Color = Color3.fromRGB(150, 0, 0)
toggleStroke.Thickness = 2

--------------------------------------------------
-- FUNCTIONALITY BINDINGS
--------------------------------------------------

-- Update variables when inputs change
targetInput.FocusLost:Connect(function()
	if targetInput.Text ~= "" then targetName = targetInput.Text end
end)

delayInput.FocusLost:Connect(function()
	local num = tonumber(delayInput.Text)
	if num then delayTime = num else delayInput.Text = tostring(delayTime) end
end)

patternInput.FocusLost:Connect(function()
	if patternInput.Text ~= "" then pattern = patternInput.Text end
end)

startBtn.MouseButton1Click:Connect(startROSpam)
stopBtn.MouseButton1Click:Connect(stopAll)

-- Toggle Menu Tween
local menuOpen = true
toggleBtn.MouseButton1Click:Connect(function()
	menuOpen = not menuOpen
	if menuOpen then
		mainFrame.Visible = true
		TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0, 280, 0, 290)}):Play()
	else
		local closeTween = TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0)})
		closeTween:Play()
		closeTween.Completed:Wait()
		mainFrame.Visible = false
	end
end)

--------------------------------------------------
-- PLAY CINEMATIC INTRO
--------------------------------------------------
task.spawn(function()
	-- Text fade in
	local tweenIn = TweenService:Create(introText, TweenInfo.new(2, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {TextTransparency = 0})
	tweenIn:Play()
	tweenIn.Completed:Wait()
	
	task.wait(1.5) -- Hold for effect
	
	-- Text & Background fade out
	TweenService:Create(introText, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.In), {TextTransparency = 1}):Play()
	local bgOut = TweenService:Create(introFrame, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.In), {BackgroundTransparency = 1})
	bgOut:Play()
	bgOut.Completed:Wait()
	
	introFrame:Destroy() -- Cleanup
	
	-- Reveal UI
	toggleBtn.Visible = true
	mainFrame.Visible = true
	TweenService:Create(mainFrame, TweenInfo.new(0.6, Enum.EasingStyle.Elastic, Enum.EasingDirection.Out), {Size = UDim2.new(0, 280, 0, 290)}):Play()
end)
