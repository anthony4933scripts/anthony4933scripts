-- Obter referências aos serviços necessários
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")

-- Criar o ScreenGui que conterá a interface
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui

-- Criar o Frame principal para o layout
local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0, 400, 0, 300)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

-- Adicionar bordas arredondadas (UIcorner)
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 12)  -- Define o raio da borda
UICorner.Parent = MainFrame

-- Adicionar um texto explicativo no Frame
local Label = Instance.new("TextLabel")
Label.Parent = MainFrame
Label.Size = UDim2.new(0, 380, 0, 40)
Label.Position = UDim2.new(0.5, -190, 0, 20)
Label.Text = "Escreva seu nome para começar:"
Label.TextColor3 = Color3.fromRGB(255, 255, 255)
Label.TextSize = 18
Label.BackgroundTransparency = 1

-- Criar o TextBox para o jogador digitar seu nome
local TextBox = Instance.new("TextBox")
TextBox.Parent = MainFrame
TextBox.Size = UDim2.new(0, 300, 0, 40)
TextBox.Position = UDim2.new(0.5, -150, 0, 70)
TextBox.PlaceholderText = "Digite seu nome aqui"
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TextBox.BorderSizePixel = 2
TextBox.BorderColor3 = Color3.fromRGB(0, 170, 255)
TextBox.ClearTextOnFocus = true

-- Criar o botão "Avançar", mas ele vai estar invisível no começo
local Button = Instance.new("TextButton")
Button.Parent = MainFrame
Button.Size = UDim2.new(0, 150, 0, 50)
Button.Position = UDim2.new(0.5, -75, 0, 150)
Button.Text = "Avançar"
Button.TextColor3 = Color3.fromRGB(255, 255, 255)
Button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
Button.Visible = false  -- Começa invisível

-- Detectar quando o jogador digitar algo no TextBox
TextBox:GetPropertyChangedSignal("Text"):Connect(function()
    if TextBox.Text ~= "" then
        Button.Visible = true  -- Mostrar o botão quando o TextBox não estiver vazio
    else
        Button.Visible = false  -- Esconder o botão se o TextBox estiver vazio
    end
end)

-- Tocar a música logo ao iniciar o script
local music = Instance.new("Sound")
music.Parent = game.Workspace
music.SoundId = "rbxassetid://1848255131"  -- ID da música
music:Play()
music.Looped = false  -- Não irá tocar em loop

-- Ação quando o botão "Avançar" for clicado
Button.MouseButton1Click:Connect(function()
    local nome = TextBox.Text
    print("Nome digitado: " .. nome)  -- Apenas um log no console

    -- Criar nova tela (segunda GUI) após clicar no botão
    local NewScreenGui = Instance.new("ScreenGui")
    NewScreenGui.Parent = PlayerGui

    -- Frame para a nova tela
    local NewFrame = Instance.new("Frame")
    NewFrame.Parent = NewScreenGui
    NewFrame.Size = UDim2.new(0, 400, 0, 300)
    NewFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
    NewFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

    -- Adicionar bordas arredondadas ao novo frame
    local NewUICorner = Instance.new("UICorner")
    NewUICorner.CornerRadius = UDim.new(0, 12)  -- Define o raio da borda
    NewUICorner.Parent = NewFrame

    -- Mensagem de boas-vindas personalizada
    local NewLabel = Instance.new("TextLabel")
    NewLabel.Parent = NewFrame
    NewLabel.Size = UDim2.new(0, 380, 0, 40)
    NewLabel.Position = UDim2.new(0.5, -190, 0, 20)
    NewLabel.Text = "Bem-vindo, " .. nome .. "!"  -- Exibe o nome na segunda tela
    NewLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    NewLabel.TextSize = 18
    NewLabel.BackgroundTransparency = 1

    -- Criar o botão de fechar com animação
    local CloseButton = Instance.new("TextButton")
    CloseButton.Parent = NewFrame
    CloseButton.Size = UDim2.new(0, 50, 0, 50)
    CloseButton.Position = UDim2.new(1, -55, 0, 10)
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

    -- Fechar a tela ao clicar no botão "Fechar" com animação
    CloseButton.MouseButton1Click:Connect(function()
        local tweenClose = TweenService:Create(NewFrame, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -200, 1, 0)})
        tweenClose:Play()
        tweenClose.Completed:Connect(function()
            NewScreenGui:Destroy()  -- Fecha a segunda tela após a animação
        end)
    end)

    -- Criar o botão de minimizar com animação
    local MinimizeButton = Instance.new("TextButton")
    MinimizeButton.Parent = NewFrame
    MinimizeButton.Size = UDim2.new(0, 50, 0, 50)
    MinimizeButton.Position = UDim2.new(1, -105, 0, 10)
    MinimizeButton.Text = "-"
    MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinimizeButton.BackgroundColor3 = Color3.fromRGB(255, 165, 0)

    -- Função para minimizar o frame com animação
    local minimized = false
    MinimizeButton.MouseButton1Click:Connect(function()
        if not minimized then
            local tweenMinimize = TweenService:Create(NewFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0, 400, 0, 50)})
            tweenMinimize:Play()
            NewLabel.Visible = false  -- Esconde o label quando minimizado
            minimized = true
        else
            local tweenMaximize = TweenService:Create(NewFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0, 400, 0, 300)})
            tweenMaximize:Play()
            NewLabel.Visible = true  -- Mostra o label novamente
            minimized = false
        end
    end)

    -- Variáveis para controlar o movimento do frame
    local dragging = false
    local dragStartPos
    local startPos

    -- Funcionalidade de mover o frame
    local function startDrag(input)
        dragging = true
        dragStartPos = input.Position
        startPos = NewFrame.Position
    end

    local function updateDrag(input)
        if dragging then
            local delta = input.Position - dragStartPos
            NewFrame.Position = UDim2.new(0, startPos.X.Offset + delta.X, 0, startPos.Y.Offset + delta.Y)
        end
    end

    local function stopDrag()
        dragging = false
    end

    -- Detectar quando o mouse começa a arrastar o frame
    NewFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            startDrag(input)
        end
    end)

    -- Detectar o movimento do mouse para mover o frame
    NewFrame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            updateDrag(input)
        end
    end)

    -- Detectar quando o mouse solta o botão para parar de arrastar
    NewFrame.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            stopDrag()
        end
    end)

    -- Remover a tela anterior
    ScreenGui:Destroy()
end)
