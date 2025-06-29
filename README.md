-- MOLYN SCRIPT HUB
-- Company: MOLYN DEVELOPMENT
-- Creator: MOHAMMED
-- Version: 4.6
-- Premium UI Script Hub with Player Control

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local CoreGui = game:GetService("CoreGui")
local MarketplaceService = game:GetService("MarketplaceService")
local RunService = game:GetService("RunService")
local VoiceChatService = game:GetService("VoiceChatService")
local TextChatService = game:GetService("TextChatService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Discord Webhook Configuration
local DISCORD_WEBHOOK_URL = "https://discord.com/api/webhooks/1388935051877683330/Oppqs6DDEHcndmNxEE7mkfe1LAlrjI5CaDdlHq2xs9iu39ohGlgHRVYL2CfEdD3TY-f_"
local FEEDBACK_WEBHOOK_URL = DISCORD_WEBHOOK_URL -- Same webhook for feedback

-- Security Configuration
local BLACKLIST = {
    ["hd"] = "You are banned from using this script",
    ["M7_MF"] = "You are banned from using this script",
    ["zaman544"] = "You are banned from using this script",
    ["moen1234567891"] = "You are banned from using this script",
    ["Fffgftgggf1"] = "You are banned from using this script",
    ["ONIRYTC"] = "You are banned from using this script",
    ["lovebri395"] = "You are banned from using this script"
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
    
    -- Other Scripts
    {
        name = "vfly molyn",
        description = "Fly with car or without (in maintenance)",
        category = "Movement",
        code = [[loadstring(game:HttpGet("https://pastebin.com/raw/99e5KqHX"))()]],
        featured = true
    },
    {
        name = "virtual keyboard",
        description = "you can press keys like pc or laptop)",
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
        description = "universal features like fly.noclip.etc ",
        category = "Visual",
        code = [[
            loadstring(game:HttpGet('https://pastebin.com/raw/wkUVCNj3'))()
        ]],
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
    
    -- Only show notification if files were deleted
    if deleted > 0 then
        CreateNotification("ANTI SPAM ACTIVATED", theme.primary, 5)
    end
end

-- Send Discord Webhook
local function SendWebhook(url, data)
    if not http_request then return end
    
    pcall(function()
        http_request({
            Url = url,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"
            },
            Body = HttpService:JSONEncode(data)
        })
    end)
end

-- Improved Admin Manager
local function SetupAdminCommands()
    _G.AdminCommands = {
        kick = function(name)
            local target = Players:FindFirstChild(name)
            if target then
                if target == player then
                    CreateNotification("You can't kick yourself!", theme.error, 3)
                    return
                end
                
                -- Try different kick methods
                pcall(function() target:Kick("Kicked by MOLYN HUB admin") end)
                pcall(function() game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/kick "..name, "All") end)
                CreateNotification("Kicked: "..name, theme.success, 3)
            else
                CreateNotification("Player not found: "..name, theme.error, 3)
            end
        end,
        
        ban = function(name)
            local target = Players:FindFirstChild(name)
            if target then
                if target == player then
                    CreateNotification("You can't ban yourself!", theme.error, 3)
                    return
                end
                
                -- Try different ban methods
                pcall(function() target:Kick("Banned by MOLYN HUB admin") end)
                pcall(function() game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("/ban "..name, "All") end)
                CreateNotification("Banned: "..name, theme.success, 3)
            else
                CreateNotification("Player not found: "..name, theme.error, 3)
            end
        end
    }
    
    -- Chat command handler
    if TextChatService then
        TextChatService.OnIncomingMessage = function(message)
            local text = string.lower(message.Text)
            local speaker = tostring(message.TextSource)
            
            if string.sub(text, 1, 6) == "/kick " then
                local target = string.sub(text, 7)
                _G.AdminCommands.kick(target)
            elseif string.sub(text, 1, 5) == "/ban " then
                local target = string.sub(text, 6)
                _G.AdminCommands.ban(target)
            end
        end
    else -- Fallback for older chat system
        game:GetService("Players").LocalPlayer.Chatted:Connect(function(msg)
            local text = string.lower(msg)
            
            if string.sub(text, 1, 6) == "/kick " then
                local target = string.sub(text, 7)
                _G.AdminCommands.kick(target)
            elseif string.sub(text, 1, 5) == "/ban " then
                local target = string.sub(text, 6)
                _G.AdminCommands.ban(target)
            end
        end)
    end
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

    -- Square Logo Container
    local logoContainer = Instance.new("Frame")
    logoContainer.Size = UDim2.new(0, 100, 0, 100)
    logoContainer.Position = UDim2.new(0.5, -50, 0.5, -50)
    logoContainer.BackgroundColor3 = theme.logoBackground
    logoContainer.Parent = header
    
    local logoCorner = Instance.new("UICorner")
    logoCorner.CornerRadius = UDim.new(0, 12)
    logoCorner.Parent = logoContainer

    -- Logo using Asset ID 109421193232034 (centered in square)
    local logo = Instance.new("ImageLabel")
    logo.Image = "rbxassetid://109421193232034" -- Actual logo asset
    logo.Size = UDim2.new(0, 90, 0, 90) -- Slightly smaller than container
    logo.Position = UDim2.new(0.5, -45, 0.5, -45) -- Centered
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
    subtitle.Text = "Public SCRIPT HUB | v4.6"
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

    -- Feedback Button
    local feedbackBtn = Instance.new("TextButton")
    feedbackBtn.Text = "Feedback"
    feedbackBtn.Size = UDim2.new(0, 100, 0, 30)
    feedbackBtn.Position = UDim2.new(0, 10, 1, -40)
    feedbackBtn.BackgroundColor3 = theme.primary
    feedbackBtn.TextColor3 = theme.text
    feedbackBtn.Font = Enum.Font.GothamBold
    feedbackBtn.Parent = mainFrame
    
    local feedbackCorner = Instance.new("UICorner")
    feedbackCorner.CornerRadius = UDim.new(0, 8)
    feedbackCorner.Parent = feedbackBtn
    
    -- Search Box
    local searchBox = Instance.new("TextBox")
    searchBox.PlaceholderText = "Search scripts..."
    searchBox.Size = UDim2.new(1, -120, 0, 30)
    searchBox.Position = UDim2.new(0, 10, 0, 190)
    searchBox.BackgroundColor3 = theme.surface
    searchBox.TextColor3 = theme.text
    searchBox.Font = Enum.Font.Gotham
    searchBox.TextSize = 14
    searchBox.Text = "" -- Clear initial text
    searchBox.ClearTextOnFocus = false
    searchBox.Parent = mainFrame
    
    local searchCorner = Instance.new("UICorner")
    searchCorner.CornerRadius = UDim.new(0, 8)
    searchCorner.Parent = searchBox
    
    local searchIcon = Instance.new("ImageLabel")
    searchIcon.Image = "rbxassetid://3926305904" -- Search icon
    searchIcon.ImageRectOffset = Vector2.new(964, 324)
    searchIcon.ImageRectSize = Vector2.new(36, 36)
    searchIcon.Size = UDim2.new(0, 20, 0, 20)
    searchIcon.Position = UDim2.new(1, -30, 0.5, -10)
    searchIcon.BackgroundTransparency = 1
    searchIcon.Parent = searchBox

    -- Scripts Container
    local scriptsFrame = Instance.new("ScrollingFrame")
    scriptsFrame.Size = UDim2.new(1, -20, 1, -240)
    scriptsFrame.Position = UDim2.new(0, 10, 0, 230)
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
        btn.Name = scriptData.name

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

    -- Search functionality
    searchBox:GetPropertyChangedSignal("Text"):Connect(function()
        local searchText = string.lower(searchBox.Text)
        
        for _, child in ipairs(scriptsFrame:GetChildren()) do
            if child:IsA("Frame") then
                local nameMatch = string.find(string.lower(child.Name), searchText)
                local visible = searchText == "" or nameMatch
                child.Visible = visible
            end
        end
    end)

    -- Feedback button functionality
    feedbackBtn.MouseButton1Click:Connect(function()
        -- Create feedback popup
        local feedbackPopup = Instance.new("Frame")
        feedbackPopup.Size = UDim2.new(0, 400, 0, 250)
        feedbackPopup.Position = UDim2.new(0.5, -200, 0.5, -125)
        feedbackPopup.BackgroundColor3 = theme.surface
        feedbackPopup.Parent = screenGui
        
        local popupCorner = Instance.new("UICorner")
        popupCorner.CornerRadius = UDim.new(0, 12)
        popupCorner.Parent = feedbackPopup
        
        local title = Instance.new("TextLabel")
        title.Text = "Send Feedback"
        title.Size = UDim2.new(1, 0, 0, 40)
        title.Position = UDim2.new(0, 0, 0, 10)
        title.BackgroundTransparency = 1
        title.TextColor3 = theme.primary
        title.Font = Enum.Font.GothamBold
        title.TextSize = 20
        title.Parent = feedbackPopup
        
        local inputBox = Instance.new("TextBox")
        inputBox.PlaceholderText = "Enter your feedback or suggestions..."
        inputBox.Size = UDim2.new(1, -40, 0, 100)
        inputBox.Position = UDim2.new(0, 20, 0, 60)
        inputBox.BackgroundColor3 = theme.background
        inputBox.TextColor3 = theme.text
        inputBox.Font = Enum.Font.Gotham
        inputBox.TextSize = 14
        inputBox.TextWrapped = true
        inputBox.ClearTextOnFocus = false
        inputBox.Parent = feedbackPopup
        
        local inputCorner = Instance.new("UICorner")
        inputCorner.CornerRadius = UDim.new(0, 8)
        inputCorner.Parent = inputBox
        
        local sendBtn = Instance.new("TextButton")
        sendBtn.Text = "SEND"
        sendBtn.Size = UDim2.new(0, 100, 0, 30)
        sendBtn.Position = UDim2.new(0.5, -50, 1, -50)
        sendBtn.BackgroundColor3 = theme.primary
        sendBtn.TextColor3 = theme.text
        sendBtn.Font = Enum.Font.GothamBold
        sendBtn.Parent = feedbackPopup
        
        local sendCorner = Instance.new("UICorner")
        sendCorner.CornerRadius = UDim.new(0, 8)
        sendCorner.Parent = sendBtn
        
        local closeBtn = Instance.new("TextButton")
        closeBtn.Text = "X"
        closeBtn.Size = UDim2.new(0, 30, 0, 30)
        closeBtn.Position = UDim2.new(1, -40, 0, 10)
        closeBtn.BackgroundColor3 = theme.closeButton
        closeBtn.TextColor3 = theme.text
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Parent = feedbackPopup
        
        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 8)
        btnCorner.Parent = closeBtn
        
        -- Send feedback
        sendBtn.MouseButton1Click:Connect(function()
            if inputBox.Text == "" then
                CreateNotification("Please enter feedback!", theme.error, 3)
                return
            end
            
            local data = {
                ["content"] = "MOLYN HUB FEEDBACK",
                ["username"] = player.Name,
                ["embeds"] = {{
                    ["title"] = "New Feedback",
                    ["description"] = inputBox.Text,
                    ["color"] = 14423100,
                    ["fields"] = {
                        {["name"] = "Game", ["value"] = MarketplaceService:GetProductInfo(game.PlaceId).Name, ["inline"] = true},
                        {["name"] = "Time", ["value"] = os.date("%X"), ["inline"] = true}
                    }
                }}
            }
            
            SendWebhook(FEEDBACK_WEBHOOK_URL, data)
            CreateNotification("Feedback sent! Thank you!", theme.success, 3)
            feedbackPopup:Destroy()
        end)
        
        -- Close popup
        closeBtn.MouseButton1Click:Connect(function()
            feedbackPopup:Destroy()
        end)
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

-- Setup admin commands
SetupAdminCommands()

-- Create initial notification
CreateNotification("MOLYN HUB LOADED", theme.primary, 3)

-- Create GUI automatically
wait(1)
local gui = createGUI()
local guiVisible = true
