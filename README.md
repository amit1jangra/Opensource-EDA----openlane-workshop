# Opensource-EDA----openlane-workshop

Stages of Openlane Flow:

RTL to GDSII  ASIC design flow with open source EDA tools 

Synthesis
  -yosys-Performs RTL synthesis using GTECH mapping
  -abc-maps the cells to standard cells described in PDK(Skywater130) and produce netlsit
  -OpenSTA-performs timing analysis on the resulting netlist
  -Fault-Design for test

Floorplan
  -init_fp-Defines the core area for macros and rows for placement and tracks for routing
  -IOplacer-places the macro and output ports
  -PDN-Power Distribution network
  -tapcells-insert well taps and decap cells

Placement(Global and Detailed)
  -RePlace-performs global placement
  -Resizer-performs optimizations on the design
  -perform timing optimization
  -perform detailed placement

Clock tree synthesis
  -TritonCTS-synthesizes clock distribution network

Routing
 - FastRoute-performs global routing
 - TritonRoute-perfoms detailed routing

SPEF Extractor-perfoms SPEF extraction that include parasitics extraction

GDSII

Magic-streams out the final GDSII layout
Checks

Magic-performs DRC and antenna check
Netgen-performs LVS check(Layout versus synthesis)

Day-1 [Inception of open-source EDA, OpenLANE and Sky130 PDK]

To start the flow we use the command

./flow.tcl -interactive (for interactive flow)

To include the packages of openlane

package require openlane 0.9

To include the design

prep -design picorv32a

![openlane flow 1](https://user-images.githubusercontent.com/8078903/173179563-d52a5399-5c23-4569-bffb-f6b65ce23bfa.png)

![open_lane_flow](https://user-images.githubusercontent.com/8078903/175562724-857219fd-f1eb-454a-ab4f-ebac744a38b5.PNG)

The inputs to the design flow are
  -Design files
  -PDK files
Design rule check(DRC)
Layout versus synthesis check (LVS)
PDK files
  -.lef files
  -.tlef files
  -Libraries of standard cells
Output of the flow is GDSII file (.gds)

.gds file is given to foundry for manufacturing the chip

Day-2 [Floorplanning and Placement]
Floorplan stage performs the following process

Die area
Core utilization
aspect ratio
Input and Output cell
power distribution network
Macroplacememt
Floorplan is performed using the command

run_floorplan

OpenLANE_flow directory contains "configuration" which contains variables required for each stage

Examples: * FP_CORE_UTIL gives the ratio of area occupied by netlist to the total are of core * FP_ASPECT_RATIO gives the aspect ratio (Height/Width)

viewing def files using magic
 - shortcuts (
    - press left click and then right click to draw a rectangle. z for zoom in this region. Z for zoom out
    - s for selecting an object in layout window and "what" command in tkcon window to get info about selected object
    - s and v for completely fit design to centre

Floorplan Def
using magic to view floorplan def
![image](https://user-images.githubusercontent.com/8078903/173085908-7bc68a76-9185-45ba-8381-321c287ab9cc.png)

![image](https://user-images.githubusercontent.com/8078903/173085739-30ee4256-a7c4-4667-820d-5a15be738d8f.png)


Placement Def
Viewing placement def in Magic
![image](https://user-images.githubusercontent.com/8078903/173083430-996c40ac-0691-4c91-a84b-bec3d7adaf23.png)

![image](https://user-images.githubusercontent.com/8078903/173081727-087d65b7-2e16-4fca-830f-57334030112f.png)


STD CELL Characterisation:
![image](https://user-images.githubusercontent.com/8078903/173179902-c9b8d5ae-c51c-4066-96f0-0627c50ef63b.png)

viewing mag file using magic
![image](https://user-images.githubusercontent.com/8078903/173180008-865541e1-855a-4ef6-917b-48accf679f94.png)
![image](https://user-images.githubusercontent.com/8078903/173180243-5c923733-4914-4689-82d5-9944bfdc2a35.png)


LEF generation from Magic
![image](https://user-images.githubusercontent.com/8078903/173184368-4e79a98a-c60c-4ea3-868f-8a366d5a21c3.png)

Day-3 [Design library cell using Magic Layout and ngspice characterization]:

Parasitic extraction

spice file generation
![image](https://user-images.githubusercontent.com/8078903/173187732-fccb0b3d-11d3-4f55-8347-3e574586102c.png)
![image](https://user-images.githubusercontent.com/8078903/173187764-653b5b3d-af4b-445b-b434-cd9a1cf99b90.png)
![image](https://user-images.githubusercontent.com/8078903/173187743-fbd05112-c1e5-4e3b-bf50-f18fd2ca6e75.png)

Std Cell (Inverter) characterisation:
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

default cif ostyle is gdsii
cif see to check drc and look at geometric grid, set cif ostyle as per requirement only one style can be active at a time
try to put cif output rules in separate style variant which can be run on demand and so that it does not slow down magic. prevent using them while using interactive layout

drc rules of two types fast and full
drc fast - while with normal back end layers check
drc full - will check everything - compute intensive, slows down magic

![image](https://user-images.githubusercontent.com/8078903/175518266-6acdcf21-1ddc-4645-a297-951d673ce913.png)
![image](https://user-images.githubusercontent.com/8078903/175518605-2fc1ae24-bd0e-4632-9cb2-c0e77aee63e0.png)

![image](https://user-images.githubusercontent.com/8078903/175520398-e3b2afcc-ce27-4da8-a4e9-4896ff05e194.png)
![image](https://user-images.githubusercontent.com/8078903/175521752-eaa6a222-5b6e-4078-ac01-5c9f8837dd61.png)

DRC error increased from 3 to 6 with drc style drc(full)
![image](https://user-images.githubusercontent.com/8078903/175521685-304eaf60-f866-45fb-a718-c24d7ea71276.png)
![image](https://user-images.githubusercontent.com/8078903/175522225-e7135fa8-08f2-4372-9cd7-2ebefd8a7b58.png)

![image](https://user-images.githubusercontent.com/8078903/175523832-d30264e0-45aa-4bdf-be62-27873fb811fc.png)
![image](https://user-images.githubusercontent.com/8078903/175523904-1a764ebb-52f9-46c8-90e7-68eef92679cd.png)
![image](https://user-images.githubusercontent.com/8078903/175526155-e89717ca-8a73-4fd2-8312-c6e737b5d67a.png)

Cell height should be odd mutiple of track y pitch and cell width should be odd multiple of x pitch.
Having a signal port at intersection of horizontal and vertical track ensures that route can reach the port from x as well as y directon.

Track info

![image](https://user-images.githubusercontent.com/8078903/175538857-8d8c2731-5488-40d4-8812-a3878da4d350.png)
![image](https://user-images.githubusercontent.com/8078903/175539085-e400667a-7093-4aa7-976b-d00214916bf4.png)

write lef

![image](https://user-images.githubusercontent.com/8078903/175543155-734c0cfa-c63c-4750-bf5a-bdc7570a3771.png)

![image](https://user-images.githubusercontent.com/8078903/175544565-802596f3-e914-42c5-b0d6-992ba291a30f.png)

Plugging custom LEF to openlane flow
If a new custom cell needs to be plugged into openlane flow, include the lefs (the one extracted earlier) as below:

In the design's config.tcl file add the below line to point to the lef location which is required during spice extraction.

    set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
Include the below command to include the additional lef into the flow:

    set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  
    add_lefs -src $lefs

config.tcl 

![image](https://user-images.githubusercontent.com/8078903/175560715-4d7ad675-6fec-481d-94dd-7e90560d7431.png)

![image](https://user-images.githubusercontent.com/8078903/175560897-61b6516d-c950-4258-8a6e-47bce74f2bdb.png)


![image](https://user-images.githubusercontent.com/8078903/175564408-6984d480-b667-48ca-bf37-de916bd451c9.png)
![image](https://user-images.githubusercontent.com/8078903/175564538-3a089c8d-2035-4b5b-8934-4610899b9fc0.png)
![image](https://user-images.githubusercontent.com/8078903/175564644-433cbbcc-6ae1-4d08-abc5-828dba9bd88c.png)
![image](https://user-images.githubusercontent.com/8078903/175564756-445db02e-ebfc-492c-af10-2803649654a1.png)
![image](https://user-images.githubusercontent.com/8078903/175564878-9ba51d98-7e35-4d0e-a2e1-4eacc8cd76bf.png)
![image](https://user-images.githubusercontent.com/8078903/175564993-49896e2f-075c-4887-828f-0d46e06ed68b.png)
![image](https://user-images.githubusercontent.com/8078903/175565121-ea788407-b91b-4c41-a482-ea0e9725e296.png)
![image](https://user-images.githubusercontent.com/8078903/175565254-e0e0ab98-5d0c-46d7-8f07-66d9ecbc1f40.png)
![image](https://user-images.githubusercontent.com/8078903/175565441-41a267c7-ec4a-4d7b-ace3-907c2399e227.png)
![image](https://user-images.githubusercontent.com/8078903/175565508-582fa30c-7ce0-4349-a6d2-d671cd500377.png)

Total negative slack after change in synthesis settings is 0

![image](https://user-images.githubusercontent.com/8078903/175570385-0d4d5ec1-03a9-4a9e-aec1-995f1c5d2acc.png)

Day-4:  timing analysis and importance of good clock tree

run_floorplan

run_placemnent
![image](https://user-images.githubusercontent.com/8078903/175578969-9918068b-798c-43f4-8051-8275dbb4de82.png)

![image](https://user-images.githubusercontent.com/8078903/175580791-6a202a0d-1921-470e-ad3d-1191c86ddf4e.png)
![image](https://user-images.githubusercontent.com/8078903/175583730-fb166609-e78f-4238-a2c5-eb6edeaad82d.png)

![image](https://user-images.githubusercontent.com/8078903/175607397-f22333ec-1032-4180-9346-516177859a32.png)


my_base.sdc

![image](https://user-images.githubusercontent.com/8078903/175613785-0d8441de-c5ff-4adf-84cf-353277c14ab1.png)

pre_sta.conf
![image](https://user-images.githubusercontent.com/8078903/175614739-a97c3e3d-287a-4e2f-8865-b4e5dd6b7ef9.png)


![image](https://user-images.githubusercontent.com/8078903/175614452-8787a2e4-2a12-4c6d-8054-c1d08a9898b1.png)
![image](https://user-images.githubusercontent.com/8078903/175614525-8fc1d117-8ff2-4ab4-bf26-481266eb88b9.png)
![image](https://user-images.githubusercontent.com/8078903/175614595-487c7f93-9c9a-4d42-8b79-366f2d038baa.png)

with picorv32a.synthesis_optimized.v

![image](https://user-images.githubusercontent.com/8078903/175615314-04d05521-4251-499a-8e8b-1f2555a9e546.png)
![image](https://user-images.githubusercontent.com/8078903/175615401-c202ca00-5298-4db7-96b1-370cec16fb27.png)

![image](https://user-images.githubusercontent.com/8078903/175615208-b509eccc-6fc7-47b9-bb08-2907141bbac2.png)

fanout very high for intial flop

![image](https://user-images.githubusercontent.com/8078903/175618886-ce0fdbdb-5c11-40a3-bb71-7764792681d6.png)

report_net -connections _11344_
help replace_cell

![image](https://user-images.githubusercontent.com/8078903/175627471-cd274433-346e-47d3-8de1-521adead118a.png)


![image](https://user-images.githubusercontent.com/8078903/177982380-caa2cd4f-1b19-4dde-9062-9cb433cbd134.png)
![image](https://user-images.githubusercontent.com/8078903/177982484-914bbda3-9db2-4aab-a332-edc73e922db0.png)
![image](https://user-images.githubusercontent.com/8078903/177982684-fbc4425e-89ad-4fc2-84f5-33dede88ab26.png)
![image](https://user-images.githubusercontent.com/8078903/177982846-f015f77f-6ea9-409e-8290-caa4d3c1140c.png)

init_floorplan

place_io

global_pacemnet_or

tap_decap_or

![image](https://user-images.githubusercontent.com/8078903/177984838-27f97cb9-9ffd-47d8-84ed-a5f1fb83bb3d.png)
![image](https://user-images.githubusercontent.com/8078903/177984928-b681402a-b82f-428e-bb9d-4a4b8f80bdad.png)

run_placement
![image](https://user-images.githubusercontent.com/8078903/177987113-7007e142-e49c-4f0f-ab1a-b0ec4cf0fba4.png)


run_cts
![image](https://user-images.githubusercontent.com/8078903/177987500-f66ff7ce-6632-49dc-b94e-90c0b89d9f10.png)

invoking open sta from openlane so that environment variables can be used defined in openlane flow

![image](https://user-images.githubusercontent.com/8078903/178019177-dea37f5e-ae9e-42a0-b296-008168bdbfc6.png)

when we use clock period as 24.73 ns setup slack is positive (10.189 ns) and when we use it as 12.0 ns it is negative  (-2.54 ns)
hold slack remains same -1.9767 ns

![image](https://user-images.githubusercontent.com/8078903/178019481-067eb334-cb1b-4e53-bc35-27eb4bd177c7.png)
![image](https://user-images.githubusercontent.com/8078903/178019368-30e38447-c13d-4a8c-aa1f-fc1afd4b9448.png)

After cts in detailed routing , actual metal traces are being laid capacitances and resistances of these come into picture. so this should increase data arrival time, which wil help in improving hold violation

We have used typical library for synthesis but for analysis we are using min, max library so analysis is not corret . so we exit from openroad but not from openlane

![image](https://user-images.githubusercontent.com/8078903/178026140-2c9b125e-030e-4764-a875-b741d5d76463.png)

![image](https://user-images.githubusercontent.com/8078903/178026501-980b06a4-e526-4f6d-96ff-1357fc7e529b.png)
![image](https://user-images.githubusercontent.com/8078903/178026632-48c86fdf-01cb-424b-b3b2-aa53467be761.png)

![image](https://user-images.githubusercontent.com/8078903/178026732-d2f46261-98ac-4cb1-8c75-cd71e61856de.png)
![image](https://user-images.githubusercontent.com/8078903/178026966-960e067f-4947-4284-97db-3790cb9c27a1.png)

triton cts does not support multicorner optimization currently

![image](https://user-images.githubusercontent.com/8078903/178030349-4c6ad5be-e790-48d0-a12d-ea25202b9d74.png)

replacing sky130_fd_sc_hd_clkbuf_1  to sky130_fd_sc_hd_clkbuf_2 using below steps have improved hold slack from  -0.0679 ns to  0.1784 ns
and setup slack changed  from 5.7807 ns to 5.7045 ns 
![image](https://user-images.githubusercontent.com/8078903/178031137-7ba8efc8-ca4e-4df2-a45f-a0cc2aca1ac0.png)


Clock skew:

![image](https://user-images.githubusercontent.com/8078903/178032186-a7e90990-5828-4753-ad32-32d3e0ffaf55.png)

insert clk_buf to clk buffer list

![image](https://user-images.githubusercontent.com/8078903/178032631-dee549df-5ae8-4bb9-8c90-964ad90d0867.png)


DAY 5 : Routing

run_cts is completed

see the (CURRENT_DEF) and check whether it is picorv32a_cts.def

Now do the command "gen_pdn" to make power distribution

The standard must be present between rails (vdd and gnd) and hence standard cells must be multiples of the gap

During pdn vdd and gnd are provided to stdcells by using pads,rings,straps,rails

Now do the command "run_routing"

Use "ROUTING_STRATEGY" to do routing

Routing is divided into two stages

Global-(FAST_ROUTE)
Detailed-Tritonroute
TritonRoute use MILP algorithm to panel routing with inter_layer and intra_layer roting

The output of fastroute is route guides

Access points and access point clusters are defined

After routing is finished,remove the vilation manually

Post route STA analysis is done

extract parasitics using Python API

so .spef is created

read_spef to know the parasitics of wires

PG routing

gen_pdn:

![image](https://user-images.githubusercontent.com/8078903/178045576-fab0b3ac-3c5a-4270-9f08-1ab2d2aee84e.png)
![image](https://user-images.githubusercontent.com/8078903/178045806-b8f40737-f834-44c4-a13e-24045219e294.png)


signal routing

run_routing:
   - Fast route (engine used for global route done fastroute, it will create routing guides for each of the nets)
   - Detail Route (detailed routing is done by tritonroute, it should ensure that it honors preprocessed routing guides from fastroute)


![image](https://user-images.githubusercontent.com/8078903/178051054-9dc6d286-1053-4d18-9b86-2de40065277a.png)
![image](https://user-images.githubusercontent.com/8078903/178051108-b4fa2f26-ef72-4f17-87cb-f695296e737e.png)
![image](https://user-images.githubusercontent.com/8078903/178051185-5d9ef7f1-88d7-47fd-9fa2-048980a71685.png)
![image](https://user-images.githubusercontent.com/8078903/178051256-67357118-059c-4a37-98a2-92cb7093e673.png)
![image](https://user-images.githubusercontent.com/8078903/178051333-d7b26cf2-a573-4148-a04f-b4fc70f915f9.png)
![image](https://user-images.githubusercontent.com/8078903/178051439-1cd3a3ea-0f1e-41de-92c1-15bcbb7196ad.png)
![image](https://user-images.githubusercontent.com/8078903/178051470-5a3661f3-b34a-43c8-9fa8-b0a82127712c.png)

![image](https://user-images.githubusercontent.com/8078903/178051941-49fc39e1-979c-432d-9827-8742ade4d1d4.png)


standalone SPEF extractor
![image](https://user-images.githubusercontent.com/8078903/178052278-0dc3c3e1-3be0-4fcd-b752-48147c917643.png)

auto generated spef in detailed routing step:
![image](https://user-images.githubusercontent.com/8078903/178052442-803d855f-5706-4288-bc0c-01a12712be81.png)

After spef generation sta.conf:

![image](https://user-images.githubusercontent.com/8078903/178053397-b2e28073-76a9-48c5-8fe7-fd83c4e206f9.png)
![image](https://user-images.githubusercontent.com/8078903/178053582-99a8d31a-0bc4-4393-933e-227e3ae1d633.png)
![image](https://user-images.githubusercontent.com/8078903/178053641-638ee0f7-06f0-4ea0-ba9c-e65ebfd8f13c.png)
![image](https://user-images.githubusercontent.com/8078903/178053668-bceed876-746c-4a43-a406-812205a6a408.png)


