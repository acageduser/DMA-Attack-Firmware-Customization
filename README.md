# DMA-Attack-Firmware-Customization
### Executive Overview
This guide provides a **comprehensive approach to hardware obfuscation** using [LambdaConcept's PCIe Screamer Squirrel DMA board](https://shop.lambdaconcept.com/home/50-screamer-pcie-squirrel.html). The project involves simulating a DMA card to avoid detection by anti-cheat systems, disguising it as a [Realtek RTL8111](https://www.aliexpress.us/item/3256805896010027.html) PCIe ethernet network card. This technique is key in cybersecurity, offering innovative methods to protect high-value hardware from automated detection systems.


# Key Skills Demonstrated
- **Firmware Development:** Customized and modified firmware to achieve specific objectives.
- **Hardware Manipulation:** Leveraged FPGA technology to simulate different hardware configurations.
- **Cybersecurity:** Implemented advanced techniques to prevent detection by sophisticated anti-cheat systems.
- **Project Management:** Documented the project comprehensively, ensuring reproducibility and clarity.

#
#
## Purpose
#### Strategic Rationale
Focusing on evading anti-cheat systems like BattleEye (BE) and Easy Anti-Cheat (EAC) mirrors the complexity and organization of sophisticated hacking groups. These systems are developed by intelligent professionals and are highly funded, making them ideal testing grounds for advanced evasion techniques. Successfully bypassing such strong detection mechanisms not only highlights technical expertise but also provides valuable insights that can be applied to broader cybersecurity applications. By tackling these challenges, this project demonstrates customization of DMA firmware, expertise in hardware manipulation, and advanced cybersecurity skills—highly valuable in the tech industry and relevant to defending against equally well-funded, malicious entities.

![Anti-Cheat Company](https://github.com/user-attachments/assets/a189bf0d-5063-411d-b707-095341bbf53b)


## Software and Hardware List
- Hardware:
  - [LabmdaConcept's PCIe Screamer Squirrel DMA Board](https://shop.lambdaconcept.com/home/50-screamer-pcie-squirrel.html)
    <img src="https://github.com/user-attachments/assets/15f259ec-d5a7-4240-9d5b-1cf19fb36ac1" width="60%">
  - [Realtek's RTL8111 - INTEGRATED GIGABIT ETHERNET CONTROLLER by Realtek Semiconductor Corp.](https://www.aliexpress.us/item/3256805896010027.html)

    <img src="https://github.com/user-attachments/assets/c866e84e-980e-4696-a445-fb1db5273403" width="60%">

- Software:
  - [MindShare - Arbor](https://www.mindshare.com/software/Arbor)
  - PCILeech-FPGA [Source Code](https://github.com/ufrisk/pcileech-fpga)
  - [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/)
  - [Xilinx Vivado](https://www.xilinx.com/support/download.html)

## Project Details
### What is the project?
This project is a hardware obfuscation simulation. Hardware obfuscation is an effective cybersecurity technique that hides the true nature of high-priority hardware by disguising it as lower-priority hardware.

### Why is hardware obfuscation effective in Cybersecurity?
Attackers often prioritize targeting high-value hardware. By disguising such hardware as low-value, the likelihood of detection is reduced, effectively stalling attackers.

#### Examples of High-Value Hardware:
- **Servers:** Critical for hosting applications, databases, and sensitive information. Targeting these can lead to significant data breaches and disruptions.
- **Network Routers and Switches:** Central to the infrastructure, compromising these can provide access to a vast amount of network traffic and control.

#### Examples of Low-Value Hardware:
- **Printers:** Commonplace and often overlooked, making them ideal candidates for disguising high-value assets.
- **Scanners:** Similar to printers, these are not typically seen as high-value targets but are prevalent in many environments.

### Advantages of Hardware Obfuscation:
- **Reduced Detection Risk:** By masking high-value assets, the chances of them being targeted are significantly reduced.
- **Increased Attack Complexity:** Attackers must spend more time and resources to identify and target valuable assets, increasing the likelihood of detection and intervention.
- **Strategic Deception:** Creating a false sense of security for attackers can lead them into traps, such as honeypots, where their activities can be monitored and analyzed.

### Potential Applications:
- **Corporate Networks:** Protecting critical servers and databases within a corporate network by disguising them as less critical devices.
- **Industrial Systems:** Securing industrial control systems and critical infrastructure by masking them as standard IT equipment.
- **Financial Institutions:** Safeguarding transaction processing systems by making them appear as non-essential office devices.
- **Government and Defense:** Enhancing the security of sensitive systems and communication networks by using hardware obfuscation techniques.







#
#
# The Project

## 1. Introduction information:
#### 35T
This project will be using a ```35T: Squirrel``` device. 75T and 100T devices will be different!
#### Why RTL8111?
I will also be using the RTL8111 as a 'donor card'. A <ins>donor card</ins> refers to the physical device I'll pull legitimate IDs off of. The donor cards information will be put onto the DMA card. Using a real network card will ensure my IDs are all accurate.
####
I picked the RTL8111 because the drivers are all well documented. This is an important step to keep in mind when choosing the right donor card. Open source drivers are best. A list of open source drivers can be found on [this wiki](https://en.wikipedia.org/wiki/Comparison_of_open-source_wireless_drivers) on the [Comparison of Open-Source Wireless Drivers](https://en.wikipedia.org/wiki/Comparison_of_open-source_wireless_drivers).

## 2. Donor Card IDs
Gathering the IDs off of the donor card is the first part of the project. I will be using [MindShare's Arbor](https://www.mindshare.com/software/Arbor) to get hardware IDs off my donor card. I will perform a PCI scan here:

![image](https://github.com/user-attachments/assets/5a4bbacf-d48a-4bf8-b8b7-76ee801066e4)

We can identify the RTL8111 is on Bus 5 within the PCI Map tab:

![image](https://github.com/user-attachments/assets/ff017c97-bf69-4dc3-85ba-a7464e394b0d)

Inside teh PCI Config tab, we can identify many values within the RTL8111 such as ```B:D:F```, ```Class```, ```Device Description```, ```Device Type```, ```Capabilities```.

![image](https://github.com/user-attachments/assets/0274c609-14d6-4984-9667-79039c511d15)

We can also identify the header for the RTL8111:

![image](https://github.com/user-attachments/assets/9661d5dd-e927-4814-967e-36b2789a35c6)

Nothing out of the ordinary on the RTL8111. We must collect some values from the RTL8111.

#### Base Registry Address (BAR)
Note the size for your base registry addresses (BARs) by clicking on BAR0 to BAR5. A BAR set to '00 00 00 00' is a special bar. The 0's indicate that this specific BAR is not used on the RTL8111.

![image](https://github.com/user-attachments/assets/d63f1258-8167-449b-986b-9cac218ce820)

```BAR0``` is ```256 bytes```.

![image](https://github.com/user-attachments/assets/8e599c98-393c-469b-ba84-6295b3a000e7)

```BAR1``` is not used.

![image](https://github.com/user-attachments/assets/b501d858-a674-488e-a3c9-5f72b419d739)

```BAR2``` and ```BAR3``` are a pair. Their size is ```4KB```.

![image](https://github.com/user-attachments/assets/8bc86fd9-3bcf-4036-b7db-260bfd4783eb)

BAR4 and BAR5 are a pair. Thier size is ```16KB```.

#### Device Serial Number (DSN) Capability Structure

![image](https://github.com/user-attachments/assets/65c570a0-a282-4fbc-b07d-4ad2d8ac66ec)

#### Advanced Error Reporting (AER) Capability Structure

![image](https://github.com/user-attachments/assets/99ecc0f0-7901-48ae-b6f4-a70bcfe1c61e)

#### Power Management (PM) Capability Structure

![image](https://github.com/user-attachments/assets/beae503e-128c-44bc-88b6-5c177974b77a)

### MSI-X Capability Structure

![image](https://github.com/user-attachments/assets/78029156-9a58-4fc2-a38f-828db6dfa3f7)

#### Virtual Channel (VC) Capability Structure

![image](https://github.com/user-attachments/assets/251cb4eb-fce1-4ec2-a76c-b9cc723ecf91)

#### Message Signaled Innterrupt (MSI) Capability Structure

![image](https://github.com/user-attachments/assets/8cb7956c-9ab4-4dff-ba17-9478e952d930)


## 3. Enabling Master Abort Handling for Enhanced Error Detection

Set the master abort flag to ```1``` in ```pcileech-fpga-4.15\PCIeSquirrel\src``` in the ```pcileech_pcie_cfg_a7.sv``` file because it helps catch invalid memory accesses during DMA transactions, which would otherwise go unnoticed if set to 0. This allows for easier debugging if there is an issue with the custom firmware. Detecting and addressing any misconfigurations or failed transactions later on (if there are any) will be easier to manage. Without this change, you risk missing critical errors that could compromise the DMA firmware functionality. This is a safeguard. Do not skip this step.

![image](https://github.com/user-attachments/assets/6378a3fc-d6cc-48a3-9790-7bfc9fc832ac)

I found it quite easily with the ```CTRL + F``` function and the string '```master abort flag```'. There is only 1 mention of this in the entire file. Make the line exactly as follows:

    rw[20]      <= 1;                       //       CFGSPACE_STATUS_REGISTER_AUTO_CLEAR [master abort flag]
Read/Write (rw) ```rw[20]``` is the correct line.

## 4. Setting the DSN

In the same file, copy the DSN value into the ```rw[127:64]``` line. This line is for the cfg_dsn. There are 16 avalible characters in this section. Be sure to copy the ```Lower DW``` over top of the first 8 numbers and copy the ```Upper DW``` into the second 8 numbers like shown:

    rw[127:64]  <= 64'h684CE00051140000;    // +008: cfg_dsn

```684CE000``` and ```51140000``` are our Lower and Upper DWs.

Save ```pcieleech_pcie_cfg_a7.sv```.

## 5. Vivado

#### Open
Vivado is what I will be using to create my ```pcileech_squirrel.xpr``` file. Be sure to know exactly where your ```PCIeSquirrel``` folder is. Mine is in ```M:\CUSTOM FW\pcileech-fpga-4.15\PCIeSquirrel```, so I will ```cd``` into that location and run ```source vivado_generate_project.tcl -notrace``` to build. 

```cd``` into your ```PCIeSquirrel```:
![image](https://github.com/user-attachments/assets/f73bb7f2-823e-4e23-8627-719028f513b7)

    source vivado_generate_project.tcl -notrace

![image](https://github.com/user-attachments/assets/912519ea-9665-43ae-9365-74f0e8e7708e)

Ensure there are no errors when creating the ```.xpr``` file. Open the file ```i_pcie_7x_0 : pcie_7x_0 (pcie_7x_0.xci)``` within the ```Sources``` box. Expand ```pcileech_squirrel_top``` and ```i_pcileech_tlps128_dst64 : pcileech_tlps128_dst64``` until you find ```i_pcie_7x_0 : pcie_7x_0 (pcie_7x_0.xci)```. Open ```i_pcie_7x_0 : pcie_7x_0 (pcie_7x_0.xci)```.

#### IDs
We will now write in the IDs we saved before into the tab ```IDs```.

![image](https://github.com/user-attachments/assets/2048f9a6-6246-4b93-9f63-261349ae4a7f)

I've updated ```Vendor ID```, ```Device ID```, ```Revision ID```, ```Subsystem Vendor ID```, and ```Subsystem ID```. No need to update any values in ```Class Code``` because the RTL8111 is already ```02 00 00```:

![image](https://github.com/user-attachments/assets/b12e8581-e8bb-45a1-8a59-8ba77f72983f)

#### BARs
Navigate to the BARs tab. BARs ```0``` and ```1``` are each 32 bit BARs becasue they contain a single BAR each. BARs ```2```, and ```4``` are 64 bit BARs because they contain two total BARs each. I/O BARs are always 32 bit. Do not enable BARs ```1```, ```3```, and ```5``` because they are unused BARs. We know this because they are hard coded to all ```0```s. BAR ```3``` is part of BAR ```2``` so do not enable BAR ```3```. Likewise, BAR ```5``` is part of BAR ```4``` so do not enable BAR ```5```. All 64 bit BARs will carry into the next BAR, so do not enable the 2nd half of each 64 bit BAR. Update the BARs:

![image](https://github.com/user-attachments/assets/cd908c22-e205-4464-a010-134b33baa3ba)

We should leave the other tabs alone because the DMA device needs to function as normal under the hood. Changing values in other tabs such as ```Power Management``` could over or under volt your DMA card. DO NOT change these tabs or you risk mechanical failure! Instead, click ```OK``` at the bottom of the screen, then click ```Generate``` without changing any default settings in the pop up box.

![image](https://github.com/user-attachments/assets/e3602a57-c994-43b2-be1b-7b8e8670cf0c)
#
Locate ```inst : pcie_7x_0_core_top (pcie_7x_0_core_top.v) (2)```

![image](https://github.com/user-attachments/assets/1cc95d3d-1d41-4408-96e6-37d20665ca72)

We will now manually edit these values. There is no friendly GUI to help us anymore like before. You (generally) read the parameter name by the ```<Capability Structure>_<Capability Name>_<Capability Description>```. For example, ```MSI_CAP_64_BIT_ADDR_CAPABLE``` is in the ```MSI``` Structure, ```CAP``` (short for capabilities) is the name of the Capability, and ```64_BIT_ADDR_CAPABLE``` is the name of the description. I've indcluded screenshots on how to read the MSI capability below. They love to abbreviate the names, so read over them carefully. I'm simply going to list out line numbers and values to change below:

#### Message Signaled Interrupt (Real Example)

![image](https://github.com/user-attachments/assets/52e76a35-d972-4aa3-a182-bd217ff0ee60)

Line 154:

    parameter         MSI_CAP_64_BIT_ADDR_CAPABLE = "TRUE",

### PCIe

![image](https://github.com/user-attachments/assets/e8192e85-8f12-4252-990d-8ce555843d6a)

Line 101:

    parameter integer DEV_CAP_ENDPOINT_L0S_LATENCY = 3,

![image](https://github.com/user-attachments/assets/b930ffb2-0b26-408e-aa30-1ee2c97ad4b1)

How to read:
```3``` is the value for the Endpoint L0s Latency at ```[8:6]```

![image](https://github.com/user-attachments/assets/07278472-fe66-4e94-b5bc-d03fe78ca3fc)

Line 102:
    
    parameter integer DEV_CAP_ENDPOINT_L1_LATENCY = 6,

![image](https://github.com/user-attachments/assets/702e23bc-0f30-491d-a86f-c6c81ce95d3a)

Line 103:

    parameter         DEV_CAP_EXT_TAG_SUPPORTED = "TRUE",

![image](https://github.com/user-attachments/assets/c49d6cde-4a56-4790-9bf4-b364d4bfe0fd)

Line 104:

    parameter integer DEV_CAP_MAX_PAYLOAD_SUPPORTED = 2,

![image](https://github.com/user-attachments/assets/ab9608d5-bf26-4c90-9039-8d89818ccd9e)

Line 105:

    parameter integer DEV_CAP_PHANTOM_FUNCTIONS_SUPPORT = 0,

![image](https://github.com/user-attachments/assets/fe13f3bc-b3e2-4e56-8a24-32d5c765912c)

Line 134:

    parameter [3:0]   LINK_CAP_MAX_LINK_SPEED = 4'h1,

![image](https://github.com/user-attachments/assets/0c04dd50-9ebe-4d29-ad85-c60e7506802c)

Line 135:

    parameter [5:0]   LINK_CAP_MAX_LINK_WIDTH = 6'h1,

![image](https://github.com/user-attachments/assets/58ab4555-0571-423d-816f-596166221ea6)

Line 139:

    parameter [3:0]   LINK_CTRL2_TARGET_LINK_SPEED = 4'h0,

![image](https://github.com/user-attachments/assets/7b910821-380d-470e-9443-3c27dee3c8f7)

Line 163:

    parameter [3:0]   PCIE_CAP_DEVICE_PORT_TYPE = 4'h0,

How to read:
```4'``` means the value is 4 bits wide. The assigned value here must fit within 4 bits.
```h``` indicates the base of the number. ```h``` stands for hex (base 16)
```0``` is the actual value assigned

```4'h0``` means it is a 4 bit hex value that is set to 0. We can see that ```0000b``` is the value assigned for ```Device/Port Type```. Checks out.

![image](https://github.com/user-attachments/assets/f0d9e6e6-512e-4da6-bf2a-cedb88d56ddb)

Line 267:

    parameter         LINK_CAP_ASPM_SUPPORT = 3,

#### Power Management Capability Structure

![image](https://github.com/user-attachments/assets/c44e2ce6-213a-4af2-a960-06fb7c600e84)

Line 167:

    parameter         PM_CAP_D1SUPPORT = "TRUE",
Line 168:
  
    parameter         PM_CAP_D2SUPPORT = "FALSE",

![image](https://github.com/user-attachments/assets/95a4a014-e692-429c-b394-367a1295d24f)

Line 171:

    parameter         PM_CSR_NOSOFTRST = "TRUE",

![image](https://github.com/user-attachments/assets/49ce4a2d-42d4-47af-8a40-f70685a03e29)

Line 310:
    parameter         PM_CAP_AUXCURRENT = 4,

![image](https://github.com/user-attachments/assets/25c852e7-e7b3-4d27-a644-23500ddbd5f7)

Line 315:

    parameter         PM_CAP_VERSION = 3,

#### MSI-X Capability Structure

This part is slightly different.

1. PCIE_BASE_PTR: Set to 0x40, which is where the PCI Express capability structure is located.

Line 292:

    parameter [7:0]   PCIE_BASE_PTR = 8'h40,

2. PCIE_CAP_NEXTPTR: Set to 0xC8, which is where the next capability, Power Management, is located.

Line 164:

    parameter [7:0]   PCIE_CAP_NEXTPTR = 8'hC8,

3. PM_BASE_PTR: Set to 0xC8, which is the offset for Power Management capability.

Line 309:

    parameter [7:0]   PM_BASE_PTR = 8'hC8,

4. PM_CAP_NEXTPTR: Set to 0xD0, pointing to the MSI-X structure at offset 0xD0.

Line 169:

    parameter [7:0]   PM_CAP_NEXTPTR = 8'hD0,

5. MSI_BASE_PTR: Set to 0xD0, the location of the MSI-X capability.

Line 280:

    parameter [7:0]   MSI_BASE_PTR = 8'hD0,

6. MSI_CAP_NEXTPTR: Set to 0x00, indicating the end of the capability chain.

Line 282:

    parameter [7:0]   MSI_CAP_NEXTPTR = 8'h00,

7. CAPABILITIES_PTR: The starting point of the capabilities list is set to 0x40, which is the offset for the PCIe capability

Line 359:

    parameter [7:0]   CAPABILITIES_PTR = PCIE_BASE_PTR,


This will link the capability structures in the configuation space based on the offsets and pointers. 

Save your work so far.

## 6. Building

This will take a while to run. It took around 10 minutes on my 10600KF + 3070 PC.

![image](https://github.com/user-attachments/assets/1296bd0d-8f1e-4fda-8143-7206d70d0774)

Run ```source vivado_build.tcl -notrace``` again in the console.

![image](https://github.com/user-attachments/assets/dd72ffeb-56c5-4392-a233-e5ae5bed6967)

It will generate ```pchileech_squirrel_top.bin```. Flash this onto your DMA Card. This project will not go over how to flash the firmware onto the DMA card as that is out of the scope of this project. This project is for building custom firmware only.

## 7. Testing

#### DMA Card Functionality Check
Connect your second PC (Attack PC) to the PC with the DMA Card installed.

insert pic of my DMA card plugged in

insert image of my Laptop plugged in

I can verify my firmware is working using [Lone DMA Test Tool](https://phoenixlabstore.com/lone-dma-test-tool/).

![image](https://github.com/user-attachments/assets/0c3a5924-8362-4597-b2ef-f20d16c097b2)


## 8. Testing My Firmware Against BattleEye Anti Cheat

DMA cards are forbidden to be plugged into your computer when launching a game equiped with BattleEye Anti Cheat. You will be banned immediately along with your hardware IDs logged on a blacklist. Do not try this part of the guide unless your firmware's configuration space does NOT resemble your vanilla card. I have altered my configuration space, however that is out of the scope of this project. I might go over this later, however if you are reading this note, it is not currently written in this guide. Do this next part at your own risk.

I will be testing on PlayerUnknown's Battlegrounds because it's a free online game that uses BattleEye anti cheat.

#### Firmware Pass Check

I have no issues logging into the game.

![image](https://github.com/user-attachments/assets/cbbaae37-bc14-43d6-8905-3075185f670c)

#
# Conclusion

This project was a success. I will keep the status of my firmware up to date here. When this method no longer works, I will update this section.

As of my last check on 07/03/2025, this method passes and works.

#
## Documentation and Drivers Used
#### Documentation
- [RTL8111](https://pdf1.alldatasheet.co.kr/datasheet-pdf/view/1253500/REALTEK/RTL8111.html)
#### Drivers
- [RTL8111](https://pdf1.alldatasheet.co.kr/datasheet-pdf/download/1253500/REALTEK/RTL8111.html)

## Read Further
- PCILeech [capabilities](https://github.com/ufrisk/pcileech)
  - PCILeech utilizes PCIe hardware devices to read and write system memory via DMA without needing drivers on the target system, supporting various hardware and software memory acquisition methods, including FPGA-based devices for full memory access and inserting kernel implants to enable advanced memory and file system access, operating on Windows and Linux.
 
## Glossary

- **DMA (Direct Memory Access)**: A method that allows hardware devices to access the system memory directly, bypassing the CPU, making it faster for data transfers.

- **FPGA (Field-Programmable Gate Array)**: A type of programmable hardware used for a variety of tasks, including simulating different hardware configurations, like network cards in this project.

- **PCIe (Peripheral Component Interconnect Express)**: A high-speed interface standard used to connect hardware devices, like GPUs or network cards, to the motherboard.

- **BAR (Base Address Register)**: A register that holds the memory or I/O address space allocated to a device, crucial for interacting with the device during PCIe transactions.

- **DSN (Device Serial Number)**: A unique identifier assigned to a hardware device, used for identification during PCIe transactions.

- **TLP (Transaction Layer Packet)**: A type of data packet used in PCIe communication for transmitting data between devices and memory.

- **AER (Advanced Error Reporting)**: A PCIe feature that provides detailed information about errors, improving diagnostics and troubleshooting.

- **VC (Virtual Channel)**: A capability structure in PCIe that allows multiple independent data paths or virtual channels to share the same physical link.

- **Master Abort**: An error condition that occurs when a device attempts to access an invalid memory location or a device that doesn’t respond. Enabling master abort handling allows these errors to be detected and logged.

- **BE (BattleEye)**: An anti-cheat software used in games to detect unauthorized modifications, including DMA-based cheating methods.

- **EAC (Easy Anti-Cheat)**: Another popular anti-cheat system designed to prevent hacking and cheating in online games.

- **Donor Card**: A physical hardware device used to extract IDs and configuration data for creating modified firmware to simulate another device.


# Note
This project is for EDUCATIONAL PURPOSES ONLY. Using this project outside of a purely educational context is not allowed. This project is to teach me a Cybersecurity strategy for detection prevention only. Following this guide assumes you understand the risks involved as it includes rewriting critical firmware files that can brick your hardware. Any firmware manipulation can lead to bricking. Consider this a warning! Follow the steps as they appear ONLY.
