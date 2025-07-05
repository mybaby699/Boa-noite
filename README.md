-- Piot Script V1
-- Gui preta zueira para Brookhaven (Delta/mobile)

local Players = game:GetService("Players")
local UserInput = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local SoundService = game:GetService("SoundService")
local LocalPlayer = Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "PiotScriptGui"

local open = false
local bg = Instance.new("Frame", ScreenGui)
bg.Size = UDim2.new(0,240,0,500)
bg.Position = UDim2.new(0,20,0,20)
bg.BackgroundColor3 = Color3.new(0,0,0)
bg.BorderSizePixel = 0
bg.Visible = false

local openBtn = Instance.new("TextButton", ScreenGui)
openBtn.Size = UDim2.new(0,40,0,40)
openBtn.Position = UDim2.new(0,20,0,20)
openBtn.Text = "📂"
openBtn.Font = Enum.Font.GothamBold
openBtn.TextSize = 24
openBtn.BackgroundColor3 = Color3.new(1,1,1)
openBtn.MouseButton1Click:Connect(function()
    open = not open
    bg.Visible = open
end)

local function newBtn(text, y, func)
    local btn = Instance.new("TextButton", bg)
    btn.Size = UDim2.new(0,200,0,30)
    btn.Position = UDim2.new(0,20,0,y)
    btn.Text = text
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 18
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.MouseButton1Click:Connect(func)
    return btn
end

-- 1) Fly
local flyOn = false
local bv
newBtn("1 - Fly", 20, function()
    flyOn = not flyOn
    if flyOn then
        bv = Instance.new("BodyVelocity", LocalPlayer.Character.HumanoidRootPart)
        bv.MaxForce = Vector3.new(9e4,9e4,9e4)
        RunService.RenderStepped:Connect(function()
            if flyOn then
                local cam = workspace.CurrentCamera
                bv.Velocity = cam.CFrame.LookVector * 50
            end
        end)
    else
        if bv then bv:Destroy() end
    end
end)

-- 2) Copiar skin FE
newBtn("2 - Copy Skin FE", 60, function()
    local pl, dist = nil, math.huge
    for _, p in pairs(Players:GetPlayers()) do
        if p~=LocalPlayer and p.Character and p.Character:FindFirstChildOfClass("HumanoidRootPart") then
            local d = (p.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if d<dist then pl,dist = p,d end
        end
    end
    if pl and pl.Character then
        local desc = pl.Character:FindFirstChildOfClass("Humanoid"):GetAppliedDescription()
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ApplyDescription(desc)
    end
end)

-- 3) Nick RGB
newBtn("3 - Nick RGB", 100, function()
    spawn(function()
        while bg.Visible do
            LocalPlayer.NameDisplayDistance = 0 -- hide default
            local colors = {Color3.new(1,0,0), Color3.new(0,1,0), Color3.new(0,0,1)}
            for i,c in ipairs(colors) do
                LocalPlayer:SetAttribute("NickColor", c)
                wait(0.5)
            end
        end
    end)
end)

-- 4) Danças R15
local danceIDs = {507770239,507770083,507770235,507770188,507770136}
newBtn("4 - Danças", 140, function()
    spawn(function()
        while bg.Visible do
            for _, id in ipairs(danceIDs) do
                local a = Instance.new("Animation")
                a.AnimationId = "rbxassetid://"..id
                local t = LocalPlayer.Character.Humanoid:LoadAnimation(a)
                t:Play()
                wait(t.Length+1)
                t:Stop()
            end
        end
    end)
end)

-- 5) Pulo infinito
local infJump = false
newBtn("5 - Pulo Infinito", 180, function()
    infJump = not infJump
    if infJump then
        UserInput.JumpRequest:Connect(function()
            LocalPlayer.Character.Humanoid:ChangeState("Jumping")
        end)
    end
end)

-- 6) Noclip
local noclipOn = false
newBtn("6 - Noclip", 220, function()
    noclipOn = not noclipOn
    if noclipOn then
        RunService.Stepped:Connect(function()
            for _, p in pairs(LocalPlayer.Character:GetDescendants()) do
                if p:IsA("BasePart") then
                    p.CanCollide = false
                end
            end
        end)
    end
end)

-- 7) Lag server
newBtn("7 - Lag Server", 260, function()
    spawn(function()
        for i=1,50 do
            local p = Instance.new("Part",Workspace)
            p.Size=Vector3.new(10,10,10)
            p.Position=Vector3.new(math.random(-500,500),math.random(10,50),math.random(-500,500))
            p.Anchored=true
            p.Transparency=1
            wait(0.1)
            p:Destroy()
        end
    end)
end)

-- 8) Desenho no céu “XD”
newBtn("8 - Sky XD", 300, function()
    for i=1,#"XD" do
        local let = string.sub("XD",i,i)
        local c = Instance.new("Part", Workspace)
        c.Size = Vector3.new(20,1,20)
        c.Position = LocalPlayer.Character.HumanoidRootPart.Position + Vector3.new(0,100,0) + Vector3.new((i-1)*25,0,0)
        c.Anchored = true
        c.BrickColor = BrickColor.Random()
    end
end)

-- 9) Sons trolls
local sounds = {
    {"Gemidão","rbxassetid://9118825191"},
    {"Zap Whats","rbxassetid://1234567"},
    {"Boa noite Bruno","rbxassetid://2345678"},
    {"Pede iFood","rbxassetid://3456789"},
    {"Humm será?","rbxassetid://4567890"},
    {"Ela tá querendo","rbxassetid://5678901"},
    {"Sirene Pol","rbxassetid://138248981"},
    {"Pega o Jack","
