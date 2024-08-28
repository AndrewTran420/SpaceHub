-- LocalScript placed in StarterPlayerScripts

-- Get the player's character and player
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Function to create and configure a highlight
local function createHighlight(target)
    local highlight = Instance.new("Highlight")
    highlight.Parent = target
    highlight.Adornee = target
    highlight.OutlineColor = Color3.fromRGB(255, 0, 0)  -- Red color for highlighting
    highlight.OutlineTransparency = 0.5  -- Adjust transparency as needed
    highlight.FillColor = Color3.fromRGB(255, 0, 0)  -- Optional, fill color
    highlight.FillTransparency = 0.5  -- Optional, fill transparency
end

-- Function to remove existing highlights from target
local function removeHighlight(target)
    for _, v in pairs(target:GetChildren()) do
        if v:IsA("Highlight") then
            v:Destroy()
        end
    end
end

-- Listen for when characters are added or updated
game.Players.PlayerAdded:Connect(function(newPlayer)
    newPlayer.CharacterAdded:Connect(function(character)
        -- Add highlight when another player joins
        createHighlight(character)
    end)
end)

-- Also highlight other players' characters currently in the game
for _, otherPlayer in pairs(game.Players:GetPlayers()) do
    if otherPlayer ~= player then
        if otherPlayer.Character then
            createHighlight(otherPlayer.Character)
        end
    end
end

-- Optional: Clean up highlights when the script ends
player.CharacterRemoving:Connect(function(character)
    removeHighlight(character)
end)
