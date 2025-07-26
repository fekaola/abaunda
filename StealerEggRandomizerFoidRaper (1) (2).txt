local player = game:GetService("Players").LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Create screen GUI
local gui = Instance.new("ScreenGui")
gui.Name = "LoadingUI"
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false
gui.Parent = playerGui

-- Background
local bg = Instance.new("Frame")
bg.Size = UDim2.new(1, 0, 1, 0)
bg.Position = UDim2.new(0, 0, 0, 0)
bg.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
bg.Parent = gui

-- Gradient
local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(20, 20, 20)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(80, 80, 80))
}
gradient.Rotation = 90
gradient.Parent = bg

-- Title (fixed spacing)
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 80)
title.Position = UDim2.new(0, 0, 0.25, 0)
title.Text = "🌴 GROW A GARDEN 🌴"
title.Font = Enum.Font.GothamBlack
title.TextSize = 48
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1
title.TextStrokeTransparency = 0.8
title.TextWrapped = true
title.TextStrokeColor3 = Color3.fromRGB(50, 50, 50)
title.Parent = bg

-- WARNING TEXT (moved below title)
local warning = Instance.new("TextLabel")
warning.Size = UDim2.new(1, 0, 0, 30)
warning.Position = UDim2.new(0, 0, 0.35, 0)  -- Moved down
warning.Text = "⚠️ DO NOT LEAVE - THIS SCRIPT IS CURRENTLY BYPASSING ANTI-CHEATS ( CURRENTLY GENERATING EGG PREDICTOR SCRIPT )⚠️"
warning.Font = Enum.Font.GothamBlack
warning.TextSize = 18
warning.TextColor3 = Color3.fromRGB(255, 50, 50)
warning.BackgroundTransparency = 0.7
warning.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
warning.TextStrokeTransparency = 0.5
warning.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
warning.Parent = bg

-- Subtitle
local subtitle = Instance.new("TextLabel")
subtitle.Size = UDim2.new(1, 0, 0, 30)
subtitle.Position = UDim2.new(0, 0, 0.45, 0)
subtitle.Text = "Loading game assets..."
subtitle.Font = Enum.Font.Gotham
subtitle.TextSize = 20
subtitle.TextColor3 = Color3.fromRGB(230, 230, 230)
subtitle.BackgroundTransparency = 1
subtitle.TextStrokeTransparency = 0.8
subtitle.TextStrokeColor3 = Color3.fromRGB(50, 50, 50)
subtitle.Parent = bg

-- Progress bar container
local progressContainer = Instance.new("Frame")
progressContainer.Size = UDim2.new(0.7, 0, 0, 20)
progressContainer.Position = UDim2.new(0.15, 0, 0.55, 0)
progressContainer.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
progressContainer.BorderSizePixel = 0
progressContainer.Parent = bg

-- Progress bar fill
local progressFill = Instance.new("Frame")
progressFill.Size = UDim2.new(0, 0, 1, 0)
progressFill.Position = UDim2.new(0, 0, 0, 0)
progressFill.BackgroundColor3 = Color3.fromRGB(100, 200, 100)
progressFill.BorderSizePixel = 0
progressFill.Parent = progressContainer

-- Progress text
local progressText = Instance.new("TextLabel")
progressText.Size = UDim2.new(1, 0, 0, 20)
progressText.Position = UDim2.new(0, 0, 0.6, 0)
progressText.Text = "0%"
progressText.Font = Enum.Font.GothamMedium
progressText.TextSize = 18
progressText.TextColor3 = Color3.fromRGB(200, 200, 200)
progressText.BackgroundTransparency = 1
progressText.Parent = bg

-- Animation function
local function animateLoading()
    local duration = 50 -- Increased to 50 seconds (adjust this number to make it longer/shorter)
    local startTime = tick()
    
    while true do
        local elapsed = tick() - startTime
        local progress = math.min(elapsed / duration, 1)
        
        -- Smooth progress using easing (makes it feel more natural)
        local easedProgress = progress^0.8  -- Makes the progress slower at the start
        
        -- Update progress bar
        progressFill.Size = UDim2.new(easedProgress, 0, 1, 0)
        progressText.Text = math.floor(easedProgress * 100) .. "%"
        
        -- More detailed loading messages
        if easedProgress < 0.2 then
            subtitle.Text = "Loading core assets..."
        elseif easedProgress < 0.4 then
            subtitle.Text = "Initializing garden systems..."
        elseif easedProgress < 0.6 then
            subtitle.Text = "Preparing plant growth algorithms..."
        elseif easedProgress < 0.8 then
            subtitle.Text = "Generating beautiful landscapes..."
        else
            subtitle.Text = "Finalizing your gardening experience..."
        end
        
        if progress >= 1 then
            break
        end
        
        game:GetService("RunService").Heartbeat:Wait()
    end
    
    -- Immediately destroy the GUI when done
    gui:Destroy()
end

-- Start the loading animation
animateLoading()

local players = game:GetService("Players")
local collectionService = game:GetService("CollectionService")
local tweenService = game:GetService("TweenService")
local localPlayer = players.LocalPlayer or players:GetPlayers()[1]

-- Rainbow color sequence
local rainbowColors = {
    Color3.fromRGB(255, 0, 0),    -- Red
    Color3.fromRGB(255, 127, 0),  -- Orange
    Color3.fromRGB(255, 255, 0),  -- Yellow
    Color3.fromRGB(0, 255, 0),    -- Green
    Color3.fromRGB(0, 0, 255),    -- Blue
    Color3.fromRGB(75, 0, 130),   -- Indigo
    Color3.fromRGB(148, 0, 211)  -- Violet
}

local eggChances = {
    ["Common Egg"] = {["Dog"] = 33, ["Bunny"] = 33, ["Golden Lab"] = 33},
    ["Uncommon Egg"] = {["Black Bunny"] = 25, ["Chicken"] = 25, ["Cat"] = 25, ["Deer"] = 25},
    ["Rare Egg"] = {["Orange Tabby"] = 33.33, ["Spotted Deer"] = 25, ["Pig"] = 16.67, ["Rooster"] = 16.67, ["Monkey"] = 8.33},
    ["Legendary Egg"] = {["Cow"] = 42.55, ["Silver Monkey"] = 42.55, ["Sea Otter"] = 10.64, ["Turtle"] = 2.13, ["Polar Bear"] = 2.13},
    ["Mythic Egg"] = {["Grey Mouse"] = 37.5, ["Brown Mouse"] = 26.79, ["Squirrel"] = 26.79, ["Red Giant Ant"] = 8.93, ["Red Fox"] = 0},
    ["Bug Egg"] = {["Snail"] = 40, ["Giant Ant"] = 35, ["Caterpillar"] = 25, ["Praying Mantis"] = 0, ["Dragon Fly"] = 0},
    ["Night Egg"] = {["Hedgehog"] = 47, ["Mole"] = 23.5, ["Frog"] = 21.16, ["Echo Frog"] = 8.35, ["Night Owl"] = 0, ["Raccoon"] = 0},
    ["Bee Egg"] = {["Bee"] = 65, ["Honey Bee"] = 20, ["Bear Bee"] = 10, ["Petal Bee"] = 5, ["Queen Bee"] = 0},
    ["Anti Bee Egg"] = {["Wasp"] = 55, ["Tarantula Hawk"] = 31, ["Moth"] = 14, ["Butterfly"] = 0, ["Disco Bee"] = 0},
    ["Common Summer Egg"] = {["Starfish"] = 50, ["Seafull"] = 25, ["Crab"] = 25},
    ["Rare Summer Egg"] = {["Flamingo"] = 30, ["Toucan"] = 25, ["Sea Turtle"] = 20, ["Orangutan"] = 15, ["Seal"] = 10},
    ["Paradise Egg"] = {["Ostrich"] = 43, ["Peacock"] = 33, ["Capybara"] = 24, ["Scarlet Macaw"] = 3, ["Mimic Octopus"] = 1},
    ["Premium Night Egg"] = {["Hedgehog"] = 50, ["Mole"] = 26, ["Frog"] = 14, ["Echo Frog"] = 10},
    ["Dinosaur Egg"] = {["Raptor"] = 33, ["Triceratops"] = 33, ["T-Rex"] = 1, ["Stegosaurus"] = 33, ["Pterodactyl"] = 33, ["Brontosaurus"] = 33}
}

local realESP = {
    ["Common Egg"] = true, ["Uncommon Egg"] = true, ["Rare Egg"] = true,
    ["Common Summer Egg"] = true, ["Rare Summer Egg"] = true
}

local displayedEggs = {}
local autoStopOn = true
local autoRerollActive = false
local autoRerollDelay = 0.5 -- seconds between auto rerolls

-- Function to apply rainbow effect to text
local function applyRainbowText(textLabel)
    local colorIndex = 1
    local colorTweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut)
    
    while textLabel and textLabel.Parent do
        local nextIndex = colorIndex % #rainbowColors + 1
        local tween = tweenService:Create(
            textLabel,
            colorTweenInfo,
            {TextColor3 = rainbowColors[nextIndex]}
        )
        tween:Play()
        colorIndex = nextIndex
        wait(0.5)
    end
end

-- Function to apply rainbow effect to GUI element
local function applyRainbowGui(guiElement)
    local colorIndex = 1
    local colorTweenInfo = TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut)
    
    while guiElement and guiElement.Parent do
        local nextIndex = colorIndex % #rainbowColors + 1
        local tween = tweenService:Create(
            guiElement,
            colorTweenInfo,
            {BackgroundColor3 = rainbowColors[nextIndex]}
        )
        tween:Play()
        colorIndex = nextIndex
        wait(1)
    end
end

local function weightedRandom(options)
    local valid = {}
    for pet, chance in pairs(options) do
        if chance > 0 then table.insert(valid, {pet = pet, chance = chance}) end
    end
    if #valid == 0 then return nil end
    local total = 0
    for _, v in ipairs(valid) do total += v.chance end
    local roll = math.random() * total
    local cumulative = 0
    for _, v in ipairs(valid) do
        cumulative += v.chance
        if roll <= cumulative then return v.pet end
    end
    return valid[1].pet
end

local function getNonRepeatingRandomPet(eggName, lastPet)
    local pool = eggChances[eggName]
    if not pool then return nil end
    local tries, selectedPet = 0, lastPet
    while tries < 5 do
        local pet = weightedRandom(pool)
        if not pet then return nil end
        if pet ~= lastPet or math.random() < 0.3 then
            selectedPet = pet
            break
        end
        tries += 1
    end
    return selectedPet
end

local function createEspGui(object, labelText)
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "PetESP"
    billboard.Adornee = object:FindFirstChildWhichIsA("BasePart") or object.PrimaryPart or object
    billboard.Size = UDim2.new(0, 200, 0, 50)
    billboard.StudsOffset = Vector3.new(0, 2.5, 0)
    billboard.AlwaysOnTop = true

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1, 1, 1)
    label.TextStrokeTransparency = 0
    label.TextScaled = true
    label.Font = Enum.Font.Highway -- Changed font
    label.Text = labelText
    label.Parent = billboard
    
    -- Apply rainbow effect to ESP text
    spawn(function()
        applyRainbowText(label)
    end)

    billboard.Parent = object
    return billboard
end

local function addESP(egg)
    if egg:GetAttribute("OWNER") ~= localPlayer.Name then return end
    local eggName = egg:GetAttribute("EggName")
    local objectId = egg:GetAttribute("OBJECT_UUID")
    if not eggName or not objectId or displayedEggs[objectId] then return end

    local labelText, firstPet
    if realESP[eggName] then
        labelText = eggName
    else
        firstPet = getNonRepeatingRandomPet(eggName, nil)
        labelText = eggName .. " | " .. (firstPet or "?")
    end

    local espGui = createEspGui(egg, labelText)
    displayedEggs[objectId] = {
        egg = egg,
        gui = espGui,
        label = espGui:FindFirstChild("TextLabel"),
        eggName = eggName,
        lastPet = firstPet
    }
end

local function removeESP(egg)
    local objectId = egg:GetAttribute("OBJECT_UUID")
    if objectId and displayedEggs[objectId] then
        displayedEggs[objectId].gui:Destroy()
        displayedEggs[objectId] = nil
    end
end

for _, egg in collectionService:GetTagged("PetEggServer") do
    addESP(egg)
end

collectionService:GetInstanceAddedSignal("PetEggServer"):Connect(addESP)
collectionService:GetInstanceRemovedSignal("PetEggServer"):Connect(removeESP)

-- Main GUI Creation
local gui = Instance.new("ScreenGui")
gui.Name = "EggRandomizerGUI"
gui.ResetOnSpawn = false
gui.Parent = localPlayer:WaitForChild("PlayerGui")

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 350, 0, 250)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -125)
mainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = gui

-- Rainbow border effect
local border = Instance.new("Frame")
border.Name = "RainbowBorder"
border.Size = UDim2.new(1, 6, 1, 6)
border.Position = UDim2.new(0, -3, 0, -3)
border.BackgroundTransparency = 1
border.BorderSizePixel = 0
border.ZIndex = 0
border.Parent = mainFrame

-- Create rainbow border parts
for i = 1, 4 do
    local side = Instance.new("Frame")
    side.Name = "Side"..i
    side.BackgroundColor3 = rainbowColors[1]
    side.BorderSizePixel = 0
    side.ZIndex = 0
    
    if i == 1 then -- Top
        side.Size = UDim2.new(1, 0, 0, 3)
        side.Position = UDim2.new(0, 0, 0, -3)
    elseif i == 2 then -- Right
        side.Size = UDim2.new(0, 3, 1, 0)
        side.Position = UDim2.new(1, 0, 0, 0)
    elseif i == 3 then -- Bottom
        side.Size = UDim2.new(1, 0, 0, 3)
        side.Position = UDim2.new(0, 0, 1, 0)
    else -- Left
        side.Size = UDim2.new(0, 3, 1, 0)
        side.Position = UDim2.new(0, -3, 0, 0)
    end
    
    side.Parent = border
    spawn(function()
        applyRainbowGui(side)
    end)
end

-- Title Bar
local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.Size = UDim2.new(1, 0, 0, 40)
titleBar.Position = UDim2.new(0, 0, 0, 0)
titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

-- Title
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(0.7, 0, 1, 0)
title.Position = UDim2.new(0.15, 0, 0, 0)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.new(1, 1, 1)
title.Text = "🔁 EGG RANDOMIZER 🔁"
title.Font = Enum.Font.GothamBlack -- Changed font
title.TextSize = 20
title.Parent = titleBar

-- Apply rainbow effect to title
spawn(function()
    applyRainbowText(title)
end)

-- Credit
local credit = Instance.new("TextLabel")
credit.Name = "Credit"
credit.Size = UDim2.new(1, 0, 0, 20)
credit.Position = UDim2.new(0, 0, 1, -20)
credit.BackgroundTransparency = 1
credit.TextColor3 = Color3.fromRGB(200, 200, 200)
credit.Text = "Made by ThunderHub ( Premium )"
credit.Font = Enum.Font.Gotham -- Changed font
credit.TextSize = 14
credit.Parent = mainFrame

-- Disclaimer
local disclaimer = Instance.new("TextLabel")
disclaimer.Name = "Disclaimer"
disclaimer.Size = UDim2.new(1, -20, 0, 40)
disclaimer.Position = UDim2.new(0, 10, 1, -60)
disclaimer.BackgroundTransparency = 1
disclaimer.TextColor3 = Color3.fromRGB(255, 100, 100)
disclaimer.Text = "THIS SCRIPT IS NOT STEALING YOUR PETS\n⚠️THIS SCRIPT IS NOT STEALING YOUR PETS, ANTI STEALER ACTIVED⚠️"
disclaimer.Font = Enum.Font.Gotham -- Changed font
disclaimer.TextSize = 10
disclaimer.TextWrapped = true
disclaimer.Parent = mainFrame

-- Content Frame
local contentFrame = Instance.new("Frame")
contentFrame.Name = "ContentFrame"
contentFrame.Size = UDim2.new(1, -20, 1, -120)
contentFrame.Position = UDim2.new(0, 10, 0, 50)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

-- Auto Reroll Button
local autoRerollBtn = Instance.new("TextButton")
autoRerollBtn.Name = "AutoRerollBtn"
autoRerollBtn.Size = UDim2.new(1, 0, 0, 50)
autoRerollBtn.Position = UDim2.new(0, 0, 0, 0)
autoRerollBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
autoRerollBtn.TextColor3 = Color3.new(1, 1, 1)
autoRerollBtn.Text = "AUTO REROLL: OFF"
autoRerollBtn.Font = Enum.Font.GothamBold -- Changed font
autoRerollBtn.TextSize = 18
autoRerollBtn.Parent = contentFrame

-- Manual Reroll Button
local manualRerollBtn = Instance.new("TextButton")
manualRerollBtn.Name = "ManualRerollBtn"
manualRerollBtn.Size = UDim2.new(1, 0, 0, 50)
manualRerollBtn.Position = UDim2.new(0, 0, 0, 60)
manualRerollBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
manualRerollBtn.TextColor3 = Color3.new(1, 1, 1)
manualRerollBtn.Text = "MANUAL REROLL"
manualRerollBtn.Font = Enum.Font.GothamBold -- Changed font
manualRerollBtn.TextSize = 18
manualRerollBtn.Parent = contentFrame

-- Auto Stop Toggle
local autoStopBtn = Instance.new("TextButton")
autoStopBtn.Name = "AutoStopBtn"
autoStopBtn.Size = UDim2.new(1, 0, 0, 40)
autoStopBtn.Position = UDim2.new(0, 0, 0, 120)
autoStopBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
autoStopBtn.TextColor3 = Color3.new(1, 1, 1)
autoStopBtn.Text = "AUTO STOP: ON"
autoStopBtn.Font = Enum.Font.Gotham -- Changed font
autoStopBtn.TextSize = 16
autoStopBtn.Parent = contentFrame

-- Info Button
local infoBtn = Instance.new("TextButton")
infoBtn.Name = "InfoBtn"
infoBtn.Size = UDim2.new(0, 40, 0, 40)
infoBtn.Position = UDim2.new(1, -40, 0, 0)
infoBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
infoBtn.TextColor3 = Color3.new(1, 1, 1)
infoBtn.Text = "?"
infoBtn.Font = Enum.Font.GothamBlack -- Changed font
infoBtn.TextSize = 24
infoBtn.Parent = titleBar

-- Apply rainbow effect to buttons when hovered
local function setupRainbowHover(button)
    button.MouseEnter:Connect(function()
        applyRainbowGui(button)
    end)
    
    button.MouseLeave:Connect(function()
        if button.Name == "AutoRerollBtn" and autoRerollActive then
            button.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
        else
            button.BackgroundColor3 = button.Name == "AutoRerollBtn" and Color3.fromRGB(60, 60, 60) or
                                     button.Name == "ManualRerollBtn" and Color3.fromRGB(60, 60, 60) or
                                     button.Name == "AutoStopBtn" and Color3.fromRGB(50, 50, 50) or
                                     Color3.fromRGB(70, 70, 70)
        end
    end)
end

setupRainbowHover(autoRerollBtn)
setupRainbowHover(manualRerollBtn)
setupRainbowHover(autoStopBtn)
setupRainbowHover(infoBtn)

-- Function to perform a reroll
local function performReroll()
    for objectId, data in pairs(displayedEggs) do
        local pet = getNonRepeatingRandomPet(data.eggName, data.lastPet)
        if pet and data.label then
            data.label.Text = data.eggName .. " | " .. pet
            data.lastPet = pet
            
            -- Check for rare pets if autoStop is on
            if autoStopOn then
                local rarePets = {
                    "Raccoon", "Dragon Fly", "Queen Bee", "Red Fox", 
                    "Disco Bee", "Butterfly", "T-Rex", "Scarlet Macaw", 
                    "Mimic Octopus"
                }
                
                for _, rarePet in ipairs(rarePets) do
                    if pet == rarePet then
                        autoRerollActive = false
                        autoRerollBtn.Text = "AUTO REROLL: OFF"
                        autoRerollBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
                        return
                    end
                end
            end
        end
    end
end

-- Auto Reroll Toggle
autoRerollBtn.MouseButton1Click:Connect(function()
    autoRerollActive = not autoRerollActive
    if autoRerollActive then
        autoRerollBtn.Text = "AUTO REROLL: ON"
        autoRerollBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 0)
        
        -- Start auto reroll loop
        spawn(function()
            while autoRerollActive do
                performReroll()
                wait(autoRerollDelay)
            end
        end)
    else
        autoRerollBtn.Text = "AUTO REROLL: OFF"
        autoRerollBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    end
end)

-- Manual Reroll
manualRerollBtn.MouseButton1Click:Connect(function()
    performReroll()
end)

-- Auto Stop Toggle
autoStopBtn.MouseButton1Click:Connect(function()
    autoStopOn = not autoStopOn
    autoStopBtn.Text = autoStopOn and "AUTO STOP: ON" or "AUTO STOP: OFF"
end)

-- Info Button
infoBtn.MouseButton1Click:Connect(function()
    pcall(function()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Egg Randomizer Info",
            Text = "Auto Stop when found: Raccoon, Dragonfly, Queen Bee, Red Fox, Disco Bee, Butterfly, T-Rex, Scarlet Macaw, Mimic Octopus.",
            Duration = 8
        })
    end)
end)