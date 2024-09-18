![Screenshot 2024-09-18 at 2 54 53 PM](https://github.com/user-attachments/assets/38907c7d-a968-4a8d-92d7-3398b73c88cf)

# 🔧 OpenCore EFI for Gigabyte Z790 GAMING X AX
👉 This repository contains OpenCore EFI configuration files for the Gigabyte Z790 GAMING X AX motherboard, designed to run macOS Sequoia 15.0 on an Intel® Core™ i5-13600KF CPU and a Radeon™ RX 6600 XT GPU.

## ✅ What works
- Core MacOS features
- Handoff*
- Continuity Camera*
- AirDrop*
- Sidecar*
- Universal Control*
- Wi-Fi and Bluetooth
- 2.5GbE Ethernet
- Sleep / Wake
- Power Nap (I turned this off though)
- Most USB ports (MacOS limits to 15 total)
- Very stable. Rarely crashes, good temps with overclock enabled, quiet.

`* = Requires Fenvi T919 BT/Wi-Fi Card`


## 💿 Preparing the installer
1. Download this repository—you need the contents of the EFI folder.
1. Download MacOS Sequoia using [gibMacOS](https://github.com/corpnewt/gibMacOS). 15.0 is the most recent as of this guide.
1. Copy the installer to a USB 2.0 flash drive using [createinstallmedia](https://support.apple.com/en-us/101578)
1. Mount the USB's `EFI` partition (`diskutil list`, then `sudo diskutil mount /dev/diskXsX`)
1. Copy the whole EFI folder itself into the EFI partition.
1. Generate your own iMacPro1,1 SMBIOS using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS).
   - You need to do this in order to get a serial number (etc.) that can work with iCloud and Apple's services. Make sure you apply the changes to the `config.plist` in your USB's `EFI` partition.
1. Generate your own CPUFriendDataProvider.kext using [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend) and replace the one in the Kexts folder.
1. Unmount the USB—it's ready to install MacOS.


## 🔧 Preparing the BIOS

#### 🚫 Disable
- Fast Boot
- Secure Boot
- VT-d
- Compatibility Support Module (CSM)
- Intel Platform Trust
- CFG Lock (MSR 0xE2 write protection)

#### ✅ Enable
- VT-x
- Hyper-Threading
- EHCI/XHCI Hand-off
- OS type: Other OS
- Above 4G Decoding
- Resizable BAR


## 📺 Installing Windows (optional)
I suggest doing this first. Make sure you create separate partitions (or, ideally, use separate NVMe drives entirely) for MacOS and Windows.


## 🖥️ Installing MacOS
1. Insert the USB drive into a **USB 2.0 port** on your computer. Sometimes USB 3.0 ports work fine, but occasionally I ran into odd issues running both the Windows and Mac installers from USB 3.0 ports.
1. Boot from USB. OpenCore loader should appear. Choose "Install MacOS Sequoia"
1. With any luck, the MacOS installer will load. If you have issues...
   - Try adding `-v` under `boot-args` in `config.plist` to see verbose output (you can also get this by pressing Ctrl+V in OpenCore loader)
   - Google and ChatGPT are your friends
   - TonyMacX86, Extremelymac, and Reddit have been immensely helpful for me in debugging.
1. Open Disk Utility and create a **Macintosh HD** partition to install MacOS onto.
1. Run the installer. Your system will reboot a few times. Make sure you choose the **MacOS Installer** boot option and not the USB. It should look like a hard drive.
1. On the final reboot, choose **Macintosh HD** to boot from.
1. Hold your breath. If it boots, hooray! If not, try the troubleshooting methods mentioned above.
1. Mount the USB's `EFI` partition (`diskutil list`, then `sudo diskutil mount /dev/diskXsX`) and copy the EFI folder to your desktop. Unmount it.
1. Mount your hard drive's `EFI` partition (`diskutil list`, then `sudo diskutil mount /dev/diskXsX`) and copy the EFI folder to it.
   - In your BIOS, choose `OpenCore` as your primary boot option. OpenCore should pick up Windows automatically.
  
## 🧹 Post-install
1. Follow [this guide](https://github.com/Edwardwich/BCM-WIFI-Sequoia) to get Wi-Fi working in Sequoia. **Don't follow the "One Key OCLP" instructions** they were meant for the Sequoia beta only. OCLP 2.0.0 and later support Sequoia out of the box.
  

## 🗼 My build
- Gigabyte Z790 Gaming X AX motherboard
- Intel i5-13600KF
- Sapphire PULSE Radeon 6600 XT 8GB
- G.SKILL Ripjaws S5 Series 64GB (2 x 32GB) DDR5 6000
- 2x 1TB WD_BLACK SN850X (one for MacOS, one for Windows)
- Fractal Design North case
- fenvi T919 PCI-E WiFi Adapter
- Thermalright CPU Contact Frame for LGA 1700
- be quiet! Dark Rock Pro 4 250W TDP CPU Cooler
- Seasonic Focus GX-850 power supply


## 🙏 Guides and articles that helped me:
- https://www.reddit.com/r/hackintosh/comments/17ou09j/successfully_built_a_hackintosh_with_i513600k_msi/
- https://github.com/scotthowson/ROG-STRIX-Z790-A-GAMING-WIFI-Intel-i9-13900k-RX-6800-XT-OpenCore-1.0.1?tab=readme-ov-file
