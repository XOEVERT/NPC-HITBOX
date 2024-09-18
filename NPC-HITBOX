-- Load the Orion UI Library
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

-- Create the window and tabs
local window = OrionLib:MakeWindow({Name = "XOEVERT Hitbox & ESP Controller🎯", HidePremium = false, SaveConfig = true, ConfigFolder = "HitboxESPConfig"})

local hitboxTab = window:MakeTab({
    Name = "Hitbox Settings",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local espTab = window:MakeTab({
    Name = "ESP Settings",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Initial settings
local hitboxSize = 5
local headHitboxSize = 5
local hitboxEnabled = false
local headHitboxEnabled = false
local espEnabled = false
local rainbowESPEnabled = false
local hitboxTransparency = 0.5
local radius = 50
local walkSpeed = 16 -- Default walk speed
local trackedParts = {}

-- Function to apply hitbox
local function applyHitbox(part)
    if hitboxEnabled and part then
        part.Size = Vector3.new(hitboxSize, hitboxSize, hitboxSize)
        part.Transparency = hitboxTransparency
        part.CanCollide = false
    end
end

-- Function to apply head hitbox
local function applyHeadHitbox(part)
    if headHitboxEnabled and part then
        part.Size = Vector3.new(headHitboxSize, headHitboxSize, headHitboxSize)
        part.Transparency = hitboxTransparency
        part.CanCollide = false
    end
end

-- Function to reset hitbox
local function resetHitbox(part)
    if part then
        part.Size = Vector3.new(2, 2, 2)
        part.Transparency = 0
        part.CanCollide = true
    end
end

-- Rainbow ESP color function
local function getRainbowColor(t)
    local hue = (tick() % t) / t
    return Color3.fromHSV(hue, 1, 1)
end

-- Function to apply ESP
local function applyESP(part)
    if not part then return end
    local highlight = part:FindFirstChildOfClass("Highlight")
    if not highlight then
        highlight = Instance.new("Highlight")
        highlight.Parent = part
    end
    if rainbowESPEnabled then
        highlight.FillColor = getRainbowColor(5)
        highlight.OutlineColor = getRainbowColor(3)
    else
        highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Default Red ESP color
        highlight.OutlineColor = Color3.fromRGB(0, 0, 0)
    end
    highlight.FillTransparency = hitboxTransparency
    highlight.OutlineTransparency = 0.5
    highlight.Adornee = part
    highlight.Enabled = espEnabled
end

-- Function to remove ESP
local function removeESP(part)
    local highlight = part:FindFirstChildOfClass("Highlight")
    if highlight then
        highlight:Destroy()
    end
end

-- Function to handle NPC hitbox expansion
local function handleHitboxes()
    local player = game.Players.LocalPlayer
    local position = player.Character.HumanoidRootPart.Position
    local region = Region3.new(position - Vector3.new(radius, radius, radius), position + Vector3.new(radius, radius, radius))
    local partsInRegion = workspace:FindPartsInRegion3(region, nil, math.huge)

    for _, part in ipairs(partsInRegion) do
        local model = part:FindFirstAncestorWhichIsA("Model")
        if model and model:FindFirstChild("HumanoidRootPart") and model ~= player.Character then
            local hitboxPart = model:FindFirstChild("HumanoidRootPart")
            if hitboxPart then
                applyHitbox(hitboxPart)
                if espEnabled then
                    applyESP(hitboxPart)
                end
                trackedParts[hitboxPart] = true
            end

            local headPart = model:FindFirstChild("Head")
            if headPart then
                applyHeadHitbox(headPart)
                trackedParts[headPart] = true
            end
        end
    end

    -- Reset hitboxes and remove ESP if they're out of range
    for part, _ in pairs(trackedParts) do
        local distance = (part.Position - player.Character.HumanoidRootPart.Position).Magnitude
        if distance > radius then
            resetHitbox(part)
            removeESP(part)
            trackedParts[part] = nil
        end
    end
end

-- Create UI components: buttons, toggles, and sliders
hitboxTab:AddButton({
    Name = "Increase Hitbox Size",
    Callback = function()
        if hitboxSize < 35 then
            hitboxSize = hitboxSize + 1
        end
    end    
})

hitboxTab:AddButton({
    Name = "Decrease Hitbox Size",
    Callback = function()
        if hitboxSize > 1 then
            hitboxSize = hitboxSize - 1
        end
    end    
})

hitboxTab:AddButton({
    Name = "Increase Head Hitbox Size",
    Callback = function()
        if headHitboxSize < 35 then
            headHitboxSize = headHitboxSize + 1
        end
    end    
})

hitboxTab:AddButton({
    Name = "Decrease Head Hitbox Size",
    Callback = function()
        if headHitboxSize > 1 then
            headHitboxSize = headHitboxSize - 1
        end
    end    
})

hitboxTab:AddButton({
    Name = "Increase Hitbox Transparency",
    Callback = function()
        if hitboxTransparency < 1 then
            hitboxTransparency = math.min(hitboxTransparency + 0.1, 1)
        end
    end    
})

hitboxTab:AddButton({
    Name = "Decrease Hitbox Transparency",
    Callback = function()
        if hitboxTransparency > 0 then
            hitboxTransparency = math.max(hitboxTransparency - 0.1, 0)
        end
    end    
})

hitboxTab:AddToggle({
    Name = "Hitbox Toggle",
    Default = false,
    Callback = function(state)
        hitboxEnabled = state
    end    
})

hitboxTab:AddToggle({
    Name = "Head Hitbox Toggle",
    Default = false,
    Callback = function(state)
        headHitboxEnabled = state
    end    
})

espTab:AddToggle({
    Name = "ESP Toggle",
    Default = false,
    Callback = function(state)
        espEnabled = state
    end    
})

espTab:AddToggle({
    Name = "Rainbow ESP",
    Default = false,
    Callback = function(state)
        rainbowESPEnabled = state
    end    
})

espTab:AddButton({
    Name = "Increase Walk Speed",
    Callback = function()
        if walkSpeed < 100 then
            walkSpeed = walkSpeed + 1
            humanoid.WalkSpeed = walkSpeed
        end
    end    
})

espTab:AddButton({
    Name = "Decrease Walk Speed",
    Callback = function()
        if walkSpeed > 16 then
            walkSpeed = walkSpeed - 1
            humanoid.WalkSpeed = walkSpeed
        end
    end    
})

-- NPC Handler Loop
spawn(function()
    while true do
        handleHitboxes()
        wait(1)
    end
end)

-- Ensure OrionLib is initialized
OrionLib:Init()
