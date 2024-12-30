-- Pegando os serviços necessários
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Função para criar uma linha entre dois pontos no jogo
local function createLineBetweenPlayers(player1, player2)
    -- Pegando as posições dos jogadores
    local char1 = player1.Character
    local char2 = player2.Character

    -- Garantindo que ambos os jogadores tenham personagens
    if char1 and char2 then
        local position1 = char1.HumanoidRootPart.Position
        local position2 = char2.HumanoidRootPart.Position

        -- Criando a parte visual da linha (usando uma "Part" no Roblox)
        local line = Instance.new("Part")
        line.Anchored = true
        line.CanCollide = false
        line.Size = Vector3.new(0.2, 0.2, (position1 - position2).Magnitude)
        line.Color = Color3.fromRGB(255, 255, 255) -- Cor branca
        line.Position = (position1 + position2) / 2 -- Posição no meio dos dois pontos
        line.CFrame = CFrame.new(position1, position2) -- Rotaciona a parte para conectar os dois pontos

        -- Adicionando a parte ao jogo
        line.Parent = game.Workspace

        -- Apagar a linha após 2 segundos
        game:GetService("Debris"):AddItem(line, 2)
    end
end

-- Função para garantir que só uma linha seja criada por jogador
local function connectPlayers()
    local localPlayer = Players.LocalPlayer

    -- Garantindo que o personagem do jogador local esteja carregado
    if localPlayer.Character then
        -- Criar linhas entre o jogador local e outros jogadores
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= localPlayer and player.Character then
                -- Criando a linha entre o jogador local e os outros jogadores
                createLineBetweenPlayers(localPlayer, player)
            end
        end
    end
end

-- Chama a função para conectar os jogadores
connectPlayers()
