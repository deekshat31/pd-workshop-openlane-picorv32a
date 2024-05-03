# pd-workshop-openlane-picorv32a
# AIM - The main objective of the ASIC Design flow is to take the design from RTL to GDSII format
| Day 1 | Day 2 | Day 3 | Day 4 | Day 5 |

## Day 1
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














