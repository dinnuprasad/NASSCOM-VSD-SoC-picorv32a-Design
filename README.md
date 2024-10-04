# NASSCOM-VSD-SoC-picorv32a-Design
# **Contents**
	1. Sky130 Day 1 - Inception of open-source EDA, OpenLANE and SKY130 PDK
		a. SKY130_D1_SK1 - How to talk to computers
			i. SKY_L1 - Introduction  to QFN-48 Package, chip, pads, core, die and IPs
			ii. SKY_L2 - Introduction to RISC-V
			iii. SKY_L3 - From Software Applications to Hardware
		b. SKY130_D1_SK2 - SoC design and OpenLANE
			i. SKY_L1 - Introduction to all components of open-source digital asic design
			ii. SKY_L2 - Simplified RTL2GDS flow
			iii. SKY_L3 - Introduction to OpenLANE and Strive chipsets
			iv. SKY_L4 - Introduction to OpenLANE detailed ASIC design flow
		c. SKY130_D1_SK3 - Get familiar to open-source EDA tools
			i. SKY_L1 - OpenLANE Directory structure in detail
			ii. SKY_L2 - Design Preparation Step
			iii. SKY_L3 - Review files after design prep and run synthesis
			iv. SKY_L4 - OpenLANE Project Git Link Description
			v. SKY_L5 - Steps to characterize synthesis results
	2. Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells
		a. SKY130_D2_SK1 - Chip Floor planning considerations
			i. SKY_L1 - Utilization factor and aspect ratio
			ii. SKY_L2 - Concept of pre-placed cells
			iii. SKY_L3 - De-coupling capacitors
			iv. SKY_L4 - Powerplanning
			v. SKY_L5 - Pin placement and logical cell placement blockage
			vi. SKY_L6 - Steps to run floorplan using OpenLANE
			vii. SKY_L7 - Review floorplan files and step to view floorplan
			viii. SKY_L8 - Review floorplan layout in Magic
		b. SKY130_D2_SK2 - Library Binding and Placement
			i. SKY_L1 - Netlist binding and initial place design
			ii. SKY_L2 - Optimize placement using estimated wire-length and capacitance
			iii. SKY_L3 - Final placement optimization
			iv. SKY_L4 - Need for libraries and characterization 
			v. SKY_L5 - Congestion aware placement using RePlAce
		c. SKY130_D2_SK3 - Cell design and characterization flows
			i. SKY_L1 - Inputs for cell design flow
			ii. SKY_L2 - Circuit design step
			iii. SKY_L3 - Layout design step
			iv. SKY_L4 - Typical characterization flow
		d. SKY130_D2_SK4 - General timing characterization parameters
			i. SKY_L1 - Timing threshold definitions
			ii. SKY_L2 - Propagation delay and transition time
	3. Sky130 Day 3 - Design library cell using Magic Layout and ngspice characterization
		a. SKY130_D3_SK1 - Labs for CMOS inverter ngspice simulations
			i. SKY_L0 - IO placer revision
			ii. SKY_L1 - SPICE deck creation for CMOS inverter
			iii. SKY_L2 - SPICE simulation lab for CMOS inverter
			iv. SKY_L3 - Switching Threshold Vm
			v. SKY_L4 - Static and dynamic simulation of CMOS inverter
			vi. SKY_L5 - Lab steps to git clone vsdstdcelldesign
		b. SKY130_D3_SK2 - Inception of Layout and CMOS fabrication process
			i. SKY_L1 - Create Active regions
			ii. SKY_L2 - Formation of N-well and P-well
			iii. SKY_L3 - Formation of gate terminal
			iv. SKY_L4 - Lightly doped drain (LDD) formation
			v. SKY_L5 - Source and drain formation
			vi. SKY_L6 - Local interconnect formation
			vii. SKY_L7 - Higher level metal formation
			viii. SKY_L8 - Lab introduction to Sky130 basic layers layout and LEF using inverter
			ix. SKY_L9 - Lab steps to create std cell layout and extract spice netlist
		c. SKY130_D3_SK3 - Sky130 Tech File Labs
			i. SKY_L1 - Lab steps to create final SPICE deck using Sky130 tech
			ii. SKY_L2 - Lab steps to characterize inverter using sky130 model files
			iii. SKY_L3 - Lab introduction to Magic tool options and DRC rules
			iv. SKY_L4 - Lab introduction to Sky130 pdk's and steps to download labs
			v. SKY_L5 - Lab introduction to Magic and steps to load Sky130 tech-rules
			vi. SKY_L6 - Lab exercise to fix poly.9 error in Sky130 tech-file
			vii. SKY_L7 - Lab exercise to implement poly resistor spacing to diff and tap
			viii. SKY_L8 - Lab challenge exercise to describe DRC error as geometrical construct
			ix. SKY_L9 - Lab challenge to find missing or incorrect rules and fix them
	4. Sky130 Day 4 - Pre-layout timing analysis and importance of good clock tree
		a. SKY130_D4_SK1 - Timing modelling using delay tables
			i. SKY_L1 - Lab steps to convert grid info to track info
			ii. SKY_L2 - Lab steps to convert magic layout to std cell LEF
			iii. SKY_L3 - Introduction  to timing libs and steps to include new cell in synthesis
			iv. SKY_L4 - Introduction to delay tables
			v. SKY_L5 - Delay table usage Part 1
			vi. SKY_L6 - Delay table usage Part 2
			vii. SKY_L7 - Lab steps to configure synthesis settings to fix slack and include vsdinv
		b. SKY130_D4_SK2 - Timing analysis with ideal clocks using OpenSTA
			i. SKY_L1 - Setup timing analysis and introduction to flip-flop setup time
			ii. SKY_L2 - Introduction to clock jitter and uncertainty
			iii. SKY_L3 - Lab steps to configure OpenSTA for post-synth timing analysis
			iv. SKY_L4 - Lab steps to optimize synthesis to reduce setup violations
			v. SKY_L5 - Lab steps to do basic timing ECO
		c. SKY130_D4_SK3 - Clock tree synthesis TritonCTS and signal integrity
			i. SKY_L1 - Clock tree routing and buffering using H-Tree algorithm
			ii. SKY_L2 - Crosstalk and clock net shielding
			iii. SKY_L3 - Lab steps to run CTS using TritonCTS
			iv. SKY_L4 - Lab steps to verify CTS runs
		d. SKY130_D4_SK4 - Timing analysis with real clocks using OpenSTA
			i. SKY_L1 - Setup timing analysis using real clocks
			ii. SKY_L2 - Hold timing analysis using real clocks
			iii. SKY_L3 - Lab steps to analyze timing with real clocks using OpenSTA 
			iv. SKY_L4 - Lab steps to execute OpenSTA with right timing libraries and CTS assignment
			v. SKY_L5 - Lab steps to observe impact of bigger CTS buffers on setup and hold timing
	5. Sky130 Day 5 - Final steps for RTL2GDS using tritonRoute and OpenSTA 
		a. SKY130_D5_SK1 - Routing and design rule check (DRC)
			i. SKY_L1 - Introduction  to Maze Routing and Lee's algorithm
			ii. SKY_L2 - Lee's Algorithm conclusion
			iii. SKY_L3 - Design Rule Check
		b. SKY130_D5_SK2 - Power Distribution Network and routing
			i. SKY_L1 - Lab steps to build power distribution network
			ii. SKY_L2 - Lab steps from power straps to std cell power
			iii. SKY_L3 - Basics of global and detail routing and configure TritonRoute
		c. SKY130_D5_SK3 - TritonRoute Features
			i. SKY_L1 - TritonRoute feature 1 - Honors pre-processed route guides
			ii. SKY_L2 - TritonRoute feature 2&3 - Inter-guide connectivity and intra-layer & inter-layer routing
			iii. SKY_L3 - TritonRoute method to handle connectivity
			iv. SKY_L4 - Routing topology algorithm and final files list post-route

   
