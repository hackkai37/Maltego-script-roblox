-- ============================================================================
-- MALTEGO v1.3 (DELTA MOBILE) - TERMINAL OSINT DE PLAYERS
-- ============================================================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local localPlayer = Players.LocalPlayer
local playerGui = localPlayer:WaitForChild("PlayerGui")

-- ============================================================
-- 0. LIMPAR GUI ANTIGA
-- ============================================================
local guiAntiga = playerGui:FindFirstChild("Maltego")
if guiAntiga then guiAntiga:Destroy() end

-- ============================================================
-- 1. CORES
-- ============================================================
local VERDE = Color3.fromRGB(0, 255, 150)
local VERDE_CLARO = Color3.fromRGB(150, 255, 200)
local VERDE_ESCURO = Color3.fromRGB(0, 200, 110)
local FUNDO = Color3.fromRGB(0, 8, 3)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Maltego"
ScreenGui.ResetOnSpawn = false
ScreenGui.DisplayOrder = 999
ScreenGui.IgnoreGuiInset = true
ScreenGui.Parent = playerGui

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 500, 0, 400)
MainFrame.Position = UDim2.new(0.05, 0, 0.1, 0)
MainFrame.BackgroundColor3 = FUNDO
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

local cantoMain = Instance.new("UICorner")
cantoMain.CornerRadius = UDim.new(0, 6)
cantoMain.Parent = MainFrame

local bordaMain = Instance.new("UIStroke")
bordaMain.Thickness = 2
bordaMain.Color = VERDE
bordaMain.Parent = MainFrame

local glowMain = Instance.new("UIStroke")
glowMain.Thickness = 6
glowMain.Color = VERDE
glowMain.Transparency = 0.7
glowMain.Parent = MainFrame

-- ============================================================
-- 1.1 FUNDO BINÁRIO
-- ============================================================
local FundoBinario = Instance.new("Frame")
FundoBinario.Size = UDim2.new(1, 0, 1, 0)
FundoBinario.BackgroundTransparency = 1
FundoBinario.ZIndex = 0
FundoBinario.ClipsDescendants = true
FundoBinario.Parent = MainFrame

local colunasBinarias = {}
local numColunas = 12

for i = 1, numColunas do
    local coluna = Instance.new("TextLabel")
    coluna.Size = UDim2.new(0, 40, 2, 0)
    coluna.Position = UDim2.new((i - 1) / numColunas, 0, -1, 0)
    coluna.BackgroundTransparency = 1
    coluna.TextColor3 = VERDE_ESCURO
    coluna.TextSize = 14
    coluna.Font = Enum.Font.Code
    coluna.TextXAlignment = Enum.TextXAlignment.Left
    coluna.TextYAlignment = Enum.TextYAlignment.Top
    coluna.TextTransparency = 0.3
    coluna.TextWrapped = true
    coluna.ZIndex = 0
    coluna.Parent = FundoBinario

    local texto = ""
    for j = 1, 60 do
        texto = texto .. tostring(math.random(0, 1)) .. "\n"
    end
    coluna.Text = texto

    table.insert(colunasBinarias, {
        label = coluna,
        velocidade = math.random(20, 60) / 1000,
    })
end

RunService.RenderStepped:Connect(function()
    for _, col in ipairs(colunasBinarias) do
        local pos = col.label.Position
        local novoY = pos.Y.Offset + col.velocidade
        if novoY > 0 then
            novoY = -400
        end
        col.label.Position = UDim2.new(pos.X.Scale, pos.X.Offset, pos.Y.Scale, novoY)
    end
end)

-- ============================================================
-- 1.2 CABEÇALHO
-- ============================================================
local Cabecalho = Instance.new("Frame")
Cabecalho.Size = UDim2.new(1, 0, 0, 34)
Cabecalho.BackgroundColor3 = Color3.fromRGB(0, 40, 15)
Cabecalho.BackgroundTransparency = 0.2
Cabecalho.BorderSizePixel = 0
Cabecalho.Active = false
Cabecalho.ZIndex = 2
Cabecalho.Parent = MainFrame

local cantoCabecalho = Instance.new("UICorner")
cantoCabecalho.CornerRadius = UDim.new(0, 6)
cantoCabecalho.Parent = Cabecalho

local tampaoCabecalho = Instance.new("Frame")
tampaoCabecalho.Size = UDim2.new(1, 0, 0.5, 0)
tampaoCabecalho.Position = UDim2.new(0, 0, 0.5, 0)
tampaoCabecalho.BackgroundColor3 = Color3.fromRGB(0, 40, 15)
tampaoCabecalho.BackgroundTransparency = 0.2
tampaoCabecalho.BorderSizePixel = 0
tampaoCabecalho.ZIndex = 2
tampaoCabecalho.Parent = Cabecalho

local BotaoMinimizar = Instance.new("TextButton")
BotaoMinimizar.Size = UDim2.new(1, 0, 1, 0)
BotaoMinimizar.BackgroundTransparency = 1
BotaoMinimizar.Text = "▬  MALTEGO  v1.3  [terminal]"
BotaoMinimizar.TextColor3 = VERDE_CLARO
BotaoMinimizar.TextSize = 14
BotaoMinimizar.Font = Enum.Font.Code
BotaoMinimizar.TextXAlignment = Enum.TextXAlignment.Left
BotaoMinimizar.Active = true
BotaoMinimizar.ZIndex = 3
BotaoMinimizar.Parent = Cabecalho

local cursorVisivel = true
task.spawn(function()
    while Cabecalho.Parent do
        task.wait(0.5)
        cursorVisivel = not cursorVisivel
        if cursorVisivel then
            BotaoMinimizar.Text = "▬  MALTEGO  v1.3  [terminal] ▮"
        else
            BotaoMinimizar.Text = "▬  MALTEGO  v1.3  [terminal]"
        end
    end
end)

-- ============================================================
-- 1.3 BOLINHA
-- ============================================================
local Bolinha = Instance.new("TextButton")
Bolinha.Size = UDim2.new(0, 60, 0, 60)
Bolinha.Position = UDim2.new(0.02, 0, 0.05, 0)
Bolinha.Text = "MAL"
Bolinha.TextSize = 14
Bolinha.TextColor3 = VERDE_CLARO
Bolinha.BackgroundColor3 = Color3.fromRGB(0, 25, 8)
Bolinha.BorderSizePixel = 0
Bolinha.Visible = false
Bolinha.Active = true
Bolinha.ZIndex = 999
Bolinha.Font = Enum.Font.Code
Bolinha.Parent = ScreenGui

local cantoBolinha = Instance.new("UICorner")
cantoBolinha.CornerRadius = UDim.new(1, 0)
cantoBolinha.Parent = Bolinha

local bordaBolinha = Instance.new("UIStroke")
bordaBolinha.Thickness = 2
bordaBolinha.Color = VERDE
bordaBolinha.Parent = Bolinha

local glowBolinha = Instance.new("UIStroke")
glowBolinha.Thickness = 6
glowBolinha.Color = VERDE
glowBolinha.Transparency = 0.6
glowBolinha.Parent = Bolinha

-- ============================================================
-- 2. DRAG
-- ============================================================
local function tornarArrastavel(frame, handle)
    local dragging = false
    local dragStart, startPos

    handle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 
        or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
        end
    end)

    handle.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 
        or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement 
        or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            if delta.Magnitude > 3 then
                frame.Position = UDim2.new(
                    startPos.X.Scale, startPos.X.Offset + delta.X,
                    startPos.Y.Scale, startPos.Y.Offset + delta.Y
                )
            end
        end
    end)
end

tornarArrastavel(MainFrame, MainFrame)
tornarArrastavel(Bolinha, Bolinha)

-- ============================================================
-- 3. MINIMIZAR
-- ============================================================
local toqueInicial = nil
local arrastouAgora = false

local function pontoDentro(pos, frame)
    local p = frame.AbsolutePosition
    local s = frame.AbsoluteSize
    return pos.X >= p.X and pos.X <= p.X + s.X
       and pos.Y >= p.Y and pos.Y <= p.Y + s.Y
end

UserInputService.TouchStarted:Connect(function(input)
    toqueInicial = input.Position
    arrastouAgora = false
end)

UserInputService.TouchMoved:Connect(function(input)
    if toqueInicial then
        local delta = (input.Position - toqueInicial).Magnitude
        if delta > 10 then
            arrastouAgora = true
        end
    end
end)

UserInputService.TouchEnded:Connect(function(input)
    if not toqueInicial then return end
    local delta = (input.Position - toqueInicial).Magnitude
    toqueInicial = nil
    if delta > 10 or arrastouAgora then return end

    local pos = input.Position
    if MainFrame.Visible and pontoDentro(pos, BotaoMinimizar) then
        MainFrame.Visible = false
        Bolinha.Visible = true
        return
    end
    if Bolinha.Visible and pontoDentro(pos, Bolinha) then
        Bolinha.Visible = false
        MainFrame.Visible = true
        return
    end
end)

-- ============================================================
-- 4. BARRA DE BUSCA
-- ============================================================
local BarraTopo = Instance.new("Frame")
BarraTopo.Size = UDim2.new(1, -16, 0, 32)
BarraTopo.Position = UDim2.new(0, 8, 0, 40)
BarraTopo.BackgroundColor3 = Color3.fromRGB(0, 20, 8)
BarraTopo.BackgroundTransparency = 0.5
BarraTopo.BorderSizePixel = 0
BarraTopo.ZIndex = 2
BarraTopo.Parent = MainFrame

local cantoBarra = Instance.new("UICorner")
cantoBarra.CornerRadius = UDim.new(0, 4)
cantoBarra.Parent = BarraTopo

local bordaBarra = Instance.new("UIStroke")
bordaBarra.Thickness = 1
bordaBarra.Color = VERDE
bordaBarra.Parent = BarraTopo

local InputBusca = Instance.new("TextBox")
InputBusca.Size = UDim2.new(0.55, -4, 1, -4)
InputBusca.Position = UDim2.new(0, 2, 0, 2)
InputBusca.Text = ""
InputBusca.PlaceholderText = "> filtrar por nome..."
InputBusca.TextColor3 = VERDE_CLARO
InputBusca.BackgroundColor3 = Color3.fromRGB(0, 10, 3)
InputBusca.BackgroundTransparency = 0.4
InputBusca.BorderSizePixel = 0
InputBusca.ClearTextOnFocus = false
InputBusca.Font = Enum.Font.Code
InputBusca.TextSize = 12
InputBusca.TextXAlignment = Enum.TextXAlignment.Left
InputBusca.ZIndex = 3
InputBusca.Parent = BarraTopo

local cantoBusca = Instance.new("UICorner")
cantoBusca.CornerRadius = UDim.new(0, 4)
cantoBusca.Parent = InputBusca

local bordaBusca = Instance.new("UIStroke")
bordaBusca.Thickness = 1
bordaBusca.Color = VERDE
bordaBusca.Parent = InputBusca

local BotaoRefresh = Instance.new("TextButton")
BotaoRefresh.Size = UDim2.new(0.22, -4, 1, -4)
BotaoRefresh.Position = UDim2.new(0.55, 2, 0, 2)
BotaoRefresh.Text = "[REFRESH]"
BotaoRefresh.TextColor3 = VERDE_CLARO
BotaoRefresh.BackgroundColor3 = Color3.fromRGB(0, 40, 18)
BotaoRefresh.BackgroundTransparency = 0.3
BotaoRefresh.BorderSizePixel = 0
BotaoRefresh.Font = Enum.Font.Code
BotaoRefresh.TextSize = 11
BotaoRefresh.ZIndex = 3
BotaoRefresh.Parent = BarraTopo

local cantoRefresh = Instance.new("UICorner")
cantoRefresh.CornerRadius = UDim.new(0, 4)
cantoRefresh.Parent = BotaoRefresh

local bordaRefresh = Instance.new("UIStroke")
bordaRefresh.Thickness = 1
bordaRefresh.Color = VERDE
bordaRefresh.Parent = BotaoRefresh

local BotaoAuto = Instance.new("TextButton")
BotaoAuto.Size = UDim2.new(0.22, -4, 1, -4)
BotaoAuto.Position = UDim2.new(0.78, 0, 0, 2)
BotaoAuto.Text = "AUTO:ON"
BotaoAuto.TextColor3 = VERDE_CLARO
BotaoAuto.BackgroundColor3 = Color3.fromRGB(0, 100, 40)
BotaoAuto.BackgroundTransparency = 0.3
BotaoAuto.BorderSizePixel = 0
BotaoAuto.Font = Enum.Font.Code
BotaoAuto.TextSize = 11
BotaoAuto.ZIndex = 3
BotaoAuto.Parent = BarraTopo

local cantoAuto = Instance.new("UICorner")
cantoAuto.CornerRadius = UDim.new(0, 4)
cantoAuto.Parent = BotaoAuto

local bordaAuto = Instance.new("UIStroke")
bordaAuto.Thickness = 1
bordaAuto.Color = VERDE
bordaAuto.Parent = BotaoAuto

-- ============================================================
-- 5. LISTA DE PLAYERS (TRANSPARENTE = BRILHO)
-- ============================================================
local ListaFrame = Instance.new("Frame")
ListaFrame.Size = UDim2.new(0.4, -12, 0.55, 0)
ListaFrame.Position = UDim2.new(0, 8, 0, 78)
ListaFrame.BackgroundColor3 = Color3.fromRGB(0, 15, 5)
ListaFrame.BackgroundTransparency = 0.5
ListaFrame.BorderSizePixel = 0
ListaFrame.ZIndex = 2
ListaFrame.Parent = MainFrame

local cantoLista = Instance.new("UICorner")
cantoLista.CornerRadius = UDim.new(0, 4)
cantoLista.Parent = ListaFrame

local bordaLista = Instance.new("UIStroke")
bordaLista.Thickness = 1
bordaLista.Color = VERDE
bordaLista.Parent = ListaFrame

local LabelLista = Instance.new("TextLabel")
LabelLista.Size = UDim2.new(1, 0, 0, 22)
LabelLista.Text = "> PLAYERS:"
LabelLista.TextColor3 = VERDE_CLARO
LabelLista.BackgroundTransparency = 1
LabelLista.Font = Enum.Font.Code
LabelLista.TextSize = 12
LabelLista.TextXAlignment = Enum.TextXAlignment.Left
LabelLista.ZIndex = 3
LabelLista.Parent = ListaFrame

local Scroll = Instance.new("ScrollingFrame")
Scroll.Size = UDim2.new(1, -4, 1, -26)
Scroll.Position = UDim2.new(0, 2, 0, 24)
Scroll.BackgroundTransparency = 1
Scroll.BorderSizePixel = 0
Scroll.ScrollBarThickness = 4
Scroll.ScrollBarImageColor3 = VERDE
Scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
Scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
Scroll.ZIndex = 3
Scroll.Parent = ListaFrame

local ScrollLayout = Instance.new("UIListLayout")
ScrollLayout.Padding = UDim.new(0, 4)
ScrollLayout.Parent = Scroll

-- ============================================================
-- 6. PAINEL DE INFO (TRANSPARENTE = BRILHO)
-- ============================================================
local InfoFrame = Instance.new("Frame")
InfoFrame.Size = UDim2.new(0.6, -12, 0.55, 0)
InfoFrame.Position = UDim2.new(0.4, 4, 0, 78)
InfoFrame.BackgroundColor3 = Color3.fromRGB(0, 15, 5)
InfoFrame.BackgroundTransparency = 0.5
InfoFrame.BorderSizePixel = 0
InfoFrame.ZIndex = 2
InfoFrame.Parent = MainFrame

local cantoInfo = Instance.new("UICorner")
cantoInfo.CornerRadius = UDim.new(0, 4)
cantoInfo.Parent = InfoFrame

local bordaInfo = Instance.new("UIStroke")
bordaInfo.Thickness = 1
bordaInfo.Color = VERDE
bordaInfo.Parent = InfoFrame

local LabelInfo = Instance.new("TextLabel")
LabelInfo.Size = UDim2.new(1, 0, 0, 22)
LabelInfo.Text = "> INFO:"
LabelInfo.TextColor3 = VERDE_CLARO
LabelInfo.BackgroundTransparency = 1
LabelInfo.Font = Enum.Font.Code
LabelInfo.TextSize = 12
LabelInfo.TextXAlignment = Enum.TextXAlignment.Left
LabelInfo.ZIndex = 3
LabelInfo.Parent = InfoFrame

local ScrollInfo = Instance.new("ScrollingFrame")
ScrollInfo.Size = UDim2.new(1, -4, 1, -26)
ScrollInfo.Position = UDim2.new(0, 2, 0, 24)
ScrollInfo.BackgroundTransparency = 1
ScrollInfo.BorderSizePixel = 0
ScrollInfo.ScrollBarThickness = 4
ScrollInfo.ScrollBarImageColor3 = VERDE
ScrollInfo.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollInfo.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScrollInfo.ZIndex = 3
ScrollInfo.Parent = InfoFrame

local InfoLayout = Instance.new("UIListLayout")
InfoLayout.Padding = UDim.new(0, 4)
InfoLayout.Parent = ScrollInfo

-- ============================================================
-- 7. RODAPÉ (TRANSPARENTE = BRILHO)
-- ============================================================
local Rodape = Instance.new("Frame")
Rodape.Size = UDim2.new(1, -16, 0, 60)
Rodape.Position = UDim2.new(0, 8, 1, -68)
Rodape.BackgroundColor3 = Color3.fromRGB(0, 15, 5)
Rodape.BackgroundTransparency = 0.5
Rodape.BorderSizePixel = 0
Rodape.ZIndex = 2
Rodape.Parent = MainFrame

local cantoRodape = Instance.new("UICorner")
cantoRodape.CornerRadius = UDim.new(0, 4)
cantoRodape.Parent = Rodape

local bordaRodape = Instance.new("UIStroke")
bordaRodape.Thickness = 1
bordaRodape.Color = VERDE
bordaRodape.Parent = Rodape

local LogLabel = Instance.new("TextLabel")
LogLabel.Size = UDim2.new(1, -8, 1, -8)
LogLabel.Position = UDim2.new(0, 4, 0, 4)
LogLabel.BackgroundTransparency = 1
LogLabel.TextColor3 = VERDE_CLARO
LogLabel.TextSize = 10
LogLabel.Font = Enum.Font.Code
LogLabel.TextXAlignment = Enum.TextXAlignment.Left
LogLabel.TextYAlignment = Enum.TextYAlignment.Top
LogLabel.TextWrapped = true
LogLabel.Text = "> maltego iniciado...\n> aguardando scan..."
LogLabel.ZIndex = 3
LogLabel.Parent = Rodape

local linhasLog = {"> maltego iniciado...", "> aguardando scan..."}

local function log(msg)
    table.insert(linhasLog, "> " .. msg)
    while #linhasLog > 4 do
        table.remove(linhasLog, 1)
    end
    LogLabel.Text = table.concat(linhasLog, "\n")
end

-- ============================================================
-- 8. HELPERS
-- ============================================================
local playerSelecionado = nil
local autoRefresh = true
local ultimaInfoUserId = nil

local function corHP(hp, maxHp)
    local pct = hp / math.max(1, maxHp)
    if pct > 0.6 then
        return VERDE
    elseif pct > 0.3 then
        return Color3.fromRGB(255, 220, 0)
    else
        return Color3.fromRGB(255, 60, 60)
    end
end

local function getDistancia(player)
    local char = localPlayer.Character
    local myRoot = char and char:FindFirstChild("HumanoidRootPart")
    local otherChar = player.Character
    local otherRoot = otherChar and otherChar:FindFirstChild("HumanoidRootPart")
    if myRoot and otherRoot then
        return math.floor((myRoot.Position - otherRoot.Position).Magnitude)
    end
    return -1
end

-- ============================================================
-- 9. INFO DO PLAYER
-- ============================================================
local function limparInfo()
    for _, filho in ipairs(ScrollInfo:GetChildren()) do
        if filho:IsA("Frame") or filho:IsA("TextLabel") or filho:IsA("ImageLabel") or filho:IsA("TextButton") then
            filho:Destroy()
        end
    end
end

local function atualizarValoresInfo(player)
    local char = player.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")

    for _, filho in ipairs(ScrollInfo:GetChildren()) do
        if filho:IsA("TextLabel") and filho:GetAttribute("Chave") then
            local chave = filho:GetAttribute("Chave")
            if chave == "HP" and hum then
                filho.Text = "  HP: " .. math.floor(hum.Health) .. "/" .. math.floor(hum.MaxHealth)
                filho.TextColor3 = corHP(hum.Health, hum.MaxHealth)
            elseif chave == "Status" and hum then
                filho.Text = "  Status: " .. (hum.Health > 0 and "VIVO" or "MORTO")
                filho.TextColor3 = hum.Health > 0 and VERDE_CLARO or Color3.fromRGB(255, 60, 60)
            elseif chave == "Estado" and hum then
                filho.Text = "  Estado: " .. tostring(hum:GetState())
            elseif chave == "Posicao" and root then
                filho.Text = string.format("  Posição: (%.0f, %.0f, %.0f)", root.Position.X, root.Position.Y, root.Position.Z)
            elseif chave == "Velocidade" and root then
                filho.Text = string.format("  Velocidade: %.1f studs/s", root.Velocity.Magnitude)
            elseif chave == "Distancia" then
                local dist = getDistancia(player)
                if dist >= 0 then
                    filho.Text = "  Distância: " .. dist .. " studs"
                end
            end
        end
    end
end

local function mostrarInfo(player, forcarRecriar)
    if not forcarRecriar and ultimaInfoUserId == player.UserId then
        atualizarValoresInfo(player)
        return
    end

    ultimaInfoUserId = player.UserId
    playerSelecionado = player
    limparInfo()

    local topo = Instance.new("Frame")
    topo.Size = UDim2.new(1, 0, 0, 60)
    topo.BackgroundTransparency = 1
    topo.Parent = ScrollInfo

    local foto = Instance.new("ImageLabel")
    foto.Size = UDim2.new(0, 50, 0, 50)
    foto.Position = UDim2.new(0, 0, 0, 5)
    foto.BackgroundColor3 = Color3.fromRGB(0, 20, 8)
    foto.BackgroundTransparency = 0.3
    foto.BorderSizePixel = 0
    foto.Image = ""
    foto.Parent = topo

    local cantoFoto = Instance.new("UICorner")
    cantoFoto.CornerRadius = UDim.new(0, 4)
    cantoFoto.Parent = foto

    local bordaFoto = Instance.new("UIStroke")
    bordaFoto.Thickness = 1
    bordaFoto.Color = VERDE
    bordaFoto.Parent = foto

    task.spawn(function()
        local ok, thumb = pcall(function()
            return Players:GetUserThumbnailAsync(player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size100x100)
        end)
        if ok and thumb and foto.Parent then
            foto.Image = thumb
        end
    end)

    local nomeLabel = Instance.new("TextLabel")
    nomeLabel.Size = UDim2.new(1, -56, 0, 25)
    nomeLabel.Position = UDim2.new(0, 56, 0, 5)
    nomeLabel.Text = "@" .. player.Name
    nomeLabel.TextColor3 = VERDE_CLARO
    nomeLabel.BackgroundTransparency = 1
    nomeLabel.Font = Enum.Font.Code
    nomeLabel.TextSize = 14
    nomeLabel.TextXAlignment = Enum.TextXAlignment.Left
    nomeLabel.Parent = topo

    local displayLabel = Instance.new("TextLabel")
    displayLabel.Size = UDim2.new(1, -56, 0, 20)
    displayLabel.Position = UDim2.new(0, 56, 0, 30)
    displayLabel.Text = player.DisplayName
    displayLabel.TextColor3 = VERDE
    displayLabel.BackgroundTransparency = 1
    displayLabel.Font = Enum.Font.Code
    displayLabel.TextSize = 12
    displayLabel.TextXAlignment = Enum.TextXAlignment.Left
    displayLabel.Parent = topo

    local function addInfo(chave, valor, cor, tag)
        local linha = Instance.new("TextLabel")
        linha.Size = UDim2.new(1, 0, 0, 18)
        linha.BackgroundTransparency = 1
        linha.Text = "  " .. chave .. ": " .. tostring(valor)
        linha.TextColor3 = cor or VERDE_CLARO
        linha.Font = Enum.Font.Code
        linha.TextSize = 11
        linha.TextXAlignment = Enum.TextXAlignment.Left
        if tag then linha:SetAttribute("Chave", tag) end
        linha.Parent = ScrollInfo
        return linha
    end

    addInfo("UserId", player.UserId, VERDE_CLARO)
    addInfo("Idade da conta", player.AccountAge .. " dias", VERDE_CLARO)

    local char = player.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local root = char and char:FindFirstChild("HumanoidRootPart")

    if hum then
        addInfo("HP", math.floor(hum.Health) .. "/" .. math.floor(hum.MaxHealth), corHP(hum.Health, hum.MaxHealth), "HP")
        addInfo("Status", hum.Health > 0 and "VIVO" or "MORTO", hum.Health > 0 and VERDE_CLARO or Color3.fromRGB(255, 60, 60), "Status")
        addInfo("Estado", tostring(hum:GetState()), VERDE_CLARO, "Estado")
    else
        addInfo("Status", "SEM PERSONAGEM", Color3.fromRGB(255, 150, 50), "Status")
    end

    if root then
        addInfo("Posição", string.format("(%.0f, %.0f, %.0f)", root.Position.X, root.Position.Y, root.Position.Z), VERDE_CLARO, "Posicao")
        addInfo("Velocidade", string.format("%.1f studs/s", root.Velocity.Magnitude), VERDE_CLARO, "Velocidade")
    end

    local dist = getDistancia(player)
    if dist >= 0 then
        addInfo("Distância", dist .. " studs", VERDE_CLARO, "Distancia")
    end

    if player.Team then
        addInfo("Time", player.Team.Name, VERDE_CLARO)
    else
        addInfo("Time", "Nenhum", Color3.fromRGB(120, 120, 120))
    end

    local okAmigo, amigo = pcall(function()
        return localPlayer:IsFriendsWith(player.UserId)
    end)
    if okAmigo then
        addInfo("Amigo seu", amigo and "SIM" or "NÃO", amigo and VERDE_CLARO or Color3.fromRGB(120, 120, 120))
    end

    if char then
        local contTools = 0
        for _, t in ipairs(char:GetChildren()) do
            if t:IsA("Tool") then contTools = contTools + 1 end
        end
        addInfo("Itens equipados", contTools, VERDE_CLARO)
    end

    local bp = player:FindFirstChildOfClass("Backpack")
    if bp then
        local itens = {}
        for _, t in ipairs(bp:GetChildren()) do
            if t:IsA("Tool") then
                table.insert(itens, t.Name)
            end
        end
        if #itens > 0 then
            addInfo("Mochila", #itens .. " itens", VERDE_CLARO)
            for _, nomeItem in ipairs(itens) do
                addInfo("  -", nomeItem, VERDE)
            end
        else
            addInfo("Mochila", "vazia", Color3.fromRGB(120, 120, 120))
        end
    end

    local btnExistente = nil
    for _, filho in ipairs(ScrollInfo:GetChildren()) do
        if filho:IsA("TextButton") and filho:GetAttribute("BtnCopiar") then
            btnExistente = filho
            break
        end
    end

    if not btnExistente then
        local btnCopiar = Instance.new("TextButton")
        btnCopiar.Size = UDim2.new(1, 0, 0, 26)
        btnCopiar.Text = "[ COPIAR ID ]"
        btnCopiar.TextColor3 = VERDE_CLARO
        btnCopiar.BackgroundColor3 = Color3.fromRGB(0, 50, 22)
        btnCopiar.BackgroundTransparency = 0.4
        btnCopiar.BorderSizePixel = 0
        btnCopiar.Font = Enum.Font.Code
        btnCopiar.TextSize = 11
        btnCopiar:SetAttribute("BtnCopiar", true)
        btnCopiar.Parent = ScrollInfo

        local cantoCopiar = Instance.new("UICorner")
        cantoCopiar.CornerRadius = UDim.new(0, 4)
        cantoCopiar.Parent = btnCopiar

        local bordaCopiar = Instance.new("UIStroke")
        bordaCopiar.Thickness = 1
        bordaCopiar.Color = VERDE
        bordaCopiar.Parent = btnCopiar

        btnCopiar.MouseButton1Click:Connect(function()
            if setclipboard then
                pcall(setclipboard, tostring(player.UserId))
                log("ID copiado: " .. player.UserId)
            else
                log("setclipboard indisponivel")
            end
        end)
    end

    log("scan completo: @" .. player.Name)
end

-- ============================================================
-- 10. LISTA DE PLAYERS
-- ============================================================
local function limparLista()
    for _, filho in ipairs(Scroll:GetChildren()) do        if filho:IsA("Frame") then
            filho:Destroy()
        end
    end
end

local function criarLinhaPlayer(player)
    local Linha = Instance.new("Frame")
    Linha.Size = UDim2.new(1, -4, 0, 50)
    Linha.BackgroundColor3 = Color3.fromRGB(0, 30, 12)
    Linha.BackgroundTransparency = 0.55
    Linha.BorderSizePixel = 0
    Linha.Parent = Scroll

    local canto = Instance.new("UICorner")
    canto.CornerRadius = UDim.new(0, 4)
    canto.Parent = Linha

    local borda = Instance.new("UIStroke")
    borda.Thickness = 1
    borda.Color = VERDE
    borda.Transparency = 0.4
    borda.Parent = Linha

    local foto = Instance.new("ImageLabel")
    foto.Size = UDim2.new(0, 40, 0, 40)
    foto.Position = UDim2.new(0, 5, 0.5, -20)
    foto.BackgroundColor3 = Color3.fromRGB(0, 20, 8)
    foto.BackgroundTransparency = 0.3
    foto.BorderSizePixel = 0
    foto.Image = ""
    foto.Parent = Linha

    local cantoFoto = Instance.new("UICorner")
    cantoFoto.CornerRadius = UDim.new(0, 4)
    cantoFoto.Parent = foto

    local bordaFoto = Instance.new("UIStroke")
    bordaFoto.Thickness = 1
    bordaFoto.Color = VERDE
    bordaFoto.Parent = foto

    task.spawn(function()
        local ok, thumb = pcall(function()
            return Players:GetUserThumbnailAsync(player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size100x100)
        end)
        if ok and thumb and foto.Parent then
            foto.Image = thumb
        end
    end)

    local nome = Instance.new("TextLabel")
    nome.Size = UDim2.new(1, -55, 0, 22)
    nome.Position = UDim2.new(0, 50, 0, 5)
    nome.Text = "@" .. player.Name
    nome.TextColor3 = VERDE_CLARO
    nome.BackgroundTransparency = 1
    nome.Font = Enum.Font.Code
    nome.TextSize = 12
    nome.TextXAlignment = Enum.TextXAlignment.Left
    nome.TextTruncate = Enum.TextTruncate.AtEnd
    nome.Parent = Linha

    local sub = Instance.new("TextLabel")
    sub.Size = UDim2.new(1, -55, 0, 18)
    sub.Position = UDim2.new(0, 50, 0, 27)
    sub.BackgroundTransparency = 1
    sub.Font = Enum.Font.Code
    sub.TextSize = 10
    sub.TextXAlignment = Enum.TextXAlignment.Left
    sub.TextColor3 = VERDE
    sub.Parent = Linha

    local char = player.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local hp = hum and math.floor(hum.Health) or 0
    local maxHp = hum and math.floor(hum.MaxHealth) or 0
    local dist = getDistancia(player)
    sub.Text = string.format("HP:%d/%d | %d studs", hp, maxHp, dist >= 0 and dist or 0)

    local botao = Instance.new("TextButton")
    botao.Size = UDim2.new(1, 0, 1, 0)
    botao.BackgroundTransparency = 1
    botao.Text = ""
    botao.ZIndex = 5
    botao.Parent = Linha

    botao.MouseButton1Click:Connect(function()
        mostrarInfo(player, false)
    end)
end

local function renderLista()
    local filtro = string.lower(InputBusca.Text)
    limparLista()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= localPlayer then
            local nomeMatch = string.lower(player.Name)
            if filtro == "" or nomeMatch:find(filtro, 1, true) then
                criarLinhaPlayer(player)
            end
        end
    end
end

-- ============================================================
-- 11. EVENTOS
-- ============================================================
BotaoRefresh.MouseButton1Click:Connect(function()
    renderLista()
    log("refresh manual: " .. #Players:GetPlayers() .. " players")
end)

InputBusca:GetPropertyChangedSignal("Text"):Connect(function()
    renderLista()
end)

BotaoAuto.MouseButton1Click:Connect(function()
    autoRefresh = not autoRefresh
    if autoRefresh then
        BotaoAuto.Text = "AUTO:ON"
        BotaoAuto.BackgroundColor3 = Color3.fromRGB(0, 100, 40)
        log("auto-refresh ativado")
    else
        BotaoAuto.Text = "AUTO:OFF"
        BotaoAuto.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
        log("auto-refresh desativado")
    end
end)

task.spawn(function()
    while true do
        task.wait(2)
        if autoRefresh and MainFrame.Visible then
            renderLista()
            if playerSelecionado and playerSelecionado.Parent then
                mostrarInfo(playerSelecionado, false)
            end
        end
    end
end)

Players.PlayerAdded:Connect(function(p)
    log("entrou: @" .. p.Name)
    task.wait(1)
    renderLista()
end)

Players.PlayerRemoving:Connect(function(p)
    log("saiu: @" .. p.Name)
    if playerSelecionado == p then
        playerSelecionado = nil
        ultimaInfoUserId = nil
        limparInfo()
    end
    task.wait(0.5)
    renderLista()
end)

-- ============================================================
-- 12. INICIALIZAÇÃO
-- ============================================================
renderLista()
log("scan inicial: " .. (#Players:GetPlayers() - 1) .. " players alvo")
