# Scriptmain.lua
monte man yesss
loadstring([[
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

local flying = false
local speed = 5
local bodyVelocity, bodyGyro

local function startFly(char)
    local hrp = char:WaitForChild("HumanoidRootPart")

    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyVelocity.Velocity = Vector3.zero
    bodyVelocity.Parent = hrp

    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
    bodyGyro.CFrame = hrp.CFrame
    bodyGyro.Parent = hrp

    flying = true

    while flying and hrp.Parent do
        local moveDir = Vector3.zero

        -- Unterstützt PC, Mobile & Controller via MoveDirection
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        if humanoid then
            moveDir = humanoid.MoveDirection
        end

        bodyVelocity.Velocity = moveDir * speed * 10
        bodyGyro.CFrame = workspace.CurrentCamera.CFrame

        task.wait()
    end
end

local function stopFly()
    flying = false
    if bodyVelocity then bodyVelocity:Destroy() end
    if bodyGyro then bodyGyro:Destroy() end
end

-- Chat Commands
player.Chatted:Connect(function(msg)
    local args = string.split(msg, " ")

    if args[1] == ";fly" then
        speed = tonumber(args[2]) or 5
        local char = player.Character or player.CharacterAdded:Wait()
        stopFly()
        startFly(char)

    elseif args[1] == ";unfly" then
        stopFly()
    end
end)
]])()
