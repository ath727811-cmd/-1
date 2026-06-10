-- تفعيل الميزات الأساسية: السرعة، كشف الأماكن، والاختفاء

local Player = game:GetService("Players").LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()

---------------------------------------------------------
-- 1. ميزة التسريع (Speedhack)
-- يمكنك تغيير الرقم 100 للسرعة التي تناسبك (السرعة الطبيعية هي 16)
---------------------------------------------------------
local CustomSpeed = 100
if Character:FindFirstChildOfClass("Humanoid") then
    Character:FindFirstChildOfClass("Humanoid").WalkSpeed = CustomSpeed
end

Player.CharacterAdded:Connect(function(NewCharacter)
    local Humanoid = NewCharacter:WaitForChild("Humanoid")
    Humanoid.WalkSpeed = CustomSpeed
end)

---------------------------------------------------------
-- 2. ميزة كشف أماكن اللاعبين (ESP / Wallhack)
-- بتعمل صندوق (Box) واسم فوق كل لاعب عشان تشوفه من ورا الحيطان
---------------------------------------------------------
local function CreateESP(TargetPlayer)
    if TargetPlayer == Player then return end

    local function ApplyESP(Char)
        local Highlight = Instance.new("Highlight")
        Highlight.Name = "ESP_Highlight"
        Highlight.FillColor = Color3.fromRGB(255, 0, 0) -- لون أحمر للكشف
        Highlight.FillTransparency = 0.5
        Highlight.OutlineColor = Color3.fromRGB(255, 255, 255) -- إطار أبيض
        Highlight.OutlineTransparency = 0
        Highlight.Parent = Char

        -- إضافة اسم اللاعب فوق رأسه
        local Billboard = Instance.new("BillboardGui")
        Billboard.Name = "ESP_Name"
        Billboard.Size = UDim2.new(0, 200, 0, 50)
        Billboard.StudsOffset = Vector3.new(0, 3, 0)
        Billboard.AlwaysOnTop = true
        
        local Label = Instance.new("TextLabel")
        Label.Size = UDim2.new(1, 0, 1, 0)
        Label.BackgroundTransparency = 1
        Label.Text = TargetPlayer.Name
        Label.TextColor3 = Color3.fromRGB(255, 255, 255)
        Label.TextStrokeTransparency = 0
        Label.TextSize = 14
        Label.Parent = Billboard
        
        local Head = Char:WaitForChild("Head", 5)
        if Head then Billboard.Parent = Head end
    end

    if TargetPlayer.Character then ApplyESP(TargetPlayer.Character) end
    TargetPlayer.CharacterAdded:Connect(ApplyESP)
end

-- تفعيل الكشف لجميع اللاعبين الحاليين والجدد
for _, v in pairs(game:GetService("Players"):GetPlayers()) do
    CreateESP(v)
end
game:GetService("Players").PlayerAdded:Connect(CreateESP)

---------------------------------------------------------
-- 3. ميزة الاختفاء (Invisibility)
-- بتخلي جسمك شفاف تماماً بالنسبة للاعبين الآخرين
---------------------------------------------------------
local function MakeInvisible(Char)
    for _, part in pairs(Char:GetDescendants()) do
        if part:IsA("BasePart") or part:IsA("Decal") then
            if part.Name ~= "HumanoidRootPart" then
                part.Transparency = 1 -- 1 تعني اختفاء كامل
            end
        end
    end
end

if Character then MakeInvisible(Character) end
Player.CharacterAdded:Connect(MakeInvisible)

print("تم تفعيل السكربت بنجاح: السرعة، كشف الأماكن، والاختفاء شغالين!")
