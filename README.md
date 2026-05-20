--[[
	👑 DmNx ULTIMATE SPAM REBUILT EDITION 💀
	VERIFIED STABLE RUN TIME ON MODERN EXECUTORS
]]

local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

local player = Players.LocalPlayer

--------------------------------------------------
-- SETTINGS & ENGINE CONFIG
--------------------------------------------------
local saying = false
local targetName = "TMX"
local delayTime = 1
local pattern = "_"

local roMessages = {
	"TMX MEH FIRE 🔥","FYTER BNEGA?🤣","BCHE LEWIS ON TOP👑","TMX MEH Rocket 🚀",
	"TMX MEH electricity ⚡","TMX meh SURF 😆","Leave marde 🤣","LEWIS ON TOP BOL 🔥",
	"TMX MEH BOOK 📚","Pil gya itni jaldi 🤣","Itna lal1u fyter🤧","Cvr bye LEWIS👑",
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

local function generateLine()
	local result = ""
	while #result < 150 do
		result = result .. pattern
	end
	return string.sub(result, 1, 150)
end

--------------------------------------------------
-- ENGINE SPAM LOOPS
--------------------------------------------------
local function startROSpam()
	if saying then return end
	saying = true

	task.spawn(function()
		while saying do
			local msgText = roMessages[math.random(1, #roMessages)]
			local msg = generateLine().."\n"..targetName.."\n"..msgText
			
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
-- SYSTEM GUI INTEGRATION
--------------------------------------------------
local gui = Instance.new("ScreenGui")
gui.Name = "DmNxUltimateSpamEngine"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

-- Safety Check Fallback Environment Assignment
local runSuccess, _ = pcall(function() gui.Parent = CoreGui end)
if not runSuccess then 
	gui.Parent = player:WaitForChild("PlayerGui") 
end

-- DmNx CINEMATIC INTRO STAGE
local introFrame = Instance.new("Frame", gui)
introFrame.Size = UDim2.new(1, 0, 1, 0)
introFrame.BackgroundColor3 = Color3.fromRGB(5, 0, 0)
introFrame.BorderSizePixel = 0
introFrame.ZIndex = 100

local introText = Instance.new("TextLabel", introFrame)
introText.Size = UDim2.new(1, 0, 1, 0)
introText.BackgroundTransparency = 1
introText.Text = "DmNx PAPA IS HERE . . ."
introText.Font = Enum.Font.Creepster
introText.TextSize = 42
introText.TextColor3 = Color3.fromRGB(200, 0, 0)
introText.TextTransparency = 1
introText.ZIndex = 101

-- MAIN ENGINE INTERFACE PANEL
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 0, 0, 0)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(14, 11, 11)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.ClipsDescendants = true
mainFrame.Active = true
mainFrame.Draggable = true

local mainCorner = Instance.new("UICorner", mainFrame)
mainCorner.CornerRadius = UDim.new(0, 12)

local mainStroke = Instance.new("UIStroke", mainFrame)
mainStroke.Color = Color3.fromRGB(160, 0, 0)
mainStroke.Thickness = 2
mainStroke.Transparency = 0.2

-- DEV HEADERS
local titleText = Instance.new("TextLabel", mainFrame)
titleText.Size = UDim2.new(1, 0, 0, 40)
titleText.BackgroundTransparency = 1
titleText.Text = "DmNx ULTIMATE SPAM"
titleText.Font = Enum.Font.GothamBlack
titleText.TextSize = 18
titleText.TextColor3 = Color3.fromRGB(220, 15, 15)

local layout = Instance.new("UIListLayout", mainFrame)
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 10)
layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

-- FACTORY ENGINE BUILD FUNCTIONS
local function createInput(placeholder, text, order)
	local container = Instance.new("Frame", mainFrame)
	container.Size = UDim2.new(0.9, 0, 0, 35)
	container.BackgroundColor3 = Color3.fromRGB(24, 18, 18)
	container.LayoutOrder = order
	
	local corner = Instance.new("UICorner", container)
	corner.CornerRadius = UDim.new(0, 6)
	
	local stroke = Instance.new("UIStroke", container)
	stroke.Color = Color3.fromRGB(90, 12, 12)
	stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
	
	local input = Instance.new("TextBox", container)
	input.Size = UDim2.new(1, -20, 1, 0)
	input.Position = UDim2.new(0, 10, 0, 0)
	input.BackgroundTransparency = 1
	input.Text = text
	input.PlaceholderText = placeholder
	input.Font = Enum.Font.GothamSemibold
	input.TextSize = 14
	input.TextColor3 = Color3.fromRGB(230, 230, 230)
	input.PlaceholderColor3 = Color3.fromRGB(110, 110, 110)
	input.TextXAlignment = Enum.TextXAlignment.Left
	
	container.MouseEnter:Connect(function()
		TweenService:Create(stroke, TweenInfo.new(0.25), {Color = Color3.fromRGB(190, 0, 0)}):Play()
	end)
	container.MouseLeave:Connect(function()
		TweenService:Create(stroke, TweenInfo.new(0.25), {Color = Color3.fromRGB(90, 12, 12)}):Play()
	end)
	
	return input
end

local function createButton(text, color, order)
	local btn = Instance.new("TextButton", mainFrame)
	btn.Size = UDim2.new(0.9, 0, 0, 40)
	btn.BackgroundColor3 = color
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 14
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.LayoutOrder = order
	btn.AutoButtonColor = false
	
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 6)
	
	local darkenColor = Color3.new(color.R * 0.75, color.G * 0.75, color.B * 0.75)
	local lightenColor = Color3.new(math.min(
