--// DmNx MODERN RGB SPAM GUI V2 👑
    end
end)

--------------------------------------------------
-- SPAM SYSTEM
--------------------------------------------------

startBtn.MouseButton1Click:Connect(function()
    if enabled then
        return
    end

    local target = nameBox.Text
    local delay = tonumber(delayBox.Text) or 2
    local style = lineBox.Text ~= "" and lineBox.Text or "_"

    if target == "" then
        status.Text = "STATUS : ENTER TARGET"
        return
    end

    enabled = true
    status.Text = "STATUS : SPAMMING"

    local line = string.rep(style,80)

    task.spawn(function()
        while enabled do
            for _,msg in ipairs(replies) do
                if not enabled then
                    break
                end

                send(line.." "..target.." "..msg)
                task.wait(delay)
            end
        end
    end)
end)

--------------------------------------------------
-- STOP BUTTON
--------------------------------------------------

stopBtn.MouseButton1Click:Connect(function()
    enabled = false
    status.Text = "STATUS : STOPPED"
end)

--------------------------------------------------
-- RGB LOOP
--------------------------------------------------

RunService.RenderStepped:Connect(function()
    local color = getRGB()

    frameStroke.Color = color
    title.TextColor3 = color
    toggleStroke.Color = color

    nameStroke.Color = color
    delayStroke.Color = color
    lineStroke.Color = color

    startStroke.Color = color
    stopStroke.Color = color

    status.TextColor3 = color
end)

--------------------------------------------------
-- NOTIFICATION
--------------------------------------------------

pcall(function()
    game:GetService("StarterGui"):SetCore("SendNotification",{
        Title = "DmNx RGB GUI",
        Text = "Loaded Successfully 👑",
        Duration = 5
    })
end)
