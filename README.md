--main
local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/realredz/BloxFruits/refs/heads/main/Source.lua"))()
--windows
local Window = Fluent:CreateWindow({
    Title = "ZXiters Hub Beta" .. Fluent.Version,
    TabWidth = 160, Size = UDim2.fromOffset(580, 460), Theme = "Dark"
})

--tabs
local Tabs = {
    Main = Window:AddTab({ Title = "Main" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

--notificação
Fluent:Notify({ Title = "New Noty", Content = "executed" })

--paragraph
Tabs.Main:AddParagraph({ Title = "Zx", Content = "ByTokyto!" })

--button
Tabs.Main:AddButton({ Title = "AutoFarm", Callback = function()-- Interface de Ativação/Desativação
local farmAtivo = false

local ScreenGui = Instance.new("ScreenGui")
local ToggleButton = Instance.new("TextButton")

ScreenGui.Parent = game.CoreGui
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Size = UDim2.new(0, 120, 0, 40)
ToggleButton.Text = "Auto Farm [OFF]"
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.TextSize = 18

ToggleButton.MouseButton1Click:Connect(function()
    farmAtivo = not farmAtivo
    ToggleButton.Text = "Auto Farm [" .. (farmAtivo and "ON" or "OFF") .. "]"
end)

-- Função de Auto Farm
spawn(function()
    while true do
        wait(1)
        if farmAtivo then
            pcall(function()
                local player = game.Players.LocalPlayer
                local character = player.Character or player.CharacterAdded:Wait()
                local level = player.Data.Level.Value
                
                -- Mapeia o inimigo baseado no nível do jogador
                local function getMobInfo()
                    if level <= 10 then
                        return "Bandit", CFrame.new(1060, 16, 1548)
                    elseif level <= 50 then
                        return "Monkey", CFrame.new(-1600, 15, 150)
                    elseif level <= 300 then
                        return "Pirate", CFrame.new(-300, 50, 400)
                    elseif level <= 750 then
                        return "Galley Pirate", CFrame.new(5581, 49, 3992)
                    elseif level <= 1200 then
                        return "Zombie", CFrame.new(-5700, 100, -725)
                    elseif level <= 1500 then
                        return "Ship Deckhand", CFrame.new(1212, 125, 33085)
                    elseif level <= 2000 then
                        return "Arctic Warrior", CFrame.new(6080, 400, -6237)
                    elseif level <= 2250 then
                        return "Water Fighter", CFrame.new(-3386, 239, -10542)
                    elseif level <= 2500 then
                        return "Dragon Crew Warrior", CFrame.new(-8743, 140, -6116)
                    else
                        return "Forest Pirate", CFrame.new(-13200, 331, -7710)
                    end
                end

                local mobName, mobPos = getMobInfo()

                -- Procura o mob
                for _, mob in pairs(workspace.Enemies:GetChildren()) do
                    if mob.Name == mobName and mob:FindFirstChild("HumanoidRootPart") and mob.Humanoid.Health > 0 then
                        repeat
                            wait()
                            character.HumanoidRootPart.CFrame = mob.HumanoidRootPart.CFrame * CFrame.new(0, 0, -3)
                            game:GetService("VirtualInputManager"):SendKeyEvent(true, "Z", false, game)
                        until mob.Humanoid.Health <= 0 or not farmAtivo
                    end
                end

                -- Teleporta para onde ficam os mobs
                character.HumanoidRootPart.CFrame = mobPos + Vector3.new(0, 5, 0)
            end)
        end
    end
end) end })


--button
Tabs.Main:AddButton({ Title = "AimBot", Callback = function()-- Aimbot simples para Blox Fruits (Delta / Arceus X)
-- Criado para fins de teste com amigos

-- Configurações
local aimbotAtivo = false
local alvo = nil
local fov = 200 -- Área de detecção do aimbot

-- Função para encontrar o jogador mais próximo dentro do FOV
function pegarAlvo()
    local players = game:GetService("Players")
    local localPlayer = players.LocalPlayer
    local camera = workspace.CurrentCamera
    local maisProximo, menorDistancia = nil, fov

    for _, player in pairs(players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local pos, dentroDaTela = camera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            if dentroDaTela then
                local distancia = (Vector2.new(pos.X, pos.Y) - Vector2.new(camera.ViewportSize.X/2, camera.ViewportSize.Y/2)).Magnitude
                if distancia < menorDistancia then
                    menorDistancia = distancia
                    maisProximo = player
                end
            end
        end
    end
    return maisProximo
end

-- Função principal do aimbot
game:GetService("RunService").RenderStepped:Connect(function()
    if aimbotAtivo then
        alvo = pegarAlvo()
        if alvo and alvo.Character and alvo.Character:FindFirstChild("HumanoidRootPart") then
            local camera = workspace.CurrentCamera
            camera.CFrame = CFrame.new(camera.CFrame.Position, alvo.Character.HumanoidRootPart.Position)
        end
    end
end)

-- Interface Simples com Toggle
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Toggle = Instance.new("TextButton", ScreenGui)

Toggle.Size = UDim2.new(0, 100, 0, 40)
Toggle.Position = UDim2.new(0, 20, 0, 100)
Toggle.Text = "Aimbot: OFF"
Toggle.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
Toggle.Font = Enum.Font.SourceSansBold
Toggle.TextSize = 20

Toggle.MouseButton1Click:Connect(function()
    aimbotAtivo = not aimbotAtivo
    Toggle.Text = "Aimbot: " .. (aimbotAtivo and "ON" or "OFF")
    Toggle.BackgroundColor3 = aimbotAtivo and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
end)end })
