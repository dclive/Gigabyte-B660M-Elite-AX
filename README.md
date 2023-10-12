# Gigabyte Aorus B660M Elite AX DDR4 1.x
Hackintosh Information for Gigabyte B660M Elite AX motherboard and MacOS 14+

****Hackintosh EFI Information for Gigabyte B660M Elite AX and i5-12400F - Fully working, OC95+, MacOS14.0+****

Motherboard:  https://www.gigabyte.com/Motherboard/B660M-AORUS-ELITE-AX-DDR4-rev-1x#kf

BIOS Version used:  F26, Sept 12, 2023: https://www.gigabyte.com/Motherboard/B660M-AORUS-ELITE-AX-DDR4-rev-1x/support#support-dl-bios

**Changes**

Initial release, 10.12.2023

**Credits**

Most content was sourced from my Asrock B660M HDV writeup.  Please see that for background details.  New stuff:  USB mapping.  Intel BT/Wifi.  Updates to OC95.  

**Tested macOS**

* OC95+ and Sonoma 14.0+ is the only focus of current testing.  

**Hardware**

* Gigabyte Aorus Elite AX DDR4 1.x with BIOS F26 works well in MacOS Sonoma.  It's safe to update, and all testing will only include that BIOS (or later) going forward.  After the flash, load all BIOS defaults.  Then disable GPU ReBar, disable serial, disable secure boot, disable CFGLock, set XMP to on (if your RAM is capable), and ... I think that's all that's required in BIOS.  
* Intel i5-12400F
* AMD RX 5700 GPU or AMD RX 6800XT GPU  [An AMD GPU is required regardless of which 12th gen CPU you use, no exceptions]
  * Most typical, RX470, RX480, RX570, RX580, RX590, Vega 56, Vega 64, RX 5700, RX6600, RX6600XT, RX6800, RX6800XT, RX6900XT will all work.  Some other variants (some RX560, for example) will work also, but you should google for more details before buying.  If you buy a 6900XT and it's the XTXH variant (you'll know because it will work, but won't be GPU-accellerated) please see the appropriate section far below.  I've left the 6900XT "plugs" in place; to remove them simply uncheck the features in OCAT; see far below.

* 32GB RAM PC4400 [2 x 16GB DIMMs]: after flashing to F26 I could no longer run this reliably at PC4400 and so flipped to PC3600.  My benchmark scores generally went up.
* 2TB NVME [ADATA 8200 Pro]
* Corsair RM650x
* PowerMac G5 Case, LaserHive MATX 120 modifications [https://thelaserhive.com/product/g5-matx-120-kit/]. :  Note:  I have no front panel USB3 ports in my case.  You'll need to handle mapping your own USB3 (internal) ports if this is important to you / if you have a different case.  Use USBToolbox in Windows for the simplest experience.  Note that process will have TWO kexts you have to put into your Kexts folder, not one.  
* The integrated Intel wireless & bluetooth module is fully working within MacOS Sonoma.

**Working**

* Bluetooth, Wi-Fi and LOM ethernet
* AMD GPU HDMI & DP Audio;motherboard 3.5MM audio in/out is untested
* Sleep / Wake works; mouse / keyboard wakes machine
* App Store, Time Machine [But to recover, keep track of your USB stick with your serials/MAC Address/etc. embedded in it!]
* Apple Watch unlock (mostly reliable, not perfect), AirDrop
* USB port mapping is complete, resulting in iPhone/iPads charging at 2100 ma, and Apple Watch at 1000 ma.  If yours (iPhone, iPad, Apple Watch) doesn't show this rate in About This Mac / System Report / USB, then your USB mapping may not be working correctly.  All 'A' USB ports on this motherboard, USB2 and USB3, are in use and 'active' (working).  USB-C port works, but is only lightly tested.  **The internal USB3 ports are generally NOT USED: if you wish to use them, remap your USB.**  

**Untested**

* N/A

**Not Working**

* Broadly, anything requiring Intel QuickSync / Intel graphics won't work, since the Intel Xe graphics on 11th-12th gen isn't supported by MacOS in any capacity whatsoever - zero functionality, no exceptions.  You must have an AMD GPU with 11th-12th-13th gen Intel CPUs.
* All video-out ports on the motherboard, as Intel 11th-12th gen (Xe) iGPU isn't supported in MacOS. 
* Universal Control and related functions don't reliably work for me.  I don't use them and can't help with troubleshooting.  

**Disabled**

* In the BIOS:  Disable:  Fast Boot, serial port, CSM, Resize Bar, Secure Boot, CFG-Lock.  Once you get everything else working, feel free to enable resize bar (and use the appropriate controls in OC) if you wish; I don't bother.

**Enabled**

* Above 4G decoding, VT-x, VT-d, XMP2.0 at 3600 mhz.

**Next Steps - Required**

You will need to do the following: 

* Prepare a USB boot disk for MacOS 14.x installation.  The easiest way is on a real Mac, although gibMacOS may work for you as well.  To follow the much easier Real Mac path, read https://support.apple.com/en-us/HT201372 and follow the directions for MacOS, including the terminal command to write the download to the USB stick.  You'll want to format the USB as HFS+ format, GUID.  The application 'TINU' also can make a bootable USB stick... (https://github.com/ITzTravelInTime/TINU)
* Download EFIAgent (https://github.com/headkaze/EFI-Agent) and mount the EFI (ESP) partition for the USB stick you just made.  Using EFIAgent again, "open" the EFI partition so it shows on the Mac desktop.  Note that EFI partitions are typically GRAY in color in EFIAgent.  To find EFIAgent, locate the new icon in the upper right clock area that looks like a circular pie.  ![Screen Shot 2021-09-25 at 7 22 44 PM](https://user-images.githubusercontent.com/4536776/134790066-27597b9e-a37f-47e0-87f5-d3ebbc2af59f.png)

 >>  Remember this process for any future EFI partitions you must mount; this is a common procedure.

* Copy the contents of the attached zipfile to the USB stick, so that your files look something like the picture: 

![EFI Layout](https://user-images.githubusercontent.com/4536776/134783624-10b0c7ba-fb29-4cf1-8017-230d22f8e18b.png)

* The EFI (ESP) partition on the USB stick has an EFI folder in it, in the root, and inside of that folder, there are two subfolders, OC and Boot, each with files in them.  Make sure your EFI partition looks just like this once you've unzipped the zipfile. 
* Note that you'll have TWO partitions on that one USB stick:  the EFS/EFI partition (which has just the EFI folder on it, with the contents above) and the other partition, usually called 'Install MacOS Ventura' which will house the Mac's OS/installation details.  

Technically, you are now done.  You should be able to boot MacOS using the USB stick, and install MacOS onto your SSD.  That said, I usually suggest configuring it a bit *after* you boot into MacOS for the first time with the right serials and ROM info: 

* Download OCAT https://github.com/ic005k/QtOpenCoreConfig and open it.  Read the tooltips showing what all the icons at the top do.  Update to the latest OCAT version by finding the update button and updating.  Don't continue until you've done this.  Run the latest OCAT version.  As of last edit, OC95 is current and fully working.  Over the course of time further updates will be required.  Become familiar with how to pull the latest OCxx release and KEXT updates from within OCAT 'into' your EFI configuration; you will do this a lot.  
* Open your USB stick's config.plist by using OCAT's OPEN icon.
* In OCAT, notice the row of icons on the left side.  Go to "PI" on the row. 
* Let's generate a new serial.  Ensure, under the GENERIC tab, that for "SystemProductName" you have the MacPro7,1.  Then click GENERATE right next to the MacPro7,1 box.  Your serial numbers are now set up.
* Note you can also use the GenSMBIOS command to do all this too (https://github.com/corpnewt/GenSMBIOS)  

Now let's fix your MAC address (ROM) 

* [Mac: This only works if you used the USB stick to install MacOS already; assumes MacOS is installed on this motherboard] Go to the Mac's terminal, type in "ifconfig", and find "en0:", your ethernet adapter.  Find the line "ether xx:xx:xx:xx:xx:xx" and write those numbers, without the :, into the ROM box.  
* [Using Windows, if Windows is installed on this motherboard] Go to Windows' commandline/powershell interface.  Type 'ipconfig /all' and find your ethernet adapter.  Find the line "Physical Address" with xx-xx-xx-xx-xx-xx letters to the right on that same line.  Key in those letters and numbers, without the -, in the ROM box. 
* [The easy way; untested] Click GENERATE immediately to the right of the ROM box. 
* Serialization and ROM setup is now complete.  Press the SAVE icon in OCAT and then quit OCAT. 
* Your USB stick is ready to use to boot your Mac and install MacOS.  
* **After all of the above:  Go to Dortania's page and read - before logging in with any AppleID accounts.  https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html**

**Final Steps**

* Assuming no other issues, your setup is now complete!   
* Restart, press the appropriate key at the boot screen so you can choose a boot disk, and boot from the USB stick (select the uEFI option if prompted).  You'll then be able to step through installation of MacOS.  You'll need to format your SSD as APFS or HFS+ (APFS is the new one; use that).  Name the newly formatted SSD something like **MacSSD** so you know that's what you'll boot from in the future.  Then you can start setup.  
* Once setup is done, use EFIAgent to copy the USB stick's EFI folder, with your serial number modifications, to the SSD's EPS (EFI) partition, and then you'll be able to boot from that disk (and you won't need the USB stick anymore, but keep it forever as a backup!). **Do note**:  Until you've copied the EFI folder from your USB stick to your SSD's EPS (EFI) partition, you must continue to boot into your USB stick before booting into MacOS.  Once you've copied the USB stick's EFI folder to the EPS (EFI) partition on the SSD, then you'll no longer need to use the USB stick to boot, and you'll just boot from the SSD's EPS (EFI) partition. 
* Versioning on this zipfile is OC95.  Future versions, if required, would have higher numbers so it is easier to see what version you have.  Keep the zipfile (name, at least) around so you know what version you have.  
* You can clean up logs and logging / bootup, if you wish, once you have everything sorted.  Doritania's guide has a post-install cleanup section with good details on that.  In the zip, logging is fully enabled, so that if there's a problem you can take a video of the screen on your phone and troubleshoot based on that.
* Use OCAuxiliaryTools to update to later OpenCore releases.  Use MacOS's built-in update mechanism to update MacOS releases.  
* Otherwise, please leave comments/issues here. 

**Benchmark Expectations**

![274719371-98f56e37-ad5d-49e4-89c2-887efaaee3a1](https://user-images.githubusercontent.com/4536776/274719371-98f56e37-ad5d-49e4-89c2-887efaaee3a1.png)

* With BIOS F26 and PC3600 RAM, I get 2288/10111 in GeekBench 6.20.   
* A typical M2 base $499 [Edu] mini is (Geekbench) 2629/9733, so the base i5-12400F (PC4400 RAM) is about 87% of the M2's speed per core, and about 104% of the M2 (mini) speed with all cores compared.  
* Cinebench scores are close with what I see online.  ![274724934-aa847776-4faf-4ab0-9878-d29f6c8ddcae](https://user-images.githubusercontent.com/4536776/274724934-aa847776-4faf-4ab0-9878-d29f6c8ddcae.png)

**Addendum:  6900 Configuration**

Most graphics cards I've listed far above work fine with no additional work required.  A very specific variant of the AMD Radeon RX6900XT, called the XTHX variant, doesn't.  If you buy a 6900XT, there's no immediately obvious way to know which you have when looking at it, or within Windows.  In MacOS, you'll know you have the XTXH because you'll get nice graphics, but there's no graphics acceleration, so doing common things becomes very, very slow.  

To get the 6900XT (XTXH) version working, you must enable the ssdt-brg0.aml file (supplied in the zip, already) in OCAT's ACPI section, and you must add the following to OCAT's DP / PCILists section:  

​	PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)	

And it must have two entries (on the right) associated with it: 

​	device-id	Data	BF730000

​	model	String	Radeon RX 6900 XT (XTXH)-DCL			(or whatever you'd like to call your 6900)

Save, reboot, and your 6900 will be enabled.  Note: Other motherboards may label their bridge chips or other bits differently.  

![209982583-cfde040a-d2bb-46a3-b8dd-ef810f555fc6](https://user-images.githubusercontent.com/4536776/209982583-cfde040a-d2bb-46a3-b8dd-ef810f555fc6.png)

**If you don't have a 6900XT, you should remove the above items.**  

**Addendum:  OC95+**

- Use OCAT to update to the latest OpenCore.  No issues to report.  DO THIS.  It's worth staying current and fully fixed.  OC95 is current in the zip and works great.  Read a simple guide I wrote here for details on howto:  https://github.com/dclive/Howto--Update-OpenCore-with-OCAT

