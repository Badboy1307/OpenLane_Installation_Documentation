# OpenLane__Documentation



## Linux Commands for installing prerequisites of OPENLANE

For UBUNTU Systems >=22.04 LTS

    sudo apt install python3-pip -> this is for installing python3
    sudo apt-get install git -> installing git for github repository
    sudo apt-get install yosys -> installing RTL synthesis framework
    sudo apt-get install make ->installing make file
    sudo apt-get install klayout -> installing IC layout editor
    sudo snap install docker -> installing docker
    pip3 install fusesoc -> installing python library called FuseSoC
    sudo apt-get install verilator -> installing Verilator
    sudo apt-get install magic ->installing magic, a layout tool
    sudo apt-get install opensta ->installing opensta, a GUI based web server
    sudo apt-get install vim -> installing vim editor
    sudo apt-get install gtkwave -> to view simulation of verilog codes
    sudo apt install -y build-essential python3 python3-venv python3-pip


## Conda download is also mandatory otherwise your openlane won’t download any pdks

• For this go to your Mozilla browser in Ubuntu • Visit https://docs.conda.io/en/latest/miniconda.html
![image](https://user-images.githubusercontent.com/60011091/134452236-51003d18-28dc-4361-9c31-d7fa26bfead2.png)


• Choose python 3.8 under Linux installers
![image](https://user-images.githubusercontent.com/60011091/134452328-242181dd-ea29-4bbb-8f63-6df50c90e6d8.png)



• Your file gets downloaded in /Downloads in Ubuntu




• Now go to your terminal and type these commands

     cd ..
     cd Downloads 
     sh Miniconda3-latest-Linux-x86_64.sh  or bash Miniconda3-latest-Linux-x86_64.sh (Your miniconda file will have a similar name)
     
     
After running above command
Miniconda file opens in the terminal just keep pressing enter until you see do you agree to license terms
You say Y/Yes to continue

The download starts automatically and it will show in the end that download was successful

Restart the terminal to see the changes to terminal.

## Commands for removal of old UBUNTU docker (Only for versions < 22.04) 

Docker versions which supports UBUNTU versions <22.04 requires these commands to be run in their pc 

      sudo apt-get remove docker docker-engine docker.io containerd runc    --> Uninstall Old docker versions 
      sudo apt-get update         -->update apt package index
      sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release                  -->prerequisites for docker 

     sudo mkdir -p /etc/apt/keyrings  --> Add Docker's GPG Keys
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg  -->Add Docker's GPG Keys
     
     echo \                                
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null                                             -->Use these command to set up the repository

### Method 1  Installing Generally

     sudo apt-get update              -->update apt package index
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin         -->download latest docker
  
### Method 2  Installing Specific Version
       
     sudo apt-cache madison docker-ce  --> searching for latest versions available for your ubuntu version

   The below image lists the versions of docker available 
      
      ![image](https://user-images.githubusercontent.com/60011091/171088618-cddfe077-0bb0-47f8-944f-d88fb3c8ef90.png)

       sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin  
       
   If for example <VERSION_STRING>= 5:20.10.16~3-0~ubuntu-jammy then, 
       
       sudo apt-get install docker-ce=5:20.10.16~3-0~ubuntu-jammy docker-ce-cli=5:20.10.16~3-0~ubuntu-jammy containerd.io docker-compose-plugin
       
  To ensure docker runs, run the below command

       sudo docker run hello-world         
    

## Commands for downloading OPENLANE and PDKS of the latest stable version

This is just around 2.3 GB as you already have all the prerequisites.
We can install this version in home directory or in Desktop or any directory of your choice.
These below commands has been done in /Desktop directory

    git clone https://github.com/The-OpenROAD-Project/openlane.git
    cd OpenLane/
    cd docker
    sudo make                      ->  installs OpenLane  docker image 

If you get any fatal repository warning add these commands but your username could be different to the below commands


Note: badboy07 was the username used here so put your username instead.

    sudo git config --global --add safe.directory /home/**badboy07**/Desktop/OpenLane
    cd ..
    ls
    export PDK_ROOT=/home/badboy07/Desktop/OpenLane/pdks
    sudo make pdk   ->installs skywater_pdks & open_pdks
    
 ## For normal testing of spm design
    
    sudo make test  ->initial testing of test design ie spm
    
    
Note: The below screenshots may vary depending on OpenLane versions downloaded using above commands.


   ![image](https://user-images.githubusercontent.com/60011091/134452405-50fc8760-dfcf-4b12-8737-7ccaa8ae9d60.png)
   
   
   ![image](https://user-images.githubusercontent.com/60011091/134452420-5e328c67-d2f2-453c-a096-38943e39bed7.png)


   ![image](https://user-images.githubusercontent.com/60011091/134452435-61292411-b67c-41b3-b1e1-8ff12a66db6f.png)



  
    sudo make mount    ->if permission denied
    
   ![image](https://user-images.githubusercontent.com/60011091/134452503-e9564660-bc54-4976-aba7-a7ff90e97a2d.png)
 

If successful you’ll get something similar to the one shown in the above image


## Existing Design in Openlane Design Directory


PicoRV32 is the test design, which is a CPU core which implements the RISC-V RV32IMC Instruction Set. 1. Configuration files As seen in the above images we have used these configuration commands as given below.

     ./flow.tcl -design picorv32a   ->This command runs the entire OpenLane flow from preparation of design to generating GDSII)
     
Or we can follow the flow from OpenLane Interactive flow for Custom Design SERV

For details of SERV you can refer to the below links 

https://github.com/olofk/serv/tree/v1.0/rtl
https://serv.readthedocs.io/en/latest/

Note: instead of picorv32a you can use your design instead

## Logic Synthesis Tool Flow for Custom Design SERV

![image](https://user-images.githubusercontent.com/60011091/171186475-3ac7eec8-fc9f-4783-88cc-7f623d73c3a6.png)

 The above image shows the flow of Yosys. Yosys takes RTL code and .lib files as input and netlist as output. Netlist is the standard cell representation of the design.


Yosys flow

     
    read design
    read_verilog serv_rf_top.v
  
  elaborate design hierarchy 
  
    hierarchy -check -top serv_rf_top
  
 the high-level stuff proc;
  
    opt
    fsm; opt;
    memory; opt
     
 mapping to internal cell library
   
    techmap; op
     
 mapping flip-flops to mycells.lib
   
    dfflibmap -liberty home/badboy07/Desktop/vsdflow/work/tools/openlane_w orking_dir/pdks/open_pdks/sky130/sky130A/libs.ref/sky130_fd_sc_hs/lib/sky130_fd_sc_hs ff_100C_1v95.lib
     
   mapping logic to mycells.lib 
      
     abc -liberty home/badboy07/Desktop/vsdflow/work/tools/openlane_w orking_dir/pdks/open_pdks/sky130/sky130A/libs.ref/sky1 30_fd_sc_hs/lib/ sky130_fd_sc_hs ff_100C_1v95.lib
  
   cleanup 
   
     clean
       
# write synthesized design 

    write_verilog serv_rf_top.synthesis

OpenLane automated flow for Custom Design SERV


    ./flow.tcl -design serv_rf_top -init_design_config
    
The above command flow.tcl has commands for running OpenLane flow so here we run the configuration flow
Then we need to edit required variable in config.tcl

    ./flow.tcl -design serv_rf_top
    
This command runs the entire OpenLane flow from preparation of design to generating GDSII.


## OpenLane Interactive flow for Custom Design SERV

    ./flow.tcl -interactive
    
This command runs OpenLane in interactive flow

Running the commands in interactive flow

    package require openlane 0.9
     
This command checks the version of OpenLane required for the flow

    prep -design serv_rf_top -tag serv_interactive
     
Preparing a design called serv_rf_top with tag for naming the current run.

    run_synthesis
This command runs the synthesis right from Yosys till OpenSTA.

Conversion of RTL design to a circuit out of components from Standard cell library (SCL).

    run_floorplan
This command performs the Floor planning and Power Distribution network(PDN) of the design.

Chip Floorplanning - Patitioning the chip die between different system blocks and placing the I/O pads

![image](https://user-images.githubusercontent.com/60011091/134455353-0ff2b52e-9fe7-41dc-a672-844c2128edd1.png)


The above image describes the Chip Floorplanning


Macro Floorplanning - We define the macro dimensions, pin locations and row definitions.

![image](https://user-images.githubusercontent.com/60011091/134455391-31b0f369-1aeb-4974-9b73-2d02ef4e664f.png)


Power Planning - Power network is constructed typically by multiple VDD and GND pins. the Power pins are all connected through power rings, vertical and horizontal metal straps.
Power rings- Power ring is designed around the core. Power rings contains both VDD and VSS rings. After the ring is placed a power is designed such that power reaches all the cells easily.

![image](https://user-images.githubusercontent.com/60011091/134455442-756501cb-dfc7-44ba-85e7-5a1049c92e25.png)

This diagram shows the power planning components.



These parallel structures reduce resistance. To address electromagnetism the power distribution network uses upper metal layers as they are thicker than lower metal layers


    run_placement
     
This command perform global placement and detailed placement of cells of the design.

![image](https://user-images.githubusercontent.com/60011091/134455819-b0119a04-43d9-4f5e-96e8-07962d350d3e.png)


Placing the cell on the floorplan rows aligned with sites. Connected cells should be placed close to each other to reduce the interconnect delay. It is done in two steps ie Global and Detailed Placement. In global placement , the approximate locations for cells is decided by placing cells in global bins. In detailed placement, cells are Placed without over lapping.


    run_cts
     
Creating a clock distribution network :

 Delivering clock to all sequential elements.
• With Minimum skew
• In Good shape
• A tree (H,X etc)

    run_routing
     
Implements interconnects using available metal layers. Metal tracks form a routing grid. Routing uses Divide and Conquer method.
There are two types of Routing ie -Global routing: Generates routing guides -Detailed routing: Uses routing guides to implement actual wiring.


![image](https://user-images.githubusercontent.com/60011091/134455873-81fd49ea-35f1-457d-9c85-c471b1be56af.png)


    run_magic
This command generates Layout

    run_magic_drc
This command performs DRC check on the layout.


    run_klayout
This command creates backup layout

    run_klayout_drc
Running DRC on layout generated from Klayout

    run_klayout_gds_xor
This command is for generating GDS2 of our design with XOR design

    run_lvs
This command checks for layout vs schematic violations


Layout Versus Schematic (LVS) checking compares the extracted netlist from the layout to the original schematic netlist to determine if they match. The comparison check is considered clean if all the devices and nets of the schematic match the devices and the nets of the layout.


    run_lef_cvc
This command is used for Circuit Validity Checker on the output spice. Voltage aware ERC checker for CDL netlists.


    run_antenna_check
This command is used for checking Antenna violations. Long metal lines and Vias introduce antenna violations. The antenna rule specifies the maximum tolerance for the ratio of a metal line area to the area of connected gates. VLSI process starts from the substrate, device layer and then metal layers.


Layout Generation by Magic Tool for SERV
 
Post Floorplanning


The below command is for generating layout after Floorplanning.

   ![image](https://user-images.githubusercontent.com/60011091/134455938-8ce605c2-94bb-41c7-abec-8fc8a47991ad.png)
   
Post Placement
The below command is for viewing layout after placement. All the images after the below command are from what was generated during Placement.

![image](https://user-images.githubusercontent.com/60011091/134455953-f418adc3-09f2-487a-a7f2-089a5c6e6883.png)


Post CTS
The below command is for viewing layout after CTS. All the images after the below command are from what was generated during CTS.

![image](https://user-images.githubusercontent.com/60011091/134455973-3b4b0c91-2df8-426e-acae-cfbf6082583d.png)


Post Routing
The below command is for viewing layout after routing. All the images after the below command are from what was generated during Routing.

![image](https://user-images.githubusercontent.com/60011091/134456046-2a2278d1-644c-4344-959c-6108503108dd.png)



Final GDSII layout for SERV design
The below command is for opening the GDS file created.

  ![image](https://user-images.githubusercontent.com/60011091/134456188-d5490101-56b1-4578-b6aa-8cfe693a9a31.png)
  
  ![image](https://user-images.githubusercontent.com/60011091/134456196-d33a25d8-853f-487b-94ff-bd366105c91c.png) 


This is how a successful flow of OpenLane flow will look like.

For further detailed explanation you can feel free to check this pdf 

[Advance_VLSI_Lab_OpenLane_RTL_GDSII_flow_final.pdf](https://github.com/Badboy1307/OpenLane_Installation_Documentation/files/7215060/Advance_VLSI_Lab_OpenLane_RTL_GDSII_flow_final.pdf)

