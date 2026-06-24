-- [[ SKYLOSRG HUB - SLAP A ZOMBIE v1.4 - GHOST GLIDE EDITION ]] --

local function getSafeFolder()
    if gethui then 
        return gethui() 
    elseif game:GetService("CoreGui"):FindFirstChild("RobloxGui") then
        return game:GetService("CoreGui")
    else
        return game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
    end
end

local TargetParent = getSafeFolder()

if TargetParent:FindFirstChild("SkylosrG_CyberHub") then
    TargetParent["SkylosrG_CyberHub"]:Destroy()
end

-- BIẾN CONFIG TOÀN CỤC
_G.FarmDistance = 10
_G.TweenSpeedPercent = 55
_G.AllowSlideShow = true
_G.AntiAFKActive = true
_G.GhostGlideActive = false -- Tính năng mới: Xuyên tường theo phím di chuyển
_G.GhostGlideSpeed = 25     -- Tốc độ lướt xuyên tường

local StartTime = os.time()
local RescueYThreshold = 10
local LastSafeCFrame = nil 

local ChadImages = {
    "rbxthumb://type=Asset&id=9832900299&w=420&h=420",
    "rbxthumb://type=Asset&id=12562940534&w=420&h=420",
    "rbxthumb://type=Asset&id=7962561531&w=420&h=420",
    "rbxthumb://type=Asset&id=10290111698&w=420&h=420"
}
local currentImageIndex = 1

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SkylosrG_CyberHub"
ScreenGui.Parent = TargetParent
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 14)
MainFrame.Position = UDim2.new(0.5, -15, 0.45, -15)
MainFrame.Size = UDim2.new(0, 30, 0, 30)
MainFrame.BackgroundTransparency = 0.5
MainFrame.Rotation = -180
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.ClipsDescendants = true
MainFrame.Visible = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(255, 0, 0)
UIStroke.Thickness = 3.5
UIStroke.Parent = MainFrame

local ToggleButton = Instance.new("ImageButton")
ToggleButton.Name = "ToggleButton"
ToggleButton.Parent = ScreenGui
ToggleButton.Position = UDim2.new(0.02, 0, 0.2, 0)
ToggleButton.Size = UDim2.new(0, 55, 0, 55)
ToggleButton.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
ToggleButton.Image = ChadImages[currentImageIndex]
ToggleButton.ScaleType = Enum.ScaleType.Crop
ToggleButton.Visible = false
ToggleButton.Active = true
ToggleButton.Draggable = true

local ToggleCorner = Instance.new("UICorner")
ToggleCorner.CornerRadius = UDim.new(1, 0)
ToggleCorner.Parent = ToggleButton

local ToggleStroke = Instance.new("UIStroke")
ToggleStroke.Color = Color3.fromRGB(200, 20, 30)
ToggleStroke.Thickness = 2.5
ToggleStroke.Parent = ToggleButton

local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
Title.Text = "   SLAP A ZOMBIE v1.4"
Title.TextColor3 = Color3.fromRGB(240, 240, 245)
Title.TextTransparency = 1
Title.TextSize = 14
Title.Font = Enum.Font.RobotoMono
Title.TextXAlignment = Enum.TextXAlignment.Left

local UptimeLabel = Instance.new("TextLabel")
UptimeLabel.Parent = MainFrame
UptimeLabel.Position = UDim2.new(0, 18, 0, 45)
UptimeLabel.Size = UDim2.new(1, -36, 0, 20)
UptimeLabel.BackgroundTransparency = 1
UptimeLabel.Text = "💀 Uptime: 00h 00m 00s"
UptimeLabel.TextColor3 = Color3.fromRGB(140, 140, 150)
UptimeLabel.TextTransparency = 1
UptimeLabel.TextSize = 13
UptimeLabel.Font = Enum.Font.Code
UptimeLabel.TextXAlignment = Enum.TextXAlignment.Left

-- NÚT CHÍNH 1: COMBAT MODE (AUTO FARM)
local FarmToggle = Instance.new("TextButton")
FarmToggle.Parent = MainFrame
FarmToggle.Position = UDim2.new(0.05, 0, 0.2, 0)
FarmToggle.Size = UDim2.new(0.42, 0, 0, 42)
FarmToggle.BackgroundColor3 = Color3.fromRGB(30, 14, 16)
FarmToggle.BackgroundTransparency = 1
FarmToggle.Text = "COMBAT [LOCK]"
FarmToggle.TextColor3 = Color3.fromRGB(190, 40, 40)
FarmToggle.TextTransparency = 1
FarmToggle.TextSize = 13
FarmToggle.Font = Enum.Font.SourceSansBold

local FarmCorner = Instance.new("UICorner")
FarmCorner.CornerRadius = UDim.new(0, 6)
FarmCorner.Parent = FarmToggle

local FarmStroke = Instance.new("UIStroke")
FarmStroke.Color = Color3.fromRGB(190, 40, 40)
FarmStroke.Thickness = 1.5
FarmStroke.Transparency = 1
FarmStroke.Parent = FarmToggle

-- NÚT CHÍNH 2: GHOST GLIDE (TWEEN XUYÊN TƯỜNG THEO PHÍM)
local GlideToggle = Instance.new("TextButton")
GlideToggle.Parent = MainFrame
GlideToggle.Position = UDim2.new(0.53, 0, 0.2, 0)
GlideToggle.Size = UDim2.new(0.42, 0, 0, 42)
GlideToggle.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
GlideToggle.BackgroundTransparency = 1
GlideToggle.Text = "GHOST GLIDE [OFF]"
GlideToggle.TextColor3 = Color3.fromRGB(160, 160, 160)
GlideToggle.TextTransparency = 1
GlideToggle.TextSize = 13
GlideToggle.Font = Enum.Font.SourceSansBold

local GlideCorner = Instance.new("UICorner")
GlideCorner.CornerRadius = UDim.new(0, 6)
GlideCorner.Parent = GlideToggle

local GlideStroke = Instance.new("UIStroke")
GlideStroke.Color = Color3.fromRGB(50, 50, 55)
GlideStroke.Thickness = 1.5
GlideStroke.Transparency = 1
GlideStroke.Parent = GlideToggle

-- KHU VỰC SETTINGS
local SettingsFrame = Instance.new("Frame")
SettingsFrame.Name = "SettingsFrame"
SettingsFrame.Parent = MainFrame
SettingsFrame.Position = UDim2.new(0.05, 0, 0.36, 0)
SettingsFrame.Size = UDim2.new(0.9, 0, 0, 210)
SettingsFrame.BackgroundTransparency = 1

local DistLabel = Instance.new("TextLabel")
DistLabel.Parent = SettingsFrame
DistLabel.Size = UDim2.new(1, 0, 0, 18)
DistLabel.BackgroundTransparency = 1
DistLabel.Text = "📏 Target Distance: 10.0 Studs"
DistLabel.TextColor3 = Color3.fromRGB(220, 220, 225)
DistLabel.TextTransparency = 1
DistLabel.TextSize = 13
DistLabel.Font = Enum.Font.SourceSansBold
DistLabel.TextXAlignment = Enum.TextXAlignment.Left

local DistBar = Instance.new("Frame")
DistBar.Parent = SettingsFrame
DistBar.Position = UDim2.new(0, 0, 0, 24)
DistBar.Size = UDim2.new(1, 0, 0, 5)
DistBar.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
DistBar.BackgroundTransparency = 1

local DistDot = Instance.new("TextButton")
DistDot.Parent = DistBar
DistDot.Position = UDim2.new(0.47, -6, -1.2, 0)
DistDot.Size = UDim2.new(0, 14, 0, 14)
DistDot.BackgroundColor3 = Color3.fromRGB(200, 20, 30)
DistDot.BackgroundTransparency = 1
DistDot.Text = ""
Instance.new("UICorner", DistDot).CornerRadius = UDim.new(1, 0)

local SpeedLabel = Instance.new("TextLabel")
SpeedLabel.Parent = SettingsFrame
SpeedLabel.Position = UDim2.new(0, 0, 0, 48)
SpeedLabel.Size = UDim2.new(1, 0, 0, 18)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.Text = "⚡ Tween Throttle: 55%"
SpeedLabel.TextColor3 = Color3.fromRGB(220, 220, 225)
SpeedLabel.TextTransparency = 1
SpeedLabel.TextSize = 13
SpeedLabel.Font = Enum.Font.SourceSansBold
SpeedLabel.TextXAlignment = Enum.TextXAlignment.Left

local SpeedBar = Instance.new("Frame")
SpeedBar.Parent = SettingsFrame
SpeedBar.Position = UDim2.new(0, 0, 0, 72)
SpeedBar.Size = UDim2.new(1, 0, 0, 5)
SpeedBar.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
SpeedBar.BackgroundTransparency = 1

local SpeedDot = Instance.new("TextButton")
SpeedDot.Parent = SpeedBar
SpeedDot.Position = UDim2.new(0.5, -6, -1.2, 0)
SpeedDot.Size = UDim2.new(0, 14, 0, 14)
SpeedDot.BackgroundColor3 = Color3.fromRGB(200, 20, 30)
SpeedDot.BackgroundTransparency = 1
SpeedDot.Text = ""
Instance.new("UICorner", SpeedDot).CornerRadius = UDim.new(1, 0)

local function createToggle(name, text, pos, defaultState, callback)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Parent = SettingsFrame
    button.Position = pos
    button.Size = UDim2.new(1, 0, 0, 35)
    button.BackgroundColor3 = defaultState and Color3.fromRGB(20, 35, 20) or Color3.fromRGB(25, 25, 28)
    button.BackgroundTransparency = 1
    button.Text = text .. (defaultState and " [ON]" or " [OFF]")
    button.TextColor3 = defaultState and Color3.fromRGB(40, 200, 40) or Color3.fromRGB(160, 160, 160)
    button.TextTransparency = 1
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 13
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 5)
    corner.Parent = button
    
    local stroke = Instance.new("UIStroke")
    stroke.Color = defaultState and Color3.fromRGB(40, 200, 40) or Color3.fromRGB(45, 45, 50)
    stroke.Thickness = 1
    stroke.Transparency = 1
    stroke.Parent = button
    
    local active = defaultState
    button.MouseButton1Click:Connect(function()
        active = not active
        if active then
            button.BackgroundColor3 = Color3.fromRGB(20, 35, 20)
            button.Text = text .. " [ON]"
            button.TextColor3 = Color3.fromRGB(40, 200, 40)
            stroke.Color = Color3.fromRGB(40, 200, 40)
        else
            button.BackgroundColor3 = Color3.fromRGB(25, 25, 28)
            button.Text = text .. " [OFF]"
            button.TextColor3 = Color3.fromRGB(160, 160, 160)
            stroke.Color = Color3.fromRGB(45, 45, 50)
        end
        callback(active)
    end)
    return button, stroke
end

local SlideBtn, SlideStrk = createToggle("SlideToggle", "🖼️ Slideshow Animation", UDim2.new(0, 0, 0, 95), true, function(state) _G.AllowSlideShow = state end)
local AFKBtn, AFKStrk = createToggle("AFKToggle", "⏳ Anti-AFK AntiDisconnect", UDim2.new(0, 0, 0, 140), true, function(state) _G.AntiAFKActive = state end)

local CloseButton = Instance.new("TextButton")
CloseButton.Parent = MainFrame
CloseButton.Position = UDim2.new(0.91, 0, 0, 5)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.BackgroundTransparency = 1
CloseButton.Text = "✕"
CloseButton.TextColor3 = Color3.fromRGB(160, 160, 160)
CloseButton.TextTransparency = 1
CloseButton.TextSize = 18

-- INTRO ĐEN ĐỎ ĐỘC LẬP
local TweenService = game:GetService("TweenService")
local function PlayEnhancedIntro()
    task.wait(0.1)
    task.spawn(function()
        while MainFrame and MainFrame.Parent do
            local toBlack = TweenService:Create(UIStroke, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Color = Color3.fromRGB(20, 20, 25)})
            if toBlack then toBlack:Play() toBlack.Completed:Wait() end
            local toRed = TweenService:Create(UIStroke, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {Color = Color3.fromRGB(255, 0, 35)})
            if toRed then toRed:Play() toRed.Completed:Wait() end
        end
    end)
    
    local frameTween = TweenService:Create(MainFrame, TweenInfo.new(1.2, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, 340, 0, 350),
        Position = UDim2.new(0.32, 0, 0.22, 0),
        BackgroundTransparency = 0,
        Rotation = 0
    })
    frameTween:Play()
    
    frameTween.Completed:Connect(function()
        local function fadeIn(element, delayTime)
            task.wait(delayTime)
            if element and element.Parent then
                TweenService:Create(element, TweenInfo.new(0.4), {TextTransparency = 0}):Play()
                if element:IsA("TextButton") and element.Name ~= "CloseButton" then
                    TweenService:Create(element, TweenInfo.new(0.4), {BackgroundTransparency = 0}):Play()
                end
            end
        end
        
        task.spawn(fadeIn, Title, 0)
        task.spawn(fadeIn, UptimeLabel, 0.1)
        task.spawn(fadeIn, FarmToggle, 0.2)
        task.spawn(fadeIn, GlideToggle, 0.2)
        task.spawn(fadeIn, CloseButton, 0.2)
        
        task.spawn(function()
            task.wait(0.3)
            if DistLabel and DistLabel.Parent then
                TweenService:Create(DistLabel, TweenInfo.new(0.4), {TextTransparency = 0}):Play()
                TweenService:Create(DistBar, TweenInfo.new(0.4), {BackgroundTransparency = 0}):Play()
                TweenService:Create(DistDot, TweenInfo.new(0.4), {BackgroundTransparency = 0}):Play()
                TweenService:Create(FarmStroke, TweenInfo.new(0.4), {Transparency = 0}):Play()
                TweenService:Create(GlideStroke, TweenInfo.new(0.4), {Transparency = 0}):Play()
            end
        end)
        
        task.spawn(function()
            task.wait(0.45)
            if SpeedLabel and SpeedLabel.Parent then
                TweenService:Create(SpeedLabel, TweenInfo.new(0.4), {TextTransparency = 0}):Play()
                TweenService:Create(SpeedBar, TweenInfo.new(0.4), {BackgroundTransparency = 0}):Play()
                TweenService:Create(SpeedDot, TweenInfo.new(0.4), {BackgroundTransparency = 0}):Play()
            end
        end)
        
        task.spawn(function()
            task.wait(0.6)
            if SlideBtn and SlideBtn.Parent then
                TweenService:Create(SlideBtn, TweenInfo.new(0.4), {TextTransparency = 0, BackgroundTransparency = 0}):Play()
                TweenService:Create(SlideStrk, TweenInfo.new(0.4), {Transparency = 0}):Play()
            end
        end)
        
        task.spawn(function()
            task.wait(0.75)
            if AFKBtn and AFKBtn.Parent then
                TweenService:Create(AFKBtn, TweenInfo.new(0.4), {TextTransparency = 0, BackgroundTransparency = 0}):Play()
                TweenService:Create(AFKStrk, TweenInfo.new(0.4), {Transparency = 0}):Play()
            end
        end)
    end)
end

task.spawn(PlayEnhancedIntro)

task.spawn(function()
    while true do
        task.wait(1.5)
        if _G.AllowSlideShow and ToggleButton and ToggleButton.Parent then
            currentImageIndex = currentImageIndex + 1
            if currentImageIndex > #ChadImages then currentImageIndex = 1 end
            ToggleButton.Image = ChadImages[currentImageIndex]
            TweenService:Create(ToggleButton, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Rotation = 8}):Play()
            task.wait(0.2)
            TweenService:Create(ToggleButton, TweenInfo.new(0.2, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Rotation = 0}):Play()
        end
    end
end)

local function openUI()
    MainFrame.Size = UDim2.new(0, 40, 0, 40)
    MainFrame.Position = UDim2.new(0.5, -20, 0.45, -20)
    MainFrame.BackgroundTransparency = 0.5
    MainFrame.Visible = true
    ToggleButton.Visible = false
    TweenService:Create(MainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, 340, 0, 350),
        Position = UDim2.new(0.32, 0, 0.22, 0),
        BackgroundTransparency = 0
    }):Play()
end

local function closeUI()
    local closeTween = TweenService:Create(MainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
        Size = UDim2.new(0, 10, 0, 10),
        Position = UDim2.new(0.5, -5, 0.45, -5),
        BackgroundTransparency = 1
    })
    closeTween:Play()
    closeTween.Completed:Connect(function()
        MainFrame.Visible = false
        ToggleButton.Size = UDim2.new(0, 140, 0, 140)
        ToggleButton.Position = UDim2.new(0.5, -70, 0.45, -70)
        ToggleStroke.Thickness = 6
        ToggleButton.Visible = true
        task.wait(0.15)
        local gatherTween = TweenService:Create(ToggleButton, TweenInfo.new(0.8, Enum.EasingStyle.Elastic, Enum.EasingDirection.Out), {
            Size = UDim2.new(0, 55, 0, 55),
            Position = UDim2.new(0.02, 0, 0.2, 0)
        })
        TweenService:Create(ToggleStroke, TweenInfo.new(0.6), {Thickness = 2.5, Color = Color3.fromRGB(200, 20, 30)}):Play()
        gatherTween:Play()
    end)
end

CloseButton.MouseButton1Click:Connect(closeUI)
ToggleButton.MouseButton1Click:Connect(openUI)

local UserInputService = game:GetService("UserInputService")
local function setupSlider(dot, bar, minVal, maxVal, isDistance)
    local dragging = false
    dot.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local percentage = math.clamp((input.Position.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
            dot.Position = UDim2.new(percentage, -6, -1.2, 0)
            local calculated = minVal + (percentage * (maxVal - minVal))
            if isDistance then
                _G.FarmDistance = math.round(calculated * 10) / 10
                DistLabel.Text = string.format("📏 Target Distance: %.1f Studs", _G.FarmDistance)
            else
                _G.TweenSpeedPercent = math.round(calculated)
                SpeedLabel.Text = string.format("⚡ Tween Throttle: %d%%", _G.TweenSpeedPercent)
            end
        end
    end)
end

setupSlider(DistDot, DistBar, 1, 20, true)
setupSlider(SpeedDot, SpeedBar, 10, 100, false)

task.spawn(function()
    while task.wait(1) do
        local duration = os.time() - StartTime
        local hours = math.floor(duration / 3600)
        local minutes = math.floor((duration % 3600) / 60)
        local seconds = duration % 60
        UptimeLabel.Text = string.format("💀 Uptime: %02dh %02dm %02ds", hours, minutes, seconds)
    end
end)

-- =========================================================
-- LOGIC QUÉT VÀ DI CHUYỂN AUTO FARM & LƯỚT XUYÊN TƯỜNG (GHOST GLIDE)
-- =========================================================
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local AutoFarmActive = false
local IsRescuing = false 
local currentTween = nil

local function getZombieFolder()
    return workspace:FindFirstChild("ActiveZombies") or workspace:FindFirstChild("Active zombies") or workspace:FindFirstChild("Active Zombies")
end

local function getClosestActiveZombie()
    local closestZombie = nil
    local shortestDistance = math.huge
    local zombieFolder = getZombieFolder()
    if not zombieFolder then return nil end
    
    for _, obj in pairs(zombieFolder:GetChildren()) do
        local humanoid = obj:FindFirstChild("Humanoid")
        local hrp = obj:FindFirstChild("HumanoidRootPart")
        if humanoid and hrp and humanoid.Health > 0 and obj.Name ~= LocalPlayer.Name then
            local isRagdoll = humanoid.PlatformStand or humanoid:GetState() == Enum.HumanoidStateType.FallingDown or humanoid:GetState() == Enum.HumanoidStateType.Physics
            if not isRagdoll then
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local distance = (LocalPlayer.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                    if distance < shortestDistance then
                        shortestDistance = distance
                        closestZombie = obj
                    end
                end
            end
        end
    end
    return closestZombie
end

-- VÒNG LẶP STEPPED XỬ LÝ VA CHẠM VÀ TWEEN DI CHUYỂN THEO PHÍM (GHOST GLIDE)
RunService.Stepped:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") and not IsRescuing then
        local character = LocalPlayer.Character
        local hrp = character.HumanoidRootPart
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        
        -- Phá Ragdoll chống lỗi kẹt trạng thái cơ thể
        if humanoid then
            if humanoid.PlatformStand then humanoid.PlatformStand = false end
            local state = humanoid:GetState()
            if state == Enum.HumanoidStateType.FallingDown or state == Enum.HumanoidStateType.Ragdoll or state == Enum.HumanoidStateType.Physics then
                humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
            end
     
