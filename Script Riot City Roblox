--// UI Setup
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)

local Frame = Instance.new("Frame", ScreenGui)
local UICorner = Instance.new("UICorner", Frame)

local Title = Instance.new("TextLabel", Frame)
local MainBtn = Instance.new("TextButton", Frame)
local CloseBtn = Instance.new("TextButton", Frame)

Frame.Size = UDim2.new(0, 240, 0, 140)
Frame.Position = UDim2.new(0.5, -120, 0.5, -70)
Frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
Frame.Active = true
UICorner.CornerRadius = UDim.new(0, 10)

-- Title
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.Text = "Hub Lua"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16

-- Style Button
local function styleBtn(btn, y)
    local c = Instance.new("UICorner", btn)
    c.CornerRadius = UDim.new(0, 8)

    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, y)
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
end

styleBtn(MainBtn, 40)
styleBtn(CloseBtn, 90)

MainBtn.Text = "AUTO ATM + PROMPT: OFF"
CloseBtn.Text = "CLOSE"

-- Variables
local enabled = false
local player = game.Players.LocalPlayer

-- Toggle
MainBtn.MouseButton1Click:Connect(function()
    enabled = not enabled
    MainBtn.Text = "AUTO ATM + PROMPT: " .. (enabled and "ON" or "OFF")
end)

-- Close
CloseBtn.MouseButton1Click:Connect(function()
    enabled = false
    ScreenGui:Destroy()
end)

-- Drag
local UIS = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

local function update(input)
    local delta = input.Position - dragStart
    Frame.Position = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
end

Frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = Frame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Frame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement
    or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UIS.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

-- MAIN SYSTEM
task.spawn(function()
    while true do
        task.wait(0.2)

        if enabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("Model") and v.Name == "ATM" then
                    local part = v:FindFirstChildWhichIsA("BasePart")

                    if part then
                        -- Teleport ke ATM
                        player.Character.HumanoidRootPart.CFrame = part.CFrame + Vector3.new(0,3,0)

                        task.wait(0.15) -- kasih waktu load

                        -- Instant ProximityPrompt di ATM itu
                        for _, obj in pairs(v:GetDescendants()) do
                            if obj:IsA("ProximityPrompt") then
                                pcall(function()
                                    fireproximityprompt(obj, 0) -- instan (0 detik)
                                end)
                            end
                        end

                        task.wait(0.25)
                    end
                end
            end
        end
    end
end)
