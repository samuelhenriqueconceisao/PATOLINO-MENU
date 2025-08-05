-- Patolino Menu

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local TweenService = game:GetService("TweenService")

local gui = Instance.new("ScreenGui")
gui.Name = "PatolinoMenu"
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- Botão 🦆
local openButton = Instance.new("TextButton")
openButton.Name = "🦆buttom"
openButton.Size = UDim2.new(0, 60, 0, 30)
openButton.Position = UDim2.new(0, 10, 0, 10)
openButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
openButton.TextColor3 = Color3.fromRGB(255, 255, 255)
openButton.Text = "🦆"
openButton.Font = Enum.Font.SourceSansBold
openButton.TextSize = 16
openButton.Parent = gui

-- Menu principal com visual moderno
local mainFrame = Instance.new("Frame")
mainFrame.Name = "Menu"
mainFrame.Size = UDim2.new(0, 500, 0, 300)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.ClipsDescendants = true
mainFrame.BackgroundTransparency = 1
mainFrame.Parent = gui

-- Arredondamento
local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 12)
uiCorner.Parent = mainFrame

-- Sombra
local shadow = Instance.new("ImageLabel")
shadow.Name = "Shadow"
shadow.AnchorPoint = Vector2.new(0.5, 0.5)
shadow.Position = UDim2.new(0.5, 0, 0.5, 0)
shadow.Size = UDim2.new(1, 40, 1, 40)
shadow.BackgroundTransparency = 1
shadow.Image = "rbxassetid://1316045217"
shadow.ImageTransparency = 0.5
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(10, 10, 118, 118)
shadow.ZIndex = -1
shadow.Parent = mainFrame

-- TÃ­tulo
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "Patolino menu"
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextXAlignment = Enum.TextXAlignment.Center
title.Parent = mainFrame

-- Aba lateral
local sideMenu = Instance.new("Frame")
sideMenu.Size = UDim2.new(0, 150, 1, -40)
sideMenu.Position = UDim2.new(0, 0, 0, 40)
sideMenu.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
sideMenu.BorderSizePixel = 0
sideMenu.Parent = mainFrame

-- Area de conteÃºdo
local contentArea = Instance.new("Frame")
contentArea.Size = UDim2.new(1, -150, 1, -40)
contentArea.Position = UDim2.new(0, 150, 0, 40)
contentArea.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
contentArea.BorderSizePixel = 0
contentArea.Parent = mainFrame

-- FunÃ§Ãµes utilitÃ¡rias
local function createCategory(name, order, contentFunction)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, 0, 0, 40)
	btn.Position = UDim2.new(0, 0, 0, (order - 1) * 40)
	btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Text = name
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 16
	btn.BorderSizePixel = 0
	btn.Parent = sideMenu

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = btn

	btn.MouseButton1Click:Connect(function()
		for _, child in pairs(contentArea:GetChildren()) do
			child:Destroy()
		end
		contentFunction()
	end)
end

local function createOption(text, position, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0.9, 0, 0, 30)
	btn.Position = UDim2.new(0.05, 0, 0, 10 + (position - 1) * 35)
	btn.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 16
	btn.Text = text
	btn.BorderSizePixel = 0
	btn.Parent = contentArea

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 6)
	corner.Parent = btn

	if callback then
		btn.MouseButton1Click:Connect(callback)
	end
end

-- Weapons
createCategory("Weapons", 1, function()
	createOption("Aimbot", 1, function()
		print("Aimbot ativado (placeholder)")
	end)

	createOption("ESP", 2, function()
		local RunService = game:GetService("RunService")
		local Players = game:GetService("Players")
		local Camera = workspace.CurrentCamera
		local LocalPlayer = Players.LocalPlayer

		local function getRainbowColor(speed)
			local t = tick() * (speed or 1)
			return Color3.fromHSV((t % 1), 1, 1)
		end

		local drawings = {}

		local function createESP(player)
			if player == LocalPlayer then return end
			drawings[player] = {
				healthText = Drawing.new("Text"),
				parts = {}
			}
			local d = drawings[player]
			d.healthText.Size = 13
			d.healthText.Center = true
			d.healthText.Outline = true
			d.healthText.Visible = false

			local bodyParts = {
				{"Head", "UpperTorso"},
				{"UpperTorso", "LowerTorso"},
				{"UpperTorso", "LeftUpperArm"},
				{"LeftUpperArm", "LeftLowerArm"},
				{"LeftLowerArm", "LeftHand"},
				{"UpperTorso", "RightUpperArm"},
				{"RightUpperArm", "RightLowerArm"},
				{"RightLowerArm", "RightHand"},
				{"LowerTorso", "LeftUpperLeg"},
				{"LeftUpperLeg", "LeftLowerLeg"},
				{"LeftLowerLeg", "LeftFoot"},
				{"LowerTorso", "RightUpperLeg"},
				{"RightUpperLeg", "RightLowerLeg"},
				{"RightLowerLeg", "RightFoot"},
			}

			for _, part in ipairs(bodyParts) do
				local line = Drawing.new("Line")
				line.Thickness = 2
				line.Visible = false
				table.insert(d.parts, {line = line, from = part[1], to = part[2]})
			end
		end

		local function removeESP(player)
			if drawings[player] then
				drawings[player].healthText:Remove()
				for _, part in ipairs(drawings[player].parts) do
					part.line:Remove()
				end
				drawings[player] = nil
			end
		end

		RunService.RenderStepped:Connect(function()
			local rainbow = getRainbowColor(0.5)
			for player, data in pairs(drawings) do
				local character = player.Character
				local hrp = character and character:FindFirstChild("HumanoidRootPart")
				local humanoid = character and character:FindFirstChildOfClass("Humanoid")

				if character and hrp and humanoid and humanoid.Health > 0 then
					local pos, onScreen = Camera:WorldToViewportPoint(hrp.Position + Vector3.new(0, 3, 0))
					data.healthText.Position = Vector2.new(pos.X, pos.Y)
					data.healthText.Text = string.format("%s - %d HP", player.Name, humanoid.Health)
					data.healthText.Visible = onScreen
					data.healthText.Color = rainbow

					for _, partData in ipairs(data.parts) do
						local from = character:FindFirstChild(partData.from)
						local to = character:FindFirstChild(partData.to)
						if from and to then
							local pos1, os1 = Camera:WorldToViewportPoint(from.Position)
							local pos2, os2 = Camera:WorldToViewportPoint(to.Position)
							partData.line.From = Vector2.new(pos1.X, pos1.Y)
							partData.line.To = Vector2.new(pos2.X, pos2.Y)
							partData.line.Visible = os1 and os2
							partData.line.Color = rainbow
						else
							partData.line.Visible = false
						end
					end
				else
					data.healthText.Visible = false
					for _, part in ipairs(data.parts) do
						part.line.Visible = false
					end
				end
			end
		end)

		for _, player in ipairs(Players:GetPlayers()) do createESP(player) end
		Players.PlayerAdded:Connect(createESP)
		Players.PlayerRemoving:Connect(removeESP)
	end)
end)

-- Farms
createCategory("Farms", 2, function()
	createOption("Farm Entregador", 1, function()
		pcall(function()
loadstring(game:HttpGet('https://raw.githubusercontent.com/Cr4ki3/Flexhub/refs/heads/main/Pizza'))()# 👾 VIRUS STORE 

*🚘 Vendemos carros, Entre supra skyliner Mercedes G36 e Skyliner limitado*

*🔫 Vendemos Armas, Entre Hk limitada Ak limitada Parafal, Ak47 Glock e usp*

*🎁 Sorteios e Outras Diversas Coisas como: Estamos sorteiando Lhamborguini limitado neste exato momento corre la!*

*🤝 Fechamos Parceria com Facções Lojas e outros tipos fora do jogo pra crescermos mais!*

*🎖 Temos o titulo de loja com + vendas ate hoje do ilha bela existindo a + 1 ano com +200 vendas!*

-# ENTRE JA EM CONTATO COMIGO, OU ENTRE NA MINHA BIO!op		end)
	end)

	createOption("Farm Erva", 2, function()
		pcall(function()
			loadstring(game:HttpGet("https://pastebin.com/raw/BcUtFrEN"))()
		end)
	end)

	createOption("Farm Uber", 3, function()
		local player = game.Players.LocalPlayer
		local character = player.Character or player.CharacterAdded:Wait()
		local hrp = character:WaitForChild("HumanoidRootPart")
		local carros = workspace:WaitForChild("CarrosSpawnados"):GetChildren()
		local carroPerto, menorDist = nil, math.huge

		for _, c in pairs(carros) do
			if c:IsA("Model") then
				local ref = c:FindFirstChild("DriveSeat") or c:FindFirstChildWhichIsA("BasePart")
				if ref then
					local dist = (ref.Position - hrp.Position).Magnitude
					if dist < menorDist then
						menorDist = dist
						carroPerto = c
					end
				end
			end
		end

		if carroPerto then
			carroPerto.Name = "ok"
			print("Carro perto renomeado pra ok")
		else
			warn("Não achei carro")
		end

		pcall(function()
			loadstring(game:HttpGet("https://raw.githubusercontent.com/vulgocj/salve/refs/heads/main/Protected_5363406451479577.lua.txt"))()
		end)
	end)

	createOption("Farm Sedex (Em breve)", 4, function()
		print("Ainda em desenvolvimento")
	end)
end)

-- Misc
createCategory("Misc", 3, function()
	createOption("Discord: https://discord.gg/sPdnEggH", 1, function()
		setclipboard("https://discord.gg/sPdnEggH")
		print("Link do Discord copiado!")
	end)

	createOption(" Produzido por Kota", 2)
end)

-- Animação abrir/fechar menu
local isOpen = false
openButton.MouseButton1Click:Connect(function()
	isOpen = not isOpen
	if isOpen then
		mainFrame.Visible = true
		TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
			BackgroundTransparency = 0
		}):Play()
	else
		local tween = TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
			BackgroundTransparency = 1
		})
		tween:Play()
		tween.Completed:Wait()
		mainFrame.Visible = false
	end
end)

-- Anti-AFK
local vu = game:GetService("VirtualUser")
player.Idled:Connect(function()
	vu:Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
	wait(1)
	vu:Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
	print("Anti-AFK acionado")
end)
