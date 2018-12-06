---?image=assets/images/gitpitch-audience.jpg
@title[Platform Build Lab]
<br><br><br><br><br>
## <span class="gold"   >UEFI & EDK II Training</span>


#### Platform Build Lab 

<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
Note:
  PITCHME.md for UEFI / EDK II Training  Platform Build Lab

  Copyright (c) 2018, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.



---  
@title[Lesson Objective]
<BR>
### <p align="center"<span class="gold"   >Platform Build Labs </span></p>
<span style="font-size:0.9em">Lab Setup and Build for OVMF or Minnowboard Max/Turbot</span> <br>
<br>
<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->
 @fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;<a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/2'>Lab 1:</a> Build a EDK II Platform using OVMF package </span><br><br>
 @fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;<a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/18'>Lab 2:</a> Hardware Setup for Minnowboard Max/Turbot </span><br><br>
 @fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;<a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/26'>Lab 3:</a> Build a EDK II Platform using Minnowboard <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Max/Turbot </span> <br><br>
  



---?image=assets/images/binary-strings-black2.jpg
@title[Lab 1 -Build OVMF Section]
<br><br><br><br><br>
## <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Lab 1: Build OvmfPkg</span>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup OvmfPkg to build and run w/ QEMU and Ubuntu</span>

---
@title[Ubuntu 16.04 Pre-requisites]
### <p align="right"><span class="gold" >Pre-requisites Ubuntu 16.04 </span></p>
Instructions from:<a href="https://github.com/tianocore/tianocore.github.io/wiki/Using-EDK-II-with-Native-GCC#Ubuntu_1604_LTS__Ubuntu_1610 
"> tianocore wiki Ubuntu_1610</a> 
- Example Ubuntu 16.04<br>
- The following need to be accessible for building Edk II Platforms, From the terminal prompt (Cnt-Alt-T) :
```shell
bash$ sudo apt-get install build-essential uuid-dev iasl git gcc-5 nasm 
```
```Assembly
; build-essential - Informational list of build-essential packages
; uuid-dev - Universally Unique ID library (headers and static libraries)
; iasl - Intel ASL compiler/decompiler (also provided by acpica-tools)
; git - support for git revision control system
; gcc-5 - GNU C compiler (v5.4.0 as of Ubuntu 16.04 LTS)
; nasm - General-purpose x86 assembler 
```
```shell
bash$ sudo apt-get install qemu
```
```Assembly
; Qemu – Emulation with Intel architecture with UEFI Shell 
```

Note:

---?image=/assets/images/slides/Slide4.JPG
@title[Create QEMU run script]
### <p align="right"><span class="gold" >Create Qemu Run Script</span></p>


Note:
Create a run-ovmf directory under the home directory

`bash$ cd ~`<br>
`bash$ mkdir ~run-ovmf`<br>
`bash$ cd run-ovmf`

- Create a directory to use as a hard disk image

'bash$ mkdir hda-contents'

- Create a Linux shell script to run the QEMU from the run-ovmf directory

`bash$ gedit RunQemu.sh`
<br>
`qemu-system-x86_64 -pflash bios.bin -hda fat:rw:hda-contents -net none     -debugcon file:debug.log -global isa-debugcon.iobase=0x402 `
<br>
Save and Exit

+++
@title[Create QEMU run script 02]
<p align="right"><span class="gold" >Create Qemu Run Script</span><span style="font-size:0.7em" ><font color="white"><br>- Copy and paste</font></span></p>
1.<span style="font-size:0.9em" >Create a run-ovmf directory under the home directory</span>
```
bash$ cd ~
bash$ mkdir ~run-ovmf
bash$ cd run-ovmf
```
2.<span style="font-size:0.9em" >Create a directory to use as a hard disk image </span>
```
bash$ mkdir hda-contents
```
3.<span style="font-size:0.9em" >Create a Linux shell script to run the QEMU from the run-ovmf directory </span>
```
bash$ gedit RunQemu.sh
// In gedit:
qemu-system-x86_64 -pflash bios.bin -hda fat:rw:hda-contents -net none     -debugcon file:debug.log -global isa-debugcon.iobase=0x402 
```
4.<span style="font-size:0.9em" >Save and Exit</span>

Note:


---
@title[Optional - Downloading the Edk II Source]
<p align="right"><span class="gold" ><b>Download the Edk II Source&nbsp;&nbsp;<i>- Optional</i></b></span></p>


<span style="font-size:0.9em" ><i>OPTIONAL</i> - Open a  “git” command prompt and create a source working directory</span>
```
  bash$ mkdir WS
  bash$ cd WS
```

<span style="font-size:0.8em" >OPTIONAL - Internet Proxies – (company Firewall used for example)</span>

```
 bash$ git config --global https.proxy <proxyname>.domain.com:<port>
 bash$ git config --global http.proxy <proxyname>.domain.com:<port>
```

<span style="font-size:0.8em" >OPTIONAL - Download edk2 source tree using Git command prompt</span>

```
  bash$ git clone https://github.com/tianocore/edk2.git
  
  bash$  make -C edk2/BaseTools
```

<span style="font-size:0.7em" ><b>@color[yellow](NOTE:)</b> Lab Material will have a different “edk2” </span>

Note:


- OPTIONAL - Open a terminal prompt and create a source working directory


`bash$ mkdir ~/src` <br>
`bash$ cd ~/src `


- OPTIONAL - Internet Proxies – (company Firewall used for example)


`bash$ export http_proxy=http://proxy-us.company.com:911` <br>
`bash$ export ftp_proxy=$http_proxy`


- OPTIONAL - Download edk2 source tree using Git

`bash$ git clone https://github.com/tianocore/edk2 `<br>


- OPTIONAL - Build the tools

`bash$ make -C edk2/BaseTools `


- NOTE: Lab Material will have a different “edk2”


---?image=assets/images/binary-strings-black2.jpg
@title[Setup Lab Material sub Section]
<br><br><br><br><br>
## <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup Lab Material </span>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Lab_Material_FW.zip</span>

---
@title[Download Lab_Material_FW -getting the Source ]
### <p align="right"><span class="gold" >Download Lab Material</span><br></span></p>
<span style="font-size:0.9em" >Download the Lab_Material_FW.zip from : </span> @fa[github gp-bullet-white] <span style="font-size:0.7em"><a href="https://github.com/tianocore-training/Lab_Material_FW/archive/master.zip">github.com Lab_Matrial_FW.zip</a></span><br>
<br>
<span style="font-size:0.9em" >OR<br>Use `git clone` to download the Lab_Material_FW<span>
```
bash$ cd $HOME
bash$ git clone https://github.com/tianocore-training/Lab_Material_FW.git
```
<span style="font-size:0.9em" >Directory Lab_Material_FW will be created</span>
```
   FW 
    - Documentation 
	- DriverWizard 
	- edk2      
	- edk2Linux 
	- LabSampleCode 
```


---?image=/assets/images/slides/Slide10.JPG
@title[Build Ovmf Edk2 -getting the Source ]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Extract the Source</font></span></p>

Note:
Extract the Downloaded Lab_Material_FW.zip to Home (this will create a directory FW )

---?image=/assets/images/slides/Slide12.JPG
@title[Build Ovmf Edk2 -getting the Source 02]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Getting the Source</font></span></p>


Note:

- Open a terminal prompt  (Alt-Cnt-T)

- Create a working  space source directory under the home directory
   
bash$ mkdir ~src

- From the FW folder, copy and paste folder “~FW/edk2” to ~src

---?image=/assets/images/slides/Slide14.JPG
@title[Build Ovmf Edk2 -getting the Source 03]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Getting the Source</font></span></p>



Note:

- Rename or mv the directory “~src/edk2/BaseTools” 
   bash$ cd ~src/edk2
   bash$ mv BaseTools BaseToolsX
- Extract the file ~FW/edk2Linux/BaseTools.tar.gz  to  ~src/edk2

---?image=/assets/images/slides/Slide16.JPG
@title[Build Ovmf Edk2 -getting the Source 04]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Getting the Source</font></span></p>


Note:
- Run Make from the Terminal prompt
bash$ cd ~src/edk2
bash$ make –C BaseTools
- Run edksetup (note This will need to be done for every new Terminal prompt)

bash$ . edksetup.sh


---?image=assets/images/binary-strings-black2.jpg
@title[Build Ovmf sub Section]
<br><br><br><br><br>
## <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Build Ovmf Platform </span>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span>


---?image=/assets/images/slides/Slide20.JPG
@title[Build Ovmf Edk2 -update target.txt]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Update Target.txt</font></span></p>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p align="right"><span style="font-size:0.6em" >More info: <a href=" https://github.com/tianocore/tianocore.github.io/wiki/OVMF "> tianocore - wiki/OVMF </a>
</span></p>

Note:
- What is OVMF?
  - Open Virtual Machine Firmware - Build with edk2
  - More info: https://github.com/tianocore/tianocore.github.io/wiki/OVMF 

- Edit the file Conf/target.txt
```
bash$ gedit Conf/target.txt
```
   
1. ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc
2. TARGET_ARCH           = X64
3. TOOL_CHAIN_TAG        = GCC5


- Save and Exit
```
bash$ cd ~src/edk2
bash$ build
```

+++
@title[Build Ovmf Edk2 -update target.txt]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" >–Update Target.txt - COPY and PASTE</span></p>
<span style="font-size:0.9em" >Edit the Conf/target.txt file - Copy and Paste</span>
```
bash$ gedit Conf/target.txt
```
<span style="font-size:0.9em" >In the gedit update: </span>
```
 ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc
   #. . .
 TARGET_ARCH           = X64
   #. . .
 TOOL_CHAIN_TAG        = GCC5
```
<span style="font-size:0.9em" >Then Build</span>
```
 bash$ cd ~src/edk2
 bash$ build
```

Note:



---?image=/assets/images/slides/Slide22.JPG
@title[Build Ovmf Edk2 -build inside Terminal]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Inside Terminal</font></span></p>

Note:
- 
---?image=/assets/images/slides/Slide24.JPG
@title[Build Ovmf Edk2 -Verify]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Verify Build Succeeded</font></span></p>
<p align="left"><span style="font-size:0.75em" >OVMF.fd should be in the Build directory<br>
&nbsp;&nbsp;&nbsp;- For GCC5 with X64, it should be located at:</span></p>
```shell
        ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd
```

Note:

---?image=/assets/images/slides/Slide29.JPG
@title[Build Ovmf Edk2 -invoke QEMU]
### <p align="right"><span class="gold" >Invoke QEMU</span><br></span></p>
<span style="font-size:0.75em" >Change to run-ovmf directory under the home directory</span>
<div class="left1">
<pre>
```
 bash$ cd $HOME/run-ovmf
```
</pre>
<span style="font-size:0.75em" >Copy the OVMF.fd BIOS image to the run-ovmf directory naming it bios.bin</span>
<pre>
```
 bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd bios.bin
```
</pre>
<span style="font-size:0.75em" >Run the RunQemu.sh Linux shell script</span>
<pre>
```
bash$ . RunQemu.sh
```
</pre>
<span style="font-size:0.75em" >QEMU will start and boot to the shell</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>
 


Note:
---?image=assets/images/gitpitch-audience.jpg
@title[End of Section]
<br><br><br><br><br>
## <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End of Lab </span>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href=''>Return to the beginning</a> </span>
 

---?image=assets/images/binary-strings-black2.jpg
@title[Lab 2 -Setup MAX HW Section]
<br><br><br><br><br>
## <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Lab 2: Platform HW Setup</span>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Setup hardware for MinnowBoard Max/Turbot </span>

---?image=/assets/images/slides2/Slide3.JPG
@title[MAX/Turbot HW]
### <p align="right"><span class="gold" >EDK II Platform (MinnowBoard MAX/Turbot)</span></p>

Note:

This lab shows the build process for an actual platform – Minnowboard.org

- Using Tianocore source
- Open source EDK II plus open source binary obj. packages

---?image=/assets/images/slides2/Slide5.JPG
@title[Workshop Lab Hardware]
### <p align="right"><span class="gold" >MinnowBoard  MAX Workshop Lab Hardware</span></p>

Note:

**Warning do not use any other power supply than 5V or the board will Fry



---?image=/assets/images/slides2/Slide7.JPG
@title[Install Ubuntu “Screen”]
### <p align="right"><span class="gold" >Install Ubuntu “Screen”</span></p>


Note:
- Terminal prompt (Cnt-Alt-T)<br>
`bash$ sudo apt-get install screen`<br>
`bash$ cd $Home`<br>
`bash$ gedit ~.screenrc`


` shell /bin/bash`

Click Save

**Warning do not use any other power supply than 5V or the board will Fry

---?image=/assets/images/slides2/Slide9.JPG
@title[Max Test System]
### <p align="right"><span class="gold" >Setup MinnowBoard Max Test System</span></p>


Note:

- Hardware:
- System Under Test (SUT) – MinnowBoard Max or Turbot
- 5V- ** power supply
- USB to 3.3V TTL Cable  (6 pin to USB Type A)
  - Connect the USB w/ 6 pin header to SUT
     -  black wire(pin 1) is closest to the SATA connector
  - Connect the USB Type A connector to Host

---?image=/assets/images/slides2/Slide11.JPG
@title[Max Test System 02]
### <p align="right"><span class="gold" >Setup MinnowBoard Max Test System</span></p>


Note:

- Open Terminal Prompt (Cnt-Alt-T)
```
bash$ dmesg                     	#(to check which USB port is assigned)
bash$ sudo chmod 666 /dev/ttyUSBn	#(where n is the FTDI number)
```
- dmesg command  shows which - ttyUSBn


---?image=/assets/images/slides2/Slide13.JPG
@title[Power on MinnowBoard MAX]
### <p align="right"><span class="gold" >Power on MinnowBoard MAX</span></p>


Note:
- Connect the Power supply cable to the MinnowBoard  MAX
```
bash$ screen /dev/ttyUSBn 115200
```
- MinnowBoard MAX should boot to the UEFI Shell in the Terminal – Screen .

- While in screen  Cnt-A then D goes back to terminal
```
 bash$ screen –r #(returns to screen)
```
- Note: Cnt-H for Backspace

**Warning do not use any other power supply than 5V or the board will Fry

---?image=assets/images/gitpitch-audience.jpg
@title[End of Section]
<br><br><br><br><br>
## <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End of Lab </span>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href=''>Return to the begining</a>&nbsp;&nbsp;  or&nbsp;&nbsp;  <a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/24'>@fa[chevron-right gp-bullet-cyan]</a>  &nbsp;&nbsp;to continue  </span>
 

---?image=assets/images/binary-strings-black2.jpg 
@title[Lab 3 -Build Max/Turbot Section]
<br><br><br><br><br>
## <p align="center"><span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Lab 3: Build MinnowBoard Turbot</span></p>
<span style="font-size:0.9em" > &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>



---?image=/assets/images/slides3/Slide3.JPG
@title[MinnowBoard MAX/ Turbot Platform]
### <p align="right"><span class="gold" >EDK II Platform (MinnowBoard MAX/Turbot)</span></p>


Note:
-  Intel® Atom processor E3800 Series  (Formerly Bay Trail-I)


---?image=/assets/images/slides3/Slide5.JPG
@title[MinnowBoard MAX/ Turbot Platform]
<br>
<p align="left"><span class="gold" >Where to get Open Source<BR> MinnowBoard Max</span></p>
<br>
- <span style="font-size:0.9em"><font  color="yellow">Open Source </font><a href="https://github.com/tianocore/tianocore.github.io/wiki/MinnowBoardMax"> Max Wiki</a></span>
  - <span style="font-size:0.9em">V 1.00 -<a href="https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoardMax-UDK2017"> Github Link</a></span>
- <span style="font-size:0.9em"><font  color="white">Binary Object Modules:<br> </font><a href="https://firmware.intel.com/projects/minnowboard-max ">firmware.intel.com</a></span>
- <span style="font-size:0.9em">How to Build<a href="https://firmware.intel.com/sites/default/files/minnowboard_max-rel_1_00-releasenotes.txt"> Release Notes</a></span>

Note:
- Step by step if NOT downloading Lab release of Minnowboard MAX/Turbot 

---?image=/assets/images/slides3/Slide7.JPG
@title[MinnowBoard MAX/ Turbot Platform]
<br>
<p align="left"><span class="gold" >Where to get Open Source<BR> MinnowBoard Max</span></p>
<br>
- <span style="font-size:0.9em"><font  color="white">Open Source </font><a href="https://github.com/tianocore/tianocore.github.io/wiki/MinnowBoardMax"> Max Wiki</a></span>
  - <span style="font-size:0.9em">V 1.00 -<a href="https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoardMax-UDK2017"> Github Link</a></span>
- <span style="font-size:0.9em"><font  color="Yellow">Binary Object Modules:<br> </font><a href="https://firmware.intel.com/projects/minnowboard-max ">firmware.intel.com</a></span>
- <span style="font-size:0.9em">How to Build<a href="https://firmware.intel.com/sites/default/files/minnowboard_max-rel_1_00-releasenotes.txt"> Release Notes</a></span>

Note:
- Step by step if NOT downloading Lab release of Minnowboard MAX/Turbot 

---
@title[Download MinnowBoard MAX Lab Source]
### <p align="right"><span class="gold" >Download MAX Lab Source</span></p>
<span style="font-size:0.9em" >Download the PlatformBuildLab_FW.zip from : </span> @fa[github gp-bullet-white] <span style="font-size:0.7em"><a href="https://github.com/tianocore-training/PlatformBuildLab_FW/archive/master.zip">github.com PlatformBuildLab_FW.zip</a></span><br>
<br>
<span style="font-size:0.9em" >OR<br>Use `git clone` to download the PlatformBuildLab_FW<span>
```
bash$ cd $HOME
bash$ git clone https://github.com/tianocore-training/PlatformBuildLab_FW.git
```
<span style="font-size:0.9em" >Directory PlatformBuildLab_FW will be created</span>
```
   FW 
    - PlatformBuildLab
	   - Max                    - Minnowboard Max Source for the Labs
	   - BaseToolsMax.tar.gz    - BaseTools for Linux GCC5 build
	   - MinnowBoard.MAX.FirmwareUpdateX64.efi  - UEFI App to flash Firmware .BIN to Target
	   . . .
```

---?image=/assets/images/slides3/Slide9.JPG
@title[MinnowBoard MAX Lab Setup]
### <p align="right"><span class="gold" >MinnowBoard MAX Lab Setup</span></p>


Note:
-  Previous Lab Setup Requirements
```
bash$ sudo apt-get install build-essential uuid-dev iasl git gcc-5 nasm 
```
Additional Lab Setup – `       ~src/FW/PlatformBuildLab`

- Max                	– MinnowBoard Max Project source code 
- BuildToolsMax.tar.gz 	– build tools for GCC compiler 
- At Terminal prompt - Install Screen utility for Serial Console to run UEFI Shell 
```
bash$ sudo apt-get install screen
```	   

---?image=/assets/images/slides3/Slide11.JPG
@title[Get the Minnowboard Max Source]
### <p align="right"><span class="gold" >Copy Minnowboard Max Source</span></p>

Note:
- Open a terminal prompt  (Alt-Cnt-T)
- Create a working  space source directory under the home directory
   - bash$ mkdir ~src
- From the FW/PlatformBuildLab folder, copy and paste folder “~FW/Max” to ~src
 


---?image=/assets/images/slides3/Slide14.JPG
@title[Get the BaseTools]
### <p align="right"><span class="gold" >Get the BaseTools for Max  </span></p>


Note:

-  Extract the Rename or mv the directory “~src/Max/edk2/BaseTools”
```
 bash$ cd ~src/Max/edk2<Br>
 bash$ mv BaseTools BaseTools
```

- Extract the file ~FW/PlatformBuildLab/BaseToolsMax.tar.gz  to  ~src/Max/edk2

---?image=/assets/images/slides3/Slide16.JPG
@title[Platform Source Directory Structure]
### <p align="right"><span class="gold" >Platform Source Directory Structure </span></p>


Note:
-  Platform Source Directory Structure
   -  Build from /Vlv2TbltDevicePkg  directory

---
@title[Steps to Build & Install Firmware]
<br><br>
### <p align="center"><span class="gold" >Steps to Build & Install Firmware</span></p>
<ol>
  <li><span style="font-size:0.9em">Open Terminal prompt (Cnt-Alt-T)</span></li>
  <li><span style="font-size:0.9em"> Cd to  project directory :    `$HOME/src/Max/edk2-platforms/Vlv2TbltDevicePkg` </span></li>
  <li><span style="font-size:0.9em">Invoke the build process</span></li>
  <li><span style="font-size:0.9em"> Locate build output (.BIN file for BIOS image)</span></li>
  <li><span style="font-size:0.9em"> Flash binary image onto the platform</span></li>
  <li><span style="font-size:0.9em"> Reset and boot new firmware to UEFI Shell</span></li>
</ol>
<br>
<br>
<span style="font-size:0.9em"><font color="yellow"><i><b>Next slides will follow the above steps</b></i></font></span>


Note:

Slide says it all
 
---
@title[fix shell properties ]
<br>
<p align="left"><span class="gold" >Fix Script Properties to Execute</span></p>
- Open Terminal prompt (Cnt-Alt-T)
- Cd to work space directory
- Fix script files to "execute"  with `chmod +x`
<BR>

```
 
 bash$ cd ~src/Max/edk2
 
 bash$ chmod +x edksetup.sh
 
 bash$ cd ~src/Max/edk2-platforms/
 
 bash$ chmod +x Vlv2TbltDevicePkg/bld_vlv.sh 
 bash$ chmod +x Vlv2TbltDevicePkg/Build_IFWI.sh
 bash$ chmod +x Vlv2TbltDevicePkg/GenBiosId
```

Note:
 Slide says it all

---?image=/assets/images/slides3/Slide20.JPG
@title[Build Process for DEBUG]
### <p align="right"><span class="gold" >Build Process for DEBUG </span></p>
<span style="font-size:0.9em">From Terminal Prompt enter:  &nbsp;&nbsp;&nbsp;&nbsp;</span><span style="font-size:0.6em"><font color="yellow">Note: <i> the Build will Pause</i></font></span>
```
bash$ cd Vlv2TbltDevicePkg 
bash$ . Build_IFWI.sh MNW2 Debug
```

Note:
 Slide says it all

---?image=/assets/images/slides3/Slide22.JPG
<!-- .slide: data-transition="none" -->		 
@title[Examine Command Line & Build Parameters]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>


+++?image=/assets/images/slides3/Slide23.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 02]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:
 
+++?image=/assets/images/slides3/Slide24.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 03]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:
+++?image=/assets/images/slides3/Slide25.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 04]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:
+++?image=/assets/images/slides3/Slide26.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 05]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:
+++?image=/assets/images/slides3/Slide27.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 06]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:

+++?image=/assets/images/slides3/Slide28.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 07]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:

+++?image=/assets/images/slides3/Slide29.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Examine Command Line & Build Parameters 08]
### <p align="right"><span class="gold" >Examine Build Parameters</span></p>

Note:

---?image=/assets/images/slides3/Slide31.JPG
@title[Build Process for Release]
### <p align="right"><span class="gold" >Build Process for Release</span></p>
From Terminal Prompt enter:
```
bash$ cd Vlv2TbltDevicePkg 
bash$ . Build_IFWI.sh MNW2 Release
```

Note:
 Slide says it all


---
@title[DEBUG & RELEASE Differences]
### <p align="right"><span class="gold" >DEBUG & RELEASE Differences</span></p>
@box[bg-purple text-white rounded fragment](<span style="font-size:0.95em" >Slower boot because the time it takes to display debug info </span>)
@box[bg-green text-white rounded fragment](<span style="font-size:0.95em" >Larger image because of debug code & embedded info </span>)
@box[bg-orange text-white rounded fragment](<span style="font-size:0.95em" >Uses the serial port for debug string output</span>)
@box[bg-blue text-white rounded fragment](<span style="font-size:0.95em" >Contains detailed debug strings that show the boot progress and various `ASSERT` / `TRACE` errors</span>)
 

Note:
### DEBUG build …
- Contains detailed debug strings that show the boot process, along with various ASSERT/TRACE errors
- Uses the serial port for debug string output
- Larger image than RELEASE, due to the embedded debug info
- Slower boot than RELEASE, due to the time it takes to display the debug info


### RELEASE build …
- Does not contain the debug strings
- Does not use the serial port for debug output
- Smaller image than DEBUG
- Faster boot than DEBUG

 
---?image=/assets/images/slides3/Slide39.JPG
@title[Build Process Completed]
### <p align="right"><span class="gold" >Build Process Completed</span></p>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<span style="font-size:0.5em">The EDK II build generates multiple firmware volumes, which are combined in the .BIN image</span>


Note:
The EDK II build generates multiple firmware volumes, which are combined in the .BIN image

---?image=/assets/images/slides3/Slide41_1.JPG
@title[Flash onto the MinnowBoard MAX]
### <p align="right"><span class="gold" >Flashing the New BIOS</span></p>

1.  <span style="font-size:0.85em" >&nbsp;&nbsp;Access Max Binary image file from build folder</span>
  - <span style="font-size:0.75em" >`~src/Max/Vlv2TbltDevicePkg/Stitch`</span>
  - <span style="font-size:0.75em" >DEBUG 	MNW2MAX1.X64.D_0099_01_GCC.bin</span>
  - <span style="font-size:0.75em" >RELEASE	MNW2MAX1.X64.R_0099_01_GCC.bin</span>
2. <span style="font-size:0.85em" >&nbsp;&nbsp;Copy BIN files to a USB Thumb drive</span>
3. <span style="font-size:0.85em" >&nbsp;&nbsp;Copy </span><span style="font-size:0.65em" >`MinnowBoard.MAX.FirmwareUpdateX64.efi`</span><span style="font-size:0.85em" > to a USB thumb &nbsp;&nbsp;drive from `~/FW/PlatformBuildLab`</span>
4. <span style="font-size:0.85em" >&nbsp;&nbsp;Boot to UEFI Shell on Max and type "`FS0:`"</span>
 
Note:
1.  Access Max Binary image file from build folder
  - ~src/Max/Vlv2TbltDevicePkg/Stitch
  - DEBUG 	MNW2MAX1_X64_D_0097_01_GCC.bin
  - RELEASE	MNW2MAX1_X64_R_0097_01_GCC.bin
2. Copy BIN files to a USB Thumb drive
3. Copy MinnowBoard.MAX.FirmwareUpdateX64.efi to a USB thumb drive from $HOME/FW/PlatformBuildLab
4. `bash$ screen /dev/ttyUSBn 115200`



---?image=/assets/images/slides3/Slide43_1.JPG
@title[Flash onto the MinnowBoard MAX 02]
### <p align="right"><span class="gold" >Flashing the New BIOS</span></p>
 
<p style="line-height:70%"><span style="font-size:0.85em" >5.  &nbsp;&nbsp;Run update `.efi` utility with either BIN file </span> <span style="font-size:0.65em" >&lpar;<i>Note</i> the “TAB” Key <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;will fill out the command line for you &rpar;</span></p>

```
FS0:\> MinnowBoard.MAX.FirmwareUpdateX64.efi MNW2MAX1_X64_D_0099_01_GCC.bin
```
<br>
<span style="font-size:0.75em" >Wait for the new firmware update to finish</span>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p style="line-height:70%"><span style="font-size:0.85em" >6. &nbsp;&nbsp;Reset and boot new firmware</span></p>

 
Note:
5. Run update .efi utility with either BIN file  (Note the “TAB” Key will fill out the command line for you 
```
FS0:\> MinnowBoard.MAX.FirmwareUpdateX64.efi MNW2MAX1_X64_D_0099_01_GCC.bin
```

6. Reset and boot new firmware


---?image=/assets/images/slides3/Slide45_1.JPG
@title[Verify after Firmware Update]
### <p align="right"><span class="gold" >Verify after Firmware Update</span></p>
 
- <span style="font-size:0.85em" >Verify that the Firmware was updated by checking the Date</span>
- <span style="font-size:0.85em" >At the shell prompt type “exit”</span>
- <span style="font-size:0.85em" >The EDK II front page will show the BIOS ID with Date/time stamp</span>
 
 
Note:

Verify that the Firmware was updated by checking the Date
At the shell prompt type “exit”
The EDK II front page will show the BIOS ID with Date/time stamp


---  
@title[Summary]
##### <p align="center"<span class="gold"   >Summary </span></p><br>

 @fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;<a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/2'>Lab 1:</a> Build a EDK II Platform using OVMF package </span><br><br>
 @fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;<a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/18'>Lab 2:</a> Hardware Setup for Minnowboard Max/Turbot </span><br><br>
 @fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;<a href='https://gitpitch.com/tianocore-training/Platform_Build_LAB/master#/26'>Lab 3:</a> Build a EDK II Platform using Minnowboard <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Max/Turbot </span> <br><br>
 

---?image=assets/images/gitpitch-audience.jpg
@title[Questions]
<br>
![Questions](/assets/images/questions.JPG =10x) 


---?image=assets/images/gitpitch-audience.jpg
@title[Logo Slide]
<br><br><br>
![Logo Slide](/assets/images/TianocoreLogo.png =10x)


---
@title[Acknowledgements]
#### <p align="center"><span class="gold"   >Acknowledgements</span></p>

```c++
/**
Redistribution and use in source (original document form) and 'compiled' forms (converted
to PDF, epub, HTML and other formats) with or without modification, are permitted provided
that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright 
notice, this list of conditions and the following disclaimer as the first lines of this 
file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML
and other formats) must reproduce the above copyright notice, this list of conditions and 
the following disclaimer in the documentation and/or other materials provided with the 
distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED 
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND 
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE 
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, 
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY 
OF SUCH DAMAGE.

Copyright (c) 2018, Intel Corporation. All rights reserved.
**/

```
