-- [[ FIXED NAH HUB LOADER ]] --
repeat wait() until game:IsLoaded()

local wait = task.wait
local spawn = task.spawn

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local CoreGui = gethui and gethui() or game:GetService("CoreGui")
local LocalPlayer = Players.LocalPlayer

-- ... [INTRO ANIMATION CODE REMAINS THE SAME] ...

-- [[ FIXED EXECUTOR DETECTION ]] --
-- Removed "Xeno" from this list because it is powerful enough for full features
local executor_name = getexecutorname and getexecutorname():match("^%s*(.-)%s*$") or "Unknown"

for _, exec in ipairs({"Solara"}) do -- Only keeping lower-tier executors here
	if string.find(executor_name, exec) then
		game:GetService("Workspace"):SetAttribute("low", true)
		break
	end
end

-- [[ FIXED SOLIX OVERRIDE ]] --
task.spawn(function()
    while true do
        task.wait(1) 
        local targetGui = gethui and gethui() or game:GetService("CoreGui")
        
        for _, gui in pairs(targetGui:GetChildren()) do
            if gui:IsA("ScreenGui") and gui.Name ~= "nah hub" then
                for _, obj in pairs(gui:GetDescendants()) do
                    
                    -- Update Branding to Your Theme
                    if obj:IsA("TextLabel") and (obj.Text:find("Solix") or obj.Text:find("Hub")) then
                        obj.Text = "nah hub"
                        obj.TextColor3 = Color3.fromRGB(255, 110, 170) 
                    end
                    
                    -- Match Background to your Dark Purple
                    if obj:IsA("Frame") and obj.BackgroundColor3 == Color3.fromRGB(15, 12, 16) then
                        obj.BackgroundColor3 = Color3.fromRGB(15, 0, 25) 
                    end

                    -- Force Your Logo
                    if obj:IsA("ImageLabel") and obj.Image ~= "" then
                        obj.Image = "rbxassetid://128862750268311" 
                    end
                end
            end
        end
    end
end)
