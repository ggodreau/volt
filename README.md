# VCX Nano Setup Guide (VX Manager, ACDelco TDS, Techline Connect, SPS2) + Chevy Volt SHVCS Fix

_All credit to @bit1817 on GM-Volt forums for investigation and write-up, this
just serves as a forkable/versionable repo for future updates and collaboration_


[GM-Volt Forum](https://www.gm-volt.com/threads/2013-service-high-voltage-charging-system-fix-updated-vcx-nano-setup-guide-vx-manager-acdelco-tds-techline-connect-sps2.341268/)

[Video Guide](https://www.youtube.com/watch?v=2A2cehoaXe4)

---
### Binaries

| Package               | Version                                                                                                                                        |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| VX Manager            | [1.8.9](https://mega.nz/file/udohRDTb#xrt4ui1A2yh0GnOWHPmm2jsAdD7PU3G7vcU2K3tCtYA)                                                             |
| GDS2                  | [v.2022.5.0](https://disk.yandex.com/d/dTT8d9YvQuejdQ)                                                                                         |
| TechLineConnect (TLC) | 1.14.0.1 (Requires ACDelco subscription)                                                                                                       |

Notes:
* The VCX site has two VXDIAG GM zip files, `GDS2.zip` and `GDS2Patch.zip` - the patch contains the trojan and doesn't seem to need to be applied for GDS2 to run within TLC. If needed, you can download the patch [here](https://mega.nz/file/oZ820RCJ#BXRW3mOGCC7mfMjtaJ71AzjwghBlCogoM7RNGWi9KmE)
* TechLineConnect only becomes available to download after the _Vehicle Programming Software_ license is purchased through [ACDelco's website](https://www.acdelcotds.com/subscriptions#)
* [Tech2Win v16.02.24](https://mega.nz/#!tIFWUIxT!XBAFZWEFPhQ3A9MtyPYfT1wojVgB1sEmt2T62nrmQcM) is not needed for reprogramming modules, or for diagnostic monitoring
* An active internet connection is needed to flash modules in the car, but is not needed to view DTCs and engine data with GDS2 (this can be done offline, and also using the VCX Nano local wifi AP)
* TLC, when used to reprogram/flash, is bootstrapped from a magic web link on ACDelco's website that provides an auth token to the software that links to your license (there is no local login to the desktop application). This was tested with Microsoft Edge within windows 10. Do not use Chrome, etc. TLC also dynamically downloads firmware from GM servers, depending on your VIN/configuration, when reprogramming occurs.

---

## READ THIS DOCUMENT BEFORE ATTEMPTING

### During this process, you will:
* Create a virtual machine
* Install and set up VX Manager
* Install and set up Techline Connect
* Use SPS2 to reprogram your vehicle using the VCX Nano

### What you need before starting:
* A laptop
* An internet connection
* An active “Service Programming System (SPS2)” subscription from ACDelco TDS
* $45 per vin (24 months access)
* VCX Nano ($130)
* OBD2 scanner

### Important information and tips:
* Read the guide and watch the video to decide if this is feasible for you. You don’t need to be an expert to be successful, but it’s definitely not “plug and play.”
* Try to follow the instructions as closely as possible. You’ll save yourself a lot of time and frustration if you do. The software is very finicky and not user-friendly.
* Don’t think you’ll save time by not using a virtual machine. There’s a good chance you’ll end up using it anyway because it won’t work without it. Do it right the first time.
  * Using a virtual machine will decrease the chance of errors and protect your computer from potential viruses. It also makes it easier to start from scratch with a fresh installation of Windows (if you end up needing to).
* Be patient. The software takes a long time to install and is not very responsive.
* Don’t plug the VCX Nano into your laptop or vehicle until the step says to.
* This guide specifically addresses the “Service High Voltage Charging System” error with 1st generation Chevy Volts. However, the process should be similar with other vehicles.
  * The “Service High Voltage Charging System” message is a common problem on first-generation Volts. Often, the only way to fix the error is to reprogram the BECM and HPCM2 modules (even if nothing is inherently wrong with the vehicle). Before reprograming the modules, you should see if clearing the codes fixes the issue. If clearing the codes does not permanently fix the  problem, you most likely need to reprogram the modules.
  * Before reprogramming, make sure the battery coolant is not low and clear any codes with an OBD2 scanner.
  * I recommend buying a [Chevy Volt 2011-2015 SHVCS Defeat Plug](https://canbustools.com/products/chevy-volt-2011-2015-shvcs-defeat-plug) ($40) to stop the SHVCS message from triggering again. You can install it before programming your vehicle if you choose to buy it.
    * You still need to reprogram the modules if the SHVCS message is already present. The defeat plug just prevents the coolant level sensor from triggering the errors again in the future.
* I created the video on October 29, 2021. Certain things may look different by the time you use this guide, but it should be similar. I will try to keep the guide updated and fix errors.
* If you don’t live in the United States, you might want to use a VPN and change your location when creating your account with ACDelco TDS. The “Service Programming System (SPS2)” subscription might have better pricing and a longer access time in the USA than where you live.


#### Step 1: Download Windows 10 Media Creation Tool and VMware Workstation (00:00)

* Search the web to find the download links or click the links below
  * [Windows 10 Media Creation Tool](https://www.microsoft.com/en-us/software-download/windows10%20)
  * [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)
    * Edit: It might be better to use [VMware Workstation 16 Player](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html) instead of VMware Workstation Pro. The Player version is more user-friendly and the Pro version is limited to a 30-day evaluation. However, either version will work and the setup process is similar in both.

####  Step 2: Open Windows 10 Media Creation Tool and download Windows 10 ISO (00:30)

* Click on the Windows 10 Media Creation Tool file you downloaded to start the installation
* When the screen says “What do you want to do?”
  * Select “Create installation media (USB flash drive, DVD, or ISO file) for another PC” and continue
* When the screen says “Choose which media to use”
  * Select “ISO file” and continue
  * Save the ISO file to your computer.

#### Step 3: Set up VMware (01:57)

* Click on the VMware Workstation installation file you downloaded in step 1 to start the installation

#### Step 4: Set up a virtual machine using VMware (02:58)

* Start the VMware Workstation program
* Select “I want to try VMware Workstation 16 for 30 days” and continue
* Select “Create a New Virtual Machine” and continue
* For the “Installer disc image file,” click browse and then select the Windows 10 ISO you downloaded in step 2 and continue
* Uncheck the box that says “Power this virtual machine after creation” and continue
* Click “Edit virtual machine settings”
  * Change the “Number of cores per processor” and “Memory.” This will make the virtual machine run smoother. The values you select depend on the specification of the laptop you’re using.
  * I suggest setting the memory to half of what your laptop contains and setting the “number of cores per processor” equal to the number of cores your CPU has.
  * My laptop has 16 GB of memory/RAM and a 6 core CPU. Therefore, I selected 8 GB of memory and 6 for the “number of cores per processor.”

#### Step 5: Power on the virtual machine and set up Windows (04:26)

* Select “Power on this virtual machine”
* As soon as the screen says “Press any key to boot from CD or DVD,” click the center of the screen and press any key. If you don’t do this fast enough, you’ll have to close the window and try again.
* When the screen says “Activate Windows”
  * Select “I don’t have a product key” and continue
* When the screen says “Select the operating system you want to install”
  * Select “Windows 10 Pro” and continue
* When the screen says “Which type of installation do you want?”
  * Select “Custom: Install Windows only (advanced)” and continue
* When the screen says “How would you like to set up?”
  * Select “Set up for personal use” and continue
* When the screen says “Let’s add your account”
  * Select “Offline account” and continue
* When the screen says “Sign in to enjoy the full range of Microsoft apps and services”
  * Select “Limited experience” and continue
* When the screen says “Choose privacy settings for your device”
  * Turn off everything and continue
* When the screen says “Let Cortana help you get things done”
  * Select “Not now” and continue

#### Step 6: Configure display settings (optional) (07:31)

* Go to system settings and find the display settings.
  * Do this if you need to change the resolution or make text larger

#### Step 7: Log in to ACDelco TDS and download Techline Connect (08:18)

_You need to have an active “Service Programming System (SPS2)” subscription from ACDelco TDS before attempting this step_

* Log in to ACDelco TDS
* Click on your vehicle programming subscription and then click “Add VIN.” It will open a new webpage.
* My subscription already shows my VIN because I’ve done this before. You don’t actually confirm the VIN until step 12.
  * Click “Download Techline Connect”

#### Step 8: Download and install VX Manager (09:43)

* Go to the [VXDiag website](https://www.vxdiagshop.com/info/download/) and find the download page.
* Click the VX Manager Mega Link
* From the Mega webpage, click download
* The download might be flagged as dangerous or a virus. Bypass the warnings (see video) and open the file. Install the program.
  * This is safe to do because you are using a virtual machine
* When the screen says “Select Components”
  * Select “GM - GDS/Tech2Win” and continue
* Before clicking finish, plug your VCX Nano into your laptop
  * When the screen says “New USB Device Detected”
    * Select “Connect to a virtual machine,” click the virtual machine from the list, and continue
* Click finish

#### Step 9: Set up VX Manager (12:52)

* Click “Firmware,” toggle the update on (if not already), and click “Upgrade.” Close the pop-up window when it finishes.
  * Edit: You might want to skip this step if you’re using an older version of VX Manager like 1.8.4, but try updating if you have problems.
* Click “Update License” and wait for it to finish.
* Click “Diagnostic” and then “PASSTHRU.” Click Install. When it finishes, close the window and close VX Manager.

#### Step 10: Install Techline Connect (14:28)

* Click on the TLCInstaller file you downloaded in step 7 and install the software
* Once you get to the login screen, close Techline Connect. It might take a few tries for the program to close.
* Go back to the TLC Admin Console webpage from step 7 and click “Launch Techline Connect”
* Continue the installation process. Techline Connect should launch when it’s finished.

#### Step 11: Prepare for vehicle programming (18:36)

* Clear the codes, if any, from your vehicle using an OBD2 scanner. I think you can do this with the software as well but I didn’t try.
* Unplug the charger from the car
* You *need* to have your laptop plugged in before continuing. If your laptop’s battery dies during the process, you may brick the module.
* Put the vehicle into accessory mode
  * 1st generation Chevy Volt: hold the power button for 10 seconds without pressing the brake
* Turn off the radio, interior lights, etc. You do not want the 12V battery to drain more than necessary.
* Plug the VCX Nano into the vehicle’s OBD2 port
* Click “Connect Vehicle”
* When the screen says “Please select a device type:”
  * Select “VXDIAG” and continue
* Click SPS2
* At some point, you'll get a pop-up that asks if you want to use the car's VIN with your subscription slot. I think it comes up after you click "next" in Step 12 (around the 20:00 mark in the video). I already did this before I made the guide so the video doesn't show it.

#### Step 12: Reprogram the Battery Energy Control Module (19:59)

_I did not fully complete this step in the video because I already reprogrammed my vehicle_

* Click “Next”
* Select “Battery Energy Control Module” and click next
* Continue to click next until the software begins to reprogram the vehicle. Don’t do anything until it completely finishes.
  * The reprogramming in this step took about 12 minutes for my car. But, your time may vary.
  * The vehicle’s instrument cluster will display warning lights during the process (i.e. Service Stabilitrak, Service Theft Deterrent System, etc.). This is normal.
  * If your module is already up-to-date, you might get a warning about reinstalling the same calibration. It's okay to do this. You still need to reprogram it even if it's up-to-date.
* After it is finished, click “Proceed Same VIN” and continue to step 13

#### Step 13: Reprogram the Hybrid Powertrain Control Module 2 (21:27)

_I did not fully complete this step in the video because I already reprogrammed my vehicle_

* Repeat step 12 but select “Hybrid Powertrain Control Module 2” instead
  * The reprogramming in this step took about 7 minutes for my car. But, your time may vary.
  * The vehicle’s instrument cluster will display warning lights during the process (i.e. Service Stabilitrak, Service Theft Deterrent System, etc.). This is normal.
  * If your module is already up-to-date, you might get a warning about reinstalling the same calibration. It's okay to do this. You still need to reprogram it even if it's up-to-date.
* After the “Hybrid Powertrain Control Module 2” is finished reprogramming, you can close the program and disconnect the VCX Nano.
  * You can ignore the message about the “Warranty Claim Code.”
* Turn off the car and turn it back on normally. *You are finished.*
  * You may have a warning that says "Battery Saver Active" if the 12V battery is low. The message will go away once the voltage returns to normal levels.
  * If your car has the “Reduced Propulsion” message after reprogramming, the battery probably needs to recalibrate itself. This will happen automatically. Just turn off the car and plug it in.
  * All other warning messages should go away after turning the car off and on.

*If you notice an error or have a suggestion for this guide, let me know by messaging me or commenting on the video.*
