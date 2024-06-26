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
* prep -design picorv32a
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
* prep -design picorv32a
* run_floorplan
After executing the above commands a floorplan def file will be generated. Below is the screenshot of the floorplan def file.

![generated floorplan def](https://github.com/Christi1992/Physical_design/assets/168098124/841a4c41-9f59-42e3-aaad-87b303204e70)

**Below is the history of the commands executed so far** 

![history](https://github.com/Christi1992/Physical_design/assets/168098124/a4eb73ba-33ad-4c6d-8109-179fb61b13df)

Now we need to look into the layout of the floorplan def, for which we need magic tool

Execute the below command to open magic:
* magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130Alibs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def def read picorv32a.floorplan.def &

**Below is the correspoding screenshot attached.**

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
* prep -design picorv32a
* run_placement

After exxecuting these commands we will get the placement def file in the corresponding results directory
B**elow is the screesnhot of the placement def and the magic command to open the placement layout**

![Placement magic ](https://github.com/Christi1992/Physical_design/assets/168098124/a9088760-9629-47ee-ae86-5a7239c2ae62)

**Layout after placement**

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

**A screenshot of the above explained process**

![Performed git clone](https://github.com/Christi1992/Physical_design/assets/168098124/f6d4f27c-a941-40ec-bf33-1304e95ec089)

![Copied 130tech file to vsdstdcelldesign directory](https://github.com/Christi1992/Physical_design/assets/168098124/c3205906-2eb1-406a-9736-5293f886f059)

**Open the inverter magic file sky130_inv.mag** 

![Magic command to load inverter cell](https://github.com/Christi1992/Physical_design/assets/168098124/bbe26d09-a86d-4f2c-b439-4fde43ab4018)

**Below is the inverter layout**

![Loaded inv cell](https://github.com/Christi1992/Physical_design/assets/168098124/52406bf8-c8aa-40a4-9e87-80a722b6c199)

**NMOS of inverter**

![NMOS](https://github.com/Christi1992/Physical_design/assets/168098124/aea7ad0e-936d-427c-b3b3-96d03f223eec)

**PMOS of inverter**

![Extracting sky130_inv into sky130_inv ext](https://github.com/Christi1992/Physical_design/assets/168098124/30b0c757-4ea7-4998-bd9d-ffb9d3b1c6dd)

**Do a spice extraction**

![extracted](https://github.com/Christi1992/Physical_design/assets/168098124/8d537849-8d5e-45c7-8c1a-8da913a55c8f)

**Screenshot of the extracted spice file**

![Spice file created](https://github.com/Christi1992/Physical_design/assets/168098124/b6569a27-a5db-427d-90b5-f7c51cec95ec)

Perform the spice simulation using the below commands
* ngspice sky130_inv.spice

![Spice simulation result](https://github.com/Christi1992/Physical_design/assets/168098124/82f6c19f-cf92-4836-9e2d-8a32e5ae7fd5)

**To observe the waveform execute the below command**
* plot y vs time a

![output plot of spicce](https://github.com/Christi1992/Physical_design/assets/168098124/d3dc28c0-a954-496a-9fa2-37300fdc72ba)

Modify the spice file like below and increase C2 to 2fF and observe the change in waveform

![Increased load to 2fF](https://github.com/Christi1992/Physical_design/assets/168098124/745f69db-dace-416b-b4e1-de18b2eb1f72)

![Plot of 2ff](https://github.com/Christi1992/Physical_design/assets/168098124/4ad27f6b-6cdc-4255-abf0-0008203d3e0d)

Now we need to calculate the following:
* Rise time: Time taken by the signal to move from 20% to 80% of its final value
* Rise time = (2.1933 - 2.1306)*10^-9 = 62.7 ps
Below are the correspoding screenshots

**20 %**

![20 percent of 3 3V rise transition](https://github.com/Christi1992/Physical_design/assets/168098124/81a0fdcd-897a-4b79-9e8f-cbd5fb260ddc)

**80%**
![80 percent of rise transition](https://github.com/Christi1992/Physical_design/assets/168098124/e56262e0-6903-4b1a-aeaf-926ac8b8f4cf)

![Correct rise values](https://github.com/Christi1992/Physical_design/assets/168098124/d6959262-b612-4c69-bdb9-2c34090550b9)

* Fall time: Time take for the signal to transition from 80% to 20% of its final value
* Fall time = (4.0955 - 4.05353)*10^-9 = 41.97 ps

**Below are the corresponding screenshots**

![fall transition 20%](https://github.com/Christi1992/Physical_design/assets/168098124/d481f6af-b1e1-4ea2-90e3-1dea949980d3)

![fall transition 80%](https://github.com/Christi1992/Physical_design/assets/168098124/13dcaffb-6276-4d6a-be99-66986f196baf)

**Fall transition values**

![fall transition](https://github.com/Christi1992/Physical_design/assets/168098124/648dc525-3eb8-4137-81a9-51585327044d)

* Propagation delay = (2.1537 - 2.1048)*10^-9 = 48.9ps

![50 % of input and output for cell rise delay](https://github.com/Christi1992/Physical_design/assets/168098124/f66fff87-0df9-41fc-ab9a-e84fe566be26)

![Cell rise delay values](https://github.com/Christi1992/Physical_design/assets/168098124/99903a33-30ce-44e3-ad5c-e4c1e09b2ca8)

* Cell fall delay = (4.07763 - 4.0499)*10^-9 = 27.73ps

![fall delay values](https://github.com/Christi1992/Physical_design/assets/168098124/5c87399b-1f89-416b-a27a-033870a70bd3)

**DRC correction and rules**
For DRC correction download the files using the below commands

* wget http://opencircuitdesign.comopen_pdks/archive/drc_tests.tgz

![DRC dowload complete](https://github.com/Christi1992/Physical_design/assets/168098124/5dcebe8a-e253-4c2c-99f3-d6a80cdb4977)

![drc files](https://github.com/Christi1992/Physical_design/assets/168098124/6737aa0c-6d74-44e8-95c1-227e121ff6a2)

To use the magic tool
* magic -d XR
Open the met3.mag file

![met3 meg file](https://github.com/Christi1992/Physical_design/assets/168098124/72b27169-54e4-4910-a0b4-d4be6de1bb59)

**To see contact cuts**
* cif see VIA2

![cif see via2](https://github.com/Christi1992/Physical_design/assets/168098124/a0a639ce-b131-484e-9330-1e136c7242c7)

# Fixing poly.9 error

![Load poly](https://github.com/Christi1992/Physical_design/assets/168098124/7c391fd4-5f1d-48da-8b1a-95b90267163f)

![box spacing micron distance](https://github.com/Christi1992/Physical_design/assets/168098124/fe29f8db-95b0-4c71-a8f9-b86c51a6307d)

To fix the poly spacing error make the below changes in the tech file

![image](https://github.com/Christi1992/Physical_design/assets/168098124/db9b522e-bcc4-42d8-8aaa-0bdb676f856d)

Now load the tech file and do check drc to fix the poly.9 error

![fixed poly error](https://github.com/Christi1992/Physical_design/assets/168098124/27b4b22a-8752-41b5-94f6-9b4dc245aefe)

**Lab challenge exercise to describe DRC error as geometric construct**
Load nwell.mag and execute the following commands

![image](https://github.com/Christi1992/Physical_design/assets/168098124/151da57d-644a-45a7-8156-c08fba775257)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/b06700a4-2105-4420-83c5-294cf5974440)

**Lab challenge exercise to find the missing or incorrect rule that should be fixed**
![image](https://github.com/Christi1992/Physical_design/assets/168098124/8a542e9c-d2be-4c8f-87ed-6f86b43ba946)

Make the below changes mentioend
![image](https://github.com/Christi1992/Physical_design/assets/168098124/741523a1-732b-4339-9bc0-899129b26b7b)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/7dcf1ea7-8439-4bdb-9d1c-cf05ab7237d0)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/3afabe61-6f05-4818-a3d8-c77a4f1a8f48)

# Day 4 - Prelayout design setup and importance of a good clock tree 
Main requirement of P&R: The input and output ports must lie on the intersection of the vertical and horizontal tracks. The width of the standard cell must be odd multiple of track pitch. The height of the standdard cell must be odd multiple of the track vertical pitch.

Go to track.info
For example li1 is in horizontal direction with a offset of 0.23 and a pitch of 0.46
![image](https://github.com/Christi1992/Physical_design/assets/168098124/745a7ac8-8deb-4eb7-811c-c61ee6366be7)

**Now check if ports A and Y are on the intersection of Li tracks**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/afe6fcec-31f6-447d-8737-11b826064530)

# Convert magic layout to std cell lef

![image](https://github.com/Christi1992/Physical_design/assets/168098124/391908f0-d950-4db1-99fa-168672772b85)

**Open the saved .mag using magic**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/afe5a303-fba1-4c07-b647-1ae511be2421)

**Create a lef file**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/40b0ff4e-98e9-4d74-9656-b8764aa99d99)

**Contents of LEF file**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/c217954f-1243-4a0f-80fa-0c348775c500)

Plug this left file into PICORV32A design

Copy the lef file into SRC directory of PICORV

![image](https://github.com/Christi1992/Physical_design/assets/168098124/9ca9018d-b821-40fb-bb1f-2fa734b5469c)

Now we are going to run from synthesis again by making some settings changes which will help in fixing the slack violations
First copy the LEF file and the libraries into SRC folder of PICORV32A

![image](https://github.com/Christi1992/Physical_design/assets/168098124/6b7d2cb2-3569-456c-8690-b53670bbf664)

Add these lines in config.tcl

![image](https://github.com/Christi1992/Physical_design/assets/168098124/6b2aee7e-4d9e-4787-a771-da77c8453346)

Its time to run synthesis now
Execute the below commands in openlane directory
* docker
* ./flow.tcl -interactive
* prep -design picorv32a
* set lefs [glob $::env(DESIGN_DIR)/src/*.lef
* add_lefs -src $lefs
* run_synthesis

**The custom cells are added and the synthesis is complete now with a slack of -23.89**

**We can make a settings change now to imprive slack**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/2277bafc-06dd-4f45-9420-9bcf76cb759e)

**SLACK is MET now!!**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/b46b90ab-3045-4522-99ee-5e4ebc4f54e5)

We can start floorplan now with the below commands
* init_floorplan
* place_io
* tap_decap_or

**Floorplan is done now**
![image](https://github.com/Christi1992/Physical_design/assets/168098124/547761af-968d-4869-90dc-7b62477894c6)

**Run placement using run_placement**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/2b8ded39-d58a-4183-abb3-e9d001ec2a6d)

**Use magic layout to view the placement layout and to view our custom cells**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/7149592a-ada3-471c-814c-ff58920c9656)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/825280c3-310e-4df6-aa0d-81afe2882081)

**Power ground rails are shared by the cells through abutment**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/212572a6-477c-4733-9ecf-669dbbb2b5f9)

## Lab steps to configure OpenSTA for post-synth timing analysis
We have to copy two files to src directory of PICORV32A
* pre_sta.conf
* my_base.sdc

![image](https://github.com/Christi1992/Physical_design/assets/168098124/8fcec6f0-b798-4193-a4b9-bb675f717d07)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/d7328c36-91b3-494d-b59b-0ebabf28d658)

**After STA**
Slack is met. SO STA is successful

![image](https://github.com/Christi1992/Physical_design/assets/168098124/1570ae44-3fa8-4f0e-ab70-a782b7f9d22c)

# Clock tree synthesis TritonCTS and signal integrity

Updated the old netlist with the new netlist

![image](https://github.com/Christi1992/Physical_design/assets/168098124/53bed49c-14c3-48ba-90c4-9dfc60409f89)

**Do floorplan on the new netlist now**
**run_floorplan**
![image](https://github.com/Christi1992/Physical_design/assets/168098124/a67176b5-4193-41a8-a911-646e655082e2)

**run_placemnt**
  
![image](https://github.com/Christi1992/Physical_design/assets/168098124/03648a61-917c-407f-ac60-fc4a3331025c)

**run_cts**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/6725b830-0a53-4369-b4ab-c1e1f8b546de)

We get another netlist after completion of CTS as clock buffers are added and it will be updated in the netlist

![image](https://github.com/Christi1992/Physical_design/assets/168098124/ec267547-ed4d-48b7-85a1-0fd490acb5c2)

**Analyse timing with real clocks**
**Include the def, lef, library files, sdc and do a report_check**
![image](https://github.com/Christi1992/Physical_design/assets/168098124/d2c0bf81-6623-499b-9d58-05f48ac9e826)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/698f4f2d-9eab-4992-9e3f-2d02e9c6f2be)

**Run CTS from placement def as we made a few settings changes
**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/43f6288b-820e-4c27-a483-1aaa166c90f4)

**Check for the current def. It must be from placement**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/07143baf-a99a-4cb5-96cc-771230c9a628)

**run_cts**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/f64b52db-09d4-4b82-aafd-9bd7cacbbd8a)

**Slack MET**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/ca7275b8-f2e1-4d90-9c93-65ffe8aa49f2)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/390f88b5-114c-4e0f-a7ec-034a6d347a6f)

**Clock skew report for setup and hold**  
![image](https://github.com/Christi1992/Physical_design/assets/168098124/38cec09c-bee9-4d72-a797-c01e3e7c8f88)

**Day 5 - Final stepls for RTL to GDS using tritonRoute and openSTA**
**PDN GENERATION**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/d93c92f2-2190-4443-8dc5-204483ffd2ef)

**For PDN visualisation**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/f335385f-2397-422f-934b-02059159e6fc)

**PDN.def is genereated now** 

![image](https://github.com/Christi1992/Physical_design/assets/168098124/83013877-2873-4b0a-ac0e-3d89108cdca0)

**Execute "run_routing"**
**Routing done**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/ca3ec0f7-c139-4dbf-b314-bed17899b086)

**Triton routing**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/69d9c877-f9b4-4390-ab02-dd9d4350a977)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/c5fcd768-285c-4d86-aee5-d930e6cc4af7)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/362d4c99-4bce-4181-be9e-2e5b5b8a6e35)

![image](https://github.com/Christi1992/Physical_design/assets/168098124/07c551e8-67e6-4d3b-8c00-ff17eec4d0d8)

**Routing output files generated**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/55c7699b-23e5-4040-b6ba-4c069553f55f)

**Fastroute guide**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/5b5c345d-f08f-42f1-8e63-ec0de682e63d)

**Routed layout in magic tool**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/f2601585-ff3c-432e-b919-ba49574373b8)

**The final GDS layout**

![image](https://github.com/Christi1992/Physical_design/assets/168098124/83166661-9c43-49bd-a848-3e02234e8ff2)
