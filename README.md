-- Sistema de Segurança (Anti-Kick, Anti-Exploit, Anti-Blacklist)
local mt = getrawmetatable(game)
setreadonly(mt, false)
local namecall = mt.__namecall
mt.__namecall = newcclosure(function(self, ...)
    local args = {...}
    if getnamecallmethod() == "Kick" or tostring(self) == "Kick" then
        return
    end
    return namecall(self, unpack(args))
end)

-- Proteção extra
game:GetService("Players").LocalPlayer.PlayerScripts.ChildAdded:Connect(function(child)
    if child.Name:lower():find("anti") or child:IsA("LocalScript") and child:FindFirstChildWhichIsA("RemoteEvent") then
        child:Destroy()
    end
end)

-- Rayfield UI
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local Window = Rayfield:CreateWindow({
    Name = "Luar.Exe V2",
    LoadingTitle = "Assombra Chorão👻",
    LoadingSubtitle = "By. Luar.Exe and Miranha.Exe",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "Luar.ExeV2",
        FileName = "Painel"
    },
    KeySystem = false,
})

-- Info
local Info = Window:CreateTab("Info", 4483362458)
Info:CreateParagraph({
    Title = "Chorou pro Luar.Exe",
    Content = "By Luar.Exe and Miranha.Exe"
})

-- PVP
local PVP = Window:CreateTab("PVP", 4483362458)

-- Anti-queda
PVP:CreateToggle({
    Name = "Anti-Queda",
    CurrentValue = false,
    Callback = function(on)
        if on then
            game.Players.LocalPlayer.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
            game.Players.LocalPlayer.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
        else
            game.Players.LocalPlayer.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, true)
            game.Players.LocalPlayer.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, true)
        end
    end,
})

-- Speed
PVP:CreateToggle({
    Name = "Speed",
    CurrentValue = false,
    Callback = function(state)
        local h = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if h then h.WalkSpeed = state and 50 or 16 end
    end,
})

-- No-Clip
local noclipCon
PVP:CreateToggle({
    Name = "No-Clip",
    CurrentValue = false,
    Callback = function(state)
        if state then
            noclipCon = game:GetService("RunService").Stepped:Connect(function()
                local char = game.Players.LocalPlayer.Character
                if char then
                    for _, part in pairs(char:GetDescendants()) do
                        if part:IsA("BasePart") then part.CanCollide = false end
                    end
                end
            end)
        elseif noclipCon then
            noclipCon:Disconnect()
        end
    end,
})

-- Tratar (cura básica)
PVP:CreateButton({
    Name = "Tratar (Vida Cheia)",
    Callback = function()
        local h = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if h then h.Health = h.MaxHealth end
    end
})

-- Visual
local Visual = Window:CreateTab("Visual", 4483362458)

-- ESP
local function addESP(player)
    if player.Character and not player.Character:FindFirstChild("Highlight") then
        local highlight = Instance.new("Highlight")
        highlight.FillColor = Color3.new(1, 1, 0)
        highlight.FillTransparency = 0.3
        highlight.OutlineColor = Color3.new(1, 1, 1)
        highlight.OutlineTransparency = 0.8
        highlight.Adornee = player.Character
        highlight.Parent = player.Character
    end
end

Visual:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Callback = function(state)
        for _, p in pairs(game.Players:GetPlayers()) do
            if p ~= game.Players.LocalPlayer then
                if state then
                    p.CharacterAdded:Connect(function()
                        wait(1)
                        addESP(p)
                    end)
                    if p.Character then
                        addESP(p)
                    end
                else
                    if p.Character and p.Character:FindFirstChild("Highlight") then
                        p.Character.Highlight:Destroy()
                    end
                end
            end
        end
    end
})

-- Infinite Yield
Visual:CreateButton({
    Name = "Infinite Yield",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    end
})

-- God Mode
Visual:CreateButton({
    Name = "God Mode",
    Callback = function()
        local char = game.Players.LocalPlayer.Character
        if char then
            for _, v in pairs(char:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.Anchored = true
                end
            end
        end
    end
})

-- Anti-Staff
Visual:CreateButton({
    Name = "Anti-Staff",
    Callback = function()
        while wait(2) do
            for _, p in pairs(game.Players:GetPlayers()) do
                if p:GetRoleInGroup(1) == "Staff" then
                    game.Players.LocalPlayer:Kick("Staff Detectado")
                end
            end
        end
    end
})

-- Revistar (mensagem loop)
local revistar = false
Visual:CreateToggle({
    Name = "Revistar (loop)",
    CurrentValue = false,
    Callback = function(v)
        revistar = v
        while revistar do
            game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/revistar morto", "All")
            wait(5)
        end
    end
})

-- Aimbot
local Aimbot = Window:CreateTab("Aimbot", 4483362458)
Aimbot:CreateButton({
    Name = "Ativar Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))()
    end,
})

-- Players
local Players = Window:CreateTab("Players", 4483362458)
Players:CreateButton({
    Name = "Bring All",
    Callback = function()
        local pos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
        for _, p in pairs(game.Players:GetPlayers()) do
            if p.Character then
                p.Character:SetPrimaryPartCFrame(CFrame.new(pos + Vector3.new(math.random(-5,5),0,math.random(-5,5))))
            end
        end
    end
})

-- Trolls
local Trolls = Window:CreateTab("Trolls", 4483362458)
local fakeLag = false
Trolls:CreateToggle({
    Name = "Fake Lag",
    CurrentValue = false,
    Callback = function(state)
        fakeLag = state
        while fakeLag do
            wait(0.1)
            game:GetService("RunService").RenderStepped:Wait()
        end
    end
})

-- Farms
local Farms = Window:CreateTab("Farms", 4483362458)
Farms:CreateButton({
    Name = "Farm Lixos da Cidade",
    Callback = function()
        -- Código do Auto-Farm pode ser adicionado aqui
    end
})

Farms:CreateButton({
    Name = "Farm Planta Suja",
    Callback = function()
        -- Código do Auto-Farm pode ser adicionado aqui
    end
})
