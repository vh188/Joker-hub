
--[[
    JOKER HUB V3
    Roblox Mobile Edition
    Interface demonstrativa
]]

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local old = playerGui:FindFirstChild("JokerHub")
if old then old:Destroy() end

local gui = Instance.new("ScreenGui")
gui.Name = "JokerHub"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = playerGui

local WHITE = Color3.fromRGB(245,245,245)
local BLACK = Color3.fromRGB(8,8,8)
local GRAY = Color3.fromRGB(35,35,35)

-- BOTÃO FLUTUANTE
local openButton = Instance.new("ImageButton")
openButton.Name = "JokerButton"
openButton.Size = UDim2.fromOffset(72,72)
openButton.Position = UDim2.new(1,-90,0.65,0)
openButton.BackgroundColor3 = BLACK
openButton.Image = "rbxassetid://0" -- COLOQUE SUA IMAGEM AQUI
openButton.ScaleType = Enum.ScaleType.Crop
openButton.Parent = gui

local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(1,0)
buttonCorner.Parent = openButton

local buttonStroke = Instance.new("UIStroke")
buttonStroke.Color = WHITE
buttonStroke.Thickness = 2
buttonStroke.Parent = openButton

-- PAINEL PRINCIPAL
local panel = Instance.new("Frame")
panel.Name = "MainPanel"
panel.Size = UDim2.new(0.86,0,0,410)
panel.Position = UDim2.new(0.5,0,0.5,0)
panel.AnchorPoint = Vector2.new(0.5,0.5)
panel.BackgroundColor3 = BLACK
panel.Visible = false
panel.Parent = gui

local panelCorner = Instance.new("UICorner")
panelCorner.CornerRadius = UDim.new(0,15)
panelCorner.Parent = panel

local panelStroke = Instance.new("UIStroke")
panelStroke.Color = WHITE
panelStroke.Thickness = 1.5
panelStroke.Parent = panel

local sizeLimit = Instance.new("UISizeConstraint")
sizeLimit.MaxSize = Vector2.new(380,460)
sizeLimit.MinSize = Vector2.new(260,380)
sizeLimit.Parent = panel

-- CABEÇALHO
local header = Instance.new("TextLabel")
header.Size = UDim2.new(1,-60,0,45)
header.Position = UDim2.fromOffset(15,5)
header.BackgroundTransparency = 1
header.Text = "♠ JOKER HUB"
header.TextColor3 = WHITE
header.TextSize = 23
header.Font = Enum.Font.GothamBlack
header.TextXAlignment = Enum.TextXAlignment.Left
header.Parent = panel

local version = Instance.new("TextLabel")
version.Size = UDim2.new(1,-30,0,20)
version.Position = UDim2.fromOffset(15,43)
version.BackgroundTransparency = 1
version.Text = "MOBILE EDITION • V3"
version.TextColor3 = Color3.fromRGB(150,150,150)
version.TextSize = 10
version.Font = Enum.Font.GothamBold
version.TextXAlignment = Enum.TextXAlignment.Left
version.Parent = panel

-- BOTÃO FECHAR
local close = Instance.new("TextButton")
close.Size = UDim2.fromOffset(35,35)
close.Position = UDim2.new(1,-45,0,8)
close.Text = "X"
close.TextColor3 = WHITE
close.TextSize = 17
close.Font = Enum.Font.GothamBold
close.BackgroundColor3 = GRAY
close.Parent = panel

Instance.new("UICorner",close).CornerRadius = UDim.new(0,8)

-- PARTÍCULAS ESPACIAIS
local particles = Instance.new("Frame")
particles.Name = "Particles"
particles.Size = UDim2.fromScale(1,1)
particles.BackgroundTransparency = 1
particles.ClipsDescendants = true
particles.ZIndex = 0
particles.Parent = gui

for i = 1,30 do
    local dot = Instance.new("Frame")
    local s = math.random(2,6)

    dot.Size = UDim2.fromOffset(s,s)
    dot.Position = UDim2.fromScale(
        math.random(),
        math.random()
    )
    dot.BackgroundColor3 =
        i % 2 == 0 and WHITE or Color3.fromRGB(80,80,80)
    dot.BackgroundTransparency = 0.2
    dot.BorderSizePixel = 0
    dot.ZIndex = 0
    dot.Parent = particles

    Instance.new("UICorner",dot).CornerRadius =
        UDim.new(1,0)

    task.spawn(function()
        while dot.Parent do
            local duration = math.random(3,8)

            local tween = TweenService:Create(
                dot,
                TweenInfo.new(
                    duration,
                    Enum.EasingStyle.Sine,
                    Enum.EasingDirection.InOut
                ),
                {
                    Position = UDim2.fromScale(
                        math.random(),
                        math.random()
                    )
                }
            )

            tween:Play()
            tween.Completed:Wait()
        end
    end)
end

-- ÁREA DAS FUNÇÕES
local content = Instance.new("Frame")
content.Size = UDim2.new(1,-30,1,-115)
content.Position = UDim2.fromOffset(15,75)
content.BackgroundTransparency = 1
content.Parent = panel

local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0,9)
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Parent = content

local function createSection(text)
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1,0,0,25)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = WHITE
    label.TextSize = 12
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = content
end

-- INDICADORES VISUAIS
local demoLine = Instance.new("Frame")
demoLine.Size = UDim2.fromOffset(3,160)
demoLine.Position = UDim2.new(0.5,0,0.5,-80)
demoLine.BackgroundColor3 = WHITE
demoLine.BorderSizePixel = 0
demoLine.Visible = false
demoLine.Parent = gui

local demoMap = Instance.new("Frame")
demoMap.Size = UDim2.fromOffset(110,110)
demoMap.Position = UDim2.new(1,-125,0,30)
demoMap.BackgroundColor3 = BLACK
demoMap.Visible = false
demoMap.Parent = gui
Instance.new("UICorner",demoMap).CornerRadius =
    UDim.new(0,10)

local mapStroke = Instance.new("UIStroke")
mapStroke.Color = WHITE
mapStroke.Parent = demoMap

local mapDot = Instance.new("Frame")
mapDot.Size = UDim2.fromOffset(10,10)
mapDot.Position = UDim2.fromScale(0.5,0.5)
mapDot.BackgroundColor3 = WHITE
mapDot.BorderSizePixel = 0
mapDot.Parent = demoMap
Instance.new("UICorner",mapDot).CornerRadius =
    UDim.new(1,0)

local aimReticle = Instance.new("TextLabel")
aimReticle.Size = UDim2.fromOffset(50,50)
aimReticle.Position = UDim2.fromScale(0.5,0.5)
aimReticle.AnchorPoint = Vector2.new(0.5,0.5)
aimReticle.BackgroundTransparency = 1
aimReticle.Text = "✛"
aimReticle.TextColor3 = WHITE
aimReticle.TextSize = 38
aimReticle.Visible = false
aimReticle.Parent = gui

-- SISTEMA DE BOTÕES
local states = {}

local function createToggle(name, description, callback)

    local row = Instance.new("Frame")
    row.Size = UDim2.new(1,0,0,55)
    row.BackgroundColor3 = GRAY
    row.Parent = content

    Instance.new("UICorner",row).CornerRadius =
        UDim.new(0,9)

    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(0.67,0,0,25)
    title.Position = UDim2.fromOffset(10,4)
    title.BackgroundTransparency = 1
    title.Text = name
    title.TextColor3 = WHITE
    title.TextSize = 13
    title.Font = Enum.Font.GothamBold
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = row

    local sub = Instance.new("TextLabel")
    sub.Size = UDim2.new(0.67,0,0,18)
    sub.Position = UDim2.fromOffset(10,28)
    sub.BackgroundTransparency = 1
    sub.Text = description
    sub.TextColor3 = Color3.fromRGB(160,160,160)
    sub.TextSize = 9
    sub.Font = Enum.Font.Gotham
    sub.TextXAlignment = Enum.TextXAlignment.Left
    sub.Parent = row

    local toggle = Instance.new("TextButton")
    toggle.Size = UDim2.fromOffset(55,28)
    toggle.Position = UDim2.new(1,-65,0.5,-14)
    toggle.BackgroundColor3 = Color3.fromRGB(65,65,65)
    toggle.Text = "OFF"
    toggle.TextColor3 = WHITE
    toggle.TextSize = 11
    toggle.Font = Enum.Font.GothamBold
    toggle.Parent = row

    Instance.new("UICorner",toggle).CornerRadius =
        UDim.new(0,7)

    states[name] = false

    toggle.MouseButton1Click:Connect(function()
        states[name] = not states[name]

        local active = states[name]

        toggle.Text = active and "ON" or "OFF"

        TweenService:Create(
            toggle,
            TweenInfo.new(0.2),
            {
                BackgroundColor3 = active
                    and Color3.fromRGB(230,230,230)
                    or Color3.fromRGB(65,65,65)
            }
        ):Play()

        toggle.TextColor3 = active and BLACK or WHITE

        callback(active)
    end)
end

createSection("VISUALIZAÇÃO")

createToggle(
    "AUTO LINE",
    "Linha visual demonstrativa",
    function(active)
        demoLine.Visible = active
    end
)

createToggle(
    "MAP RADAR",
    "Mini mapa com marcador fictício",
    function(active)
        demoMap.Visible = active
    end
)

createSection("TREINAMENTO")

createToggle(
    "AIM TRAINER",
    "Retículo visual de treinamento",
    function(active)
        aimReticle.Visible = active
    end
)

-- RODAPÉ
local footer = Instance.new("TextLabel")
footer.Size = UDim2.new(1,-30,0,22)
footer.Position = UDim2.new(0,15,1,-28)
footer.BackgroundTransparency = 1
footer.Text = "JOKER HUB  •  BLACK & WHITE"
footer.TextColor3 = Color3.fromRGB(130,130,130)
footer.TextSize = 9
footer.Font = Enum.Font.GothamBold
footer.Parent = panel

-- ABRIR E FECHAR
local function animatePanel(open)
    if open then
        panel.Visible = true
        panel.Size = UDim2.fromOffset(250,350)

        TweenService:Create(
            panel,
            TweenInfo.new(0.25,Enum.EasingStyle.Back),
            {Size = UDim2.new(0.86,0,0,410)}
        ):Play()
    else
        local tween = TweenService:Create(
            panel,
            TweenInfo.new(0.18),
            {Size = UDim2.fromOffset(250,350)}
        )
        tween:Play()
        tween.Completed:Wait()
        panel.Visible = false
    end
end

openButton.MouseButton1Click:Connect(function()
    animatePanel(not panel.Visible)
end)

close.MouseButton1Click:Connect(function()
    animatePanel(false)
end)

-- ARRASTAR PAINEL NO CELULAR
local dragging = false
local dragStart
local startPosition

panel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch
    or input.UserInputType == Enum.UserInputType.MouseButton1 then

        dragging = true
        dragStart = input.Position
        startPosition = panel.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.Touch
        or input.UserInputType == Enum.UserInputType.MouseMovement
    ) then

        local delta = input.Position - dragStart

        panel.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch
    or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

print("JOKER HUB V3 carregado!")
