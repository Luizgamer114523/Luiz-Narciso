-- Infinite Jump (para uso em seu próprio jogo)
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local enabled = true -- começa ativado; altere se quiser começar desativado

local function getHumanoid()
    local char = player.Character or player.CharacterAdded:Wait()
    return char:FindFirstChildWhichIsA("Humanoid")
end

local humanoid = getHumanoid()
-- atualiza humanoid caso o personagem seja respawnado
player.CharacterAdded:Connect(function(char)
    humanoid = char:WaitForChild("Humanoid")
end)

-- Quando jogador solicita pulo (dispara ao apertar tecla de pular)
UserInputService.JumpRequest:Connect(function()
    if not enabled then return end
    if humanoid and humanoid.Parent then
        -- Força o humanoid a entrar no estado de pulo
        humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- Tecla para ativar/desativar (K)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.K then
        enabled = not enabled
        -- pequena notificação no output (remova/alterar para GUI se quiser)
        if enabled then
            warn("Pulo infinito: ATIVADO")
        else
            warn("Pulo infinito: DESATIVADO")
        end
    end
end)
