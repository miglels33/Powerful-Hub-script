-- Powerful Script - Enhanced Admin Menu for Roblox
-- Features: Noclip, Infinite Jump, Speed Control, Jump Power, Teleport Tool, Spin

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Color Scheme
local ColorScheme = {
    Background = Color3.fromRGB(30, 30, 40),
    Primary = Color3.fromRGB(0, 170, 255),
    Secondary = Color3.fromRGB(50, 50, 70),
    Text = Color3.fromRGB(255, 255, 255),
    Success = Color3.fromRGB(0, 200, 100),
    Danger = Color3.fromRGB(200, 50, 50)
}

-- Create GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PowerfulScriptMenu"
screenGui.Parent = player.PlayerGui

-- Main Icon (using your requested ID)
local icon = Instance.new("ImageButton")
icon.Name = "MenuIcon"
icon.Image = "rbxassetid://12817026986"
icon.Size = UDim2.new(0, 60, 0, 60)
icon.Position = UDim2.new(0.85, 0, 0.85, 0)
icon.BackgroundColor3 = ColorScheme.Primary
icon.BackgroundTransparency = 0.3
icon.ImageColor3 = ColorScheme.Text
icon.AutoButtonColor = false
icon.Parent = screenGui

-- Icon hover effect
icon.MouseEnter:Connect(function()
    local tween = TweenService:Create(icon, TweenInfo.new(0.2), {BackgroundTransparency = 0, Size = UDim2.new(0, 65, 0, 65)})
    tween:Play()
end)

icon.MouseLeave:Connect(function()
    local tween = TweenService:Create(icon, TweenInfo.new(0.2), {BackgroundTransparency = 0.3, Size = UDim2.new(0, 60, 0, 60)})
    tween:Play()
end)

-- Main Menu Frame
local menuFrame = Instance.new("Frame")
menuFrame.Name = "MenuFrame"
menuFrame.Size = UDim2.new(0, 250, 0, 400)
menuFrame.Position = UDim2.new(0.5, -125, 0.5, -200)
menuFrame.BackgroundColor3 = ColorScheme.Background
menuFrame.Visible = false
menuFrame.Active = true
menuFrame.Draggable = true
menuFrame.Parent = screenGui

-- Shadow Effect
local shadow = Instance.new("ImageLabel")
shadow.Name = "Shadow"
shadow.Image = "rbxassetid://1316045217"
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(10, 10, 118, 118)
shadow.ImageTransparency = 0.8
shadow.Size = UDim2.new(1, 10, 1, 10)
shadow.Position = UDim2.new(0, -5, 0, -5)
shadow.BackgroundTransparency = 1
shadow.Parent = menuFrame

-- Title
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Text = "POWERFUL SCRIPT"
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = ColorScheme.Primary
title.TextColor3 = ColorScheme.Text
title.Font = Enum.Font.GothamBold
title.TextSize = 14
title.Parent = menuFrame

-- Content Scroll Frame
local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Name = "Content"
scrollFrame.Size = UDim2.new(1, 0, 1, -40)
scrollFrame.Position = UDim2.new(0, 0, 0, 40)
scrollFrame.BackgroundTransparency = 1
scrollFrame.ScrollBarThickness = 5
scrollFrame.ScrollBarImageColor3 = ColorScheme.Primary
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, 600)
scrollFrame.Parent = menuFrame

-- Function to create styled buttons
local function createButton(name, text, positionY)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Text = text
    button.Size = UDim2.new(0.9, 0, 0, 35)
    button.Position = UDim2.new(0.05, 0, 0, positionY)
    button.BackgroundColor3 = ColorScheme.Secondary
    button.TextColor3 = ColorScheme.Text
    button.Font = Enum.Font.Gotham
    button.TextSize = 12
    button.AutoButtonColor = false
    button.Parent = scrollFrame
    
    button.MouseEnter:Connect(function()
        button.BackgroundColor3 = ColorScheme.Primary
    end)
    
    button.MouseLeave:Connect(function()
        button.BackgroundColor3 = ColorScheme.Secondary
    end)
    
    return button
end

-- Function to create sliders
local function createSlider(name, text, positionY, min, max, defaultValue)
    local sliderFrame = Instance.new("Frame")
    sliderFrame.Name = name.."Frame"
    sliderFrame.Size = UDim2.new(0.9, 0, 0, 60)
    sliderFrame.Position = UDim2.new(0.05, 0, 0, positionY)
    sliderFrame.BackgroundTransparency = 1
    sliderFrame.Parent = scrollFrame
    
    local label = Instance.new("TextLabel")
    label.Name = name.."Label"
    label.Text = text
    label.Size = UDim2.new(1, 0, 0, 20)
    label.BackgroundTransparency = 1
    label.TextColor3 = ColorScheme.Text
    label.Font = Enum.Font.Gotham
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = sliderFrame
    
    local valueLabel = Instance.new("TextLabel")
    valueLabel.Name = name.."Value"
    valueLabel.Text = defaultValue
    valueLabel.Size = UDim2.new(0.2, 0, 0, 20)
    valueLabel.Position = UDim2.new(0.8, 0, 0, 0)
    valueLabel.BackgroundTransparency = 1
    valueLabel.TextColor3 = ColorScheme.Text
    valueLabel.Font = Enum.Font.GothamBold
    valueLabel.TextSize = 12
    valueLabel.TextXAlignment = Enum.TextXAlignment.Right
    valueLabel.Parent = sliderFrame
    
    local sliderContainer = Instance.new("Frame")
    sliderContainer.Name = "Container"
    sliderContainer.Size = UDim2.new(1, 0, 0, 20)
    sliderContainer.Position = UDim2.new(0, 0, 0, 25)
    sliderContainer.BackgroundColor3 = ColorScheme.Secondary
    sliderContainer.Parent = sliderFrame
    
    local slider = Instance.new("TextButton")
    slider.Name = name.."Slider"
    slider.Text = ""
    slider.Size = UDim2.new(1, 0, 1, 0)
    slider.BackgroundTransparency = 1
    slider.Parent = sliderContainer
    
    local fill = Instance.new("Frame")
    fill.Name = "Fill"
    fill.Size = UDim2.new((defaultValue - min)/(max - min), 0, 1, 0)
    fill.BackgroundColor3 = ColorScheme.Primary
    fill.BorderSizePixel = 0
    fill.Parent = sliderContainer
    
    local dragging = false
    
    slider.MouseButton1Down:Connect(function()
        dragging = true
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    slider.MouseMoved:Connect(function()
        if dragging then
            local x = (UserInputService:GetMouseLocation().X - slider.AbsolutePosition.X) / slider.AbsoluteSize.X
            x = math.clamp(x, 0, 1)
            
            local tween = TweenService:Create(fill, TweenInfo.new(0.1), {Size = UDim2.new(x, 0, 1, 0)})
            tween:Play()
            
            local value = math.floor(min + (max - min) * x)
            valueLabel.Text = tostring(value)
            
            if name == "Speed" then
                humanoid.WalkSpeed = value
            elseif name == "JumpPower" then
                humanoid.JumpPower = value
            end
        end
    end)
    
    return sliderFrame
end

-- Create menu controls
local yPosition = 10

-- Noclip
local noclipButton = createButton("Noclip", "🚀 Noclip: OFF", yPosition)
yPosition += 40

local noclipActive = false
local noclipConnection

noclipButton.MouseButton1Click:Connect(function()
    noclipActive = not noclipActive
    if noclipActive then
        noclipButton.Text = "🚀 Noclip: ON"
        noclipButton.BackgroundColor3 = ColorScheme.Success
        
        noclipConnection = RunService.Stepped:Connect(function()
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        noclipButton.Text = "🚀 Noclip: OFF"
        noclipButton.BackgroundColor3 = ColorScheme.Secondary
        
        if noclipConnection then
            noclipConnection:Disconnect()
            
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end
end)

-- Infinite Jump
local infJumpButton = createButton("InfJump", "🦘 Infinite Jump: OFF", yPosition)
yPosition += 40

local infJumpActive = false
local infJumpConnection

infJumpButton.MouseButton1Click:Connect(function()
    infJumpActive = not infJumpActive
    if infJumpActive then
        infJumpButton.Text = "🦘 Infinite Jump: ON"
        infJumpButton.BackgroundColor3 = ColorScheme.Success
        
        infJumpConnection = UserInputService.JumpRequest:Connect(function()
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end)
    else
        infJumpButton.Text = "🦘 Infinite Jump: OFF"
        infJumpButton.BackgroundColor3 = ColorScheme.Secondary
        
        if infJumpConnection then
            infJumpConnection:Disconnect()
        end
    end
end)

-- Speed Control
local speedSlider = createSlider("Speed", "🏃‍♂️ Speed:", yPosition, 0, 1000, humanoid.WalkSpeed)
yPosition += 70

-- Jump Power
local jumpSlider = createSlider("JumpPower", "🦘 Jump Power:", yPosition, 0, 5000, humanoid.JumpPower)
yPosition += 70

-- Teleport Tool
local teleportButton = createButton("TeleportTool", "✨ Give Teleport Tool", yPosition)
yPosition += 40

teleportButton.MouseButton1Click:Connect(function()
    local backpack = player:FindFirstChild("Backpack")
    local character = player.Character or player.CharacterAdded:Wait()
    
    if backpack then
        local oldTool = backpack:FindFirstChild("TeleportTool")
        if oldTool then
            oldTool:Destroy()
        end
    end
    
    if character then
        local oldTool = character:FindFirstChild("TeleportTool")
        if oldTool then
            oldTool:Destroy()
        end
    end
    
    local tool = Instance.new("Tool")
    tool.Name = "TeleportTool"
    tool.RequiresHandle = false
    tool.CanBeDropped = false
    
    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(0.5, 0.5, 0.5)
    handle.Transparency = 1
    handle.Parent = tool
    
    tool.Equipped:Connect(function()
        tool.Activated:Connect(function()
            local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                local mouse = player:GetMouse()
                if mouse.Target then
                    -- Using your requested particle ID
                    local particles = Instance.new("ParticleEmitter")
                    particles.Texture = "rbxassetid://114554427006548"
                    particles.LightEmission = 1
                    particles.Size = NumberSequence.new(0.5)
                    particles.Speed = NumberRange.new(5)
                    particles.Lifetime = NumberRange.new(0.5)
                    particles.EmissionDirection = Enum.NormalId.Top
                    particles.Parent = humanoidRootPart
                    
                    humanoidRootPart.CFrame = CFrame.new(mouse.Hit.Position + Vector3.new(0, 3, 0))
                    
                    game:GetService("Debris"):AddItem(particles, 1)
                end
            end
        end)
    end)
    
    tool.Parent = backpack or character
    
    teleportButton.Text = "✔ Teleport Tool Added"
    teleportButton.BackgroundColor3 = ColorScheme.Success
    wait(1.5)
    teleportButton.Text = "✨ Give Teleport Tool"
    teleportButton.BackgroundColor3 = ColorScheme.Secondary
end)

-- Fast Spin
local spinButton = createButton("Spin", "🌀 Spin: OFF", yPosition)
yPosition += 40

local spinActive = false
local spinConnection

spinButton.MouseButton1Click:Connect(function()
    spinActive = not spinActive
    if spinActive then
        spinButton.Text = "🌀 Spin: ON"
        spinButton.BackgroundColor3 = ColorScheme.Success
        
        spinConnection = RunService.Heartbeat:Connect(function(delta)
            if character then
                local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    humanoidRootPart.CFrame = humanoidRootPart.CFrame * CFrame.Angles(0, math.rad(2000 * delta), 0)
                end
            end
        end)
    else
        spinButton.Text = "🌀 Spin: OFF"
        spinButton.BackgroundColor3 = ColorScheme.Secondary
        
        if spinConnection then
            spinConnection:Disconnect()
        end
    end
end)

-- Close Button
local closeButton = createButton("Close", "❌ Close Menu", yPosition)
closeButton.MouseButton1Click:Connect(function()
    menuFrame.Visible = false
end)

-- Update scroll frame size
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, yPosition + 50)

-- Toggle menu with animation
icon.MouseButton1Click:Connect(function()
    menuFrame.Visible = not menuFrame.Visible
    if menuFrame.Visible then
        menuFrame.Position = UDim2.new(0.5, -125, 0.5, -200)
        menuFrame.Size = UDim2.new(0, 0, 0, 0)
        menuFrame.Visible = true
        
        local tweenIn = TweenService:Create(menuFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out), 
            {Size = UDim2.new(0, 250, 0, 400)})
        tweenIn:Play()
    else
        local tweenOut = TweenService:Create(menuFrame, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.In), 
            {Size = UDim2.new(0, 0, 0, 0)})
        tweenOut:Play()
        
        tweenOut.Completed:Wait()
        menuFrame.Visible = false
    end
end)

-- Handle character respawns
player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = newCharacter:WaitForChild("Humanoid")
    
    -- Reapply settings
    if speedSlider then
        humanoid.WalkSpeed = tonumber(speedSlider:FindFirstChild("SpeedValue").Text)
    end
    if jumpSlider then
        humanoid.JumpPower = tonumber(jumpSlider:FindFirstChild("JumpPowerValue").Text)
    end
    
    -- Reconnect active features
    if noclipActive and noclipConnection then
        noclipConnection:Disconnect()
        noclipConnection = RunService.Stepped:Connect(function()
            if character then
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end
    
    if spinActive and spinConnection then
        spinConnection:Disconnect()
        spinConnection = RunService.Heartbeat:Connect(function(delta)
            if character then
                local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    humanoidRootPart.CFrame = humanoidRootPart.CFrame * CFrame.Angles(0, math.rad(2000 * delta), 0)
                end
            end
        end)
    end
end)
