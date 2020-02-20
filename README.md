# boot.efi-macOS-X-Catalina-10.15.1
### boot.efi working in VirtualBox with macOS Catalina

#### Original boot.efi from "Install macOS Catalina" (10.15.1), works in VirtualBox and can fix VirtualBox boot issue in later versions of Catalina.

## STUCK AT BOOT SOLVED !!! 

**Step 1**: Download Catalina 10.15.2 or Catalina 10.15.3 with a Mac.  

**Step 2**: Create a modified ISO installer changing boot.efi for Catalina 10.15.1 boot.efi.
You can download Catalina 10.15.1 boot.efi here
[https://github.com/chapuza/boot.efi-macOS-X-Catalina-10.15.1/blob/master/boot.efi](url)

Copy downloaded boot.efi to your Desktop. 

Open Terminal in the Mac and type:

`hdiutil create -o /tmp/Catalina -size 8050m -volname Catalina -layout SPUD -fs HFS+J`

`hdiutil attach /tmp/Catalina.dmg -noverify -mountpoint /Volumes/Catalina`

`sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Catalina --nointeraction`

`bless --folder "/Volumes/Install macOS Catalina/System/Library/CoreServices" --bootefi ~/Desktop/boot.efi`

`hdiutil detach /Volumes/Install\ macOS\ Catalina\`

`hdiutil convert /tmp/Catalina.dmg -format UDTO -o ~/Desktop/Catalina.cdr`

`mv ~/Desktop/Catalina.cdr ~/Desktop/Catalina.iso`

NOW YOU HAVE A VIRTUALBOX BOOTABLE CATALINA 10.15.2 OR CATALINA 10.15.3 IN YOUR DESKTOP.  Copy it and you can start the installation in VirtualBox with this ISO installer.

**Step 3:** Run the Catalina.iso installer in VirtualBox 6.1 , but do not forget to prepare the machine before closing VirtualBox and running CMD as administrator (in Windows host) and typing:

`cd "C:\Program Files\Oracle\Virtualbox\"`

`VBoxManage.exe modifyvm "??????Virtual Machine Name, in my case Mac OS X Catalina????" --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff`

`VBoxManage setextradata "?????Virtual Machine Name, in my case Mac OS X Catalina?????" "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"`

`VBoxManage setextradata "?????Virtual Machine Name, in my case Mac OS X Catalina???" "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"`

`VBoxManage setextradata "????Virtual Machine Name, in my case Mac OS X Catalina????" "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"`

`VBoxManage setextradata "????Virtual Machine Name, in my case Mac OS X Catalina????" "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"`

`VBoxManage setextradata "????Virtual Machine Name, in my case Mac OS X Catalina????" "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1`

`VBoxManage setextradata "?????Virtual Machine Name, in my case Mac OS X Catalina?????" VBoxInternal2/EfiGraphicsResolution ????Your Screen Resolution, in my case 1600x900???`

`VBoxManage setextradata "???Virtual Machine Name, in my case Mac OS X Catalina????" "VBoxInternal2/EfiBootArgs" " "`

`VBoxManage setextradata "????Virtual Machine Name, in my case Mac OS X Catalina????" "VBoxInternal2/EfiBootArgs" "usb=0x800,keepsyms=1,-serial=0x1"`

`VBoxManage setextradata "????Virtual Machine Name, in my case Mac OS X Catalina????" GUI/HidLedsSync "0"`

**********************************
Preinstall Catalina with the Catalina.iso installer (format as APFS and give name to your virtual HardDrive (in my case: SSD_Catalina), and run "Install macOS Catalina") . When finish, it automatically will reboot and the machine will stuck at boot. So "Turn off " the virtual machine.

**Step 4**: Start the machine with Catalina.iso in the virtual CD-ROM and maintain pressed ESC key. This will load the UEFI settings at start.  Choose "BOOT MANAGER" and "UEFI VBOX CD-ROM" so the iso installer will run again 
![UEFI](https://user-images.githubusercontent.com/11962055/74863385-1ac69d80-534e-11ea-80ef-47210fdbab68.png)
![UEFI2](https://user-images.githubusercontent.com/11962055/74863657-89a3f680-534e-11ea-8324-aec17746fc73.png)

In the installer open Terminal (Utilities ---> Terminal) and type:

`diskutil list`

Search and choose your Preboot volume disk identifier, in my case : disk1s2
![Preboot](https://user-images.githubusercontent.com/11962055/74865058-d8eb2680-5350-11ea-8f29-3b9d007bca2f.png)

Type in Terminal your case, (my case is):

`diskutil mount /dev/disk1s2`    

Search your Preboot Folder id name with:

`ls  /Volumes/Preboot`

Bless it (make it bootable) with, in my case:

`bless --folder /Volumes/Preboot/2F74E6DA-173A-4233-B52B-5F5B6E909F83/com.apple.installer --bootefi "/Volumes/Image Volume/System/Library/CoreServices/boot.efi"`

Turn off the machine,  and restart normally. The installation will start. It will take more or less 30 minutes.
![Installing](https://user-images.githubusercontent.com/11962055/74868772-15218580-5357-11ea-891c-f5f5fdd0de57.png)

**Step 5**: When the installation will finish the machine will automatically reboot, and then the boot will stuck again. 
So "Turn off " the virtual machine again. 
Start the machine with Catalina.iso in the virtual CD-ROM and maintain pressed ESC key.
This will load the UEFI settings at start. Choose "BOOT MANAGER" and "UEFI VBOX CD-ROM" so the iso installer will run again.

![UEFI](https://user-images.githubusercontent.com/11962055/74863385-1ac69d80-534e-11ea-80ef-47210fdbab68.png)
![UEFI2](https://user-images.githubusercontent.com/11962055/74863657-89a3f680-534e-11ea-8324-aec17746fc73.png)

In the installer open Terminal (Utilities ---> Terminal) and type:

`diskutil list`

Search and choose your Recovery and Preboot volume disk identifiers, in my case : disk1s3 and disk1s2
![Preboot](https://user-images.githubusercontent.com/11962055/74865058-d8eb2680-5350-11ea-8f29-3b9d007bca2f.png)

Type in Terminal your case, (my case is):

`diskutil mount /dev/disk1s3` 

`diskutil mount /dev/disk1s2`

Search your Preboot Folder id name with:

`ls  /Volumes/Preboot`

Bless it (make it bootable) with, in my case:

`bless --folder /Volumes/Preboot/2F74E6DA-173A-4233-B52B-5F5B6E909F83/com.apple.installer --bootefi "/Volumes/Image Volume/System/Library/CoreServices/boot.efi"`

Search your Recovery  Folder id name with:

`ls  /Volumes/Recovery`

In my case:

`cp -p /Volumes/Preboot/2F74E6DA-173A-4233-B52B-5F5B6E909F83/System/Library/CoreServices/boot.efi /Volumes/Recovery/2F74E6DA-173A-4233-B52B-5F5B6E909F83`

`cp -p /Volumes/Preboot/2F74E6DA-173A-4233-B52B-5F5B6E909F83/System/Library/CoreServices/boot.efi "/Volumes/???In my case, SSD_Catalina????/System/Library/CoreServices"`

**Step 6**:

> OPTION 1: 

Turn off the machine,  and restart, and the Mac will be ready and it  will start and let you create your user account.

END (but the sound does not work by default since macOS High Sierra 10.13.2).

> OPTION 2: Fix the Sound. 

Turn off the machine  and download macOS X High Sierra 10.13.1 AppleHDA.kext inside a virtual hard drive here:
[https://github.com/chapuza/AppleHDA.kext-macOS-High-Sierra-10.13.1-for-VirtualBox/blob/master/VirtBoxSoundMacOS.vdi](url)

Connect VirtBoxSoundMacOS.vdi to the virtual machine and start the machine with ESC key pressed to run the installer again. 

In the installer open Terminal (Utilities ---> Terminal) and type (in my case):

`rm -R "/Volumes/?????In my case,SSD_Catalina/System/Library/Extensions/AppleHDA.kext`

`cp -p -R /Volumes/FixSound/AppleHDA.kext "/Volumes/?????In my case,SSD_Catalina??????/System/Library/Extensions"`

Rebuild kext cache with (in my case): 

`kextcache -i "/Volumes/?????In my case,SSD_Catalina????"`

Turn off the virtual machine  and restart it normally,now the Mac will start with sound, only your chosen language installed, only your chosen language keyboard installed, so you can create your user account listening to Siri asking you if you want to turn on "Accesibility" and "VoiceOver".

Enjoy.
END.
