-- ESP simples com Team Check
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Função para criar o ESP
local function createESP(player)
    if player == LocalPlayer or player.Team == LocalPlayer.Team then return end

    local box = Drawing.new("Square")
    box.Color = Color3.new(1, 0, 0)
    box.Thickness = 2
    box.Transparency = 1
    box.Visible = false
    box.Filled = false

    local nameText = Drawing.new("Text")
    nameText.Color = Color3.new(1, 1, 1)
    nameText.Size = 16
    nameText.Center = true
    nameText.Outline = true
    nameText.Visible = false

    RunService.RenderStepped:Connect(function()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Team ~= LocalPlayer.Team then
            local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
            if onScreen then
                local distance = (LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).magnitude
                local scale = 2000 / distance
                local size = Vector2.new(50, 100) * scale

                box.Size = size
                box.Position = Vector2.new(pos.X - size.X / 2, pos.Y - size.Y / 2)
                box.Visible = true

                nameText.Text = player.Name
                nameText.Position = Vector2.new(pos.X, pos.Y - size.Y / 2 - 15)
                nameText.Visible = true
            else
                box.Visible = false
                nameText.Visible = false
            end
        else
            box.Visible = false
            nameText.Visible = false
        end
    end)
end

-- Aplicar ESP a todos os jogadores existentes
for _, player in ipairs(Players:GetPlayers()) do
    createESP(player)
end

-- Novos jogadores
Players.PlayerAdded:Connect(function(player)
    createESP(player)
end)
