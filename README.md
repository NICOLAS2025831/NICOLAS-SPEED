-- LocalScript - Speed 1000 com verificação de time funcional

local Players = game:GetService("Players")
local player = Players.LocalPlayer

local allowedTeamName = "Testadores" -- Nome EXATO do time permitido
local speedOn = 1000
local speedOff = 16

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

local speedAtivo = false

local function alterarVelocidade(valor)
	local char = player.Character or player.CharacterAdded:Wait()
	local humanoid = char:FindFirstChildWhichIsA("Humanoid")
	if humanoid then
		humanoid.WalkSpeed = valor
	end
end

local function verificarTime()
	-- Verifica se o jogador está em um time e compara com o nome esperado
	local team = player.Team
	return team and team.Name == allowedTeamName
end

button.MouseButton1Click:Connect(function()
	if not verificarTime() then
		button.Text = "Acesso Negado!"
		wait(1.5)
		button.Text = speedAtivo and "Desativar Speed" or "Ativar Speed 1000"
		return
	end

	speedAtivo = not speedAtivo
	if speedAtivo then
		alterarVelocidade(speedOn)
		button.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
		button.Text = "Desativar Speed"
	else
		alterarVelocidade(speedOff)
		button.BackgroundColor3 = Color3.fromRGB(255, 85, 0)
		button.Text = "Ativar Speed 1000"
	end
end)

player.CharacterAdded:Connect(function(char)
	local humanoid = char:WaitForChild("Humanoid")
	if humanoid then
		humanoid.WalkSpeed = speedAtivo and speedOn or speedOff
	end
end)
