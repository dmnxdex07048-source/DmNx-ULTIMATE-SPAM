-- JUST CHANGE THIS PART OF YOUR SCRIPT

local Window = Rayfield:CreateWindow({
   Name = "🔥 DmNx ULTIMATE SPAM 🔥",
   LoadingTitle = "DmNx ULTIMATE SPAM",
   LoadingSubtitle = "By DmNx Ji",
   ConfigurationSaving = {
      Enabled = false,
   },
   Discord = {
      Enabled = false,
   },
   KeySystem = false,
})

local MainTab = Window:CreateTab("🔥 Spammer", 4483362458)

MainTab:CreateInput({
   Name = "🎯 Target Name",
   PlaceholderText = "Enter Target Name",
   RemoveTextAfterFocusLost = false,
   Callback = function(t)
      TargetName = t
   end,
})

MainTab:CreateInput({
   Name = "🌈 Custom RP Name",
   PlaceholderText = "Enter RP Name",
   RemoveTextAfterFocusLost = false,
   Callback = function(t)
      if t ~= "" then
         RP_NAME = t
      end
   end,
})

MainTab:CreateDropdown({
   Name = "✨ Spam Symbol",
   Options = {"@", "#", "$", "%", "*", "Ω", "Σ", "&"},
   CurrentOption = {"@"},
   Callback = function(o)
      SelectedSymbol = o[1]
   end,
})

MainTab:CreateSlider({
   Name = "⏱ Spam Delay",
   Range = {1,5},
   Increment = 0.1,
   Suffix = " Seconds",
   CurrentValue = 2,
   Callback = function(v)
      SpamDelay = v
   end,
})

MainTab:CreateButton({
   Name = "🚀 START SPAM",
   Callback = function()
      if not IsSpamming then
         IsSpamming = true
         SendSpam()

         Rayfield:Notify({
            Title = "DmNx ULTIMATE SPAM",
            Content = "Spam Started Successfully",
            Duration = 3,
            Image = 4483362458,
         })
      end
   end,
})

MainTab:CreateButton({
   Name = "🛑 STOP SPAM",
   Callback = function()
      IsSpamming = false

      Rayfield:Notify({
         Title = "DmNx ULTIMATE SPAM",
         Content = "Spam Stopped",
         Duration = 3,
         Image = 4483362458,
      })
   end,
})

-- FEATURES TAB

local FeatureTab = Window:CreateTab("⚡ Features", 4483362458)

FeatureTab:CreateToggle({
   Name = "🛡 Mod Detector",
   CurrentValue = true,
   Callback = function(v)
      ModDetector = v
   end
})

FeatureTab:CreateToggle({
   Name = "🪑 Ultra No Sit",
   CurrentValue = false,
   Callback = function(v)
      NoSit = v
   end
})

FeatureTab:CreateToggle({
   Name = "💥 Anti-Fling",
   CurrentValue = false,
   Callback = function(v)
      AntiFling = v
   end
})

FeatureTab:CreateToggle({
   Name = "🔥 Anti-Bang",
   CurrentValue = false,
   Callback = function(v)
      AntiBang = v
   end
})

FeatureTab:CreateButton({
   Name = "💀 Reset Character",
   Callback = function()
      local char = game.Players.LocalPlayer.Character

      if char then
         local hum = char:FindFirstChildOfClass("Humanoid")

         if hum then
            hum.Health = 0
         end
      end
   end
})

FeatureTab:CreateButton({
   Name = "🔄 Rejoin Server",
   Callback = function()
      game:GetService("TeleportService"):Teleport(
         game.PlaceId,
         game.Players.LocalPlayer
      )
   end
})

FeatureTab:CreateButton({
   Name = "❌ Remove RP Name",
   Callback = function()
      CustomNameActive = false

      local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

      if remote then
         remote:FireServer("SetRPName", "")
      end

      Rayfield:Notify({
         Title = "DmNx",
         Content = "RP Name Removed",
         Duration = 3,
         Image = 4483362458,
      })
   end
})

FeatureTab:CreateButton({
   Name = "📋 Copy Discord",
   Callback = function()
      if setclipboard then
         setclipboard("https://discord.gg/dmnx")
      end

      Rayfield:Notify({
         Title = "DmNx Empire",
         Content = "Discord Link Copied",
         Duration = 3,
         Image = 4483362458,
      })
   end
})

-- CREDITS TAB

local CreditTab = Window:CreateTab("👑 Credits", 4483362458)

CreditTab:CreateLabel("OWNER - DmNx Ji")
CreditTab:CreateLabel("TESTER - DmNxZeru")
CreditTab:CreateLabel("SCRIPT - DmNx ULTIMATE SPAM")

Rayfield:Notify({
   Title = "DmNx ULTIMATE SPAM",
   Content = "Loaded Successfully!",
   Duration = 5,
   Image = 4483362458,
})
