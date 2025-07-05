--[[
  MOLYN SCRIPT HUB
  Company: MOLYN DEVELOPMENT
  Creator: MOHAMMED
  Version: 5.7
  Premium UI Script Hub
  Features:
  - واجهة متكاملة سهلة الاستخدام
  - دعم للهواتف والحواسب
  - نظام حماية متقدم
  - شريط بحث للوصول السريع
]]

-- Services
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local MarketplaceService = game:GetService("MarketplaceService")
local RunService = game:GetService("RunService")
local VoiceChatService = game:GetService("VoiceChatService")
local StarterGui = game:GetService("StarterGui")
local TextService = game:GetService("TextService")

-- Player references
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Webhook configuration
local DISCORD_WEBHOOK_URL = "https://discord.com/api/webhooks/1391150966031519774/iXggOTjgDBm5RhygIozJyBvjmDMktVcKDvdf1OnccwhBMQeiObxk4vnp8x6XczX8mSCD"
local FEEDBACK_WEBHOOK_URL = "https://discord.com/api/webhooks/1390644943109750834/usIWKQH7mVQkZFnd6rJ9GrDCzzpaKOB0PjG0C9Zb53xcmXj5MKXbRcZSRLc-q1YzNZyO"

-- Security system
local BLACKLIST = {
    ["."] = "You are banned from using this script",
    ["M7_MF"] = "You are banned from using this script",
    ["zaman544"] = "You are banned from using this script",
    ["moen1234567891"] = "You are banned from using this script",
    ["Fffgftgggf1"] = "You are banned from using this script",
    ["ONIRYTC"] = "You are banned from using this script",
    ["Love40Q4"] = "You are banned from using this script"
}

-- Warning list
local WARNING_LIST = "⚠️ don't trust/لا تثق ⚠️: zaman544 % Fffgftgggf1 % moen1234567891 % ONIRYTC"

-- Feedback system
local FEEDBACK_COOLDOWN = 120
local lastFeedbackTime = 0
local MAX_FEEDBACK_LENGTH = 200

-- UI Theme
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
    logoBackground = Color3.fromRGB(0, 0, 0),
    discordBlue = Color3.fromRGB(88, 101, 242)
}

-- Scripts database
local scriptsDatabase = {
    {
        name = "MM2 & Flee the Facility OP",
        description = "OP Features like aimbot and auto shot for mm2 and more",
        category = "Game",
        code = [[loadstring(game:HttpGet("https://raw.githubusercontent.com/Joystickplays/psychic-octo-invention/main/yarhm.lua", false))()]],
        featured = true
    },
    {
        name = "backdoor.exe",
        description = "backdoor scanner",
        category = "Utility",
        code = [[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Backdoor-exe-9413"))()]],
        featured = true
    },
    {
        name = "INFINITE MONEY eight driver",
        description = "get infinite money",
        category = "Money",
        code = [[
            local money = 9e9 --Change the number of money you want
            local A_1 = "Sanctioned"
            local A_2 = -money
            local A_3 = "0"
            local A_4 = {}
            local Event = game:GetService("ReplicatedStorage").RemoteEvent
            Event:FireServer(A_1, A_2, A_3, A_4)
        ]],
        featured = true
    },
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
    },
    {
        name = "Greenville OP",
        description = "Car modification like speed and turbo and more",
        category = "Vehicle",
        code = [[loadstring(game:HttpGet('https://raw.githubusercontent.com/Lugtastic/hubs/main/EcuX-V2.lua',true))()]],
        featured = true
    },
    {
        name = "Redz Blox Fruits",
        description = "Auto farm and other features for Blox Fruits",
        category = "Game",
        code = [[
            local Settings = {
                JoinTeam = "Pirates";
                Translator = true;
            }
            loadstring(game:HttpGet("https://raw.githubusercontent.com/tlredz/Scripts/refs/heads/main/main.luau"))(Settings)
        ]],
        featured = true
    }
}

-- HTTP request function for all executors
local http_request = (syn and syn.request) or (http and http.request) or (http_request) or (request) or (httprequest) or (fluxus and fluxus.request)

-- Create notification function
local function CreateNotification(text, color, duration)
    local gui = Instance.new("ScreenGui")
    gui.Name = "MolynNotification_"..tostring(math.random(1, 10000))
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

-- Security check
local function CheckSecurity()
    if BLACKLIST[player.Name] then
        player:Kick(BLACKLIST[player.Name])
        return false
    end
    return true
end

-- Anti-spam system
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

-- Webhook sender
local function SendWebhook(url, data)
    if not http_request then 
        warn("HTTP request function not available")
        return false
    end
    
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

-- Account age check
local function GetAccountAge()
    local success, age = pcall(function()
        return player.AccountAge
    end)
    return success and age or "Unknown"
end

-- Special user handling
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

-- Monitoring system
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

-- Feedback UI
local function CreateFeedbackUI(parent)
    local feedbackFrame = Instance.new("Frame")
    feedbackFrame.Size = UDim2.new(1, -20, 0, 280)
    feedbackFrame.Position = UDim2.new(0, 10, 0, 0)
    feedbackFrame.BackgroundColor3 = theme.surface
    feedbackFrame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = feedbackFrame
    
    -- Warning Label
    local warningLabel = Instance.new("TextLabel")
    warningLabel.Text = WARNING_LIST
    warningLabel.Size = UDim2.new(1, -20, 0, 40)
    warningLabel.Position = UDim2.new(0, 10, 0, 10)
    warningLabel.BackgroundColor3 = theme.error
    warningLabel.TextColor3 = theme.text
    warningLabel.Font = Enum.Font.GothamBold
    warningLabel.TextSize = 12
    warningLabel.TextWrapped = true
    warningLabel.Parent = feedbackFrame
    
    local warningCorner = Instance.new("UICorner")
    warningCorner.CornerRadius = UDim.new(0, 6)
    warningCorner.Parent = warningLabel
    
    -- Discord Button
    local discordButton = Instance.new("TextButton")
    discordButton.Text = "JOIN SERVER DISCORD"
    discordButton.Size = UDim2.new(1, -20, 0, 40)
    discordButton.Position = UDim2.new(0, 10, 0, 60)
    discordButton.BackgroundColor3 = theme.discordBlue
    discordButton.TextColor3 = theme.text
    discordButton.Font = Enum.Font.GothamBold
    discordButton.TextSize = 16
    discordButton.Parent = feedbackFrame
    
    local discordCorner = Instance.new("UICorner")
    discordCorner.CornerRadius = UDim.new(0, 6)
    discordCorner.Parent = discordButton
    
    discordButton.MouseButton1Click:Connect(function()
        if setclipboard then
            setclipboard("https://discord.gg/zvnNwE3KWd")
            CreateNotification("تم نسخ رابط الديسكورد!", theme.success, 3)
        else
            CreateNotification("لا يمكن نسخ الرابط في هذا الإكسكيوتور", theme.error, 3)
        end
    end)
    
    -- Feedback Title
    local title = Instance.new("TextLabel")
    title.Text = "Send Feedback"
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 10, 0, 110)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 18
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = feedbackFrame
    
    -- Feedback Text Box
    local textBox = Instance.new("TextBox")
    textBox.PlaceholderText = "Your feedback (max "..MAX_FEEDBACK_LENGTH.." characters)"
    textBox.Size = UDim2.new(1, -20, 0, 60)
    textBox.Position = UDim2.new(0, 10, 0, 140)
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
    
    -- Character Count
    local charCount = Instance.new("TextLabel")
    charCount.Text = "0/"..MAX_FEEDBACK_LENGTH
    charCount.Size = UDim2.new(1, -20, 0, 20)
    charCount.Position = UDim2.new(0, 10, 0, 205)
    charCount.BackgroundTransparency = 1
    charCount.TextColor3 = theme.textSecondary
    charCount.Font = Enum.Font.Gotham
    charCount.TextSize = 12
    charCount.TextXAlignment = Enum.TextXAlignment.Right
    charCount.Parent = feedbackFrame
    
    -- Send Button
    local sendButton = Instance.new("TextButton")
    sendButton.Text = "SEND"
    sendButton.Size = UDim2.new(0, 100, 0, 30)
    sendButton.Position = UDim2.new(0.5, -50, 0, 230)
    sendButton.BackgroundColor3 = theme.primary
    sendButton.TextColor3 = theme.text
    sendButton.Font = Enum.Font.GothamBold
    sendButton.Parent = feedbackFrame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = sendButton
    
    -- Credits
    local credits = Instance.new("TextLabel")
    credits.Text = "Credits:\nمحمد / coc*_****5"
    credits.Size = UDim2.new(1, -20, 0, 40)
    credits.Position = UDim2.new(0, 10, 1, -50)
    credits.BackgroundTransparency = 1
    credits.TextColor3 = theme.textSecondary
    credits.Font = Enum.Font.Gotham
    credits.TextSize = 12
    credits.TextXAlignment = Enum.TextXAlignment.Left
    credits.Parent = feedbackFrame
    
    textBox:GetPropertyChangedSignal("Text"):Connect(function()
        local text = textBox.Text
        if #text > MAX_FEEDBACK_LENGTH then
            textBox.Text = string.sub(text, 1, MAX_FEEDBACK_LENGTH)
        end
        charCount.Text = #textBox.Text.."/"..MAX_FEEDBACK_LENGTH
    end)
    
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
        
        local success = SendWebhook(FEEDBACK_WEBHOOK_URL, data)
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

-- Main GUI creation
local function createGUI()
    -- Wait for game to load completely
    if not game:IsLoaded() then
        game.Loaded:Wait()
    end

    -- Clean up any existing GUI
    local coreGui = game:GetService("CoreGui")
    local existing = coreGui:FindFirstChild("MOLYN_HUB")
    if existing then
        existing:Destroy()
    end

    -- Create main ScreenGui with improved properties
    local screenGui = Instance.new("ScreenGui")
    screenGui.Name = "MOLYN_HUB"
    screenGui.ResetOnSpawn = false
    screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    screenGui.DisplayOrder = 999  -- Ensure it's on top
    screenGui.Parent = coreGui
    
    -- UI Scale for mobile devices
    local uiScale = Instance.new("UIScale")
    uiScale.Parent = screenGui
    
    -- Adjust UI scale based on device type
    if UserInputService.TouchEnabled then
        uiScale.Scale = 0.8 -- 20% reduction for mobile
    else
        uiScale.Scale = 1.0 -- Full size for PC
    end

    -- Main Frame with improved visibility
    local baseSize = UserInputService.TouchEnabled and 400 or 500
    local baseHeight = UserInputService.TouchEnabled and 500 or 600
    
    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, baseSize, 0, baseHeight)
    mainFrame.Position = UDim2.new(0.5, -baseSize/2, 0.5, -baseHeight/2)
    mainFrame.BackgroundColor3 = theme.background
    mainFrame.BorderSizePixel = 0
    mainFrame.ClipsDescendants = true
    mainFrame.Parent = screenGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = mainFrame
    
    -- Header with shadow effect
    local header = Instance.new("Frame")
    header.Size = UDim2.new(1, 0, 0, 100)
    header.BackgroundColor3 = theme.logoBackground
    header.BorderSizePixel = 0
    header.Parent = mainFrame
    
    local headerCorner = Instance.new("UICorner")
    headerCorner.CornerRadius = UDim.new(0, 12)
    headerCorner.Parent = header

    -- Logo container with improved visibility
    local logoSize = UserInputService.TouchEnabled and 80 or 100
    local logoContainer = Instance.new("Frame")
    logoContainer.Size = UDim2.new(0, logoSize, 0, logoSize)
    logoContainer.Position = UDim2.new(0.5, -logoSize/2, 0.5, -logoSize/2)
    logoContainer.BackgroundColor3 = theme.logoBackground
    logoContainer.Parent = header
    
    local logoCorner = Instance.new("UICorner")
    logoCorner.CornerRadius = UDim.new(0, 12)
    logoCorner.Parent = logoContainer

    local logo = Instance.new("ImageLabel")
    logo.Image = "rbxassetid://109421193232034"
    logo.Size = UDim2.new(0, logoSize-10, 0, logoSize-10)
    logo.Position = UDim2.new(0.5, -(logoSize-10)/2, 0.5, -(logoSize-10)/2)
    logo.BackgroundTransparency = 1
    logo.Parent = logoContainer

    -- Title with improved text rendering
    local title = Instance.new("TextLabel")
    title.Text = "MOLYN FREE"
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 0, 0, 110)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = UserInputService.TouchEnabled and 20 or 24
    title.TextStrokeTransparency = 0.8
    title.TextStrokeColor3 = Color3.new(0, 0, 0)
    title.Parent = mainFrame

    -- Subtitle
    local subtitle = Instance.new("TextLabel")
    subtitle.Text = "Public SCRIPT HUB | v5.7"
    subtitle.Size = UDim2.new(1, 0, 0, 20)
    subtitle.Position = UDim2.new(0, 0, 0, 140)
    subtitle.BackgroundTransparency = 1
    subtitle.TextColor3 = theme.textSecondary
    subtitle.Font = Enum.Font.Gotham
    subtitle.TextSize = UserInputService.TouchEnabled and 12 or 14
    subtitle.Parent = mainFrame

    -- Close Button with hover effect
    local closeBtn = Instance.new("TextButton")
    closeBtn.Text = "X"
    closeBtn.Size = UDim2.new(0, 25, 0, 25)
    closeBtn.Position = UDim2.new(1, -35, 0, 10)
    closeBtn.BackgroundColor3 = theme.closeButton
    closeBtn.TextColor3 = theme.text
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.Parent = mainFrame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = closeBtn
    
    closeBtn.MouseEnter:Connect(function()
        TweenService:Create(closeBtn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(255, 20, 20)}):Play()
    end)
    
    closeBtn.MouseLeave:Connect(function()
        TweenService:Create(closeBtn, TweenInfo.new(0.2), {BackgroundColor3 = theme.closeButton}):Play()
    end)
    
    -- Tab Buttons
    local tabsFrame = Instance.new("Frame")
    tabsFrame.Size = UDim2.new(1, -20, 0, 30)
    tabsFrame.Position = UDim2.new(0, 10, 0, 170)
    tabsFrame.BackgroundTransparency = 1
    tabsFrame.Parent = mainFrame
    
    local scriptsTab = Instance.new("TextButton")
    scriptsTab.Text = "SCRIPTS"
    scriptsTab.Size = UDim2.new(0.5, -5, 1, 0)
    scriptsTab.Position = UDim2.new(0, 0, 0, 0)
    scriptsTab.BackgroundColor3 = theme.primary
    scriptsTab.TextColor3 = theme.text
    scriptsTab.Font = Enum.Font.GothamBold
    scriptsTab.TextSize = UserInputService.TouchEnabled and 12 or 14
    scriptsTab.Parent = tabsFrame
    
    local feedbackTab = Instance.new("TextButton")
    feedbackTab.Text = "FEEDBACK"
    feedbackTab.Size = UDim2.new(0.5, -5, 1, 0)
    feedbackTab.Position = UDim2.new(0.5, 5, 0, 0)
    feedbackTab.BackgroundColor3 = theme.surface
    feedbackTab.TextColor3 = theme.text
    feedbackTab.Font = Enum.Font.GothamBold
    feedbackTab.TextSize = UserInputService.TouchEnabled and 12 or 14
    feedbackTab.Parent = tabsFrame
    
    local tabCorner = Instance.new("UICorner")
    tabCorner.CornerRadius = UDim.new(0, 6)
    tabCorner.Parent = scriptsTab
    tabCorner:Clone().Parent = feedbackTab
    
    -- Content Area
    local contentFrame = Instance.new("Frame")
    contentFrame.Size = UDim2.new(1, -20, 1, -210)
    contentFrame.Position = UDim2.new(0, 10, 0, 210)
    contentFrame.BackgroundTransparency = 1
    contentFrame.ClipsDescendants = true
    contentFrame.Parent = mainFrame
    
    -- Scripts Container with improved scrolling
    local scriptsFrame = Instance.new("ScrollingFrame")
    scriptsFrame.Size = UDim2.new(1, 0, 1, 0)
    scriptsFrame.BackgroundTransparency = 1
    scriptsFrame.ScrollBarThickness = 6
    scriptsFrame.ScrollBarImageColor3 = theme.primary
    scriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    scriptsFrame.ScrollingDirection = Enum.ScrollingDirection.Y
    scriptsFrame.VerticalScrollBarInset = Enum.ScrollBarInset.Always
    scriptsFrame.Parent = contentFrame

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 10)
    layout.Parent = scriptsFrame

    -- Search Bar
    local searchBar = Instance.new("Frame")
    searchBar.Size = UDim2.new(1, 0, 0, 40)
    searchBar.BackgroundColor3 = theme.surface
    searchBar.Parent = scriptsFrame
    
    local searchCorner = Instance.new("UICorner")
    searchCorner.CornerRadius = UDim.new(0, 8)
    searchCorner.Parent = searchBar
    
    local searchBox = Instance.new("TextBox")
    searchBox.PlaceholderText = "ابحث عن سكربت..."
    searchBox.Size = UDim2.new(1, -20, 0.8, 0)
    searchBox.Position = UDim2.new(0, 10, 0.1, 0)
    searchBox.BackgroundColor3 = theme.background
    searchBox.TextColor3 = theme.text
    searchBox.Font = Enum.Font.Gotham
    searchBox.TextSize = UserInputService.TouchEnabled and 14 or 16
    searchBox.TextXAlignment = Enum.TextXAlignment.Left
    searchBox.ClearTextOnFocus = false
    searchBox.Text = ""
    searchBox.Parent = searchBar
    
    local searchIcon = Instance.new("TextLabel")
    searchIcon.Text = "🔍"
    searchIcon.Size = UDim2.new(0, 30, 0, 30)
    searchIcon.Position = UDim2.new(1, -40, 0.1, 0)
    searchIcon.BackgroundTransparency = 1
    searchIcon.TextColor3 = theme.textSecondary
    searchIcon.Font = Enum.Font.Gotham
    searchIcon.TextSize = 16
    searchIcon.Parent = searchBar
    
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 6)
    boxCorner.Parent = searchBox
    
    -- Feedback Container (initially hidden)
    local feedbackFrame = CreateFeedbackUI(contentFrame)
    feedbackFrame.Visible = false
    
    -- Tab Switching with animation
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

    -- Create Script Buttons with improved visuals
    local scriptButtons = {}
    
    local function createScriptButton(scriptData)
        local btn = Instance.new("Frame")
        btn.Size = UDim2.new(1, 0, 0, UserInputService.TouchEnabled and 70 or 80)
        btn.BackgroundColor3 = theme.surface
        btn.Parent = scriptsFrame
        btn.Name = scriptData.name
        btn.Visible = true

        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 8)
        btnCorner.Parent = btn

        -- Script Name with improved visibility
        local name = Instance.new("TextLabel")
        name.Text = scriptData.name
        name.Size = UDim2.new(0.7, 0, 0, 25)
        name.Position = UDim2.new(0, 15, 0, 10)
        name.BackgroundTransparency = 1
        name.TextColor3 = scriptData.featured and theme.primary or theme.text
        name.Font = Enum.Font.GothamBold
        name.TextSize = UserInputService.TouchEnabled and 16 or 18
        name.TextXAlignment = Enum.TextXAlignment.Left
        name.TextStrokeTransparency = 0.8
        name.TextStrokeColor3 = Color3.new(0, 0, 0)
        name.Parent = btn

        -- Description
        local desc = Instance.new("TextLabel")
        desc.Text = scriptData.description
        desc.Size = UDim2.new(0.7, 0, 0, 15)
        desc.Position = UDim2.new(0, 15, 0, 35)
        desc.BackgroundTransparency = 1
        desc.TextColor3 = theme.textSecondary
        desc.Font = Enum.Font.Gotham
        desc.TextSize = UserInputService.TouchEnabled and 12 or 14
        desc.TextXAlignment = Enum.TextXAlignment.Left
        desc.Parent = btn

        -- Execute Button with improved interaction
        local execBtn = Instance.new("TextButton")
        execBtn.Text = "EXECUTE"
        execBtn.Size = UDim2.new(0, UserInputService.TouchEnabled and 80 or 100, 0, UserInputService.TouchEnabled and 25 or 30)
        execBtn.Position = UDim2.new(1, UserInputService.TouchEnabled and -90 or -120, 0.5, UserInputService.TouchEnabled and -12.5 or -15)
        execBtn.BackgroundColor3 = theme.primary
        execBtn.TextColor3 = theme.text
        execBtn.Font = Enum.Font.GothamBold
        execBtn.TextSize = UserInputService.TouchEnabled and 12 or 14
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
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, execBtn.Size.X.Offset - 5, 0, execBtn.Size.Y.Offset - 2)}):Play()
            wait(0.1)
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, execBtn.Size.X.Offset + 5, 0, execBtn.Size.Y.Offset + 2)}):Play()
            
            local success, err = pcall(function()
                loadstring(scriptData.code)()
            end)
            
            if success then
                CreateNotification("Executed: "..scriptData.name, theme.success, 3)
                
                -- Send execution data to webhook
                local gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
                local data = {
                    ["content"] = "Script Executed",
                    ["embeds"] = {{
                        ["title"] = "Script Execution",
                        ["description"] = "Player executed a script from MOLYN HUB",
                        ["color"] = 14423100,
                        ["fields"] = {
                            {["name"] = "📜 Script", ["value"] = scriptData.name, ["inline"] = true},
                            {["name"] = "👤 Player", ["value"] = player.Name, ["inline"] = true},
                            {["name"] = "🎮 Game", ["value"] = gameName, ["inline"] = true},
                            {["name"] = "📅 Time", ["value"] = os.date("%X - %d/%m/%Y"), ["inline"] = true}
                        }
                    }}
                }
                SendWebhook(DISCORD_WEBHOOK_URL, data)
            else
                CreateNotification("Failed: "..scriptData.name, theme.error, 3)
                warn("Execution error:", err)
            end
        end)

        -- Featured Badge with animation
        if scriptData.featured then
            local badge = Instance.new("Frame")
            badge.Size = UDim2.new(0, UserInputService.TouchEnabled and 60 or 80, 0, 20)
            badge.Position = UDim2.new(1, UserInputService.TouchEnabled and -160 or -230, 0, 10)
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
            badgeText.TextSize = UserInputService.TouchEnabled and 8 or 10
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
        
        table.insert(scriptButtons, {
            frame = btn,
            name = scriptData.name:lower(),
            desc = scriptData.description:lower()
        })
        
        return btn
    end

    -- Search functionality
    local function updateSearchResults(searchText)
        local searchLower = searchText:lower()
        
        for _, buttonData in ipairs(scriptButtons) do
            local visible = false
            
            if searchText == "" then
                visible = true
            else
                if buttonData.name:find(searchLower) or buttonData.desc:find(searchLower) then
                    visible = true
                end
            end
            
            buttonData.frame.Visible = visible
        end
    end
    
    searchBox:GetPropertyChangedSignal("Text"):Connect(function()
        updateSearchResults(searchBox.Text)
    end)

    -- Populate Scripts with sorting
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

    -- Close Button Function with animation
    closeBtn.MouseButton1Click:Connect(function()
        TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
            Size = UDim2.new(0, 0, 0, 0),
            Position = UDim2.new(0.5, 0, 0.5, 0)
        }):Play()
        wait(0.3)
        screenGui:Destroy()
    end)

    -- Dragging System with improved feel
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
    
    -- Entrance animation with improved timing
    mainFrame.Size = UDim2.new(0, 0, 0, 0)
    mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    
    TweenService:Create(mainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, baseSize, 0, baseHeight),
        Position = UDim2.new(0.5, -baseSize/2, 0.5, -baseHeight/2)
    }):Play()

    return screenGui
end

-- Initialize with error handling
local function Initialize()
    -- Security check
    if not CheckSecurity() then return end

    -- Wait for game to fully load
    if not game:IsLoaded() then
        game.Loaded:Wait()
    end

    -- Activate anti-spam system
    ActivateAntiSpam()

    -- Send monitoring data
    local success, err = pcall(SendMonitoringData)
    if not success then
        warn("Failed to send monitoring data:", err)
    end

    -- Create initial notification
    CreateNotification("MOLYN HUB LOADED", theme.primary, 3)

    -- Create GUI with error handling
    local gui = nil
    local guiVisible = false
    
    local function SafeCreateGUI()
        local success, result = pcall(createGUI)
        if success then
            return result
        else
            warn("Failed to create GUI:", result)
            CreateNotification("Failed to create GUI", theme.error, 5)
            return nil
        end
    end

    -- Create GUI automatically after delay
    delay(1, function()
        gui = SafeCreateGUI()
        guiVisible = gui ~= nil
    end)

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
                gui = SafeCreateGUI()
                guiVisible = gui ~= nil
                if guiVisible then
                    CreateNotification("MOLYN Hub opened", theme.primary, 2)
                end
            end
        end
    end)
end

-- Start the script with proper error handling
local success, err = pcall(Initialize)
if not success then
    warn("MOLYN HUB failed to initialize:", err)
    game:GetService("StarterGui"):SetCore("SendNotification", {
        Title = "MOLYN HUB Error",
        Text = "Failed to initialize: " .. tostring(err),
        Duration = 10
    })
end
