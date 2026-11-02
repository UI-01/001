

local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = game:GetService("Players").LocalPlayer

local KeySystem = {
	ThemeObjects = {},
	Connections = {},
	Themes = {
		Default = {
			Main = Color3.fromRGB(25, 25, 25),
			Second = Color3.fromRGB(32, 32, 32),
			Stroke = Color3.fromRGB(60, 60, 60),
			Text = Color3.fromRGB(240, 240, 240),
			TextDark = Color3.fromRGB(150, 150, 150)
		}
	},
	SelectedTheme = "Default"
}

local Xen0KeyGui = Instance.new("ScreenGui")
Xen0KeyGui.Name = "Xen0KeySystem"
if syn then
	syn.protect_gui(Xen0KeyGui)
	Xen0KeyGui.Parent = game.CoreGui
else
	Xen0KeyGui.Parent = gethui() or game.CoreGui
end

if gethui then
	for _, Interface in ipairs(gethui():GetChildren()) do
		if Interface.Name == Xen0KeyGui.Name and Interface ~= Xen0KeyGui then
			Interface:Destroy()
		end
	end
else
	for _, Interface in ipairs(game.CoreGui:GetChildren()) do
		if Interface.Name == Xen0KeyGui.Name and Interface ~= Xen0KeyGui then
			Interface:Destroy()
		end
	end
end

function KeySystem:IsRunning()
	if gethui then
		return Xen0KeyGui.Parent == gethui()
	else
		return Xen0KeyGui.Parent == game:GetService("CoreGui")
	end
end

local function AddConnection(Signal, Function)
	if (not KeySystem:IsRunning()) then return end
	local SignalConnect = Signal:Connect(Function)
	table.insert(KeySystem.Connections, SignalConnect)
	return SignalConnect
end

task.spawn(function()
	while (KeySystem:IsRunning()) do task.wait() end
	for _, Connection in next, KeySystem.Connections do
		Connection:Disconnect()
	end
end)

local function AddDraggingFunctionality(DragPoint, Main)
	pcall(function()
		local Dragging, DragInput, MousePos, FramePos = false
		DragPoint.InputBegan:Connect(function(Input)
			if Input.UserInputType == Enum.UserInputType.MouseButton1 then
				Dragging = true
				MousePos = Input.Position
				FramePos = Main.Position

				Input.Changed:Connect(function()
					if Input.UserInputState == Enum.UserInputState.End then
						Dragging = false
					end
				end)
			end
		end)
		DragPoint.InputChanged:Connect(function(Input)
			if Input.UserInputType == Enum.UserInputType.MouseMovement then
				DragInput = Input
			end
		end)
		UserInputService.InputChanged:Connect(function(Input)
			if Input == DragInput and Dragging then
				local Delta = Input.Position - MousePos
				TweenService:Create(Main, TweenInfo.new(0.45, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Position  = UDim2.new(FramePos.X.Scale,FramePos.X.Offset + Delta.X, FramePos.Y.Scale, FramePos.Y.Offset + Delta.Y)}):Play()
			end
		end)
	end)
end   

local function Create(Name, Properties, Children)
	local Object = Instance.new(Name)
	for i, v in next, Properties or {} do Object[i] = v end
	for i, v in next, Children or {} do v.Parent = Object end
	return Object
end

local function ReturnProperty(Object)
	if Object:IsA("Frame") or Object:IsA("TextButton") then return "BackgroundColor3" end 
	if Object:IsA("UIStroke") then return "Color" end 
	if Object:IsA("TextLabel") or Object:IsA("TextBox") then return "TextColor3" end   
end

local function AddThemeObject(Object, Type)
	if not KeySystem.ThemeObjects[Type] then KeySystem.ThemeObjects[Type] = {} end    
	table.insert(KeySystem.ThemeObjects[Type], Object)
	Object[ReturnProperty(Object)] = KeySystem.Themes[KeySystem.SelectedTheme][Type]
	return Object
end    

function KeySystem:CreateWindow(Config)
	local KeyConfig = Config or {}
	local ExpectedKey = KeyConfig.Key or "XENO_KEY"
	local Linkvertise = KeyConfig.Link or "https://linkvertise.com"
	local OnSuccess = KeyConfig.Callback or function() end
	local ActiveTheme = KeySystem.Themes[KeySystem.SelectedTheme]
	
	local MainFrame = Create("Frame", {
		Name = "Xen0KeyWindow",
		Size = UDim2.new(0, 360, 0, 220),
		Position = UDim2.new(0.5, -180, 0.5, -110),
		BackgroundColor3 = ActiveTheme.Main,
		BorderSizePixel = 0,
		Parent = Xen0KeyGui
	}, {
		Create("UICorner", { CornerRadius = UDim.new(0, 8) })
	})
	
	local FrameStroke = Create("UIStroke", { Color = ActiveTheme.Stroke, Thickness = 1, Parent = MainFrame })
	AddThemeObject(FrameStroke, "Stroke")
	AddThemeObject(MainFrame, "Main")
	
	local TopBar = Create("Frame", {
		Name = "TopBar",
		Size = UDim2.new(1, 0, 0, 42),
		BackgroundColor3 = ActiveTheme.Second,
		BorderSizePixel = 0,
		Parent = MainFrame
	}, {
		Create("UICorner", { CornerRadius = UDim.new(0, 8) })
	})
	
	local CoverFrame = Create("Frame", {
		Size = UDim2.new(1, 0, 0, 10),
		Position = UDim2.new(0, 0, 1, -10),
		BackgroundColor3 = ActiveTheme.Second,
		BorderSizePixel = 0,
		Parent = TopBar
	})
	
	AddThemeObject(TopBar, "Second")
	AddThemeObject(CoverFrame, "Second")
	AddDraggingFunctionality(TopBar, MainFrame)
	
	local TitleLabel = Create("TextLabel", {
		Size = UDim2.new(1, -20, 1, 0),
		Position = UDim2.new(0, 15, 0, 0),
		BackgroundTransparency = 1,
		Text = KeyConfig.Name or "XEN0 HUB // SECURITY LAYER",
		TextColor3 = ActiveTheme.Text,
		Font = Enum.Font.Code,
		TextSize = 13,
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = TopBar
	})
	AddThemeObject(TitleLabel, "Text")
	
	local StatusLabel = Create("TextLabel", {
		Size = UDim2.new(1, -30, 0, 20),
		Position = UDim2.new(0, 15, 0, 52),
		BackgroundTransparency = 1,
		Text = "Status: Awaiting Verification",
		TextColor3 = ActiveTheme.TextDark,
		Font = Enum.Font.Code,
		TextSize = 11,
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = MainFrame
	})
	AddThemeObject(StatusLabel, "TextDark")
	
	local InputWrapper = Create("Frame", {
		Size = UDim2.new(1, -30, 0, 42),
		Position = UDim2.new(0, 15, 0, 82),
		BackgroundColor3 = ActiveTheme.Second,
		BorderSizePixel = 0,
		Parent = MainFrame
	}, {
		Create("UICorner", { CornerRadius = UDim.new(0, 6) })
	})
	local InputStroke = Create("UIStroke", { Color = ActiveTheme.Stroke, Thickness = 1, Parent = InputWrapper })
	AddThemeObject(InputWrapper, "Second")
	AddThemeObject(InputStroke, "Stroke")
	
	local KeyBox = Create("TextBox", {
		Size = UDim2.new(1, -16, 1, 0),
		Position = UDim2.new(0, 8, 0, 0),
		BackgroundTransparency = 1,
		Text = "",
		PlaceholderText = "Input security access hash key...",
		PlaceholderColor3 = ActiveTheme.TextDark,
		TextColor3 = ActiveTheme.Text,
		Font = Enum.Font.Code,
		TextSize = 13,
		TextXAlignment = Enum.TextXAlignment.Left,
		Parent = InputWrapper
	})
	AddThemeObject(KeyBox, "Text")
	
	local VerifyButton = Create("Frame", {
		Size = UDim2.new(0, 158, 0, 38),
		Position = UDim2.new(0, 15, 0, 142),
		BackgroundColor3 = ActiveTheme.Second,
		BorderSizePixel = 0,
		Parent = MainFrame
	}, {
		Create("UICorner", { CornerRadius = UDim.new(0, 6) })
	})
	local VerifyStroke = Create("UIStroke", { Color = ActiveTheme.Stroke, Thickness = 1, Parent = VerifyButton })
	AddThemeObject(VerifyButton, "Second")
	AddThemeObject(VerifyStroke, "Stroke")
	
	local VerifyText = Create("TextLabel", {
		Size = UDim2.new(1, 0, 1, 0),
		BackgroundTransparency = 1,
		Text = "VERIFY KEY",
		TextColor3 = ActiveTheme.Text,
		Font = Enum.Font.Code,
		TextSize = 13,
		Parent = VerifyButton
	})
	AddThemeObject(VerifyText, "Text")
	
	local VerifyTrigger = Create("TextButton", { Size = UDim2.new(1, 0, 1, 0), Text = "", BackgroundTransparency = 1, Parent = VerifyButton })
	
	local GetKeyButton = Create("Frame", {
		Size = UDim2.new(0, 158, 0, 38),
		Position = UDim2.new(1, -173, 0, 142),
		BackgroundColor3 = ActiveTheme.Second,
		BorderSizePixel = 0,
		Parent = MainFrame
	}, {
		Create("UICorner", { CornerRadius = UDim.new(0, 6) })
	})
	local GetKeyStroke = Create("UIStroke", { Color = ActiveTheme.Stroke, Thickness = 1, Parent = GetKeyButton })
	AddThemeObject(GetKeyButton, "Second")
	AddThemeObject(GetKeyStroke, "Stroke")
	
	local GetKeyText = Create("TextLabel", {
		Size = UDim2.new(1, 0, 1, 0),
		BackgroundTransparency = 1,
		Text = "GET CODE LINK",
		TextColor3 = ActiveTheme.Text,
		Font = Enum.Font.Code,
		TextSize = 13,
		Parent = GetKeyButton
	})
	AddThemeObject(GetKeyText, "Text")
	
	local GetKeyTrigger = Create("TextButton", { Size = UDim2.new(1, 0, 1, 0), Text = "", BackgroundTransparency = 1, Parent = GetKeyButton })
	
	AddConnection(VerifyTrigger.MouseButton1Click, function()
		if KeyBox.Text == ExpectedKey then
			StatusLabel.Text = "Authentication Success! Loading Hub..."
			StatusLabel.TextColor3 = Color3.fromRGB(80, 255, 100)
			FrameStroke.Color = Color3.fromRGB(80, 255, 100)
			
			task.wait(1)
			Xen0KeyGui:Destroy()
			OnSuccess()
		else
			StatusLabel.Text = "Authentication Failed: Incorrect Key"
			StatusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
			FrameStroke.Color = Color3.fromRGB(255, 100, 100)
			
			task.delay(2, function()
				if MainFrame and FrameStroke then
					StatusLabel.Text = "Status: Awaiting Verification"
					StatusLabel.TextColor3 = ActiveTheme.TextDark
					FrameStroke.Color = ActiveTheme.Stroke
				end
			end)
		end
	end)
	
	AddConnection(GetKeyTrigger.MouseButton1Click, function()
		if setclipboard then
			setclipboard(Linkvertise)
			StatusLabel.Text = "URL successfully pushed to clipboard!"
			StatusLabel.TextColor3 = ActiveTheme.Text
		else
			StatusLabel.Text = "Clipboard execution error."
			StatusLabel.TextColor3 = Color3.fromRGB(255, 150, 50)
		end
	end)
end

return KeySystem
