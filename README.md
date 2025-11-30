-- DAVID HUB BETA - CascadeUI Compacto
local CascadeUI = loadstring(game:HttpGet('https://raw.githubusercontent.com/SquidGurr/CascadeUI/main/CascadeUI.lua'))()

local Window = CascadeUI:CreateWindow({
    Title = "DAVID HUB BETA",
    Size = UDim2.new(0, 400, 0, 350),
    Position = UDim2.new(0.5, -200, 0.5, -175)
})

-- Aba Auto Farm
local AutoFarmTab = Window:CreateTab("Auto Farm")
local AutoFarmSection = AutoFarmTab:CreateSection("Scripts de Auto Farm")

AutoFarmSection:CreateButton({
    Name = "Lixeiro Hub",
    Callback = function()
        loadstring(game:HttpGet("https://pastebin.com/raw/5zj4depA", true))()
    end
})

-- Aba Teleport
local TeleportTab = Window:CreateTab("Teleport")
local TeleportSection = TeleportTab:CreateSection("Locais para Teleportar")

local locais = {
    ["🎰 Cassino"] = Vector3.new(358.96044921875, 31.594722747802734, -677.1649169921875),
    ["💰 Banco"] = Vector3.new(-357.4081115722656, 42.09412384033203, 512.9352416992188),
    ["🛒 Mercado"] = Vector3.new(-27.838581085205078, 42.27449035644531, 456.5347900390625),
    ["⚫ Mercado Black"] = Vector3.new(-245.01686096191406, 30.133441925048828, -236.37423706054688),
    ["💎 Cassino Jóias"] = Vector3.new(274.03900146484375, 62.174949645996094, -743.015380859375),
    ["🌳 Praça"] = Vector3.new(172.67263793945312, 43.77525329589844, 272.34918212890625),
    ["🏥 Hospital"] = Vector3.new(556.4091796875, 41.745765686035156, 796.4718017578125),
    ["📦 Entregar Boldin"] = Vector3.new(1148.663330078125, 41.749114990234375, 3724.093017578125),
    ["🔧 Fazer"] = Vector3.new(3433.80908203125, 41.745758056640625, 827.1817626953125)
}

for nome, coordenadas in pairs(locais) do
    TeleportSection:CreateButton({
        Name = nome,
        Callback = function()
            local jogador = game.Players.LocalPlayer
            local personagem = jogador.Character or jogador.CharacterAdded:Wait()
            local raiz = personagem:WaitForChild("HumanoidRootPart")
            
            raiz.CFrame = CFrame.new(coordenadas)
        end
    })
end

-- Aba Speed & Reviver
local SpeedTab = Window:CreateTab("Speed & Reviver")
local SpeedSection = SpeedTab:CreateSection("Controle de Velocidade")

-- Variáveis de controle
local Correndo = false
local VelocidadeNormal = 16
local VelocidadeCorrida = 50
local SpeedGUI = nil

-- Função para atualizar velocidade
local function AtualizarVelocidade()
    local character = game.Players.LocalPlayer.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            if Correndo then
                humanoid.WalkSpeed = VelocidadeCorrida
            else
                humanoid.WalkSpeed = VelocidadeNormal
            end
        end
    end
end

-- Criar GUI de Speed
local function CriarSpeedGUI()
    if SpeedGUI then
        SpeedGUI:Destroy()
        SpeedGUI = nil
    end

    -- Criar a GUI
    SpeedGUI = Instance.new("ScreenGui")
    SpeedGUI.Name = "CorrerGUI"
    SpeedGUI.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    SpeedGUI.ResetOnSpawn = false

    -- Frame principal QUADRADO
    local MainFrame = Instance.new("Frame")
    MainFrame.Size = UDim2.new(0, 60, 0, 60)
    MainFrame.Position = UDim2.new(0.78, 0, 0.82, 0)
    MainFrame.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    MainFrame.BorderSizePixel = 1
    MainFrame.BorderColor3 = Color3.fromRGB(100, 100, 100)
    MainFrame.BackgroundTransparency = 0
    MainFrame.Parent = SpeedGUI

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 8)
    UICorner.Parent = MainFrame

    -- Botão de Correr
    local CorrerButton = Instance.new("TextButton")
    CorrerButton.Size = UDim2.new(1, 0, 1, 0)
    CorrerButton.Position = UDim2.new(0, 0, 0, 0)
    CorrerButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    CorrerButton.BorderSizePixel = 0
    CorrerButton.Text = "Correr"
    CorrerButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CorrerButton.TextSize = 14
    CorrerButton.Font = Enum.Font.GothamBold
    CorrerButton.Parent = MainFrame

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 8)
    ButtonCorner.Parent = CorrerButton

    -- Sistema de arrastar
    local dragging = false
    local dragInput, dragStart, startPos

    local function update(input)
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end

    CorrerButton.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = MainFrame.Position
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    CorrerButton.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)

    -- Clique do botão de correr
    CorrerButton.MouseButton1Click:Connect(function()
        Correndo = not Correndo
        AtualizarVelocidade()
    end)

    -- Atualizar quando o personagem spawnar
    game.Players.LocalPlayer.CharacterAdded:Connect(function(character)
        wait(1)
        AtualizarVelocidade()
    end)

    -- Atualizar velocidade inicial
    if game.Players.LocalPlayer.Character then
        AtualizarVelocidade()
    end
end

-- Toggle para Botão Correr (mantendo como toggle)
SpeedSection:CreateToggle({
    Name = "Ativar Botão Correr",
    Default = false,
    Callback = function(Value)
        if Value then
            CriarSpeedGUI()
        else
            if SpeedGUI then
                SpeedGUI:Destroy()
                SpeedGUI = nil
            end
            Correndo = false
            AtualizarVelocidade()
        end
    end
})

-- Botão Reviver
SpeedSection:CreateButton({
    Name = "🔁 Reviver Personagem",
    Callback = function()
        local player = game.Players.LocalPlayer
        
        -- Procurar o evento que o jogo usa para morte/revive
        local action = game.ReplicatedStorage:FindFirstChild("MorteEvent")
        if action and action:FindFirstChild("Action") then
            action = action.Action
        else
            action = nil
        end
        
        if player.Character then
            local hum = player.Character:FindFirstChild("Humanoid")
            if hum then
                -- 1. Reviver o personagem local diretamente
                hum.Health = hum.MaxHealth
                hum:ChangeState(Enum.HumanoidStateType.Running)
                hum.PlatformStand = false
                hum.Sit = false
                
                -- 2. Acionar revive do servidor (se existir)
                if action then
                    pcall(function()
                        action:FireServer("Action1")
                    end)
                end
                
                -- 3. Fechar e apagar a tela de morte
                local pgui = player:FindFirstChild("PlayerGui")
                if pgui then
                    local tela = pgui:FindFirstChild("TelaMorte")
                    if tela then
                        tela.Enabled = false
                        tela:Destroy()
                    end
                end
            end
        end
    end
})

-- Slider de velocidade (até 250)
SpeedSection:CreateSlider({
    Name = "Velocidade de Corrida",
    Min = 16,
    Max = 250,
    Default = 50,
    Callback = function(Value)
        VelocidadeCorrida = Value
        if Correndo then
            AtualizarVelocidade()
        end
    end
})

-- Aba ESP Player (ESP do Rayfield)
local ESPTab = Window:CreateTab("ESP Player")
local ESPSection = ESPTab:CreateSection("ESP Player")

-- Variáveis do ESP
local ESPActive = false

-- Função para obter vida do jogador
local function GetPlayerHealth(player)
    if player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid then
            return math.floor(humanoid.Health), math.floor(humanoid.MaxHealth)
        end
    end
    return 0, 100
end

-- Função para obter team do jogador
local function GetPlayerTeam(player)
    if player.Team then
        return player.Team.Name
    end
    return "Sem Team"
end

-- Função para criar ESP
local function CreateESP()
    for _, player in ipairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character then
            local head = player.Character:FindFirstChild("Head")
            if head then
                -- Remover ESP antigo
                local oldESP = head:FindFirstChild("PlayerESP")
                if oldESP then
                    oldESP:Destroy()
                end
                
                -- Criar ESP
                local billboard = Instance.new("BillboardGui")
                billboard.Name = "PlayerESP"
                billboard.Size = UDim2.new(0, 120, 0, 35)
                billboard.StudsOffset = Vector3.new(0, 2.0, 0)
                billboard.AlwaysOnTop = true
                billboard.Adornee = head
                billboard.MaxDistance = 500
                billboard.Parent = head
                
                local uiListLayout = Instance.new("UIListLayout")
                uiListLayout.Padding = UDim.new(0, 1)
                uiListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
                uiListLayout.VerticalAlignment = Enum.VerticalAlignment.Center
                uiListLayout.Parent = billboard
                
                -- Nome
                local nameLabel = Instance.new("TextLabel")
                nameLabel.Size = UDim2.new(1, 0, 0, 12)
                nameLabel.BackgroundTransparency = 1
                nameLabel.Text = player.Name
                nameLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                nameLabel.TextSize = 10
                nameLabel.Font = Enum.Font.GothamBold
                nameLabel.TextStrokeTransparency = 0
                nameLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                nameLabel.TextXAlignment = Enum.TextXAlignment.Center
                nameLabel.Parent = billboard
                
                -- Vida (SEMPRE BRANCO)
                local healthLabel = Instance.new("TextLabel")
                healthLabel.Size = UDim2.new(1, 0, 0, 10)
                healthLabel.BackgroundTransparency = 1
                healthLabel.Text = "HP: 100 / 100"
                healthLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                healthLabel.TextSize = 8
                healthLabel.Font = Enum.Font.GothamBold
                healthLabel.TextStrokeTransparency = 0
                healthLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                healthLabel.TextXAlignment = Enum.TextXAlignment.Center
                healthLabel.Parent = billboard
                
                -- Team
                local teamLabel = Instance.new("TextLabel")
                teamLabel.Size = UDim2.new(1, 0, 0, 10)
                teamLabel.BackgroundTransparency = 1
                teamLabel.Text = "Team: " .. GetPlayerTeam(player)
                teamLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                teamLabel.TextSize = 8
                teamLabel.Font = Enum.Font.GothamBold
                teamLabel.TextStrokeTransparency = 0
                teamLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                teamLabel.TextXAlignment = Enum.TextXAlignment.Center
                teamLabel.Parent = billboard
                
                -- Função para atualizar informações
                local function UpdatePlayerInfo()
                    if not player.Character or not head.Parent then return end
                    
                    local health, maxHealth = GetPlayerHealth(player)
                    local team = GetPlayerTeam(player)
                    
                    healthLabel.Text = "HP: " .. health .. " / " .. maxHealth
                    healthLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                    
                    teamLabel.Text = "Team: " .. team
                    
                    if team == "Médico" or team:lower():find("medic") then
                        teamLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
                    elseif team == "Polícia" or team:lower():find("police") then
                        teamLabel.TextColor3 = Color3.fromRGB(0, 100, 255)
                    elseif team == "Criminoso" or team:lower():find("criminal") then
                        teamLabel.TextColor3 = Color3.fromRGB(255, 50, 50)
                    else
                        teamLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                    end
                end
                
                coroutine.wrap(function()
                    while ESPActive and player.Character and head.Parent do
                        UpdatePlayerInfo()
                        wait(0.5)
                    end
                end)()
                
                UpdatePlayerInfo()
            end
        end
    end
end

-- Função para remover ESP
local function RemoveESP()
    for _, player in ipairs(game.Players:GetPlayers()) do
        if player.Character then
            local head = player.Character:FindFirstChild("Head")
            if head and head:FindFirstChild("PlayerESP") then
                head.PlayerESP:Destroy()
            end
        end
    end
end

-- Toggle para ESP Player (igual ao "Ativar Botão Correr")
ESPSection:CreateToggle({
    Name = "Ativar ESP Player",
    Default = false,
    Callback = function(Value)
        ESPActive = Value
        if Value then
            CreateESP()
        else
            RemoveESP()
        end
    end
})

-- Detectar novos players quando ESP estiver ativo
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        wait(1)
        if ESPActive then
            CreateESP()
        end
    end)
end)

print("DAVID HUB BETA CascadeUI carregado com sucesso!")
