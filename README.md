-- Auto-Farm Script para Blox Fruits com voo, atravessar paredes e pegar missões
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local tool = character:FindFirstChildOfClass("Tool")

-- Função para ativar o modo de voo
local function enableFly()
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(0, 50, 0)
    bodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
    bodyVelocity.Parent = character.HumanoidRootPart
end

-- Função para desativar o modo de voo
local function disableFly()
    for _, v in pairs(character.HumanoidRootPart:GetChildren()) do
        if v:IsA("BodyVelocity") then
            v:Destroy()
        end
    end
end

-- Função para atravessar paredes
local function noClip()
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = false
        end
    end
end

-- Função para pegar missões
local function getMission()
    for _, npc in pairs(workspace.NPCs:GetChildren()) do
        if npc:FindFirstChild("Head") and npc.Head:FindFirstChild("QuestMarker") then
            humanoid:MoveTo(npc.Head.Position)
            wait(1)
            fireproximityprompt(npc.Head.QuestMarker.ProximityPrompt)
            wait(1)
        end
    end
end

-- Função para atacar inimigos
local function attackEnemy(enemy)
    if tool and enemy and enemy:FindFirstChild("Humanoid") then
        repeat
            tool:Activate()
            humanoid:MoveTo(enemy.HumanoidRootPart.Position)
            wait(0.1)
        until enemy.Humanoid.Health <= 0 or not enemy.Parent
    end
end

-- Função para encontrar inimigos próximos
local function findEnemies()
    local enemies = {}
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            table.insert(enemies, v)
        end
    end
    return enemies
end

-- Loop principal de auto-farm
while wait(1) do
    enableFly()
    noClip()
    getMission()
    local enemies = findEnemies()
    for _, enemy in pairs(enemies) do
        attackEnemy(enemy)
    end
    disableFly()
end
