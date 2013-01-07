digging=true

-- This function toggles the vertical direction of movement for the Turtle
function changedir()
        if direction==1 then
                direction=0
        else
                direction=1
        end
end

function emptycargo()
        for z=1, 9 do
                turtle.select(z)
                turtle.drop()
        end
end

-- Main Digging Script
while digging==true do
        direction=1
        height=39
        length=10
        curheight=0
        x=0
        w=0
        ret=false

        -- Digging Script
        -- Horizontal Direction
        while w<length do
                -- Vertical Direction
                while x<height do
                        if direction==1 then
                                turtle.digUp()
                                ret=turtle.up()
                                if ret==true then
                                        x=x+1
                                        curheight=curheight+1
                                        ret=false
                                end
                        else
                                turtle.dig()
                                turtle.digDown()
                                ret=turtle.down()
                                if ret==true then
                                        x=x+1
                                        curheight=curheight-1
                                        ret=false
                                end
                        end
                end
                x=0

                -- If it has finished going down, go an extra space forward (Since it digs forward on the way down to negate falling blocks)
                if direction==0 then
                        while x<2 do
                                turtle.dig()
                                ret=turtle.forward()
                                if ret==true then
                                        x=x+1
                                        ret=false
                                end
                        end
                        emptycargo()
                else
                        while x<1 do
                                turtle.dig()
                                ret=turtle.forward()
                                if ret==true then
                                        x=x+1
                                        ret=false
                                end
                        end
                end
                changedir()
                w=w+1
                x=0
        end

        -- Once the main digging has been completed, this section returns the Turtle to the ground
        if direction==0 then
                while curheight>0 do
                        ret=turtle.down()
                        if ret==true then
                                curheight=curheight-1
                                ret=false
                        else
                                -- Turtle has stuff beneath it, remove it
                                turtle.digDown()
                        end
                end
        end

        -- Continue Digging Input
        ret=print("Continue Digging? Y\/N")
        continue=io.read()
        if continue=="n" or continue=="N" then
                digging=false
                ret=print("Digging stopped, dumping cargo.")
                emptycargo()
        elseif continue=="y" or continue=="Y" then
                ret=print("Continuing digging, dumping cargo.")
                emptycargo()
        end
end