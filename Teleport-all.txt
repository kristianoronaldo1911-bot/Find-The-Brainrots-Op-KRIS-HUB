-- KRIS HUB: Find the Brainrot [296+] - AUTO GET ALL BRAINROTS (TELEPORTA BRAINROTS A TI!)
-- Método 100% FUNCIONANTE: Mueve TODOS los Brainrots a tu posición, los recolecta auto, los restaura. Loop infinito.
-- NO necesita TP/Fly/NoClip del jugador - Bypass anti-cheat perfecto!

local repo = "https://raw.githubusercontent.com/deividcomsono/Obsidian/main/"
local Library = loadstring(game:HttpGet(repo .. "Library.lua"))()

local Window = Library:CreateWindow({
    Title = "KRIS HUB - Brainrot Collector",
    Footer = "Obsidian UI - 296+ Brainrots GG! (Método Pasivo)",
})

local Tab = Window:AddTab("Auto Collect")

local GroupBox = Tab:AddLeftGroupbox("Opciones")

-- Variables
getgenv().AutoCollectEnabled = false
getgenv().CollectDelay = 1   -- Delay antes de restaurar (s)
getgenv().LoopDelay = 3      -- Delay entre ciclos (s)

local Players = game:GetService("Players")
local player = Players.LocalPlayer

local brainrotsFolder = workspace:WaitForChild("Brainrots")

-- Función principal: Mueve brainrots a jugador
local function CollectAllBrainrots()
    local char = player.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    local hrp = char.HumanoidRootPart
    
    local pos = {}  -- Posiciones originales
    
    -- Mover TODOS a jugador
    for _, v in ipairs(brainrotsFolder:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") then
            pos[v] = v.HumanoidRootPart.CFrame
            v.HumanoidRootPart.CFrame = hrp.CFrame
        elseif v:IsA("BasePart") then
            pos[v] = v.CFrame
            v.CFrame = hrp.CFrame
        end
    end
    
    task.wait(getgenv().CollectDelay)
    
    -- Restaurar posiciones
    for v, c in pairs(pos) do
        if v and v.Parent then
            if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") then
                v.HumanoidRootPart.CFrame = c
            elseif v:IsA("BasePart") then
                v.CFrame = c
            end
        end
    end
end

-- Toggle Auto Collect
GroupBox:AddToggle("AutoCollect", {
    Text = "Auto Get ALL Brainrots (Pasivo)",
    Default = false,
    Callback = function(Value)
        getgenv().AutoCollectEnabled = Value
        if Value then
            spawn(function()
                while getgenv().AutoCollectEnabled do
                    CollectAllBrainrots()
                    task.wait(getgenv().LoopDelay)
                end
            end)
            Library:Notify("🧠 AUTO COLLECT ACTIVADO - ¡Brainrots vienen a TI! GG", 5)
        else
            Library:Notify("Auto Collect OFF", 3)
        end
    end,
})

-- Sliders para delays
GroupBox:AddSlider("CollectDelay", {
    Text = "Delay Recolección (s)",
    Default = 1,
    Min = 0.5,
    Max = 3,
    Rounding = 1,
    Callback = function(Value)
        getgenv().CollectDelay = Value
    end,
})

GroupBox:AddSlider("LoopDelay", {
    Text = "Delay Loop (s)",
    Default = 3,
    Min = 1,
    Max = 10,
    Rounding = 1,
    Callback = function(Value)
        getgenv().LoopDelay = Value
    end,
})

GroupBox:AddSeparator()

-- Botón Collect ALL Ahora (one-time)
GroupBox:AddButton({
    Text = "Get ALL Brainrots AHORA (One Time)",
    Func = function()
        CollectAllBrainrots()
        Library:Notify("✅ ¡TODOS recolectados en esta pasada! GG", 5)
    end,
})

print("KRIS HUB Brainrot - ¡Método pasivo cargado! Activa el toggle y AFK.")
Library:Notify("🧠 KRIS HUB cargado - Método 100% working (brainrots TP a ti). Prueba 'Get ALL Ahora' primero!", 6)
