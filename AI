-- Senko

-- Variables

local Model = script:WaitForChild("Sword")
local MainEvent = script.Parent:WaitForChild("Main")
local Cooldowns = {};

local ActiveBlocks = {};

local CP = game:GetService("ContentProvider")

-- Functions

for _,v in pairs(script:WaitForChild("Anims"):GetChildren()) do
	if v:IsA('Animation') then
		CP:PreloadAsync({v})
	end
end

MainEvent.OnServerEvent:Connect(function(plr, action, array)
	if action == 'Equip' then
		
		local Clone = Model:Clone()
		Clone.Parent = plr.Character
		
		Clone.Name = plr.Name.. " 's Ais Sword"
		
		local Motor = Instance.new("Motor6D", Clone)
		Motor.Name = 'Weld'
		
		Motor.Part0 = plr.Character['Right Arm']
		Motor.Part1 = Clone
		
		Motor.C0 = CFrame.new(0,-1,-2.5) * CFrame.Angles(0,math.rad(90), 0)
		
	end
	
	if action == 'SpecialMove' then
		local Name = array['Name']
		
		if Name == 'Avenger' then
			if script.Skills[Name] then
				if Cooldowns[plr.UserId..'-Avenger'] then return end
				
				Cooldowns[plr.UserId..'-Avenger'] = true
				
				local CurrentAm = 0
				
				
				local Field = script.Skills[Name].Field:Clone()
				Field.Parent = workspace
				
				Field.Name = plr.Name.. " 's Avenger Skill Field"
				Field.CFrame = plr.Character.PrimaryPart.CFrame * CFrame.new(0,-4.5,0) * CFrame.Angles(0,0,math.rad(90))
				
				coroutine.wrap(function()
				
					for i = 1,0.75,-0.01 do
						Field.Transparency = i 
						wait()
					end
				end)()
				
				coroutine.wrap(function()
					wait(2)
					
					for i = 0.75,1,0.01 do
						Field.Transparency = i 
						wait()
					end
					Field:Destroy()
					
				end)()
				
				for i = 1,50 do
					CurrentAm = CurrentAm + 1
					
					local x,z = plr.Character.Torso.Position.X, plr.Character.Torso.Position.Z
					x,z = math.random(x-3, x+3), math.random(z-3, z+3)
					
					local pos = Vector3.new(x, plr.Character.Torso.Position.Y, z)
					
					local c = script.Skills[Name].Sword:Clone()
					c.Parent = plr.Character
					c.Position = pos
					
					c.CFrame = c.CFrame * CFrame.Angles(0, math.rad(math.random(0,90)), 0)
										
					--c.CFrame = Field.CFrame * CFrame.new(math.random(1,3) , 2, math.random(1,3)) * CFrame.Angles(0,0,math.rad(-90))
					c.Script.Disabled = false
					wait(0.3)
				end
				
				if CurrentAm == 50 then
					wait(10)
					
					Cooldowns[plr.UserId..'-Avenger'] = nil
				end
				
			end
			
		end
		
	end
	
	if action == 'Block' then
		
		local bool = array['Bool']
		
		if bool == false then
			-- Stop block
			
			--// Stop anim
			
			--// 
			
			ActiveBlocks[plr.UserId].Effect:Destroy()
			ActiveBlocks[plr.UserId].Anim:Stop(0.25)
			
			ActiveBlocks[plr.UserId] = nil
			
			plr.Character.Humanoid.WalkSpeed = 16 -- 16
			plr.Character.Humanoid.JumpPower = 50 -- 50
			
			MainEvent:FireClient(plr, 'ChangeBlock', false) -- changing block bool
			
			coroutine.wrap(function()
				wait(1)
				Cooldowns[plr.UserId..'-Block'] = nil
			end)()
			
		elseif bool == true then
			-- Block
			
			if Cooldowns[plr.UserId..'-Block'] then return end
			Cooldowns[plr.UserId..'-Block'] = true
			
			--// Play anim
			
			local BlockEff = script:WaitForChild("BlockEff"):Clone()
			BlockEff.Parent = plr.Character
			
			local Weld = Instance.new("Motor6D", BlockEff)
			Weld.Part0 = plr.Character.PrimaryPart
			Weld.Part1 = BlockEff
			
			Weld.C0 = CFrame.new(0,-1,-2) * CFrame.Angles(0,math.rad(-90), 0)
			
			--// Tweening effect.
			
		
			local Anima = plr.Character.Humanoid:LoadAnimation(script.Anims.Block)
			Anima:Play()
			
			ActiveBlocks[plr.UserId] = {
				Effect = BlockEff,
				Anim = Anima,
			};
			
			
			
			plr.Character.Humanoid.WalkSpeed = 4 -- 16
			plr.Character.Humanoid.JumpPower = 0 -- 50
			
			MainEvent:FireClient(plr, 'ChangeBlock', true) -- changing block bool
		end
		
	end
	
	if action == 'Attack' then
		
		local anim = plr.Character.Humanoid:LoadAnimation(script.Anims.Attack["1"])
		
		anim:Play()
		
	end
	
	if action == 'WeldEquip' then
		if Cooldowns[plr.UserId..'-Equip'] then return end
		
		
		local Bool = array['Bool']
		
		if Bool == false then
			
			Cooldowns[plr.UserId..'-Equip'] = true
			
			-- UnEquip
			
			local Anim = plr.Character.Humanoid:LoadAnimation(script.Anims.UnEquip)
			Anim:Play()
			
			Anim.Stopped:Wait()
			
			plr.Character:WaitForChild(plr.Name.. " 's Ais Sword"):Destroy()
			
			local Weld = script.MainSword:Clone()
			Weld.Parent = plr.Character
			
			Weld.Name = 'UnEquip Ais Sword'
						
			local motor = Instance.new("Motor6D", Weld)
			motor.Name = 'Torso'
			motor.Part0 = plr.Character.Torso
			
			motor.Part1 = Weld
			
			motor.C0 = CFrame.Angles(0, math.rad(-90), 0) * CFrame.new(1.3,-1,1) * CFrame.Angles(0,0, math.rad(-45)) * CFrame.new(0.4,-1.2, 0.1)
			
			coroutine.wrap(function()
				wait(1)
				Cooldowns[plr.UserId..'-Equip'] = nil
			end)()
			
		end
		
		if Bool == true then
			Cooldowns[plr.UserId..'-Equip'] = true
			-- Equip
			
			--// Unwelding
			
			local unequipSword = plr.Character:WaitForChild('UnEquip Ais Sword')
			if not unequipSword then warn'ugh' return end
			
			unequipSword.Torso:Destroy()
			
			local newWeld = Instance.new("Motor6D", unequipSword)
			newWeld.Part0 = plr.Character['Right Arm']
			
			newWeld.Part1 = unequipSword
			
			newWeld.C0 = CFrame.new(0,-1,-2.5) * CFrame.Angles(0,math.rad(90), 0)
			
			
			local Anim = plr.Character.Humanoid:LoadAnimation(script.Anims.Equip)
			Anim:Play()
			
			Anim.Stopped:Wait()
			
		
			local Clone = Model:Clone()
			Clone.Parent = plr.Character
			
			Clone.Name = plr.Name.. " 's Ais Sword"
			
			local Motor = Instance.new("Motor6D", Clone)
			Motor.Name = 'Weld'
			
			Motor.Part0 = plr.Character['Right Arm']
			Motor.Part1 = Clone
			
			Motor.C0 = CFrame.new(0,-1,-2.5) * CFrame.Angles(0,math.rad(90), 0)
			
			unequipSword:Destroy()
			
			coroutine.wrap(function()
				wait(1)
				Cooldowns[plr.UserId..'-Equip'] = nil
			end)()
			
		end
		
	end
end)

script.Parent.Sub.OnServerInvoke = function(plr, action, other)
	if action == 'Check' then
		
		local toCheck = other
		
		if Cooldowns[plr.UserId..'-'.. toCheck] then
			return true
		else
			return false
		end
		
	end
end
