-- [[ NAH HUB FIXED: RIVALS + XENO PC SUPPORT ]] --
repeat wait() until game:IsLoaded()

local wait = task.wait
local spawn = task.spawn

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local CoreGui = gethui and gethui() or game:GetService("CoreGui")
local LocalPlayer = Players.LocalPlayer

-- [[ GAME CONFIGURATION ]] --
local GameList = {
	["3808223175"] = { id = "4fe2dfc202115670b1813277df916ab2", keyless = false }, -- Jujutsu Infinite
	["994732206"]  = { id = "e2718ddebf562c5c4080dfce26b09398", keyless = false }, -- Blox Fruits
	["1511883870"] = { id = "fefdf5088c44beb34ef52ed6b520507c", keyless = false }, -- Shindo Life
	["6035872082"] = { id = "3bb7969a9ecb9e317b0a24681327c2e2", keyless = false }, -- RIVALS (RESTORED)
	["245662005"]  = { id = "21ad7f491e4658e9dc9529a60c887c6e", keyless = true },  -- Jailbreak
	["3317771874"] = { id = "e95ef6f27596e636a7d706375c040de4", keyless = true },  -- Pet Simulator 99
}

local executor_name = getexecutorname and getexecutorname():match("^%s*(.-)%s*$") or "Unknown"
local game_id = tostring(game.GameId)
local game_config = GameList[game_id]

-- Check if game is supported
if not game_config then
	LocalPlayer:Kick("Game not supported by nah hub.")
	return
end

-- [[ XENO PC OPTIMIZATION ]] --
-- We removed Xeno from the "low" list so it runs Rivals at full speed
for _, exec in ipairs({"Solara"}) do
	if string.find(executor_name, exec) then
		game:GetService("Workspace"):SetAttribute("low", true)
		break
	end
end

-- ... [INTRO ANIMATION CODE] ...

-- [[ THE NAH HUB RESKINNER LOOP ]] --
-- This forces the Solix menu to look like YOUR hub
task.spawn(function()
    while true do
        task.wait(1) 
        local targetGui = gethui and gethui() or game:GetService("CoreGui")
        
        for _, gui in pairs(targetGui:GetChildren()) do
            if gui:IsA("ScreenGui") and gui.Name ~= "nah hub" then
                for _, obj in pairs(gui:GetDescendants()) do
                    
                    -- Change Name to nah hub
                    if obj:IsA("TextLabel") and (obj.Text:find("Solix") or obj.Text:find("Hub")) then
                        obj.Text = "nah hub"
                        obj.TextColor3 = Color3.fromRGB(255, 110, 170) 
                    end
                    
                    -- Background Color (Purple/Pink Theme)
                    if obj:IsA("Frame") and obj.BackgroundColor3 == Color3.fromRGB(15, 12, 16) then
                        obj.BackgroundColor3 = Color3.fromRGB(15, 0, 25) 
                    end

                    -- Swap Logo
                    if obj:IsA("ImageLabel") and obj.Image ~= "" then
                        obj.Image = "rbxassetid://128862750268311" 
                    end
                end
            end
        end
    end
end)

-- [[ LOAD THE MAIN SCRIPT ]] --
local script_id = game_config.id
local luarmor_api = loadstring(game:HttpGet("https://sdkapi-public.luarmor.net/library.lua"))()
luarmor_api.script_id = script_id

-- If keyless, just run it. If not, the UI you built will handle the key.
if game_config.keyless then
    pcall(function() luarmor_api.load_script() end)
end
