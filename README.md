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



