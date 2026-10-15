
-- Sistema básico de ovos e velocidade
-- Local: ServerScriptService

local Players = game:GetService("Players")

local VELOCIDADE_MAXIMA = 32

Players.PlayerAdded:Connect(function(player)
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local ovos = Instance.new("IntValue")
    ovos.Name = "Ovos"
    ovos.Value = 0
    ovos.Parent = leaderstats

    player.CharacterAdded:Connect(function(character)
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.WalkSpeed = VELOCIDADE_MAXIMA
    end)
end)

-- Coloque este Script no servidor.
-- Para cada ovo, crie uma Part chamada Ovo
-- e adicione um ProximityPrompt dentro dela.

local function configurarOvo(ovo)
    local prompt = ovo:FindFirstChildOfClass("ProximityPrompt")
    if not prompt then return end

    prompt.ActionText = "Coletar"
    prompt.ObjectText = "Ovo"

    prompt.Triggered:Connect(function(player)
        if not ovo.Parent then return end

        local stats = player:FindFirstChild("leaderstats")
        local contador = stats and stats:FindFirstChild("Ovos")

        if contador then
            contador.Value += 1
            ovo:Destroy()
        end
    end)
end

local pastaOvos = workspace:WaitForChild("Ovos")

for _, ovo in ipairs(pastaOvos:GetChildren()) do
    configurarOvo(ovo)
end

pastaOvos.ChildAdded:Connect(configurarOvo)
