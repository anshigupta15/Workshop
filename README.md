## Advanced Physical Design Using OpenLANE/Sky130

![Advanced-Physical-Design-using-OpenLANE_Sky130_1](https://user-images.githubusercontent.com/86367130/123914575-2b087700-d99d-11eb-819f-feb44e307b87.png)

# Table of Contents

1.[Inception of open-source EDA,Open LANE and Sky130 PDK](#first-day1)

 + How to Talk to Computers
     + [Introducion to QFN-48 Package, chips, pads, core, die and IPs](#first-content-1)
     + [Introduction to RISC-V](#first-content-2)
     + [From Sotware Applicatins to Hardware](#first-content-3)
 
 + SOC Design and Open LANE
     + [Inroduction to all components of open-source digital asic design](#defines)
     + [Simplified RTL2GDS Flow](#defines)
     + [Introduction to Open LANE and Strive Chipsets](#defines)
     + [Introduction to Open LANE Detailed ASIC Design Flow](#defines)

+ Get famiier to Open Source EDA Tools
    + [Open LANE Directory Structure in Detail](#defines)
    + [Design Preparation Step](#defines)
    + [Review Files After Design Prep and RUN Synthesis](#defines)
    + [Open LANE Project Git Link Description](#defines)
    + [Seps to characterize Synthesis Results](#defines)
    

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























































































