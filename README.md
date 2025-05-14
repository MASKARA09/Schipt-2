-- Ativar ou desativar o ESP
local espAtivado = true
local teamCheck = true

-- Função para desenhar texto 3D na tela
function DrawText3D(x, y, z, text)
    local onScreen, _x, _y = World3dToScreen2d(x, y, z)
    local p = GetGameplayCamCoords()
    local dist = #(vector3(x, y, z) - p)

    local scale = (1 / dist) * 2
    local fov = (1 / GetGameplayCamFov()) * 100
    scale = scale * fov

    if onScreen then
        SetTextScale(0.35, 0.35)
        SetTextFont(4)
        SetTextProportional(1)
        SetTextColour(255, 0, 0, 215) -- Vermelho
        SetTextEntry("STRING")
        SetTextCentre(true)
        AddTextComponentString(text)
        DrawText(_x, _y)
    end
end

-- Loop principal
Citizen.CreateThread(function()
    while true do
        Wait(0)

        if not espAtivado then return end

        local localPlayer = PlayerId()
        local localPed = PlayerPedId()

        for _, player in ipairs(GetActivePlayers()) do
            if player ~= localPlayer then
                local ped = GetPlayerPed(player)

                if DoesEntityExist(ped) and not IsEntityDead(ped) then
                    if teamCheck then
                        -- Substitua esta função pela lógica correta de time do seu servidor
                        if GetPlayerTeam and GetPlayerTeam(player) == GetPlayerTeam(localPlayer) then
                            goto continue
                        end
                    end

                    local coords = GetEntityCoords(ped)
                    DrawText3D(coords.x, coords.y, coords.z + 1.0, GetPlayerName(player))
                end
            end
            ::continue::
        end
    end
end)
