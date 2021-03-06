## Advanced Physical Design Using OpenLANE/Sky130

![Advanced-Physical-Design-using-OpenLANE_Sky130_1](https://user-images.githubusercontent.com/86367130/123914575-2b087700-d99d-11eb-819f-feb44e307b87.png)

# Table of Contents

1.[Inception of open-source EDA,Open LANE and Sky130 PDK](#first-day1)

 + [How to Talk to Computers](#first-content-1)
 + [SOC Design and Open LANE](#first-content-4)
    
2.[Good FloorPlan vs BadFloorPlan and introduction to library cells](#day2)

   + [Chip Floor Planning Considerations](#second-content-1)
   + [Library Binding and Placement](#second-content-2)
   + [Cell Design and Characterizaton Flows](#second-content-3)
   + [General Timing Charaterization Parameters](#second-content-4)

3.[Design Library Cell Using Magic Layout and ngSpice characterization](#day3)

   + [Labs for CMOS inverter Ngspice simulations](#t-con-1)
   + [Inception of Layout CMOS Fabrication Process](#t-con-2)
   + [Sky130 Tech File Labs](#t-con-3)

4.[Prelayout timing analysis and importance of good clock tree](#day4)

   + [Timing Modeling using delay tables](#f-con-1)
   + [Timing analysis with ideal clocks using open STA](#f-con-2)
   + [Clock tree synthesis TritonCTS and signal integrity](#f-con-3)
   + [Timing analysis with real clocks using open STA](#f-con-4)

5.[Final Steps From RTL2GDS using tritonRoute and open STA](#day5)

   + [Routing and Design Rule Check(DRC)](#fi-con-1)
   + [Power distribution network and routing](#fi-con-2)
   + [TritonRoute Features](#fi-con-3)

# <a name="first-day1"></a>Inception of open-source EDA,Open LANE and Sky130 PDK

+ **<a name="first-content-1"></a>Introducion to QFN-48 Package, chips, pads, core, die and IPs**

**QFN-48(Quad Flat No-Leads) Package**:
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
 
 Yosys-Perform RTL Simulation
 abc-It performs technology mapping and the netlist is created.
 Open STA-Perform static timing analysis after synthesis.
 
 This will execute both yosys and abc pass will be done
 ![image](https://user-images.githubusercontent.com/86367130/124074001-759ef780-da60-11eb-9940-a4d7a7bd883d.png)
 
 Synthesis Successful-
 ![image](https://user-images.githubusercontent.com/86367130/124074193-bbf45680-da60-11eb-9fd2-e21536123299.png)
 
 Characterize Synthesis Results:
 Number of D Flip-Flops=1613
 Number of Cells=14876
 Flop Count of the Design=1613/14876=0.1084 or 10.84 percent
 
 ![image](https://user-images.githubusercontent.com/86367130/124074488-2c02dc80-da61-11eb-9339-5a6d0688df99.png)
 
 **Cell LEF**- It holds information about the metal layer information, PNR boundary and pin positions.
 **Technology LEF** - Holds information DRC rules,vias and metal layers used in placement and routing
 
 
 
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
 
+ **Labs**
 
 run_floorplan - command use to run the floorplan
 
 Width of the die=660685/1000=660.685 micron
 Heigth of the Die=671405/1000=671.405 micron
 Area=Width*Height=443587.212 square microns
 
 ![image](https://user-images.githubusercontent.com/86367130/124104313-c4a85500-da7f-11eb-84ed-c2b8bd31e66e.png)
 
 Review the FloorPlan in Magic
 
 ![image](https://user-images.githubusercontent.com/86367130/124109678-0be51480-da85-11eb-83c3-ff81362557c9.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124106660-0b974a00-da82-11eb-9591-04ab590a8af4.png)
 
 Changed the Placement of Pins to 2 :
 
 ![image](https://user-images.githubusercontent.com/86367130/124194996-29e36100-dae7-11eb-9ba2-e38e36931582.png)

 
 
 + <a name="second-content-2"> Libarary Binding and Placement
 
 Placement and Routing
 
 Step-1 Bind the Netlist with Physical Cells
 Every gate is square and rectangular shaped and the informtion about their dimensions, delay and the required conditions are present in a library. Every gate will be present in different dimensions in the library.
 
 Step-2- Place the Gates on the FloorPlan
 1. Gates are placed near to input -output pins.
 2. Buffers are added if the gates are far away from I/O pins which is charaterized by the timing information that is slew which depends on value of capacitance of the wire length .This is to ensure that the signal integrity is maintained.
 3. When the cells are abutted the delay is minimal
 
**Labs for Placement**:
 
Placement-Reduction of HPWL(Half Parameter Wire Length in Global Placement) and Overflow(If overflow value decreases the design will converge).
 
 ![image](https://user-images.githubusercontent.com/86367130/124114440-32f21500-da8a-11eb-8ee8-abb6d7c70374.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124114701-7fd5eb80-da8a-11eb-976a-43ca6b8bf98b.png)
 
 + <a name="second-content-3"> Cell Design and Characterization Flows
 
 Standard Cells- They are defined in the libraries and they have different versions of same gate with different sizes and threshold voltage.
 Larger the  threshold voltage more will be the time taken to switch.
 Same size gate can have different threshold voltage.
 
 Cell Design Flow is divided into 3 parts:
 1. **Input-Input needed to design the cell**
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
 
 2. **Design the cell**
    Design of the cell requires 3 different steps:
 
    1.Circuit Design-It is based on Spice Simulations. By knowing swithcing threshold value we can design our p-mos and n-mos gates and decide the value of W/L.
 
    2.Layout Design-Element the logic in the form p-mos and n-mos and get their respective Network Graphs.
     Euler's Path-Path which is traced only once.Based on the Euler's Path a Stick Diagram is drawn.
     Convert the Stick Diagram to the Layout adhering to the DRC rules given by the foundary.
     GDSII is the typical Layout file
     LEF-Defines width and height of the cell
     Extractes Spice Netlist (.cir)- Contains resistances and capacitances of each and every element of layout
     Extract the parasitics of the Layout and characterize it in terms of timing.
     
    3.**Characteriztion Flow**
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
 
3.**Output of the cell used by EDA tools**
 We get outputs in the form of :
 CDL(Circuit Description language),GDSII,LEF(Library Exchange Format),Spice Extracted Netlist,timing,noise and power libs.
 
 + <a name="second-content-4"> General Timing Characterization Parameters
 
 **Timing Threshold Definitions**
 
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
 
 **Transition Time**:
 
 For the slew rising waveform substarct the low from high timing threshold. 
 For the slew falling waveform substarct the high from low timing threshold. 
 
# <a name="day3"> Design Library Cell Using Magic Layout and ngSpice characterization
 
 + **<a name="t-con-1"> Labs for CMOS inverter Ngspice simulation**
 
 VTC- Spice Simulations:
     1. Create A Spice Deck- Connecticity information of the Netlist, input and ouput information.
     2. Define W/L value of P-Mos and N-mos and Load capacitance, input gate voltage and Vss
     3. Identify the nodes and name them.
      
   Writing Spice Deck:
  ![image](https://user-images.githubusercontent.com/86367130/124197423-1c7ca580-daec-11eb-9a99-ee77e94979e6.png)

 Simulation Commands:
           
 ![image](https://user-images.githubusercontent.com/86367130/124197473-38804700-daec-11eb-96a4-16343a4d491e.png)
  Model Files:
 ![image](https://user-images.githubusercontent.com/86367130/124197555-65ccf500-daec-11eb-8853-d5ca45107c6e.png)
 When the W/L ratio of PMOS is greater the graph , the transfer characteristic shift to the right.
 
 ![image](https://user-images.githubusercontent.com/86367130/124201830-a893ca80-daf6-11eb-9760-e069eaffe2e8.png)

 Switching Threshold: Point at which device switches
           
 Layout Design on Inverter-
 ![image](https://user-images.githubusercontent.com/86367130/124205185-5b1b5b80-dafe-11eb-9b89-5387fc6cdcc8.png)



     
 + **<a name="t-con-2"> Inception of Layout CMOS Fabrication Process**
 
 **16-Mask CMOS Process**
 
    1.Selecting a Substrate:
           p-type substarte- Has high resistivity (5-50 ohms), Doping Level(10^15 per cm3) and orientation(100)
 
    2.Create an active-region for Transistors
 
  ![image](https://user-images.githubusercontent.com/86367130/124148898-bec96880-daad-11eb-9ffc-e6ab6579b274.png)
 
           Create 40nm layer of Sio2 and 80nm layer of Si3N4 and 1um layer of photoresist.
           The areas covered by **mask-1** are protected and rest are exposed to the chemical reactions and extra photoresist is washed away.
           Remove the mask.
           The exposed areas which are not under the photoresist are exposed to etching. So, silicon nitride is etched off.
           Remove the photoresist
           Place the setup in the oxidation furnace and silicon nitride protects some areas for extra oxidation.This process is called as **LOCOS** (Local Oxidation of        Silicon) and Bird's beak are formed.
 
This isolates the two transistors.
 
  ![image](https://user-images.githubusercontent.com/86367130/124151217-efaa9d00-daaf-11eb-9cfa-b21be0cdf5eb.png)
          
          Now, silicon nitride is stripped out using hot phosphoric acid

      
     3.Create N-Well and P-Well Formation:
               
                      A layer of photoresist is deposited.
                      Mask is placed above it and the setup is exposed to UV Light and area is washed away which is not exposed to UV Light.Next remove the mask and make P-Well
                      with Boron and diffused into substarte with ion Implantation at 200keV.
 
   ![image](https://user-images.githubusercontent.com/86367130/124166885-06a5bb00-dac1-11eb-8d76-04e3f6dc2425.png)
 
 Similary a **N-Well** is formed with **Phosphorus** with ion-plantation.Energy required is high for Phosphorus implantation as they are heavier that is around 400keV.
 
![image](https://user-images.githubusercontent.com/86367130/124168064-43be7d00-dac2-11eb-9c24-05517917ad1e.png)
 
 Next step is to take the structure into driving furnace at high temperature about 1100 degrees centigrades about 4-6 hrs to drive-in the atoms to form clear wells.This process is called **Twin-Tub** process.
 
     4.Formation of Gate
           
              Threshold voltage depend on body effect coefficient which in turn depends on Doping conc. and oxide capacitance. These are controlled in fabrication steps to    maintain a particular threshold voltage.
  **Mask-4**- Is used and boron is implanted with low energy of **60keV** just at the surface to attain required doping concentration with depends on threshold voltage.
   Similarly, **Mask-5** is used As or P on the N-well with low energy just to maintain enough doping level.
 
 ![image](https://user-images.githubusercontent.com/86367130/124171432-33100600-dac6-11eb-9571-0a3225776e2f.png)

               The HF can be used to remove the oxide layer and a high quality oxide can re-grown of 10nm thick depending upon the threshold voltage.
 A Polysilicon layer is deposited of about 0.4um thick and in order to make gate area of less resistance more impurities are doped into of any N-type impurity.
 **Mask-6**- It is used to draw the gates and reamining areas of Polysilicon can be etched away.
 
      5.Lightly-Doped Drain(LDD) formation-
 
               The doping profile for P-Mos id P+,P- and N and for N-Mos is N+,N- and P.This profile is used for 2 reasons:
 **Hot electron effect** and ** Short Channel Effect**
     **Hot electron effect**- When the device size reduces and power supply is not redesigned and so the Electric Field (E=V/d) increases and as a result elecron attain large energy which break the Si-Si bonds leading to more e and holes disturbing the doping profile.
     And the energy may be too high which crosses electron barrier of 3.2 eV barrier between Si conduction band Sio2 band.
     **Short Channel Effect**-When we change gate length from 1um to 0.5 um, the drain penetrates into channel area and thus it becomes difficult to control the current by gate area.
 
 ![image](https://user-images.githubusercontent.com/86367130/124175223-f7c40600-daca-11eb-8dad-ebd85987852b.png)

 
     **Mask-7**- Is used to implant the N- dopant into the P-well.
     **Mask-8**- Is used to implant the P- dopant into the N-Well
  Side-Wall Spacers- Are formed by depositing thick Silicon Nitride or Silicon oxide Layer and is etched out by Anisotropic Plasma Etching.These spacers prevent the N-or P- implants being disturbed by the P+ or N+ doping implants.
 
      6. Source and Drain Formation-
 
                Screen oxide is added to prevent channeling during implants.
                Mask-9- The structure is exposed with As with 75keV and side-wall spacers keep the LDD intact.
                Mask-10-P+ implant with B as 50keV.And keep the structure in high temperature furnace at 1000 deg celcius that is annealing.
 
 ![image](https://user-images.githubusercontent.com/86367130/124176166-2c848d00-dacc-11eb-8e2f-6e89a89a8694.png)

      7. Steps to form contacts and interconnects(local)
                Etch the screen oxide used earlier using HF
                Deposit Titanium(very low resistivity) on wafer surface using sputtering. The titanium is hit Argon gas and the particles get deposited on the substrate.Then                   create a contact by heating it at about 650-700 deg celcius in N2 ambient for 60 sec resulting in low resistance TiSi2 and TiN deposited on top of it for local communication.
 
 ![image](https://user-images.githubusercontent.com/86367130/124178225-f3015100-dace-11eb-80d9-13be4e4fc107.png)

      Mask-11 : Is applied and extra TiN is etched with RCA cleaning.
      
      8. Higher Level Metal Formation:
                Planarize the surface with a thick layer of SiO2 doped with P(phophosilicate glass) deposited on wafer surface.It helps to reduce the temperature and CMP(Chemical mechanical Polishing) is done for planarizing the surface.Now, by **Mask-12** we etch out sio2 for contact holes. Then remove the photoresist and deposit a thin 10nm layer of TiN which acts as adhesion layer and followed by Blanket Tungsten Layer and with CMP the structure is planarized.
 
  ![image](https://user-images.githubusercontent.com/86367130/124179392-6f486400-dad0-11eb-881d-59116e731e23.png)

                Now Al Layer is deposited and it is patterned with Mask-13 and rest Al is plasma etched out.
                Next higher level metal is formed by depositing Sio2 and CMP done and Mask-14 is used to pattern the same. Next again TiN layer is deposited and W thereafter and Mask-15 is used to make the third level of interconnect.
 
 ![image](https://user-images.githubusercontent.com/86367130/124179742-de25bd00-dad0-11eb-8c57-385d8f340d08.png)
                
                The higher level interconnect is thicker than the lower level.Silicon Nitride is used to protect the chip and Mask-16 is used to open contact holes on this layer.
 
 ![image](https://user-images.githubusercontent.com/86367130/124180060-483e6200-dad1-11eb-8635-5899af6b847c.png)
 
 **Spice Simulation**
 
 Extract Spice netlist
 
 ![ext2spice2](https://user-images.githubusercontent.com/86367130/124253271-7bbad400-db45-11eb-9416-41925b263b58.PNG)

 Editing Spice Deck:
 
 ![sky130inv](https://user-images.githubusercontent.com/86367130/124253382-9725df00-db45-11eb-8e61-9adfb24604b3.PNG)
 
![image](https://user-images.githubusercontent.com/86367130/124293654-817bde00-db74-11eb-88c4-4bc607c22c74.png)

 Plot y vs time a
 
 ![plot y vs time](https://user-images.githubusercontent.com/86367130/124253476-af95f980-db45-11eb-968a-445ccb9340ca.PNG)

 
 When Load Capacitance is = 0.2fF, Waveform is as shown below
 ![image](https://user-images.githubusercontent.com/86367130/124249402-76f42100-db41-11eb-899c-f40a5bb67b7b.png)

 Charaterizing the Cell
 
 1. Rise Transition-Time taken by output waveform to rise from 20-80 percent of Vdd =0.064ns
 2. Fall Transition-Time taken by output waveform to fall from 80-20 percent of vdd=27ps
  ![image](https://user-images.githubusercontent.com/86367130/124385756-ad25d200-dcf4-11eb-9074-a102d569f37b.png)
 
 3.Cell Rise Propagation Delay(propagation delay when  the output of the cell is rising)=0.061ns
 4. Cell Fall Delay(Propagation Delay when output if fallng)=40ps
 
 **Convert grid info to track info**
 
 Some rules for standard cell design
 
 1.Input and output port must be on the intersection of vertical and horizontal lines
 
 2.Width of the standard should be in odd multiples of horizontal track pitch
 
 3.Height of the standard should be in multiples of vertical track pitch
 
 grid <x-spacing> <y-spacing> <x-origin> <y-origin>
 
 The dimension of the grid changed according to the track file data
 and input and output ports are found at the intersection of tracks.
 
 ![image](https://user-images.githubusercontent.com/86367130/124298000-5fd12580-db79-11eb-8d1a-82d97b48e94d.png)
 
 Width of the standard cell should be in odd multiples of x-pitch
 Here width is about 3 times the x-pitch and the height is about 9 times the y-pitch
 
 ![image](https://user-images.githubusercontent.com/86367130/124308077-5a2e0c80-db86-11eb-8d1c-c9fd5c456993.png)
 
 When we extract lef file the ports are referred to as pins in the MACRO
 
 ![image](https://user-images.githubusercontent.com/86367130/124309056-ccebb780-db87-11eb-9853-4d8804139cf4.png)
 
 Now ports are identified as input-output ports and are defined by port class and port use.
 ![image](https://user-images.githubusercontent.com/86367130/124310014-3a4c1800-db89-11eb-8c40-4dd9602d2a1b.png)

 So, for output port Port class output and port use signal
 For VPWR port class input and port use power
 For VGND port class input and port use power
 
 Saving the cell with a custom name:
  
 ![image](https://user-images.githubusercontent.com/86367130/124310693-31a81180-db8a-11eb-89b4-e61c146e50a7.png)
A mag file is created with the same name 
 
![image](https://user-images.githubusercontent.com/86367130/124310808-5d2afc00-db8a-11eb-8b76-918b119203c1.png)
 
 Now lef file is extracted by the following command :
 
 ![image](https://user-images.githubusercontent.com/86367130/124311120-dde9f800-db8a-11eb-9128-d71f2e8b5a75.png)
A lef file is create hence,
 ![image](https://user-images.githubusercontent.com/86367130/124311191-feb24d80-db8a-11eb-8d46-d4b02772ba03.png)
So, ports are converted into pins
![image](https://user-images.githubusercontent.com/86367130/124311334-328d7300-db8b-11eb-8188-23e4d6dea3de.png)
 
 Now in order to integrate the cell with picorv32a ,copy the lef file and libraries into the src folder and make changes to config.tcl file
 
 ![image](https://user-images.githubusercontent.com/86367130/124351653-3bc52100-dc19-11eb-8637-e4a7dea53ec5.png)

 And the cell has been integrated in the synthesis:
 
 ![image](https://user-images.githubusercontent.com/86367130/124351620-0c161900-dc19-11eb-9dca-d6f569733195.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124385315-a4340100-dcf2-11eb-86d1-8d9fff7db303.png)
 
# <a name="day4"> Prelayout Timing Analysis and importance of good clock tree
 
 + **<a name="f-con-1">Timing modelling using delay tables**
 
 **Clock gating**
 And and OR gates Logic are used with the clock to stop the clock and save static and dynamic power.
 
 ![image](https://user-images.githubusercontent.com/86367130/124324026-e13bae80-db9f-11eb-9c19-6bff07b747a0.png)
 
 Delay Tables:
 
 Its a 2-D table with timing analysis when varying input slew and output capacitances.
 
 Every component has a delay table which are timing models for that gate.
 
 + **<a name="f-con-2"> Timing Analysis using Ideal Clocks Using Open STA**
 
 STA -Labs
 
  
 READ.me file with the strategy
 
 ![image](https://user-images.githubusercontent.com/86367130/124352629-10453500-dc1f-11eb-8d09-28e8affc664d.png)
Synthesis run has tns=-711.59ns
 wns=-23.89ns
So we can reduce the slack by increasing the including the area and introducing buffers in the synthesis,increase the fanout and increase the size of cell which is creating a slack.
 ![image](https://user-images.githubusercontent.com/86367130/124354855-cca4f800-dc2b-11eb-815f-99fc26c1df58.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124354909-20174600-dc2c-11eb-8ce8-feb9a87bd3e5.png)

 My_base
 ![image](https://user-images.githubusercontent.com/86367130/124355281-e21b2180-dc2d-11eb-8daa-6ed58336e9ab.png)
 
 Then sta sta.conf to run static timing analysis
 
 Introducing bufferes,upsizing the cell and increasing the Fanout has reduced the delay

 Now replacing the synthesis file with modified one
 
 Command in STA:
            write verilog <location of synthesis file>/<name of the file>
 
 By increasing the size of the buffers increased and more iterations were used placement.
 
 + **<a name="f-con-3"> Clock Tree Synthesis TriTonCTS and Signal Integrity**
 
 **Clock Tree Synthesis**
             Skew-Time difference to reach the clock to different flip-flops.It should be close to zero.
 
![image](https://user-images.githubusercontent.com/86367130/124363379-831ed200-dc58-11eb-8bdd-f2c7ee4b05c9.png)
 
                  So, **H-Tree** -Calculates the distance of the clock to all flip-flops and comes at the mid-pont of it from where the tree is built and looks H-shaped.
 Here skew will be close to zero.
   
  ![image](https://user-images.githubusercontent.com/86367130/124363488-22dc6000-dc59-11eb-8b0e-bffd02a8726e.png)
 
 **Clock Tree Buffering:**
 
Due to transition of clock across the wire the signal might loose its integrity because of resistances and capacitances across the wire.
             So, the solution is clock buffers.They have equal rise and fall time.
 
 Here red coloured bufferes are clock buffers
 
 ![image](https://user-images.githubusercontent.com/86367130/124363629-0856b680-dc5a-11eb-8c7e-c96b0a0ee0fd.png)
 
 **Clock Net Shielding and Crosstalk**
 
 Crosstalk can occur on the lines.So the lines are shielded from the signal adjacent wires or from outside.
 
 Crosstalk can result glitch and delta delay.
                Glitch-Coupling capacitance can result in droop in the signal in another circuit which if inverted can result in high logic resulting in loss of signal integrity.For eg: A memory can go to reset if reset goes to high.The shirlded wired are connected to Vdd or ground.The shields dont switch and their is less possibility of glitch.
  ![image](https://user-images.githubusercontent.com/86367130/124363813-37215c80-dc5b-11eb-94e2-65f8104f9a56.png)
 
 Delta Delay: 
               If the victim and the aggressor both are switching. The delay of the victim gets impacted by the total delay gets increased by delta delay which results in skew.
 
 ![image](https://user-images.githubusercontent.com/86367130/124364018-5bca0400-dc5c-11eb-95c0-bbf80a885d98.png)



 
 Parameters considered in CTS:
 
![image](https://user-images.githubusercontent.com/86367130/124360487-be64d500-dc47-11eb-9e45-d8d5d770a1f4.png)
 
 Running Clock-Tree-Synthesis:
 
 In Clock Buffers get added which modifies the Netlist and a new file picorv32a.synthesis_cts.v is created
 
            run_cts
 
![image](https://user-images.githubusercontent.com/86367130/124374898-7fbd3200-dcbc-11eb-9376-7f40b74e1fb6.png)

 ![image](https://user-images.githubusercontent.com/86367130/124374986-20abed00-dcbd-11eb-936b-e4b7f2f13133.png)
![image](https://user-images.githubusercontent.com/86367130/124375098-e98a0b80-dcbd-11eb-82ed-7cf3efaffe5b.png)

 
 + **<a name="f-con-4">Timing Analysis Using Real Clocks Using Open STA**
 
 In real clock system , the clock will reach the flip-flop through a set of buffers and wires.
 
 ![image](https://user-images.githubusercontent.com/86367130/124364403-f4618380-dc5e-11eb-8c66-093101e3bebf.png)

 
                  The RHS is data required time and LHS is Data arrival Time.Data required time should be greater than data arrival time, otherwise it is a negative slack.
 Slack should be either positive zero.
 
![image](https://user-images.githubusercontent.com/86367130/124364275-07278880-dc5e-11eb-85fc-342ffb226791.png)
 
 Delta1 and Delta2 - Clock network delay of launch and capture flop.

Skew is Delta1 - Delta2
             The circuit  violating the timing constraints will have a slack.
 
 **Hold-Timing Analysis** Single Clock
 
 Hold time refers to the second mux delay in the flip-flop and the setup time is mux1 delay.
 No data should arrive when it is sending data outside and that time is the hold time.
 
 ![image](https://user-images.githubusercontent.com/86367130/124364523-b9138480-dc5f-11eb-867c-a953b872d1fc.png)
 
 RHS is data required time and LHS is data arrival time.
 So, in hold timing analysis slack=Data Arrival Time-Data Required Time.So, slack should not be negative or zero.
 
 ![image](https://user-images.githubusercontent.com/86367130/124364809-72268e80-dc61-11eb-9bd5-8be6925a0088.png)

**Time Analysis after CTS:**
 
 Invoking openroad and reading the Lef File:
![image](https://user-images.githubusercontent.com/86367130/124375250-e80d1300-dcbe-11eb-9709-2d828217b63e.png)
 
 Reading DEF File:
 
 ![image](https://user-images.githubusercontent.com/86367130/124375306-30c4cc00-dcbf-11eb-82de-070273fcb855.png)

 Write and read Db: A db is created in openlane
 ![image](https://user-images.githubusercontent.com/86367130/124375394-9a44da80-dcbf-11eb-9ada-820254b70c79.png)

 Read Liberty
![image](https://user-images.githubusercontent.com/86367130/124378201-d0895680-dccd-11eb-8014-60c24652725d.png)
Reading SDC File
 ![image](https://user-images.githubusercontent.com/86367130/124378249-12b29800-dcce-11eb-8263-6782a34c3f00.png)
  Set Propagated Clock
 ![image](https://user-images.githubusercontent.com/86367130/124378320-51485280-dcce-11eb-9ef5-0b6376f2891f.png)

 ![image](https://user-images.githubusercontent.com/86367130/124378389-b308bc80-dcce-11eb-9780-6497a0141d85.png)
 
Slack MET:
 
 Hold Slack=1.8922
 
 Setup Slack=3.8467
 ![image](https://user-images.githubusercontent.com/86367130/124378424-de8ba700-dcce-11eb-8642-cded31265576.png)
 
 Clock Buffer List-
 ![image](https://user-images.githubusercontent.com/86367130/124378978-121c0080-dcd2-11eb-8c19-eaa70c543618.png)
 
 Removing a buf_1 and replacing with a bigger buffer buf_2
 ![image](https://user-images.githubusercontent.com/86367130/124379746-645f2080-dcd6-11eb-8fea-03e7de906660.png)

 Replacing Cts_def with placement
 
 Inside Openroad
 
![image](https://user-images.githubusercontent.com/86367130/124380540-c0c43f00-dcda-11eb-9bc9-eda08500a8ef.png)
 
 Setup Slack=4.7821
 Hold Slack =1.922
 
 Clock Skew for Hold=0.39
 Clock Skew for Setup=0.39
 
 Re-Inserting the Clock_Buf_1
 ![image](https://user-images.githubusercontent.com/86367130/124380779-53b1a900-dcdc-11eb-87f1-bafca0981a6a.png)

 
 To kill a process=
 kill -9 <processid>



 # <a name="day5">Final Steps for RTL2GDS using tritonRoute and OpenSTA
 
 + **<a name="fi-con-1"> Routing and Design Rule Check(DRC)**
 
 **Routing:**
 
 Routing Algorithm finds the best possible connection between 2 end points.
 
 **Maze Routing (Lee's Algorithm):**
                  Algorithm creates a routing grid in the background. It labels the adjaacent grid to 1 (vertical and horizontal) and next adjaacent grids to 2 and so on till the target is reached.So, there are many ways to reach the target and the routes with least bends are preferred(that is L shaped)
 
 ![image](https://user-images.githubusercontent.com/86367130/124374626-8ea2e500-dcba-11eb-8450-d8ad68ef80d0.png)
 
 **Design Rule Check while Routing**-
                    1.Minimum width of the wire according to its optical wavelength and minimum pitch of the wire.
 
 There might be a signal short while routing if 2 signal are travelling across same wire.So, the layer of the wire is chnaged to another layer. Bottom metal layer is connected to the top with the help of vias.
 
 Parasitic Extraction: Resistances and Capacitances of wires are extracted.
 
 + **<a name="fi-con-2"> Power Distribution Network and Routing
 
 **Generate Power Distribution Network**- gen_pdn
 
 Height of the standard cell=2.72=Pitch of power rails
 
![image](https://user-images.githubusercontent.com/86367130/124381128-688f3c00-dcde-11eb-96e1-5854e91172ab.png)
 
 Power rails from the pads to ring to straps to the standard cell 
 
 ![image](https://user-images.githubusercontent.com/86367130/124381297-5d88db80-dcdf-11eb-9807-06da455bf6af.png)
 
         run_routing
 
 Routing is divided into :
 
 ![image](https://user-images.githubusercontent.com/86367130/124381562-d6d4fe00-dce0-11eb-988e-d4f98affc03b.png)

 
 Fast or Global Route(Fast Route):Routing region into rectangular grid cells
 
 Detail Route: Triton Route 
         Realises segments and vias in accordance to global route.
 
 ![image](https://user-images.githubusercontent.com/86367130/124381826-1c45fb00-dce2-11eb-8289-e7fbc276bd4c.png)
 
 Routing Viewed in Magic:
 ![image](https://user-images.githubusercontent.com/86367130/124389252-f29dcb80-dd03-11eb-89d5-fca6a349dab6.png)
 
 ![image](https://user-images.githubusercontent.com/86367130/124389426-a4d59300-dd04-11eb-8fb3-858db58cae8d.png)


 + **<a name="fi-con-3"> Triton Route Features**
 
 + It performs initial detail route
 + Honours pre-processed route guides
 + Assume route guide for each net satisfies inter-guide connectivity.
 + Works on **MILP(Mixed-Integer Linear Programming)** based panel-routing with intra-layer parallel and inter-layer sequential routing framework.
 
 Pre-processed Route Guides:
 
 Should have unit width
 Should be in preferred direction
 
                        The metal layers have opposite preferred direction to reduce the parallel capacitance developed between them.
 
 ![image](https://user-images.githubusercontent.com/86367130/124390227-3266b200-dd08-11eb-8ef0-ceaa1a0c5c8f.png)
 
 Inter-guide connectivity:
 
 2 guides are connected if:
            they are on same metal layer with touching edges
             they are on neighbouring metal layers with nonzero vertical overlap area.
 
 Detailed Routing:
      Inputs: DEF,LEF,Processed Route Guides
      Output: Detailed Routing Solution with optimized wirelength and vias count.
      Contraints: Route Guide honouring,connectivity constraints and design rules
      
 Handling Connectivity:
 
Access Point:
                       On-grid point on the metal layer in the route guide used to connect lower layer-segmenst, upper layer segments,pins and IO Ports.
 
 Access Point Cluster:
                    
                       Is the union of all Access Points derived from some lower layer segment,upper layer guide, a pin or a IO Port.
 
 ![image](https://user-images.githubusercontent.com/86367130/124391913-8b3a4880-dd10-11eb-8ee9-1e64faec0746.png)
 
 Algorithm for Routing Topology:
 
 Find the minimal and the most optimal point between two APCs.
 
 ![image](https://user-images.githubusercontent.com/86367130/124392046-3d721000-dd11-11eb-8169-9d1e5778c059.png)
 
 # Acknowledgement
 
 + Kunal Ghosh
 + Nickson Jose
 + Mili and Mansi
 
 
 
 

 


 
               
 
 
 

 
 

 
 
 
 
 

      

 
 
 

 
 
 
 
 
 
 
   
 


 
 
 
 


 
 
    

 
    




 
 
 
    
























































































