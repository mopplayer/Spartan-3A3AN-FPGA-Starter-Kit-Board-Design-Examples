EDK MicroBlaze Spartan-3A Starter Kit Reference Design

Christophe Charpentier
5/9/2007

Requirements:
-------------
EDK 9.1.01
ISE 9.1.03


Hardware:
---------
Xilinx Spartan-3A Starter Kit board
USB Cable (provided with the Kit)
Serial cable
Ethernet cross-over cable


Implementation:
---------------
XPS is used to implement the system.
The EDK MCH_OPB_DDR2 controller code has been modified to meet timing on the DQS outputs.


Software:
---------
The main application is loaded in internal LMB BRAM.
The system tests and WebServer applications run from DDR2 memory


Step by Step Instructions:
--------------------------
1) Open the system.xmp file in XPS

2) The system contains
	- MicroBlaze v6 running at 66.67MHz
	- LMB BRAM (16KB)
	- MCH OPB DDR2 DRAM running at 133MHz (64MB)
	- OPB UartLite (9600)
	- OPB GPIO for LEDs
	- OPB GPIO for buttons
	- OPB GPIO for DIP switches
	- OPB EMC Controller for BPI Flash (4MB)
	- OPB Timer
	- OPB Interrupt Controller
	- OPB LCD Controller (custom core)
	- OPB Rotary Switch (custom core)
	- OPB Ethernet (10/100)
	- Hardware Debug Module

3) The Implementation is done by XPS
	- Go to Hardware > Generate Bitstream to generate a bit file

4) Compile the code
	- Go to Software > Build All User Applications

6) Configure the FPGA
	- Power on the Spartan-3A Starter Kit. Connect the USB cable
	- Connect the serial cable
	- Open Hyperterminal at 9600,8,1, no parity
	- Go to Device Configuration > Download Bitstream

7) The application will output the result of the DDR2 memory test to the serial terminal


To run the System Tests application:
------------------------------------
1) Open XMD

2) Type: cd system_tests

3) Type: rst

4) Download the application to DDR2
	- Type: dow executable.elf

5) Run the application
	- Type: run


To run the WebServer application:
---------------------------------
1) Create the Memory File System Image
	- Go to Project > Launch EDK Shell
	- Type: 
		cd WebPage
	- Type:
		mfsgen �cvbfs ../WebServer/image.mfs 600 404.html index.html logoV2005.gif xapp433.pdf
	- Close the shell

2) Connect a cross-over Ethernet cable

3) Download the application to memory
	- Open XMD
	- Type:
		rst
	- Type:
		dow -data WebServer/image.mfs 0x25000000
	- Type: 
		dow WebServer/executable.elf
	- Type: 
		con

4) In the serial terminal, configure the Ethernet address

5) The rotary switch will update the first line of the LCD. Pushing on 
   the switch will update Line 2 with the contents of Line 1.

6) Open a web browser
	- Type (use the IP address configured above): 
		http:/192.168.0.2:80
	- Follow the instructions on the web page
