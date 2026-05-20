--// RP NAME TOGGLE //--
getgenv().RPNameEnabled = true

FeatureTab:CreateToggle({
    Name = "RGB RP Name",
    CurrentValue = true,
    Callback = function(state)
        getgenv().RPNameEnabled = state

        -- REMOVE RP NAME WHEN OFF
        if not state then
            local remote = game:GetService("ReplicatedStorage"):FindFirstChild("FocusPocus")

            if remote then
                remote:FireServer("SetRPName", "")
            end
        end
    end,
})

--// DRAGGABLE WINDOW //--
task.spawn(function()
    pcall(function()
        local gui = game:GetService("CoreGui"):FindFirstChild("Rayfield")

        if gui then
            for _,v in pairs(gui:GetDescendants()) do
                if v:IsA("Frame") and v.Name == "Main" then
                    v.Active = true
                    v.Draggable = true
                end
            end
        end
    end)
end)

--// RGB RP NAME LOOP //--
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()

            -- STOP IF TOGGLE OFF
            if not getgenv().RPNameEnabled then
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
