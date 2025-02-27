l-- Referência ao repositório open-source
print("Carregando script open-source da GreenHub...")

-- Open-source link
local openSourceLink = "https://raw.githubusercontent.com/username20338/Greenhub/main/script.lua"
loadstring(game:HttpGet(openSourceLink))()

-- ✅ Auto-Farm NPCs
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local function AutoFarm()
    while wait(0.5) do
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                -- Checando se o NPC está dentro do alcance
                if (LocalPlayer.Character.HumanoidRootPart.Position - npc.HumanoidRootPart.Position).Magnitude < 50 then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                    wait(0.2)
                    local args = {
                        [1] = npc
                    }
                    ReplicatedStorage.Remotes.Damage:FireServer(unpack(args))
                end
            end
        end
    end
end

-- ✅ ESP de Jogadores (Mostra Todos no Mapa)
local function ESP()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
            -- Verificando se a Billboard já foi criada
            if not player.Character.Head:FindFirstChild("BillboardGui") then
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
end

-- ✅ Teleporte para um Local Específico
local function Teleport(pos)
    -- Verificando se o personagem está carregado e pode ser teleportado
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(pos)
    end
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
ButtonFarm.MouseButton1Click:Connect(function()
    -- Exibe feedback ao clicar
    ButtonFarm.Text = "Fazendo Auto-Farm..."
    wait(0.2)
    ButtonFarm.Text = "Auto-Farm"
    AutoFarm()
end)

local ButtonESP = Instance.new("TextButton", Frame)
ButtonESP.Size = UDim2.new(1, 0, 0.3, 0)
ButtonESP.Position = UDim2.new(0, 0, 0.35, 0)
ButtonESP.Text = "Ativar ESP"
ButtonESP.MouseButton1Click:Connect(function()
    -- Exibe feedback ao clicar
    ButtonESP.Text = "ESP Ativado"
    wait(0.2)
    ButtonESP.Text = "Ativar ESP"
    ESP()
end)

local ButtonTeleport = Instance.new("TextButton", Frame)
ButtonTeleport.Size = UDim2.new(1, 0, 0.3, 0)
ButtonTeleport.Position = UDim2.new(0, 0, 0.7, 0)
ButtonTeleport.Text = "Teleport para Ilha"
ButtonTeleport.MouseButton1Click:Connect(function()
    -- Exibe feedback ao clicar
    ButtonTeleport.Text = "Teleportando..."
    wait(0.2)
    ButtonTeleport.Text = "Teleport para Ilha"
    Teleport(Vector3.new(1200, 150, 3400)) -- Modifique para a coordenada desejada
end)
