local Players = game:GetService("Players")

-- Função que roda sempre que um jogador entra no jogo
local function onPlayerAdded(player)
	-- Cria a pasta que o Roblox reconhece como o placar de líderes
	local leaderstats = Instance.new("Folder")
	leaderstats.Name = "leaderstats"
	leaderstats.Parent = player

	-- Cria a pontuação (ex: Moedas, Pontos ou Ouro)
	local pontos = Instance.new("IntValue")
	pontos.Name = "Pontos"
	pontos.Value = 0 -- O jogador começa com 0 pontos
	pontos.Parent = leaderstats
end

-- Conecta a função ao evento de entrada de jogadores
Players.PlayerAdded:Connect(onPlayerAdded)
# Roblox-atari -- uma matriz de "pixels" estilo Atari retro no Roblox
local tamanhoDaTela = 16 -- Tela de 16x16 blocos
local tamanhoPixel = 2
local iniciarX = 0
local iniciarY = 20
local iniciarZ = 0

for x = 1, tamanhoDaTela do
    for y = 1, tamanhoDaTela do
        local pixel = Instance.new("Part")
        pixel.Size = Vector3.new(tamanhoPixel, tamanhoPixel, 0.5)
        pixel.Position = Vector3.new(iniciarX + (x * tamanhoPixel), iniciarY + (y * tamanhoPixel), iniciarZ)
        pixel.Anchored = true
        pixel.Material = Enum.Material.Neon -- Dá o efeito brilhante de monitor CRT antigo
        pixel.Parent = workspace
        
        -- Loop para fazer os pixels piscarem em cores retro (8-bits)
        task.spawn(function()
            local coresRetro = {
                Color3.fromRGB(0, 0, 0),       -- Preto
                Color3.fromRGB(255, 0, 0),     -- Vermelho Atari
                Color3.fromRGB(0, 255, 0),     -- Verde
                Color3.fromRGB(0, 0, 255),     -- Azul
                Color3.fromRGB(255, 255, 0)    -- Amarelo
            }
            while true do
                pixel.Color = coresRetro[math.random(1, #coresRetro)]
                task.wait(math.random(5, 10) / 10) -- Muda de cor em tempos aleatórios
            end
        end)
    end
end
# Hi, I'm Roblox Atari 👋

> Building, playing, and exploring—one idea at a time.

I’m interested in **Roblox** and **Atari**, where creative worlds and classic gaming meet. This profile is a home for the projects I’m working on, the things I’m learning, and the ideas I’m excited to share.

## 🎮 What I’m into

- 🧱 Creating and exploring experiences on Roblox
- 🕹️ Classic Atari games and gaming history
- 💡 Learning new skills and turning ideas into projects

## 🚀 Featured projects

*More to come! Check back soon to see what I’m building.*

<!-- Add your best projects here:
- [Project name](link) — A short description of what it does and what you contributed.
-->

## 🌱 Currently exploring

Roblox, Atari, and whatever sparks my curiosity next.

## 🤝 Say hello

<!-- Add your preferred social or community links here. -->
- GitHub: [@your-username](https://github.com/your-username)
local Players = game:GetService("Players")
local ContextActionService = game:GetService("ContextActionService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local rootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- Trava os eixos para o jogo parecer 2D (estilo Atari lateral)
humanoid.WalkSpeed = 0 
rootPart.Anchored = true

-- Configurações do "Pixel" do Atari (Tamanho do passo no grid)
local GRID_SIZE = 4
local MOVE_DELAY = 0.15
local lastMove = 0

local function moverAtari(actionName, inputState, inputObject)
	if inputState ~= Enum.UserInputState.Begin then return end
	
	-- Evita comandos rápidos demais (Simula o processador lento do Atari)
	if tick() - lastMove < MOVE_DELAY then return end
	lastMove = tick()
	
	local currentPos = rootPart.Position
	
	if actionName == "IrEsquerda" then
		rootPart.Position = currentPos + Vector3.new(-GRID_SIZE, 0, 0)
	elseif actionName == "IrDireita" then
		rootPart.Position = currentPos + Vector3.new(GRID_SIZE, 0, 0)
	elseif actionName == "PularAtari" then
		-- Pulo estilo "teleporte" rápido do Atari
		rootPart.Position = currentPos + Vector3.new(0, GRID_SIZE * 2, 0)
		task.wait(0.2)
		rootPart.Position = rootPart.Position + Vector3.new(0, -GRID_SIZE * 2, 0)
	end
end

-- Vincula os botões do teclado aos movimentos travados
ContextActionService:BindAction("IrEsquerda", moverAtari, false, Enum.KeyCode.A, Enum.KeyCode.Left)
ContextActionService:BindAction("IrDireita", moverAtari, false, Enum.KeyCode.D, Enum.KeyCode.Right)
ContextActionService:BindAction("PularAtari", moverAtari, false, Enum.KeyCode.W, Enum.KeyCode.Space)
local Players = game:GetService("Players")

Players.PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function(character)
		local humanoid = character:WaitForChild("Humanoid")
		
		-- Velocidade e pulo originais do Roblox clássico
		humanoid.WalkSpeed = 16
		humanoid.JumpPower = 50
		humanoid.UseJumpPower = true
		
		-- Desativa as animações modernas (R15) forçando o estilo bloco (R6)
		-- Nota: Certifique-se de configurar o jogo nas opções do Studio para rodar apenas em R6!
		
		-- Remove o "NameTag" moderno e deixa apenas o nome simples sobre a cabeça
		humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.Viewer
		humanoid.HealthDisplayType = Enum.HumanoidHealthDisplayType.DisplayWhenDamaged
	end)
end)
local DataStoreService = game:GetService("DataStoreService")
local Players = game:GetService("Players")

-- Cria ou acessa o banco de dados do seu jogo para 2026
local meuJogoDataStore = DataStoreService:GetDataStore("SalvarPontos_v2026")

local function onPlayerAdded(player)
	local leaderstats = Instance.new("Folder")
	leaderstats.Name = "leaderstats"
	leaderstats.Parent = player

	local pontos = Instance.new("IntValue")
	pontos.Name = "Pontos"
	pontos.Value = 0
	pontos.Parent = leaderstats

	-- Carrega os dados salvos do jogador
	local playerUserId = "Player_" .. player.UserId
	local sucesso, dadosSalvos = pcall(function()
		return meuJogoDataStore:GetAsync(playerUserId)
	end)

	if sucesso and dadosSalvos then
		pontos.Value = dadosSalvos
		print("Dados de " .. player.Name .. " carregados com sucesso!")
	else
		print("Nenhum dado antigo encontrado para " .. player.Name .. ". Começando do zero.")
	end
end

local function onPlayerRemoving(player)
	-- Salva os dados quando o jogador sai do servidor
	local playerUserId = "Player_" .. player.UserId
	local pontosParaSalvar = player.leaderstats.Pontos.Value

	local sucesso, erro = pcall(function()
		meuJogoDataStore:SetAsync(playerUserId, pontosParaSalvar)
	end)

	if sucesso then
		print("Dados de " .. player.Name .. " salvos com sucesso!")
	else
		warn("Erro ao salvar dados: " .. tostring(erro))
	end
end

-- Conecta os eventos
Players.PlayerAdded:Connect(onPlayerAdded)
Players.PlayerRemoving:Connect(onPlayerRemoving)

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- 1. Cria a ScreenGui (A base de toda a interface)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "InterfaceJogo2026"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- 2. Cria o Frame de fundo do Placar
local placarFrame = Instance.new("Frame")
placarFrame.Size = UDim2.new(0, 200, 0, 60)
placarFrame.Position = UDim2.new(0.02, 0, 0.02, 0) -- Canto superior esquerdo da tela
placarFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25) -- Fundo escuro moderno
placarFrame.BorderSizePixel = 0
placarFrame.Parent = screenGui

-- Arredonda os cantos do design (Padrão visual atual)
local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 12)
uiCorner.Parent = placarFrame

-- Adiciona uma borda brilhante discreta
local uiStroke = Instance.new("UIStroke")
uiStroke.Color = Color3.fromRGB(0, 170, 255)
uiStroke.Thickness = 2
uiStroke.Parent = placarFrame

-- 3. Cria o Texto que mostra os Pontos
local textoPontos = Instance.new("TextLabel")
textoPontos.Size = UDim2.new(1, 0, 1, 0)
textoPontos.BackgroundTransparency = 1
textoPontos.Text = "PONTOS: 0"
textoPontos.TextColor3 = Color3.fromRGB(255, 255, 255)
textoPontos.Font = Enum.Font.GothamBold -- Fonte moderna
textoPontos.TextSize = 18
textoPontos.Parent = placarFrame

-- 4. Função para atualizar o texto na tela com efeito visual de "Pulso"
local leaderstats = player:WaitForChild("leaderstats", 10)
if leaderstats then
	local pontosValue = leaderstats:WaitForChild("Pontos")
	
	local function atualizarVisual(novoValor)
		textoPontos.Text = "PONTOS: " .. tostring(novoValor)
		
		-- Efeito de piscar verde rapidamente ao ganhar pontos (Tendência 2026)
		local tweenInfo = TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out, 0, true)
		local tween = TweenService:Create(textoPontos, tweenInfo, {TextColor3 = Color3.fromRGB(0, 255, 100)})
		tween:Play()
	end
	
	-- Atualiza sempre que o valor mudar no servidor
	pontosValue.Changed:Connect(atualizarVisual)
	atualizarVisual(pontosValue.Value) -- Atualização inicial
end
local Workspace = game:GetService("Workspace")

-- Configurações do tamanho do mapa que vai aparecer
local LARGURA_MAPA = 10
local COMPRIMENTO_MAPA = 10
local TAMANHO_BLOCO = 6

local function gerarJogo()
	-- Cria uma pasta para organizar o mapa gerado
	local mapaPasta = Instance.new("Folder")
	mapaPasta.Name = "MapaGerado2026"
	mapaPasta.Parent = Workspace

	-- Loop que constrói o chão bloco por bloco automaticamente
	for x = 1, LARGURA_MAPA do
		for z = 1, COMPRIMENTO_MAPA do
			local bloco = Instance.new("Part")
			bloco.Shape = Enum.PartType.Block
			bloco.Size = Vector3.new(TAMANHO_BLOCO, 1, TAMANHO_BLOCO)
			
			-- Calcula a posição de cada pedaço do jogo
			bloco.Position = Vector3.new(x * TAMANHO_BLOCO, 0, z * TAMANHO_BLOCO)
			bloco.Anchored = true
			bloco.Material = Enum.Material.SmoothPlastic
			
			-- Cria um padrão de cores xadrez moderno (azul e cinza escuro)
			if (x + z) % 2 == 0 then
				bloco.Color = Color3.fromRGB(30, 30, 35)
			else
				bloco.Color = Color3.fromRGB(0, 120, 255)
			end
			
			bloco.Parent = mapaPasta
		end
	end
	print("🎮 O mapa do jogo apareceu e foi gerado com sucesso!")
end

-- Executa a geração assim que o jogo liga
gerarJogo()
