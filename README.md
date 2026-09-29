--[[
    WIND CLOUD HUB
    Full-Screen Sky Loading + Animated Hub
]]

--==================================================
-- SERVICES
--==================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local TeleportService = game:GetService("TeleportService")

local LocalPlayer = Players.LocalPlayer

--==================================================
-- CONFIG
--==================================================

local Config = {
    Name = "WIND CLOUD HUB",
    Version = "2.0",

    SkyTop = Color3.fromRGB(55, 155, 240),
    SkyMiddle = Color3.fromRGB(105, 200, 250),
    SkyBottom = Color3.fromRGB(225, 248, 255),

    Accent = Color3.fromRGB(55, 155, 235),
    AccentDark = Color3.fromRGB(35, 125, 205),

    White = Color3.fromRGB(255, 255, 255),
    Cloud = Color3.fromRGB(242, 250, 255),

    Panel = Color3.fromRGB(232, 247, 255),
    PanelDark = Color3.fromRGB(190, 225, 245),

    Text = Color3.fromRGB(35, 75, 100),
    SubText = Color3.fromRGB(90, 130, 150),

    Danger = Color3.fromRGB(235, 80, 90)
}

--==================================================
-- REMOVE OLD HUB
--==================================================

pcall(function()

    local oldCore = game.CoreGui:FindFirstChild("WindCloudHub")

    if oldCore then
        oldCore:Destroy()
    end

end)

pcall(function()

    local oldPlayerGui =
        LocalPlayer.PlayerGui:FindFirstChild("WindCloudHub")

    if oldPlayerGui then
        oldPlayerGui:Destroy()
    end

end)

--==================================================
-- SCREEN GUI
--==================================================

local ScreenGui = Instance.new("ScreenGui")

ScreenGui.Name = "WindCloudHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.DisplayOrder = 999

pcall(function()
    ScreenGui.ScreenInsets = Enum.ScreenInsets.None
end)

pcall(function()
    ScreenGui.Parent = game.CoreGui
end)

if not ScreenGui.Parent then
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
end

--==================================================
-- HELPERS
--==================================================

local function Corner(parent, radius)

    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius)
    corner.Parent = parent

    return corner
end

local function Stroke(
    parent,
    color,
    thickness,
    transparency
)

    local stroke = Instance.new("UIStroke")

    stroke.Color = color
    stroke.Thickness = thickness or 1
    stroke.Transparency = transparency or 0

    stroke.Parent = parent

    return stroke
end

local function Tween(
    object,
    time,
    properties,
    style,
    direction
)

    local tween = TweenService:Create(
        object,

        TweenInfo.new(
            time,
            style or Enum.EasingStyle.Quad,
            direction or Enum.EasingDirection.Out
        ),

        properties
    )

    tween:Play()

    return tween
end

--==================================================
-- FULL SCREEN SKY CREATOR
--==================================================

local function CreateFullScreenSky(
    name,
    parent,
    zIndex
)

    local sky = Instance.new("Frame")

    sky.Name = name

    sky.AnchorPoint = Vector2.new(0, 0)

    sky.Position =
        UDim2.new(0, 0, 0, 0)

    sky.Size =
        UDim2.new(1, 0, 1, 0)

    sky.BackgroundColor3 =
        Config.SkyMiddle

    sky.BorderSizePixel = 0

    sky.ClipsDescendants = false

    sky.ZIndex = zIndex

    sky.Parent = parent

    local gradient =
        Instance.new("UIGradient")

    gradient.Rotation = 90

    gradient.Color =
        ColorSequence.new({

            ColorSequenceKeypoint.new(
                0,
                Config.SkyTop
            ),

            ColorSequenceKeypoint.new(
                0.5,
                Config.SkyMiddle
            ),

            ColorSequenceKeypoint.new(
                1,
                Config.SkyBottom
            )

        })

    gradient.Parent = sky

    return sky
end

--==================================================
-- HUB SKY LAYER
--==================================================

local SkyLayer =
    Instance.new("CanvasGroup")

SkyLayer.Name =
    "HubSkyLayer"

SkyLayer.AnchorPoint =
    Vector2.new(0, 0)

SkyLayer.Position =
    UDim2.new(0, 0, 0, 0)

SkyLayer.Size =
    UDim2.new(1, 0, 1, 0)

SkyLayer.BackgroundTransparency = 1

SkyLayer.GroupTransparency = 1

SkyLayer.ClipsDescendants = false

SkyLayer.ZIndex = 0

SkyLayer.Parent = ScreenGui

local SkyBackground =
    CreateFullScreenSky(
        "SkyBackground",
        SkyLayer,
        0
    )

--==================================================
-- HUB SUN
--==================================================

local Sun = Instance.new("Frame")

Sun.Name = "Sun"

Sun.Size =
    UDim2.new(0, 125, 0, 125)

Sun.Position =
    UDim2.new(0.82, 0, 0.06, 0)

Sun.BackgroundColor3 =
    Color3.fromRGB(
        255,
        239,
        150
    )

Sun.BackgroundTransparency = 0.18

Sun.BorderSizePixel = 0

Sun.ZIndex = 1

Sun.Parent = SkyLayer

Corner(Sun, 100)

--==================================================
-- CLOUD CREATOR
--==================================================

local function CreateCloud(
    parent,
    position,
    size,
    transparency,
    zIndex
)

    local cloud =
        Instance.new("Frame")

    cloud.Size = size

    cloud.Position =
        position

    cloud.BackgroundTransparency = 1

    cloud.ZIndex = zIndex

    cloud.Parent = parent

    local pieces = {

        {
            X = 0.02,
            Y = 0.35,
            W = 0.42,
            H = 0.55
        },

        {
            X = 0.27,
            Y = 0.03,
            W = 0.45,
            H = 0.85
        },

        {
            X = 0.57,
            Y = 0.3,
            W = 0.43,
            H = 0.62
        }

    }

    for _, piece in ipairs(pieces) do

        local bubble =
            Instance.new("Frame")

        bubble.Size =
            UDim2.new(
                piece.W,
                0,
                piece.H,
                0
            )

        bubble.Position =
            UDim2.new(
                piece.X,
                0,
                piece.Y,
                0
            )

        bubble.BackgroundColor3 =
            Config.White

        bubble.BackgroundTransparency =
            transparency

        bubble.BorderSizePixel = 0

        bubble.ZIndex = zIndex

        bubble.Parent = cloud

        Corner(bubble, 100)
    end

    return cloud
end

--==================================================
-- HUB CLOUDS
--==================================================

local HubClouds = {}

table.insert(
    HubClouds,

    CreateCloud(
        SkyLayer,
        UDim2.new(
            -0.08,
            0,
            0.12,
            0
        ),

        UDim2.new(
            0,
            300,
            0,
            110
        ),

        0.25,

        1
    )
)

table.insert(
    HubClouds,

    CreateCloud(
        SkyLayer,
        UDim2.new(
            0.75,
            0,
            0.18,
            0
        ),

        UDim2.new(
            0,
            285,
            0,
            105
        ),

        0.28,

        1
    )
)

table.insert(
    HubClouds,

    CreateCloud(
        SkyLayer,
        UDim2.new(
            0.08,
            0,
            0.77,
            0
        ),

        UDim2.new(
            0,
            330,
            0,
            120
        ),

        0.32,

        1
    )
)

table.insert(
    HubClouds,

    CreateCloud(
        SkyLayer,
        UDim2.new(
            0.78,
            0,
            0.72,
            0
        ),

        UDim2.new(
            0,
            270,
            0,
            105
        ),

        0.3,

        1
    )
)

--==================================================
-- CLOUD ANIMATION
--==================================================

for index, cloud in ipairs(HubClouds) do

    task.spawn(function()

        local originalPosition =
            cloud.Position

        while cloud.Parent do

            local direction =
                index % 2 == 0
                and 22
                or -22

            local firstTween =
                Tween(
                    cloud,
                    5 + index,

                    {
                        Position =
                            UDim2.new(
                                originalPosition.X.Scale,
                                originalPosition.X.Offset
                                    + direction,

                                originalPosition.Y.Scale,
                                originalPosition.Y.Offset
                            )
                    },

                    Enum.EasingStyle.Sine,
                    Enum.EasingDirection.InOut
                )

            firstTween.Completed:Wait()

            if not cloud.Parent then
                break
            end

            local secondTween =
                Tween(
                    cloud,
                    5 + index,

                    {
                        Position =
                            originalPosition
                    },

                    Enum.EasingStyle.Sine,
                    Enum.EasingDirection.InOut
                )

            secondTween.Completed:Wait()
        end

    end)
end

--==================================================
-- LOADING SCREEN
--==================================================

local LoadingScreen =
    Instance.new("CanvasGroup")

LoadingScreen.Name =
    "LoadingScreen"

LoadingScreen.AnchorPoint =
    Vector2.new(0, 0)

LoadingScreen.Position =
    UDim2.new(0, 0, 0, 0)

LoadingScreen.Size =
    UDim2.new(1, 0, 1, 0)

LoadingScreen.BackgroundTransparency = 1

LoadingScreen.GroupTransparency = 0

LoadingScreen.ClipsDescendants = false

LoadingScreen.ZIndex = 500

LoadingScreen.Parent = ScreenGui

--==================================================
-- LOADING SKY
--==================================================

local LoadingSky =
    CreateFullScreenSky(
        "LoadingSky",
        LoadingScreen,
        500
    )

--==================================================
-- LOADING SUN
--==================================================

local LoadingSun =
    Instance.new("Frame")

LoadingSun.Size =
    UDim2.new(0, 135, 0, 135)

LoadingSun.Position =
    UDim2.new(0.82, 0, 0.06, 0)

LoadingSun.BackgroundColor3 =
    Color3.fromRGB(
        255,
        239,
        150
    )

LoadingSun.BackgroundTransparency = 0.18

LoadingSun.BorderSizePixel = 0

LoadingSun.ZIndex = 501

LoadingSun.Parent = LoadingScreen

Corner(LoadingSun, 100)

--==================================================
-- LOADING CLOUDS
--==================================================

CreateCloud(
    LoadingScreen,

    UDim2.new(
        -0.08,
        0,
        0.12,
        0
    ),

    UDim2.new(
        0,
        310,
        0,
        115
    ),

    0.2,

    502
)

CreateCloud(
    LoadingScreen,

    UDim2.new(
        0.74,
        0,
        0.22,
        0
    ),

    UDim2.new(
        0,
        300,
        0,
        110
    ),

    0.24,

    502
)

CreateCloud(
    LoadingScreen,

    UDim2.new(
        0.02,
        0,
        0.78,
        0
    ),

    UDim2.new(
        0,
        340,
        0,
        120
    ),

    0.3,

    502
)

--==================================================
-- LOADING LOGO
--==================================================

local LoadingLogo =
    Instance.new("Frame")

LoadingLogo.Size =
    UDim2.new(0, 100, 0, 100)

LoadingLogo.Position =
    UDim2.new(
        0.5,
        -50,
        0.35,
        -50
    )

LoadingLogo.BackgroundColor3 =
    Config.AccentDark

LoadingLogo.BorderSizePixel = 0

LoadingLogo.ZIndex = 510

LoadingLogo.Parent = LoadingScreen

Corner(LoadingLogo, 25)

Stroke(
    LoadingLogo,
    Config.White,
    3,
    0.1
)

local LoadingLogoText =
    Instance.new("TextLabel")

LoadingLogoText.Size =
    UDim2.fromScale(1, 1)

LoadingLogoText.BackgroundTransparency = 1

LoadingLogoText.Text = "W"

LoadingLogoText.TextColor3 =
    Config.White

LoadingLogoText.Font =
    Enum.Font.GothamBlack

LoadingLogoText.TextSize = 52

LoadingLogoText.ZIndex = 511

LoadingLogoText.Parent =
    LoadingLogo

--==================================================
-- LOADING TITLE
--==================================================

local LoadingTitle =
    Instance.new("TextLabel")

LoadingTitle.Size =
    UDim2.new(0.8, 0, 0, 50)

LoadingTitle.Position =
    UDim2.new(
        0.1,
        0,
        0.53,
        0
    )

LoadingTitle.BackgroundTransparency = 1

LoadingTitle.Text =
    "WIND CLOUD HUB"

LoadingTitle.TextColor3 =
    Config.White

LoadingTitle.Font =
    Enum.Font.GothamBlack

LoadingTitle.TextSize = 27

LoadingTitle.ZIndex = 510

LoadingTitle.Parent =
    LoadingScreen

--==================================================
-- LOADING SUBTITLE
--==================================================

local LoadingSubtitle =
    Instance.new("TextLabel")

LoadingSubtitle.Size =
    UDim2.new(
        0.8,
        0,
        0,
        25
    )

LoadingSubtitle.Position =
    UDim2.new(
        0.1,
        0,
        0.61,
        0
    )

LoadingSubtitle.BackgroundTransparency = 1

LoadingSubtitle.Text =
    "Preparing the clouds..."

LoadingSubtitle.TextColor3 =
    Config.White

LoadingSubtitle.Font =
    Enum.Font.Gotham

LoadingSubtitle.TextSize = 13

LoadingSubtitle.ZIndex = 510

LoadingSubtitle.Parent =
    LoadingScreen

--==================================================
-- LOADING BAR
--==================================================

local LoadingBar =
    Instance.new("Frame")

LoadingBar.Size =
    UDim2.new(
        0,
        280,
        0,
        8
    )

LoadingBar.Position =
    UDim2.new(
        0.5,
        -140,
        0.69,
        0
    )

LoadingBar.BackgroundColor3 =
    Config.White

LoadingBar.BackgroundTransparency = 0.45

LoadingBar.BorderSizePixel = 0

LoadingBar.ZIndex = 510

LoadingBar.Parent =
    LoadingScreen

Corner(LoadingBar, 20)

local LoadingFill =
    Instance.new("Frame")

LoadingFill.Size =
    UDim2.new(
        0,
        0,
        1,
        0
    )

LoadingFill.BackgroundColor3 =
    Config.White

LoadingFill.BorderSizePixel = 0

LoadingFill.ZIndex = 511

LoadingFill.Parent =
    LoadingBar

Corner(LoadingFill, 20)

local LoadingPercent =
    Instance.new("TextLabel")

LoadingPercent.Size =
    UDim2.new(
        0,
        100,
        0,
        25
    )

LoadingPercent.Position =
    UDim2.new(
        0.5,
        -50,
        0.73,
        0
    )

LoadingPercent.BackgroundTransparency = 1

LoadingPercent.Text =
    "0%"

LoadingPercent.TextColor3 =
    Config.White

LoadingPercent.Font =
    Enum.Font.GothamBold

LoadingPercent.TextSize = 11

LoadingPercent.ZIndex = 510

LoadingPercent.Parent =
    LoadingScreen

--==================================================
-- MAIN HUB
--==================================================

local Main =
    Instance.new("Frame")

Main.Name =
    "MainWindow"

Main.Size =
    UDim2.new(
        0,
        0,
        0,
        0
    )

Main.AnchorPoint =
    Vector2.new(
        0.5,
        0.5
    )

Main.Position =
    UDim2.new(
        0.5,
        0,
        0.5,
        0
    )

Main.BackgroundColor3 =
    Config.Panel

Main.BackgroundTransparency =
    0.08

Main.BorderSizePixel = 0

Main.ClipsDescendants = true

Main.ZIndex = 10

Main.Parent = ScreenGui

Corner(Main, 16)

Stroke(
    Main,
    Config.White,
    2,
    0.05
)

--==================================================
-- HEADER
--==================================================

local Header =
    Instance.new("Frame")

Header.Size =
    UDim2.new(
        1,
        0,
        0,
        52
    )

Header.BackgroundColor3 =
    Config.White

Header.BackgroundTransparency =
    0.18

Header.BorderSizePixel = 0

Header.ZIndex = 11

Header.Parent = Main

Corner(Header, 16)

--==================================================
-- LOGO
--==================================================

local Logo =
    Instance.new("Frame")

Logo.Size =
    UDim2.new(
        0,
        34,
        0,
        34
    )

Logo.Position =
    UDim2.new(
        0,
        10,
        0.5,
        -17
    )

Logo.BackgroundColor3 =
    Config.AccentDark

Logo.ZIndex = 12

Logo.Parent = Header

Corner(Logo, 10)

local LogoText =
    Instance.new("TextLabel")

LogoText.Size =
    UDim2.fromScale(1, 1)

LogoText.BackgroundTransparency = 1

LogoText.Text = "W"

LogoText.TextColor3 =
    Config.White

LogoText.Font =
    Enum.Font.GothamBlack

LogoText.TextSize = 19

LogoText.ZIndex = 13

LogoText.Parent = Logo

--==================================================
-- TITLE
--==================================================

local Title =
    Instance.new("TextLabel")

Title.Size =
    UDim2.new(
        0,
        240,
        0,
        25
    )

Title.Position =
    UDim2.new(
        0,
        54,
        0,
        7
    )

Title.BackgroundTransparency = 1

Title.Text =
    "WIND CLOUD HUB"

Title.TextColor3 =
    Config.Text

Title.Font =
    Enum.Font.GothamBlack

Title.TextSize = 15

Title.TextXAlignment =
    Enum.TextXAlignment.Left

Title.ZIndex = 12

Title.Parent = Header

local Subtitle =
    Instance.new("TextLabel")

Subtitle.Size =
    UDim2.new(
        0,
        240,
        0,
        18
    )

Subtitle.Position =
    UDim2.new(
        0,
        54,
        0,
        28
    )

Subtitle.BackgroundTransparency = 1

Subtitle.Text =
    "Ride the wind."

Subtitle.TextColor3 =
    Config.SubText

Subtitle.Font =
    Enum.Font.Gotham

Subtitle.TextSize = 10

Subtitle.TextXAlignment =
    Enum.TextXAlignment.Left

Subtitle.ZIndex = 12

Subtitle.Parent = Header

--==================================================
-- MINIMIZE
--==================================================

local Minimize =
    Instance.new("TextButton")

Minimize.Size =
    UDim2.new(
        0,
        31,
        0,
        31
    )

Minimize.Position =
    UDim2.new(
        1,
        -72,
        0.5,
        -15
    )

Minimize.BackgroundColor3 =
    Config.PanelDark

Minimize.Text = "-"

Minimize.TextColor3 =
    Config.Text

Minimize.Font =
    Enum.Font.GothamBold

Minimize.TextSize = 17

Minimize.AutoButtonColor = false

Minimize.ZIndex = 12

Minimize.Parent = Header

Corner(Minimize, 9)

--==================================================
-- CLOSE
--==================================================

local Close =
    Instance.new("TextButton")

Close.Size =
    UDim2.new(
        0,
        31,
        0,
        31
    )

Close.Position =
    UDim2.new(
        1,
        -36,
        0.5,
        -15
    )

Close.BackgroundColor3 =
    Config.PanelDark

Close.Text = "×"

Close.TextColor3 =
    Config.Danger

Close.Font =
    Enum.Font.GothamBold

Close.TextSize = 18

Close.AutoButtonColor = false

Close.ZIndex = 12

Close.Parent = Header

Corner(Close, 9)

--==================================================
-- DRAGGING
--==================================================

local dragging = false
local dragStart
local startPosition

Header.InputBegan:Connect(function(input)

    if input.UserInputType ==
        Enum.UserInputType.MouseButton1

        or input.UserInputType ==
        Enum.UserInputType.Touch then

        dragging = true

        dragStart =
            input.Position

        startPosition =
            Main.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)

    if not dragging then
        return
    end

    if input.UserInputType ==
        Enum.UserInputType.MouseMovement

        or input.UserInputType ==
        Enum.UserInputType.Touch then

        local delta =
            input.Position -
            dragStart

        Main.Position =
            UDim2.new(
                startPosition.X.Scale,
                startPosition.X.Offset +
                    delta.X,

                startPosition.Y.Scale,
                startPosition.Y.Offset +
                    delta.Y
            )
    end
end)

UserInputService.InputEnded:Connect(function(input)

    if input.UserInputType ==
        Enum.UserInputType.MouseButton1

        or input.UserInputType ==
        Enum.UserInputType.Touch then

        dragging = false
    end
end)

--==================================================
-- MINIMIZE / RESTORE
--==================================================

local minimized = false
local closed = false

Minimize.MouseButton1Click:Connect(function()

    if closed then
        return
    end

    minimized = not minimized

    if minimized then

        Tween(
            SkyLayer,
            0.45,
            {
                GroupTransparency = 1
            }
        )

        Tween(
            Main,
            0.3,
            {
                Size =
                    UDim2.new(
                        0,
                        600,
                        0,
                        52
                    )
            }
        )

        Minimize.Text = "+"

    else

        Tween(
            Main,
            0.3,
            {
                Size =
                    UDim2.new(
                        0,
                        600,
                        0,
                        410
                    )
            }
        )

        SkyLayer.Visible = true

        Tween(
            SkyLayer,
            0.65,
            {
                GroupTransparency = 0
            }
        )

        Minimize.Text = "-"
    end
end)

--==================================================
-- CLOSE
--==================================================

Close.MouseButton1Click:Connect(function()

    if closed then
        return
    end

    closed = true

    Tween(
        SkyLayer,
        0.4,
        {
            GroupTransparency = 1
        }
    )

    local closeTween =
        Tween(
            Main,
            0.4,
            {
                Size =
                    UDim2.new(
                        0,
                        0,
                        0,
                        0
                    )
            },

            Enum.EasingStyle.Back,
            Enum.EasingDirection.In
        )

    closeTween.Completed:Connect(function()

        if ScreenGui then
            ScreenGui:Destroy()
        end

    end)
end)

--==================================================
-- RIGHT CONTROL
--==================================================

UserInputService.InputBegan:Connect(function(
    input,
    processed
)

    if processed then
        return
    end

    if input.KeyCode ==
        Enum.KeyCode.RightControl then

        if closed then
            return
        end

        Main.Visible =
            not Main.Visible

        if Main.Visible then

            SkyLayer.Visible = true

            Tween(
                SkyLayer,
                0.5,
                {
                    GroupTransparency = 0
                }
            )

        else

            Tween(
                SkyLayer,
                0.4,
                {
                    GroupTransparency = 1
                }
            )
        end
    end
end)

--==================================================
-- SIDEBAR
--==================================================

local Sidebar =
    Instance.new("Frame")

Sidebar.Size =
    UDim2.new(
        0,
        145,
        1,
        -52
    )

Sidebar.Position =
    UDim2.new(
        0,
        0,
        0,
        52
    )

Sidebar.BackgroundColor3 =
    Config.White

Sidebar.BackgroundTransparency =
    0.35

Sidebar.BorderSizePixel = 0

Sidebar.ZIndex = 11

Sidebar.Parent = Main

local SidebarPadding =
    Instance.new("UIPadding")

SidebarPadding.PaddingTop =
    UDim.new(0, 10)

SidebarPadding.PaddingLeft =
    UDim.new(0, 8)

SidebarPadding.PaddingRight =
    UDim.new(0, 8)

SidebarPadding.Parent =
    Sidebar

local SidebarLayout =
    Instance.new("UIListLayout")

SidebarLayout.Padding =
    UDim.new(0, 7)

SidebarLayout.SortOrder =
    Enum.SortOrder.LayoutOrder

SidebarLayout.Parent =
    Sidebar

--==================================================
-- CONTENT
--==================================================

local Content =
    Instance.new("Frame")

Content.Size =
    UDim2.new(
        1,
        -157,
        1,
        -64
    )

Content.Position =
    UDim2.new(
        0,
        153,
        0,
        58
    )

Content.BackgroundTransparency = 1

Content.ZIndex = 11

Content.Parent = Main

--==================================================
-- TABS
--==================================================

local Tabs = {}
local FirstTab = true

local function CreateTab(name)

    local button =
        Instance.new("TextButton")

    button.Size =
        UDim2.new(
            1,
            0,
            0,
            39
        )

    button.BackgroundColor3 =
        Config.Cloud

    button.BackgroundTransparency =
        0.25

    button.Text = name

    button.TextColor3 =
        Config.SubText

    button.Font =
        Enum.Font.GothamBold

    button.TextSize = 12

    button.TextXAlignment =
        Enum.TextXAlignment.Left

    button.AutoButtonColor = false

    button.ZIndex = 12

    button.Parent = Sidebar

    Corner(button, 9)

    local padding =
        Instance.new("UIPadding")

    padding.PaddingLeft =
        UDim.new(0, 13)

    padding.Parent =
        button

    local page =
        Instance.new("ScrollingFrame")

    page.Size =
        UDim2.fromScale(1, 1)

    page.BackgroundTransparency = 1

    page.BorderSizePixel = 0

    page.ScrollBarThickness = 3

    page.ScrollBarImageColor3 =
        Config.AccentDark

    page.AutomaticCanvasSize =
        Enum.AutomaticSize.Y

    page.CanvasSize =
        UDim2.new(0, 0, 0, 0)

    page.Visible = false

    page.ZIndex = 12

    page.Parent = Content

    local pagePadding =
        Instance.new("UIPadding")

    pagePadding.PaddingRight =
        UDim.new(0, 5)

    pagePadding.PaddingBottom =
        UDim.new(0, 8)

    pagePadding.Parent =
        page

    local layout =
        Instance.new("UIListLayout")

    layout.Padding =
        UDim.new(0, 8)

    layout.SortOrder =
        Enum.SortOrder.LayoutOrder

    layout.Parent =
        page

    Tabs[name] = {
        Button = button,
        Page = page
    }

    button.MouseButton1Click:Connect(function()

        for _, tab in pairs(Tabs) do

            tab.Page.Visible = false

            Tween(
                tab.Button,
                0.15,
                {
                    BackgroundColor3 =
                        Config.Cloud,

                    TextColor3 =
                        Config.SubText
                }
            )
        end

        page.Visible = true

        Tween(
            button,
            0.15,
            {
                BackgroundColor3 =
                    Config.AccentDark,

                TextColor3 =
                    Config.White
            }
        )
    end)

    if FirstTab then

        FirstTab = false

        page.Visible = true

        button.BackgroundColor3 =
            Config.AccentDark

        button.TextColor3 =
            Config.White
    end

    return page
end

--==================================================
-- COMPONENTS
--==================================================

local function AddSection(
    parent,
    text
)

    local label =
        Instance.new("TextLabel")

    label.Size =
        UDim2.new(
            1,
            0,
            0,
            24
        )

    label.BackgroundTransparency = 1

    label.Text =
        string.upper(text)

    label.TextColor3 =
        Config.AccentDark

    label.Font =
        Enum.Font.GothamBlack

    label.TextSize = 11

    label.TextXAlignment =
        Enum.TextXAlignment.Left

    label.Parent =
        parent
end

local function AddButton(
    parent,
    text,
    callback
)

    local button =
        Instance.new("TextButton")

    button.Size =
        UDim2.new(
            1,
            0,
            0,
            39
        )

    button.BackgroundColor3 =
        Config.Cloud

    button.BackgroundTransparency =
        0.1

    button.Text = text

    button.TextColor3 =
        Config.Text

    button.Font =
        Enum.Font.GothamMedium

    button.TextSize = 12

    button.TextXAlignment =
        Enum.TextXAlignment.Left

    button.AutoButtonColor = false

    button.Parent =
        parent

    Corner(button, 9)

    local padding =
        Instance.new("UIPadding")

    padding.PaddingLeft =
        UDim.new(0, 12)

    padding.Parent =
        button

    button.MouseEnter:Connect(function()

        Tween(
            button,
            0.12,
            {
                BackgroundColor3 =
                    Config.PanelDark
            }
        )
    end)

    button.MouseLeave:Connect(function()

        Tween(
            button,
            0.12,
            {
                BackgroundColor3 =
                    Config.Cloud
            }
        )
    end)

    button.MouseButton1Click:Connect(function()

        pcall(callback)

    end)

    return button
end

local function AddToggle(
    parent,
    text,
    defaultState,
    callback
)

    local enabled =
        defaultState or false

    local frame =
        Instance.new("Frame")

    frame.Size =
        UDim2.new(
            1,
            0,
            0,
            40
        )

    frame.BackgroundColor3 =
        Config.Cloud

    frame.BackgroundTransparency =
        0.1

    frame.Parent =
        parent

    Corner(frame, 9)

    local label =
        Instance.new("TextLabel")

    label.Size =
        UDim2.new(
            1,
            -70,
            1,
            0
        )

    label.Position =
        UDim2.new(
            0,
            12,
            0,
            0
        )

    label.BackgroundTransparency = 1

    label.Text = text

    label.TextColor3 =
        Config.Text

    label.Font =
        Enum.Font.GothamMedium

    label.TextSize = 12

    label.TextXAlignment =
        Enum.TextXAlignment.Left

    label.Parent =
        frame

    local switch =
        Instance.new("Frame")

    switch.Size =
        UDim2.new(
            0,
            40,
            0,
            20
        )

    switch.Position =
        UDim2.new(
            1,
            -50,
            0.5,
            -10
        )

    switch.BackgroundColor3 =
        enabled
        and Config.AccentDark
        or Color3.fromRGB(
            170,
            195,
            210
        )

    switch.Parent =
        frame

    Corner(switch, 20)

    local knob =
        Instance.new("Frame")

    knob.Size =
        UDim2.new(
            0,
            16,
            0,
            16
        )

    knob.Position =
        enabled
        and UDim2.new(
            1,
            -18,
            0.5,
            -8
        )
        or UDim2.new(
            0,
            2,
            0.5,
            -8
        )

    knob.BackgroundColor3 =
        Config.White

    knob.Parent =
        switch

    Corner(knob, 20)

    local click =
        Instance.new("TextButton")

    click.Size =
        UDim2.fromScale(1, 1)

    click.BackgroundTransparency = 1

    click.Text = ""

    click.Parent =
        frame

    click.MouseButton1Click:Connect(function()

        enabled =
            not enabled

        Tween(
            switch,
            0.18,
            {
                BackgroundColor3 =
                    enabled
                    and Config.AccentDark
                    or Color3.fromRGB(
                        170,
                        195,
                        210
                    )
            }
        )

        Tween(
            knob,
            0.2,
            {
                Position =
                    enabled
                    and UDim2.new(
                        1,
                        -18,
                        0.5,
                        -8
                    )
                    or UDim2.new(
                        0,
                        2,
                        0.5,
                        -8
                    )
            },
            Enum.EasingStyle.Back
        )

        pcall(function()
            callback(enabled)
        end)

    end)

    return frame
end

local function AddSlider(
    parent,
    text,
    minimum,
    maximum,
    default,
    callback
)

    local value =
        default or minimum

    local frame =
        Instance.new("Frame")

    frame.Size =
        UDim2.new(
            1,
            0,
            0,
            52
        )

    frame.BackgroundColor3 =
        Config.Cloud

    frame.BackgroundTransparency =
        0.1

    frame.Parent =
        parent

    Corner(frame, 9)

    local label =
        Instance.new("TextLabel")

    label.Size =
        UDim2.new(
            0.7,
            0,
            0,
            22
        )

    label.Position =
        UDim2.new(
            0,
            12,
            0,
            4
        )

    label.BackgroundTransparency = 1

    label.Text = text

    label.TextColor3 =
        Config.Text

    label.Font =
        Enum.Font.GothamMedium

    label.TextSize = 12

    label.TextXAlignment =
        Enum.TextXAlignment.Left

    label.Parent =
        frame

    local valueLabel =
        Instance.new("TextLabel")

    valueLabel.Size =
        UDim2.new(
            0,
            55,
            0,
            22
        )

    valueLabel.Position =
        UDim2.new(
            1,
            -67,
            0,
            4
        )

    valueLabel.BackgroundTransparency = 1

    valueLabel.Text =
        tostring(value)

    valueLabel.TextColor3 =
        Config.AccentDark

    valueLabel.Font =
        Enum.Font.GothamBold

    valueLabel.TextSize = 11

    valueLabel.TextXAlignment =
        Enum.TextXAlignment.Right

    valueLabel.Parent =
        frame

    local bar =
        Instance.new("Frame")

    bar.Size =
        UDim2.new(
            1,
            -24,
            0,
            7
        )

    bar.Position =
        UDim2.new(
            0,
            12,
            0,
            34
        )

    bar.BackgroundColor3 =
        Color3.fromRGB(
            170,
            205,
            220
        )

    bar.Parent =
        frame

    Corner(bar, 10)

    local fill =
        Instance.new("Frame")

    fill.Size =
        UDim2.new(
            (value - minimum)
                / (maximum - minimum),

            0,

            1,
            0
        )

    fill.BackgroundColor3 =
        Config.AccentDark

    fill.Parent =
        bar

    Corner(fill, 10)

    local sliding = false

    local function Update(input)

        local percentage =
            math.clamp(
                (
                    input.Position.X -
                    bar.AbsolutePosition.X
                )
                / bar.AbsoluteSize.X,

                0,
                1
            )

        value =
            math.floor(
                minimum +
                (
                    maximum -
                    minimum
                ) * percentage
            )

        valueLabel.Text =
            tostring(value)

        fill.Size =
            UDim2.new(
                percentage,
                0,
                1,
                0
            )

        pcall(function()
            callback(value)
        end)
    end

    bar.InputBegan:Connect(function(input)

        if input.UserInputType ==
            Enum.UserInputType.MouseButton1

            or input.UserInputType ==
            Enum.UserInputType.Touch then

            sliding = true

            Update(input)
        end
    end)

    UserInputService.InputChanged:Connect(function(input)

        if not sliding then
            return
        end

        if input.UserInputType ==
            Enum.UserInputType.MouseMovement

            or input.UserInputType ==
            Enum.UserInputType.Touch then

            Update(input)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)

        if input.UserInputType ==
            Enum.UserInputType.MouseButton1

            or input.UserInputType ==
            Enum.UserInputType.Touch then

            sliding = false
        end
    end)

    return frame
end

local function AddTextBox(
    parent,
    text,
    placeholder,
    callback
)

    local frame =
        Instance.new("Frame")

    frame.Size =
        UDim2.new(
            1,
            0,
            0,
            40
        )

    frame.BackgroundColor3 =
        Config.Cloud

    frame.BackgroundTransparency =
        0.1

    frame.Parent =
        parent

    Corner(frame, 9)

    local label =
        Instance.new("TextLabel")

    label.Size =
        UDim2.new(
            0.5,
            0,
            1,
            0
        )

    label.Position =
        UDim2.new(
            0,
            12,
            0,
            0
        )

    label.BackgroundTransparency = 1

    label.Text = text

    label.TextColor3 =
        Config.Text

    label.Font =
        Enum.Font.GothamMedium

    label.TextSize = 12

    label.TextXAlignment =
        Enum.TextXAlignment.Left

    label.Parent =
        frame

    local input =
        Instance.new("TextBox")

    input.Size =
        UDim2.new(
            0,
            130,
            0,
            26
        )

    input.Position =
        UDim2.new(
            1,
            -140,
            0.5,
            -13
        )

    input.BackgroundColor3 =
        Config.White

    input.BackgroundTransparency =
        0.15

    input.PlaceholderText =
        placeholder or "Type..."

    input.PlaceholderColor3 =
        Config.SubText

    input.Text = ""

    input.TextColor3 =
        Config.Text

    input.Font =
        Enum.Font.Gotham

    input.TextSize = 11

    input.ClearTextOnFocus = false

    input.Parent =
        frame

    Corner(input, 7)

    input.FocusLost:Connect(
        function(enterPressed)

            if enterPressed then

                pcall(function()
                    callback(input.Text)
                end)

            end
        end
    )

    return frame
end

--==================================================
-- TABS
--==================================================

local PlayerTab =
    CreateTab("Player")

local CombatTab =
    CreateTab("Combat")

local VisualsTab =
    CreateTab("Visuals")

local SettingsTab =
    CreateTab("Settings")

--==================================================
-- PLAYER
--==================================================

AddSection(
    PlayerTab,
    "Movement"
)

AddSlider(
    PlayerTab,
    "Walk Speed",
    16,
    200,
    16,

    function(value)

        local character =
            LocalPlayer.Character

        local humanoid =
            character
            and character:FindFirstChildOfClass(
                "Humanoid"
            )

        if humanoid then
            humanoid.WalkSpeed = value
        end
    end
)

AddSlider(
    PlayerTab,
    "Jump Power",
    50,
    300,
    50,

    function(value)

        local character =
            LocalPlayer.Character

        local humanoid =
            character
            and character:FindFirstChildOfClass(
                "Humanoid"
            )

        if humanoid then

            humanoid.UseJumpPower = true

            humanoid.JumpPower = value
        end
    end
)

local infiniteJump = false

AddToggle(
    PlayerTab,
    "Infinite Jump",
    false,

    function(state)

        infiniteJump = state
    end
)

UserInputService.JumpRequest:Connect(
    function()

        if not infiniteJump then
            return
        end

        local character =
            LocalPlayer.Character

        local humanoid =
            character
            and character:FindFirstChildOfClass(
                "Humanoid"
            )

        if humanoid then

            humanoid:ChangeState(
                Enum.HumanoidStateType.Jumping
            )

        end
    end
)

local noclip = false

AddToggle(
    PlayerTab,
    "Noclip",
    false,

    function(state)

        noclip = state
    end
)

RunService.Stepped:Connect(
    function()

        if not noclip then
            return
        end

        local character =
            LocalPlayer.Character

        if character then

            for _, object in ipairs(
                character:GetDescendants()
            ) do

                if object:IsA("BasePart") then
                    object.CanCollide = false
                end

            end

        end
    end
)

--==================================================
-- COMBAT
--==================================================

AddSection(
    CombatTab,
    "Player Utilities"
)

AddTextBox(
    CombatTab,
    "Teleport To",
    "Username...",

    function(text)

        if text == "" then
            return
        end

        local target

        for _, player in ipairs(
            Players:GetPlayers()
        ) do

            if player ~= LocalPlayer then

                if string.lower(
                    player.Name
                ):sub(
                    1,
                    #text
                )
                ==
                string.lower(text) then

                    target = player

                    break
                end
            end
        end

        if target
            and target.Character
            and target.Character:
                FindFirstChild(
                    "HumanoidRootPart"
                )
            and LocalPlayer.Character
            and LocalPlayer.Character:
                FindFirstChild(
                    "HumanoidRootPart"
                ) then

            LocalPlayer.Character
                .HumanoidRootPart.CFrame =

                target.Character
                .HumanoidRootPart.CFrame
        end
    end
)

AddButton(
    CombatTab,
    "Reset Character",

    function()

        local character =
            LocalPlayer.Character

        local humanoid =
            character
            and character:FindFirstChildOfClass(
                "Humanoid"
            )

        if humanoid then
            humanoid.Health = 0
        end
    end
)

--==================================================
-- VISUALS
--==================================================

AddSection(
    VisualsTab,
    "Visual Settings"
)

local espEnabled = false

local function ApplyESP()

    for _, player in ipairs(
        Players:GetPlayers()
    ) do

        if player ~= LocalPlayer
            and player.Character then

            local old =
                player.Character:
                FindFirstChild(
                    "WindCloudHighlight"
                )

            if old then
                old:Destroy()
            end

            if espEnabled then

                local highlight =
                    Instance.new("Highlight")

                highlight.Name =
                    "WindCloudHighlight"

                highlight.FillColor =
                    Config.Accent

                highlight.FillTransparency =
                    0.45

                highlight.OutlineColor =
                    Config.White

                highlight.OutlineTransparency =
                    0

                highlight.Parent =
                    player.Character
            end
        end
    end
end

AddToggle(
    VisualsTab,
    "Player Highlights",
    false,

    function(state)

        espEnabled = state

        ApplyESP()
    end
)

Players.PlayerAdded:Connect(
    function(player)

        player.CharacterAdded:Connect(
            function()

                task.wait(0.5)

                if espEnabled then
                    ApplyESP()
                end

            end
        )
    end
)

local fullbright = false

AddToggle(
    VisualsTab,
    "Fullbright",
    false,

    function(state)

        fullbright = state

        if state then

            Lighting.Brightness = 2

            Lighting.ClockTime = 14

            Lighting.FogEnd = 100000

            Lighting.GlobalShadows = false

        else

            Lighting.Brightness = 1

            Lighting.GlobalShadows = true

        end
    end
)

AddSlider(
    VisualsTab,
    "Camera FOV",
    70,
    120,
    70,

    function(value)

        local camera =
            workspace.CurrentCamera

        if camera then
            camera.FieldOfView = value
        end
    end
)

--==================================================
-- SETTINGS
--==================================================

AddSection(
    SettingsTab,
    "Wind Cloud"
)

AddButton(
    SettingsTab,
    "Rejoin Server",

    function()

        TeleportService:
            TeleportToPlaceInstance(
                game.PlaceId,
                game.JobId,
                LocalPlayer
            )
    end
)

AddButton(
    SettingsTab,
    "Reset UI Position",

    function()

        Tween(
            Main,
            0.35,

            {
                Position =
                    UDim2.new(
                        0.5,
                        0,
                        0.5,
                        0
                    )
            }
        )
    end
)

AddSection(
    SettingsTab,
    "Information"
)

local Info =
    Instance.new("TextLabel")

Info.Size =
    UDim2.new(
        1,
        0,
        0,
        78
    )

Info.BackgroundColor3 =
    Config.Cloud

Info.BackgroundTransparency =
    0.1

Info.Text =
    "WIND CLOUD HUB\n\n" ..
    "Version " ..
    Config.Version ..
    "\nRightControl = Show / Hide"

Info.TextColor3 =
    Config.Text

Info.Font =
    Enum.Font.GothamMedium

Info.TextSize = 11

Info.TextWrapped = true

Info.TextXAlignment =
    Enum.TextXAlignment.Left

Info.TextYAlignment =
    Enum.TextYAlignment.Center

Info.Parent =
    SettingsTab

Corner(Info, 9)

local InfoPadding =
    Instance.new("UIPadding")

InfoPadding.PaddingLeft =
    UDim.new(0, 12)

InfoPadding.Parent =
    Info

--==================================================
-- LOADING TRANSITION
--==================================================

task.spawn(function()

    local duration = 3.2

    local startTime =
        os.clock()

    while LoadingScreen.Parent do

        local progress =
            math.clamp(
                (
                    os.clock()
                    - startTime
                )
                / duration,

                0,
                1
            )

        LoadingFill.Size =
            UDim2.new(
                progress,
                0,
                1,
                0
            )

        LoadingPercent.Text =
            tostring(
                math.floor(
                    progress * 100
                )
            )
            .. "%"

        if progress >= 1 then
            break
        end

        RunService.RenderStepped:Wait()
    end

    LoadingSubtitle.Text =
        "Welcome to the clouds."

    LoadingPercent.Text =
        "100%"

    task.wait(0.5)

    --==================================================
    -- LOADING SKY + LOADING SCREEN FADE
    --==================================================

    Tween(
        LoadingScreen,
        0.9,
        {
            GroupTransparency = 1
        },
        Enum.EasingStyle.Quad,
        Enum.EasingDirection.Out
    )

    task.wait(0.95)

    if LoadingScreen
        and LoadingScreen.Parent then

        LoadingScreen:Destroy()
    end

    --==================================================
    -- HUB SKY FADE IN
    --==================================================

    SkyLayer.Visible = true

    SkyLayer.GroupTransparency = 1

    Tween(
        SkyLayer,
        0.7,
        {
            GroupTransparency = 0
        },
        Enum.EasingStyle.Quad,
        Enum.EasingDirection.Out
    )

    --==================================================
    -- HUB OPEN
    --==================================================

    Tween(
        Main,
        0.65,
        {
            Size =
                UDim2.new(
                    0,
                    600,
                    0,
                    410
                ),

            Position =
                UDim2.new(
                    0.5,
                    0,
                    0.5,
                    0
                )
        },

        Enum.EasingStyle.Back,
        Enum.EasingDirection.Out
    )

end)
