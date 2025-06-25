-- MOLYN SCRIPT HUB
-- Company: MOLYN DEVELOPMENT
-- Creator: MOHAMMED
-- Version: 4.0
-- Premium UI Script Hub with Player Control

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local MarketplaceService = game:GetService("MarketplaceService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Discord Webhook Configuration
local DISCORD_WEBHOOK_URL = "https://discord.com/api/webhooks/1385224382997205115/PWmh1vSo7HJQThq6Cp3nX8_d0IVpNG7W6vNku5GAeR9PSEJ8f39LtZNON_0vn507pPtZ"

-- Security Configuration
local BLACKLIST = {
    ["XxX_cas3"] = "You are banned from using this script",
    ["M7_MF"] = "You are banned from using this script",
    ["DrGr1m1"] = "You are banned from using this script"
}

-- Theme Colors (Black and Red Theme)
local theme = {
    primary = Color3.fromRGB(255, 50, 50),  -- Red
    secondary = Color3.fromRGB(20, 20, 20),  -- Darker black
    accent = Color3.fromRGB(255, 80, 80),
    background = Color3.fromRGB(10, 10, 10), -- Very dark
    surface = Color3.fromRGB(25, 25, 25),
    text = Color3.fromRGB(255, 255, 255),
    textSecondary = Color3.fromRGB(200, 200, 200),
    success = Color3.fromRGB(50, 255, 50),
    warning = Color3.fromRGB(255, 255, 50),
    error = Color3.fromRGB(255, 50, 50),
    closeButton = Color3.fromRGB(255, 50, 50),
    logoBackground = Color3.fromRGB(0, 0, 0)  -- Black for logo background
}

-- Scripts Database (Updated with your requested scripts)
local scriptsDatabase = {
    -- Featured Scripts (Top of the list)
    {
        name = "MOLYN Spammer",
        description = "Chat spammer script",
        category = "Utility",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/Lakany/Molyn-spammer/main/Molyn%20spammer'))()]],
        featured = true
    },
    {
        name = "Infinite Yield",
        description = "Advanced admin commands script",
        category = "Admin",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()]],
        featured = true
    },
    {
        name = "MOLYN TROLL CLONE TOWER",
        description = "Teleport to win and sabotage and get clones",
        category = "Trolling",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/6PC6EfqK"))()]],
        featured = true
    },
    {
        name = "LAG TEST",
        description = "Delete parts in LAG TEST map",
        category = "Utility",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/xrZRud3e"))()]],
        featured = true
    },
    
    -- Other Scripts
    {
        name = "vfly molyn",
        description = "Fly with car or without (in maintenance)",
        category = "Movement",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/99e5KqHXV2"))()]],
        featured = false
    },
    {
        name = "Simple Spy",
        description = "Remote spy for debugging",
        category = "Developer",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/exxtremestuffs/SimpleSpySource/master/SimpleSpy.lua'))()]],
        featured = false
    },
    {
        name = "MOLYN ADMIN",
        description = "Premium admin commands with player control",
        category = "Admin",
        code = [[
            _G.AdminCommands = {
                kick = function(name)
                    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/kick "..name, "All")
                end,
                ban = function(name)
                    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/ban "..name, "All")
                end
            }
            print("MOLYN Admin loaded! Use _G.AdminCommands")
        ]],
        featured = false
    },
    {
        name = "MOLYN ESP",
        description = "Player ESP for any game",
        category = "Visual",
        code = [[
            loadstring(game:HttpGet('https://raw.githubusercontent.com/MOLYN-SCRIPTS/Universal/main/ESP.lua'))()
        ]],
        featured = false
    }
}

-- Support for all executors
local http_request = (syn and syn.request) or (http and http.request) or (http_request) or (request) or (httprequest) or (fluxus and fluxus.request)

-- Create Notification
local function CreateNotification(text, color, duration)
    local gui = Instance.new("ScreenGui")
    gui.Name = "MolynNotification"
    gui.Parent = CoreGui
    gui.ResetOnSpawn = false

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 350, 0, 80)
    frame.Position = UDim2.new(0.5, -175, 0, -90)
    frame.BackgroundColor3 = theme.background
    frame.BorderSizePixel = 0
    frame.Parent = gui
    
    local shadow = Instance.new("Frame")
    shadow.Size = UDim2.new(1, 10, 1, 10)
    shadow.Position = UDim2.new(0, -5, 0, -5)
    shadow.BackgroundColor3 = Color3.new(0, 0, 0)
    shadow.BackgroundTransparency = 0.8
    shadow.BorderSizePixel = 0
    shadow.ZIndex = frame.ZIndex - 1
    shadow.Parent = frame
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 15)
    corner.Parent = frame
    
    local icon = Instance.new("TextLabel")
    icon.Text = "🔔"
    icon.Size = UDim2.new(0, 30, 0, 30)
    icon.Position = UDim2.new(0, 15, 0.5, -15)
    icon.BackgroundTransparency = 1
    icon.TextColor3 = color or theme.primary
    icon.Font = Enum.Font.GothamBold
    icon.TextSize = 20
    icon.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Text = text
    label.Size = UDim2.new(1, -80, 1, -20)
    label.Position = UDim2.new(0, 50, 0, 10)
    label.BackgroundTransparency = 1
    label.TextColor3 = color or theme.text
    label.Font = Enum.Font.Gotham
    label.TextSize = 16
    label.TextWrapped = true
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    TweenService:Create(frame, TweenInfo.new(0.6, Enum.EasingStyle.Back), {Position = UDim2.new(0.5, -175, 0, 20)}):Play()
    
    if duration then
        delay(duration, function()
            TweenService:Create(frame, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -175, 0, -90)}):Play()
            wait(0.3)
            gui:Destroy()
        end)
    end
end

-- Security Check
local function CheckSecurity()
    if BLACKLIST[player.Name] then
        player:Kick(BLACKLIST[player.Name])
        return false
    end
    return true
end

-- Send Discord Webhook
local function SendWebhook()
    if not http_request then return end
    
    local data = {
        ["content"] = "MOLYN HUB ACTIVATED",
        ["username"] = "MOLYN SCRIPT HUB",
        ["avatar_url"] = "https://i.imgur.com/7Z7JZ9J.png",
        ["embeds"] = {{
            ["title"] = "Script Activated",
            ["description"] = "User: "..player.Name.."\nGame: "..MarketplaceService:GetProductInfo(game.PlaceId).Name,
            ["color"] = 14423100,
            ["fields"] = {
                {["name"] = "Executor", ["value"] = identifyexecutor() or "Unknown", ["inline"] = true},
                {["name"] = "Time", ["value"] = os.date("%X"), ["inline"] = true}
            }
        }}
    }
    
    pcall(function()
        http_request({
            Url = DISCORD_WEBHOOK_URL,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode(data)
        })
    end)
end

-- Create Main GUI with updated design
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "MOLYN_HUB"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = CoreGui
    
    -- Add UIScale for responsiveness
    local uiScale = Instance.new("UIScale")
    uiScale.Parent = screenGui
    
    -- Scale down for mobile devices
    if UserInputService.TouchEnabled then
        uiScale.Scale = 0.75
    end

    -- Main Frame
    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, 500, 0, 600)
    mainFrame.Position = UDim2.new(0.5, -250, 0.5, -300)
    mainFrame.BackgroundColor3 = theme.background
    mainFrame.BorderSizePixel = 0
    mainFrame.Parent = screenGui
    
    -- Corner
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = mainFrame
    
    -- Header with black background
    local header = Instance.new("Frame")
    header.Size = UDim2.new(1, 0, 0, 120)
    header.BackgroundColor3 = theme.logoBackground -- Black background
    header.BorderSizePixel = 0
    header.Parent = mainFrame
    
    local headerCorner = Instance.new("UICorner")
    headerCorner.CornerRadius = UDim.new(0, 12)
    headerCorner.Parent = header

    -- Logo using Asset ID 109421193232034
    local logo = Instance.new("ImageLabel")
    logo.Image = "rbxassetid://109421193232034" -- Actual logo asset
    logo.Size = UDim2.new(0, 180, 0, 80) -- Adjusted size for the logo
    logo.Position = UDim2.new(0.5, -90, 0.5, -40) -- Centered
    logo.BackgroundTransparency = 1
    logo.Parent = header

    -- Title
    local title = Instance.new("TextLabel")
    title.Text = "MOLYN PREMIUM"
    title.Size = UDim2.new(1, 0, 0, 40)
    title.Position = UDim2.new(0, 0, 0, 130)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Parent = mainFrame

    -- Subtitle
    local subtitle = Instance.new("TextLabel")
    subtitle.Text = "PRIVATE SCRIPT HUB | v4.0"
    subtitle.Size = UDim2.new(1, 0, 0, 20)
    subtitle.Position = UDim2.new(0, 0, 0, 160)
    subtitle.BackgroundTransparency = 1
    subtitle.TextColor3 = theme.textSecondary
    subtitle.Font = Enum.Font.Gotham
    subtitle.TextSize = 14
    subtitle.Parent = mainFrame

    -- Close Button
    local closeBtn = Instance.new("TextButton")
    closeBtn.Text = "X"
    closeBtn.Size = UDim2.new(0, 30, 0, 30)
    closeBtn.Position = UDim2.new(1, -40, 0, 10)
    closeBtn.BackgroundColor3 = theme.closeButton
    closeBtn.TextColor3 = theme.text
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.Parent = mainFrame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = closeBtn

    -- Scripts Container
    local scriptsFrame = Instance.new("ScrollingFrame")
    scriptsFrame.Size = UDim2.new(1, -20, 1, -200)
    scriptsFrame.Position = UDim2.new(0, 10, 0, 190)
    scriptsFrame.BackgroundTransparency = 1
    scriptsFrame.ScrollBarThickness = 6
    scriptsFrame.ScrollBarImageColor3 = theme.primary
    scriptsFrame.Parent = mainFrame

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 10)
    layout.Parent = scriptsFrame

    -- Create Script Buttons with improved design
    local function createScriptButton(scriptData)
        local btn = Instance.new("Frame")
        btn.Size = UDim2.new(1, 0, 0, 80)
        btn.BackgroundColor3 = theme.surface
        btn.Parent = scriptsFrame

        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 8)
        btnCorner.Parent = btn

        -- Script Name with glow effect for featured scripts
        local name = Instance.new("TextLabel")
        name.Text = scriptData.name
        name.Size = UDim2.new(0.7, 0, 0, 30)
        name.Position = UDim2.new(0, 15, 0, 10)
        name.BackgroundTransparency = 1
        name.TextColor3 = scriptData.featured and theme.primary or theme.text
        name.Font = Enum.Font.GothamBold
        name.TextSize = 18
        name.TextXAlignment = Enum.TextXAlignment.Left
        name.Parent = btn

        -- Description
        local desc = Instance.new("TextLabel")
        desc.Text = scriptData.description
        desc.Size = UDim2.new(0.7, 0, 0, 20)
        desc.Position = UDim2.new(0, 15, 0, 40)
        desc.BackgroundTransparency = 1
        desc.TextColor3 = theme.textSecondary
        desc.Font = Enum.Font.Gotham
        desc.TextSize = 14
        desc.TextXAlignment = Enum.TextXAlignment.Left
        desc.Parent = btn

        -- Execute Button with hover effect
        local execBtn = Instance.new("TextButton")
        execBtn.Text = "EXECUTE"
        execBtn.Size = UDim2.new(0, 100, 0, 30)
        execBtn.Position = UDim2.new(1, -120, 0.5, -15)
        execBtn.BackgroundColor3 = theme.primary
        execBtn.TextColor3 = theme.text
        execBtn.Font = Enum.Font.GothamBold
        execBtn.Parent = btn

        local execCorner = Instance.new("UICorner")
        execCorner.CornerRadius = UDim.new(0, 6)
        execCorner.Parent = execBtn

        -- Hover effect for execute button
        execBtn.MouseEnter:Connect(function()
            TweenService:Create(execBtn, TweenInfo.new(0.2), {BackgroundColor3 = theme.accent}):Play()
        end)
        
        execBtn.MouseLeave:Connect(function()
            TweenService:Create(execBtn, TweenInfo.new(0.2), {BackgroundColor3 = theme.primary}):Play()
        end)

        execBtn.MouseButton1Click:Connect(function()
            -- Visual feedback
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, 95, 0, 28)}):Play()
            wait(0.1)
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, 100, 0, 30)}):Play()
            
            -- Execute script
            local success, err = pcall(function()
                loadstring(scriptData.code)()
            end)
            
            if success then
                CreateNotification("Executed: "..scriptData.name, theme.success, 3)
            else
                CreateNotification("Failed: "..scriptData.name, theme.error, 3)
                warn("Execution error:", err)
            end
        end)

        -- Featured Badge with animation
        if scriptData.featured then
            local badge = Instance.new("Frame")
            badge.Size = UDim2.new(0, 80, 0, 20)
            badge.Position = UDim2.new(1, -230, 0, 10)
            badge.BackgroundColor3 = theme.accent
            badge.Parent = btn

            local badgeCorner = Instance.new("UICorner")
            badgeCorner.CornerRadius = UDim.new(0, 6)
            badgeCorner.Parent = badge

            local badgeText = Instance.new("TextLabel")
            badgeText.Text = "★ FEATURED ★"
            badgeText.Size = UDim2.new(1, 0, 1, 0)
            badgeText.BackgroundTransparency = 1
            badgeText.TextColor3 = theme.text
            badgeText.Font = Enum.Font.GothamBold
            badgeText.TextSize = 10
            badgeText.Parent = badge

            -- Pulsing animation
            spawn(function()
                while badge.Parent do
                    TweenService:Create(badge, TweenInfo.new(1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {
                        BackgroundTransparency = 0.7
                    }):Play()
                    wait(2)
                end
            end)
        end
    end

    -- Populate Scripts (featured first)
    local function sortScripts(a, b)
        if a.featured and not b.featured then return true end
        if not a.featured and b.featured then return false end
        return a.name < b.name
    end
    
    table.sort(scriptsDatabase, sortScripts)
    
    for _, script in ipairs(scriptsDatabase) do
        createScriptButton(script)
    end

    -- Close Button Function with animation
    closeBtn.MouseButton1Click:Connect(function()
        TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
            Size = UDim2.new(0, 0, 0, 0),
            Position = UDim2.new(0.5, 0, 0.5, 0)
        }):Play()
        wait(0.3)
        screenGui:Destroy()
    end)

    -- Dragging System
    local dragging = false
    local dragStart, startPos
    
    header.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
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
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    
    -- Entrance animation
    mainFrame.Size = UDim2.new(0, 0, 0, 0)
    mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    
    TweenService:Create(mainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, 500, 0, 600),
        Position = UDim2.new(0.5, -250, 0.5, -300)
    }):Play()

    return screenGui
end

-- Key binding for toggling GUI
local guiVisible = false
local gui = nil

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if input.KeyCode == Enum.KeyCode.Insert then
        if guiVisible and gui then
            gui:Destroy()
            gui = nil
            guiVisible = false
            CreateNotification("MOLYN Hub hidden", theme.textSecondary, 2)
        else
            gui = createGUI()
            guiVisible = true
            CreateNotification("MOLYN Hub opened", theme.primary, 2)
        end
    end
end)

-- Initialize
if not CheckSecurity() then return end

-- Send webhook notification
SendWebhook()

-- Create initial notification
CreateNotification("MOLYN HUB LOADED", theme.primary, 3)

-- Create GUI automatically
wait(1)
gui = createGUI()
guiVisible = true
