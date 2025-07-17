--[[
  Sistema de Permissão + ESP/Murder GUI Centralizado (Draggable)
  1. Primeiro mostra uma GUI de verificação ("Verificar").
  2. Só executa o script principal se o player for "ShotzBanido1".
  3. Se permitido: aparece "permitido" e abre a GUI principal (com botão ESP).
  4. Se não permitido: aparece "Não permitido" e fecha tudo após 5 segundos.
  5. O botão ESP está na GUI principal e ativa/desativa o box em volta dos jogadores, mostrando o nome do time na cor do team.
]]

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- GUI de Verificação
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PermissionCheckGUI"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local verifyFrame = Instance.new("Frame")
verifyFrame.Size = UDim2.new(0, 240, 0, 120)
verifyFrame.AnchorPoint = Vector2.new(0.5,0.5)
verifyFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
verifyFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
verifyFrame.BorderSizePixel = 3
verifyFrame.BorderColor3 = Color3.fromRGB(140, 40, 255) -- Borda roxa
verifyFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 18)
UICorner.Parent = verifyFrame

local verifyTitle = Instance.new("TextLabel")
verifyTitle.Parent = verifyFrame
verifyTitle.Size = UDim2.new(1, 0, 0, 44)
verifyTitle.Position = UDim2.new(0, 0, 0, 0)
verifyTitle.BackgroundTransparency = 1
verifyTitle.Text = "Verificação de Permissão"
verifyTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
verifyTitle.Font = Enum.Font.GothamBold
verifyTitle.TextSize = 20

local verifyBtn = Instance.new("TextButton")
verifyBtn.Size = UDim2.new(0.8, 0, 0, 36)
verifyBtn.Position = UDim2.new(0.1, 0, 0, 64)
verifyBtn.BackgroundColor3 = Color3.fromRGB(100, 70, 180)
verifyBtn.Text = "Verificar"
verifyBtn.TextColor3 = Color3.fromRGB(255,255,255)
verifyBtn.Font = Enum.Font.GothamSemibold
verifyBtn.TextSize = 18
verifyBtn.Parent = verifyFrame
local verifyBtnCorner = Instance.new("UICorner")
verifyBtnCorner.CornerRadius = UDim.new(0, 12)
verifyBtnCorner.Parent = verifyBtn

-- Notificação
local function showNotification(msg, duration)
    duration = duration or 2
    local notif = Instance.new("Frame")
    notif.Size = UDim2.new(0, 320, 0, 40)
    notif.Position = UDim2.new(0.5, -160, 0.12, 0)
    notif.BackgroundColor3 = Color3.fromRGB(32,32,48)
    notif.AnchorPoint = Vector2.new(0.5,0)
    notif.Parent = ScreenGui
    notif.ZIndex = 200
    notif.BorderSizePixel = 2
    notif.BorderColor3 = Color3.fromRGB(140, 40, 255)

    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, 12)
    notifCorner.Parent = notif

    local notifText = Instance.new("TextLabel")
    notifText.Size = UDim2.new(1,0,1,0)
    notifText.Position = UDim2.new(0,0,0,0)
    notifText.BackgroundTransparency = 1
    notifText.Text = msg
    notifText.TextColor3 = Color3.fromRGB(255,100,255)
    notifText.Font = Enum.Font.GothamBold
    notifText.TextSize = 20
    notifText.Parent = notif
    notifText.ZIndex = notif.ZIndex + 1

    -- Fade out animation
    spawn(function()
        wait(duration)
        for i = 1, 10 do
            notif.BackgroundTransparency = i/10
            notifText.TextTransparency = i/10
            wait(0.05)
        end
        notif:Destroy()
    end)
end

-- Script principal (ESP/Murder GUI)
local function showMainScript()
    verifyFrame:Destroy()

    local Teams = game:GetService("Teams")
    local UIS = game:GetService("UserInputService")
    local murderTeamName = "Murderer"
    local sheriffTeamName = "Sheriff"

    local mainGui = Instance.new("ScreenGui")
    mainGui.Name = "CoolESP_GUI"
    mainGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
    mainGui.ResetOnSpawn = false

    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, 250, 0, 260)
    mainFrame.AnchorPoint = Vector2.new(0.5,0.5)
    mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
    mainFrame.BorderSizePixel = 3
    mainFrame.BorderColor3 = Color3.fromRGB(140, 40, 255)
    mainFrame.Parent = mainGui

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 18)
    UICorner.Parent = mainFrame

    local titleBar = Instance.new("Frame")
    titleBar.Size = UDim2.new(1, 0, 0, 38)
    titleBar.BackgroundColor3 = Color3.fromRGB(32, 32, 48)
    titleBar.Parent = mainFrame
    titleBar.ZIndex = mainFrame.ZIndex + 1

    local title = Instance.new("TextLabel")
    title.Parent = titleBar
    title.Size = UDim2.new(0.8, -38, 1, 0)
    title.Position = UDim2.new(0, 10, 0, 0)
    title.BackgroundTransparency = 1
    title.Text = "ESP & Murder GUI"
    title.TextColor3 = Color3.fromRGB(255, 255, 255)
    title.Font = Enum.Font.GothamBold
    title.TextSize = 20
    title.TextXAlignment = Enum.TextXAlignment.Left

    -- Botão de minimizar no canto
    local minimizeBtn = Instance.new("ImageButton")
    minimizeBtn.Size = UDim2.new(0, 28, 0, 28)
    minimizeBtn.Position = UDim2.new(1, -35, 0.5, -14)
    minimizeBtn.BackgroundTransparency = 1
    minimizeBtn.Image = "rbxassetid://7072725342"
    minimizeBtn.Parent = titleBar
    minimizeBtn.ZIndex = titleBar.ZIndex + 1

    local buttonNames = {"ESP", "Atirar no Murder", "Bypass"}
    local buttons = {}

    for i, name in ipairs(buttonNames) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(0.8, 0, 0, 40)
        btn.Position = UDim2.new(0.1, 0, 0, 45 + (i-1)*60)
        btn.BackgroundColor3 = Color3.fromRGB(55, 85, 115)
        btn.Text = name
        btn.TextColor3 = Color3.fromRGB(255,255,255)
        btn.Font = Enum.Font.GothamSemibold
        btn.TextSize = 18
        btn.Parent = mainFrame
        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 12)
        btnCorner.Parent = btn
        buttons[name] = btn
    end

    -- Minimizar GUI só pelo ícone!
    local minimized = false
    local function setMinimized(state)
        minimized = state
        for _, btn in pairs(buttons) do
            btn.Visible = not minimized
        end
        mainFrame.Size = minimized and UDim2.new(0, 250, 0, 45) or UDim2.new(0, 250, 0, 260)
        title.Text = minimized and "Toque para restaurar" or "ESP & Murder GUI"
        minimizeBtn.Image = minimized and "rbxassetid://7072725478" or "rbxassetid://7072725342"
    end

    minimizeBtn.MouseButton1Click:Connect(function()
        setMinimized(not minimized)
    end)

    titleBar.InputBegan:Connect(function(input)
        if minimized and (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) then
            setMinimized(false)
        end
    end)

    -- Drag & drop (Mouse e Touch)
    local dragging = false
    local dragStart, startPos

    titleBar.InputBegan:Connect(function(input)
        if not minimized and (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) then
            dragging = true
            dragStart = input.Position
            startPos = mainFrame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    titleBar.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    UIS.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    -- NOVO SISTEMA ESP: atualizado por eventos, não por RenderStepped
    local espEnabled = false
    local espBoxes = {}
    local espLabels = {}

    local function removeEsp()
        for _, v in pairs(espBoxes) do
            v:Destroy()
        end
        espBoxes = {}
        for _, v in pairs(espLabels) do
            v:Destroy()
        end
        espLabels = {}
    end

    local function getTeamInfo(player)
        if player.Team then
            return player.Team.Name, player.Team.TeamColor.Color
        end
        return "Sem Team", Color3.fromRGB(200, 200, 200)
    end

    local function createEspBoxAndLabel(player)
        if player == LocalPlayer or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end
        local teamName, color = getTeamInfo(player)
        local adornee = player.Character:FindFirstChild("HumanoidRootPart")
        if not adornee then return end

        -- ESP Box
        local box = Instance.new("BoxHandleAdornment")
        box.Adornee = adornee
        box.Size = Vector3.new(4, 6, 2)
        box.Color3 = color
        box.Transparency = 0.6
        box.AlwaysOnTop = true
        box.ZIndex = 10
        box.Parent = mainGui
        table.insert(espBoxes, box)

        -- Team Name Label
        local billboard = Instance.new("BillboardGui")
        billboard.Adornee = adornee
        billboard.Size = UDim2.new(0, 100, 0, 22)
        billboard.StudsOffset = Vector3.new(0, 4, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = mainGui

        local textLabel = Instance.new("TextLabel")
        textLabel.Size = UDim2.new(1,0,1,0)
        textLabel.BackgroundTransparency = 1
        textLabel.Text = teamName
        textLabel.TextColor3 = color
        textLabel.Font = Enum.Font.GothamBold
        textLabel.TextSize = 18
        textLabel.Parent = billboard

        table.insert(espLabels, billboard)
    end

    local function updateEsp()
        removeEsp()
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                createEspBoxAndLabel(player)
            end
        end
    end

    -- Atualização por eventos
    local function setupEspListeners()
        -- Para cada player atual, conecta CharacterAdded
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                player.CharacterAdded:Connect(function()
                    if espEnabled then updateEsp() end
                end)
            end
        end
        -- Quando um novo player entra
        Players.PlayerAdded:Connect(function(player)
            if player ~= LocalPlayer then
                player.CharacterAdded:Connect(function()
                    if espEnabled then updateEsp() end
                end)
                if espEnabled then updateEsp() end
            end
        end)
        -- Quando um player sai
        Players.PlayerRemoving:Connect(function(player)
            if espEnabled then updateEsp() end
        end)
    end
    setupEspListeners()

    buttons["ESP"].MouseButton1Click:Connect(function()
        espEnabled = not espEnabled
        buttons["ESP"].Text = espEnabled and "ESP (ON)" or "ESP"
        if espEnabled then
            updateEsp()
        else
            removeEsp()
        end
    end)

    -- Notificação principal
    local function showMainNotification(msg, duration)
        duration = duration or 2
        local notif = Instance.new("Frame")
        notif.Size = UDim2.new(0, 320, 0, 40)
        notif.Position = UDim2.new(0.5, -160, 0.12, 0)
        notif.BackgroundColor3 = Color3.fromRGB(32,32,48)
        notif.AnchorPoint = Vector2.new(0.5,0)
        notif.Parent = mainGui
        notif.ZIndex = 200
        notif.BorderSizePixel = 2
        notif.BorderColor3 = Color3.fromRGB(140, 40, 255)

        local notifCorner = Instance.new("UICorner")
        notifCorner.CornerRadius = UDim.new(0, 12)
        notifCorner.Parent = notif

        local notifText = Instance.new("TextLabel")
        notifText.Size = UDim2.new(1,0,1,0)
        notifText.Position = UDim2.new(0,0,0,0)
        notifText.BackgroundTransparency = 1
        notifText.Text = msg
        notifText.TextColor3 = Color3.fromRGB(255,100,255)
        notifText.Font = Enum.Font.GothamBold
        notifText.TextSize = 20
        notifText.Parent = notif
        notifText.ZIndex = notif.ZIndex + 1

        spawn(function()
            wait(duration)
            for i = 1, 10 do
                notif.BackgroundTransparency = i/10
                notifText.TextTransparency = i/10
                wait(0.05)
            end
            notif:Destroy()
        end)
    end

    -- Atirar no Murder
    local function findTeamPlayer(teamName)
        for _, player in ipairs(Players:GetPlayers()) do
            if player.Team and player.Team.Name == teamName then
                return player
            end
        end
        return nil
    end

    local function hasGun(player)
        for _, tool in ipairs(player.Backpack:GetChildren()) do
            if tool:IsA("Tool") and tool.Name:lower():find("gun") then
                return tool
            end
        end
        local toolChar = player.Character and player.Character:FindFirstChildOfClass("Tool")
        if toolChar and toolChar.Name:lower():find("gun") then
            return toolChar
        end
        return nil
    end

    local function fireOnMurder()
        local sheriff = findTeamPlayer(sheriffTeamName)
        local murderer = findTeamPlayer(murderTeamName)
        local isSheriff = LocalPlayer.Team and LocalPlayer.Team.Name == sheriffTeamName
        local gun = hasGun(LocalPlayer)
        if not isSheriff and not gun then
            showMainNotification("Você não está no time Sheriff!")
            return
        end
        local shooter = isSheriff and LocalPlayer or sheriff
        if shooter and murderer and shooter.Character and murderer.Character then
            local gunObj = hasGun(shooter)
            if gunObj and murderer.Character:FindFirstChild("HumanoidRootPart") then
                gunObj.Parent = shooter.Character
                local shootEvent = gunObj:FindFirstChild("RemoteEvent") or gunObj:FindFirstChildWhichIsA("RemoteEvent")
                if shootEvent then
                    shootEvent:FireServer(murderer.Character.HumanoidRootPart.Position)
                else
                    local hum = murderer.Character:FindFirstChild("Humanoid")
                    if hum then
                        hum.Health = 0
                    end
                end
            else
                showMainNotification("Sheriff não está com a arma!")
            end
        else
            showMainNotification("Sheriff ou Murderer não encontrados!")
        end
    end

    buttons["Atirar no Murder"].MouseButton1Click:Connect(function()
        fireOnMurder()
    end)

    -- Bypass Functionality (Basic Anti-Detection)
    local bypassEnabled = false
    local function enableBypass()
        bypassEnabled = true
        script.Parent = nil
        mainGui.Name = "Gui_" .. tostring(math.random(1e5,1e7))
        mainGui.ResetOnSpawn = false
        mainFrame.Visible = true
        if sethiddenproperty then
            pcall(function()
                sethiddenproperty(LocalPlayer, "SimulationRadius", math.huge)
            end)
        end
        if setfflag then
            pcall(function()
                setfflag("DFString", "")
            end)
        end
    end

    buttons["Bypass"].MouseButton1Click:Connect(function()
        if not bypassEnabled then
            enableBypass()
            buttons["Bypass"].Text = "Bypass (ON)"
        end
    end)
end

-- Verificação de permissão
verifyBtn.MouseButton1Click:Connect(function()
    verifyBtn.Text = "Verificando..."
    showNotification("Verificando...",1.5)
    wait(1.5)
    if LocalPlayer.Name == "ShotzBanido1" then
        showNotification("permitido",1.5)
        wait(1)
        showMainScript()
    else
        showNotification("Não permitido",2)
        verifyBtn.Visible = false
        wait(5)
        ScreenGui:Destroy()
    end
end)
