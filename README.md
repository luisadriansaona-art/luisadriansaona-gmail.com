local player = game.Players.LocalPlayer

-- Crear GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BelixHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Crear Frame
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 200)
frame.Position = UDim2.new(0.05, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- Crear título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "BELIX HUB"
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true
title.Parent = frame

-- Variables para TP
local savedPosition = nil
local humanoidRootPart = nil

local function updateHRP()
	local char = player.Character or player.CharacterAdded:Wait()
	humanoidRootPart = char:WaitForChild("HumanoidRootPart")
end
updateHRP()
player.CharacterAdded:Connect(updateHRP)

-- Función para botones
local function createButton(text, posY, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, -10, 0, 40)
	btn.Position = UDim2.new(0, 5, 0, posY)
	btn.Text = text
	btn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.SourceSans
	btn.TextScaled = true
	btn.Parent = frame
	btn.MouseButton1Click:Connect(callback)
end

-- TP: guardar posición
createButton("TP (Guardar)", 50, function()
	if humanoidRootPart then
		savedPosition = humanoidRootPart.CFrame
		warn("📌 Posición guardada")
	end
end)

-- TP2: teletransportar
createButton("TP2 (Ir)", 100, function()
	if humanoidRootPart and savedPosition then
		humanoidRootPart.CFrame = savedPosition
		warn("✅ Teletransportado")
	else
		warn("❌ No hay posición guardada")
	end
end)

-- TRAS: atravesar paredes
createButton("TRAS (Noclip)", 150, function()
	local char = player.Character
	if char then
		for _, part in pairs(char:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
		warn("🚪 Ahora puedes atravesar paredes")
	end
end) 