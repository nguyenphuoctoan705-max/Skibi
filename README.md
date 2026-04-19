pcall(function()
repeat task.wait() until game:IsLoaded()

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = LocalPlayer.PlayerGui
ScreenGui.ResetOnSpawn = false

local Frame = Instance.new("Frame",ScreenGui)
Frame.Size = UDim2.new(0,230,0,260)
Frame.Position = UDim2.new(0,20,0,100)
Frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
Frame.Active = true
Frame.Draggable = true

local Title = Instance.new("TextLabel",Frame)
Title.Size = UDim2.new(1,0,0,25)
Title.Text = "toàn hub"
Title.TextColor3 = Color3.new(1,1,1)
Title.BackgroundTransparency = 1

local Credit = Instance.new("TextLabel",Frame)
Credit.Size = UDim2.new(1,0,0,20)
Credit.Position = UDim2.new(0,0,0,25)
Credit.Text = "Made by Toàn"
Credit.TextColor3 = Color3.new(1,1,1)
Credit.BackgroundTransparency = 1

-- Buttons
local ESPBtn = Instance.new("TextButton",Frame)
ESPBtn.Size = UDim2.new(1,-20,0,30)
ESPBtn.Position = UDim2.new(0,10,0,55)
ESPBtn.Text = "ESP OFF"

local KillBtn = Instance.new("TextButton",Frame)
KillBtn.Size = UDim2.new(1,-20,0,30)
KillBtn.Position = UDim2.new(0,10,0,95)
KillBtn.Text = "Auto Kill OFF"

local SpeedBtn = Instance.new("TextButton",Frame)
SpeedBtn.Size = UDim2.new(1,-20,0,30)
SpeedBtn.Position = UDim2.new(0,10,0,135)
SpeedBtn.Text = "Speed OFF"

local PlayerBox = Instance.new("TextBox",Frame)
PlayerBox.Size = UDim2.new(1,-20,0,30)
PlayerBox.Position = UDim2.new(0,10,0,175)
PlayerBox.PlaceholderText = "Tên người chơi"

local TeleportBtn = Instance.new("TextButton",Frame)
TeleportBtn.Size = UDim2.new(1,-20,0,30)
TeleportBtn.Position = UDim2.new(0,10,0,215)
TeleportBtn.Text = "Teleport"

-- Variables
local ESP = false
local AutoKill = false
local Speed = false

-- ESP
local function ESPPlayer(player,color,text)
if player == LocalPlayer then return end

local function apply(char)
if not ESP then return end

if char:FindFirstChild("ESP") then
char.ESP:Destroy()
end

local bill = Instance.new("BillboardGui",char)
bill.Name = "ESP"
bill.Size = UDim2.new(0,100,0,40)
bill.AlwaysOnTop = true
bill.Adornee = char:FindFirstChild("Head")

local label = Instance.new("TextLabel",bill)
label.Size = UDim2.new(1,0,1,0)
label.BackgroundTransparency = 1
label.Text = text
label.TextColor3 = color
label.TextStrokeTransparency = 0

end

if player.Character then
apply(player.Character)
end

player.CharacterAdded:Connect(apply)

end

-- ESP Loop
task.spawn(function()
while task.wait(1) do
if ESP then
for _,v in pairs(Players:GetPlayers()) do
if v ~= LocalPlayer then

if v.Backpack:FindFirstChild("Knife") or (v.Character and v.Character:FindFirstChild("Knife")) then
ESPPlayer(v,Color3.fromRGB(255,0,0),"🔪 Murder")

elseif v.Backpack:FindFirstChild("Gun") then
ESPPlayer(v,Color3.fromRGB(0,150,255),"🔫 Sheriff")

else
ESPPlayer(v,Color3.fromRGB(0,255,0),"🙂 Innocent")
end

end
end
end
end
end)

-- Auto Kill
task.spawn(function()
while task.wait(0.2) do

if not AutoKill then continue end

local char = LocalPlayer.Character
if not char then continue end

local knife = LocalPlayer.Backpack:FindFirstChild("Knife") or char:FindFirstChild("Knife")

if knife then

local hrp = char:FindFirstChild("HumanoidRootPart")
if not hrp then continue end

local nearest
local dist = math.huge

for _,v in pairs(Players:GetPlayers()) do
if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then

local hum = v.Character:FindFirstChild("Humanoid")
if hum and hum.Health > 0 then

local d = (hrp.Position - v.Character.HumanoidRootPart.Position).Magnitude

if d < dist then
dist = d
nearest = v
end

end
end
end

if nearest then
hrp.CFrame = nearest.Character.HumanoidRootPart.CFrame
end

end
end
end)

-- Speed
task.spawn(function()
while task.wait() do
if Speed and LocalPlayer.Character then
LocalPlayer.Character.Humanoid.WalkSpeed = 30
end
end
end)

-- Teleport
TeleportBtn.MouseButton1Click:Connect(function()

for _,v in pairs(Players:GetPlayers()) do
if string.lower(v.Name):find(string.lower(PlayerBox.Text)) then

if v.Character and LocalPlayer.Character then
LocalPlayer.Character.HumanoidRootPart.CFrame =
v.Character.HumanoidRootPart.CFrame
end

end
end

end)

-- Toggle
ESPBtn.MouseButton1Click:Connect(function()
ESP = not ESP
ESPBtn.Text = ESP and "ESP ON" or "ESP OFF"
end)

KillBtn.MouseButton1Click:Connect(function()
AutoKill = not AutoKill
KillBtn.Text = AutoKill and "Auto Kill ON" or "Auto Kill OFF"
end)

SpeedBtn.MouseButton1Click:Connect(function()
Speed = not Speed
SpeedBtn.Text = Speed and "Speed ON" or "Speed OFF"
end)

-- Hide Menu
UserInputService.InputBegan:Connect(function(input,gp)
if gp then return end
if input.KeyCode == Enum.KeyCode.RightShift then
Frame.Visible = not Frame.Visible
end
end)

end)
