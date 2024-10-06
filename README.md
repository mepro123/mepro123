local ui = Instance.new("ScreenGui")
local textlabel = Instance.new("TextLabel")
local button = Instance.new("TextButton")
local button_2 = Instance.new("TextButton")
local button_3 = Instance.new("TextButton")

ui.Parent = game.Players.LocalPlayer.PlayerGui
ui.Name = "SpeedRunSimUI"
textlabel.Parent = ui
textlabel.Position = UDim2.new(0.709, 0,0.019, 0)
textlabel.Size = UDim2.new(0, 260,0, 37)
textlabel.Name = "Main"
textlabel.TextScaled = true
textlabel.Text = "Main (Made by tham333333_3)"
button.Parent = textlabel
button.Position = UDim2.new(-0.003, 0,0.976, 0)
button.Name = "Button"
button.Size = UDim2.new(0, 260,0, 89)
button.TextScaled = true
button.Text = "Spawn "..pet_spawn
button_2.Parent = button
button_2.Position = UDim2.new(-0.003, 0,0.995, 0)
button_2.Size = UDim2.new(0, 260,0, 36)
button_2.Text = "Close UI[K TO OPEN]"
button_2.TextScaled = true
button_3.Parent = button
button_3.Position = UDim2.new(-0.003, 0,1.399, 0)
button_3.Text = "Destroy UI"
button_3.TextScaled = true
button_3.Size = UDim2.new(0, 260,0, 36)

button.MouseButton1Down:Connect(function()
	local args = {
		[1] = pet_spawn,
		[2] = false
	}

	game:GetService("ReplicatedStorage").Remotes.CanBuyEgg:InvokeServer(unpack(args))

	game:GetService("ReplicatedStorage").Remotes.CanBuyEgg:InvokeServer(pet_spawn, false)
	print("Spawned "..pet_spawn)
end)

button_2.MouseButton1Down:Connect(function()
	button.Visible = false
	
	local UIS = game:GetService('UserInputService')
	local Player = game.Players.LocalPlayer
	local Character = Player.Character


	UIS.InputBegan:connect(function(input)--When a player has pressed LeftShift it will play the animation and it will set the normal walking speed (16) to 35.
		if input.KeyCode == Enum.KeyCode.K then
			button.Visible = true
		end
	end)
end)

button_3.MouseButton1Down:Connect(function()
	ui:Destroy()
end)

local UserInputService = game:GetService("UserInputService")

local gui = textlabel

local dragging
local dragInput
local dragStart
local startPos

local function update(input)
	local delta = input.Position - dragStart
	gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

gui.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = gui.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

gui.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		update(input)
	end
end)
