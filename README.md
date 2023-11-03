# pes_cps
This Project is done as a part of course project in subject known as "Physical Design of ASIC".In this project we will complete RTL design to GDS with the help of OpenLane using SKYwater PDKs

## Introduction
At this moment due to increases in vehicle’s traffic,
parking is one of the biggest issue people are facing 
especially in metro city across the world. In this project I
have created a car parking system using Verilog HDL.

## State Diagram
In this system we are using 5 states which are as follows
1) IDLE: This is initial state of the system.
2) WAIT_PASSWORD: This state will reach 
when vehicle is at the gate and password request 
has generated.
3) RIGHT PASSWORD: This state is for if
password given is right.
4) WRONG PASSWORD: Thisstate is for if given 
is wrong and it will request password another 
time.
5) STOP: If there is another car inside then is state 
will generate.

![Car Parking System](https://user-images.githubusercontent.com/70513539/182803214-cc2d78b4-3ab4-4587-bfd6-a50657b7da2b.jpg)
## About iverilog 
Icarus Verilog is an implementation of the Verilog hardware description language.
## About GTKWave
GTKWave is a fully featured GTK+ v1. 2 based wave viewer for Unix and Win32 which reads Ver Structural Verilog Compiler generated AET files as well as standard Verilog VCD/EVCD files and allows their viewing
### Installing iverilog and GTKWave

#### For Ubuntu

Open your terminal and type the following to install iverilog and GTKWave
```
$   sudo apt get update
$   sudo apt get install iverilog gtkwave
```


### Functional Simulation
To clone the Repository and download the Netlist files for Simulation, enter the following commands in your terminal.
```
$   iverilog cps.v cps_tb.v
$   ./a.out
```
![Screenshot from 2023-10-25 20-44-15](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/fe92a906-93eb-4f2e-9bc6-46f1db885b9b)

```
$   gtkwave dump.vcd
```
![Screenshot from 2023-10-25 20-44-55](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/1c7b2fc5-b77b-4f5f-876e-47993a009c69)

## Synthesis of Verilog Code
#### About Yosys
Yosys is a framework for Verilog RTL synthesis. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains
```
$   sudo apt-get update
$   sudo apt-get -y install yosys
```
to synthesize
```
$   yosys -s yosys_run.sh
```
## GLS - Gate Level Simulation
GLS is generating the simulation output by running test bench with netlist file generated from synthesis as design under test. Netlist is logically same as RTL code, therefore, same test bench can be used for it.

**Why GLS?**

The main reasons for running GLS are as follows:

  * To verify the power up and reset operation of the design and also to check that the design does not have any unintentional dependencies on initial conditions.
  * To give confidence in verification of low power structures, absent in RTL and added during synthesis. 
  * It is a probable method to catch multicycle paths if tests exercising them are available.
  * Power estimation is done on netlist for the power numbers. 
  * To verify DFT structures absent in RTL and added during or after synthesis. Scan chains are generally inserted after the gate level netlist has been created. Hence, gate level simulations are often used to determine whether scan chains are correct. GLS is also required to simulate ATPG patterns. 
  * Tester patterns (patterns to screen parts for functional or structural defects on tester) simulations are done on gate level netlist.
  * To help reveal glitches on edge sensitive signals due to combination logic. Using both worst and best-case timing may be necessary.
  * It is a probable method to check the critical timing paths of asynchronous designs that are skipped by STA.
  * To avoid simulation artifacts that can mask bugs at RTL level (because of no delays at RTL level).
  * Could give insight to constructs that can cause simulation-synthesis mismatch and can cause issues at the netlist level.
  * To check special logic circuits and design topology that may include feedback and/or initial state considerations, or circuit tricks. If a designer is concerned about some logic then this is good candidate for gate simulation. Typically, it is a good idea to check reset circuits in gate simulation. Also, to check if we have any uninitialized logic in the design which is not intended and can cause issues on silicon.
  * To check if design works at the desired frequency with actual delays in place.
  * It is a probable method to find out the need for synchronizers if absent in design. It will cause “x” propagation on timing violation on that flop.

Below picture gives an insight of the procedure. Here while using iverilog, you also include gate level verilog models to generate GLS simulation.

![image](https://user-images.githubusercontent.com/72696170/183296780-4bad9547-69e9-4cee-b791-acb5a38951bf.png)



## Pre-Synthesis Simulation


![Screenshot from 2022-08-17 21-06-10](https://user-images.githubusercontent.com/70513539/185191281-f9b2f2ba-3362-43fd-b6aa-a299bad6f539.png)


## Post-Synthesis Simulation

![gtkwave](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/6d8f6243-3e37-4900-b778-61a75a54b456)

# read design
```
read_verilog cps.v
```
# generic synthesis
```
synth -top cps
```
# mapping to mycells.lib
```
dfflibmap -liberty /home/dhanush/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty /home/dhanush/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime,{D};strash;dch,-f;map,-M,1,{D}
clean
flatten
```

# write synthesized design
```
write_verilog -noattr cps_synth.v
```
```
show
```
![Screenshot from 2023-10-28 16-13-42](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/b18dbf39-eb40-4ce5-9ba2-866528082a71)

![Screenshot from 2023-10-28 16-01-49](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/95454aab-45d5-48e6-b1ca-a95a95a0cb56)


## Stats

![Screenshot from 2023-10-25 20-42-19](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/2f7ed401-8242-4c6b-a28b-14743fa9fccc)
## Physical Design using SKYwater 130 PDK:

In this module we will see various ASIC Design flows such as
1) RTL Design
2) Synthesis
3) FloorPlan
4) Placement
5) Clock Tree Synthesis(CTS)
6) Routing
7) SignOff
8) Tapeout

We have already completed first two parts of ASIC Design flow RTL design and Synthesis,from now we will complete remaining steps with the help of 
OpenLane and Magic softwares.

## essential

![Screenshot from 2023-11-01 19-35-08](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/083bc3ff-0854-428e-a29f-752783c33527)


## Installation of ngspice magic and OpenLANE

**ngspice**
- Download the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory
```
cd $HOME
sudo apt-get install libxaw7-dev
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
sudo make
sudo make install
```

**magic**

Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow. More about magic at http://opencircuitdesign.com/magic/index.html

```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
sudo make
sudo make install
```

**OpenLANE**
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, CU-GR, Klayout and a number of custom scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII. To know more go to https://github.com/The-OpenROAD-Project/OpenLane

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
# After reboot
docker run hello-world (should show you the output under 'Example Output' in https://hub.docker.com/_/hello-world)

- To install the PDKs and Tools
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
```
To test
```
make test
```

## OpenLANE Flow


![Screenshot from 2023-11-03 20-45-09](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/9bd0ccd1-5d19-418f-a842-db134f254b60)
![Screenshot from 2023-11-03 20-44-31](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/f0698631-014b-4f14-ada2-b30b5037afe5)

- Type ```make mount``` in the main Openlane folder.
- Then type ```./flow.tcl -interactive```.
- To prep the design type

```
prep -design pes_ripple_counter
```
![Screenshot from 2023-11-03 13-15-36](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/6bae2735-691e-4e75-8808-cb1e3fd5e7c0)


### Synthesis
- Type
```
run_synthesis
```
![Screenshot from 2023-11-03 13-18-07](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/ca151bcf-7dff-4427-b6ea-7fcd0b1b1a92)


**Physical Cells**
![Screenshot from 2023-11-03 13-17-34](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/b7f385a1-e028-4cf0-b8bc-683c092fff5f)



### Floorplan
- Now to run the floorplan we type
```
run_floorplan
```
![Screenshot from 2023-11-03 13-18-56](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/ad61c321-90bc-4d22-b320-b66f04eca87f)


- To view the design we type
```
magic -T /home/aniruddhan/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_ripco.def &
```
![Screenshot from 2023-11-03 13-21-59](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/7ca5efa2-626b-4a03-9dbb-0fee1b0020f2)
![Screenshot from 2023-11-03 13-23-06](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/d35de798-f2c7-4623-87f7-b9187dada85f)

### Placement
- Now to run the placement we type
```
run_placement
```
![Screenshot from 2023-11-03 13-25-20](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/022b7e30-44ba-484b-9e66-327c8536c282)

- To view the design we type
```
magic -T /home/aniruddhan/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_ripco.def &
```

![Screenshot from 2023-11-03 13-28-17](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/a045f301-e363-49d9-a493-0011c4643c87)
![Screenshot from 2023-11-03 13-28-53](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/eaaf640f-7189-47b8-ba40-b43d7916cde7)


### CTS(Clock Tree Synthesis)
- Now to run cts we type
```
run_cts
```
![image](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/88876983-57e7-4cf6-862b-e484f46cdaa2)

**Report**
The reports generated are given below
![Screenshot from 2023-11-03 13-41-42](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/6aee70a3-7e3f-4267-8fab-83fdc2088672)
![Screenshot from 2023-11-03 13-40-50](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/8dea8e56-7c56-441a-9b00-483b7148be07)
![Screenshot from 2023-11-03 13-40-36](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/18b9cb82-0489-41ab-8d81-a9588f7d836e)
![Screenshot from 2023-11-03 13-40-07](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/05e782a0-a61b-47c8-91a3-8ee9424d3a29)
![Screenshot from 2023-11-03 13-39-26](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/b873fc9c-6eef-4279-967a-a0f59553327d)
![Screenshot from 2023-11-03 13-39-05](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/841b1945-8206-44ba-ab23-23c0118920c8)
![Screenshot from 2023-11-03 13-38-11](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/f981ef36-7359-4ed7-8c94-a64a046751e7)

### Routing
- Now to run routing we type
```
run_routing
```

- To view the design we type
```
magic -T /home/aniruddhan/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_ripco.def &
```
![Screenshot from 2023-11-03 13-52-06](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/bd8bc2e7-771d-4e20-b95b-2fa7ff1323ee)
![Screenshot from 2023-11-03 13-47-49](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/0fff0f92-1a56-4920-8af8-a689d7485c83)
![Screenshot from 2023-11-03 13-43-21](https://github.com/dhanush-kumar-invo/pes_cps/assets/73644447/9660263b-ecf9-4228-bb69-9d942cf962ac)


**Statistics**
- Area = 3069 um2
- Internal Power = 3.86e-04 W
- Switching Power = 1.96e-04 w
- Leakage Power = 1.55e-09 w
- Total Power = 5.89e-04 w
