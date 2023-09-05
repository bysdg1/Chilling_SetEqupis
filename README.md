Stats_Units = {}
Equpi_Unit = {}
function Open_Units()
    if not game:GetService("Players").LocalPlayer.PlayerGui.collection.Enabled then 
        game:GetService("VirtualInputManager"):SendKeyEvent(true,"K",false,game)
        wait(0.1)
        game:GetService("VirtualInputManager"):SendKeyEvent(false,"K",false,game)
        wait(3)
    end
end

function Mouse_Click(x, y)
    Mouse_X = x + 10
    Mouse_Y = y + 50
    game:GetService("VirtualInputManager"):SendMouseButtonEvent(Mouse_X, Mouse_Y, 0, true, game, 1)
    game:GetService("VirtualInputManager"):SendMouseButtonEvent(Mouse_X, Mouse_Y, 0, false, game, 1)
end

function Click_Attack()
    local Attack = game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.Banner.SortModes:GetChildren()
    for i,v in pairs(Attack) do 
        if v['Name'] == 'Attack' then
            Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y-5)
            wait(1)
        end
    end
end

function Check_Stats()
    local Data_Stats = {}
    for i,v in pairs(Stats_Units) do
        for i1,v1 in pairs(v['Attributes']) do 
            if v1 == 'GRND' then 
                table.insert(Data_Stats, v['Stats'])
            end
        end
    end
    return Data_Stats
end

function Check_Equpis(value ,NameUnits)
    for i2,v2 in pairs(Equpi_Unit) do 
        warn(v2 == NameUnits)
        if v2 == NameUnits then 
            func_return = true
            return func_return
        end
    end
end

function UnEqupi_All()
    local UE_ALL = game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.BottomMain.UnequipAll:GetChildren()
    for i,v in pairs(UE_ALL) do
        if v['Name'] == 'Main' then 
            Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y)
            wait(1)
        end
    end
end

function Save_Stats_Units(uuid)
    local Units_Option_Attributes = {}
    local Attributes = game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.UnitOptions.Main.Desc.Attributes:GetChildren()
    for i,v in pairs(Attributes) do 
        if v['ClassName'] == 'TextButton' then
            table.insert(Units_Option_Attributes, v.text.Text)
        end
    end
    local Units_Option_Name       = game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.UnitOptions.Main.Desc.name.Text
    local Units_Stats             = game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.UnitStats.Attack.Main.TextLabel.Text

    local Table_Data = {
        Name = Units_Option_Name,
        UUID = uuid,
        Attributes = Units_Option_Attributes,
        Stats = Units_Stats
    }
    table.insert(Stats_Units, Table_Data)
end

function Check_Units()
    Player = 1
    while task.wait() do
        local Units_Count , _ = string.gsub(game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.List.Top.Capacity.Text, "/100","")
        if Player <= tonumber(Units_Count) and Player <= 17 then
            local Collection = game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.List.Outer.UnitFrames:GetChildren()
            for i,v in pairs(Collection) do  
                if v['Name'] == "CollectionUnitFrame" then 
                    if v['LayoutOrder'] == Player then 
                        if game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.UnitOptions.Visible == true then 
                            Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y)
                        end

                        if game:GetService("Players").LocalPlayer.PlayerGui.collection.grid.UnitOptions.Visible == false then 
                            Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y)
                            task.wait(.5)
                            Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y)
                            Save_Stats_Units(v['_uuid'].Value)
                            Player = Player + 1
                            task.wait(1)
                        end
                    end
                end
            end
        else
            print("break")
            break
        end
    end
end
function Equpi_Units_1()
    for i,v in pairs(Stats_Units) do
        for i1,v1 in pairs(v['Attributes']) do 
            if v1 == 'GRND' then 
                local Value_Attack = math.max(table.unpack(Check_Stats()))
                if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                    warn('Value_Attack 1', math.floor(v['Stats']) ,' ', math.floor(Value_Attack))
                    wait(1)
                    game:GetService("ReplicatedStorage").endpoints.client_to_server.equip_unit:InvokeServer(v['UUID'])
                    wait(1)
                    table.insert(Equpi_Unit, v['Name'])
                    table.remove(Stats_Units, i)
                    wait(1)
                    return
                end
            end
        end
    end
end
function Equpi_Units_2()
    select_units2 = false
    while task.wait() do
        for i,v in pairs(Stats_Units) do
            for i1,v1 in pairs(v['Attributes']) do 
                if v1 == 'GRND' then 
                    local Value_Attack = math.max(table.unpack(Check_Stats()))
                    if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                        if Check_Equpis(v, v['Name']) then 
                            print('remove ,', i)
                            table.remove(Stats_Units, i)
                            select_units2 = true
                        end
                    else
                        select_units2 = false
                    end
                    if not select_units2 then 
                        if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                            warn('Value_Attack 2', math.floor(v['Stats']) ,' ', math.floor(Value_Attack))
                            game:GetService("ReplicatedStorage").endpoints.client_to_server.equip_unit:InvokeServer(v['UUID'])
                            wait(1)
                            table.insert(Equpi_Unit, v['Name'])
                            table.remove(Stats_Units, i)
                            wait(1)
                            return
                        end
                    end
                end
            end
        end
    end
end
function Equpi_Units_3()
    select_units3 = false
    while task.wait() do
        for i,v in pairs(Stats_Units) do
            for i1,v1 in pairs(v['Attributes']) do 
                if v1 == 'GRND' then 
                    local Value_Attack = math.max(table.unpack(Check_Stats()))
                    if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                        if Check_Equpis(v, v['Name']) then 
                            print('remove ,', i)
                            table.remove(Stats_Units, i)
                            select_units3 = true
                        end
                    else
                        select_units3 = false
                    end
                    if not select_units3 then 
                        if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                            warn('Value_Attack 3', math.floor(v['Stats']) ,' ', math.floor(Value_Attack))
                            game:GetService("ReplicatedStorage").endpoints.client_to_server.equip_unit:InvokeServer(v['UUID'])
                            wait(1)
                            table.insert(Equpi_Unit, v['Name'])
                            table.remove(Stats_Units, i)
                            wait(1)
                            return
                        end
                    end
                end
            end
        end
    end
end
function Equpi_Units_4()
    select_units4 = false
    while task.wait() do
        for i,v in pairs(Stats_Units) do
            for i1,v1 in pairs(v['Attributes']) do 
                if v1 == 'GRND' then 
                    local Value_Attack = math.max(table.unpack(Check_Stats()))
                    if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                        if Check_Equpis(v, v['Name']) then 
                            print('remove ,', i)
                            table.remove(Stats_Units, i)
                            select_units4 = true
                        end
                    else
                        select_units4 = false
                    end
                    if not select_units4 then 
                        if math.floor(v['Stats']) == math.floor(Value_Attack) then 
                            warn('Value_Attack 4', math.floor(v['Stats']) ,' ', math.floor(Value_Attack))
                            game:GetService("ReplicatedStorage").endpoints.client_to_server.equip_unit:InvokeServer(v['UUID'])
                            wait(1)
                            table.insert(Equpi_Unit, v['Name'])
                            table.remove(Stats_Units, i)
                            wait(1)
                            return
                        end
                    end
                end
            end
        end
    end
end

function Reedemcode()
    codes = {"TWOMILLION","subtomaokuma","CHALLENGEFIX","GINYUFIX","RELEASE","SubToKelvingts","SubToBlamspot","KingLuffy","TOADBOIGAMING","noclypso","FictioNTheFirst","GOLDENSHUTDOWN","GOLDEN"
    ,"SINS2","subtosnowrbx","Cxrsed","subtomaokuma","VIGILANTE","HAPPYEASTER","ENTERTAINMENT","DRESSROSA","BILLION","MADOKA","AINCRAD","ANNIVERSARY","OVERLORD","SupperTierMagicSoon",
    "NEWCODE0819"}
    for _, v in pairs(codes) do
        pcall(function() game:GetService("ReplicatedStorage").endpoints["client_to_server"]["redeem_code"]:InvokeServer(v)()    end) 
    end
    wait(1)
end

function SummonUnits()
    while task.wait() do 
        local gems_count = game.Players.LocalPlayer:FindFirstChild("_stats").gem_amount.Value
        if gems_count >= 50 then 
            local args = {
                [1] = tostring("EventClover"),
                [2] = tostring("gems")
            }
            game:GetService("ReplicatedStorage").endpoints.client_to_server.buy_from_banner:InvokeServer(unpack(args)) 
            wait(1.5)
        else
            return
        end
    end
end

function Click_Claim()
    local Button = game:GetService("Players").LocalPlayer.PlayerGui.DailyRewards.Main:GetChildren()
    for i,v in pairs(Button) do
        if v['Name'] == 'Claim' then
            Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y-5)
            wait(.1)
            return
        end
    end
end

function Check_UI_Units()
    while task.wait() do
        local Check_UI_Units = game:GetService("Players").LocalPlayer.PlayerGui.HatchInfo.Enabled
        if Check_UI_Units then
            local Button = game:GetService("Players").LocalPlayer.PlayerGui.HatchInfo:GetChildren()
            for i,v in pairs(Button) do
                if v['Name'] == 'Shiny' then
                    Mouse_Click(v.AbsolutePosition.X, v.AbsolutePosition.Y-5)
                    wait(1)
                end
            end
        else
            return
        end
    end
end

if game.PlaceId == 8304191830 then
    repeat task.wait() until game.Workspace:FindFirstChild(game.Players.LocalPlayer.Name)
    repeat task.wait() until game.Players.LocalPlayer.PlayerGui:FindFirstChild("collection"):FindFirstChild("grid"):FindFirstChild("List"):FindFirstChild("Outer"):FindFirstChild("UnitFrames")
    repeat task.wait() until game.ReplicatedStorage.packages:FindFirstChild("assets")
    repeat task.wait() until game.ReplicatedStorage.packages:FindFirstChild("StarterGui")
end
if game.PlaceId == 8304191830 then
    wait(5)
    Reedemcode()
    SummonUnits()
    Check_UI_Units()
    Click_Claim()
    Open_Units()
    Click_Attack()
    UnEqupi_All()
    Check_Units()
    Equpi_Units_1()
    Equpi_Units_2()
    Equpi_Units_3()
    Equpi_Units_4()
end

-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
-- //  Chilling Game :D \\ --
