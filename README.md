local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

-- ✅ Auto-Farm NPCs
local function AutoFarm()
    while wait(0.5) do
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                wait(0.2)
                local args = {
                    [1] = npc
                }
                game:GetService("ReplicatedStorage").Remotes.Damage:FireServer(unpack(args))
            end
        end
    end
end

-- ✅ ESP de Jogadores (Mostra Todos no Mapa)
local function ESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local Billboard = Instance.new("BillboardGui", player.Character.Head)
            Billboard.Size = UDim2.new(2, 0, 2, 0)
            Billboard.AlwaysOnTop = true
            
            local NameTag = Instance.new("TextLabel", Billboard)
            NameTag.Text = player.Name
            NameTag.Size = UDim2.new(1, 0, 1, 0)
            NameTag.TextColor3 = Color3.fromRGB(255, 0, 0)
            NameTag.BackgroundTransparency = 1
        end
    end
end

-- ✅ Teleporte para um Local Específico
local function Teleport(pos)
    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(pos)
end

-- 🚀 GUI Simples para Ativar Funções
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 200, 0, 200)
Frame.Position = UDim2.new(0.8, 0, 0.2, 0)
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

local ButtonFarm = Instance.new("TextButton", Frame)
ButtonFarm.Size = UDim2.new(1, 0, 0.3, 0)
ButtonFarm.Text = "Auto-Farm"
ButtonFarm.MouseButton1Click:Connect(AutoFarm)

local ButtonESP = Instance.new("TextButton", Frame)
ButtonESP.Size = UDim2.new(1, 0, 0.3, 0)
ButtonESP.Position = UDim2.new(0, 0, 0.35, 0)
ButtonESP.Text = "Ativar ESP"
ButtonESP.MouseButton1Click:Connect(ESP)

local ButtonTeleport = Instance.new("TextButton", Frame)
ButtonTeleport.Size = UDim2.new(1, 0, 0.3, 0)
ButtonTeleport.Position = UDim2.new(0, 0, 0.7, 0)
ButtonTeleport.Text = "Teleport para Ilha"
ButtonTeleport.MouseButton1Click:Connect(function()
    Teleport(Vector3.new(1200, 150, 3400)) -- Modifique para a coordenada desejada
end)
