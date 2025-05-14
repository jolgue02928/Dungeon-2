-- ServerScript que gerencia o fluxo completo da dungeon
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local portalService = ReplicatedStorage:WaitForChild("PortalService")
local dungeonService = ReplicatedStorage:WaitForChild("DungeonService")

-- Função para iniciar a dungeon
local function startDungeon()
    print("Iniciando a Dungeon...")

    -- Etapa 1: Abrir o Portal (PortalTierIII)
    local portalArgs = { "PortalTierIII", 1 }
    portalService.RF.request_item_consumption:InvokeServer(unpack(portalArgs))
    print("Portal Tier III ativado!")

    wait(2)  -- Aguardar um pouco para o portal abrir

    -- Etapa 2: Pular o tempo de espera para começar a dungeon
    portalService.RF.skip_portal:InvokeServer()
    print("Tempo de espera pulado, dungeon iniciada!")

    -- Etapa 3: Invocar mobs (primeira wave)
    local pillarUUID = "d568ba3f-30f6-4847-b168-16827e1d1fd9"  -- Exemplo de UUID do pilar
    dungeonService.RF.spawn_wave:InvokeServer(pillarUUID)
    print("Mobs da primeira wave invocados!")

    wait(5)  -- Simulando o tempo para derrotar a wave de mobs

    -- Etapa 4: Matar os mobs e abrir a próxima porta da dungeon
    print("Mobs derrotados! Abrindo próxima porta...")
    local nextDoorUUID = "d568ba3f-30f6-4847-b168-16827e1d1fd9"  -- UUID da próxima porta
    dungeonService.RF.open_door:InvokeServer(nextDoorUUID)

    -- Repetir o processo de invocar mobs, derrotar e abrir portas até dropar a chave do Boss
    wait(3)

    -- Etapa 5: Invocar a chave do Boss
    local bossKeyUUID = "bossKeyUUID"  -- Substitua pelo UUID real da chave do Boss
    dungeonService.RF.spawn_boss_key:InvokeServer(bossKeyUUID)
    print("Chave do Boss Droppada!")

    wait(2)

    -- Etapa 6: Derrotar o Boss
    local bossUUID = "bossUUID"  -- Substitua pelo UUID do Boss
    dungeonService.RF.defeat_boss:InvokeServer(bossUUID)
    print("Boss derrotado!")

    -- Etapa 7: Recompensar o jogador após derrotar o Boss
    -- Aqui você pode adicionar código para recompensar o jogador, como dar itens ou moedas.
    -- Exemplo:
    local player = game.Players:GetPlayerByUserId(playerUserId)  -- Substitua com o UserId do jogador que completou a dungeon
    if player then
        player.leaderstats.Rewards.Value = player.leaderstats.Rewards.Value + 100  -- Adicionando recompensa
        print("Jogador recompensado com 100 moedas!")
    end
end

-- Disparar o fluxo de execução da dungeon automaticamente ao iniciar o jogo
startDungeon()
