-- LocalScript - Speed 1000 com verificação de time

local Players = game:GetService("Players")
local player = Players.LocalPlayer

local allowedTeam = "Testadores" -- Altere aqui para o nome exato do time
local speedValue = 1000
local defaultSpeed = 16

-- GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "SpeedGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 200, 0, 50)
button.Position = UDim2.new(0.5, -100, 0.8, 0)
button.BackgroundColor3 = Color3.fromRGB(255, 85, 0)
button.TextColor3 = Color3.new(1, 1, 1)
button.Font = Enum.Font.GothamBold
button.TextSize = 22
button.Text = "Ativar Speed 1000"
button.BorderSizePixel = 0
button.Parent = screenGui

local uiCorner = Instance.new("UICorner", button)
uiCorner.CornerRadius = UDim.new(0, 12)

local toggle = false

-- Função para alterar velocidade
local function setSpeed(speed)
	local char = player.Character or player.CharacterAdded:Wait()
	local humanoid = char:FindFirstChildWhichIsA("Humanoid")
	if humanoid then
		humanoid.WalkSpeed = speed
	end
end

-- Botão de controle
button.MouseButton1Click:Connect(function()
	if not player.Team or player.Team.Name ~= allowedTeam then
		button.Text = "Time Incorreto!"
		wait(1.5)
		button.Text = toggle and "Desativar Speed" or "Ativar Speed 1000"
		return
	end

	toggle = not toggle
	if toggle then
		setSpeed(speedValue)
		button.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
		button.Text = "Desativar Speed"
	else
		setSpeed(defaultSpeed)
		button.BackgroundColor3 = Color3.fromRGB(255, 85, 0)
		button.Text = "Ativar Speed 1000"
	end
end)

-- Resetar velocidade quando respawnar
player.CharacterAdded:Connect(function(char)
	char:WaitForChild("Humanoid").WalkSpeed = toggle and speedValue or defaultSpeed
end)
