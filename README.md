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

![Screenshot 2024-10-04 195649](https://github.com/user-attachments/assets/bf6f4eb3-633d-4304-b929-9fbe15a30fce)
![Screenshot 2024-10-04 195732](https://github.com/user-attachments/assets/27a9d942-38ad-4bfa-af8e-210bf7c85ad2)

To Calculare the Flop Ratio:-
```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF = Flop\ Ratio * 100
```

Calculation of Flop Ratio of DFF from synthesis statistics report file,
```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF = 0.108429685 * 100 = 10.84296854\ \%
```

## Section 2: Good floorplan vs bad floorplan and introduction to library cells.

Section 2 tasks:- 
1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

## <a id="section1"></a>Section 1: Inception of open_source EDA, OpenLANE and SKY130 PDK
...
## <a id="section2"></a>Section 2: Good floorplan vs bad floorplan and introduction to library cells
...
## <a id="section3"></a>Section 3: Design library cell using Magic Layout and ngspice characterization
...
## <a id="section4"></a>Section 4: Pre-layout timing analysis and importance of good clock tree
...
## <a id="section5"></a>Section 5: Final steps for RTL2GDS using tritonRoute and OpenSTA
