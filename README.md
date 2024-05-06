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













