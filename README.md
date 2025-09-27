-- Script no ServerScriptService

local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Cria RemoteEvents se não existirem
local function getOrCreateEvent(name)
    local ev = ReplicatedStorage:FindFirstChild(name)
    if not ev then
        ev = Instance.new("RemoteEvent")
        ev.Name = name
        ev.Parent = ReplicatedStorage
    end
    return ev
end

local congelarEvent = getOrCreateEvent("CongelarAnimaisEvent")
local descongelarEvent = getOrCreateEvent("DescongelarAnimaisEvent")
local highlightEvent = getOrCreateEvent("HighlightAnimaisEvent")

local animaisFolder = workspace:WaitForChild("Animais")

-- Função que congela os animais
local function CongelarAnimais()
	for _, animal in ipairs(animaisFolder:GetChildren()) do
		if animal:FindFirstChild("Humanoid") then
			animal.Humanoid:ChangeState(Enum.HumanoidStateType.Physics)
		end
		if animal:FindFirstChild("HumanoidRootPart") then
			for _, obj in ipairs(animal.HumanoidRootPart:GetChildren()) do
				if obj:IsA("BodyMover") then
					obj:Destroy()
				end
			end
		end
	end
	print(">> Todos os animais foram congelados!")
end

-- Função que descongela os animais
local function DescongelarAnimais()
	for _, animal in ipairs(animaisFolder:GetChildren()) do
		if animal:FindFirstChild("Humanoid") then
			animal.Humanoid:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)
		end
	end
	print(">> Todos os animais foram descongelados!")
end

-- Função que destaca os animais
local function DestacarAnimais()
	for _, animal in ipairs(animaisFolder:GetChildren()) do
		if not animal:FindFirstChild("Highlight") then
			local hl = Instance.new("Highlight")
			hl.Name = "Highlight"
			hl.FillTransparency = 1 -- deixa só as linhas
			hl.OutlineColor = Color3.fromRGB(0, 255, 0) -- verde
			hl.Parent = animal
		end
	end
	print(">> Todos os animais foram destacados!")
end

-- Liga eventos aos botões
congelarEvent.OnServerEvent:Connect(function(player)
	print(player.Name .. " clicou em Congelar Animais!")
	CongelarAnimais()
end)

descongelarEvent.OnServerEvent:Connect(function(player)
	print(player.Name .. " clicou em Descongelar Animais!")
	DescongelarAnimais()
end)

highlightEvent.OnServerEvent:Connect(function(player)
	print(player.Name .. " clicou em Destacar Animais!")
	DestacarAnimais()
end)
