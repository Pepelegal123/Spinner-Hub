-- Auto-Farm Script para Blox Fruits
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local tool = character:FindFirstChildOfClass("Tool")

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
    local enemies = findEnemies()
    for _, enemy in pairs(enemies) do
        attackEnemy(enemy)
    end
end
