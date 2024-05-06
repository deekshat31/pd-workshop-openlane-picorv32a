# pd-workshop-openlane-picorv32a
# AIM - The main objective of the ASIC Design flow is to take the design from RTL to GDSII format
| Day 1 | Day 2 | Day 3 | Day 4 | Day 5 |


Tools Used
Yosys - Synthesis of RTL Design
ABC - Mapping of Netlist
OpenSTA - Static Timing Analysis
OpenROAD - Floorplanning, Placement, CTS, Optimization, Routing
TritonRoute - Detailed Routing
Magic VLSI - Layout Tool
NGSPICE SPICE - Extraction and Simulation
SPEF_EXTRACTOR - Generation of SPEF file from DEF file

## Day 1
### Inception of open-source EDA, OpenLANE and Sky130 PDK
-----How to talk to computers----
-IC Terminologies Used :
1) Package - ICs are basically presents as packages. These packages are materials which contains the semiconductor device. An example of QFN-48 (Quad Falt No-Leads) with 48 pins is taken here.
2) Chip - It sits in the centre of the package.
3) Core - It is the place where all the logic units (gates, muxs, etc) are presnet inside the chip.
4) Pads - These are the itermediate structure through which the internal signals from the core of IC is connected to the external pins of the chip.
5) Die - It is the block which consists of semiconducting material and it can be used to build certain functional cuircuit which can be further sent for fabrication.
   ![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/8da29ef6-7f70-419d-b41c-d93178310894)
   
*Introduction to RISC-V*
RISC-V is an open instruction set architechture rooted on reduced instruction set computer principles. It is an open source ISA used for processor design.
Characterstics :
It uses one clock cycle per instruction.
It follows the th RISC Princples.
It has both 32-bit and 64-bit varients. It also support floating point instruction.
It avoids micro-architechture or technology dependent features.
It accelerates the time for design to reach the market as it uses open-source IP.

*Software to Hardware*
The flow shows how the high level language (at software end) gets converted to machine language (at hardware end) and then gets executed on the package.

What happens when we run a program?
Suppose a C program needs to run on a hardware. So we nned to pass this C program into the hardware. So, firstly it is compiled into assembly language (RISC-V assembly language program). Now this assmebly language is converted into the machine language program (basically 1's and 0's). Now this 1's and 0's are understanable by the hardware.

How does an application run on a computer?
The application software enters the system software (major component of it are OS, Compiler and Assembler).
The OS handles I/O operations, memories and many low level functions.
then the program passes to Compiler which changes the program to Assembly language (compiled into instructions depends upon the hardware).
Now the instruction set goes to Assembler. Assembler converts the instruction set to machine language (binary numbers).
The system software converts the apllication software into binary language.
Now these binary numbers enter our chip layout and according the function is performed.

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/1c03ea16-b159-406e-8ace-bb91f07b6017)

----SoC design and OpenLane----
Introduction to Digital design
For designing Digital ASIC ICs we require following components and some of it's opensource resources are also mentioned.

RTL models (old IP's) {github.com, librecores.org, etc}
EDA tool {OpenROAD, OpenLANE, etc}
PDK Data {SKYWater 130}
In the workshop every component is used from sources which are open soucre. The following image gives an idea about each component as an open source resource.

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/f583749c-9723-4ace-90b7-3940911779cb)


What is a PDK?
PDK stands for Process Design Kit, it is provided by foundaries and it consists of library or set of building blocks which are used to build ICs. Every component in the library is separate building block and are made of certain foundary rules.

Environment Setup
The OpenLANE flow requires various open source tools as well as their supporting tools to be installed for the complete Physical design flow. Installing this tools one by one is tedious as well as one can get lost in the steps. Installation can be done easily using some set of scripts present in following repositories VSDFlow (for installing Yosys, OpenSTA, Magic, OpenTimer, netgent, etc) and OpenLANE Build Scripts.

*Simplified RTL to GDSII Flow*
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/311e64d9-0e8c-4e02-8745-95d7e686a508)


The flow starts from the HDL code i.e. an RTL model and ends with GDSII file. The major implimenation steps are:

1) Synthesis - During synthesis the HDL design is translated into circuits, which are made up of components present in the standard cell library. The resultant circuit is described in HDL and its referred as gate level netlist which is functional equivalent of RTL code. 
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/1e44ae8b-f10f-4622-bde1-3062625471cc)

2) Floor Planning - In Floor planning the chip area is being planned which in turn creates a robust Power distribution to power the circuits. The die is partitioned into different building blocks or components, also the I?O pads are distributed. 
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/6cb36470-8caf-485c-b583-228b8175b18c)


3) Power Planning - The power network is constructed typically for a chip was it has to power multiple VDD and ground pins. The power pin are connected to all component through rings and multiple horizontal and vertical strips. Sach parallel structure is meant to reduce the resistance.
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/f5fe0d6e-0c04-46c5-979a-8597b18d058f)


4) Placement -To reduce the interconnect delay conical cells are placed very close to each other and this is also done to enable successful routing afterwards. Placement is done in two ways Global placement and detailed placement. Global placement provide optimal result and these may or may not be legal where as the detail placement is always legal.
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/639e570d-fbb9-4b11-ac09-d15b7d3e2c96)


5) Clock Tree Synthesis (CTS) - Before signal routing clock routing is done so that the clock distribution is done to every sequential block. Clock distribution network delivers the clock to each of the sequential block. It is done so that there is minimum skew and latency. It usually follows a shape i.e., H-tree, X-tree, etc.
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/7bbd3c20-192b-41ba-85da-2abebc81ebd9)


6) Routing - The signal routing is done using metal layers. It is essential to find valid pattern of horizontal and verticle wires to implement the nets that connects the cells together. Router uses the available metal layers as defined by the PDK.
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/b4f9bd78-6c51-46e9-8910-79726db4d88c)


7) Verification and Sign-offs - After PnR and CTS we perform verifications, to check whether our layout is valid or not. These verifications consists of Physical verification such as DRC and LVS. Design Rules Checking (DRC) ensures that the layout follows the design rules and Layout Vs Schematic ensures that the final layout is as per the synthesised gate level netlist or not. Finally Static Timing Analysis is done (STA) to make sure that all the timing constraints are met by the circuit.

*About OpenLANE*
OpenLANE is a flow which uses various open source tools for the RTL to GDSII flow. It has the striVe family of open everything SoCs (Open PDK, Open EDA, Open RTL). The various tools it uses are Yosys, OpenROAD, Magic, Netgen, SPEF_Extraction, etc.
It is tuned for SKYWater 130nm open PDK.
OpenLANE ASIC flow is shown below. 
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/8dccf2e6-be9a-41d0-b29c-5612a7953e95)

The flow starts with RTL Synthesis. RTL is fed to Yosys with the design constraints. Yosys translates the RTL into a logic circuit using generic components.

the circuit can be optimized and then mapped with standard cell library usin the tool abc. There are abc scrript to guide the optimization. OpenLANE has several abc scripts which has different synthesis statergies (least area, least power consumption, etc). The synthesis exploration utility is for statergy exploration and report generation.

OpenLANE has design exploration utility which can be used to sweep the design configurations (16 in total) and it genrates reports which has different design matrix and also shows the number of violations in layout. It is used for regression testing and to find the best configuration of our design.

OpenSTA performs the Static timing analysis on the netlist which is generated during synthesis.

Now after synthesis, the testing part starts (DFT) i.e., scan insertion, Automatic Test Pattern Generation (ATPG), Test Pattern Compaction, Fault Coverage and Fault Simulation. This step is optional.

Nest step is Physical implementation. This part is done with the help of OpenROAD application. It performs PnR which consists of FP+PP, Placement (Global and Detailed), Optimization, CTS and routing (Global and Detailed). TritonRoute is used for detailed routing.

Logic equivalence checking (LEC) is performed as the circuit changes due to optimization process as compare to the one generated during synthesis. This is done using Yosys tool to make sure the functionality is equivalent.

During insertion there is a special step that is fake antenna insertion. It is required to address the antenna rule violations. The concept of fake antenna is something like we have already considered the antenna so that on later stage we do not have any antenna violations. Hence we add fake antenna diode next to every cell input after placement. Then antenna checker is run from the Magic tool against the layout.

Fake antenna diode cell is created and added to standard cell library.

The sign off include STA, DRC and LVS. It also involves interconnect RC extraxtion from the routed layout followed by STA using OpenSTA.
Physical signoffs include DRC and LVS. DRC and LVS is performed using Magic tool. Circuit extraction is done NetGen.

----Getting familier to open-source EDA tools----

Contents of the OpenLANE Directory
The following content is specific to the workshop. There are lot of other files present in the directory too.

OpenLane folder - It contains all the tools and the file that need to be invoked during the flow.
Designs - This folder consists of all the designs requried during the flow (picorv32a is the design used in this workshop)
PDKs - This folder contains all the pdk related files as well as information. (open pdk, Sky130, Skywater pdk).
open pdk consists of the scripts.
sky130A pdk consists of the libs.ref (has files specific to process such as timing, lef-both tech and cell) and libs.tech (has all the files specific to the tool) files.
skywater pdk consists of skywater 130 nm pdks.
NOTE: - Here sky130_fd_sc_hd libs.tech is being used. 4. config files - It bypasses any configuration that has already been done i.e., many of the switches use default value that is already present in the OpenLane flow. The precedence order of Openlane settings are:

sky130_xyz_config.tcl
config.tcl
Default value (already set in OpenLane)

## LAB Day 1

Step 1: Starting OpenLane

Go to openlane folder.
cd work/tools/openlane_working_dir/openlane
Then run the docker command.
docker
Now run the flow.tcl file with interactive mode.
./flow.tcl -interactive
Now import packages
package require openlane 0.9
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/4d896ff5-ee81-43a9-aa58-6f7eb806beb1)


Now we are good to go to execute our commands.
NOTE - The above commands are to be run everytime we use OpenLANE for RTL2GDSII flow.

Step 2: Design Preparation
Before design prep we wont have runs folder inside : ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a
Now , Creating file for our design i.e., setting up the design. It merges the cell LEF files and the technology LEF files generating merged.lef which is present in the temp folder.
prep -design picorv32a
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/2e43163c-9444-4c18-a911-324c3c43a3ab)

This marks the creation of new folder inside picorv32a named as runs folder which consists of new folder whose name is the date on which the command is run. The following folder has results, reports, command logs, PDK Sources, etc files.
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/fa7dc897-dded-4ead-8cee-c4baade07932)

Step 3: Running Synthesis

Yosys synthesis is run when the command for synthesis is entered. Along with it abc scripts are also run and OpenSTA is also run.

run_synthesis
After running synthesis; logs, reports and results are created.

The report folder have the following files:

1-yosys_4.chk.rpt
1-yosys_4.stat.rpt
1-yosys_dff.stat
1-yosys_pre.stat
2-opensta.min_max.rpt
2-opensta.rpt
2-opensta.slew.rpt
2-opensta.timing.rpt
2-opensta_tns.rpt
2-opensta_wns.rpt
Also a netlis file is created in the results --> symthesis folder named picorv32a.synthesis.v

### TASK 1: Finding the d flip flop ratio
Count of d flip flop (sky130_fd_sc_hd_dfxtp_2) = 1613
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/651afc09-a724-4c7b-bfec-51ed03107721)

Number of cells = 14876
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/80a38984-c8a4-4050-9373-25d6e0210209)

flop ratio = (count of d flip flops / number of cells) = 1613/14876 = 0.108429 (10.8429 %)

The synthesis statistics report :
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/0c902e1d-1c30-4370-85ca-a89074bfeea8)



## Day 2
### Good Floorplan Vs Bad Floorplan and introduction to library cells 
----Chip Floor Planning Consideration----

Step 1) Define width and height of the core and die :
 Lets say we have a netlist (connectivity between electronic device)

 ![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/ebf00b06-6571-4d4c-8506-264d326eb2c5)
 Lets convert it into physical dimension ,

 ![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/3111d40e-1a23-4ce1-a022-8c634be4e12c)
Now lets try to remove these wires and club these block together to get an area , lets assuming W=1 and H=1 of both standard cells and flip flop;

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/2431ce5b-72f6-4359-aa32-d0743dbb60fc)
Total Area = 2*2 = 4 sq units
Now there are primarily 2 important terms : Core and Die 
Core - Section of chip where fundamental logic of design is placed 
Die -We wont exceed this area
If netlist completely occupies the core region then its said to be 100 % Utilised . 
#### Utilisation Factor =( Area Ocuupied by Netlist )/(Total Area of Core)
So , here Utilization Factor =1
#### Aspect Ratio = (Height / Width)
When Aspect Ratio =1 , means its a square shaped chip and if other than 1 means its a rectangle shaped chip . 

Ex: ![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/2058a5e4-3442-48f9-92a0-ef4d93464266)
Here , Utilization Factor = 0.25 and Aspect Ratio = 1

Step 2 ) Define the location of Preplaced cell :
We have a netlist and lets cut it into 2;
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/8b928864-2294-48c5-a06b-1f3b8265f844)

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/4b39738d-380e-431b-90f1-1da0a36c3305)
Both of these blocks will be implemented separately ;
![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/9f1b5a15-3017-4d79-8820-9b90f583d469)
Now will black box them and detach them  so that they now become invisible from the top . Now lets separate them as 2 different IPs/modules .

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/89825352-176e-4ba9-acfb-9b307c969d0b)

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/e87ed8b5-8b9f-4a02-ab00-ee3a02bf2f3e)
These modules can be multiple times replicated .
### Floorplanning - The arrangement of these IPs in a chip 
These IPs blocks have user defined location and hence are placed in the chip before automated placement and routing and known as preplaced cells .

Step 3 ) Decoupling capacitors :
Say we have some complex circuit , now assume the amount of switching current it needs ; during switching it needs a huge amount of Current , at that time power supply should be able 
to cater to power needs of all blocks .

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/e8bd2af9-de0d-4ca1-8893-59941b107cb6)
### Decoupling Capacitor(DC) acts as a coupling element between macros and main power supply .
We keep these DCs already charged so whenever there is need they will feed the blocks.

Step 4 ) Power Planning :
Here we create a mesh kind of  structure so instead of having one single power supply we will have multiple sources of power supply .

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/a7cd401d-db56-4428-88d2-2bd2df9a02b0)

Step 5 ) Pin Placement and logical cell placement blockage :
Depending on where we are tryong to place the cells , the input and output pins are placed randomly ;
### Pins are placed in the region between the die and the core .

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/86146391-1f89-4f33-aaa5-54b3c44d9d58)
Logical Cell Placement Blockage - It places blockage between the die and the core so that no logical cells are accidently placed here as only pin placement is alowed here.

![image](https://github.com/deekshat31/pd-workshop-openlane-picorv32a/assets/168093649/51eca152-9e59-4f76-afaa-c10e3c098d04)

*Lab Work*
STEPS TO RUN FLOORPLAN USING OPENLANE :
So till now synthesis was done successfully .















