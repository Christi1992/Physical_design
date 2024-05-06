# VSD SOC DESIGN 
# DAY 1 - Inception of open-source EDA, Openlane and sky130PDK 
## An example of IC package: QFN-48 pins
![image](https://github.com/Christi1992/Physical_design/assets/168098124/8cccc627-a9c8-4e52-a8c2-5c26d0ba8ee4)

## ASIC design inputs:
* RTL libraries
* EDA tools
* PDK kit
## Simplified RTL to GDSII flow
![image](https://github.com/Christi1992/Physical_design/assets/168098124/3aae3771-1ca1-4478-b680-05e4bd1b58ee)

# Synthesis: 
The process converting RTL to Synthesized gate level netlist. The inputs of Synthesis are RTL files, libraries, SDC constraints. Output is the netlist which has the gate level interconnections. Now we are going to do synthesis for PICORV32A design. 
We have to do data preparation for PICORV32A. The corresponding screenshot is shared below.

![design setup ](https://github.com/Christi1992/Physical_design/assets/168098124/7773004f-c794-44db-9d52-3b4f22f8665d)

Execute the below commands to run synthesis
* docker
* ./flow.tcl -interactive
* run_synthesis
The below is the logs of synthesis

![synthesis](https://github.com/Christi1992/Physical_design/assets/168098124/9885b164-5a62-4e02-a8d9-f532e69eca96)

# Syntheis task - To calculate the ratio of D flip flops.
Total no of cells = 14876, Total no of D flip flops = 1613, D flip flop ratio = 1613/14876 * 100 = 10.842%

The below is the screenshot of the netlist generated

![netlist generated](https://github.com/Christi1992/Physical_design/assets/168098124/839305ce-e052-4f20-aed1-6cb805a7c015)

The below is the screenshot of the timing report after synthesis

![WNS after synth](https://github.com/Christi1992/Physical_design/assets/168098124/619a13ba-a209-4113-8ba8-e46a2f1fd4af)

# Day 2 - Good floorplan vs. Bad floorplan and introduction to library cells

Floorplan is the process of creating the core area, die area, macro placement, IO pin placement, physical cell placement. Sanity checks are performed on the netlist generated, library files and SDC files. Sometimes powerplan is also done together after macro placement. We have to generate a def file after floorplan to get the locations of the macros and IO pin locations. 

Execute the below commands to run the floorplan:
* docker
* ./flow.tcl -interactive
* run_floorplan
After executing the above commands a floorplan def file will be generated. Below is the screenshot of the floorplan def file.

![generated floorplan def](https://github.com/Christi1992/Physical_design/assets/168098124/841a4c41-9f59-42e3-aaad-87b303204e70)

Below is the history of the commands executed so far 

![history](https://github.com/Christi1992/Physical_design/assets/168098124/a4eb73ba-33ad-4c6d-8109-179fb61b13df)

Now we need to look into the layout of the floorplan def, for which we need magic tool

Execute the below command to open magic:
* magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130Alibs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def def read picorv32a.floorplan.def &

Below is the correspoding screenshot attached.

![Opened magic](https://github.com/Christi1992/Physical_design/assets/168098124/c56b61d4-8345-4e4c-aac5-9353f31cfe2e)

# Floorplan layout
![Opened magic layout](https://github.com/Christi1992/Physical_design/assets/168098124/84383509-d48b-4ff7-999e-f9742f8c8aba)

# An image of the decap cells and IO pins

![floorplan layout](https://github.com/Christi1992/Physical_design/assets/168098124/e01c93ab-a1c7-44ef-9810-33f9adf48d80)

Below is the snip of a Pin and its metal layer information is present in the tkcon window

![tkcon tcl](https://github.com/Christi1992/Physical_design/assets/168098124/e25990ef-4f58-4c3f-a80d-91efc2baed01)

# Placement
Next we have to run placement stage.
Execute the below commands 
* docker
* ./flow.tcl -interactive
* run_placement

After exxecuting these commands we will get the placement def file in the corresponding results directory
Below is the screesnhot of the placement def and the magic command to open the placement layout

![Placement magic ](https://github.com/Christi1992/Physical_design/assets/168098124/a9088760-9629-47ee-ae86-5a7239c2ae62)

Below is the layout after placement

![Placement magic file](https://github.com/Christi1992/Physical_design/assets/168098124/b518ad05-a02b-41f3-bcb4-8bc84aaf762b)

# Day 3 - Design library cell using magic layout and Ngspice characterisation

Run synthesis with an environment settings change for IO pins distance and observe the layout
Below is thee screenshot for the set of commands to do them 

![Floorplan after IO2](https://github.com/Christi1992/Physical_design/assets/168098124/b6cce208-ce8b-4d10-8896-083b56d3e829)

We are basically changing the equidistance spacing of the IO pins. We can observe this change done using magic layout

![IO pins not equidistant after the setting change](https://github.com/Christi1992/Physical_design/assets/168098124/1d438c55-ca74-40f0-a9c2-f9668b03cbea)

The next task is to clone the inverter from Githouse repository
Execute the below commad in your work directory
* git clone https://github.com/nickson-jose/vsdstdcelldesign.git

I have attached a screenshot of the above explained process

![Performed git clone](https://github.com/Christi1992/Physical_design/assets/168098124/f6d4f27c-a941-40ec-bf33-1304e95ec089)

![Copied 130tech file to vsdstdcelldesign directory](https://github.com/Christi1992/Physical_design/assets/168098124/c3205906-2eb1-406a-9736-5293f886f059)

Open the inverter magic file sky130_inv.mag 

![Magic command to load inverter cell](https://github.com/Christi1992/Physical_design/assets/168098124/bbe26d09-a86d-4f2c-b439-4fde43ab4018)

**Below is the inverter layout**

![Loaded inv cell](https://github.com/Christi1992/Physical_design/assets/168098124/52406bf8-c8aa-40a4-9e87-80a722b6c199)

**NMOS of inverter**

![NMOS](https://github.com/Christi1992/Physical_design/assets/168098124/aea7ad0e-936d-427c-b3b3-96d03f223eec)

**PMOS of inverter**

![Extracting sky130_inv into sky130_inv ext](https://github.com/Christi1992/Physical_design/assets/168098124/30b0c757-4ea7-4998-bd9d-ffb9d3b1c6dd)

**Do a spice extraction**

![extracted](https://github.com/Christi1992/Physical_design/assets/168098124/8d537849-8d5e-45c7-8c1a-8da913a55c8f)

Below is the screenshot of the extracted spice file

![Spice file created](https://github.com/Christi1992/Physical_design/assets/168098124/b6569a27-a5db-427d-90b5-f7c51cec95ec)

Perform the spice simulation using the below commands
* ngspice sky130_inv.spice

![Spice simulation result](https://github.com/Christi1992/Physical_design/assets/168098124/82f6c19f-cf92-4836-9e2d-8a32e5ae7fd5)

To observe the waveform execute the below command
* plot y vs time a

![output plot of spicce](https://github.com/Christi1992/Physical_design/assets/168098124/d3dc28c0-a954-496a-9fa2-37300fdc72ba)

Modify the spice file like below and increase C2 to 2fF and observe the change in waveform

![Increased load to 2fF](https://github.com/Christi1992/Physical_design/assets/168098124/745f69db-dace-416b-b4e1-de18b2eb1f72)

![Plot of 2ff](https://github.com/Christi1992/Physical_design/assets/168098124/4ad27f6b-6cdc-4255-abf0-0008203d3e0d)

Now we need to calculate the following:
* Rise time: Time taken by the signal to move from 20% to 80% of its final value
* Rise time = (2.1933 - 2.1306)*10^-9 = 62.7 ps
Below are the correspoding screenshots

# 20 %

![20 percent of 3 3V rise transition](https://github.com/Christi1992/Physical_design/assets/168098124/81a0fdcd-897a-4b79-9e8f-cbd5fb260ddc)

# 80%
![80 percent of rise transition](https://github.com/Christi1992/Physical_design/assets/168098124/e56262e0-6903-4b1a-aeaf-926ac8b8f4cf)

![Correct rise values](https://github.com/Christi1992/Physical_design/assets/168098124/d6959262-b612-4c69-bdb9-2c34090550b9)

* Fall time: Time take for the signal to transition from 80% to 20% of its final value
* Fall time = (4.0955 - 4.05353)*10^-9 = 41.97 ps
Below are the corresponding screenshots

![fall transition 20%](https://github.com/Christi1992/Physical_design/assets/168098124/d481f6af-b1e1-4ea2-90e3-1dea949980d3)

![fall transition 80%](https://github.com/Christi1992/Physical_design/assets/168098124/13dcaffb-6276-4d6a-be99-66986f196baf)

![fall transition](https://github.com/Christi1992/Physical_design/assets/168098124/648dc525-3eb8-4137-81a9-51585327044d)

* Propagation delay = (2.1537 - 2.1048)*10^-9 = 48.9ps

![50 % of input and output for cell rise delay](https://github.com/Christi1992/Physical_design/assets/168098124/f66fff87-0df9-41fc-ab9a-e84fe566be26)

![Cell rise delay values](https://github.com/Christi1992/Physical_design/assets/168098124/99903a33-30ce-44e3-ad5c-e4c1e09b2ca8)

* Cell fall delay = (4.07763 - 4.0499)*10^-9 = 27.73ps

![fall delay values](https://github.com/Christi1992/Physical_design/assets/168098124/5c87399b-1f89-416b-a27a-033870a70bd3)

# DRC correction and rules
For DRC correction download the files using the below commands

* wget http://opencircuitdesign.comopen_pdks/archive/drc_tests.tgz

![DRC dowload complete](https://github.com/Christi1992/Physical_design/assets/168098124/5dcebe8a-e253-4c2c-99f3-d6a80cdb4977)


   

















