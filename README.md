# DMA-Attack-Firmware-Customization
This guide provides a **comprehensive approach to hardware obfuscation** using [LambdaConcept's PCIe Screamer Squirrel DMA board](https://shop.lambdaconcept.com/home/50-screamer-pcie-squirrel.html). The project involves simulating a DMA card to avoid detection by anti-cheat systems, disguising it as a [Realtek RTL8111](https://www.aliexpress.us/item/3256805896010027.html) PCIe ethernet network card. This technique is key in cybersecurity, offering innovative methods to protect high-value hardware from automated detection systems. This project is for EDUCATIONAL PURPOSES ONLY.

# Key Skills Demonstrated
- **Firmware Development:** Customized and modified firmware to achieve specific objectives.
- **Hardware Manipulation:** Leveraged FPGA technology to simulate different hardware configurations.
- **Cybersecurity:** Implemented advanced techniques to prevent detection by sophisticated anti-cheat systems.
- **Project Management:** Documented the project comprehensively, ensuring reproducibility and clarity.

#
#
## Purpose
- #### Strategic Rationale
  - The decision to focus on evading anti-cheat systems is strategic. Anti-cheat systems, like BattleEye (BE) and Easy Anti-Cheat (EAC), are highly funded, well-organized, and developed by intelligent professionals. These characteristics **mirror how sophisticated hacking groups operate**. By successfully evading such strong systems, the techniques developed can be adapted to broader cybersecurity applications, providing valuable insights into defending against equally well-funded and organized malicious entities.

This project demonstrates the customization of DMA firmware to evade detection by anti-cheat systems such as BE and EAC. This work highlights expertise in firmware development, hardware manipulation, and cybersecurity, showcasing advanced skills that are highly valuable in the tech industry.

## Software and Hardware List
- Hardware:
  - [LabmdaConcept's PCIe Screamer Squirrel DMA Board](https://shop.lambdaconcept.com/home/50-screamer-pcie-squirrel.html)
    <img src="https://github.com/user-attachments/assets/15f259ec-d5a7-4240-9d5b-1cf19fb36ac1" width="60%">
  - [Realtek's RTL8111 - INTEGRATED GIGABIT ETHERNET CONTROLLER by Realtek Semiconductor Corp.](https://www.aliexpress.us/item/3256805896010027.html)

    <img src="https://github.com/user-attachments/assets/c866e84e-980e-4696-a445-fb1db5273403" width="60%">

- Software:
  - [Arbor](https://arbor-obfuscation-software.com)
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

### This project has begun, however it is currently in development and not yet completed. Currently waiting for some hardware from China. Will develop further once the hardware arrives! (Update: 13 September 2024 - Shipping date delivery set for 24-27 September 2024)

## 1. Introduction information:
#### 35T
This project will be using a 35T: Squirrel device. 75T and 100T devices will be different!
#### Why RTL8111?
I will also be using the RTL8111 as a 'donor card'. A <ins>donor card</ins> refers to the physical device I'll pull legitimate IDs off of. The donor cards information will be put onto the DMA card. Using a real network card will ensure my IDs are all accurate.
####
I picked the RTL8111 because the drivers are all well documented. This is an important step to keep in mind when choosing the right donor card. Open source drivers are best. A list of open source drivers can be found on [this wiki](https://en.wikipedia.org/wiki/Comparison_of_open-source_wireless_drivers) on the [Comparison of Open-Source Wireless Drivers](https://en.wikipedia.org/wiki/Comparison_of_open-source_wireless_drivers).

## 2. Donor Card IDs
Gathering the IDs off of the donor card is the first part of the project. 

#
Currently waiting for some hardware from China. Will develop further once the hardware arrives! (Update: 13 September 2024 - Shipping date delivery set for 24-27 September 2024)

#
#
## Documentation and Drivers Used
#### Documentation
- [RTL8111](https://pdf1.alldatasheet.co.kr/datasheet-pdf/view/1253500/REALTEK/RTL8111.html)
#### Drivers
- [RTL8111](https://pdf1.alldatasheet.co.kr/datasheet-pdf/download/1253500/REALTEK/RTL8111.html)

## Read Further
- PCILeech [capabilities](https://github.com/ufrisk/pcileech)
  - PCILeech utilizes PCIe hardware devices to read and write system memory via DMA without needing drivers on the target system, supporting various hardware and software memory acquisition methods, including FPGA-based devices for full memory access and inserting kernel implants to enable advanced memory and file system access, operating on Windows and Linux.
