local limiteAlcance = 150 -- Distância inicial em Studs (ajustável de 0 a 1000)
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local Cam = game.Workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local MasterAimbot = false 
local AimbotAtivado = false 
local MenuVisivel = true
local AlvoAtual = nil
local HighlightsAtivado = false -- Começa estritamente DESATIVADO por padrão

-- Tabelas para o sistema de troca de cores do ESP 3D
local PaletaCores = {
    Color3.fromRGB(255, 0, 0),   -- Vermelho
    Color3.fromRGB(0, 255, 0),   -- Verde
    Color3.fromRGB(0, 100, 255), -- Azul
    Color3.fromRGB(255, 255, 0), -- Amarelo
    Color3.fromRGB(180, 0, 255)  -- Roxo
}
local NomesCores = {"VERMELHO", "VERDE", "AZUL", "AMARELO", "ROXO"}

local IndiceCorInimigo = 1 -- Padrão: Vermelho
local IndiceCorAliado = 2  -- Padrão: Verde

local ListaPartes = {"Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg"}
local IndiceParteVisual = 1
local ParteAlvo = ListaPartes[IndiceParteVisual] 

local MapeamentoMembros = {
    ["Head"] = {"Head"},
    ["Torso"] = {"HumanoidRootPart", "Torso", "UpperTorso"},
    ["Left Arm"] = {"LeftArm", "LeftUpperArm", "LeftHand"},
    ["Right Arm"] = {"RightArm", "RightUpperArm", "RightHand"},
    ["Left Leg"] = {"LeftLeg", "LeftUpperLeg", "LeftFoot"},
    ["Right Leg"] = {"RightLeg", "RightUpperLeg", "RightFoot"}
}

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "L_Lawliet_Hub_Xeno_V10"
ScreenGui.ResetOnSpawn = false

local successGui, _ = pcall(function() ScreenGui.Parent = CoreGui end)
if not successGui or not ScreenGui.Parent then 
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui", 10) 
end

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 450, 0, 420)
MainFrame.Position = UDim2.new(0.5, -225, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true 
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local BackgroundImage = Instance.new("ImageLabel")
BackgroundImage.Name = "BackgroundImage"
BackgroundImage.Size = UDim2.new(1, 0, 1, 0)
BackgroundImage.Image = "rbxassetid://72881473632557" 
BackgroundImage.ImageTransparency = 0.45 
BackgroundImage.BackgroundTransparency = 1
BackgroundImage.ScaleType = Enum.ScaleType.Crop
BackgroundImage.Parent = MainFrame

local ImgCorner = Instance.new("UICorner")
ImgCorner.CornerRadius = UDim.new(0, 10)
ImgCorner.Parent = BackgroundImage

local TitleText = Instance.new("TextLabel")
TitleText.Size = UDim2.new(1, 0, 0, 40)
TitleText.BackgroundTransparency = 1
TitleText.Text = " L LAWLIET - PRIVATE HUB"
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Font = Enum.Font.SpecialElite 
TitleText.TextSize = 18
TitleText.Parent = MainFrame

local AimbotBtn = Instance.new("TextButton")
AimbotBtn.Size = UDim2.new(0, 130, 0, 45)
AimbotBtn.Position = UDim2.new(0, 15, 0, 80)
AimbotBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
AimbotBtn.BackgroundTransparency = 0.2
AimbotBtn.Text = "SISTEMA: OFF"
AimbotBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
AimbotBtn.Font = Enum.Font.SourceSansBold
AimbotBtn.TextSize = 13
AimbotBtn.Parent = MainFrame

local BtnCorner1 = Instance.new("UICorner")
BtnCorner1.CornerRadius = UDim.new(0, 6)
BtnCorner1.Parent = AimbotBtn

local MembroBtn = Instance.new("TextButton")
MembroBtn.Size = UDim2.new(0, 140, 0, 45)
MembroBtn.Position = UDim2.new(0, 155, 0, 80)
MembroBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MembroBtn.BackgroundTransparency = 0.2
MembroBtn.Text = "OPÇÃO: HEAD"
MembroBtn.TextColor3 = Color3.fromRGB(230, 230, 230)
MembroBtn.Font = Enum.Font.SourceSansBold
MembroBtn.TextSize = 13
MembroBtn.Parent = MainFrame

local BtnCorner2 = Instance.new("UICorner")
BtnCorner2.CornerRadius = UDim.new(0, 6)
BtnCorner2.Parent = MembroBtn

local AplicarBtn = Instance.new("TextButton")
AplicarBtn.Size = UDim2.new(0, 125, 0, 45)
AplicarBtn.Position = UDim2.new(0, 305, 0, 80)
AplicarBtn.BackgroundColor3 = Color3.fromRGB(30, 50, 30)
AplicarBtn.BackgroundTransparency = 0.2
AplicarBtn.Text = "SELECIONAR"
AplicarBtn.TextColor3 = Color3.fromRGB(150, 255, 150)
AplicarBtn.Font = Enum.Font.SourceSansBold
AplicarBtn.TextSize = 13
AplicarBtn.Parent = MainFrame

local BtnCorner3 = Instance.new("UICorner")
BtnCorner3.CornerRadius = UDim.new(0, 6)
BtnCorner3.Parent = AplicarBtn

local EspBtn = Instance.new("TextButton")
EspBtn.Size = UDim2.new(0, 420, 0, 40)
EspBtn.Position = UDim2.new(0, 15, 0, 135)
EspBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
EspBtn.BackgroundTransparency = 0.2
EspBtn.Text = "ESP CONTORNO: OFF" 
EspBtn.TextColor3 = Color3.fromRGB(255, 100, 100) 
EspBtn.Font = Enum.Font.SourceSansBold
EspBtn.TextSize = 14
EspBtn.Parent = MainFrame

local EspCorner = Instance.new("UICorner")
EspCorner.CornerRadius = UDim.new(0, 6)
EspCorner.Parent = EspBtn

local CorInimigoBtn = Instance.new("TextButton")
CorInimigoBtn.Size = UDim2.new(0, 205, 0, 40)
CorInimigoBtn.Position = UDim2.new(0, 15, 0, 185)
CorInimigoBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CorInimigoBtn.BackgroundTransparency = 0.2
CorInimigoBtn.Text = "COR INIMIGO: VERMELHO"
CorInimigoBtn.TextColor3 = PaletaCores[IndiceCorInimigo]
CorInimigoBtn.Font = Enum.Font.SourceSansBold
CorInimigoBtn.TextSize = 12
CorInimigoBtn.Parent = MainFrame

local CorInimigoCorner = Instance.new("UICorner")
CorInimigoCorner.CornerRadius = UDim.new(0, 6)
CorInimigoCorner.Parent = CorInimigoBtn

local CorAliadoBtn = Instance.new("TextButton")
CorAliadoBtn.Size = UDim2.new(0, 205, 0, 40)
CorAliadoBtn.Position = UDim2.new(0, 230, 0, 185)
CorAliadoBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CorAliadoBtn.BackgroundTransparency = 0.2
CorAliadoBtn.Text = "COR ALIADO: VERDE"
CorAliadoBtn.TextColor3 = PaletaCores[IndiceCorAliado]
CorAliadoBtn.Font = Enum.Font.SourceSansBold
CorAliadoBtn.TextSize = 12
CorAliadoBtn.Parent = MainFrame

local CorAliadoCorner = Instance.new("UICorner")
CorAliadoCorner.CornerRadius = UDim.new(0, 6)
CorAliadoCorner.Parent = CorAliadoBtn

local SliderLabel = Instance.new("TextLabel")
SliderLabel.Size = UDim2.new(0, 420, 0, 25)
SliderLabel.Position = UDim2.new(0, 15, 0, 240)
SliderLabel.BackgroundTransparency = 1
SliderLabel.Text = "LIMITE DE ALCANCE: " .. limiteAlcance .. " STUDS"
SliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
SliderLabel.Font = Enum.Font.SourceSansBold
SliderLabel.TextSize = 14
SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
SliderLabel.Parent = MainFrame

local SliderBG = Instance.new("Frame")
SliderBG.Size = UDim2.new(0, 420, 0, 10)
SliderBG.Position = UDim2.new(0, 15, 0, 270)
SliderBG.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SliderBG.BorderSizePixel = 0
SliderBG.Parent = MainFrame

local SliderBGCorner = Instance.new("UICorner")
SliderBGCorner.CornerRadius = UDim.new(0, 4)
SliderBGCorner.Parent = SliderBG

local SliderFill = Instance.new("Frame")
SliderFill.Size = UDim2.new(limiteAlcance / 1000, 0, 1, 0)
SliderFill.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
SliderFill.BorderSizePixel = 0
SliderFill.Parent = SliderBG

local SliderFillCorner = Instance.new("UICorner")
SliderFillCorner.CornerRadius = UDim.new(0, 4)
SliderFillCorner.Parent = SliderFill

local SliderBtn = Instance.new("TextButton")
SliderBtn.Size = UDim2.new(0, 16, 0, 16)
SliderBtn.Position = UDim2.new(limiteAlcance / 1000, -8, 0.5, -8)
SliderBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
SliderBtn.Text = ""
SliderBtn.Parent = SliderBG

local SliderBtnCorner = Instance.new("UICorner")
SliderBtnCorner.CornerRadius = UDim.new(1, 0)
SliderBtnCorner.Parent = SliderBtn

local dragging = false

local function updateSlider()
    local location = UserInputService:GetMouseLocation()
    local relativeX = location.X - SliderBG.AbsolutePosition.X
    local percentage = math.clamp(relativeX / SliderBG.AbsoluteSize.X, 0, 1)
    
    limiteAlcance = math.floor(percentage * 1000)
    
    SliderBtn.Position = UDim2.new(percentage, -8, 0.5, -8)
    SliderFill.Size = UDim2.new(percentage, 0, 1, 0)
    SliderLabel.Text = "LIMITE DE ALCANCE: " .. limiteAlcance .. " STUDS"
end

SliderBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        updateSlider()
    end
end)

local FooterText = Instance.new("TextLabel")
FooterText.Size = UDim2.new(1, -20, 0, 30)
FooterText.Position = UDim2.new(0, 10, 1, -35)
FooterText.BackgroundTransparency = 1
FooterText.Text = "Aperte [F] para travar | [Ctrl Esq] Ocultar Hub | [Delete] Desinstalar"
FooterText.TextColor3 = Color3.fromRGB(200, 200, 200)
FooterText.Font = Enum.Font.SourceSansItalic
FooterText.TextSize = 13
FooterText.Parent = MainFrame

-- ====================================================================
-- SISTEMA ESP: HITBOX 3D REAL (SELECTIONBOX EM CORE_GUI)
-- ====================================================================
local CaixaFolder = Instance.new("Folder")
CaixaFolder.Name = "HubHitboxes3D"
pcall(function() CaixaFolder.Parent = CoreGui end)
if not CaixaFolder.Parent then CaixaFolder.Parent = LocalPlayer:WaitForChild("PlayerGui") end

local function gerenciarHitboxJogador(jogador)
if jogador == LocalPlayer then return end
local char = jogador.Character
local nomeCaixa = "3DBox_" .. jogador.Name
local hitboxExistente = CaixaFolder:FindFirstChild(nomeCaixa)
-- Se o ESP estiver em OFF ou limite for 0, limpa imediatamente
if not HighlightsAtivado or limiteAlcance <= 0 or not char or not char:FindFirstChild("HumanoidRootPart") then
if hitboxExistente then hitboxExistente:Destroy() end
return
end
local rootPart = char.HumanoidRootPart
local humanoid = char:FindFirstChildOfClass("Humanoid")
local meuRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
if rootPart and humanoid and humanoid.Health > 0 and meuRoot then
local distanciaReal3D = (meuRoot.Position - rootPart.Position).Magnitude
-- Filtro pelo limite de distância física do Slider
if distanciaReal3D <= limiteAlcance then
if not hitboxExistente then
hitboxExistente = Instance.new("SelectionBox")
hitboxExistente.Name = nomeCaixa
hitboxExistente.LineThickness = 0.05
hitboxExistente.SurfaceTransparency = 0.85 -- Dá um preenchimento 3D suave dentro da caixa
hitboxExistente.Parent = CaixaFolder
end
-- Alvos da caixa e cores baseadas em equipes com pcall de segurança
hitboxExistente.Adornee = char -- Contorna o modelo inteiro do personagem em 3D
local sucessoTeam, resultadoTeam = pcall(function()
return jogador.Team and LocalPlayer.Team and jogador.Team == LocalPlayer.Team
end)
if sucessoTeam and resultadoTeam then
hitboxExistente.Color3 = PaletaCores[IndiceCorAliado]
hitboxExistente.SurfaceColor3 = PaletaCores[IndiceCorAliado]
else
hitboxExistente.Color3 = PaletaCores[IndiceCorInimigo]
hitboxExistente.SurfaceColor3 = PaletaCores[IndiceCorInimigo]
end
hitboxExistente.Visible = true
else
if hitboxExistente then hitboxExistente:Destroy() end
end
else
if hitboxExistente then hitboxExistente:Destroy() end
end
end
local function gerenciarConexoesJogador(jogador)
if jogador == LocalPlayer then return end
if jogador.Character then gerenciarHitboxJogador(jogador) end
jogador.CharacterAdded:Connect(function(char)
char:WaitForChild("HumanoidRootPart", 5)
task.wait(0.1)
gerenciarHitboxJogador(jogador)
end)
jogador:GetPropertyChangedSignal("Team"):Connect(function()
gerenciarHitboxJogador(jogador)
end)
end
for _, jogador in ipairs(Players:GetPlayers()) do
gerenciarConexoesJogador(jogador)
end
Players.PlayerAdded:Connect(gerenciarConexoesJogador)
local function limparTodasHitboxes()
CaixaFolder:ClearAllChildren()
end
CorInimigoBtn.MouseButton1Click:Connect(function()
IndiceCorInimigo = IndiceCorInimigo + 1
if IndiceCorInimigo > #PaletaCores then IndiceCorInimigo = 1 end
CorInimigoBtn.Text = "COR INIMIGO: " .. NomesCores[IndiceCorInimigo]
CorInimigoBtn.TextColor3 = PaletaCores[IndiceCorInimigo]
end)
CorAliadoBtn.MouseButton1Click:Connect(function()
IndiceCorAliado = IndiceCorAliado + 1
if IndiceCorAliado > #PaletaCores then IndiceCorAliado = 1 end
CorAliadoBtn.Text = "COR ALIADO: " .. NomesCores[IndiceCorAliado]
CorAliadoBtn.TextColor3 = PaletaCores[IndiceCorAliado]
end)
EspBtn.MouseButton1Click:Connect(function()
HighlightsAtivado = not HighlightsAtivado
if HighlightsAtivado then
EspBtn.Text = "ESP CONTORNO: ON"
EspBtn.TextColor3 = Color3.fromRGB(100, 255, 100)
else
EspBtn.Text = "ESP CONTORNO: OFF"
EspBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
limparTodasHitboxes()
end
end)
-- ====================================================================
local function atualizarMenu()
if not MasterAimbot then
AimbotBtn.Text = "SISTEMA: OFF"
AimbotBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
elseif MasterAimbot and not AimbotAtivado then
AimbotBtn.Text = "PRONTO [F]"
AimbotBtn.TextColor3 = Color3.fromRGB(255, 255, 100)
else
AimbotBtn.Text = AlvoAtual and ("ALVO: " .. string.upper(AlvoAtual.Name)) or "PROCURANDO..."
AimbotBtn.TextColor3 = AlvoAtual and Color3.fromRGB(100, 255, 255) or Color3.fromRGB(100, 255, 100)
end
MembroBtn.Text = "OPÇÃO: " .. string.upper(ListaPartes[IndiceParteVisual])
if ListaPartes[IndiceParteVisual] == ParteAlvo then
AplicarBtn.Text = "ATIVADO"
AplicarBtn.BackgroundColor3 = Color3.fromRGB(20, 40, 20)
AplicarBtn.TextColor3 = Color3.fromRGB(100, 200, 100)
else
AplicarBtn.Text = "SELECIONAR"
AplicarBtn.BackgroundColor3 = Color3.fromRGB(50, 40, 30)
AplicarBtn.TextColor3 = Color3.fromRGB(255, 200, 100)
end
end
AimbotBtn.MouseButton1Click:Connect(function()
MasterAimbot = not MasterAimbot
if not MasterAimbot then AimbotAtivado = false; AlvoAtual = nil end
atualizarMenu()
end)
MembroBtn.MouseButton1Click:Connect(function()
IndiceParteVisual = IndiceParteVisual + 1
if IndiceParteVisual > #ListaPartes then IndiceParteVisual = 1 end
atualizarMenu()
end)
AplicarBtn.MouseButton1Click:Connect(function()
ParteAlvo = ListaPartes[IndiceParteVisual]
AlvoAtual = nil
atualizarMenu()
end)
UserInputService.InputBegan:Connect(function(input, processed)
if UserInputService:GetFocusedTextBox() then return end
if input.KeyCode == Enum.KeyCode.F and MasterAimbot then
AimbotAtivado = not AimbotAtivado
if not AimbotAtivado then AlvoAtual = nil end
atualizarMenu()
elseif input.KeyCode == Enum.KeyCode.LeftControl then
MenuVisivel = not MenuVisivel
MainFrame.Visible = MenuVisivel
elseif input.KeyCode == Enum.KeyCode.Delete then
MasterAimbot = false; AimbotAtivado = false
limparTodasHitboxes()
CaixaFolder:Destroy()
ScreenGui:Destroy()
end
end)
local function pegarMembroReal(character, nomeMembroFiltro)
if not character or not MapeamentoMembros[nomeMembroFiltro] then return nil end
for _, nomePeca in ipairs(MapeamentoMembros[nomeMembroFiltro]) do
local pecaEncontrada = character:FindFirstChild(nomePeca)
if pecaEncontrada then return pecaEncontrada end
end
return nil
end
RunService.RenderStepped:Connect(function()
-- Garante a atualização de cor, posição e tamanho de todas as caixas tridimensionais
for _, jogador in ipairs(Players:GetPlayers()) do
gerenciarHitboxJogador(jogador)
end
if not MasterAimbot then return end
if not AimbotAtivado then return end
if limiteAlcance <= 0 then return end
local part = (AlvoAtual and AlvoAtual.Character and AlvoAtual.Character:FindFirstChildOfClass("Humanoid") and AlvoAtual.Character:FindFirstChildOfClass("Humanoid").Health > 0) and pegarMembroReal(AlvoAtual.Character, ParteAlvo) or nil
if part and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
local distanciaReal3D = (LocalPlayer.Character.HumanoidRootPart.Position - part.Position).Magnitude
local sucessoTeam, resultadoTeam = pcall(function()
return AlvoAtual.Team and LocalPlayer.Team and AlvoAtual.Team == LocalPlayer.Team
end)
local mesmoTime = sucessoTeam and resultadoTeam
if distanciaReal3D > limiteAlcance or mesmoTime then
part = nil
AlvoAtual = nil
end
end
if part then
Cam.CFrame = CFrame.new(Cam.CFrame.Position, part.Position)
else
AlvoAtual = nil
local menorDistanciaMouse = math.huge
for _, jogador in ipairs(Players:GetPlayers()) do
local sucessoTeam, resultadoTeam = pcall(function()
return jogador.Team and LocalPlayer.Team and jogador.Team == LocalPlayer.Team
end)
local mesmoTime = successTeam and resultadoTeam
if jogador ~= LocalPlayer and not mesmoTime and jogador.Character and jogador.Character:FindFirstChildOfClass("Humanoid") and jogador.Character:FindFirstChildOfClass("Humanoid").Health > 0 then
local membroValido = pegarMembroReal(jogador.Character, ParteAlvo)
if membroValido and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
local distanciaReal3D = (LocalPlayer.Character.HumanoidRootPart.Position - membroValido.Position).Magnitude
if distanciaReal3D <= limiteAlcance then
local posicaoTela, visivel = Cam:WorldToViewportPoint(membroValido.Position)
if visivel then
local distanciaMouse = (Vector2.new(posicaoTela.X, posicaoTela.Y) - (Cam.ViewportSize / 2)).Magnitude
if distanciaMouse < menorDistanciaMouse then
menorDistanciaMouse = distanciaMouse
AlvoAtual = jogador
end
end
end
end
end
end
end
end)
