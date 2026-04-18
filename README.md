repeat wait() until game:IsLoaded()

local wait = task.wait
local spawn = task.spawn

-- Service Retrieval optimized for PC & Mobile Executors (Delta, Solara, etc.)
local function GetService(name)
    local s = game:GetService(name)
    return (cloneref and cloneref(s)) or s
end

local Players = GetService("Players")
local TweenService = GetService("TweenService")
local Lighting = GetService("Lighting")
local HttpService = GetService("HttpService")
local UserInputService = GetService("UserInputService")
local Workspace = GetService("Workspace")
local CoreGui = gethui and gethui() or GetService("CoreGui")

local LocalPlayer = Players.LocalPlayer

local title = "nah hub"
local labels = {}
local spacing = 48
local script_key = ""

-- 1. INTRO START
local blur = Instance.new("BlurEffect")
blur.Size = 0
blur.Parent = Lighting
TweenService:Create(blur, TweenInfo.new(0.25, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), { Size = 35 }):Play()

local gui = Instance.new("ScreenGui")
gui.Name = "nah hub"
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false
gui.Parent = CoreGui

local root = Instance.new("Frame")
root.Size = UDim2.new(1, 0, 1, 0)
root.BackgroundTransparency = 1
root.Parent = gui

local background = Instance.new("Frame")
background.Size = UDim2.new(1, 0, 1, 0)
background.BackgroundColor3 = Color3.fromRGB(15, 0, 25)
background.BackgroundTransparency = 1
background.ZIndex = 0
background.Parent = root
TweenService:Create(background, TweenInfo.new(0.25, Enum.EasingStyle.Sine), { BackgroundTransparency = 0.18 }):Play()

local image = Instance.new("ImageLabel")
image.AnchorPoint = Vector2.new(0.5, 0.5)
image.Position = UDim2.new(0.5, 0, 0.4, 0)
image.Size = UDim2.new(0, 0, 0, 0)
image.BackgroundTransparency = 1
image.Image = "rbxassetid://128862750268311"
image.ImageTransparency = 1
image.ScaleType = Enum.ScaleType.Fit
image.Parent = gui

TweenService:Create(image, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
	Size = UDim2.new(0, 200, 0, 200),
	ImageTransparency = 0
}):Play()

-- LEGO SWIPE DISAPPEAR FEATURE
local function SwipeAway()
	local TweenData = TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.In)

	TweenService:Create(image, TweenData, {
		Position = UDim2.new(0.5, 0, -0.6, 0),
		ImageTransparency = 1
	}):Play()

	for i, lbl in ipairs(labels) do
		local direction = (i % 2 == 0) and 1.6 or -0.6
		TweenService:Create(lbl, TweenData, {
			Position = UDim2.new(direction, 0, 0.65, 0),
			TextTransparency = 1,
			Rotation = math.random(-65, 65)
		}):Play()
	end

	TweenService:Create(background, TweenInfo.new(0.3), { BackgroundTransparency = 1 }):Play()
	TweenService:Create(blur, TweenInfo.new(0.3), { Size = 0 }):Play()
	task.wait(0.6)
	gui:Destroy()
	blur:Destroy()
end

for i = 1, #title do
	local char = title:sub(i, i)
	local lbl = Instance.new("TextLabel")
	lbl.Text = char
	lbl.Font = Enum.Font.GothamBlack
	lbl.TextColor3 = Color3.fromRGB(255, 110, 170)
	lbl.TextTransparency = 1
	lbl.TextStrokeTransparency = 1
	lbl.TextSize = 44
	lbl.AnchorPoint = Vector2.new(0.5, 0.5)
	lbl.Position = UDim2.new(0.5, (i - (#title / 2 + 0.5)) * spacing, 0.65, 0) 
	lbl.BackgroundTransparency = 1
	lbl.Parent = root

	local grad = Instance.new("UIGradient")
	grad.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 110, 170)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 255, 255))
	})
	grad.Rotation = -45
	grad.Parent = lbl

	TweenService:Create(lbl, TweenInfo.new(0.15, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
		TextTransparency = 0,
		TextStrokeTransparency = 0.4,
		TextSize = 68
	}):Play()

	task.delay(0.15, function()
		TweenService:Create(lbl, TweenInfo.new(0.15, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), { TextSize = 54 }):Play()
	end)

	table.insert(labels, lbl)
	task.wait(0.09)
end

task.wait(1.5)
SwipeAway()

-- 2. GAME LIST & EXECUTOR CHECK (INCLUDING DELTA)
local GameList = {
	["3808223175"] = { id = "4fe2dfc202115670b1813277df916ab2", keyless = false }, -- Jujutsu Infinite
	["994732206"]  = { id = "e2718ddebf562c5c4080dfce26b09398", keyless = false }, -- Blox Fruits
	["1511883870"] = { id = "fefdf5088c44beb34ef52ed6b520507c", keyless = false }, -- Shindo Life
	["6035872082"] = { id = "3bb7969a9ecb9e317b0a24681327c2e2", keyless = false }, -- Rivals
	["245662005"]  = { id = "21ad7f491e4658e9dc9529a60c887c6e", keyless = true },  -- Jailbreak
	["7018190066"] = { id = "98f5c64a0a9ecca29517078597bbcbdb", keyless = true },  -- Dead Rails
	["7074860883"] = { id = "0c8fdf9bb25a6a7071731b72a90e3c69", keyless = false }, -- Arise Crossover
	["7436755782"] = { id = "e4ea33e9eaf0ae943d59ea98f2444ebe", keyless = true },  -- Grow a Garden
	["7326934954"] = { id = "00e140acb477c5ecde501c1d448df6f9", keyless = true },  -- 99 Nights
	["7671049560"] = { id = "c0b41e859f576fb70183206224d4a75f", keyless = false }, -- The Forge
	["6760085372"] = { id = "e380382a05647eabda3a9892f95952c6", keyless = true },  -- Jujutsu: Zero
	["3317771874"] = { id = "e95ef6f27596e636a7d706375c040de4", keyless = true },  -- Pet Sim 99
	["9363735110"] = { id = "4948419832e0bd4aa588e628c45b6f8d", keyless = false }, -- Escape Tsunami
	["9509842868"] = { id = "ad4ccd094f8b6f972bff36de80475abe", keyless = true },  -- Garden Horizons
	["5130394318"] = { id = "3e7a75a970118d0f0cf629369524dc7d", keyless = true },  -- Bizarre Lineage
}

local executor_name = (getexecutorname and getexecutorname()) or "Unknown PC"
local game_id = tostring(game.GameId)
local game_config = GameList[game_id]

if not game_config then
	LocalPlayer:Kick("Game not supported by nah hub.")
	return
end

-- Added Delta to the low-mode attribute check
for _, exec in ipairs({"Xeno", "Solara", "Wave", "Delta"}) do
	if string.find(executor_name, exec) then
		Workspace:SetAttribute("low", true)
		break
	end
end

-- 3. LUARMOR LOAD
local luarmor_api = loadstring(game:HttpGet("https://sdkapi-public.luarmor.net/library.lua"))()
luarmor_api.script_id = game_config.id

if game_config.keyless then
	pcall(function() luarmor_api.load_script() end)
else
    -- Assuming Key UI code goes here
end

-- [[ THE END OF SCRIPT - REBRANDING FEATURE ]]
task.spawn(function()
    while true do
        task.wait(1) 
        local targetGui = gethui and gethui() or game:GetService("CoreGui")
        
        for _, otherGui in pairs(targetGui:GetChildren()) do
            if otherGui:IsA("ScreenGui") and otherGui.Name ~= "nah hub" then
                for _, obj in pairs(otherGui:GetDescendants()) do
                    
                    if obj:IsA("TextLabel") and (obj.Text:find("Solix") or obj.Text:find("Hub")) then
                        obj.Text = "nah hub"
                        obj.TextColor3 = Color3.fromRGB(255, 110, 170)
                    end
                    
                    if obj:IsA("Frame") and (obj.BackgroundColor3 == Color3.fromRGB(15, 12, 16) or obj.Name:find("Main")) then
                        obj.BackgroundColor3 = Color3.fromRGB(15, 0, 25)
                    end

                    if obj:IsA("ImageLabel") and obj.Image ~= "" then
                        obj.Image = "rbxassetid://128862750268311"
                    end
                end
            end
        end
    end
end)
