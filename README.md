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

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```

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
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

```

Screenshots of running each commands
