![Screenshot from 2022-08-15 17-17-44](https://user-images.githubusercontent.com/69398841/184629923-4325bf41-9446-4510-a93a-1fa72bdca850.png)
# iiitb_lifo - LIFO Buffer

## Introduction
LIFO (Last in First out) Buffer
Many times in embedded programming, we need to use a data structure to organize our data, and a good example of this is the buffer. Oftentimes, we have a source of data, or a producer, and we also have a destination for data, a consumer. These two devices usually need to pass data in between one another. It is possible that these two devices often do not work at the same speed. Sometimes, one is much faster at producing or consuming the data. This means that we cannot just call the consumer from the producer to handle that data directly with a function call. We need an intermediate location to store data to act as a buffer for this interface. This buffer is usually implemented as a data structure in shared memory between these two processing interfaces.
The LIFO buffer adds data and removes data from the same end. The LIFO data structure can be referred to as a stack, but not be confused with the processor stack. The LIFO differs from the processor stack because it adds and removes data of the same type, whereas the stack on the processor adds or removes different types of data depending on the type of calling convention or function. In terms of memory, this buffer usually is implemented in an array-like format but it is not an array. LIFO buffers are a contiguous piece of memory that require special methods to add and remove data. 

## Application
A stack is a reserved area in the memory which is used by the processor to store data that is not being used currently by the processor but is still is required future processing steps. Stack memories are useful when there is a need to call a subroutine function. If the registers used by the subroutine were being used in the calling portion of the program, then they cannot be erased. Upon returning to the calling portion of the program these registers should have the same values that they had before the subroutine call and not the values of the variables used in the subroutine. When a processor switches between tasks in a multitasking environment then a context of the all the tasks that are not currently running must be stored to allow for the switching back to them. This is done by storing the context of the inactive tasks in the stack.

Stacks are implemented as Last In First Out (LIFO) Buffers. The term PUSH to stack means putting data in the stack and the term POP from stack means removing the data from the top of stack (TOS)

The push and pull operations for a stack are described below:

    Push Operation
    Steps:
        1. Decrement Stack Pointer
        2. Push/Write Data to the top of the stack


    Pop Operation
    Steps:
        1. Pop/Read Data from the top of the stack
        2. Increment the Stack Pointer

## Refernce Circuit Diagram
![Screenshot1](https://user-images.githubusercontent.com/69398841/183976203-462cef60-0784-41bc-a0d9-5b296b5dfb0c.png)

## Software used
### Icarus Verilog
Icarus Verilog is a Verilog simulation and synthesis tool. It operates as a compiler, compiling source code written in Verilog (IEEE-1364) into some target format. For batch simulation, the compiler can generate an intermediate form called vvp assembly. This intermediate form is executed by the ``vvp'' command. For synthesis, the compiler generates netlists in the desired format. The compiler proper is intended to parse and elaborate design descriptions written to the IEEE standard IEEE Std 1364-2005.
Icarus Verilog is available for Linux, FreeBSD, OpenSolaris, AIX, Microsoft Windows, and Mac OS X. Released under the GNU General Public License, Icarus Verilog is free software.


### GTKWave
GTKWave is an analysis tool used to perform debugging on Verilog or VHDL simulation models. With the exception of interactive VCD viewing, it is not intended to be run interactively with simulation, but instead relies on a post-mortem approach through the use of dumpfiles. It is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing.
[GTKWave Documentation](http://gtkwave.sourceforge.net/gtkwave.pdf)

### Installing procedure for Ubuntu
```
$ sudo apt-get install verilog
$ sudo apt-get install gtkwave
```
For more information: click [here](https://iverilog.fandom.com/wiki/Installation_Guide#Ubuntu_Linux)

## Functional Simulation
To clone the repository and download the files for simulation, enter the following commands in your terminal:
```
$ sudo apt install git
$ git clone https://github.com/yashkthr/iiitb_lifo.git
$ cd iiitb_lifo/
```

## RTL Simulation
Verification of the design is done at two points. First, it is applied to the initial verilog design. This process is called the Register Transfer Level (RTL) simulation. This verifies only the logic without delays. The input to this verification process is a test bench written in verilog.
To simulate and run the verilog, enter the following command in your terminal.
```
$ iverilog iiitb_lifo.v iiitb_lifo_tb.v
$ ./a.out
```
For pre-synthesis waveform:
```
$ gtkwave iiitb_lifo_out.vcd
```
![iiitb_lifo_gtkwave](https://user-images.githubusercontent.com/69398841/183555415-b5bf45a9-1a3c-4b97-ad85-8bbc00d7a289.png)

## Synthesis of verilog code
### Yosys
This is a framework for RTL synthesis tools. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys can be adapted to perform any synthesis job by combining the existing passes (algorithms) using synthesis scripts and adding additional passes as needed by extending the yosys C++ code base. Yosys is free software licensed under the ISC license (a GPL compatible license that is similar in terms to the MIT license or the 2-clause BSD license).
[Yosys GitHub](https://github.com/YosysHQ/yosys)

For the synthesis of verilog code, run following command:
```
yosys
yosys> script yosys_run.sh
yosys> show
yosys> stat
yosys> exit
```
The following commands are executed:
```
read_verilog iiitb_lifo.v
synth -top iiitb_lifo
dfflibmap -liberty /home/yash/Desktop/iiitb_lifo/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty /home/yash/Desktop/iiitb_lifo/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
opt
clean
flatten
write_verilog -noattr iiitb_lifo_net.v
```
![Screenshot from 2022-08-15 16-39-37](https://user-images.githubusercontent.com/69398841/184625093-1dd5055e-79f8-4915-a850-84817e9920b7.png)
## Gate Level Simulation GLS
When we write the RTL code, we test it by giving it some stimulus through the testbench and check it for the desired specifications. Similarly, we run the netlist as the design under test (dut) with the same testbench. Gate level simulation is done to verify the logical correctness of the design after synthesis. Also, it ensures the timing of the design.
```
$ iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 verilog_files/primitives.v verilog_files.v/sky130_fd_sc_hd.v iiitb_lifo_net.v iiitb_lifo_tb.v
$ ./a.out
```
For post-synthesis waveform:
```
$ gtkwave iiitb_lifo_out.vcd
```
![Screenshot from 2022-08-15 16-29-32](https://user-images.githubusercontent.com/69398841/184625753-ffee4d0e-0704-4bcd-9ad2-2e96696913dc.png)

# 6. Layout

## Openlane
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, CU-GR, Klayout and a number of custom scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII.

more at https://github.com/The-OpenROAD-Project/OpenLane
## Installation instructions 
```
$   apt install -y build-essential python3 python3-venv python3-pip
```
Docker installation process: https://docs.docker.com/engine/install/ubuntu/

goto home directory->
```
$   git clone https://github.com/The-OpenROAD-Project/OpenLane.git
$   cd OpenLane/
$   sudo make
```
To test the open lane
```
$ sudo make test
```
It takes approximate time of 5min to complete. After 43 steps, if it ended with saying **Basic test passed** then open lane installed succesfully.

## Magic
Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.

More about magic at http://opencircuitdesign.com/magic/index.html

Run following commands one by one to fulfill the system requirement.

```
$   sudo apt-get install m4
$   sudo apt-get install tcsh
$   sudo apt-get install csh
$   sudo apt-get install libx11-dev
$   sudo apt-get install tcl-dev tk-dev
$   sudo apt-get install libcairo2-dev
$   sudo apt-get install mesa-common-dev libglu1-mesa-dev
$   sudo apt-get install libncurses-dev
```
**To install magic**
goto home directory

```
$   git clone https://github.com/RTimothyEdwards/magic
$   cd magic/
$   ./configure
$   sudo make
$   sudo make install
```
type **magic** terminal to check whether it installed succesfully or not. type **exit** to exit magic.

## Generating the Layout
 
Download the config.json file and place it in the `iiitb_lifo` folder.To generate the layout, type the following commands
```
cd OpenLane
make mount
./flow.tcl -design iiitb_lifo
```
To see the layout we use a tool called magic which we installed earlier.Type the following command in the terminal opened in the path to your design/runs/latest run folder/final/def/
 
```
$   magic -T /home/parallels/Desktop/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../../tmp/merged.max.lef def read iiitb_rv32i.def &
```

## Author
- Yash Kothari, Post Graduate Student, IIIT Bangalore

## Acknowledgement
- Kunal Ghosh, Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.
- Dr. Murali, Co-ordinator (M.Tech), IIIT Bangalore
- Dr. Madhav Rao, ECE Department, IIIT Bangalore

## Contact Information
- Yash Kothari | Yash.Kothari@iiitb.ac.in
- Kunal Ghosh | kunalghosh@gmail.com
- Dr. Murali | coordinator-mtech@iiitb.ac.in
- Dr. Madhav Rao | mr@iiitb.ac.in
