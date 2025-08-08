-- LocalScript (StarterPlayerScripts ou StarterGui)
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "OverUnityGui"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Painel principal
local panel = Instance.new("Frame")
panel.Name = "OverUnityPanel"
panel.Size = UDim2.new(0, 320, 0, 250)
panel.Position = UDim2.new(0.5, -160, 0.5, -125)
panel.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
panel.Visible = true
panel.Parent = screenGui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "Over Unity"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20
title.Parent = panel

-- Botão fechar
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 60, 0, 25)
closeButton.Position = UDim2.new(1, -65, 0, 2)
closeButton.Text = "Fechar"
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.Parent = panel

-- Bolinha azul
local blueBall = Instance.new("TextButton")
blueBall.Size = UDim2.new(0, 50, 0, 50)
blueBall.Position = UDim2.new(0, 10, 1, -60)
blueBall.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
blueBall.Text = "+"
blueBall.TextSize = 30
blueBall.TextColor3 = Color3.fromRGB(255, 255, 255)
blueBall.Font = Enum.Font.SourceSansBold
blueBall.Visible = false
blueBall.Parent = screenGui
Instance.new("UICorner", blueBall).CornerRadius = UDim.new(1, 0)

-- Animação bolinha
local function animateBlueBall()
	while blueBall.Visible do
		local tweenUp = TweenService:Create(blueBall, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {Position = UDim2.new(0, 10, 1, -65)})
		local tweenDown = TweenService:Create(blueBall, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {Position = UDim2.new(0, 10, 1, -55)})
		tweenUp:Play() tweenUp.Completed:Wait()
		tweenDown:Play() tweenDown.Completed:Wait()
	end
end

closeButton.MouseButton1Click:Connect(function()
	panel.Visible = false
	blueBall.Visible = true
	task.spawn(animateBlueBall)
end)

blueBall.MouseButton1Click:Connect(function()
	panel.Visible = true
	blueBall.Visible = false
end)

-- Abas
local tabBar = Instance.new("Frame")
tabBar.Size = UDim2.new(1, -10, 0, 30)
tabBar.Position = UDim2.new(0, 5, 0, 35)
tabBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
tabBar.Parent = panel

local baseTab = Instance.new("TextButton")
baseTab.Size = UDim2.new(0, 80, 1, 0)
baseTab.Text = "Base"
baseTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
baseTab.TextColor3 = Color3.fromRGB(255, 255, 255)
baseTab.Parent = tabBar

local stealTab = Instance.new("TextButton")
stealTab.Size = UDim2.new(0, 80, 1, 0)
stealTab.Position = UDim2.new(0, 90, 0, 0)
stealTab.Text = "Steal"
stealTab.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
stealTab.TextColor3 = Color3.fromRGB(255, 255, 255)
stealTab.Parent = tabBar

-- Frame Base
local baseFrame = Instance.new("Frame")
baseFrame.Size = UDim2.new(1, -10, 1, -70)
baseFrame.Position = UDim2.new(0, 5, 0, 65)
baseFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
baseFrame.Parent = panel

-- AutoLockBase
local autoLockActive = false
local autoLockButton = Instance.new("TextButton")
autoLockButton.Size = UDim2.new(0, 150, 0, 40)
autoLockButton.Position = UDim2.new(0, 10, 0, 10)
autoLockButton.Text = "AutoLockBase: OFF"
autoLockButton.BackgroundColor3 = Color3.fromRGB(100, 100, 150)
autoLockButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoLockButton.Parent = baseFrame

autoLockButton.MouseButton1Click:Connect(function()
	autoLockActive = not autoLockActive
	autoLockButton.Text = "AutoLockBase: " .. (autoLockActive and "ON" or "OFF")
end)

-- Detecta abertura e fecha
task.spawn(function()
	while task.wait(1) do
		if autoLockActive then
			local base = workspace:FindFirstChild("Base_" .. player.Name)
			if base and base:FindFirstChild("Porta") and base.Porta.Open.Value then
				base.Porta.Open.Value = false
			end
		end
	end
end)

-- Botão tempo
local tempoButton = Instance.new("TextButton")
tempoButton.Size = UDim2.new(0, 150, 0, 40)
tempoButton.Position = UDim2.new(0, 10, 0, 60)
tempoButton.Text = "Tempo p/ abrir"
tempoButton.BackgroundColor3 = Color3.fromRGB(150, 100, 100)
tempoButton.TextColor3 = Color3.fromRGB(255, 255, 255)
tempoButton.Parent = baseFrame

tempoButton.MouseButton1Click:Connect(function()
	local tempo = 5
	for i = tempo, 0, -1 do
		tempoButton.Text = "Abrindo em: " .. i
		task.wait(1)
	end
	tempoButton.Text = "Tempo p/ abrir"
end)

-- Frame Steal
local stealFrame = Instance.new("Frame")
stealFrame.Size = UDim2.new(1, -10, 1, -70)
stealFrame.Position = UDim2.new(0, 5, 0, 65)
stealFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
stealFrame.Visible = false
stealFrame.Parent = panel

-- Steal.v1
local stealV1Button = Instance.new("TextButton")
stealV1Button.Size = UDim2.new(0, 150, 0, 40)
stealV1Button.Position = UDim2.new(0, 10, 0, 10)
stealV1Button.Text = "Steal.v1"
stealV1Button.BackgroundColor3 = Color3.fromRGB(150, 100, 50)
stealV1Button.TextColor3 = Color3.fromRGB(255, 255, 255)
stealV1Button.Parent = stealFrame

stealV1Button.MouseButton1Click:Connect(function()
	local base = workspace:FindFirstChild("Base_" .. player.Name)
	if base and base:FindFirstChild("Centro") then
		player.Character:PivotTo(base.Centro.CFrame)
	end
end)

-- Steal Avançado
local stealAvancadoButton = Instance.new("TextButton")
stealAvancadoButton.Size = UDim2.new(0, 150, 0, 40)
stealAvancadoButton.Position = UDim2.new(0, 10, 0, 60)
stealAvancadoButton.Text = "StealAvançado"
stealAvancadoButton.BackgroundColor3 = Color3.fromRGB(50, 150, 150)
stealAvancadoButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stealAvancadoButton.Parent = stealFrame

-- Lista players
local playerListFrame = Instance.new("Frame")
playerListFrame.Size = UDim2.new(0, 200, 0, 150)
playerListFrame.Position = UDim2.new(0, 170, 0, 10)
playerListFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
playerListFrame.Visible = false
playerListFrame.Parent = stealFrame
local uiListLayout = Instance.new("UIListLayout", playerListFrame)

stealAvancadoButton.MouseButton1Click:Connect(function()
	for _, child in ipairs(playerListFrame:GetChildren()) do
		if child:IsA("TextButton") then child:Destroy() end
	end
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= player then
			local btn = Instance.new("TextButton")
			btn.Size = UDim2.new(1, 0, 0, 30)
			btn.Text = p.Name
			btn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
			btn.TextColor3 = Color3.fromRGB(255, 255, 255)
			btn.Parent = playerListFrame
			btn.MouseButton1Click:Connect(function()
				local baseTarget = workspace:FindFirstChild("Base_" .. p.Name)
				if baseTarget and baseTarget:FindFirstChild("Centro") then
					player.Character:PivotTo(baseTarget.Centro.CFrame)
				end
			end)
		end
	end
	playerListFrame.Visible = not playerListFrame.Visible
end)

-- Troca de abas
baseTab.MouseButton1Click:Connect(function()
	baseFrame.Visible = true
	stealFrame.Visible = false
end)

stealTab.MouseButton1Click:Connect(function()
	baseFrame.Visible = false
	stealFrame.Visible = true
end)
