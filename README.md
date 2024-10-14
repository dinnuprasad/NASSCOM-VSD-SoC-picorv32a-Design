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

## Section 2: Good floorplan vs bad floorplan and introduction to library cells.

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
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

#### 1. Clone custom inverter standard cell design from github repository

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

Screenshot of commands run
