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
