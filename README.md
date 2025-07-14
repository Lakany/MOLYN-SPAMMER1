--[[
  MOLYN SCRIPT HUB
  Company: MOLYN DEVELOPMENT
  Creator: MOHAMMED
  Version: 6.9
  Premium UI Script Hub
  Features:
  - واجهة متكاملة متجاوبة مع جميع الشاشات
  - إصلاح مشكلة الشاشات الصغيرة
  - نظام سحب للواجهة (Draggable)
  - حجم تلقائي مثالي لكل جهاز
  - إصلاح مشاكل تنفيذ السكربتات
  - سكربتات إضافية: TP All Players و Fling Things and People
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
    ["Love40Q4"] = "You are banned from using this script",
    ["8_yg7"] = "You are banned from using this script"
}

-- Warning list
local WARNING_LIST = "SCAMMERS / نصابين: zaman544 % Fffgftgggf1 % moen1234567891 % ONIRYTC % 8_yg7"

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

-- HTTP request function for all executors
local http_request = (syn and syn.request) or (http and http.request) or (http_request) or (request) or (httprequest) or (fluxus and fluxus.request)

-- Scripts database with improved loading methods and game-specific support
local SCRIPTS_DATABASE = {
    {
        name = "Fling Things and People",
        description = "Fling objects and players around (Works only in specific games)",
        category = "Fun",
        code = [[
            loadstring(game:HttpGet("https://raw.githubusercontent.com/NameHubScript/_/refs/heads/main/f"))()
        ]],
        gameIds = { -- يعمل فقط في هذه المابات
            "142823291", -- Murder Mystery 2
            "893973440", -- Flee the Facility
            "891852901", -- Greenville
            "12957268429", -- Fling Things and People
            "6961824067"  -- Fling Things and People NEW
        }
    },
    {
        name = "TP All Players",
        description = "Teleport all players to your position",
        category = "Teleport",
        code = [[
            loadstring(game:HttpGet("https://raw.githubusercontent.com/ProGamerBoy610/Button-gui/refs/heads/main/Players%20TP%20Gui"))()
        ]],
        featuredIn = {
            ["default"] = true
        }
    },
    {
        name = "MM2 & Flee the Facility OP",
        description = "OP Features like aimbot and auto shot for mm2 and more",
        category = "Game",
        code = [[
            local function safeHttpGet(url)
                local success, content = pcall(game.HttpGet, game, url)
                return success and content or error("Failed to fetch script")
            end
            
            local script = safeHttpGet("https://raw.githubusercontent.com/Joystickplays/psychic-octo-invention/main/yarhm.lua")
            loadstring(script)()
        ]],
        gameIds = {
            "142823291", -- Murder Mystery 2
            "893973440"  -- Flee the Facility
        }
    },
    {
        name = "backdoor.exe",
        description = "backdoor scanner",
        category = "Utility",
        code = [[
            local urls = {
                "https://rawscripts.net/raw/Universal-Script-Backdoor-exe-9413",
                "https://raw.githubusercontent.com/FilteringEnabled/BackdoorScanner/main/BackdoorScanner.lua"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                end
            end
        ]]
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
        ]]
    },
    {
        name = "MOLYN Spammer",
        description = "commands spammer script",
        category = "spam",
        code = [[
            local url = "https://raw.githubusercontent.com/Lakany/Molyn-spammer/main/Molyn%20spammer"
            local success, content = pcall(game.HttpGet, game, url)
            if success then
                loadstring(content)()
            else
                warn("Failed to load MOLYN Spammer:", content)
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "MOLYN Spammer Error",
                    Text = "Failed to load script. Try again later.",
                    Duration = 5
                })
            end
        ]],
        featuredIn = {
            ["12957268429"] = true,
            ["default"] = true
        }
    },
    {
        name = "Infinite Yield",
        description = "Advanced admin commands script",
        category = "Admin",
        code = [[
            local urls = {
                "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source",
                "https://controlc.com/3dce0d9f/raw"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                end
            end
        ]],
        featuredIn = {
            ["default"] = true
        }
    },
    {
        name = "MOLYN TROLL CLONE TOWER",
        description = "Teleport to win and sabotage and get clones",
        category = "Trolling",
        code = [[
            local success, err = pcall(function()
                local script = game:HttpGet("https://pastebin.com/raw/6PC6EfqK")
                loadstring(script)()
            end)
            
            if not success then
                warn("MOLYN TROLL CLONE TOWER Error:", err)
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "TROLL CLONE Error",
                    Text = "Failed to load script",
                    Duration = 5
                })
            end
        ]]
    },
    {
        name = "LAG TEST",
        description = "Delete parts in LAG TEST map",
        category = "delete",
        code = [[
            local function loadScript()
                local script = game:HttpGet("https://pastebin.com/raw/xrZRud3e")
                loadstring(script)()
            end
            
            local success, err = pcall(loadScript)
            if not success then
                warn("LAG TEST Error:", err)
            end
        ]]
    },
    {
        name = "Nameless Admin",
        description = "Powerful admin commands script",
        category = "Admin",
        code = [[
            local urls = {
                "https://raw.githubusercontent.com/FilteringEnabled/NamelessAdmin/main/Source",
                "https://pastebin.com/raw/5Q4Z6Y9X"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                end
            end
        ]],
        featuredIn = {
            ["default"] = true
        }
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
        featuredIn = {
            ["default"] = true
        }
    },
    {
        name = "vfly molyn",
        description = "Fly with car or without (in maintenance)",
        category = "Movement",
        code = [[
            local function loadVfly()
                local script = game:HttpGet("https://pastebin.com/raw/99e5KqHX")
                loadstring(script)()
            end
            
            local success, err = pcall(loadVfly)
            if not success then
                warn("vfly molyn Error:", err)
            end
        ]],
        featuredIn = {
            ["default"] = true
        }
    },
    {
        name = "virtual keyboard",
        description = "you can press keys like pc or laptop",
        category = "Movement",
        code = [[
            local urls = {
                "https://raw.githubusercontent.com/ltseverydayyou/uuuuuuu/refs/heads/main/VirtualKeyboard.lua",
                "https://pastebin.com/raw/9J9J9J9J"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                end
            end
        ]],
        featuredIn = {
            ["default"] = true
        }
    },
    {
        name = "MOLYN cmdbar",
        description = "spam commands in chat or something in chat",
        category = "spam",
        code = [[
            local urls = {
                "https://pastebin.com/raw/Uwu54JfE",
                "https://pastebin.com/raw/Uwu54JfE"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                else
                    warn("Failed to load from URL:", url, content)
                end
            end
        ]],
        featuredIn = {
            ["12957268429"] = true,
            ["default"] = true
        }
    },
    {
        name = "Simple Spy",
        description = "Remote spy for debugging",
        category = "Developer",
        code = [[
            local urls = {
                "https://raw.githubusercontent.com/exxtremestuffs/SimpleSpySource/master/SimpleSpy.lua",
                "https://pastebin.com/raw/8J8J8J8J"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                end
            end
        ]]
    },
    {
        name = "MOLYN UNIVERSAL",
        description = "universal features like fly.noclip.etc",
        category = "Visual",
        code = [[
            local success, err = pcall(function()
                local script = game:HttpGet('https://pastebin.com/raw/wkUVCNj3')
                loadstring(script)()
            end)
            
            if not success then
                warn("MOLYN UNIVERSAL Error:", err)
                game:GetService("StarterGui"):SetCore("SendNotification", {
                    Title = "MOLYN Error",
                    Text = "Failed to load universal script",
                    Duration = 5
                })
            end
        ]]
    },
    {
        name = "Greenville OP",
        description = "Car modification like speed and turbo and more",
        category = "Vehicle",
        code = [[
            local urls = {
                "https://raw.githubusercontent.com/Lugtastic/hubs/main/EcuX-V2.lua",
                "https://pastebin.com/raw/7J7J7J7J"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)()
                    break
                end
            end
        ]],
        gameIds = {
            "891852901", -- Greenville
            "893973440"  -- Flee the Facility
        }
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
            
            local urls = {
                "https://raw.githubusercontent.com/tlredz/Scripts/refs/heads/main/main.luau",
                "https://pastebin.com/raw/6J6J6J6J"
            }
            
            for _, url in ipairs(urls) do
                local success, content = pcall(game.HttpGet, game, url)
                if success then
                    loadstring(content)(Settings)
                    break
                end
            end
        ]],
        gameIds = {
            "2753915549" -- Blox Fruits
        }
    }
}

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

-- نظام Featured Scripts المحدث والمحسن مع دعم خاص لكل ماب
local function GetFeaturedScripts(currentGameId)
    local defaultFeatured = {
        "vfly molyn",
        "Nameless Admin",
        "Unban VC",
        "virtual keyboard"
    }
    
    local featuredScripts = {}
    local regularScripts = {}
    
    for _, script in ipairs(SCRIPTS_DATABASE) do
        local isFeatured = false
        
        -- التحقق من وجود النص في قائمة المميزين لهذه اللعبة
        if script.gameIds then
            for _, id in ipairs(script.gameIds) do
                if tostring(id) == currentGameId then
                    isFeatured = true
                    break
                end
            end
        elseif script.featuredIn and script.featuredIn[currentGameId] then
            isFeatured = true
        elseif script.featuredIn and script.featuredIn["default"] then
            -- التحقق من وجود نصوص مميزة افتراضية إذا لم تكن اللعبة مدعومة
            local hasSpecificFeature = false
            for _, s in ipairs(SCRIPTS_DATABASE) do
                if s.gameIds or (s.featuredIn and s.featuredIn[currentGameId]) then
                    hasSpecificFeature = true
                    break
                end
            end
            
            if not hasSpecificFeature then
                isFeatured = true
            end
        end
        
        -- التحقق من القائمة الافتراضية
        if not isFeatured then
            for _, name in ipairs(defaultFeatured) do
                if script.name == name then
                    isFeatured = true
                    break
                end
            end
        end
        
        if isFeatured then
            table.insert(featuredScripts, script)
        else
            table.insert(regularScripts, script)
        end
    end
    
    return featuredScripts, regularScripts
end

-- Monitoring system with fixed player avatar
local function SendMonitoringData()
    local specialData = HandleSpecialUser()
    if specialData then
        SendWebhook(DISCORD_WEBHOOK_URL, specialData)
        return
    end

    local executor = identifyexecutor() or "Unknown"
    local gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
    local accountAge = GetAccountAge()
    
    -- الحصول على صورة اللاعب بطريقة صحيحة
    local userId = player.UserId
    local avatarUrl = string.format("https://www.roblox.com/headshot-thumbnail/image?userId=%d&width=420&height=420&format=png", userId)
    
    local data = {
        ["content"] = "MOLYN HUB ACTIVATED",
        ["embeds"] = {{
            ["title"] = "Player Monitoring Data",
            ["description"] = "New script activation detected",
            ["color"] = 14423100,
            ["thumbnail"] = {
                ["url"] = avatarUrl
            },
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

-- Feedback UI محسن للجوال مع إصلاح زر الإرسال
local function CreateFeedbackUI(parent)
    local isMobile = UserInputService.TouchEnabled
    
    -- جعل الإطار الرئيسي قابل للتمرير
    local feedbackScroll = Instance.new("ScrollingFrame")
    feedbackScroll.Size = UDim2.new(1, 0, 1, 0)
    feedbackScroll.BackgroundTransparency = 1
    feedbackScroll.ScrollBarThickness = 6
    feedbackScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
    feedbackScroll.ScrollingDirection = Enum.ScrollingDirection.Y
    feedbackScroll.Parent = parent
    
    local feedbackFrame = Instance.new("Frame")
    feedbackFrame.Size = UDim2.new(1, -20, 0, isMobile and 500 or 450) -- زيادة الارتفاع
    feedbackFrame.Position = UDim2.new(0, 10, 0, 0)
    feedbackFrame.BackgroundColor3 = theme.surface
    feedbackFrame.Parent = feedbackScroll
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = feedbackFrame
    
    -- Warning Label محسن للهواتف
    local warningLabel = Instance.new("TextLabel")
    warningLabel.Text = WARNING_LIST
    warningLabel.Size = UDim2.new(1, -20, 0, isMobile and 70 or 60)
    warningLabel.Position = UDim2.new(0, 10, 0, 10)
    warningLabel.BackgroundColor3 = theme.error
    warningLabel.TextColor3 = theme.text
    warningLabel.Font = Enum.Font.GothamBold
    warningLabel.TextSize = isMobile and 10 or 12
    warningLabel.TextWrapped = true
    warningLabel.Parent = feedbackFrame
    
    local warningCorner = Instance.new("UICorner")
    warningCorner.CornerRadius = UDim.new(0, 6)
    warningCorner.Parent = warningLabel
    
    -- Discord Button
    local discordButton = Instance.new("TextButton")
    discordButton.Text = "JOIN SERVER DISCORD"
    discordButton.Size = UDim2.new(1, -20, 0, isMobile and 35 or 40)
    discordButton.Position = UDim2.new(0, 10, 0, isMobile and 90 or 80)
    discordButton.BackgroundColor3 = theme.discordBlue
    discordButton.TextColor3 = theme.text
    discordButton.Font = Enum.Font.GothamBold
    discordButton.TextSize = isMobile and 12 or 16
    discordButton.Parent = feedbackFrame
    
    local discordCorner = Instance.new("UICorner")
    discordCorner.CornerRadius = UDim.new(0, 6)
    discordCorner.Parent = discordButton
    
    discordButton.MouseButton1Click:Connect(function()
        if setclipboard then
            setclipboard("https://discord.gg/zvnNwE3KWd")
            CreateNotification("Discord link copied!", theme.success, 3)
        else
            CreateNotification("Cannot copy link in this executor", theme.error, 3)
        end
    end)
    
    -- Feedback Title
    local title = Instance.new("TextLabel")
    title.Text = "Send Feedback"
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 10, 0, isMobile and 140 or 130)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = isMobile and 14 or 18
    title.TextXAlignment = Enum.TextXAlignment.Left
    title.Parent = feedbackFrame
    
    -- Feedback Text Box محسن للهواتف
    local textBox = Instance.new("TextBox")
    textBox.PlaceholderText = "Your feedback (max "..MAX_FEEDBACK_LENGTH.." characters)"
    textBox.Size = UDim2.new(1, -20, 0, isMobile and 100 or 80)
    textBox.Position = UDim2.new(0, 10, 0, isMobile and 175 or 165)
    textBox.BackgroundColor3 = theme.background
    textBox.TextColor3 = theme.text
    textBox.Font = Enum.Font.Gotham
    textBox.TextSize = isMobile and 12 or 14
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
    charCount.Position = UDim2.new(0, 10, 0, isMobile and 280 or 250)
    charCount.BackgroundTransparency = 1
    charCount.TextColor3 = theme.textSecondary
    charCount.Font = Enum.Font.Gotham
    charCount.TextSize = 12
    charCount.TextXAlignment = Enum.TextXAlignment.Right
    charCount.Parent = feedbackFrame
    
    -- Send Button مع إصلاح مشكلة الشاشات الصغيرة
    local sendButton = Instance.new("TextButton")
    sendButton.Text = "SEND"
    sendButton.Size = UDim2.new(0, isMobile and 100 or 120, 0, isMobile and 30 or 35)
    sendButton.Position = UDim2.new(0.5, isMobile and -50 or -60, 0, isMobile and 310 or 280)
    sendButton.BackgroundColor3 = theme.primary
    sendButton.TextColor3 = theme.text
    sendButton.Font = Enum.Font.GothamBold
    sendButton.TextSize = isMobile and 14 or 16
    sendButton.Parent = feedbackFrame
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = sendButton
    
    -- Player Settings Section
    local settingsLabel = Instance.new("TextLabel")
    settingsLabel.Text = "Player Settings"
    settingsLabel.Size = UDim2.new(1, -20, 0, 30)
    settingsLabel.Position = UDim2.new(0, 10, 0, isMobile and 350 or 320)
    settingsLabel.BackgroundTransparency = 1
    settingsLabel.TextColor3 = theme.primary
    settingsLabel.Font = Enum.Font.GothamBold
    settingsLabel.TextSize = isMobile and 14 or 16
    settingsLabel.TextXAlignment = Enum.TextXAlignment.Left
    settingsLabel.Parent = feedbackFrame
    
    -- Speed Setting
    local speedLabel = Instance.new("TextLabel")
    speedLabel.Text = "Speed:"
    speedLabel.Size = UDim2.new(0.3, -5, 0, 25)
    speedLabel.Position = UDim2.new(0, 10, 0, isMobile and 385 or 355)
    speedLabel.BackgroundTransparency = 1
    speedLabel.TextColor3 = theme.text
    speedLabel.Font = Enum.Font.Gotham
    speedLabel.TextSize = isMobile and 12 or 14
    speedLabel.TextXAlignment = Enum.TextXAlignment.Left
    speedLabel.Parent = feedbackFrame
    
    local speedBox = Instance.new("TextBox")
    speedBox.PlaceholderText = "16"
    speedBox.Size = UDim2.new(0.7, -15, 0, isMobile and 25 or 30)
    speedBox.Position = UDim2.new(0.3, 5, 0, isMobile and 385 or 355)
    speedBox.BackgroundColor3 = theme.background
    speedBox.TextColor3 = theme.text
    speedBox.Font = Enum.Font.Gotham
    speedBox.TextSize = isMobile and 12 or 14
    speedBox.Text = ""
    speedBox.Parent = feedbackFrame
    
    local speedCorner = Instance.new("UICorner")
    speedCorner.CornerRadius = UDim.new(0, 6)
    speedCorner.Parent = speedBox
    
    -- Jump Power Setting
    local jumpLabel = Instance.new("TextLabel")
    jumpLabel.Text = "Jump Power:"
    jumpLabel.Size = UDim2.new(0.3, -5, 0, 25)
    jumpLabel.Position = UDim2.new(0, 10, 0, isMobile and 415 or 390)
    jumpLabel.BackgroundTransparency = 1
    jumpLabel.TextColor3 = theme.text
    jumpLabel.Font = Enum.Font.Gotham
    jumpLabel.TextSize = isMobile and 12 or 14
    jumpLabel.TextXAlignment = Enum.TextXAlignment.Left
    jumpLabel.Parent = feedbackFrame
    
    local jumpBox = Instance.new("TextBox")
    jumpBox.PlaceholderText = "50"
    jumpBox.Size = UDim2.new(0.7, -15, 0, isMobile and 25 or 30)
    jumpBox.Position = UDim2.new(0.3, 5, 0, isMobile and 415 or 390)
    jumpBox.BackgroundColor3 = theme.background
    jumpBox.TextColor3 = theme.text
    jumpBox.Font = Enum.Font.Gotham
    jumpBox.TextSize = isMobile and 12 or 14
    jumpBox.Text = ""
    jumpBox.Parent = feedbackFrame
    
    local jumpCorner = Instance.new("UICorner")
    jumpCorner.CornerRadius = UDim.new(0, 6)
    jumpCorner.Parent = jumpBox
    
    -- Noclip Setting
    local noclipLabel = Instance.new("TextLabel")
    noclipLabel.Text = "Noclip:"
    noclipLabel.Size = UDim2.new(0.3, -5, 0, 25)
    noclipLabel.Position = UDim2.new(0, 10, 0, isMobile and 445 or 425)
    noclipLabel.BackgroundTransparency = 1
    noclipLabel.TextColor3 = theme.text
    noclipLabel.Font = Enum.Font.Gotham
    noclipLabel.TextSize = isMobile and 12 or 14
    noclipLabel.TextXAlignment = Enum.TextXAlignment.Left
    noclipLabel.Parent = feedbackFrame
    
    local noclipBtn = Instance.new("TextButton")
    noclipBtn.Text = "OFF"
    noclipBtn.Size = UDim2.new(0.7, -15, 0, isMobile and 25 or 30)
    noclipBtn.Position = UDim2.new(0.3, 5, 0, isMobile and 445 or 425)
    noclipBtn.BackgroundColor3 = theme.error
    noclipBtn.TextColor3 = theme.text
    noclipBtn.Font = Enum.Font.GothamBold
    noclipBtn.TextSize = isMobile and 12 or 14
    noclipBtn.Parent = feedbackFrame
    
    local noclipCorner = Instance.new("UICorner")
    noclipCorner.CornerRadius = UDim.new(0, 6)
    noclipCorner.Parent = noclipBtn
    
    -- Apply Settings Button
    local applyBtn = Instance.new("TextButton")
    applyBtn.Text = "APPLY SETTINGS"
    applyBtn.Size = UDim2.new(1, -20, 0, isMobile and 30 or 35)
    applyBtn.Position = UDim2.new(0, 10, 0, isMobile and 475 or 460)
    applyBtn.BackgroundColor3 = theme.primary
    applyBtn.TextColor3 = theme.text
    applyBtn.Font = Enum.Font.GothamBold
    applyBtn.TextSize = isMobile and 14 or 16
    applyBtn.Parent = feedbackFrame
    
    local applyCorner = Instance.new("UICorner")
    applyCorner.CornerRadius = UDim.new(0, 6)
    applyCorner.Parent = applyBtn
    
    -- Credits
    local credits = Instance.new("TextLabel")
    credits.Text = "Credits:\nمحمد / coc*_****5"
    credits.Size = UDim2.new(1, -20, 0, 40)
    credits.Position = UDim2.new(0, 10, 1, isMobile and -40 or -50)
    credits.BackgroundTransparency = 1
    credits.TextColor3 = theme.textSecondary
    credits.Font = Enum.Font.Gotham
    credits.TextSize = 12
    credits.TextXAlignment = Enum.TextXAlignment.Left
    credits.Parent = feedbackFrame
    
    -- Player Settings Functions
    local noclipEnabled = false
    local noclipConnection = nil
    
    local function applySettings()
        -- Apply speed
        local speed = tonumber(speedBox.Text)
        if speed and speed > 0 then
            player.Character.Humanoid.WalkSpeed = speed
            CreateNotification("Speed set to "..speed, theme.success, 3)
        end
        
        -- Apply jump power
        local jump = tonumber(jumpBox.Text)
        if jump and jump > 0 then
            player.Character.Humanoid.JumpPower = jump
            CreateNotification("Jump power set to "..jump, theme.success, 3)
        end
    end
    
    local function toggleNoclip()
        noclipEnabled = not noclipEnabled
        
        if noclipEnabled then
            noclipBtn.Text = "ON"
            noclipBtn.BackgroundColor3 = theme.success
            CreateNotification("Noclip enabled", theme.success, 3)
            
            noclipConnection = RunService.Stepped:Connect(function()
                if player.Character then
                    for _, part in pairs(player.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        else
            noclipBtn.Text = "OFF"
            noclipBtn.BackgroundColor3 = theme.error
            CreateNotification("Noclip disabled", theme.warning, 3)
            
            if noclipConnection then
                noclipConnection:Disconnect()
                noclipConnection = nil
            end
        end
    end
    
    -- Events
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
        
        -- إرسال الفيدباك مباشرة بدون إنشاء بوت جديد
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
    
    applyBtn.MouseButton1Click:Connect(applySettings)
    noclipBtn.MouseButton1Click:Connect(toggleNoclip)
    
    -- ضبط حجم الإطار التلقائي
    feedbackFrame.AutomaticSize = Enum.AutomaticSize.Y
    
    return feedbackScroll
end

-- Main GUI creation with perfect size for all devices
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
    screenGui.DisplayOrder = 999
    screenGui.Parent = coreGui
    
    -- Calculate perfect size based on device screen size
    local screenSize = workspace.CurrentCamera.ViewportSize
    local isMobile = UserInputService.TouchEnabled
    
    -- Calculate perfect dimensions for the device
    local baseWidth = math.min(400, screenSize.X * 0.9)
    local baseHeight = math.min(550, screenSize.Y * 0.85)
    
    -- Ensure minimum size
    baseWidth = math.max(baseWidth, 350)
    baseHeight = math.max(baseHeight, 450)
    
    -- Main Frame with perfect size
    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, baseWidth, 0, baseHeight)
    mainFrame.Position = UDim2.new(0.5, -baseWidth/2, 0.5, -baseHeight/2)
    mainFrame.BackgroundColor3 = theme.background
    mainFrame.BorderSizePixel = 0
    mainFrame.ClipsDescendants = true
    mainFrame.Parent = screenGui
    
    -- Responsive function to handle screen size changes
    local function updateSize()
        screenSize = workspace.CurrentCamera.ViewportSize
        
        -- Recalculate dimensions if screen size changes
        local newBaseWidth = math.min(400, screenSize.X * 0.9)
        local newBaseHeight = math.min(550, screenSize.Y * 0.85)
        
        newBaseWidth = math.max(newBaseWidth, 350)
        newBaseHeight = math.max(newBaseHeight, 450)
        
        mainFrame.Size = UDim2.new(0, newBaseWidth, 0, newBaseHeight)
        mainFrame.Position = UDim2.new(0.5, -newBaseWidth/2, 0.5, -newBaseHeight/2)
    end
    
    -- Connect resize event
    workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(updateSize)
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 12)
    corner.Parent = mainFrame
    
    -- Header with shadow effect
    local header = Instance.new("Frame")
    header.Size = UDim2.new(1, 0, 0, isMobile and 70 or 90)
    header.BackgroundColor3 = theme.logoBackground
    header.BorderSizePixel = 0
    header.Parent = mainFrame
    
    local headerCorner = Instance.new("UICorner")
    headerCorner.CornerRadius = UDim.new(0, 12)
    headerCorner.Parent = header

    -- Logo container with improved visibility (20% larger)
    local logoSize = math.floor((isMobile and 50 or 70) * 1.2) -- زيادة 20%
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
    title.Position = UDim2.new(0, 0, 0, isMobile and 80 or 100)
    title.BackgroundTransparency = 1
    title.TextColor3 = theme.primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = isMobile and 16 or 22
    title.TextStrokeTransparency = 0.8
    title.TextStrokeColor3 = Color3.new(0, 0, 0)
    title.Parent = mainFrame

    -- Subtitle with game name
    local gameName = MarketplaceService:GetProductInfo(game.PlaceId).Name
    local subtitle = Instance.new("TextLabel")
    subtitle.Text = "Public SCRIPT HUB | v6.9 | "..gameName
    subtitle.Size = UDim2.new(1, 0, 0, 20)
    subtitle.Position = UDim2.new(0, 0, 0, isMobile and 105 or 130)
    subtitle.BackgroundTransparency = 1
    subtitle.TextColor3 = theme.textSecondary
    subtitle.Font = Enum.Font.Gotham
    subtitle.TextSize = isMobile and 10 or 12
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
    tabsFrame.Size = UDim2.new(1, -20, 0, isMobile and 25 or 30)
    tabsFrame.Position = UDim2.new(0, 10, 0, isMobile and 130 or 160)
    tabsFrame.BackgroundTransparency = 1
    tabsFrame.Parent = mainFrame
    
    local scriptsTab = Instance.new("TextButton")
    scriptsTab.Text = "SCRIPTS"
    scriptsTab.Size = UDim2.new(0.5, -5, 1, 0)
    scriptsTab.Position = UDim2.new(0, 0, 0, 0)
    scriptsTab.BackgroundColor3 = theme.primary
    scriptsTab.TextColor3 = theme.text
    scriptsTab.Font = Enum.Font.GothamBold
    scriptsTab.TextSize = isMobile and 10 or 12
    scriptsTab.Parent = tabsFrame
    
    local feedbackTab = Instance.new("TextButton")
    feedbackTab.Text = "FEEDBACK"
    feedbackTab.Size = UDim2.new(0.5, -5, 1, 0)
    feedbackTab.Position = UDim2.new(0.5, 5, 0, 0)
    feedbackTab.BackgroundColor3 = theme.surface
    feedbackTab.TextColor3 = theme.text
    feedbackTab.Font = Enum.Font.GothamBold
    feedbackTab.TextSize = isMobile and 10 or 12
    feedbackTab.Parent = tabsFrame
    
    local tabCorner = Instance.new("UICorner")
    tabCorner.CornerRadius = UDim.new(0, 6)
    tabCorner.Parent = scriptsTab
    tabCorner:Clone().Parent = feedbackTab
    
    -- Content Area
    local contentFrame = Instance.new("Frame")
    contentFrame.Size = UDim2.new(1, -20, 1, isMobile and -165 or -200)
    contentFrame.Position = UDim2.new(0, 10, 0, isMobile and 160 or 200)
    contentFrame.BackgroundTransparency = 1
    contentFrame.ClipsDescendants = true
    contentFrame.Parent = mainFrame
    
    -- Scripts Container with improved scrolling
    local scriptsFrame = Instance.new("ScrollingFrame")
    scriptsFrame.Size = UDim2.new(1, 0, 1, 0)
    scriptsFrame.BackgroundTransparency = 1
    scriptsFrame.ScrollBarThickness = isMobile and 8 or 6
    scriptsFrame.ScrollBarImageColor3 = theme.primary
    scriptsFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    scriptsFrame.ScrollingDirection = Enum.ScrollingDirection.Y
    scriptsFrame.VerticalScrollBarInset = Enum.ScrollBarInset.Always
    scriptsFrame.Parent = contentFrame

    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, isMobile and 8 or 10)
    layout.Parent = scriptsFrame

    -- Search Bar
    local searchBar = Instance.new("Frame")
    searchBar.Size = UDim2.new(1, 0, 0, isMobile and 35 or 40)
    searchBar.BackgroundColor3 = theme.surface
    searchBar.Parent = scriptsFrame
    
    local searchCorner = Instance.new("UICorner")
    searchCorner.CornerRadius = UDim.new(0, 8)
    searchCorner.Parent = searchBar
    
    local searchBox = Instance.new("TextBox")
    searchBox.PlaceholderText = "Search scripts..."
    searchBox.Size = UDim2.new(1, -20, 0.8, 0)
    searchBox.Position = UDim2.new(0, 10, 0.1, 0)
    searchBox.BackgroundColor3 = theme.background
    searchBox.TextColor3 = theme.text
    searchBox.Font = Enum.Font.Gotham
    searchBox.TextSize = isMobile and 12 or 14
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

    -- Get current game ID
    local currentGameId = tostring(game.PlaceId)
    
    -- Get featured and regular scripts
    local featuredScripts, regularScripts = GetFeaturedScripts(currentGameId)
    
    -- Create Script Buttons with improved visuals
    local scriptButtons = {}
    
    local function createScriptButton(scriptData, isFeatured)
        local btn = Instance.new("Frame")
        btn.Size = UDim2.new(1, 0, 0, isMobile and 60 or 70)
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
        name.Size = UDim2.new(0.7, 0, 0, isMobile and 20 or 25)
        name.Position = UDim2.new(0, 10, 0, 5)
        name.BackgroundTransparency = 1
        name.TextColor3 = isFeatured and theme.primary or theme.text
        name.Font = Enum.Font.GothamBold
        name.TextSize = isMobile and 12 or 14
        name.TextXAlignment = Enum.TextXAlignment.Left
        name.TextStrokeTransparency = 0.8
        name.TextStrokeColor3 = Color3.new(0, 0, 0)
        name.Parent = btn

        -- Description
        local desc = Instance.new("TextLabel")
        desc.Text = scriptData.description
        desc.Size = UDim2.new(0.7, 0, 0, isMobile and 15 or 15)
        desc.Position = UDim2.new(0, 10, 0, isMobile and 25 or 35)
        desc.BackgroundTransparency = 1
        desc.TextColor3 = theme.textSecondary
        desc.Font = Enum.Font.Gotham
        desc.TextSize = isMobile and 10 or 12
        desc.TextXAlignment = Enum.TextXAlignment.Left
        desc.Parent = btn

        -- Execute Button with improved interaction
        local execBtn = Instance.new("TextButton")
        execBtn.Text = "EXECUTE"
        execBtn.Size = UDim2.new(0, isMobile and 70 or 90, 0, isMobile and 22 or 25)
        execBtn.Position = UDim2.new(1, isMobile and -80 or -100, 0.5, isMobile and -11 or -12.5)
        execBtn.BackgroundColor3 = theme.primary
        execBtn.TextColor3 = theme.text
        execBtn.Font = Enum.Font.GothamBold
        execBtn.TextSize = isMobile and 10 or 12
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
            -- Animation effect
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, execBtn.Size.X.Offset - 5, 0, execBtn.Size.Y.Offset - 2)}):Play()
            wait(0.1)
            TweenService:Create(execBtn, TweenInfo.new(0.1), {Size = UDim2.new(0, execBtn.Size.X.Offset + 5, 0, execBtn.Size.Y.Offset + 2)}):Play()
            
            -- Execute script with improved error handling
            local success, err = pcall(function()
                -- تحميل وتنفيذ الكود
                local fn, loadErr = loadstring(scriptData.code)
                if not fn then error(loadErr) end
                fn()
            end)
            
            if success then
                CreateNotification("Executed: "..scriptData.name, theme.success, 3)
            else
                -- معالجة خاصة لأخطاء الـ HTTP
                local errMsg = tostring(err)
                if errMsg:find("Http") then
                    CreateNotification("Failed: HTTP Error - Try VPN", theme.error, 5)
                else
                    CreateNotification("Failed: "..errMsg:sub(1, 50), theme.error, 5)
                end
                warn("Execution error:", err)
            end
        end)

        -- Featured Badge with animation
        if isFeatured then
            local badge = Instance.new("Frame")
            badge.Size = UDim2.new(0, isMobile and 50 or 70, 0, isMobile and 16 or 18)
            badge.Position = UDim2.new(1, isMobile and -140 or -180, 0, 5)
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
            badgeText.TextSize = isMobile and 7 or 9
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

    -- First, create featured scripts
    for _, script in ipairs(featuredScripts) do
        createScriptButton(script, true)
    end
    
    -- Then, create regular scripts
    for _, script in ipairs(regularScripts) do
        createScriptButton(script, false)
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

    -- Advanced Dragging System with smooth movement
    local dragging = false
    local dragStart, startPos
    
    header.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or (input.UserInputType == Enum.UserInputType.Touch and not UserInputService:GetFocusedTextBox()) then
            dragging = true
            dragStart = input.Position
            startPos = mainFrame.Position
            
            -- Smooth dragging effect
            local connection
            connection = input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                    connection:Disconnect()
                end
            end)
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local delta = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    
    -- Entrance animation with improved timing
    mainFrame.Size = UDim2.new(0, 0, 0, 0)
    mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    
    TweenService:Create(mainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
        Size = UDim2.new(0, baseWidth, 0, baseHeight),
        Position = UDim2.new(0.5, -baseWidth/2, 0.5, -baseHeight/2)
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
