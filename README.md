-- Sistema de Segurança Avançado
pcall(function()
    local mt = getrawmetatable(game)
    setreadonly(mt, false)
    local old = mt.__namecall
    mt.__namecall = newcclosure(function(self, ...)
        local args = {...}
        local method = getnamecallmethod()
        if method == "Kick" or method == "kick" then
            return warn("Tentativa de Kick Bloqueada.")
        end
        return old(self, unpack(args))
    end)
end)

-- Anti-Blacklist & Anti-Exploit Bypass
pcall(function()
    for _, v in pairs(game.Players.LocalPlayer:GetDescendants()) do
        if v:IsA("StringValue") and string.find(v.Name:lower(), "ban") then
            v:Destroy()
        end
    end
end)

-- Carregar Rayfield
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

-- Criar Janela Principal
local Window = Rayfield:CreateWindow({
    Name = "Luar.Exe V2",
    LoadingTitle = "Assombra Chorão👻",
    LoadingSubtitle = "By. Luar.Exe and Miranha.Exe",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "LuarExe",
        FileName = "config",
    },
    KeySystem = false,
})

-- Aba Info
local Info = Window:CreateTab("Info", 4483362458)
Info:CreateParagraph({
    Title = "Chorou Pro Luar.Exe e Pro Miranha",
    Content = "Será???"
})

-- Aba PVP
local PVP = Window:CreateTab("PVP", 4483362458)

-- Speed
PVP:CreateToggle({
    Name = "Speed",
    CurrentValue = false,
    Callback = function(val)
        local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            hum.WalkSpeed = val and 50 or 16
        end
    end
})

-- No-Clip
local NoClipConnection
PVP:CreateToggle({
    Name = "No-Clip",
    CurrentValue = false,
    Callback = function(state)
        if state then
            NoClipConnection = game:GetService("RunService").Stepped:Connect(function()
                for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end)
        else
            if NoClipConnection then NoClipConnection:Disconnect() end
        end
    end
})

-- Anti-Queda
PVP:CreateToggle({
    Name = "Anti-Dano de Queda",
    CurrentValue = false,
    Callback = function(state)
        game:GetService("RunService").Heartbeat:Connect(function()
            if state then
                local char = game.Players.LocalPlayer.Character
                if char then
                    local hum = char:FindFirstChildWhichIsA("Humanoid")
                    if hum then hum:SetStateEnabled(Enum.HumanoidStateType.Freefall, false) end
                end
            end
        end)
    end
})

-- Tratar (Regeneração)
PVP:CreateButton({
    Name = "Tratar",
    Callback = function()
        local char = game.Players.LocalPlayer.Character
        if char then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum then hum.Health = hum.MaxHealth end
        end
    end
})

-- Aba Visual
local Visual = Window:CreateTab("Visual", 4483362458)

-- ESP Funcional Pós-Morte
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

Visual:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Callback = function(state)
        if state then
            local function createESP(player)
                local highlight = Instance.new("Highlight")
                highlight.Name = "HighlightESP"
                highlight.FillColor = Color3.new(1, 1, 0)
                highlight.OutlineColor = Color3.new(1, 1, 1)
                highlight.FillTransparency = 0.3
                highlight.OutlineTransparency = 0
                highlight.Adornee = player.Character
                highlight.Parent = player.Character
            end

            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Players.LocalPlayer and player.Character then
                    createESP(player)
                end
                player.CharacterAdded:Connect(function()
                    task.wait(1)
                    createESP(player)
                end)
            end
        else
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character and player.Character:FindFirstChild("HighlightESP") then
                    player.Character.HighlightESP:Destroy()
                end
            end
        end
    end
})

-- Mandar Revistar (loop 100x)
local revistarAtivo = false
Visual:CreateToggle({
    Name = "Mandar Revistar (Spam 100x)",
    CurrentValue = false,
    Callback = function(estado)
        revistarAtivo = estado
        if revistarAtivo then
            task.spawn(function()
                for i = 1, 100 do
                    if not revistarAtivo then break end
                    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/revistar morto", "All")
                    wait(0.1)
                end
            end)
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
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        if hum then hum.Name = "God" hum.MaxHealth = math.huge hum.Health = math.huge end
    end
})

-- Anti-Staff (Remove scripts suspeitos)
Visual:CreateButton({
    Name = "Anti-Staff",
    Callback = function()
        for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if v:IsA("Script") or v:IsA("LocalScript") then
                v:Destroy()
            end
        end
    end
})

-- Aba Players
local PlayersTab = Window:CreateTab("Players", 4483362458)

PlayersTab:CreateButton({
    Name = "Bring All",
    Callback = function()
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr.Character then
                plr.Character:MoveTo(game.Players.LocalPlayer.Character.HumanoidRootPart.Position + Vector3.new(3, 0, 3))
            end
        end
    end
})

-- Aba Trolls
local Trolls = Window:CreateTab("Trolls", 4483362458)

Trolls:CreateToggle({
    Name = "Fake Lag",
    CurrentValue = false,
    Callback = function(on)
        if on then
            game:GetService("RunService"):Set3dRenderingEnabled(false)
        else
            game:GetService("RunService"):Set3dRenderingEnabled(true)
        end
    end
})

-- Aba Aimbot
local Aimbot = Window:CreateTab("Aimbot", 4483362458)
Aimbot:CreateButton({
    Name = "Ativar Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Aimbot-fov-mobile-7607"))()
    end
})

-- Aba Farms
local Farms = Window:CreateTab("Farms", 4483362458)

Farms:CreateButton({
    Name = "Auto-Farm: Lixos da Cidade",
    Callback = function()
        while true do
            for _, v in pairs(workspace:GetDescendants()) do
                if v.Name:lower():find("lixo") then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.CFrame
                    wait(0.5)
                end
            end
        end
    end
})
