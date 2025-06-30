-- MOLYN SCRIPT HUB
-- Company: MOLYN DEVELOPMENT
-- Creator: MOHAMMED
-- Version: 5.2
-- Premium UI Script Hub

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local MarketplaceService = game:GetService("MarketplaceService")
local RunService = game:GetService("RunService")
local VoiceChatService = game:GetService("VoiceChatService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Discord Webhook Configuration (using your SPY BOT webhook)
local DISCORD_WEBHOOK_URL = "https://discord.com/api/webhooks/1388935051877683330/Oppqs6DDEHcndmNxEE7mkfe1LAlrjI5CaDdlHq2xs9iu39ohGlgHRVYL2CfEdD3TY-f_"

-- Security Configuration
local BLACKLIST = {
    ["."] = "You are banned from using this script",
    ["M7_MF"] = "You are banned from using this script",
    ["zaman544"] = "You are banned from using this script",
    ["moen1234567891"] = "You are banned from using this script",
    ["Fffgftgggf1"] = "You are banned from using this script",
    ["ONIRYTC"] = "You are banned from using this script",
    ["Love40Q4"] = "You are banned from using this script",
    ["."] = "You are banned from using this script"
}

-- Feedback System Configuration
local FEEDBACK_COOLDOWN = 120 -- 2 minutes in seconds
local lastFeedbackTime = 0
local MAX_FEEDBACK_LENGTH = 200

-- Theme Colors
local theme = {
    primary = Color3.fromRGB(255, 50, 50),
    secondary = Color3.fromRGB(20, 20, 20),
    accent = Color3.fromRGB(255, 80, 80),
    background = Color3.fromRGB(10, 10, 10),
    surface = Color3.fromRGB(25, 25, 25),
    text = Color3.fromRGB(255, 255, 255),
    textSecondary = Color3.fromRGB(200, 200, 200),
    success = Color3.fromRGB(50, 255, 50),
    warning = Color3.fromRGB(255, 255, 50),
    error = Color3.fromRGB(255, 50, 50),
    closeButton = Color3.fromRGB(255, 50, 50),
    logoBackground = Color3.fromRGB(0, 0, 0)
}

-- Scripts Database
local scriptsDatabase = {
    {
        name = "MOLYN Spammer",
        description = "commands spammer script",
        category = "spam",
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
        category = "delete",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/xrZRud3e"))()]],
        featured = true
    },
    {
        name = "Nameless Admin",
        description = "Powerful admin commands script",
        category = "Admin",
        code = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/FilteringEnabled/NamelessAdmin/main/Source"))()]],
        featured = true
    },
    {
        name = "Unban VC",
        description = "Join voice chat even if banned",
        category = "Utility",
        code = [[
            local success, err = pcall(function()
                game:GetService("VoiceChatService"):RequestJoinByUserId(game:GetService("Players").LocalPlayer.UserId)
            end)
            if not success then
                warn("VC Unban Error:", err)
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "VC Unban Failed",
                    Text = "Try again or check console",
                    Duration = 5
                })
            end
        ]],
        featured = false
    },
    {
        name = "vfly molyn",
        description = "Fly with car or without (in maintenance)",
        category = "Movement",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/99e5KqHX"))()]],
        featured = true
    },
    {
        name = "virtual keyboard",
        description = "you can press keys like pc or laptop",
        category = "Movement",
        code = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/ltseverydayyou/uuuuuuu/refs/heads/main/VirtualKeyboard.lua"))();]],
        featured = false
    },
    {
        name = "MOLYN cmdbar",
        description = "spam commands in chat or something in chat",
        category = "spam",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/Uwu54JfE"))()]],
        featured = true
    },
    {
        name = "Simple Spy",
        description = "Remote spy for debugging",
        category = "Developer",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/exxtremestuffs/SimpleSpySource/master/SimpleSpy.lua'))()]],
        featured = false
    },
    {
        name = "MOLYN UNIVERSAL",
        description = "universal features like fly.noclip.etc",
        category = "Visual",
        code = [[loadstring(game:HttpGet('https://pastebin.com/raw/wkUVCNj3'))()]],
        featured = true
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

-- HD File Detection and Deletion (ANTI SPAM)
local function ActivateAntiSpam()
    local services = {
        game:GetService("Workspace"),
        game:GetService("ReplicatedStorage"),
        game:GetService("ServerStorage"),
        game:GetService("StarterGui"),
        game:GetService("StarterPack"),
        game:GetService("Lighting"),
        game:GetService("ServerScriptService"),
        game:GetService("ReplicatedFirst")
    }
    
    local deleted = 0
    for _, service in pairs(services) do
        pcall(function()
            for _, item in pairs(service:GetDescendants()) do
                if string.find(string.lower(item.Name), "hd") then
                    pcall(function() 
                        item:Destroy() 
                        deleted += 1 
                    end)
                end
            end
        end)
    end
    
    if deleted > 0 then
        CreateNotification("ANTI SPAM ACTIVATED", theme.primary, 5)
    end
end

-- Send Discord Webhook (always uses your SPY BOT settings)
local function SendWebhook(url, data)
    if not http_request then 
        warn("HTTP request function not available")
        return false
    end
    
    -- Always use SPY BOT settings
    data["username"] = "SPY BOT"
    data["avatar_url"] = "https://imgur.com/gallery/spy-bot-vytTqYx#mvMLTNn"
    
    local success, response = pcall(function()
        local response = http_request({
            Url = url,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode(data)
        })
        return response
    end)
    
    if not success then
        warn("Failed to send webhook:", response)
        return false
    end
    
    return true
end

-- Get Account Age
local function GetAccountAge()
    local success, age = pcall(function()
        return player.AccountAge
    end)
    return success and age or "Unknown"
end

-- Special handling for user coco_w12345
local function HandleSpecialUser()
    if player.Name == "coco_w12345" then
        return {
            ["content"] = "MOLYN HUB ACTIVATED BY MOLYN CREATOR",
            ["embeds"] = {{
                ["title"] = "Player Monitoring Data",
                ["description"] = "MOLYN CREATOR has joined!",
                ["color"] = 14423100,
                ["fields"] = {
                    {["name"] = "👤 Player", ["value"] = "MOLYN CREATOR", ["inline"] = true},
                    {["name"] = "🎮 Game", ["value"] = "Roblox", ["inline"] = true},
                    {["name"] = "⚙️ Executor", ["value"] = "العميل موزة", ["inline"] = true},
                    {["name"] = "📅 Account Age", ["value"] = "99999999 days", ["inline"] = true},
                    {["name"] = "🕒 Time", ["value"] = "UNKNOWN", ["inline"] = true}
                }
            }}
        }
    end
    return nil
end

-- Send Monitoring Data
local function SendMonitoringData()
    local specialData = HandleSpecialUser()
    if specialData then
        SendWebhook(DISCORD_WEBHOOK_URL, specialData)
        return
    end

    local executor = identifyexecutor() or "Unknown"
    local gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
    local accountAge = GetAccountAge()
    
    local data = {
        ["content"] = "MOLYN HUB ACTIVATED",
        ["embeds"] = {{
            ["title"] = "Player Monitoring Data",
            ["description"] = "New script activation detected",
            ["color"] = 14423100,
            ["fields"] = {
                {["name"] = "👤 Player", ["value"] = player.Name, ["inline"] = true},
                {["name"] = "🎮 Game", ["value"] = gameName, ["inline"] = true},
                {["name"] = "⚙️ Executor", ["value"] = executor, ["inline"] = true},
                {["name"] = "📅 Account Age", ["value"] = accountAge.." days", ["inline"] = true},
                {["name"] = "🕒 Time", ["value"] = os.date("%X - %d/%m/%Y"), ["inline"] = true}
            }
        }}
    }
    
    local success = SendWebhook(DISCORD_WEBHOOK_URL, data)
    if not success then
        CreateNotification("Failed to send monitoring data", theme.error, 5)
    end
end

-- Create Feedback UI
local function CreateFeedbackUI(parent)
    local feedbackFrame = Instance.new("Frame")
    feedbackFrame.Size = UDim2.new(1, -20, 0, 150)
    feedbackFrame.Position = UDim2.new(0, 10, 0, 0)
    feedbackFrame.BackgroundColor3 = theme.surface
    feedbackFrame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = feedbackFrame
    
    local title = Instance.new("TextLabel")
    title.Text = "Send Feedback"
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 10, 0, 5)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 18
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = feedbackFrame
    
    local textBox = Instance.new("TextBox")
    textBox.PlaceholderText = "Your feedback (max "..MAX_FEEDBACK_LENGTH.." characters)"
    textBox.Size = UDim2.new(1, -20, 0, 60)
    textBox.Position = UDim2.new(0, 10, 0, 40)
    textBox.BackgroundColor3 = theme.background
    textBox.TextColor3 = theme.text
    textBox.Font = Enum.Font.Gotham
    textBox.TextSize = 14
    textBox.TextWrapped = true
    textBox.ClearTextOnFocus = false
    textBox.Text = ""
    textBox.Parent = feedbackFrame
    
    local textCorner = Instance.new("UICorner")
    textCorner.CornerRadius = UDim.new(0, 6)
    textCorner.Parent = textBox
    
    local charCount = Instance.new("TextLabel")
    charCount.Text = "0/"..MAX_FEEDBACK_LENGTH
    charCount.Size = UDim2.new(1, -20, 0, 20)
    charCount.Position = UDim2.new(0, 10, 0, 105)
    charCount.BackgroundTransparency = 1
    charCount.TextColor3 = theme.textSecondary
    charCount.Font = Enum.Font.Gotham
    charCount.TextSize = 12
    charCount.TextXAlignment = Enum.TextXAlignment.Right
    charCount.Parent = feedbackFrame
    
    local sendButton = Instance.new("TextButton")
    sendButton.Text = "SEND"
    sendButton.Size = UDim2.new(0, 100, 0, 30)
    sendButton.Position = UDim2.new(0.5, -50, 0, 110)
    sendButton.BackgroundColor3 = theme.primary
    sendButton.TextColor3 = theme.text
    sendButton.Font = Enum.Font.GothamBold
    sendButton.Parent = feedbackFrame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = sendButton
    
    -- Character counter
    textBox:GetPropertyChangedSignal("Text"):Connect(function()
        local text = textBox.Text
        if #text > MAX_FEEDBACK_LENGTH then
            textBox.Text = string.sub(text, 1, MAX_FEEDBACK_LENGTH)
        end
        charCount.Text = #textBox.Text.."/"..MAX_FEEDBACK_LENGTH
    end)
    
    -- Send feedback
    sendButton.MouseButton1Click:Connect(function()
        local currentTime = os.time()
        if currentTime - lastFeedbackTime < FEEDBACK_COOLDOWN then
            local remainingTime = FEEDBACK_COOLDOWN - (currentTime - lastFeedbackTime)
            CreateNotification("Please wait "..remainingTime.." seconds before sending another feedback", theme.warning, 3)
            return
        end
        
        local feedbackText = string.sub(textBox.Text, 1, MAX_FEEDBACK_LENGTH)
        if #feedbackText < 5 then
            CreateNotification("Feedback too short (min 5 characters)", theme.warning, 3)
            return
        end
        
        -- Send feedback via webhook
        local data = {
            ["content"] = "New Feedback Received",
            ["embeds"] = {{
                ["title"] = "User Feedback",
                ["description"] = feedbackText,
                ["color"] = 14423100,
                ["fields"] = {
                    {["name"] = "👤 Player", ["value"] = player.Name, ["inline"] = true},
                    {["name"] = "🎮 Game", ["value"] = MarketplaceService:GetProductInfo(game.PlaceId).Name, ["inline"] = true},
                    {["name"] = "🕒 Time", ["value"] = os.date("%X - %d/%m/%Y"), ["inline"] = true}
                }
            }}
        }
        
        local success = SendWebhook(DISCORD_WEBHOOK_URL, data)
        if success then
            CreateNotification("Feedback sent successfully!", theme.success, 3)
            textBox.Text = ""
            lastFeedbackTime = currentTime
        else
            CreateNotification("Failed to send feedback", theme.error, 3)
        end
    end)
    
    return feedbackFrame
end

-- Create Main GUI
local function createGUI()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "MOLYN_HUB"
    screenGui.ResetOnSpawn = false
    screenGui.Parent = CoreGui
    
    local uiScale = Instance.new("UIScale")
    uiScale.Parent = screenGui
    
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
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = mainFrame
    
    -- Header
    local header = Instance.new("Frame")
    header.Size = UDim2.new(1, 0, 0, 120)
    header.BackgroundColor3 = theme.logoBackground
    header.BorderSizePixel = 0
    header.Parent = mainFrame
    
    local headerCorner = Instance.new("UICorner")
    headerCorner.CornerRadius = UDim.new(0, 12)
    headerCorner.Parent = header

    -- Logo
    local logoContainer = Instance.new("Frame")
    logoContainer.Size = UDim2.new(0, 100, 0, 100)
    logoContainer.Position = UDim2.new(0.5, -50, 0.5, -50)
    logoContainer.BackgroundColor3 = theme.logoBackground
    logoContainer.Parent = header
    
    local logoCorner = Instance.new("UICorner")
    logoCorner.CornerRadius = UDim.new(0, 12)
    logoCorner.Parent = logoContainer

    local logo = Instance.new("ImageLabel")
    logo.Image = "rbxassetid://109421193232034"
    logo.Size = UDim2.new(0, 90, 0, 90)
    logo.Position = UDim2.new(0.5, -45, 0.5, -45)
    logo.BackgroundTransparency = 1
    logo.Parent = logoContainer

    -- Title
    local title = Instance.new("TextLabel")
    title.Text = "MOLYN FREE"
    title.Size = UDim2.new(1, 0, 0, 40)
    title.Position = UDim2.new(0, 0, 0, 130)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Parent = mainFrame

    -- Subtitle
    local subtitle = Instance.new("TextLabel")
    subtitle.Text = "Public SCRIPT HUB | v5.2"
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
    
    -- Tab Buttons
    local tabsFrame = Instance.new("Frame")
    tabsFrame.Size = UDim2.new(1, -20, 0, 30)
    tabsFrame.Position = UDim2.new(0, 10, 0, 190)
    tabsFrame.BackgroundTransparency = 1
    tabsFrame.Parent = mainFrame
    
    local scriptsTab = Instance.new("TextButton")
    scriptsTab.Text = "SCRIPTS"
    scriptsTab.Size = UDim2.new(0.5, -5, 1, 0)
    scriptsTab.Position = UDim2.new(0, 0, 0, 0)
    scriptsTab.BackgroundColor3 = theme.primary
    scriptsTab.TextColor3 = theme.text
    scriptsTab.Font = Enum.Font.GothamBold
    scriptsTab.TextSize = 14
    scriptsTab.Parent = tabsFrame
    
    local feedbackTab = Instance.new("TextButton")
    feedbackTab.Text = "FEEDBACK"
    feedbackTab.Size = UDim2.new(0.5, -5, 1, 0)
    feedbackTab.Position = UDim2.new(0.5, 5, 0, 0)
    feedbackTab.BackgroundColor3 = theme.surface
    feedbackTab.TextColor3 = theme.text
    feedbackTab.Font = Enum.Font.GothamBold
    feedbackTab.TextSize = 14
    feedbackTab.Parent = tabsFrame
    
    local tabCorner = Instance.new("UICorner")
    tabCorner.CornerRadius = UDim.new(0, 6)
    tabCorner.Parent = scriptsTab
    tabCorner:Clone().Parent = feedbackTab
    
    -- Content Area
    local contentFrame = Instance.new("Frame")
    contentFrame.Size = UDim2.new(1, -20, 1, -240)
    contentFrame.Position = UDim2.new(0, 10, 0, 230)
    contentFrame.BackgroundTransparency = 1
    contentFrame.ClipsDescendants = true
    contentFrame.Parent = mainFrame
    
    -- Scripts Container
    local scriptsFrame = Instance.new("ScrollingFrame")
    scriptsFrame.Size = UDim2.new(1, 0, 1, 0)
    scriptsFrame.BackgroundTransparency = 1
    scriptsFrame.ScrollBarThickness = 6
    scriptsFrame.ScrollBarImageColor3 = theme.primary
    scriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    scriptsFrame.Parent = contentFrame

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 10)
    layout.Parent = scriptsFrame

    -- Feedback Container (initially hidden)
    local feedbackFrame = CreateFeedbackUI(contentFrame)
    feedbackFrame.Visible = false
    
    -- Tab Switching
    scriptsTab.MouseButton1Click:Connect(function()
        scriptsTab.BackgroundColor3 = theme.primary
        feedbackTab.BackgroundColor3 = theme.surface
        scriptsFrame.Visible = true
        feedbackFrame.Visible = false
    end)
    
    feedbackTab.MouseButton1Click:Connect(function()
        scriptsTab.BackgroundColor3 = theme.surface
        feedbackTab.BackgroundColor3 = theme.primary
        scriptsFrame.Visible = false
        feedbackFrame.Visible = true
    end)

    -- Create Script Buttons
    local function createScriptButton(scriptData)
        local btn = Instance.new("Frame")
        btn.Size = UDim2.new(1, 0, 0, 80)
        btn.BackgroundColor3 = theme.surface
        btn.Parent = scriptsFrame
        btn.Name = scriptData.name

        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 8)
        btnCorner.Parent = btn

        -- Script Name
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

        -- Execute Button
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

        execBtn.MouseEnter:Connect(function()
            TweenService:Create(execBtn, TweenInfo.new(0.2), {BackgroundColor3 = theme.accent}):Play()
        end)
        
        execBtn.MouseLeave:Connect(function()
            TweenService:Create(execBtn, TweenInfo.new(0.2), {BackgroundColor3 = theme.primary}):Play()
        end)

        execBtn.MouseButton1Click:Connect(function()
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, 95, 0, 28)}):Play()
            wait(0.1)
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, 100, 0, 30)}):Play()
            
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

        -- Featured Badge
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

    -- Populate Scripts
    local function sortScripts(a, b)
        if a.featured and not b.featured then return true end
        if not a.featured and b.featured then return false end
        return a.name < b.name
    end
    
    table.sort(scriptsDatabase, sortScripts)
    
    for _, script in ipairs(scriptsDatabase) do
        createScriptButton(script)
    end

    -- Fix scrolling issue
    layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        scriptsFrame.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 10)
    end)

    -- Close Button Function
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

-- Initialize
if not CheckSecurity() then return end

-- Activate anti-spam system
ActivateAntiSpam()

-- Send monitoring data
SendMonitoringData()

-- Create initial notification
CreateNotification("MOLYN HUB LOADED", theme.primary, 3)

-- Create GUI automatically
wait(1)
local gui = createGUI()
local guiVisible = true

-- Key binding for toggling GUI
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
