## Advanced Physical Design Using OpenLANE/Sky130

![Advanced-Physical-Design-using-OpenLANE_Sky130_1](https://user-images.githubusercontent.com/86367130/123914575-2b087700-d99d-11eb-819f-feb44e307b87.png)

# Table of Contents

1.[Inception of open-source EDA,Open LANE and Sky130 PDK](#first-day1)

 + How to Talk to Computers
     + [Introducion to QFN-48 Package, chips, pads, core, die and IPs](#first-content-1)
     + [Introduction to RISC-V](#first-content-2)
     + [From Sotware Applicatins to Hardware](#first-content-3)
 
 + SOC Design and Open LANE
     + [Simplified RTL2GDS Flow](#first-content-4)
     + [Introduction to Open LANE and Strive Chipsets](#first-content-5)
     + [Introduction to Open LANE Detailed ASIC Design Flow](#first-content-6)

+ Get famiier to Open Source EDA Tools
    + [Open LANE Directory Structure in Detail](#first-content-7)
    + [Design Preparation Step](#defines)
    + [Review Files After Design Prep and RUN Synthesis](#defines)
    + [Seps to characterize Synthesis Results](#defines)

2.[Good FloorPlan vs BadFloorPlan and introduction to library cells](#day2)

   + [Chip Floor Planning Considerations](#second-content-1)
   + [Library Binding and Placement](#second-content-2)
   + [Cell Design and Characterizaton Flows](#second-content-3)
   + [General Timing Charaterization Parameters](#second-content-4)

3.[Design Library Cell Using Magic Layout and ngspice characterization](#day3)

   + [Labs for CMOS inverter Ngspice simulations](#t-con-1)
   + [Inception of Layout CMOS Fabrication Process](#t-con-2)
   + [Sky130 Tech File Labs](#t-con-3)

# <a name="first-day1"></a>Inception of open-source EDA,Open LANE and Sky130 PDK

+ <a name="first-content-1"></a>Introducion to QFN-48 Package, chips, pads, core, die and IPs

QFN-48(Quad Flat No-Leads) Package:
     It consists of 48 pins with 7mm * 7mm size with the pins cnnected to the chip placed inside the package.
     Pads-Send the signals inside and outside the chip through pads.
     Chip- It is the place where all the digital Logic is fabricated with a combination of various gates.
     1.Foundary IPs(Intellectual Property)- Some amount intelligence techniques are used to build them
     2.Macros-A Pure Digita Logic
     
+ <a name="first-content-2"></a>RISC-V Architecture
      When an application or C-program is to be run on a Layout, it a first complied in its Assembly Language Program that is a RISC-V Assembly Language and then converted to Machine Language Program in terms of zeros and ones which is then run on a Layout.
      This RISC-V specification is implemented using a RTL Code for eg: picorv32 cpu core.

+ <a name="first-content-3"></a>From Software TO Hardware
![from software to hardware](https://user-images.githubusercontent.com/86367130/123951827-d546c580-d9c2-11eb-972e-4febab5c381f.PNG)

The instruction set produced by the compiler is the Instruction Set Architecture or Architecture of the computer. In this case it is RISC-V Architecture.HDL acts as an interface between RISC-V Architecture and hardware.

+ <a name="first-content-4"></a>Simplified RTL2GDS Flow
![image](https://user-images.githubusercontent.com/86367130/123952777-19869580-d9c4-11eb-8cbd-729b0e8aeeda.png)

Synthesis: Converts RTL design to a circuit out of the components from standard cell library(SCL).Standard cells have regular layout. each cell has different views/models.
For eg: Liberty View-This include power and delay models

Floor and Power Planning: Plan the silicon area and robust power distribution to power the circuits.
      Chip Floor Planning: Partition the chip die between different system building blocks and place the I\O Pads.
      Macro-Floor Planning:Dimensions, pin-locations,rows and routing tracks are defined.
      Power Planning: Power network is constucted where chip is power by multiple Vdd and Groung Pins.These pins ar connected to components through vertical and horizontal metal       straps.These use upper metal layers which are wider and have lower resistance 
      
Placement: Place the cells on the floor plan rows, aligned with the sites.Closser cells should be placed adjacent to reduce interconnect delay 
2 Steps:
Global Placement- Find optimal posiiton for all cellsand these are not legal so cells may overlap.
Detailed Placement-The detials obtained from Global Placement are minimally altered

Clock Distributon Network:
  -To deliver clock to all sequential circuis (eg;FFs)
  -With minimum skew-Can be ensured by H-Tree or X-tree
  
 Routing:Implement interconnect using metal layers.This requires to enquire a valid horizontal and vertical patterns to implement the nets. PDK defies the thickess, pitch, width of the metal layer and also the vias required to connect different metal layers.
 
 A Divide and Conquer Approach is used
    -Global Routing:Generated the routing guides
    -Detailed Routing:Used the guide to implement the actual wiring
    
 After Routing, Layout is confired through:
 Design Rule Checking(DRC)-Makes sure that the final Layout adheres to all Design Rules
 Layout Vs Schematic(LVS):Ensured that the final Layout matched the Gate-Level Netlist
 
 Timing Verification(STA): To ensure all timing constraints are met.
 
 + <a name="first-content-5"></a>Introduction to Open LANE and Strive Chipsets
 
 It is an open source tool
 
 Strive- Familyof open evrything SOCs
      Open EDA,Open PDK, Open RTL
      
 Open Lane -Creates a clean GDSII flow with no human intervention that is, there ar no DRC and LVS violations. Can be used to harden MACROS and chips.
 
 + <a name="first-content-6"></a>Introduction to Open LANE Detailed ASIC Design Flow

![image](https://user-images.githubusercontent.com/86367130/123957764-cc0d2700-d9c9-11eb-8afd-40089d6f0136.png)

Synthesis Exploration-Gives us the analysis of area and delay of the design.
Design Exploarion -Runs regression testing and comparision of different designs.
Design For Test:
    + Scan Insertion
    + Autmatic Test Pattern generation(ATPG)
    + Test Patterns Compaction
    + Fault Coverage
    + Fault Simulation
    
Physical Implementation- Is done through Open ROAD
Logic Equivalence Check -Done through yosys
Dealing with Antenna Rules Violations-Metal wires can act as an antenna so their size should be limited. This is ensured by the router.
    -Bridging- Attaches higher metal layer to intermediary.
    -Add antenna diodes to leak away the charges provided by SCL
    
Magic is used for DRC
Magic and Netgen ar used by LVS
    
+ <a name="first-content-7"> Open LANE Directory Structure in Detail
 
 Diretory Structure in 
 
 Invoke OpenLANE
 
 ![image](https://user-images.githubusercontent.com/86367130/123965735-5f4a5a80-d9d2-11eb-8c22-e8473462601d.png)

 interactive- Instead to running the completeflow at once it will run the process step by step
 
 Input all the Packges that are required to run the flow:
 
 ![image](https://user-images.githubusercontent.com/86367130/123966857-6a51ba80-d9d3-11eb-879e-097cd9978e8b.png)
 
 After Design Preparation, we will run the Synthesis:
 This will execute both yosys and abc pass will be done
 ![image](https://user-images.githubusercontent.com/86367130/124074001-759ef780-da60-11eb-9940-a4d7a7bd883d.png)
 
 Synthesis Successful-
 ![image](https://user-images.githubusercontent.com/86367130/124074193-bbf45680-da60-11eb-9fd2-e21536123299.png)
 
 Charaterize Synthesis Results:
 Number of D Flip-Flops=1613
 Number of Cells=14876
 Flop Count of the Design=1613/14876=0.1084 or 10.84 percent
 
 ![image](https://user-images.githubusercontent.com/86367130/124074488-2c02dc80-da61-11eb-9339-5a6d0688df99.png)
 
 


 
 


# <a name="day2"> Good FloorPlan vs BadFloorPlan and introduction to library cells
 
+ <a name="second-content-1"> Chip Floor Planning Considerations
 
 1. Utilization factor:
 
        If for example,the following logic is taken:
 
 ![Capture](https://user-images.githubusercontent.com/86367130/123980231-00d7a900-d9df-11eb-9b10-48618fefc413.PNG)
 
 If the area of the single unit is 1sq. unit,then if all the gatesare clubbed together then the total minumum area of the Netist will be 4sq. units.
 If the netlist occupies 100 percent area of the core then the Utilization Factor will be 100 percent.
 
 ![image](https://user-images.githubusercontent.com/86367130/123981298-dafed400-d9df-11eb-9331-59675e44d954.png)
 If Utilization Factor is 1, then the core is completly occupied and any extra cells cannot be accomodated in the core area.Ideally ,utilization factor should 0.5 or 0.6.
 
 
 2.Aspect Ratio:
     It is the ratio of height and width of the core.
 
 If the aspect ratio is 1 , then chip is square-shaped otherwise it is of rectangle shape.
 
 3.Define Location of Pre-Placed Cells:
      Pre-Placed Cells
 Logic is divided into blocks so that we can implement a Logic multiple times
 
  ![image](https://user-images.githubusercontent.com/86367130/123984897-a5a7b580-d9e2-11eb-95a5-9fdebc370912.png)
 
 4.DeCoupling Capacitors-Are attached in parallel of the gates so that timing vioalations can be avoided . They supply the current to the circuit when the design is decoupled and use the rest of the time to charge themselves to Vdd.
 
 5.Power Planning- To avoid the voltage drop of supply voltage across the circuit the power lines are placed across the circuit in terms of vertical and horizontal lines.
 
 6.Pin -Placement
 
+ Labs
 
 run_floorplan - command use to run the floorplan
 
 Width of the die=660685/1000=660.685 micron
 Heigth of the Die=671405/1000=671.405 micron
 Area=Width*Height=443587.212 square microns
 
 ![image](https://user-images.githubusercontent.com/86367130/124104313-c4a85500-da7f-11eb-84ed-c2b8bd31e66e.png)
 
 Review the FloorPlan in Magic
 
 ![image](https://user-images.githubusercontent.com/86367130/124109678-0be51480-da85-11eb-83c3-ff81362557c9.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124106660-0b974a00-da82-11eb-9591-04ab590a8af4.png)
 
 + <a name="second-content-2"> Libarary Binding and Placement
 
 Placement and Routing
 
 Step-1 Bind the Netlist with Physical Cells
 Every gate is square and rectangular shaped and the informtion about their dimensions, delay and the required conditions are present in a library. Every gate will be present in different dimensions in the library.
 
 Step-2- Place the Gates on the FloorPlan
 1. Gates are placed near to input -output pins.
 2. Buffers are added if the gates are far away from I/O pins which is charaterized by the timing information that is slew which depends on value of capacitance of the wire length .This is to ensure that the signal integrity is maintained.
 3. When the cells are abutted the delay is minimal
 
Labs for Placement:
 
Placement-Reduction of HPWL(Half Parameter Wire Length in Global Placement) and Overflow(If overflow value decreases the design will converge).
 
 ![image](https://user-images.githubusercontent.com/86367130/124114440-32f21500-da8a-11eb-8ee8-abb6d7c70374.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124114701-7fd5eb80-da8a-11eb-976a-43ca6b8bf98b.png)
 
 + <a name="second-content-3"> Cell Design and Characterization Flows
 
 Standard Cells- They are defined in the libraries and they have different versions of same gate with different sizes and threshold voltage.
 Larger the  threshold voltage more will be the time taken to switch.
 Same size gate can have different threshold voltage.
 
 Cell Design Flow is divided into 3 parts:
 1. Input-Input needed to design the cell
 Foundary provides PDKs,DRC and LVS rules, SPICE models,library and user-defined specifications.
 
 Some DRC Rules :
 Polywidth-2 Lambda
 Extensio Over ctive Area-3 Lambda
 Poly to Active Spacing-1 or 2 Lambda
 
 User-Defined Specs
 Cell Height: It is decided by the separation of power and ground rails.
 Cell Width: It depends on drive strength. If cell has drive strength of 1 it will be difficult to drive short whereas if they have drive strength of 10 they can drive more       longer wires.
 Supply Voltage- Top Level designer decides the supply voltage.
 Metal layers,Pin Locations and drawn gatelength
 
 2. Design the cell
    Design of the cell requires 3 different steps:
 
    1.Circuit Design-It is based on Spice Simulations. By knowing swithcing threshold value we can design our p-mos and n-mos gates and decide the value of W/L.
 
    2.Layout Design-Element the logic in the form p-mos and n-mos and get their respective Network Graphs.
     Euler's Path-Path which is traced only once.Based on the Euler's Path a Stick Diagram is drawn.
     Convert the Stick Diagram to the Layout adhering to the DRC rules given by the foundary.
     GDSII is the typical Layout file
     LEF-Defines width and height of the cell
     Extractes Spice Netlist (.cir)- Contains resistances and capacitances of each and every element of layout
     Extract the parasitics of the Layout and characterize it in terms of timing.
     
    3.Characteriztion Flow
 Steps for the Characterization
      1.Read the spice model files 
      2.Read the extracted SpiceNetlist
      3.Recognize the behavior of Buffer
      4.Read the sub-circuits of the Logic
      5.Attach the necessary Power Source
      6.Timing, Noise and Power Characterization 
      7.Apply the stimulus
      8.Necessary output capacitances
      9.Provide necessary simuation command
      10.Feed -in all the these configurations to the characterizatio software called GUNA which will generate timing, noise and Power models.
 
3.Output of the cell used by EDA tools
 We get outputs in the form of :
 CDL(Circuit Description language)-
 
 + <a name="second-content-4"> General Timing Characterization Parameters
 
 Timing Threshold Definitions
 
 1.slew_low_rise_thr : Threshold of the waveform  towards the lower side of the power supply rising from low to high .Typical values are from 20-30 percent.
 2.slew_high_rise_thr:Threshold of the waveform  towards the upper side of the power supply rising from low to high .Typical values are from 20-30 percent
 3.slew_low_fall_thr:  Lower threshold for falling waveform.
 4.slew_high_fall_thr: Upper threshold for falling waveform.
  
 These values are used to calculate slew of the waveform.
 
in_rise_thr,in_fall_thr:Related to the input waveforms.50 percent point are taken
 
 out_rise_th,out_fall_thr:related to output waveform. 50 percent are taken
 
Propagation Delay- Take the difference of the 50 percent to calculate the delay.
 Negative delay can be observed if we shift the threshold values.
 And if the distance between 2 units is high it might  result in higher slew resulting in negative delay.
 
 Transition Time:
 
 For the slew rising waveform substarct the low from high timing threshold. 
 For the slew falling waveform substarct the high from low timing threshold. 
 
 
 
 
   
 


 
 
 
 


 
 
    

 
    




 
 
 
    
























































































