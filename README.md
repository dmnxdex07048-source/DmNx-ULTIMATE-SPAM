--// RP NAME SYSTEM //--
getgenv().RPNameEnabled = true

FeatureTab:CreateToggle({
    Name = "RGB RP Name",
    CurrentValue = true,
    Callback = function(state)
        getgenv().RPNameEnabled = state

        local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

        -- REMOVE NAME WHEN TOGGLE OFF
        if not state and remote then
            remote:FireServer("SetRPName", "")
        end
    end,
})

--// RGB RP NAME LOOP //--
task.spawn(function()
    while true do
        task.wait(0.1)

        pcall(function()
            if not getgenv().RPNameEnabled then
                return
            end

            local player = game.Players.LocalPlayer
            local char = player.Character

            if not char then
                return
            end

            -- SET BROOKHAVEN RP NAME
            local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

            if remote then
                remote:FireServer("SetRPName", CustomName)
            end

            -- RGB HEAD NAME
            local head = char:FindFirstChild("Head")

            if head then
                for _,v in pairs(head:GetChildren()) do
                    if v:IsA("BillboardGui") then
                        local txt = v:FindFirstChildOfClass("TextLabel")

                        if txt then
                            txt.Text = CustomName
                            txt.Font = Enum.Font.RobotoMono
                            txt.TextColor3 = Color3.fromHSV(tick()%5/5,1,1)
                        end
                    end
                end
            end
        end)
    end
end)

--// DRAGGABLE RAYFIELD //--
task.spawn(function()
    task.wait(2)

    pcall(function()
        local RayfieldGUI =
            game:GetService("CoreGui"):FindFirstChild("Rayfield")

        if RayfieldGUI then
            for _,obj in pairs(RayfieldGUI:GetDescendants()) do
                if obj:IsA("Frame") and obj.Name == "Main" then
                    obj.Active = true

                    local UIS = game:GetService("UserInputService")
                    local dragToggle = nil
                    local dragInput = nil
                    local dragStart = nil
                    local startPos = nil

                    local function updateInput(input)
                        local Delta = input.Position - dragStart

                        obj.Position = UDim2.new(
                            startPos.X.Scale,
                            startPos.X.Offset + Delta.X,
                            startPos.Y.Scale,
                            startPos.Y.Offset + Delta.Y
                        )
                    end

                    obj.InputBegan:Connect(function(input)
                        if input.UserInputType == Enum.UserInputType.MouseButton1 then
                            dragToggle = true
                            dragStart = input.Position
                            startPos = obj.Position

                            input.Changed:Connect(function()
                                if input.UserInputState == Enum.UserInputState.End then
                                    dragToggle = false
                                end
                            end)
                        end
                    end)

                    obj.InputChanged:Connect(function(input)
                        if input.UserInputType == Enum.UserInputType.MouseMovement then
                            dragInput = input
                        end
                    end)

                    UIS.InputChanged:Connect(function(input)
                        if input == dragInput and dragToggle then
                            updateInput(input)
                        end
                    end)
                end
            end
        end
    end)
end)
