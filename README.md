-- Criar ScreenGui
screenGui local = Instância.new("ScreenGui")
screenGui.Nome = "StyleGui"
screenGui.Parent = jogo.Jogadores.LocalPlayer:WaitForChild("PlayerGui")

-- Função para habilitar o arrasto
função local enableDragging(guiElement)
    arrastando localmente, dragInput, dragStart, startPos

    guiElement.InputBegan:Conectar(função(entrada)
        se input.UserInputType == Enum.UserInputType.MouseButton1 ou input.UserInputType == Enum.UserInputType.Touch então
            arrastando = verdadeiro
            arrastarStart = entrada.Posição
            startPos = guiElement.Position

            entrada.Alterado:Conectar(função()
                se input.UserInputState == Enum.UserInputState.End então
                    arrastando = falso
                fim
            fim)
        fim
    fim)

    guiElement.InputChanged:Conectar(função(entrada)
        se input.UserInputType == Enum.UserInputType.MouseMovement ou input.UserInputType == Enum.UserInputType.Touch então
            arrastarInput = entrada
        fim
    fim)

    jogo:GetService("UserInputService").InputChanged:Connect(função(entrada)
        se entrada == arrastarEntrada e arrastando então
            delta local = input.Posição - dragStart
            guiElement.Posição = UDim2.new(
                startPos.X.Escala,
                startPos.X.Offset + delta.X,
                startPos.Y.Escala,
                startPos.Y.Offset + delta.Y
            )
        fim
    fim)
fim

-- Criar quadro principal
quadro local = Instance.new("Quadro")
frame.Tamanho = UDim2.novo(0, 300, 0, 400)
frame.Posição = UDim2.new(0,5, -150, 0,5, -200)
quadro.BackgroundColor3 = Cor3.fromRGB(40, 40, 40)
quadro.BorderSizePixel = 0
quadro.Parent = screenGui

-- Adicione cantos arredondados ao quadro
frameCorner local = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.novo(0, 10)
frameCorner.Parent = quadro

-- Adicionar barra de arrasto ao quadro
local dragBar = Instance.new("Quadro")
dragBar.Size = UDim2.new(1, 0, 0, 30)
dragBar.Posição = UDim2.new(0, 0, 0, 0)
dragBar.BackgroundColor3 = Cor3.fromRGB(50, 50, 50)
arrastarBarra.TamanhoDaBordaPixel = 0
dragBar.Parent = quadro

dragBarText local = Instância.new("TextLabel")
dragBarText.Size = UDim2.new(1, 0, 1, 0)
dragBarText.Text = "Arraste-me"
dragBarText.Font = Enum.Fonte.GothamBold
dragBarText.TextSize = 18
dragBarText.TextColor3 = Color3.fromRGB(255, 255, 255)
dragBarText.BackgroundTransparency = 1
dragBarText.Parent = arrastarBarra

-- Habilitar arrastar para o quadro usando a barra de arrasto
enableDragging(quadro)

-- Crie botões para cada estilo
estilos locais = {"Sae", "Rin", "Chigiri", "Shidou", "Nagi", "Isagi", "Gagamaru"}
deslocamento local y = 40

local desiredStyle = nil -- Padrão para nil, significando que nenhum estilo é selecionado
local isRolling = false -- Evita múltiplas rolagens ao mesmo tempo

para _, styleName em ipairs(estilos) faça
    estilo localButton = Instance.new("TextButton")
    estiloButton.Size = UDim2.new(0, 280, 0, 40)
    styleButton.Position = UDim2.new(0, 10, 0, yOffset)
    styleButton.Text = nomedoestilo
    styleButton.Font = Enum.Font.GothamBold
    styleButton.TextSize = 18
    styleButton.TextColor3 = Cor3.fromRGB(255, 255, 255)
    styleButton.BackgroundColor3 = Cor3.fromRGB(50, 150, 50)
    styleButton.BorderSizePixel = 0
    styleButton.Parent = quadro

    -- Adicione cantos arredondados ao botão
    botão localCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 10)
    buttonCorner.Parent = estiloBotão

    -- Defina a posição para o próximo botão
    yDeslocamento = yDeslocamento + 50

    -- Funcionalidade de clique de botão
    styleButton.MouseButton1Click:Conectar(função()
        se está rolando então
            print("Já rolando. Por favor aguarde.")
            retornar
        fim

        desiredStyle = nomedoestilo
        print("Tentando rolar para estilo: " .. desiredStyle)
        isRolling = verdadeiro

        tarefa.spawn(função()
            jogador local = jogo:GetService("Jogadores").LocalPlayer
            enquanto está rolando faça
                tarefa.espera(0,5)
                se player:FindFirstChild("PlayerStats") e player.PlayerStats:FindFirstChild("Style") então
                    local currentStyle = jogador.PlayerStats.Style.Value
                    se currentStyle ~= desiredStyle então
                        -- Acionar a função Spin
                        jogo:GetService("ReplicatedStorage").Packages.Knit.Services.StyleService.RE.Spin:FireServer()
                        print("Ação de rotação disparada para estilo: " .. desiredStyle)
                    outro
                        print("Estilo alterado com sucesso para: " .. desiredStyle)
                        isRolling = false -- Pare de rolar quando o estilo desejado for alcançado
                    fim
                fim
            fim
        fim)
    fim)
fim

-- Crie o botão Ocultar/Mostrar
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 100, 0, 40)
toggleButton.Position = UDim2.new(0, 10, 0, 250) -- Posicionado no lado esquerdo da tela
toggleButton.Text = "Ocultar"
toggleButton.Font = Enum.Font.GothamBold
toggleButton.TextSize = 18
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.BackgroundColor3 = Cor3.fromRGB(150, 50, 50)
toggleButton.BorderSizePixel = 0
toggleButton.Parent = screenGui -- Anexa o botão ao ScreenGui, não ao Frame

-- Adicione cantos arredondados ao botão
local toggleButtonCorner = Instance.new("UICorner")
toggleButtonCorner.CornerRadius = UDim.novo(0, 10)
toggleButtonCorner.Parent = botão de alternância

-- Funcionalidade do botão de alternância: ocultar/exibir a IU
local isVisible = verdadeiro
toggleButton.MouseButton1Click:Conectar(função()
    isVisible = não éVisível
    frame.Visible = isVisible -- Alterna apenas a visibilidade do quadro, não a tela inteiraGui
    toggleButton.Text = isVisible e "Hide" ou "Open" -- Altera o texto do botão para "Hide" quando visível, "Open" quando oculto
    toggleButton.BackgroundColor3 = isVisible e Color3.fromRGB(150, 50, 50) ou Color3.fromRGB(50, 150, 50) -- Ajustar a cor do botão
fim)
