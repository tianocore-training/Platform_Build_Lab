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
Instructions from:<a href="https://github.com/tianocore/tianocore.github.io/wiki/Using-EDK-II-with-Native-GCC#Ubuntu_1604_LTS__Ubuntu_1610 "> tianocore wiki Ubuntu_1610</a> 
- Example Ubuntu 16.04<br>
- The following need to be accessible for building Edk II Platforms, From the terminal prompt (Cnt-Alt-T) :
```bash
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
```bash
bash$ sudo apt-get install qemu
```
```Assembly
; Qemu – Emulation with Intel architecture with UEFI Shell 
```

Note:

---?image=/assets/images/slidesx/Slide5.JPG
@title[Create QEMU run script]
### <p align="right"><span class="gold" >Create Qemu Run Script</span></p>
<span style="font-size:0.9em" >1. Create a run-ovmf directory under the home directory</span>
```bash
bash$ cd ~
bash$ mkdir ~run-ovmf
bash$ cd run-ovmf
```
<span style="font-size:0.9em" >2. Create a directory to use as a hard disk image </span>
```bash
bash$ mkdir hda-contents
```
<span style="font-size:0.9em" >3. Create a Linux shell script to run the QEMU from the run-ovmf directory </span>
```bash
bash$ gedit RunQemu.sh
```


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
```bash
bash$ cd ~
bash$ mkdir ~run-ovmf
bash$ cd run-ovmf
```
2.<span style="font-size:0.9em" >Create a directory to use as a hard disk image </span>
```bash
bash$ mkdir hda-contents
```
3.<span style="font-size:0.9em" >Create a Linux shell script to run the QEMU from the run-ovmf directory </span>
```bash
bash$ gedit RunQemu.sh
// In gedit:
qemu-system-x86_64 -pflash bios.bin -hda fat:rw:hda-contents -net none     -debugcon file:debug.log -global isa-debugcon.iobase=0x402 
```
<span style="font-size:0.9em" >Save and Exit</span>

Note:


---
@title[Optional - Downloading the Edk II Source]
<p align="right"><span class="gold" ><b>Download the Edk II Source&nbsp;&nbsp;<i>- Optional</i></b></span></p>


<span style="font-size:0.9em" ><i>OPTIONAL</i> - Open a  “git” command prompt and create a source working directory</span>
```bash
  bash$ mkdir WS
  bash$ cd WS
```

<span style="font-size:0.8em" >OPTIONAL - Internet Proxies – (company Firewall used for example)</span>

```bash
 bash$ git config --global https.proxy <proxyname>.domain.com:<port>
 bash$ git config --global http.proxy <proxyname>.domain.com:<port>
```

<span style="font-size:0.8em" >OPTIONAL - Download edk2 source tree using Git command prompt</span>

```bash
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
```bash
bash$ cd $HOME
bash$ git clone https://github.com/tianocore-training/Lab_Material_FW.git
```
<span style="font-size:0.9em" >Directory Lab_Material_FW will be created</span>
```bash
   FW 
    - Documentation 
	- DriverWizard 
	- edk2      
	- edk2Linux 
	- LabSampleCode 
```


---?image=/assets/images/slidesx/Slide10.JPG
@title[Build Ovmf Edk2 -getting the Source ]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-50 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Extract the Source</font></span></p>
@snapend

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:80%" align="left"><span style="font-size:0.80em;  " >1. Extract the Downloaded `Lab_Material_FW-master.zip` to `Home` &lpar; this will create a directory `FW`&rpar; </span></p>
<br>
@snapend


Note:
Extract the Downloaded Lab_Material_FW.zip to Home (this will create a directory FW )

---?image=/assets/images/slidesx/Slide11.JPG
@title[Build Ovmf Edk2 -getting the Source 02]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-50 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">– Copy the Source</font></span></p>
@snapend

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:80%" align="left"><span style="font-size:0.80em;  " >
2. Open a terminal prompt (Alt-Cnt-T)<br>
3. Create a working space directory "src" under the home directory<br>&nbsp;&nbsp;<font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp;bash$ mkdir ~src&nbsp;&nbsp;</span></font><br>
4. From the downloaded "`Lab_Material_FW`" folder, <b>copy</b>&nbsp;and &nbsp;<b>paste</b> folder &nbsp;"`.../FW/edk2`"&nbsp; to &nbsp;`~src`
</span></p>
<br>
@snapend


Note:

- Open a terminal prompt  (Alt-Cnt-T)

- Create a working  space source directory under the home directory
   
bash$ mkdir ~src

- From the FW folder, copy and paste folder “~FW/edk2” to ~src

---?image=/assets/images/slidesx/Slide12.JPG
@title[Build Ovmf Edk2 -getting the Source 03]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-50 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">– Getting BaseTools</font></span></p>
@snapend

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:80%" align="left"><span style="font-size:0.750em;  " >
 5. Rename or `mv` the direcotry "`~src/edk2/BaseTools`"<br><font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp;bash$ cd ~src/edk2&nbsp;&nbsp;<br>
&nbsp;&nbsp;bash$ mv BaseTools BaseToolsX&nbsp;&nbsp;</span></font><br>
 6. Extract the file `~FW/edk2Linux/BaseTools.tar.gz`  to  `~src/edk2`
</span></p>
<br>
@snapend


Note:

- Rename or mv the directory “~src/edk2/BaseTools” 
   bash$ cd ~src/edk2
   bash$ mv BaseTools BaseToolsX
- Extract the file ~FW/edk2Linux/BaseTools.tar.gz  to  ~src/edk2

---?image=/assets/images/slidesx/Slide13.JPG
@title[Build Ovmf Edk2 -getting the Source 04]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-50 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">– Building `BaseTools`</font></span></p>
@snapend

@snap[north-west span-100 ]
<br>
<p style="line-height:60%" align="left"><span style="font-size:0.750em;  " ><br>
 7. Run Make from the Terminal prompt <br><font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp;bash$ cd ~src/edk2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;bash$ make -C BaseTools&nbsp;&nbsp;</span></font><br>
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
 8. Run edksetup <span style="font-size:0.55em;  " >(note This will need to be done for every new Terminal prompt)</span><br>
</span></p>
<br>
@snapend

@snap[south-west span-100 ]
<p style="line-height:60%" align="left"><span style="font-size:0.650em;  " >
<font face="Consolas"><span style="background-color: #000000; "> 
&nbsp;&nbsp;bash$ . edksetup.sh &nbsp;&nbsp;</span></font>
</span></p>
<br>
<br>
@snapend

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


---?image=/assets/images/slidesx/Slide15.JPG
@title[Build Ovmf Edk2 -update target.txt]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-60 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">– Update Target.txt & Build</font></span></p>
@snapend


@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:70%" align="left"><span style="font-size:0.80em;  " >
 @size[1.1em](What is OVMF?)<br>
   Open Virtual Machine Firmware - Build with edk2<br>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp;bash$ gedit Conf/target.txt&nbsp;&nbsp;</span></font><br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Save and Exit<br><br>
<font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp;bash$ cd ~src/edk2 &nbsp;&nbsp;<br>
&nbsp;&nbsp;bash$ build  &nbsp;&nbsp;&nbsp;&nbsp;</span></font>
</span></p>
<br>
@snapend


@snap[south-east span-100 ]
<p align="right"><span style="font-size:0.6em" >More info: <a href=" https://github.com/tianocore/tianocore.github.io/wiki/OVMF "> tianocore - wiki/OVMF </a>
</span></p>
@snapend

Note:
- What is OVMF?
  - Open Virtual Machine Firmware - Build with edk2
  - More info: https://github.com/tianocore/tianocore.github.io/wiki/OVMF 

- Edit the file Conf/target.txt
```bash
bash$ gedit Conf/target.txt
```
   
1. ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc
2. TARGET_ARCH           = X64
3. TOOL_CHAIN_TAG        = GCC5


- Save and Exit
```bash
bash$ cd ~src/edk2
bash$ build
```

+++
@title[Build Ovmf Edk2 -update target.txt]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-65 ]
<br>
<p align="right"><span style="font-size:0.75em" ><font color="white">– Update Target.txt - COPY and PASTE</font></span></p>
@snapend
<br>
<span style="font-size:0.9em" >Edit the Conf/target.txt file - Copy and Paste</span>
```bash
bash$ gedit Conf/target.txt
```
<span style="font-size:0.9em" >In the gedit update: </span>
```xml
 ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc
   #. . .
 TARGET_ARCH           = X64
   #. . .
 TOOL_CHAIN_TAG        = GCC5
```
<span style="font-size:0.9em" >Then Build</span>
```bash
 bash$ cd ~src/edk2
 bash$ build
```

Note:



---?image=/assets/images/slidesx/Slide17.JPG
@title[Build Ovmf Edk2 -build inside Terminal]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>

@snap[north-east span-50 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Inside Terminal</font></span></p>
@snapend


Note:
- Inside Terminal

---?image=/assets/images/slidesx/Slide18.JPG
@title[Build Ovmf Edk2 -Verify]
### <p align="right"><span class="gold" >Build EDK II Ovmf</span><br></span></p>
@snap[north-east span-50 ]
<br>
<p align="right"><span style="font-size:0.8em" ><font color="#e49436">–Verify Build Succeeded</font></span></p>
@snapend


@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:80%" align="left"><span style="font-size:0.80em;  " >
 OVMF.fd should be in the Build directory<br>
 &nbsp;&nbsp;&nbsp;- For GCC5 with X64, it should be located at:<br>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp; ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd &nbsp;&nbsp;</span></font>
</span></p>
<br>
@snapend


Note:

---?image=/assets/images/slidesx/Slide19.JPG
@title[Build Ovmf Edk2 -invoke QEMU]
### <p align="right"><span class="gold" >Invoke QEMU</span><br></span></p>
<span style="font-size:0.75em" >Change to run-ovmf directory under the home directory</span>
<div class="left1">
<pre class='bash'>
```
 bash$ cd $HOME/run-ovmf
```
</pre>
<span style="font-size:0.75em" >Copy the OVMF.fd BIOS image to the run-ovmf directory naming it bios.bin</span>
<pre class='bash'>
```
 bash$ cp ~/src/edk2/Build/OvmfX64/DEBUG_GCC5/FV/OVMF.fd bios.bin
```
</pre>
<span style="font-size:0.75em" >Run the RunQemu.sh Linux shell script</span>
<pre class='bash'>
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

---?image=/assets/images/slidesx/Slide22.JPG
@title[MAX/Turbot HW]
### <p align="right"><span class="gold" >EDK II Platform (MinnowBoard MAX/Turbot)</span></p>
@snap[south-west span-45 ]
<br>
<p style="line-height:60%" align="left"><span style="font-size:0.6em" >Intel<sup>&reg;</sup> Atom processor E3800 Series<br> &lpar;Formerly Bay Trail-I&rpar;</span></p>
<br>
@snapend

Note:

This lab shows the build process for an actual platform – Minnowboard.org

- Using Tianocore source
- Open source EDK II plus open source binary obj. packages

---?image=/assets/images/slidesx/Slide23.JPG
@title[Workshop Lab Hardware]
### <p align="right"><span class="gold" >MinnowBoard  MAX Workshop Lab Hardware</span></p>
@snap[south span-100 ]
<br>
<p style="line-height:80%" ><span style="background-color: #FFFFFF; font-size:0.60em; "> <font color="red">&nbsp;<b>**Warning</b> do not use any other power supply than 5V or the board will Fry&nbsp;&nbsp;</font></span></p>
<br>
@snapend
Note:

**Warning do not use any other power supply than 5V or the board will Fry



---?image=/assets/images/slidesx/Slide24.JPG
@title[Install Ubuntu “Screen”]
### <p align="right"><span class="gold" >Install Ubuntu “Screen”</span></p>

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:70%" align="left"><span style="font-size:0.80em;  " >
 Terminal prompt (Cnt-Alt-T)<br>
 Install "Screen"
 <br>
 <br>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp; bash$ sudo apt-get install screen &nbsp;&nbsp;<br>
&nbsp;&nbsp; bash$ cd $Home &nbsp;&nbsp;<br>
&nbsp;&nbsp; bash$ gedit ~.screenrc &nbsp;&nbsp;
</span></font><br><br>
Inside the editor, type<br> @size[.8em]("`shell /bin/bash`") &nbsp;&nbsp;then save
</span></p>
<br>
@snapend

@snap[north-east span-45 ]
<br>
<br>
<p style="line-height:70%" align="left"><span style="font-size:0.60em;  " >
 <font color="#87E2A9"> While in screen<br>
 <b>Cnt-A</b> then <b>D</b> goes back to Terminal
 <br></font><br>
 @size[.9em](`bash$ screen -r`) <br>
 (Returns to screen)
</span></p>
 <br>
@snapend



@snap[south-east span-80 ]
<p style="line-height:70%" align="left"><span style="font-size:0.60em;  " >
There may be other serial terminal applications that are supported. <br>
</span></p>
@snapend

Note:
- Terminal prompt (Cnt-Alt-T)<br>
`bash$ sudo apt-get install screen`<br>
`bash$ cd $Home`<br>
`bash$ gedit ~.screenrc`


` shell /bin/bash`

Click Save

---?image=/assets/images/slidesx/Slide25.JPG
@title[Max Test System]
### <p align="right"><span class="gold" >Setup MinnowBoard Max Test System</span></p>

@snap[north-west span-60 ]
<br>
<br>
<br>
<p style="line-height:50%"><span style="font-size:0.7em"><b>Hardware:</b></span></p>
<ul style="list-style-type:none; line-height:0.6;">
  <li><span style="font-size:0.5em">- System Under Test (SUT) - MinnowBoard Max/Turbot  </span>  </li>
  <li><span style="font-size:0.5em">- USB to 3.3V TTL Cable  (6 pin to USB Type A) </span>  </li>
  <li><span style="font-size:0.5em">- 5V power supply </span>  </li>
</ul>
<p style="line-height:50%"><span style="font-size:0.6em">
Connect the USB w/ 6 pin header to SUT (MAX) <br>
&nbsp;&nbsp;&nbsp;- black wire (pin 1) is closest to the SATA connector<br><br>
Connect the USB Type A connector to Host (Laptop) <br><br>
</span></p>
<br>
@snapend

@snap[south span-100 ]
<br>
<p style="line-height:80%" ><span style="background-color: #FFFFFF; font-size:0.560em; "> <font color="red">&nbsp;<b>**Warning</b> do not use any other power supply than 5V or the board will Fry&nbsp;&nbsp;</font></span></p>
@snapend

Note:

- Hardware:
- System Under Test (SUT) – MinnowBoard Max or Turbot
- 5V- ** power supply
- USB to 3.3V TTL Cable  (6 pin to USB Type A)
  - Connect the USB w/ 6 pin header to SUT
     -  black wire(pin 1) is closest to the SATA connector
  - Connect the USB Type A connector to Host

---?image=/assets/images/slidesx/Slide26.JPG
@title[Max Test System 02]
### <p align="right"><span class="gold" >Setup MinnowBoard Max Test System</span></p>
@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:70%" align="left"><span style="font-size:0.80em;  " >
 Open Terminal prompt (Cnt-Alt-T)<br>
 <br>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.60em;"> 
&nbsp;&nbsp; bash$ dmesg &nbsp;&nbsp;<br>
&nbsp;&nbsp; bash$ sudo chmod 666 /dev/ttyUSB@color[cyan](<i>n</i>) &nbsp;&nbsp;
</span></font>
</span></p>
<br>
@snapend

@snap[north-east span-55 ]
<br>
<br>
<p style="line-height:70%" align="left"><span style="font-size:0.60em;  " >
 <br>
 <br>
 (to check which USB port is assigned)<br>
 (where @color[cyan](<i>n</i>) is the FTDI number ) &nbsp;&nbsp;
</span></p>
<br>
@snapend

Note:

- Open Terminal Prompt (Cnt-Alt-T)
```
bash$ dmesg                     	#(to check which USB port is assigned)
bash$ sudo chmod 666 /dev/ttyUSBn	#(where n is the FTDI number)
```
- dmesg command  shows which - ttyUSBn


---?image=/assets/images/slidesx/Slide27.JPG
@title[Power on MinnowBoard MAX]
### <p align="right"><span class="gold" >Power on MinnowBoard MAX</span></p>
@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:80%" align="left"><span style="font-size:0.80em;  " >
 Connect the Power supply cable to the MinnowBoard  MAX <br>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp; bash$ screen /dev/ttyUSB@color[cyan](<i>n</i>) 115200 &nbsp;&nbsp;
</span></font><br>
MinnowBoard MAX should boot to the UEFI Shell in the Terminal – Screen
</span></p>
<br>
@snapend

@snap[north-east span-25 ]
<br>
<br>
<br>
<br>
<br>
<br>
<p style="line-height:80%" align="left"><span style="font-size:0.560em;  " >
 <br>
 <br>
 @color[yellow](While in Screen<br> <b>Cnt-A</b> then <b>D</b> goes back to terminal.)<br>
 <br>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.90em;"> 
&nbsp;&nbsp; bash$ screen-r &nbsp;&nbsp;
</span></font><br> 
(returns to Screen)
  &nbsp;&nbsp;
</span></p>
<br>
@snapend




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



---?image=/assets/images/slidesx/Slide30.JPG
@title[MinnowBoard MAX/ Turbot Platform]
### <p align="right"><span class="gold" >EDK II Platform (MinnowBoard MAX/Turbot)</span></p>

@snap[south-west span-45 ]
<br>
<p style="line-height:60%" align="left"><span style="font-size:0.6em" >Intel<sup>&reg;</sup> Atom processor E3800 Series<br> &lpar;Formerly Bay Trail-I&rpar;</span></p>
<br>
@snapend
Note:
-  Intel® Atom processor E3800 Series  (Formerly Bay Trail-I)


---?image=/assets/images/slidesx/Slide31.JPG
@title[MinnowBoard MAX/ Turbot Platform]
<br>
<p align="left"><span class="gold" ><b>Where to get Open Source<BR> MinnowBoard Max</b></span></p>
<br>
- <span style="font-size:0.9em"><font  color="yellow">Open Source </font><a href="https://github.com/tianocore/tianocore.github.io/wiki/MinnowBoardMax"> Max Wiki</a></span>
  - <span style="font-size:0.9em">V 1.00 -<a href="https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoardMax-UDK2017"> Github Link</a></span>
- <span style="font-size:0.9em"><font  color="white">Binary Object Modules:<br> </font><a href="https://firmware.intel.com/projects/minnowboard-max ">firmware.intel.com</a></span>
- <span style="font-size:0.9em">How to Build<a href="https://firmware.intel.com/sites/default/files/minnowboard_max-rel_1_00-releasenotes.txt"> Release Notes</a></span>

Note:
- Step by step if NOT downloading Lab release of Minnowboard MAX/Turbot 

---?image=/assets/images/slidesx/Slide32.JPG
@title[MinnowBoard MAX/ Turbot Platform]
<br>
<p align="left"><span class="gold" ><b>Where to get Open Source<BR> MinnowBoard Max</b></span></p>
<br>
- <span style="font-size:0.9em"><font  color="white">Open Source </font><a href="https://github.com/tianocore/tianocore.github.io/wiki/MinnowBoardMax"> Max Wiki</a></span>
  - <span style="font-size:0.9em">V 1.00 -<a href="https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoardMax-UDK2017"> Github Link</a></span>
- <span style="font-size:0.9em"><font  color="yellow">Binary Object Modules:<br> </font><a href="https://firmware.intel.com/projects/minnowboard-max ">firmware.intel.com</a></span>
- <span style="font-size:0.9em">How to Build<a href="https://firmware.intel.com/sites/default/files/minnowboard_max-rel_1_00-releasenotes.txt"> Release Notes</a></span>

Note:
- Step by step if NOT downloading Lab release of Minnowboard MAX/Turbot 

---
@title[Download MinnowBoard MAX Lab Source]
### <p align="right"><span class="gold" >Download MAX Lab Source</span></p>
<span style="font-size:0.9em" >Download the PlatformBuildLab_FW.zip from : </span> @fa[github gp-bullet-white] <span style="font-size:0.7em"><a href="https://github.com/tianocore-training/PlatformBuildLab_FW/archive/master.zip">github.com PlatformBuildLab_FW.zip</a></span><br>
<br>
<span style="font-size:0.9em" >OR<br>Use `git clone` to download the PlatformBuildLab_FW<span>
```bash
bash$ cd $HOME
bash$ git clone https://github.com/tianocore-training/PlatformBuildLab_FW.git
```
<span style="font-size:0.9em" >Directory PlatformBuildLab_FW will be created</span>
```bash
   FW 
    - PlatformBuildLab
	   - Max                    - Minnowboard Max Source for the Labs
	   - BaseToolsMax.tar.gz    - BaseTools for Linux GCC5 build
	   - MinnowBoard.MAX.FirmwareUpdateX64.efi  - UEFI App to flash Firmware .BIN to Target
	   . . .
```

---?image=/assets/images/slidesx/Slide34.JPG
@title[MinnowBoard MAX Lab Setup]
### <p align="right"><span class="gold" >MinnowBoard MAX Lab Setup</span></p>

@snap[north-west span-100 ]
<br>
<p style="line-height:70%"><span style="font-size:0.8em"><br>
@color[#87E2A9](Previous Lab Setup Requirements)<br></span>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.550em;"> 
&nbsp; bash$ sudo apt-get install build-essential uuid-dev iasl git gcc-5 nasm  &nbsp;
</span></font><br>
<span style="font-size:0.8em">
@color[#87E2A9](Additional Lab Setup -)</span><br>
<span style="font-size:0.55em">&nbsp;&nbsp;&nbsp;
@color[#87E2A9]( `PlatformLab_FW/FW/PlatformBuildLab`) 
</span></p>

@snapend


@snap[south-west span-100 ]
<p style="line-height:40%" align="left"><span style="font-size:0.55em">
<b>Directories:</b><br>&nbsp;&nbsp;
&bull;Max<br>&nbsp;&nbsp;
&bull;BuildToolsMax.tar.gz<br><br>&nbsp;&nbsp;
At the Terminal prompt - install Screen utility for Serial Console to run UEFI Shell<br>&nbsp;&nbsp;
 <font face="Consolas"><span style="background-color: #000000; font-size:0.850em;"> 
&nbsp;&nbsp; bash$ sudo apt-get screen &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></font>
&nbsp;&nbsp;
</snap></p>
<br>
@snapend

@snap[south-east span-70 ]
<p style="line-height:40%" align="left"><span style="font-size:0.55em"><font color="yellow">
<br>&nbsp;&nbsp;
&hyphen;&nbsp;&nbsp;MinnowBoard Max Project source code<br>&nbsp;&nbsp;
&hyphen;&nbsp;&nbsp;Build tools for GCC compiler<br><br>&nbsp;&nbsp;
&nbsp;&nbsp;<br>&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;
</font></snap></p>
<br>
@snapend


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

---?image=/assets/images/slidesx/Slide35.JPG
@title[Get the Minnowboard Max Source]
### <p align="right"><span class="gold" >Copy Minnowboard Max Source</span></p>

@snap[north-west span-100 ]
<br>
<p style="line-height:70%"><span style="font-size:0.8em"><br>
Open a terminal prompt(Alt-Cnt-T)<br>
Create a working  space source directory under the home directory<br>
</span>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.60em;"> 
&nbsp;&nbsp; bash$ mkdir ~src &nbsp;&nbsp;&nbsp;&nbsp;
</span></font><br>
<span style="font-size:0.65em">
From the `FW/PlatformBuildLab` folder, copy and paste folder "`~FW/Max`" to `~src`
</span></p>

@snapend
Note:
- Open a terminal prompt  (Alt-Cnt-T)
- Create a working  space source directory under the home directory
   - bash$ mkdir ~src
- From the FW/PlatformBuildLab folder, copy and paste folder “~FW/Max” to ~src
 


---?image=/assets/images/slidesx/Slide36.JPG
@title[Get the BaseTools]
### <p align="right"><span class="gold" >Get the BaseTools for Max  </span></p>
@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:70%"><span style="font-size:0.8em">
Rename or <b>mv</b> the directory @size[.8em]("`~src/Max/edk2/BaseTools`")<br>
</span>
 <font face="Consolas"><span style="background-color: #000000; font-size:0.650em;"> 
&nbsp;&nbsp; bash$ cd ~src/Max/edk2 &nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp; bash$ mv BaseTools BaseToolsX &nbsp;&nbsp;<br>
&nbsp;&nbsp; bash$ tar -xf BaseToolsMax.tar.xz &nbsp;&nbsp;<br>
</span></font><br>
<span style="font-size:0.7em">
Extract the file @size[.8em](`~FW/PlatformBuildLab/BaseToolsMax.tar.gz`)  to  @size[.8em](`~src/Max/edk2`)
</span></p>

@snapend

Note:

-  Extract the Rename or mv the directory “~src/Max/edk2/BaseTools”
```
 bash$ cd ~src/Max/edk2<Br>
 bash$ mv BaseTools BaseTools
```

- Extract the file ~FW/PlatformBuildLab/BaseToolsMax.tar.gz  to  ~src/Max/edk2
  - bash$ tar -xf BaseToolsMax.tar.xz 
  

---?image=/assets/images/slidesx/Slide37.JPG
@title[Platform Source Directory Structure]
### <p align="right"><span class="gold" >Platform Source Directory Structure </span></p>


Note:
-  Platform Source Directory Structure
   -  Build from /Vlv2TbltDevicePkg  directory


---
@title[Steps to Build & Install Firmware]
<br>
### <p align="center"><span class="gold" >Steps to Build & Install Firmware</span></p>
<ul style="list-style-type:none; line-height:0.9;">
  <li><span style="font-size:0.9em">@size[1.125em](<font color="yellow"> &#10102;</font>)&nbsp; Open Terminal prompt & Cd to <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@size[.7em]( `$HOME/src/Max/edk2-platforms/Vlv2TbltDevicePkg`)</span></li>
  <li><span style="font-size:0.9em">@size[1.125em](<font color="yellow"> &#10103;</font>)&nbsp; Fix-up "chmod" script files</span></li>
  <li><span style="font-size:0.9em">@size[1.125em](<font color="yellow"> &#10104;</font>)&nbsp; Invoke the build process</span></li>
  <li><span style="font-size:0.9em">@size[1.125em](<font color="yellow"> &#10105;</font>)&nbsp; Locate build output (.BIN file for BIOS image)</span></li>
  <li><span style="font-size:0.9em">@size[1.125em](<font color="yellow"> &#10106;</font>)&nbsp; Flash binary image onto the platform</span></li>
  <li><span style="font-size:0.9em">@size[1.125em](<font color="yellow"> &#10107;</font>)&nbsp; Reset and verify the new firmware</span></li>
</ul>
<br>
<br>
<span style="font-size:0.9em"><font color="yellow"><i><b>Next slides will follow the above steps</b></i></font></span>


Note:

Slide says it all
 

 
---
@title[fix-up shell properties ]
<br>
<p align="left"><span class="gold" >@size[1.1](<b>Fix-up Script Properties to Execute</b>)</span></p>
<p style="line-height:70%"><span style="font-size:0.8em">
@size[1.25em](<font color="yellow"> &#10102;</font>)&nbsp;&nbsp;Open Terminal prompt (Cnt-Alt-T) & <br>
&nbsp; &nbsp; &nbsp; &nbsp; Cd to work space directory<br>
@size[1.25em](<font color="yellow"> &#10103;</font>)&nbsp;&nbsp;Fix script files to "execute"  with `chmod +x`
</span></p>
<BR>

```bash
 
 bash$ cd ~src/Max/edk2
 
 bash$ chmod +x edksetup.sh
 
 bash$ cd ~src/Max/edk2-platforms/
 
 bash$ chmod +x Vlv2TbltDevicePkg/bld_vlv.sh 
 bash$ chmod +x Vlv2TbltDevicePkg/Build_IFWI.sh
 bash$ chmod +x Vlv2TbltDevicePkg/GenBiosId
```

Note:
 Slide says it all


---
@title[Platform Build Scripts]
<p align="right"><span class="gold" >@size[1.1em](<b>Platform Build Scripts</b>)</span></p>

@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" align="center"><span style="font-size:0.8em">Platform Build Scripts<br>&nbsp; </span></p>)
<p style="line-height:80%"><span style="font-size:0.8em">Many Platform have a bash or bat script file to pre or post process the EDK II build process</span></p>

<p style="line-height:70%"><span style="font-size:0.7em">For MinnowBoard MAX : `Build_IFWI.bat or Build_IFWI.sh` <br></span>
<span style="font-size:0.7em"><Example:<br>
&nbsp;Build_IFWI  <br>
&nbsp;&nbsp;&ndash; pre build processing <br>
&nbsp;&nbsp;&ndash;  calls `vlv_bld` - a platform script to preform the EDK II `build` <br> 
&nbsp;&nbsp;&ndash; determines date <br>
&nbsp;&nbsp;&ndash; board ID<br>
&nbsp;&nbsp;&ndash; post build stitching<br>
</span></p>

Note:

For the platform edk II builds usually a script is called that will do pre and post build processing.  
There is also this capability that is part of the .dsc but many developers have not taken advantage of this feature


---?image=/assets/images/slidesx/Slide41.JPG
@title[Build Process for DEBUG]
### <p align="right"><span class="gold" >Build Process for DEBUG </span></p>

@snap[north-west span-20  ]
<br>
<br>
<br>
<br>
@size[1.125em](<font color="yellow"> &#10104;</font>)
@snapend

@snap[north-east span-95  ]
<br>
<br>
<p align="left"><span style="font-size:0.85em">From Terminal Prompt enter:  &nbsp;&nbsp;</span><span style="font-size:0.6em"><font color="yellow">Note: <i> the Build will Pause</i></font></span></p>
<pre class='bash'>
```
bash$ cd Vlv2TbltDevicePkg 
bash$ . Build_IFWI.sh MNW2 Debug
```
</pre>
@snapend



Note:
 Slide says it all


---
@title[Examine Command Line & Build Parameters]
<p align="right"><span class="gold" >@size[1.1em](<b>Examine Build Parameters</b>)</span></p>

@snap[north-west span-100 ]
<br>
<br>
@box[bg-black text-yellow rounded my-box-pad2  ](<p style="line-height:60%" align="left"><span style="font-size:0.65em; font-family:Consolas; " >&nbsp;&nbsp;build<br><br><br>&nbsp;&nbsp;</span></p>)
@snapend


@snap[north-east span-85  fragment]
<br>
<br>
<p style="line-height:60%" align="left"><span style="font-size:0.60em; font-family:Consolas; " >
<font color="#75deFF">-D SYMBOLIC_DEBUG=TRUE&nbsp;&nbsp;  -D LOGGING=TRUE<br>
 . . . -D <i> Option &lpar;n&rpar;</i> </font>
</span></p>
@snapend


@snap[north-east span-30  fragment]
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.8em"><br></span></p>
@box[bg-white text-black rounded my-box-pad2  ](<p style="line-height:70%" align="left"><span style="font-size:0.8em"><font color="blue"><b>&nbsp;&nbsp;MACROS</font><br>&nbsp;&nbsp;Logging<br>&nbsp;&nbsp;Symbolic Debug<br>&nbsp;&nbsp;</b></span></p>)
@snapend


@snap[north-west span-100 fragment ]
<br>
<br>
<br>
<br>
<p style="line-height:60%" align="left"><span style="font-size:0.8em"><font color="#87E2A9"><br><br><b>Properties from `Conf\Target.txt`</b></font></span></p>
<table id="recTable">
	<tr class="fragment">
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>TARGET</b></span></p></td>
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>= @color[yellow](DEBUG)</b></span></p></td>
		<td align="left" bgcolor="#0070C0" height=".0025"><p style="line-height:010%"><span style="font-size:0.6em" ><b>Build Mode</b></span></p></td>
	</tr>
	<tr class="fragment">
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>TARGET_ARCH</b></span></p></td>
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>= @color[yellow](IA32 X64)</b></span></p></td>
		<td align="left" bgcolor="#0070C0" height=".0025"><p style="line-height:010%"><span style="font-size:0.6em" ><b>CPU Architecture</b></span></p></td>
	</tr>
	<tr class="fragment">
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>TOOL_CHAIN_TAG</b></span></p></td>
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>= @color[yellow](GCC5)</b></span></p></td>
		<td align="left" bgcolor="#0070C0" height=".0025"><p style="line-height:010%"><span style="font-size:0.6em" ><b>VS Tool Chain</b></span></p></td>
	</tr>
	<tr class="fragment">
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>ACTIVE_PLATFORM</b></span></p></td>
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:040%"><span style="font-size:0.460em; font-family:Consolas; " ><b>= @color[yellow](Vlv2TbltDevicePkg /PlatformPkgX64)</b></span></p></td>
		<td align="left" bgcolor="#0070C0" height=".0025"><p style="line-height:010%"><span style="font-size:0.6em" ><b>Platform DSC file</b></span></p></td>
	</tr>
	<tr class="fragment">
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:040%"><span style="font-size:0.460em; font-family:Consolas; " ><b>MAX&lowbar;CONCURRENT&lowbar; THREAD_NUMBER</b></span></p></td>
		<td align="left" bgcolor="#404040" height=".0025"><p style="line-height:010%"><span style="font-size:0.460em; font-family:Consolas; " ><b>= @color[yellow](1)</b></span></p></td>
		<td align="left" bgcolor="#0070C0" height=".0025" width="35%"><p style="line-height:010%"><span style="font-size:0.6em" ><b>Thread Count</b></span></p></td>
	</tr>
</table>

 
@snapend

---
@title[Examine Platform Parameters]
<p align="right"><span class="gold" >@size[1.1em](<b>Platform Build and PCD Parameters</b>)</span></p>

@box[bg-purple-pp text-white rounded my-box-pad2  ](<p style="line-height:70%" align="center"><span style="font-size:0.8em">Platform Parameters<br>&nbsp; </span></p>)
<p style="line-height:80%"><span style="font-size:0.8em">Many Platform Parameters are defined in  a top .DSC file that controls  PCD and build switches</span></p>

<p style="line-height:70%"><span style="font-size:0.7em">For MinnowBoard MAX : `PlatformPkgConfig.dsc` <br>Example:</span></p>

```xml
 #
 # TRUE is ENABLE. FASLE is DISABLE.
 #
  //  . . .
 DEFINE SECURE_BOOT_ENABLE = TRUE
 DEFINE USER_IDENTIFICATION_ENABLE = FALSE
 DEFINE VARIABLE_INFO_ENABLE = FALSE
 DEFINE S3_ENABLE = TRUE
 DEFINE CAPSULE_ENABLE = TRUE
 DEFINE CAPSULE_RESET_ENABLE = TRUE
  // . . .

```

Note:

many will have "ifdef" statements in the major .dsc file in order to enable a feature or not


---?image=/assets/images/slidesx/Slide44.JPG
@title[Build Process for Release]
### <p align="right"><span class="gold" >Build Process for Release</span></p>


@snap[north-west span-20  ]
<br>
<br>
<br>
@size[1.125em](<font color="yellow"> &#10104;</font>)
@snapend

@snap[north-east span-95  ]
<br>
<br>
<p align="left"><span style="font-size:0.85em">From Terminal Prompt enter:  &nbsp;&nbsp;</span><span style="font-size:0.6em"><font color="yellow">Note: <i> the Build will Pause</i></font></span></p>
<pre class='bash'>
```
bash$ cd Vlv2TbltDevicePkg 
bash$ . Build_IFWI.sh MNW2 Release
```
</pre>
@snapend


@snap[north-east span-30  fragment]
<br>
<br>
<br>
<br>
<p style="line-height:40%" align="left"><span style="font-size:0.8em"><br></span></p>
@box[bg-white text-black rounded my-box-pad2  ](<p style="line-height:70%" align="left"><span style="font-size:0.8em"><font color="blue"><b>&nbsp;&nbsp;Note MACROS</font><br>&nbsp;&nbsp;Logging<br>&nbsp;&nbsp;Symbolic Debug<br>&nbsp;&nbsp;<font color="blue">Set to FALSE</font><br>&nbsp;&nbsp;</b></span></p>)
@snapend


Note:

From Terminal Prompt enter:
```
bash$ cd Vlv2TbltDevicePkg 
bash$ . Build_IFWI.sh MNW2 Release
```

 Slide says it all


---
@title[DEBUG & RELEASE Differences]
### <p align="right"><span class="gold" >DEBUG & RELEASE Differences</span></p>

@box[bg-purple-pp text-white rounded my-box-pad2 fragment](<p style="line-height:70%"><span style="font-size:0.9em" >Slower boot because the time it takes to display debug info <br>&nbsp; </span></p>)
@box[bg-green-pp text-white rounded my-box-pad2 fragment](<p style="line-height:70%"><span style="font-size:0.9em" >Larger image because of debug code & embedded info<br>&nbsp;  </span></p>)
@box[bg-gold2 text-white rounded my-box-pad2  fragment](<p style="line-height:70%"><span style="font-size:0.9em" >Uses the serial port for debug string output<br>&nbsp; </span></p>)
@box[bg-royal text-white rounded my-box-pad2  fragment](<p style="line-height:80%"><span style="font-size:0.9em" >Contains detailed debug strings that show the<br> boot progress and various `ASSERT` / `TRACE` errors<br>&nbsp; </span></p>)
 

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

 
---?image=/assets/images/slidesx/Slide46.JPG
@title[Build Process Completed]
### <p align="right"><span class="gold" >Build Process Completed</span></p>
@snap[north-west span-100  ]
<br>
<br>
<span style="font-size:0.9em" >@size[1.25em](<font color="yellow"> &#10105;</font>)&nbsp;&nbsp;Locate the build .BIN image</span>
@snapend

@snap[south-west span-100  ]
<p style="line-height:70%" align="left"><span style="font-size:0.7em" >
The platform build script post build process will stitch the multiple firmware volumes generated by the EDK II build process into the final <b> .BIN</b> image.
</span></p>
@snapend

Note:

### Build Process Completed


- The EDK II build generates multiple firmware volumes, which are combined in the .BIN image
- typically the platform script will call a stitching process to combine all the images together in  post processing after the EDK II build

 

The EDK II build generates multiple firmware volumes, which are combined in the .BIN image


---?image=/assets/images/slidesx/Slide47.JPG
@title[Flash onto the MinnowBoard MAX]
### <p align="right"><span class="gold" >Flashing the New BIOS</span></p>
@snap[north-west span-100  ]
<br>
<br>
<span style="font-size:0.9em" >@size[1.25em](<font color="yellow"> &#10106;</font>)&nbsp;&nbsp;Flash the binary image</span><br>
<span style="font-size:0.85em" >1.&nbsp;&nbsp;Access Max Binary image file from build folder</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;  - <span style="font-size:0.75em" >`~src/Max/Vlv2TbltDevicePkg/Stitch`</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;  - <span style="font-size:0.75em" >DEBUG 	MNW2MAX1.X64.D_0099_01_GCC.bin</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;  - <span style="font-size:0.75em" >RELEASE	MNW2MAX1.X64.R_0099_01_GCC.bin</span><br>
<span style="font-size:0.85em" >2. &nbsp;&nbsp;Copy BIN files to a USB Thumb drive</span><br>
<span style="font-size:0.85em" >3. &nbsp;&nbsp;Copy </span><span style="font-size:0.65em" >`MinnowBoard.MAX.FirmwareUpdateX64.efi`</span><span style="font-size:0.85em" > to a USB thumb<br>&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;drive from `~/FW/PlatformBuildLab`</span><br>
<span style="font-size:0.85em" >4. &nbsp;&nbsp;Boot to UEFI Shell on Max and type "`FS0:`"</span>
@snapend 
 
Note:
1.  Access Max Binary image file from build folder
  - ~src/Max/Vlv2TbltDevicePkg/Stitch
  - DEBUG 	MNW2MAX1_X64_D_0097_01_GCC.bin
  - RELEASE	MNW2MAX1_X64_R_0097_01_GCC.bin
2. Copy BIN files to a USB Thumb drive
3. Copy MinnowBoard.MAX.FirmwareUpdateX64.efi to a USB thumb drive from $HOME/FW/PlatformBuildLab
4. `bash$ screen /dev/ttyUSBn 115200`



---?image=/assets/images/slidesx/Slide48.JPG
@title[Flash onto the MinnowBoard MAX 02]
### <p align="right"><span class="gold" >Flashing the New BIOS</span></p>
 
<p style="line-height:70%"><span style="font-size:0.85em" >5.  &nbsp;&nbsp;Run update `.efi` utility with either BIN file </span> <span style="font-size:0.65em" >&lpar;<i>Note</i> the “TAB” Key <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;will fill out the command line for you &rpar;</span></p>

```bash
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
<p style="line-height:70%"><span style="font-size:0.85em" >&nbsp;&nbsp;Reset and boot new firmware</span></p>

 
Note:
5. Run update .efi utility with either BIN file  (Note the “TAB” Key will fill out the command line for you 
```bash
FS0:\> MinnowBoard.MAX.FirmwareUpdateX64.efi MNW2MAX1_X64_D_0099_01_GCC.bin
```

Reset and boot new firmware


---?image=/assets/images/slidesx/Slide49.JPG
@title[Verify after Firmware Update]
### <p align="right"><span class="gold" >Verify after Firmware Update</span></p>
@snap[north-west span-100  ]
<br>
<br>
<span style="font-size:0.9em" >@size[1.25em](<font color="yellow"> &#10107;</font>)&nbsp;&nbsp;Reboot and Verify</span><br>
 
- <span style="font-size:0.8em" >Verify that the Firmware was updated by checking the Date</span><br>
- <span style="font-size:0.8em" >At the shell prompt type “exit”</span><br>
- <span style="font-size:0.8em" >The EDK II front page will show the BIOS ID with Date/time stamp</span><br>
@snapend
 
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
