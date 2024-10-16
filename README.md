# NASSCOM-VSD-SoC-picorv32a-Design
This repository consists of a 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM using Open_Source PDKs and EDA tools. 
# **Contents**
<ol>
  <li><a href="#Section 1">Section 1: Inception of open-source EDA, OpenLANE and Sky130 PDK.</a></li>
  <li><a href="#Section 2">Section 2: Good floorplan vs bad floorplan and introduction to library cells.</a></li>
  <li><a href="#Section 3">Section 3: Design library cell using Magic Layout and ngspice characterization.</a></li>
  <li><a href="#Section 4">Section 4: Pre-layout timing analysis and importance of good clock tree.</a></li>
  <li><a href="#Section 5">Section 5: Final steps for RTL2GDS using tritonRoute and OpenSTA.</a></li>
</ol>
<br>

## Section 1: Inception of open_source EDA, OpenLANE and SKY130 PDK

Section 1 tasks:-
1. Performing Synthesis for the picorv32a design using OpenLANE flow and generate the necessary outputs.
2. Calculating the flop ratio.

#### 1. Performing Synthesis for the picorv32a design using OpenLANE flow and generate the necessary outputs.

Commands to invoke the OpenLANE is

```bash
# Change the directory to openlane flow directory by 
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# As to invoke the OpenLANE, we need to run the docker. We aliased that long command with a single word 'docker', so by running this command we can invoke the OpenLANE flow docker.
docker
```

```tcl
# To access the OpenLANE flow interactively i.e., step by step
./flow.tcl -interactive

# For proper functionality of the OpenLANE flow, we require some packages
package require openlane 0.9

# Upto now OpenLANE flow is successfully invoked and it is ready to run the design.
```

![Screenshot 2024-10-04 200324](https://github.com/user-attachments/assets/3a8d3277-0e08-4950-812b-d8f8724f3fb8)

```tcl
# Now we need to prepare the design for the design 'picorv32a'
prep -design picorv32a

# After preparation of the design, Synthesis is started by  
run_synthesis
```

![Screenshot 2024-10-04 200401](https://github.com/user-attachments/assets/f697647e-b519-4295-94a7-13c3f9a26b51)
![Screenshot 2024-10-04 195505](https://github.com/user-attachments/assets/21f3a6f4-4de5-454c-8371-c4081d2e13b4)

After completion of synthesis, gate level netlist is generated using Yosys.

![Screenshot 2024-10-04 195821](https://github.com/user-attachments/assets/f67e8ea2-ddcb-40bc-b871-db75ae63b8a6)

#### 2. Calculate the flop ratio.

To Calculare the Flop Ratio:-
```math
Flop\ Ratio = \frac{Number\ of\ Required\ Cells}{Total\ Number\ of\ Cells}
```
```math
Percentage = Flop\ Ratio * 100
```

![Screenshot 2024-10-04 195649](https://github.com/user-attachments/assets/bf6f4eb3-633d-4304-b929-9fbe15a30fce)

![Screenshot 2024-10-04 195732](https://github.com/user-attachments/assets/27a9d942-38ad-4bfa-af8e-210bf7c85ad2)

Calculation of Flop Ratio of DFFs from statistics report file,
```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFFs = 0.108429685 * 100 = 10.8429685\ \%
```

## Section 2: Good floorplan vs bad floorplan and introduction to library cells

Section 2 tasks:- 
1. Run picorv32a design floorplan.
2. Calculate the die area in microns from floorplan.def file.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run picorv32a design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

#### 1. Running picorv32a design floorplan.

```tcl
# Gate level netlist is synthesized, after floorplanning is designed by,
run_floorplan
```

To design according to the required optimizations, there are various switches are used to control the flow and design according to the requirements,

![Screenshot 2024-10-09 180532](https://github.com/user-attachments/assets/42d30a89-607b-4af3-8600-2c3c1b04e0e4)

![Screenshot 2024-10-09 180910](https://github.com/user-attachments/assets/54081c06-ad23-46f1-a467-d389360f91f0)

![Screenshot 2024-10-09 180934](https://github.com/user-attachments/assets/f8ef167b-70e5-4f9a-8a23-a9ccbaf82424)

#### 2. Calculate the die area in microns from floorplan.def file.

```math
Area\ of\ die\ in\ microns = Die\ width\ in\ microns * Die\ height\ in\ microns
```

![Screenshot 2024-10-09 181714](https://github.com/user-attachments/assets/75ba539d-9a23-4ee4-b2a0-7715e8369d36)

According to floorplan.def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212\ Square\ Microns
```

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

```bash
# Change directory to path containing generated floorplan.def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_12-26/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Floorplan.def in the magic tool,

![Screenshot 2024-10-09 184645](https://github.com/user-attachments/assets/c3a4aa8b-652b-464f-ba1c-78f843d1113a)

We can see the port placement, diagonally placing decap & tap cells and unplaced stnd_cells placed at the origin,

![Screenshot 2024-10-09 185118](https://github.com/user-attachments/assets/afab80ce-0858-498e-82a8-d6411053c1ee)

Port layer information as per config.tcl,

![Screenshot 2024-10-09 185427](https://github.com/user-attachments/assets/7b9d09fa-1058-4b3d-9127-f8898f0820da)
![Screenshot 2024-10-09 185519](https://github.com/user-attachments/assets/56ce2789-c173-445c-8611-fb1bc3c303e4)

#### 4. Run picorv32a design congestion aware placement using OpenLANE flow and generate necessary outputs.

```tcl
# Congestion aware placement
run_placement
```

![Screenshot 2024-10-09 191233](https://github.com/user-attachments/assets/d2c8bbb8-8373-46a6-90ad-6ee8bda196b8)
![Screenshot 2024-10-09 191346](https://github.com/user-attachments/assets/72e1af16-a768-45b9-a0b6-d598db29118b)

#### 5. Load generated placement def in magic tool and explore the placement.

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_13-39/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Screenshot 2024-10-09 193312](https://github.com/user-attachments/assets/cf14e136-f867-4536-9450-79136e319e08)

Metal layers to place the cells,

![image](https://github.com/user-attachments/assets/2955cee3-b626-4b2b-8062-be38bac2b69d)
![image](https://github.com/user-attachments/assets/a197314f-d239-4fc6-8157-847468aa5879)

Formation of via in between the metal layers,

![image](https://github.com/user-attachments/assets/142d2f49-bbce-4d09-936a-1133ed3ded5f)

Standard cells are placed,

![Screenshot 2024-10-09 194409](https://github.com/user-attachments/assets/3369fe19-1ff9-4ed5-a126-bbc707cfc990)

## Section 3 - Design library cell using Magic Layout and ngspice characterization

* Section 3 tasks:-
1. Clone custom inverter standard cell design from github repository.
2. Spice extraction of inverter.
3. Practised designing inverter layout, in which pmos is double the width of nmos.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.

#### 1. Clone custom inverter standard cell design from github repository.

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

![Screenshot 2024-10-14 144752](https://github.com/user-attachments/assets/0736f2fa-1cad-4860-8c37-c68ed970b7a6)

Custom layout in magic, 

![Screenshot 2024-10-14 145445](https://github.com/user-attachments/assets/7814ef32-95d1-4b17-a08b-fd792cb19a04)

Metal layers in the skywater 130nm process design kit,

![Screenshot 2024-10-14 150106](https://github.com/user-attachments/assets/d8db2671-0b80-4abc-b136-2ab2eb92f2eb)

Analysing the custom inverter layout

![Screenshot 2024-10-14 150345](https://github.com/user-attachments/assets/95971680-98ef-4112-8fa5-578a65217d80)
![Screenshot 2024-10-14 150408](https://github.com/user-attachments/assets/f07a7a1b-c6cf-40bb-a03e-226cda4da600)

#### 2. Spice extraction of inverter.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Extracted spice netlist from the custom inverter,

![Screenshot 2024-10-14 151105](https://github.com/user-attachments/assets/2d2b32e4-3a38-4bd2-8df3-fc9b72741960)

#### 3. Practised designing inverter layout, in which pmos is double the width of nmos.

![Picture2](https://github.com/user-attachments/assets/99e5e08c-7444-481b-a1d7-e9db9e209b97)

DRC errors while creating the nmos,

![Picture1](https://github.com/user-attachments/assets/0cae978f-61a2-4dda-bce9-d15bec5333fc)

Verifying my layout by doing simulation in ngspice,

![Picture3](https://github.com/user-attachments/assets/462dcbd3-ea81-45a3-beb0-94c1915b313a)
![Picture4](https://github.com/user-attachments/assets/37075ebc-9b87-444f-8965-c225a1673dd0)

![Picture5](https://github.com/user-attachments/assets/8c4ff37d-2f49-4cd9-a0bf-053d56388727)

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Screenshot 2024-10-14 151347](https://github.com/user-attachments/assets/508c4fd8-6f9a-42b0-af6f-43a72e00df64)

Cloned custom inverter spice netlist is edited to do a simulation in ngspice,

![Screenshot 2024-10-14 154154](https://github.com/user-attachments/assets/8e947071-e9b1-4f68-b617-2acc2c46d7c8)

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
ngspice sky130_inv.spice

plot y vs time a
```

![Screenshot 2024-10-14 154355](https://github.com/user-attachments/assets/d0729763-7624-4540-b274-f46d2ba392d7)
![Screenshot 2024-10-14 154422](https://github.com/user-attachments/assets/37ad43e4-63cf-43fe-803c-f481cb8413d4)

To get the rise transistion time, we need to take the 20% and 80% of the output rising waveform.

Let's take 20% is 660mV

![Screenshot 2024-10-14 154741](https://github.com/user-attachments/assets/4e120abd-f499-4ed4-a5e9-8ab804c135cc)
![Screenshot 2024-10-14 154950](https://github.com/user-attachments/assets/5cc1ca87-4ebc-43c0-ba9a-acd03048cdb0)

And 80% is 2.64V

![Screenshot 2024-10-14 155135](https://github.com/user-attachments/assets/e01cfe48-58f7-4787-960d-8672a7d90313)
![Screenshot 2024-10-14 155154](https://github.com/user-attachments/assets/e469eeaf-c8f9-447b-9e22-839e06e68576)

```math
Rise\ transition\ time = 2.24688 - 2.18239 = 0.06449\ ns = 64.49\ ps
```

To get the fall transistion time, we need to take the 20% and 80% of the output falling waveform.

Let's take 20% is 660mV

![Screenshot 2024-10-14 155547](https://github.com/user-attachments/assets/1f6c766d-5ca6-4167-ab86-4d1de8578e07)
![Screenshot 2024-10-14 155603](https://github.com/user-attachments/assets/8dee8a36-d14c-4eee-9b95-dfcf0e2161ec)

And 80% is 2.64V

![Screenshot 2024-10-14 155721](https://github.com/user-attachments/assets/0ec1f023-e894-481f-8009-9194ddb9f0d1)
![Screenshot 2024-10-14 155744](https://github.com/user-attachments/assets/1ad17b8d-65b3-4ed4-949f-ba83c1d7e6f9)

```math
Fall\ transition\ time = 4.09508 - 4.05298 = 0.0421\ ns = 42.1\ ps
```

To get the rise cell delay, we need to take the 50% of output rise of 50% of input fall.

50% of 3.3V is 1.65V

![Screenshot 2024-10-14 155917](https://github.com/user-attachments/assets/8e53840b-c118-40e9-ae13-84934d00f086)
![Screenshot 2024-10-14 160013](https://github.com/user-attachments/assets/a1b7d6bc-5cc5-46df-9956-6a4329f2385e)

```math
Rise\ Cell\ Delay = 2.21159 - 2.14992 = 0.06167\ ns = 61.67\ ps
```

To get the fall cell delay, we need to take the 50% of output fall of 50% of input rise.

50% of 3.3V is 1.65V

![Screenshot 2024-10-14 160148](https://github.com/user-attachments/assets/1f03347b-ec7c-4de7-a4fb-c9e7ac39b5d4)
![Screenshot 2024-10-14 160200](https://github.com/user-attachments/assets/f89b102c-0554-4dbe-96fa-85a114e5b04c)

```math
Fall\ Cell\ Delay = 4.07 - 4.05 = 0.02\ ns = 20\ ps
```

## Section 4 - Pre-layout timing analysis and importance of good clock tree

* Section 4 tasks:-
1. Verify the custom layout and save the finalized layout with custom name and open it.
2. Generate lef from the layout.
3. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.
4. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.
5. Run openlane flow synthesis with newly inserted custom inverter cell.
6. Reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
7. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.
8. Do Post-Synthesis timing analysis with OpenSTA tool.
9. Make timing ECO fixes to remove all violations.
10. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.
11. Post-CTS OpenROAD timing analysis.
12. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

#### 1. Verify the custom layout and save the finalized layout with custom name and open it.

![Screenshot 2024-10-14 173216](https://github.com/user-attachments/assets/e0a7cb7b-8918-4d18-97fe-4ae6eb568cc7)

Save the custom layout with our reference name

![Screenshot 2024-10-14 173856](https://github.com/user-attachments/assets/3a845e81-458f-4e99-8bbd-744e2997e09d)

```tcl
# Command to save as
save sky130_myinv.mag
```

Command to open the newly saved layout

```bash
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_myinv.mag &
```
#### 2. Generate lef from the layout.

Command for tkcon window to write lef

```tcl
# lef command
lef write
```

![Screenshot 2024-10-14 174130](https://github.com/user-attachments/assets/c25fe87e-41db-411b-a1b3-8fe0bf50d1c4)

Newly created lef file after saving the file with our name,

![Screenshot 2024-10-14 174220](https://github.com/user-attachments/assets/77c70e67-d23f-4a6b-9472-c1622c7734da)

#### 3. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

```bash
# Copy lef file
cp sky130_myinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

![Screenshot 2024-10-14 175458](https://github.com/user-attachments/assets/009e1239-3610-413f-9127-e94d79dc30a4)

#### 4. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow

```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

Edited config.tcl to include the added lef and change library to ones we added in src directory

![Screenshot 2024-10-14 180856](https://github.com/user-attachments/assets/c4840ae6-1b84-45cf-807c-6b396b378671)

#### 5. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

![Screenshot 2024-10-14 181347](https://github.com/user-attachments/assets/9217f72f-a0ee-4779-8db9-8fc148dd2952)
![Screenshot 2024-10-14 181514](https://github.com/user-attachments/assets/4fa6f090-1dc4-4450-9fb2-9c38eb320729)
![Screenshot 2024-10-14 181858](https://github.com/user-attachments/assets/67f4971a-6512-4400-9104-6f5ef2e39eb6)

#### 6. Reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Noting down current design values generated before modifying parameters to improve timing

![Screenshot 2024-10-14 181841](https://github.com/user-attachments/assets/1a1016ef-fbc9-4b90-ae16-56f1098106f2)

Commands to view and change parameters to improve timing and run synthesis

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 09-10_13-39 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Verifying the our newly inserted layout in lef

![Screenshot 2024-10-14 183701](https://github.com/user-attachments/assets/6b7f9831-9880-468d-9eb5-009b46ed5f3c)

![Screenshot 2024-10-14 183404](https://github.com/user-attachments/assets/669e411d-634f-4376-9437-e4756139ee82)
![Screenshot 2024-10-14 184034](https://github.com/user-attachments/assets/683712b6-18d6-4f5c-beae-f7b5f67327f6)
![Screenshot 2024-10-14 185607](https://github.com/user-attachments/assets/2a715657-71a4-43a9-92ad-cf38c80037f3)

Comparing to previously noted run values area has increased and worst negative slack has become 0

![Screenshot 2024-10-14 185554](https://github.com/user-attachments/assets/538c9340-47b7-4c85-828b-d0eb6b34ce60)

#### 7. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command

```tcl
# Now we can run floorplan
run_floorplan
```

Screenshots of command run

![Screenshot 2024-10-14 185959](https://github.com/user-attachments/assets/ff620f52-d908-4dd0-9800-93f6a3a72eed)
![Screenshot 2024-10-14 190241](https://github.com/user-attachments/assets/952b86a1-9e81-47a4-a833-58d014bb83d5)
![Screenshot 2024-10-14 190301](https://github.com/user-attachments/assets/23f8cc1d-f107-4508-b4e4-64468367dd4b)

Since we are facing unexpected un-explainable error while using `run_floorplan` command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on `Floorplan Commands` section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```tcl
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or
```

![Screenshot 2024-10-14 190320](https://github.com/user-attachments/assets/4eac8549-7d9c-412a-844d-85a83043e4d1)
![Screenshot 2024-10-14 190355](https://github.com/user-attachments/assets/b8409dea-b472-4c02-a457-b84b1f7e3f5e)
![Screenshot 2024-10-14 190424](https://github.com/user-attachments/assets/12e15de4-ae08-470c-a0ae-67d35ab4a7b3)

Now that floorplan is done we can do placement using following command

```tcl
# Now we are ready to run placement
run_placement
```

![Screenshot 2024-10-14 190640](https://github.com/user-attachments/assets/2505e9d0-ee67-4db7-9905-e8b223abd8a7)
![Screenshot 2024-10-14 190745](https://github.com/user-attachments/assets/525f33de-ce1b-4479-8375-0f6c7be5d10a)

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_13-39/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Screenshot 2024-10-14 191324](https://github.com/user-attachments/assets/81bec916-6475-4643-84ee-9de6de8fede3)

#### 8. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

![Screenshot 2024-10-14 202244](https://github.com/user-attachments/assets/6d5b3750-00b6-40f3-8edb-4ff8d41be65a)

Creating `pre_sta.conf` for STA analysis in `openlane` directory

![Screenshot 2024-10-14 210956](https://github.com/user-attachments/assets/e13b4da5-ca4b-423c-9a3f-65dc845c5f97)

Creating `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src` directory based on the file `openlane/scripts/base.sdc`

![Screenshot 2024-10-14 205839](https://github.com/user-attachments/assets/e37076a8-0c6f-4790-96be-5c76fe125022)

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

![Screenshot 2024-10-14 211217](https://github.com/user-attachments/assets/2c3f6c17-e734-4323-8036-2ab5bac881b9)
![Screenshot 2024-10-14 211242](https://github.com/user-attachments/assets/638d98d9-18af-46bf-a13a-cb0c56088740)

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis 

```tcl
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 09-10_13-39 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

![Screenshot 2024-10-14 211834](https://github.com/user-attachments/assets/83afd674-4d1a-4c45-abea-1e3252e5188f)
![Screenshot 2024-10-14 211934](https://github.com/user-attachments/assets/109125e8-20dd-4e8b-88e0-0c3dd7b638bb)

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

![Screenshot 2024-10-14 212008](https://github.com/user-attachments/assets/94543de2-be86-49f9-b950-3e34131a394b)
![Screenshot 2024-10-14 212022](https://github.com/user-attachments/assets/16aa2ffc-1864-4f37-b901-3910764c21e5)

#### 9. Make timing ECO fixes to remove all violations.

![Screenshot 2024-10-14 212128](https://github.com/user-attachments/assets/13e17679-5858-4914-a1c8-1f28cee29fa0)

Commands to perform analysis and optimize timing by replacing the cell or increasing the drive strength

```tcl
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

![Screenshot 2024-10-14 213633](https://github.com/user-attachments/assets/409616ca-2de1-4821-871c-99252bdadbb8)

Result - slack reduced

![Screenshot 2024-10-14 213648](https://github.com/user-attachments/assets/6f900aa4-9eb9-4f03-9469-3fcb2ce563bb)

![Screenshot 2024-10-14 214341](https://github.com/user-attachments/assets/c1f78d52-8ff9-4af4-8320-c276dfcff970)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or2_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 5
```
![Screenshot 2024-10-14 214710](https://github.com/user-attachments/assets/b50ed3a2-f659-4b6b-92e0-ba06f92bb611)

Result - slack reduced

![Screenshot 2024-10-14 214733](https://github.com/user-attachments/assets/3a801b24-8dfe-41fa-a1c5-ee5e24277820)

*We started ECO fixes at wns -23.89000 and now we stand at wns -22.23.28808 we reduced around 0.60192 ns of violation. Likewise we reduce the Violations and these violations must be waived in ECO fixes*

#### 10. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use `write_verilog` and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist

```bash
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-10_14-50/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

![Screenshot 2024-10-14 215834](https://github.com/user-attachments/assets/90ce3165-01c1-478a-bb2c-511cc6dc3893)

Commands to write verilog

```tcl
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-10_14-50/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

![Screenshot 2024-10-14 220017](https://github.com/user-attachments/assets/3a8ee55c-477c-4138-ba7b-fcbb17232c60)

We are taking the earlier 0 violation design which is clean for further stages

Commands load the design and run necessary stages

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 09-10_13-39 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# With placement done we are now ready to run CTS
run_cts
```

![Screenshot 2024-10-15 110139](https://github.com/user-attachments/assets/187ac161-33c2-4182-833a-381f56655ea3)
![Screenshot 2024-10-15 110235](https://github.com/user-attachments/assets/02fa60ce-3850-47d1-be50-f5c478fce33d)
![Screenshot 2024-10-15 110328](https://github.com/user-attachments/assets/20b77a5d-cc2a-49c0-a751-3de29c9a2c21)
![Screenshot 2024-10-15 110419](https://github.com/user-attachments/assets/f769148a-539c-42e7-a5ac-b5d721c2bed4)
![Screenshot 2024-10-15 110508](https://github.com/user-attachments/assets/a0fec5d3-8b38-4369-8502-b033678d6c49)
![Screenshot 2024-10-15 110558](https://github.com/user-attachments/assets/1bd050e9-70fe-4986-9489-ecd00194465e)
![Screenshot 2024-10-15 110620](https://github.com/user-attachments/assets/9bc79f8e-fec1-43fe-8821-121fa0a9ddce)

#### 11. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/09-10_13-39/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/09-10_13-39/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/09-10_13-39/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

![Screenshot 2024-10-15 113328](https://github.com/user-attachments/assets/bbd825fd-b58d-4948-a5b9-4c490fb71226)
![Screenshot 2024-10-15 113524](https://github.com/user-attachments/assets/117b5f9b-472d-4a06-9c49-6a2c9004e26b)
![Screenshot 2024-10-15 113556](https://github.com/user-attachments/assets/a00db51a-3dad-4a28-a406-c16965279f0d)
![Screenshot 2024-10-15 114021](https://github.com/user-attachments/assets/633245a6-fa61-4525-8345-090ea103c046)
![Screenshot 2024-10-15 114110](https://github.com/user-attachments/assets/8fa5c266-c532-4128-b694-15426ea82cb6)


#### 12. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing `CTS_CLK_BUFFER_LIST`

```tcl
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/09-10_13-39/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/09-10_13-39/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/09-10_13-39/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/09-10_13-39/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```

![Screenshot 2024-10-15 114528](https://github.com/user-attachments/assets/e7e468eb-c4ad-4c73-8e25-8e06b5be445b)
![Screenshot 2024-10-15 114700](https://github.com/user-attachments/assets/ae865324-f15f-4b6f-ad40-a1d88bf29de9)
![Screenshot 2024-10-15 114807](https://github.com/user-attachments/assets/4c39456c-6e29-429b-b2ca-d9c8825e1b0f)
![Screenshot 2024-10-15 114910](https://github.com/user-attachments/assets/2ca3fba1-69bc-4740-875d-0a5e505887b4)

## Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA

* Section 5 tasks:-
1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
2. Perfrom detailed routing using TritonRoute.
3. Post-Route parasitic extraction using SPEF extractor.

#### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

```
# Now that CTS is done we can do power distribution network
gen_pdn 
```

![Screenshot 2024-10-15 115038](https://github.com/user-attachments/assets/883826e5-d9dd-493f-a5f9-10267d378226)
![Screenshot 2024-10-15 115053](https://github.com/user-attachments/assets/1bd7d5fe-5c0a-45d7-850d-e2a8e1f4a729)

Commands to load PDN def in magic in another terminal

```bash
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_13-39/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 17-pdn.def &
```

![Screenshot 2024-10-15 115700](https://github.com/user-attachments/assets/5df63411-2b2b-45d1-bec1-f6e72a058f62)
![Screenshot 2024-10-15 115737](https://github.com/user-attachments/assets/682b3017-1189-4ef7-9990-b33dad79f96a)

#### 2. Perfrom detailed routing using TritonRoute and explore the routed layout.

Command to perform routing

```tcl
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```

![Screenshot 2024-10-15 115902](https://github.com/user-attachments/assets/45f941b4-5b69-474b-8ded-08d04ab1d7f6)
![Screenshot 2024-10-15 115931](https://github.com/user-attachments/assets/7b600b1a-443c-4cc8-afc1-c1f7bb6866e5)
![Screenshot 2024-10-15 120713](https://github.com/user-attachments/assets/75567dbb-7992-40b9-8f71-05ac7c7a1013)
![Screenshot 2024-10-15 120649](https://github.com/user-attachments/assets/97f435ee-22d6-4511-97e7-e0d94319f6ef)

Commands to load routed def in magic in another terminal

```bash
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_13-39/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

![Screenshot 2024-10-15 121042](https://github.com/user-attachments/assets/24e38f88-fdac-47d6-a973-c6c591a5143d)
![Screenshot 2024-10-15 121109](https://github.com/user-attachments/assets/e19e6bde-6fb0-452b-be9e-754db938ac37)
![Screenshot 2024-10-15 121206](https://github.com/user-attachments/assets/3f286e96-d456-4a0b-bcdd-b7b9b741d113)

#### 3. Post-Route parasitic extraction using SPEF extractor.

Commands for SPEF extraction using external tool

```bash
# Change directory
cd Desktop/work/tools/SPEF_EXTRACTOR

# Command extract spef
python3 /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_13-39/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/09-10_13-39/results/routing/picorv32a.def
```

![Screenshot 2024-10-15 122056](https://github.com/user-attachments/assets/c16e341b-b5ca-4634-ac90-80f469f495dd)

picorv32a.spef file,

![Screenshot 2024-10-15 122124](https://github.com/user-attachments/assets/357a9b3f-e4b4-451f-a614-3e0bc7141fb1)

![Screenshot 2024-10-15 122428](https://github.com/user-attachments/assets/7c881e14-8b79-4509-88e3-90b15325bd61)

*Upto now, we don't have the opensource tools to do the sign-off checks like LVS, DRC, ERC, ARC etc., we can do the checks using some commerical tools*

# Acknowledgements

* [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
* https://skywater-pdk.readthedocs.io/en/main/
* http://opencircuitdesign.com/magic/
