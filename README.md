# Opensource-EDA----openlane-workshop
RTL to GDSII  ASIC design flow with open source EDA tools 
 - Synthesis (yosys for cell mapping and ABC for synthesis, early STA analysis)
 - Floorplan
 - Placement of cells

![openlane flow 1](https://user-images.githubusercontent.com/8078903/173179563-d52a5399-5c23-4569-bffb-f6b65ce23bfa.png)

viewing def files using magic
 - shortcuts (
    - press left click and then right click to draw a rectangle. z for zoom in this region. Z for zoom out
    - s for selecting an object in layout window and "what" command in tkcon window to get info about selected object
    - s and v for completely fit design to centre

Floorplan Def
![image](https://user-images.githubusercontent.com/8078903/173085908-7bc68a76-9185-45ba-8381-321c287ab9cc.png)

![image](https://user-images.githubusercontent.com/8078903/173085739-30ee4256-a7c4-4667-820d-5a15be738d8f.png)


Placement Def

![image](https://user-images.githubusercontent.com/8078903/173083430-996c40ac-0691-4c91-a84b-bec3d7adaf23.png)

![image](https://user-images.githubusercontent.com/8078903/173081727-087d65b7-2e16-4fca-830f-57334030112f.png)


STD CELL Characterisation:
![image](https://user-images.githubusercontent.com/8078903/173179902-c9b8d5ae-c51c-4066-96f0-0627c50ef63b.png)
viewing mag file using magic
![image](https://user-images.githubusercontent.com/8078903/173180008-865541e1-855a-4ef6-917b-48accf679f94.png)
![image](https://user-images.githubusercontent.com/8078903/173180243-5c923733-4914-4689-82d5-9944bfdc2a35.png)


LEF generation from Magic
![image](https://user-images.githubusercontent.com/8078903/173184368-4e79a98a-c60c-4ea3-868f-8a366d5a21c3.png)

Parasitic extraction
spice file generation
![image](https://user-images.githubusercontent.com/8078903/173187732-fccb0b3d-11d3-4f55-8347-3e574586102c.png)
![image](https://user-images.githubusercontent.com/8078903/173187764-653b5b3d-af4b-445b-b434-cd9a1cf99b90.png)
![image](https://user-images.githubusercontent.com/8078903/173187743-fbd05112-c1e5-4e3b-bf50-f18fd2ca6e75.png)

Inverter characterisation:
ngspice input file for transient analysis as input of inverter is swept
![image](https://user-images.githubusercontent.com/8078903/175472804-7d96e61a-3b16-4bba-a042-ed3731a40c42.png)

For simulation, ngspice is invoked in ther terminal:
ngspice sky130_inv.spice

The output "y" is to be plotted with "time" and swept over the input "a":
plot y vs time a
![image](https://user-images.githubusercontent.com/8078903/175363982-79e8d810-845f-4231-b371-fc32739f08fa.png)
![image](https://user-images.githubusercontent.com/8078903/175364081-0fd0cd1d-66af-42db-92b4-683c0be1c1fa.png)
![image](https://user-images.githubusercontent.com/8078903/175472931-ca6cd0af-3a01-4dfa-8f01-ecd479632610.png)


Magic Features & DRC rules
![image](https://user-images.githubusercontent.com/8078903/175479542-a35b1f3a-295e-44a4-9780-37f1dfb773ad.png)
tar xfz drc_test.tgz

The .magicrc loads the tech file required by the user. Since this file sets up the tech file, sky130.tech need not be mentioned in the command used to invoke Magic. Hecen Magic can be invoked more conveniently now:
magic -d XR

updating sky130A.tech file

![image](https://user-images.githubusercontent.com/8078903/175505493-5735f12b-fa2a-43df-a730-b0d781a5be03.png)

![image](https://user-images.githubusercontent.com/8078903/175505229-a5eeb35d-663e-4b17-91dc-7e06e9b532a6.png)

After updating techfile new src violations highlighted. Earlier it was 10 now 35 DRC errors

![image](https://user-images.githubusercontent.com/8078903/175505888-d36023d7-8be4-4f1c-a051-e7d02342079a.png)
![image](https://user-images.githubusercontent.com/8078903/175506391-5ccecec2-fb7f-4237-b854-072c97338095.png)

*nsd in sky130A.tech modified to alldiff

![image](https://user-images.githubusercontent.com/8078903/175510835-c3f01bb3-54fa-45da-b2a3-a54382485647.png)
Before above change
![image](https://user-images.githubusercontent.com/8078903/175511001-72deb262-583b-4a5f-a347-da352a36f48c.png)
After above change
![image](https://user-images.githubusercontent.com/8078903/175511708-1af952fc-d7df-4d9d-8f7f-9bd7ceabb985.png)
![image](https://user-images.githubusercontent.com/8078903/175511902-4f5d33ad-069d-4cae-afb0-d7b4cd96b8ed.png)






