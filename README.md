
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")

-- Função para desenhar uma linha entre dois pontos
local function drawLine(from, to, color)
    local line = Instance.new("Part")
    line.Size = Vector3.new(0.2, 0.2, (from - to).Magnitude)  -- Tamanho da linha
    line.Anchored = true
    line.CanCollide = false
    line.Color = color
    line.Material = Enum.Material.SmoothPlastic

    -- Posicionar a linha no meio entre os dois pontos
    line.CFrame = CFrame.new((from + to) / 2, to)

    line.Parent = Workspace

    -- Depois de um tempo, a linha desaparece
    game:GetService("Debris"):AddItem(line, 5)  -- Apaga a linha após 5 segundos
end

-- Função para verificar se é um aliado
local function isAlly(player1, player2)
    return player1.Team == player2.Team
end

-- Função para atualizar o ESP para todos os jogadores
local function updateESP()
    local localPlayer = Players.LocalPlayer
    if not localPlayer then return end

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local target = player.Character.HumanoidRootPart.Position
            local playerPos = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
            
            if playerPos then
                local color = isAlly(localPlayer, player) and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(0, 0, 0)
                drawLine(playerPos.Position, target, color)
            end
        end
    end
end

-- Atualiza o ESP a cada 0.5 segundos
while true do
    updateESP()
    wait(0.5)
end
