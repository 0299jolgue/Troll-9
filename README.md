-- Jolgue Hub Completo
if game.CoreGui:FindFirstChild("JolgueHub") then
    game.CoreGui.JolgueHub:Destroy()
end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "JolgueHub"
gui.ResetOnSpawn = false

local restoreBtn = Instance.new("TextButton", gui)
restoreBtn.Size = UDim2.new(0, 140, 0, 35)
restoreBtn.Position = UDim2.new(0, 10, 0.5, -20)
restoreBtn.BackgroundColor3 = Color3.fromRGB(30,30,30)
restoreBtn.TextColor3 = Color3.new(1,1,1)
restoreBtn.Font = Enum.Font.GothamBold
restoreBtn.TextSize = 16
restoreBtn.Text = "Abrir Jolgue Hub"
restoreBtn.Visible = false
Instance.new("UICorner", restoreBtn)

local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 320, 0, 400)
mainFrame.Position = UDim2.new(0.5, -160, 0.5, -200)
mainFrame.BackgroundColor3 = Color3.fromRGB(45,45,45)
Instance.new("UICorner", mainFrame)

restoreBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    restoreBtn.Visible = false
end)

local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, -50, 0, 40)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Jolgue Hub"
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.TextXAlignment = Enum.TextXAlignment.Left

local minimize = Instance.new("TextButton", mainFrame)
minimize.Size = UDim2.new(0, 35, 0, 35)
minimize.Position = UDim2.new(1, -40, 0, 5)
minimize.BackgroundColor3 = Color3.fromRGB(70,70,70)
minimize.TextColor3 = Color3.new(1,1,1)
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 20
minimize.Text = "-"
Instance.new("UICorner", minimize)

minimize.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    restoreBtn.Visible = true
end)

-- NAV
local nav = Instance.new("Frame", mainFrame)
nav.Size = UDim2.new(1, -20, 0, 40)
nav.Position = UDim2.new(0, 10, 0, 45)
nav.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", nav)
layout.FillDirection = Enum.FillDirection.Horizontal
layout.HorizontalAlignment = Enum.HorizontalAlignment.Left
layout.Padding = UDim.new(0, 10)

-- Botão criador padrão
local function criarBotao(texto, parent, callback)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Text = texto
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 16
    btn.TextColor3 = Color3.new(1,1,1)
    btn.BackgroundColor3 = Color3.fromRGB(65,65,65)
    Instance.new("UICorner", btn)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

-- Abas
local abaMain = Instance.new("Frame", mainFrame)
abaMain.Size = UDim2.new(1, -20, 0, 280)
abaMain.Position = UDim2.new(0, 10, 0, 90)
abaMain.BackgroundTransparency = 1
abaMain.Visible = true

local abaArmas = Instance.new("Frame", mainFrame)
abaArmas.Size = abaMain.Size
abaArmas.Position = abaMain.Position
abaArmas.BackgroundTransparency = 1
abaArmas.Visible = false

-- Botões de navegação
criarBotao("Principal", nav, function()
    abaMain.Visible = true
    abaArmas.Visible = false
end)

criarBotao("Armas", nav, function()
    abaMain.Visible = false
    abaArmas.Visible = true
end)

criarBotao("Fechar", nav, function()
    gui:Destroy()
end)

-- Layout Principal
local layoutMain = Instance.new("UIListLayout", abaMain)
layoutMain.Padding = UDim.new(0, 8)
layoutMain.SortOrder = Enum.SortOrder.LayoutOrder

-- Botão Girar Player
local btnGirar = criarBotao("Girar Player", abaMain, function() end)
local girar = false
btnGirar.MouseButton1Click:Connect(function()
    girar = not girar
    btnGirar.Text = girar and "Parar Giro" or "Girar Player"
    task.spawn(function()
        while girar and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") do
            LocalPlayer.Character:SetPrimaryPartCFrame(
                LocalPlayer.Character.PrimaryPart.CFrame * CFrame.Angles(0, 2.094, 0)
            )
            task.wait(1/60)
        end
    end)
end)

-- Botão Infinite Yield
criarBotao("Infinite Yield", abaMain, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
end)

-- Layout Armas
local layoutArmas = Instance.new("UIListLayout", abaArmas)
layoutArmas.Padding = UDim.new(0, 8)
layoutArmas.SortOrder = Enum.SortOrder.LayoutOrder

-- Criador de Tools
local function criarTool(nome, cor, aoAtivar)
    local tool = Instance.new("Tool")
    tool.Name = nome
    tool.RequiresHandle = true

    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1, 1, 4)
    handle.BrickColor = BrickColor.new(cor)
    handle.Material = Enum.Material.Metal
    handle.CanCollide = false
    handle.Anchored = false
    handle.Parent = tool

    tool.Activated:Connect(aoAtivar)
    return tool
end

-- Espada falsa
criarBotao("Espada", abaArmas, function()
    local espada = criarTool("EspadaTroll", "Really red", function()
        local char = LocalPlayer.Character
        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum then
                hum:TakeDamage(1)
                print("Cortou o ar!")
            end
        end
    end)
    espada.Parent = LocalPlayer.Backpack
end)

-- Arma falsa melhorada
criarBotao("Arma", abaArmas, function()
    local arma = criarTool("ArmaFake", "Black", function()
        local origin = LocalPlayer.Character.Head.Position
        local direction = LocalPlayer.Character.Head.CFrame.LookVector

        local bala = Instance.new("Part")
        bala.Size = Vector3.new(0.3, 0.3, 0.3)
        bala.Shape = Enum.PartType.Ball
        bala.Material = Enum.Material.Neon
        bala.BrickColor = BrickColor.new("Bright yellow")
        bala.Position = origin + direction * 2
        bala.CanCollide = false
        bala.Anchored = false
        bala.Parent = workspace

        local bv = Instance.new("BodyVelocity", bala)
        bv.Velocity = direction * 150
        bv.MaxForce = Vector3.new(1e5, 1e5, 1e5)

        local sound = Instance.new("Sound", bala)
        sound.SoundId = "rbxassetid://138186576"
        sound.Volume = 1
        sound:Play()

        bala.Touched:Connect(function(hit)
            local hum = hit.Parent and hit.Parent:FindFirstChildOfClass("Humanoid")
            if hum and hum ~= LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
                hum:TakeDamage(5)
                bala:Destroy()
            end
        end)

        game.Debris:AddItem(bala, 5)
    end)
    arma.Parent = LocalPlayer.Backpack
end)
