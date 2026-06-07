-- ╔══════════════════════════════════════════╗
-- ║        QM GUI Library - v1.0             ║
-- ║     Designed & Coded by QM               ║
-- ║     Rayfield-Inspired Professional UI    ║
-- ╚══════════════════════════════════════════╝

local VirtualInputManager = game:GetService("VirtualInputManager")
local runS = game:GetService("RunService")
local pl = game:GetService("Players")
local lp = pl.LocalPlayer
local char = lp.Character or lp.CharacterAdded:Wait()
local ws = game:GetService("Workspace")
local camera = workspace.CurrentCamera
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local mousePPos = UIS:GetMouseLocation()
runS.RenderStepped:Connect(function() mousePPos = UIS:GetMouseLocation() end)
local Center = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)

-- Settings
local FullSettings = {
    AimBot = {
        Checks = {
            TeamCheck = true,
            WallCheck = true,
            AliveCheck = true
        },
        Fov = {
            Enable = true,
            Visible = true,
            Thickness = 0.6,
            Color = Color3.fromRGB(255, 255, 255),
            LockColor = Color3.fromRGB(255, 0, 0),
            OffColor = Color3.fromRGB(150, 150, 150),
            Filled = false,
            Size = 360
        },
        Values = {
            Enable = true,
            Toggle = true,
            HitPart = "HitboxHead",
            HitPartList = {"Head", "LeftFoot", "LeftHand", "LeftLowerArm", "LeftLowerLeg", "LeftUpperArm", "LowerTorso", "RightFoot", "RightHand", "RightLowerArm", "RightLowerLeg", "RightUpperArm", "RightUpperLeg", "UpperTorso", "HitboxBody", "FakeMass", "HitboxBodySmall", "HumanoidRootPart"},
            TriggerKey = Enum.UserInputType.MouseButton2,
        }
    },
    Esp = {
        Checks = {
            TeamCheck = true,
            WallCheck = false,
            AliveCheck = true
        },
        Values = {
            Enabled = true,
            FillColor = Color3.fromRGB(255, 255, 255),
            FillTransparency = 0.5,
            OutlineColor = Color3.fromRGB(200, 200, 200),
            OutlineTransparency = 0
        }
    },
    Movement = {
        Fly = {
            Enabled = false,
            Speed = 50
        }
    }
}

-- ══════════════════════════════════════════
--              GUI CREATION
-- ══════════════════════════════════════════

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "QM_GUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = lp.PlayerGui

-- Theme
local Theme = {
    Background = Color3.fromRGB(15, 15, 20),
    Sidebar = Color3.fromRGB(20, 20, 28),
    TopBar = Color3.fromRGB(18, 18, 25),
    Card = Color3.fromRGB(25, 25, 35),
    CardHover = Color3.fromRGB(30, 30, 42),
    Accent = Color3.fromRGB(100, 130, 255),
    AccentDark = Color3.fromRGB(70, 100, 220),
    Text = Color3.fromRGB(230, 230, 240),
    TextDim = Color3.fromRGB(140, 140, 160),
    Border = Color3.fromRGB(40, 40, 55),
    Toggle_On = Color3.fromRGB(100, 130, 255),
    Toggle_Off = Color3.fromRGB(50, 50, 65),
    Slider_Track = Color3.fromRGB(40, 40, 55),
    Danger = Color3.fromRGB(255, 80, 80),
    Success = Color3.fromRGB(80, 220, 120),
}

-- Utility
local function Tween(obj, props, duration, style, dir)
    local ti = TweenInfo.new(duration or 0.25, style or Enum.EasingStyle.Quart, dir or Enum.EasingDirection.Out)
    TweenService:Create(obj, ti, props):Play()
end

local function MakeRound(obj, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius or 8)
    c.Parent = obj
    return c
end

local function MakePadding(obj, t, b, l, r)
    local p = Instance.new("UIPadding")
    p.PaddingTop = UDim.new(0, t or 8)
    p.PaddingBottom = UDim.new(0, b or 8)
    p.PaddingLeft = UDim.new(0, l or 12)
    p.PaddingRight = UDim.new(0, r or 12)
    p.Parent = obj
    return p
end

local function MakeStroke(obj, color, thickness, transparency)
    local s = Instance.new("UIStroke")
    s.Color = color or Theme.Border
    s.Thickness = thickness or 1
    s.Transparency = transparency or 0
    s.Parent = obj
    return s
end

-- ══════════════════════════════════════════
--              MAIN WINDOW
-- ══════════════════════════════════════════

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 660, 0, 480)  -- Aumentado para más espacio
MainFrame.Position = UDim2.new(0.5, -330, 0.5, -240)
MainFrame.BackgroundColor3 = Theme.Background
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui
MakeRound(MainFrame, 12)
MakeStroke(MainFrame, Theme.Border, 1)

-- Drop Shadow
local Shadow = Instance.new("ImageLabel")
Shadow.Name = "Shadow"
Shadow.AnchorPoint = Vector2.new(0.5, 0.5)
Shadow.BackgroundTransparency = 1
Shadow.Position = UDim2.new(0.5, 0, 0.5, 8)
Shadow.Size = UDim2.new(1, 40, 1, 40)
Shadow.ZIndex = 0
Shadow.Image = "rbxassetid://6014261993"
Shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
Shadow.ImageTransparency = 0.4
Shadow.ScaleType = Enum.ScaleType.Slice
Shadow.SliceCenter = Rect.new(49, 49, 450, 450)
Shadow.Parent = MainFrame

-- Animate in
MainFrame.Size = UDim2.new(0, 0, 0, 0)
MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
Tween(MainFrame, {Size = UDim2.new(0, 660, 0, 480), Position = UDim2.new(0.5, -330, 0.5, -240)}, 0.5, Enum.EasingStyle.Back)

-- Dragging
local dragging, dragStart, startPos
MainFrame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)
MainFrame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
UIS.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- ══════════════════════════════════════════
--              TOP BAR
-- ══════════════════════════════════════════

local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 48)
TopBar.Position = UDim2.new(0, 0, 0, 0)
TopBar.BackgroundColor3 = Theme.TopBar
TopBar.BorderSizePixel = 0
TopBar.ZIndex = 5
TopBar.Parent = MainFrame

local AccentLine = Instance.new("Frame")
AccentLine.Size = UDim2.new(1, 0, 0, 2)
AccentLine.Position = UDim2.new(0, 0, 1, -2)
AccentLine.BackgroundColor3 = Theme.Accent
AccentLine.BorderSizePixel = 0
AccentLine.Parent = TopBar

local AccentGrad = Instance.new("UIGradient")
AccentGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Theme.AccentDark),
    ColorSequenceKeypoint.new(0.5, Theme.Accent),
    ColorSequenceKeypoint.new(1, Theme.AccentDark),
})
AccentGrad.Parent = AccentLine

local LogoFrame = Instance.new("Frame")
LogoFrame.Size = UDim2.new(0, 36, 0, 36)
LogoFrame.Position = UDim2.new(0, 8, 0.5, -18)
LogoFrame.BackgroundColor3 = Theme.Accent
LogoFrame.BorderSizePixel = 0
LogoFrame.Parent = TopBar
MakeRound(LogoFrame, 8)

local LogoLabel = Instance.new("TextLabel")
LogoLabel.Text = "Q"
LogoLabel.Font = Enum.Font.GothamBold
LogoLabel.TextSize = 20
LogoLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
LogoLabel.BackgroundTransparency = 1
LogoLabel.Size = UDim2.new(1, 0, 1, 0)
LogoLabel.Parent = LogoFrame

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Text = "QM Cheat Suite"
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 16
TitleLabel.TextColor3 = Theme.Text
TitleLabel.BackgroundTransparency = 1
TitleLabel.Position = UDim2.new(0, 54, 0, 8)
TitleLabel.Size = UDim2.new(0, 200, 0, 18)
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TopBar

local SubLabel = Instance.new("TextLabel")
SubLabel.Text = "by QM  •  v1.0"
SubLabel.Font = Enum.Font.Gotham
SubLabel.TextSize = 11
SubLabel.TextColor3 = Theme.TextDim
SubLabel.BackgroundTransparency = 1
SubLabel.Position = UDim2.new(0, 54, 0, 27)
SubLabel.Size = UDim2.new(0, 200, 0, 14)
SubLabel.TextXAlignment = Enum.TextXAlignment.Left
SubLabel.Parent = TopBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Text = "✕"
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 14
CloseBtn.TextColor3 = Theme.TextDim
CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Size = UDim2.new(0, 32, 0, 32)
CloseBtn.Position = UDim2.new(1, -40, 0.5, -16)
CloseBtn.Parent = TopBar
MakeRound(CloseBtn, 6)

CloseBtn.MouseEnter:Connect(function()
    Tween(CloseBtn, {BackgroundTransparency = 0, TextColor3 = Color3.fromRGB(255, 255, 255)}, 0.15)
end)
CloseBtn.MouseLeave:Connect(function()
    Tween(CloseBtn, {BackgroundTransparency = 1, TextColor3 = Theme.TextDim}, 0.15)
end)
CloseBtn.MouseButton1Click:Connect(function()
    Tween(MainFrame, {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}, 0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In)
    task.wait(0.35)
    ScreenGui:Destroy()
end)

local MinBtn = Instance.new("TextButton")
MinBtn.Text = "─"
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 14
MinBtn.TextColor3 = Theme.TextDim
MinBtn.BackgroundColor3 = Color3.fromRGB(255, 190, 0)
MinBtn.BackgroundTransparency = 1
MinBtn.Size = UDim2.new(0, 32, 0, 32)
MinBtn.Position = UDim2.new(1, -76, 0.5, -16)
MinBtn.Parent = TopBar
MakeRound(MinBtn, 6)

local minimized = false
MinBtn.MouseEnter:Connect(function()
    Tween(MinBtn, {BackgroundTransparency = 0, TextColor3 = Color3.fromRGB(255, 255, 255)}, 0.15)
end)
MinBtn.MouseLeave:Connect(function()
    Tween(MinBtn, {BackgroundTransparency = 1, TextColor3 = Theme.TextDim}, 0.15)
end)
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        Tween(MainFrame, {Size = UDim2.new(0, 660, 0, 48)}, 0.3)
    else
        Tween(MainFrame, {Size = UDim2.new(0, 660, 0, 480)}, 0.3)
    end
end)

-- ══════════════════════════════════════════
--              SIDEBAR (CON SCROLL)
-- ══════════════════════════════════════════

local Sidebar = Instance.new("ScrollingFrame")  -- Cambiado a ScrollingFrame
Sidebar.Name = "Sidebar"
Sidebar.Size = UDim2.new(0, 150, 1, -48)
Sidebar.Position = UDim2.new(0, 0, 0, 48)
Sidebar.BackgroundColor3 = Theme.Sidebar
Sidebar.BorderSizePixel = 0
Sidebar.ScrollBarThickness = 4
Sidebar.ScrollBarImageColor3 = Theme.Accent
Sidebar.CanvasSize = UDim2.new(0, 0, 0, 0)
Sidebar.AutomaticCanvasSize = Enum.AutomaticSize.Y
Sidebar.Parent = MainFrame

local SidebarBorder = Instance.new("Frame")
SidebarBorder.Size = UDim2.new(0, 1, 1, 0)
SidebarBorder.Position = UDim2.new(1, -1, 0, 0)
SidebarBorder.BackgroundColor3 = Theme.Border
SidebarBorder.BorderSizePixel = 0
SidebarBorder.Parent = Sidebar

local SidebarList = Instance.new("UIListLayout")
SidebarList.SortOrder = Enum.SortOrder.LayoutOrder
SidebarList.Padding = UDim.new(0, 4)
SidebarList.Parent = Sidebar

local SidebarPad = Instance.new("UIPadding")
SidebarPad.PaddingTop = UDim.new(0, 12)
SidebarPad.PaddingLeft = UDim.new(0, 8)
SidebarPad.PaddingRight = UDim.new(0, 8)
SidebarPad.PaddingBottom = UDim.new(0, 12)
SidebarPad.Parent = Sidebar

-- ══════════════════════════════════════════
--              CONTENT AREA
-- ══════════════════════════════════════════

local ContentArea = Instance.new("Frame")
ContentArea.Name = "ContentArea"
ContentArea.Size = UDim2.new(1, -150, 1, -48)
ContentArea.Position = UDim2.new(0, 150, 0, 48)
ContentArea.BackgroundColor3 = Theme.Background
ContentArea.BorderSizePixel = 0
ContentArea.ClipsDescendants = true
ContentArea.Parent = MainFrame

-- ══════════════════════════════════════════
--              TAB SYSTEM
-- ══════════════════════════════════════════

local Tabs = {}
local ActiveTab = nil

local function CreateTab(name, icon)
    local TabBtn = Instance.new("TextButton")
    TabBtn.Name = name .. "Btn"
    TabBtn.Size = UDim2.new(1, 0, 0, 36)
    TabBtn.BackgroundColor3 = Theme.Card
    TabBtn.BackgroundTransparency = 1
    TabBtn.Text = ""
    TabBtn.BorderSizePixel = 0
    TabBtn.LayoutOrder = #Tabs + 1
    TabBtn.Parent = Sidebar
    MakeRound(TabBtn, 8)

    local BtnIcon = Instance.new("TextLabel")
    BtnIcon.Text = icon or "◆"
    BtnIcon.Font = Enum.Font.GothamBold
    BtnIcon.TextSize = 13
    BtnIcon.TextColor3 = Theme.TextDim
    BtnIcon.BackgroundTransparency = 1
    BtnIcon.Position = UDim2.new(0, 8, 0, 0)
    BtnIcon.Size = UDim2.new(0, 20, 1, 0)
    BtnIcon.TextXAlignment = Enum.TextXAlignment.Center
    BtnIcon.Parent = TabBtn

    local BtnLabel = Instance.new("TextLabel")
    BtnLabel.Text = name
    BtnLabel.Font = Enum.Font.Gotham
    BtnLabel.TextSize = 13
    BtnLabel.TextColor3 = Theme.TextDim
    BtnLabel.BackgroundTransparency = 1
    BtnLabel.Position = UDim2.new(0, 30, 0, 0)
    BtnLabel.Size = UDim2.new(1, -30, 1, 0)
    BtnLabel.TextXAlignment = Enum.TextXAlignment.Left
    BtnLabel.Parent = TabBtn

    local ActiveBar = Instance.new("Frame")
    ActiveBar.Size = UDim2.new(0, 3, 0.6, 0)
    ActiveBar.Position = UDim2.new(0, 0, 0.2, 0)
    ActiveBar.BackgroundColor3 = Theme.Accent
    ActiveBar.BorderSizePixel = 0
    ActiveBar.BackgroundTransparency = 1
    ActiveBar.Parent = TabBtn
    MakeRound(ActiveBar, 2)

    local Page = Instance.new("ScrollingFrame")
    Page.Name = name .. "Page"
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.Position = UDim2.new(0, 0, 0, 0)
    Page.BackgroundTransparency = 1
    Page.BorderSizePixel = 0
    Page.ScrollBarThickness = 4
    Page.ScrollBarImageColor3 = Theme.Accent
    Page.CanvasSize = UDim2.new(0, 0, 0, 0)
    Page.AutomaticCanvasSize = Enum.AutomaticSize.Y
    Page.Visible = false
    Page.Parent = ContentArea

    local PageList = Instance.new("UIListLayout")
    PageList.SortOrder = Enum.SortOrder.LayoutOrder
    PageList.Padding = UDim.new(0, 8)
    PageList.Parent = Page

    local PagePad = Instance.new("UIPadding")
    PagePad.PaddingTop = UDim.new(0, 14)
    PagePad.PaddingBottom = UDim.new(0, 14)
    PagePad.PaddingLeft = UDim.new(0, 14)
    PagePad.PaddingRight = UDim.new(0, 14)
    PagePad.Parent = Page

    local tab = {
        Button = TabBtn,
        Page = Page,
        Icon = BtnIcon,
        Label = BtnLabel,
        Bar = ActiveBar,
        LayoutOrder = #Tabs + 1,
    }

    TabBtn.MouseButton1Click:Connect(function()
        if ActiveTab == tab then return end
        if ActiveTab then
            ActiveTab.Page.Visible = false
            Tween(ActiveTab.Button, {BackgroundTransparency = 1}, 0.15)
            Tween(ActiveTab.Label, {TextColor3 = Theme.TextDim}, 0.15)
            Tween(ActiveTab.Icon, {TextColor3 = Theme.TextDim}, 0.15)
            Tween(ActiveTab.Bar, {BackgroundTransparency = 1}, 0.15)
        end
        ActiveTab = tab
        tab.Page.Visible = true
        Tween(TabBtn, {BackgroundTransparency = 0.7}, 0.15)
        Tween(BtnLabel, {TextColor3 = Theme.Accent}, 0.15)
        Tween(BtnIcon, {TextColor3 = Theme.Accent}, 0.15)
        Tween(ActiveBar, {BackgroundTransparency = 0}, 0.15)
    end)

    TabBtn.MouseEnter:Connect(function()
        if ActiveTab == tab then return end
        Tween(TabBtn, {BackgroundTransparency = 0.85}, 0.15)
    end)
    TabBtn.MouseLeave:Connect(function()
        if ActiveTab == tab then return end
        Tween(TabBtn, {BackgroundTransparency = 1}, 0.15)
    end)

    Tabs[name] = tab
    return tab
end

-- ══════════════════════════════════════════
--           COMPONENT LIBRARY
-- ══════════════════════════════════════════

local function CreateSection(parent, title)
    local SectionFrame = Instance.new("Frame")
    SectionFrame.Size = UDim2.new(1, 0, 0, 0)
    SectionFrame.BackgroundTransparency = 1
    SectionFrame.AutomaticSize = Enum.AutomaticSize.Y
    SectionFrame.BorderSizePixel = 0
    SectionFrame.Parent = parent

    local SectionList = Instance.new("UIListLayout")
    SectionList.SortOrder = Enum.SortOrder.LayoutOrder
    SectionList.Padding = UDim.new(0, 6)
    SectionList.Parent = SectionFrame

    local Header = Instance.new("Frame")
    Header.Size = UDim2.new(1, 0, 0, 22)
    Header.BackgroundTransparency = 1
    Header.LayoutOrder = 0
    Header.Parent = SectionFrame

    local HeaderLine = Instance.new("Frame")
    HeaderLine.Size = UDim2.new(1, 0, 0, 1)
    HeaderLine.Position = UDim2.new(0, 0, 0.5, 0)
    HeaderLine.BackgroundColor3 = Theme.Border
    HeaderLine.BorderSizePixel = 0
    HeaderLine.Parent = Header

    local HeaderLabel = Instance.new("TextLabel")
    HeaderLabel.Text = "  " .. title .. "  "
    HeaderLabel.Font = Enum.Font.GothamBold
    HeaderLabel.TextSize = 11
    HeaderLabel.TextColor3 = Theme.TextDim
    HeaderLabel.BackgroundColor3 = Theme.Background
    HeaderLabel.BorderSizePixel = 0
    HeaderLabel.Size = UDim2.new(0, 0, 1, 0)
    HeaderLabel.AutomaticSize = Enum.AutomaticSize.X
    HeaderLabel.Position = UDim2.new(0, 10, 0, 0)
    HeaderLabel.TextXAlignment = Enum.TextXAlignment.Left
    HeaderLabel.Parent = Header

    return SectionFrame
end

local function CreateToggle(parent, labelText, default, callback)
    local ToggleFrame = Instance.new("Frame")
    ToggleFrame.Size = UDim2.new(1, 0, 0, 40)
    ToggleFrame.BackgroundColor3 = Theme.Card
    ToggleFrame.BorderSizePixel = 0
    ToggleFrame.Parent = parent
    MakeRound(ToggleFrame, 8)
    MakeStroke(ToggleFrame, Theme.Border, 1)

    local Label = Instance.new("TextLabel")
    Label.Text = labelText
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 13
    Label.TextColor3 = Theme.Text
    Label.BackgroundTransparency = 1
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.Size = UDim2.new(1, -70, 1, 0)
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = ToggleFrame

    local SwitchBg = Instance.new("Frame")
    SwitchBg.Size = UDim2.new(0, 38, 0, 20)
    SwitchBg.Position = UDim2.new(1, -50, 0.5, -10)
    SwitchBg.BackgroundColor3 = default and Theme.Toggle_On or Theme.Toggle_Off
    SwitchBg.BorderSizePixel = 0
    SwitchBg.Parent = ToggleFrame
    MakeRound(SwitchBg, 10)

    local Knob = Instance.new("Frame")
    Knob.Size = UDim2.new(0, 14, 0, 14)
    Knob.Position = default and UDim2.new(0, 21, 0.5, -7) or UDim2.new(0, 3, 0.5, -7)
    Knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    Knob.BorderSizePixel = 0
    Knob.Parent = SwitchBg
    MakeRound(Knob, 7)

    local value = default
    local Clickable = Instance.new("TextButton")
    Clickable.Size = UDim2.new(1, 0, 1, 0)
    Clickable.BackgroundTransparency = 1
    Clickable.Text = ""
    Clickable.Parent = ToggleFrame

    Clickable.MouseEnter:Connect(function()
        Tween(ToggleFrame, {BackgroundColor3 = Theme.CardHover}, 0.15)
    end)
    Clickable.MouseLeave:Connect(function()
        Tween(ToggleFrame, {BackgroundColor3 = Theme.Card}, 0.15)
    end)

    Clickable.MouseButton1Click:Connect(function()
        value = not value
        if value then
            Tween(SwitchBg, {BackgroundColor3 = Theme.Toggle_On}, 0.2)
            Tween(Knob, {Position = UDim2.new(0, 21, 0.5, -7)}, 0.2)
        else
            Tween(SwitchBg, {BackgroundColor3 = Theme.Toggle_Off}, 0.2)
            Tween(Knob, {Position = UDim2.new(0, 3, 0.5, -7)}, 0.2)
        end
        if callback then callback(value) end
    end)

    return {Frame = ToggleFrame, GetValue = function() return value end, SetValue = function(v)
        value = v
        if value then
            Tween(SwitchBg, {BackgroundColor3 = Theme.Toggle_On}, 0.2)
            Tween(Knob, {Position = UDim2.new(0, 21, 0.5, -7)}, 0.2)
        else
            Tween(SwitchBg, {BackgroundColor3 = Theme.Toggle_Off}, 0.2)
            Tween(Knob, {Position = UDim2.new(0, 3, 0.5, -7)}, 0.2)
        end
    end}
end

local function CreateSlider(parent, labelText, min, max, default, callback)
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(1, 0, 0, 52)
    SliderFrame.BackgroundColor3 = Theme.Card
    SliderFrame.BorderSizePixel = 0
    SliderFrame.Parent = parent
    MakeRound(SliderFrame, 8)
    MakeStroke(SliderFrame, Theme.Border, 1)

    local Label = Instance.new("TextLabel")
    Label.Text = labelText
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 13
    Label.TextColor3 = Theme.Text
    Label.BackgroundTransparency = 1
    Label.Position = UDim2.new(0, 12, 0, 6)
    Label.Size = UDim2.new(1, -70, 0, 18)
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = SliderFrame

    local ValueLabel = Instance.new("TextLabel")
    ValueLabel.Text = tostring(default)
    ValueLabel.Font = Enum.Font.GothamBold
    ValueLabel.TextSize = 12
    ValueLabel.TextColor3 = Theme.Accent
    ValueLabel.BackgroundTransparency = 1
    ValueLabel.Position = UDim2.new(1, -55, 0, 6)
    ValueLabel.Size = UDim2.new(0, 45, 0, 18)
    ValueLabel.TextXAlignment = Enum.TextXAlignment.Right
    ValueLabel.Parent = SliderFrame

    local TrackBg = Instance.new("Frame")
    TrackBg.Size = UDim2.new(1, -24, 0, 6)
    TrackBg.Position = UDim2.new(0, 12, 0, 34)
    TrackBg.BackgroundColor3 = Theme.Slider_Track
    TrackBg.BorderSizePixel = 0
    TrackBg.ClipsDescendants = true
    TrackBg.Parent = SliderFrame
    MakeRound(TrackBg, 3)

    local TrackFill = Instance.new("Frame")
    local fillPct = (default - min) / (max - min)
    TrackFill.Size = UDim2.new(fillPct, 0, 1, 0)
    TrackFill.BackgroundColor3 = Theme.Accent
    TrackFill.BorderSizePixel = 0
    TrackFill.Parent = TrackBg
    MakeRound(TrackFill, 3)

    local Knob = Instance.new("Frame")
    Knob.Size = UDim2.new(0, 14, 0, 14)
    Knob.AnchorPoint = Vector2.new(0.5, 0.5)
    Knob.Position = UDim2.new(fillPct, 0, 0.5, 0)
    Knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    Knob.BorderSizePixel = 0
    Knob.ZIndex = 3
    Knob.Parent = TrackBg
    MakeRound(Knob, 7)
    MakeStroke(Knob, Theme.Accent, 2)

    local value = default
    local draggingSlider = false

    local function UpdateSlider(inputX)
        local absX = TrackBg.AbsolutePosition.X
        local absW = TrackBg.AbsoluteSize.X
        local pct = math.clamp((inputX - absX) / absW, 0, 1)
        value = math.floor(min + (max - min) * pct + 0.5)
        ValueLabel.Text = tostring(value)
        Tween(TrackFill, {Size = UDim2.new(pct, 0, 1, 0)}, 0.05)
        Tween(Knob, {Position = UDim2.new(pct, 0, 0.5, 0)}, 0.05)
        if callback then callback(value) end
    end

    TrackBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            draggingSlider = true
            UpdateSlider(input.Position.X)
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if draggingSlider and input.UserInputType == Enum.UserInputType.MouseMovement then
            UpdateSlider(input.Position.X)
        end
    end)
    UIS.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            draggingSlider = false
        end
    end)

    return {Frame = SliderFrame, GetValue = function() return value end}
end

local function CreateDropdown(parent, labelText, options, default, callback)
    local DDFrame = Instance.new("Frame")
    DDFrame.Size = UDim2.new(1, 0, 0, 40)
    DDFrame.BackgroundColor3 = Theme.Card
    DDFrame.BorderSizePixel = 0
    DDFrame.ClipsDescendants = false
    DDFrame.ZIndex = 10
    DDFrame.Parent = parent
    MakeRound(DDFrame, 8)
    MakeStroke(DDFrame, Theme.Border, 1)

    local Label = Instance.new("TextLabel")
    Label.Text = labelText
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 13
    Label.TextColor3 = Theme.Text
    Label.BackgroundTransparency = 1
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.Size = UDim2.new(0.5, 0, 1, 0)
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.ZIndex = 10
    Label.Parent = DDFrame

    local SelectedLabel = Instance.new("TextLabel")
    SelectedLabel.Text = default or options[1]
    SelectedLabel.Font = Enum.Font.GothamBold
    SelectedLabel.TextSize = 12
    SelectedLabel.TextColor3 = Theme.Accent
    SelectedLabel.BackgroundTransparency = 1
    SelectedLabel.Position = UDim2.new(0.5, 0, 0, 0)
    SelectedLabel.Size = UDim2.new(0.5, -40, 1, 0)
    SelectedLabel.TextXAlignment = Enum.TextXAlignment.Right
    SelectedLabel.ZIndex = 10
    SelectedLabel.Parent = DDFrame

    local Arrow = Instance.new("TextLabel")
    Arrow.Text = "▾"
    Arrow.Font = Enum.Font.GothamBold
    Arrow.TextSize = 14
    Arrow.TextColor3 = Theme.TextDim
    Arrow.BackgroundTransparency = 1
    Arrow.Position = UDim2.new(1, -28, 0, 0)
    Arrow.Size = UDim2.new(0, 20, 1, 0)
    Arrow.TextXAlignment = Enum.TextXAlignment.Center
    Arrow.ZIndex = 10
    Arrow.Parent = DDFrame

    local Menu = Instance.new("Frame")
    Menu.Size = UDim2.new(1, 0, 0, 0)
    Menu.Position = UDim2.new(0, 0, 1, 4)
    Menu.BackgroundColor3 = Theme.Card
    Menu.BorderSizePixel = 0
    Menu.ClipsDescendants = true
    Menu.ZIndex = 20
    Menu.Visible = false
    Menu.Parent = DDFrame
    MakeRound(Menu, 8)
    MakeStroke(Menu, Theme.Border, 1)

    local MenuList = Instance.new("UIListLayout")
    MenuList.SortOrder = Enum.SortOrder.LayoutOrder
    MenuList.Parent = Menu

    local MenuPad = Instance.new("UIPadding")
    MenuPad.PaddingTop = UDim.new(0, 4)
    MenuPad.PaddingBottom = UDim.new(0, 4)
    MenuPad.Parent = Menu

    local isOpen = false
    local value = default or options[1]
    local ITEM_HEIGHT = 28

    for i, opt in ipairs(options) do
        local Item = Instance.new("TextButton")
        Item.Size = UDim2.new(1, 0, 0, ITEM_HEIGHT)
        Item.BackgroundColor3 = Theme.Card
        Item.BackgroundTransparency = 1
        Item.Text = opt
        Item.Font = Enum.Font.Gotham
        Item.TextSize = 12
        Item.TextColor3 = opt == value and Theme.Accent or Theme.Text
        Item.TextXAlignment = Enum.TextXAlignment.Left
        Item.ZIndex = 20
        Item.LayoutOrder = i
        Item.Parent = Menu
        MakePadding(Item, 0, 0, 12, 8)

        Item.MouseEnter:Connect(function()
            Tween(Item, {BackgroundTransparency = 0.7}, 0.1)
        end)
        Item.MouseLeave:Connect(function()
            if value ~= opt then
                Tween(Item, {BackgroundTransparency = 1}, 0.1)
            end
        end)
        Item.MouseButton1Click:Connect(function()
            value = opt
            SelectedLabel.Text = opt
            for _, child in ipairs(Menu:GetChildren()) do
                if child:IsA("TextButton") then
                    Tween(child, {TextColor3 = child.Text == value and Theme.Accent or Theme.Text, BackgroundTransparency = child.Text == value and 0.7 or 1}, 0.1)
                end
            end
            isOpen = false
            Tween(Menu, {Size = UDim2.new(1, 0, 0, 0)}, 0.2)
            Tween(Arrow, {Rotation = 0}, 0.2)
            task.wait(0.21)
            Menu.Visible = false
            if callback then callback(value) end
        end)
    end

    local Clickable = Instance.new("TextButton")
    Clickable.Size = UDim2.new(1, 0, 1, 0)
    Clickable.BackgroundTransparency = 1
    Clickable.Text = ""
    Clickable.ZIndex = 11
    Clickable.Parent = DDFrame

    Clickable.MouseButton1Click:Connect(function()
        isOpen = not isOpen
        if isOpen then
            Menu.Visible = true
            Menu.Size = UDim2.new(1, 0, 0, 0)
            Tween(Menu, {Size = UDim2.new(1, 0, 0, #options * ITEM_HEIGHT + 8)}, 0.25)
            Tween(Arrow, {Rotation = 180}, 0.2)
        else
            Tween(Menu, {Size = UDim2.new(1, 0, 0, 0)}, 0.2)
            Tween(Arrow, {Rotation = 0}, 0.2)
            task.wait(0.21)
            Menu.Visible = false
        end
    end)

    return {Frame = DDFrame, GetValue = function() return value end}
end

local function CreateColorDisplay(parent, labelText, default, callback)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, 0, 0, 40)
    Frame.BackgroundColor3 = Theme.Card
    Frame.BorderSizePixel = 0
    Frame.Parent = parent
    MakeRound(Frame, 8)
    MakeStroke(Frame, Theme.Border, 1)

    local Label = Instance.new("TextLabel")
    Label.Text = labelText
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 13
    Label.TextColor3 = Theme.Text
    Label.BackgroundTransparency = 1
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.Size = UDim2.new(1, -70, 1, 0)
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Frame

    local ColorBox = Instance.new("Frame")
    ColorBox.Size = UDim2.new(0, 28, 0, 20)
    ColorBox.Position = UDim2.new(1, -42, 0.5, -10)
    ColorBox.BackgroundColor3 = default
    ColorBox.BorderSizePixel = 0
    ColorBox.Parent = Frame
    MakeRound(ColorBox, 6)
    MakeStroke(ColorBox, Theme.Border, 1)

    return {Frame = Frame, ColorBox = ColorBox, SetColor = function(c)
        ColorBox.BackgroundColor3 = c
    end}
end

local function CreateInfo(parent, labelText, valueText)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, 0, 0, 40)
    Frame.BackgroundColor3 = Theme.Card
    Frame.BorderSizePixel = 0
    Frame.Parent = parent
    MakeRound(Frame, 8)
    MakeStroke(Frame, Theme.Border, 1)

    local Label = Instance.new("TextLabel")
    Label.Text = labelText
    Label.Font = Enum.Font.Gotham
    Label.TextSize = 13
    Label.TextColor3 = Theme.Text
    Label.BackgroundTransparency = 1
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.Size = UDim2.new(0.6, 0, 1, 0)
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Frame

    local Val = Instance.new("TextLabel")
    Val.Text = valueText
    Val.Font = Enum.Font.GothamBold
    Val.TextSize = 12
    Val.TextColor3 = Theme.Accent
    Val.BackgroundTransparency = 1
    Val.Position = UDim2.new(0.6, 0, 0, 0)
    Val.Size = UDim2.new(0.4, -12, 1, 0)
    Val.TextXAlignment = Enum.TextXAlignment.Right
    Val.Parent = Frame

    return {Frame = Frame, ValueLabel = Val}
end

-- ══════════════════════════════════════════
--              CREATE TABS
-- ══════════════════════════════════════════

local AimbotTab = CreateTab("Aimbot", "◎")
local EspTab = CreateTab("ESP", "◈")
local MovementTab = CreateTab("Movement", "▲")
local SettingsTab = CreateTab("Settings", "⚙")

-- ══════════════════════════════════════════
--              AIMBOT TAB
-- ══════════════════════════════════════════

local AimbotPage = AimbotTab.Page

local CoreSection = CreateSection(AimbotPage, "CORE")

CreateToggle(CoreSection, "Enable Aimbot", FullSettings.AimBot.Values.Enable, function(v)
    FullSettings.AimBot.Values.Enable = v
end)

CreateToggle(CoreSection, "Toggle Mode (hold vs toggle)", FullSettings.AimBot.Values.Toggle, function(v)
    FullSettings.AimBot.Values.Toggle = v
end)

CreateInfo(CoreSection, "Trigger Key", "RMB (Mouse2)")

CreateDropdown(CoreSection, "Hit Part", FullSettings.AimBot.Values.HitPartList, FullSettings.AimBot.Values.HitPart, function(v)
    FullSettings.AimBot.Values.HitPart = v
end)

local ChecksSection = CreateSection(AimbotPage, "CHECKS")

CreateToggle(ChecksSection, "Team Check", FullSettings.AimBot.Checks.TeamCheck, function(v)
    FullSettings.AimBot.Checks.TeamCheck = v
end)

CreateToggle(ChecksSection, "Wall Check", FullSettings.AimBot.Checks.WallCheck, function(v)
    FullSettings.AimBot.Checks.WallCheck = v
end)

CreateToggle(ChecksSection, "Alive Check", FullSettings.AimBot.Checks.AliveCheck, function(v)
    FullSettings.AimBot.Checks.AliveCheck = v
end)

local FovSection = CreateSection(AimbotPage, "FOV CIRCLE")

CreateToggle(FovSection, "Enable FOV", FullSettings.AimBot.Fov.Enable, function(v)
    FullSettings.AimBot.Fov.Enable = v
end)

CreateToggle(FovSection, "Visible FOV", FullSettings.AimBot.Fov.Visible, function(v)
    FullSettings.AimBot.Fov.Visible = v
end)

CreateToggle(FovSection, "Filled FOV", FullSettings.AimBot.Fov.Filled, function(v)
    FullSettings.AimBot.Fov.Filled = v
end)

CreateSlider(FovSection, "FOV Size", 10, 500, FullSettings.AimBot.Fov.Size, function(v)
    FullSettings.AimBot.Fov.Size = v
end)

CreateSlider(FovSection, "FOV Thickness", 1, 10, math.floor(FullSettings.AimBot.Fov.Thickness * 10), function(v)
    FullSettings.AimBot.Fov.Thickness = v / 10
end)

CreateColorDisplay(FovSection, "FOV Color", FullSettings.AimBot.Fov.Color)
CreateColorDisplay(FovSection, "Lock Color", FullSettings.AimBot.Fov.LockColor)
CreateColorDisplay(FovSection, "Off Color", FullSettings.AimBot.Fov.OffColor)

-- ══════════════════════════════════════════
--              ESP TAB
-- ══════════════════════════════════════════

local EspPage = EspTab.Page

local EspCoreSection = CreateSection(EspPage, "CORE")

CreateToggle(EspCoreSection, "Enable ESP", FullSettings.Esp.Values.Enabled, function(v)
    FullSettings.Esp.Values.Enabled = v
end)

local EspChecksSection = CreateSection(EspPage, "CHECKS")

CreateToggle(EspChecksSection, "Team Check", FullSettings.Esp.Checks.TeamCheck, function(v)
    FullSettings.Esp.Checks.TeamCheck = v
end)

CreateToggle(EspChecksSection, "Wall Check (Occluded)", FullSettings.Esp.Checks.WallCheck, function(v)
    FullSettings.Esp.Checks.WallCheck = v
end)

CreateToggle(EspChecksSection, "Alive Check", FullSettings.Esp.Checks.AliveCheck, function(v)
    FullSettings.Esp.Checks.AliveCheck = v
end)

local EspVisSection = CreateSection(EspPage, "VISUALS")

CreateColorDisplay(EspVisSection, "Fill Color", FullSettings.Esp.Values.FillColor)

CreateSlider(EspVisSection, "Fill Transparency", 0, 10, math.floor(FullSettings.Esp.Values.FillTransparency * 10), function(v)
    FullSettings.Esp.Values.FillTransparency = v / 10
end)

CreateColorDisplay(EspVisSection, "Outline Color", FullSettings.Esp.Values.OutlineColor)

CreateSlider(EspVisSection, "Outline Transparency", 0, 10, math.floor(FullSettings.Esp.Values.OutlineTransparency * 10), function(v)
    FullSettings.Esp.Values.OutlineTransparency = v / 10
end)

-- ══════════════════════════════════════════
--              MOVEMENT TAB (FLY)
-- ══════════════════════════════════════════

local MovementPage = MovementTab.Page

local FlySection = CreateSection(MovementPage, "FLY")

CreateInfo(FlySection, "Fly Toggle Key", "G")

local flyToggle = CreateToggle(FlySection, "Enable Fly (G key)", FullSettings.Movement.Fly.Enabled, function(v)
    FullSettings.Movement.Fly.Enabled = v
    if not v then
        if flyEnabled then
            stopFly()
        end
    end
end)

local speedSlider = CreateSlider(FlySection, "Fly Speed", 10, 200, FullSettings.Movement.Fly.Speed, function(v)
    FullSettings.Movement.Fly.Speed = v
    if flyBodyVelocity then
        flyBodyVelocity.MaxForce = Vector3.new(1, 1, 1) * 1e6
        flyBodyVelocity.Velocity = Vector3.new(0, 0, 0)
    end
end)

-- Fly logic variables
local flyEnabled = false
local flyBodyVelocity = nil
local flyBodyGyro = nil
local flyConnection = nil

-- Function to start flying
local function startFly()
    local char = lp.Character
    if not char then return end
    
    local humanoid = char:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Climbing, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Flying, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Freefall, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.GettingUp, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Jumping, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Landed, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Physics, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.PlatformStanding, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Running, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.StrafingNoPhysics, false)
    humanoid:SetStateEnabled(Enum.HumanoidStateType.Swimming, false)
    
    humanoid:ChangeState(Enum.HumanoidStateType.Physics)
    humanoid.PlatformStand = true
    
    flyBodyVelocity = Instance.new("BodyVelocity")
    flyBodyVelocity.MaxForce = Vector3.new(1, 1, 1) * 1e6
    flyBodyVelocity.Velocity = Vector3.new(0, 0, 0)
    flyBodyVelocity.Parent = char
    
    flyBodyGyro = Instance.new("BodyGyro")
    flyBodyGyro.MaxTorque = Vector3.new(1e6, 1e6, 1e6)
    flyBodyGyro.CFrame = char.HumanoidRootPart.CFrame
    flyBodyGyro.Parent = char
    
    flyEnabled = true
    
    if flyConnection then flyConnection:Disconnect() end
    flyConnection = runS.RenderStepped:Connect(function()
        if not flyEnabled or not FullSettings.Movement.Fly.Enabled then
            return
        end
        
        local char = lp.Character
        if not char or not char:FindFirstChild("HumanoidRootPart") then return end
        
        local hrp = char.HumanoidRootPart
        local humanoid = char:FindFirstChild("Humanoid")
        if not humanoid then return end
        
        local speed = FullSettings.Movement.Fly.Speed
        local moveDirection = Vector3.new(0, 0, 0)
        
        if UIS:IsKeyDown(Enum.KeyCode.W) then
            moveDirection = moveDirection + camera.CFrame.LookVector
        end
        if UIS:IsKeyDown(Enum.KeyCode.S) then
            moveDirection = moveDirection - camera.CFrame.LookVector
        end
        if UIS:IsKeyDown(Enum.KeyCode.A) then
            moveDirection = moveDirection - camera.CFrame.RightVector
        end
        if UIS:IsKeyDown(Enum.KeyCode.D) then
            moveDirection = moveDirection + camera.CFrame.RightVector
        end
        if UIS:IsKeyDown(Enum.KeyCode.Space) then
            moveDirection = moveDirection + Vector3.new(0, 1, 0)
        end
        if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then
            moveDirection = moveDirection - Vector3.new(0, 1, 0)
        end
        
        if moveDirection.Magnitude > 0 then
            moveDirection = moveDirection.Unit
        end
        
        if flyBodyVelocity then
            flyBodyVelocity.Velocity = moveDirection * speed
        end
        
        if flyBodyGyro then
            flyBodyGyro.CFrame = CFrame.new(hrp.Position, hrp.Position + camera.CFrame.LookVector)
        end
    end)
end

-- Function to stop flying
local function stopFly()
    flyEnabled = false
    if flyConnection then
        flyConnection:Disconnect()
        flyConnection = nil
    end
    local char = lp.Character
    if char then
        local humanoid = char:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.PlatformStand = false
            for _, state in pairs(Enum.HumanoidStateType:GetEnumItems()) do
                humanoid:SetStateEnabled(state, true)
            end
            humanoid:ChangeState(Enum.HumanoidStateType.Landed)
        end
        if flyBodyVelocity then
            flyBodyVelocity:Destroy()
            flyBodyVelocity = nil
        end
        if flyBodyGyro then
            flyBodyGyro:Destroy()
            flyBodyGyro = nil
        end
    end
end

-- Toggle fly with G key
UIS.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.G then
        if FullSettings.Movement.Fly.Enabled then
            if flyEnabled then
                stopFly()
                Notify("Movement", "Fly disabled (G key)", 1)
                if flyToggle then flyToggle.SetValue(false) end
                FullSettings.Movement.Fly.Enabled = false
            else
                startFly()
                Notify("Movement", "Fly enabled! Use WASD + Space/Ctrl", 2)
                if flyToggle then flyToggle.SetValue(true) end
                FullSettings.Movement.Fly.Enabled = true
            end
        end
    end
end)

-- Monitor character changes
lp.CharacterAdded:Connect(function(newChar)
    char = newChar
    if FullSettings.Movement.Fly.Enabled and flyEnabled then
        task.wait(0.5)
        startFly()
    end
end)

-- Monitor GUI toggle changes
local lastFlyState = FullSettings.Movement.Fly.Enabled
local flyMonitor = runS.RenderStepped:Connect(function()
    if FullSettings.Movement.Fly.Enabled ~= lastFlyState then
        lastFlyState = FullSettings.Movement.Fly.Enabled
        if lastFlyState and not flyEnabled then
            startFly()
        elseif not lastFlyState and flyEnabled then
            stopFly()
        end
    end
end)

-- ══════════════════════════════════════════
--              SETTINGS TAB
-- ══════════════════════════════════════════

local SettingsPage = SettingsTab.Page

local AboutSection = CreateSection(SettingsPage, "ABOUT")

CreateInfo(AboutSection, "Script", "QM Cheat Suite")
CreateInfo(AboutSection, "Version", "1.0.0")
CreateInfo(AboutSection, "Author", "QM")
CreateInfo(AboutSection, "Engine", "Rayfield-Inspired")

local CtrlSection = CreateSection(SettingsPage, "CONTROLS")

local DestroyBtn = Instance.new("TextButton")
DestroyBtn.Size = UDim2.new(1, 0, 0, 40)
DestroyBtn.BackgroundColor3 = Color3.fromRGB(180, 50, 50)
DestroyBtn.Text = "Destroy GUI"
DestroyBtn.Font = Enum.Font.GothamBold
DestroyBtn.TextSize = 13
DestroyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
DestroyBtn.BorderSizePixel = 0
DestroyBtn.Parent = CtrlSection
MakeRound(DestroyBtn, 8)

DestroyBtn.MouseButton1Click:Connect(function()
    if flyConnection then flyConnection:Disconnect() end
    if flyMonitor then flyMonitor:Disconnect() end
    stopFly()
    Tween(MainFrame, {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}, 0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In)
    task.wait(0.35)
    ScreenGui:Destroy()
end)

-- ══════════════════════════════════════════
--         ACTIVATE FIRST TAB
-- ══════════════════════════════════════════

do
    ActiveTab = AimbotTab
    AimbotTab.Page.Visible = true
    AimbotTab.Button.BackgroundTransparency = 0.7
    AimbotTab.Label.TextColor3 = Theme.Accent
    AimbotTab.Icon.TextColor3 = Theme.Accent
    AimbotTab.Bar.BackgroundTransparency = 0
end

-- ══════════════════════════════════════════
--              NOTIFICATION SYSTEM
-- ══════════════════════════════════════════

local NotifFrame = Instance.new("Frame")
NotifFrame.Size = UDim2.new(0, 280, 0, 0)
NotifFrame.Position = UDim2.new(1, -296, 1, -16)
NotifFrame.AnchorPoint = Vector2.new(0, 1)
NotifFrame.BackgroundTransparency = 1
NotifFrame.Parent = ScreenGui

local NotifList = Instance.new("UIListLayout")
NotifList.SortOrder = Enum.SortOrder.LayoutOrder
NotifList.VerticalAlignment = Enum.VerticalAlignment.Bottom
NotifList.Padding = UDim.new(0, 8)
NotifList.Parent = NotifFrame

NotifFrame.Size = UDim2.new(0, 280, 1, -16)

local notifCount = 0
local function Notify(title, message, duration)
    notifCount = notifCount + 1
    local n = notifCount

    local Card = Instance.new("Frame")
    Card.Size = UDim2.new(1, 0, 0, 0)
    Card.BackgroundColor3 = Theme.Card
    Card.BorderSizePixel = 0
    Card.ClipsDescendants = true
    Card.LayoutOrder = n
    Card.Parent = NotifFrame
    MakeRound(Card, 10)
    MakeStroke(Card, Theme.Accent, 1)

    local AccentBar = Instance.new("Frame")
    AccentBar.Size = UDim2.new(0, 3, 1, 0)
    AccentBar.BackgroundColor3 = Theme.Accent
    AccentBar.BorderSizePixel = 0
    AccentBar.Parent = Card
    MakeRound(AccentBar, 2)

    local NTitle = Instance.new("TextLabel")
    NTitle.Text = title
    NTitle.Font = Enum.Font.GothamBold
    NTitle.TextSize = 13
    NTitle.TextColor3 = Theme.Text
    NTitle.BackgroundTransparency = 1
    NTitle.Position = UDim2.new(0, 16, 0, 8)
    NTitle.Size = UDim2.new(1, -20, 0, 16)
    NTitle.TextXAlignment = Enum.TextXAlignment.Left
    NTitle.Parent = Card

    local NMsg = Instance.new("TextLabel")
    NMsg.Text = message
    NMsg.Font = Enum.Font.Gotham
    NMsg.TextSize = 12
    NMsg.TextColor3 = Theme.TextDim
    NMsg.BackgroundTransparency = 1
    NMsg.Position = UDim2.new(0, 16, 0, 26)
    NMsg.Size = UDim2.new(1, -20, 0, 30)
    NMsg.TextXAlignment = Enum.TextXAlignment.Left
    NMsg.TextWrapped = true
    NMsg.Parent = Card

    Tween(Card, {Size = UDim2.new(1, 0, 0, 62)}, 0.3, Enum.EasingStyle.Back)
    task.delay(duration or 3, function()
        Tween(Card, {Size = UDim2.new(1, 0, 0, 0), BackgroundTransparency = 1}, 0.3)
        task.wait(0.35)
        Card:Destroy()
    end)
end

-- ══════════════════════════════════════════
--           F4 TOGGLE GUI
-- ══════════════════════════════════════════

local guiVisible = true
UIS.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.F4 then
        guiVisible = not guiVisible
        MainFrame.Visible = guiVisible
        if guiVisible then
            Notify("QM Cheat Suite", "GUI Opened (F4)", 2)
        else
            Notify("QM Cheat Suite", "GUI Hidden (F4)", 2)
        end
    end
end)

-- ══════════════════════════════════════════
--           AIMBOT LOGIC
-- ══════════════════════════════════════════

do
    local FOV = Drawing.new("Circle")
    FOV.Visible = FullSettings.AimBot.Fov.Visible
    FOV.Thickness = FullSettings.AimBot.Fov.Thickness
    FOV.Color = FullSettings.AimBot.Fov.Color
    FOV.Filled = FullSettings.AimBot.Fov.Filled
    FOV.Radius = FullSettings.AimBot.Fov.Size
    FOV.Position = mousePPos

    runS.RenderStepped:Connect(function()
        FOV.Position = mousePPos
        FOV.Visible = FullSettings.AimBot.Fov.Visible and FullSettings.AimBot.Fov.Enable
        FOV.Radius = FullSettings.AimBot.Fov.Size
        FOV.Thickness = FullSettings.AimBot.Fov.Thickness
        FOV.Filled = FullSettings.AimBot.Fov.Filled
    end)

    coroutine.wrap(function()
        local lock = false

        local function GetPartToFov(Part)
            for _, v in ipairs(pl:GetPlayers()) do
                if v ~= lp and v.Character and v.Character:FindFirstChild(Part) then
                    if FullSettings.AimBot.Checks.AliveCheck and v.Character:FindFirstChildOfClass("Humanoid") and v.Character.Humanoid.Health <= 0 then continue end

                    local ray = workspace:FindPartOnRayWithIgnoreList(
                        Ray.new(camera.CFrame.Position,
                        (v.Character[Part].Position - camera.CFrame.Position).Unit *
                        (v.Character[Part].Position - camera.CFrame.Position).Magnitude),
                        {lp.Character, camera}
                    )

                    if FullSettings.AimBot.Checks.WallCheck and (not ray or not ray:IsDescendantOf(v.Character)) then continue end
                    if FullSettings.AimBot.Checks.TeamCheck and v.Character:FindFirstChild("HumanoidRootPart") and v.Character.HumanoidRootPart:FindFirstChild("TeammateLabel") then continue end

                    local vPos = camera:WorldToViewportPoint(v.Character[Part].Position)
                    local distance = (Vector2.new(vPos.X, vPos.Y) - mousePPos).Magnitude

                    if FullSettings.AimBot.Fov.Enable and (distance > FullSettings.AimBot.Fov.Size) then continue end
                    return v
                end
            end
        end

        UIS.InputBegan:Connect(function(input, gameProcessedEvent)
            if gameProcessedEvent then return end
            if FullSettings.AimBot.Values.Toggle == false or FullSettings.AimBot.Values.TriggerKey == nil then
                lock = true
            else
                if input.UserInputType == FullSettings.AimBot.Values.TriggerKey then
                    lock = not lock
                end
            end
        end)

        while task.wait() do
            if FullSettings.AimBot.Values.Enable then
                local ok, _ = pcall(function()
                    return lp.PlayerGui.MainGui.MainFrame.Lobby.Currency.Visible
                end)
                local inLobby = false
                if ok then
                    pcall(function()
                        inLobby = lp.PlayerGui.MainGui.MainFrame.Lobby.Currency.Visible
                    end)
                end

                if not inLobby then
                    local Target = GetPartToFov(FullSettings.AimBot.Values.HitPart)

                    if Target ~= nil then
                        FOV.Color = FullSettings.AimBot.Fov.LockColor
                    else
                        FOV.Color = FullSettings.AimBot.Fov.Color
                    end

                    if (FullSettings.AimBot.Values.Toggle == true and lock == false) or
                       (FullSettings.AimBot.Values.Toggle == false and not UIS:IsMouseButtonPressed(FullSettings.AimBot.Values.TriggerKey)) then
                        FOV.Color = FullSettings.AimBot.Fov.OffColor
                    end

                    if Target and Target.Character and Target.Character:FindFirstChild(FullSettings.AimBot.Values.HitPart) and lock and
                       camera:WorldToViewportPoint(Target.Character[FullSettings.AimBot.Values.HitPart].Position).Z > 0 then
                        if not FullSettings.AimBot.Values.Toggle and FullSettings.AimBot.Values.TriggerKey and
                           not UIS:IsMouseButtonPressed(FullSettings.AimBot.Values.TriggerKey) then continue end
                        camera.CFrame = CFrame.new(
                            camera.CFrame.Position + (Target.Character[FullSettings.AimBot.Values.HitPart].Position - camera.CFrame.Position).Unit * 0.5,
                            Target.Character[FullSettings.AimBot.Values.HitPart].Position
                        )
                        VirtualInputManager:SendMouseButtonEvent(Center.X, Center.Y, 0, true, game, 0)
                        task.wait()
                        VirtualInputManager:SendMouseButtonEvent(Center.X, Center.Y, 0, false, game, 0)
                    end
                end
            end
        end
    end)()
end

-- ══════════════════════════════════════════
--              ESP LOGIC
-- ══════════════════════════════════════════

do
    coroutine.wrap(function()
        while task.wait() do
            for _, v in pairs(pl:GetPlayers()) do
                if v ~= lp and v.Character then
                    local Esp = v.Character:FindFirstChild("Esp")

                    if FullSettings.Esp.Checks.AliveCheck and v.Character:FindFirstChildOfClass("Humanoid") and v.Character.Humanoid.Health <= 0 then
                        if Esp then Esp:Destroy() end continue
                    end
                    if FullSettings.Esp.Checks.TeamCheck and v.Character:FindFirstChild("HumanoidRootPart") and v.Character.HumanoidRootPart:FindFirstChild("TeammateLabel") then
                        if Esp then Esp:Destroy() end continue
                    end
                    if not Esp then
                        Esp = Instance.new("Highlight")
                        Esp.RobloxLocked = true
                        Esp.Name = "Esp"
                        Esp.Adornee = v.Character
                        Esp.Parent = v.Character
                    end

                    if Esp then
                        Esp.DepthMode = FullSettings.Esp.Checks.WallCheck and Enum.HighlightDepthMode.Occluded or Enum.HighlightDepthMode.AlwaysOnTop
                        Esp.Enabled = FullSettings.Esp.Values.Enabled
                        Esp.FillColor = FullSettings.Esp.Values.FillColor
                        Esp.FillTransparency = FullSettings.Esp.Values.FillTransparency
                        Esp.OutlineColor = FullSettings.Esp.Values.OutlineColor
                        Esp.OutlineTransparency = FullSettings.Esp.Values.OutlineTransparency
                    end
                end
            end
        end
    end)()
end

-- ══════════════════════════════════════════
--              STARTUP NOTIFICATION
-- ══════════════════════════════════════════

task.wait(0.6)
Notify("QM Cheat Suite", "Loaded successfully! Press F4 to hide/show GUI | Press G to fly", 4)
