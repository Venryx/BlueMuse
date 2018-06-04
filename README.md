# BlueMuse
* Windows 10 app to stream data from Muse EEG headsets via LSL (Lab Streaming Layer).

# Features
* Auto detects Muse headsets and provides a visual interface to open and close data streams. 
* Can stream from multiple Muses simultaneously.
* Shows latest timestamp received and the current sample rate for each stream.

# Command Line Interface
**All commands will launch BlueMuse if it isn't already open.**

Start BlueMuse
```powershell
start bluemuse:
```

Start streaming first Muse found: 
```powershell
start bluemuse://start?streamfirst=true
```
Start streaming specific Muse(s) - by MAC address or device name: 
```powershell
start bluemuse://start?addresses={MAC1 or Name1},{MAC2 or Name2},{MAC3 or Name3}....
```
Start streaming all Muses: 
```powershell
start bluemuse://start?startall
```
Stop streaming specific Muse(s) - by MAC address or device name: 
```powershell
start bluemuse://stop?addresses={MAC1 or Name1},{MAC2 or Name2},{MAC3 or Name3},....
```
Stop streaming all Muses: 
```powershell
start bluemuse://stop?stopall
```

Close the program: 
```powershell
start bluemuse://shutdown
```

**"startall" and "stopall" are not meant for launch, they are used when BlueMuse is already running.**

# Installation
1. Download latest version from the /Dist folder and unzip or find your desired version in BlueMuse/AppPackages folder.
2. Double click BlueMuse_xxx.cer then click "Install Certificate".
3. Select current user or local machine depending on preference and click "Next".
4. Select "Place all certificates in the following store".
5. Press "Browse...".
6. Select install for Local Machine.
7. Select "Trusted Root Certification Authorities" and click "OK".
8. Click "Next" and click "Finish" to install certificate.

9. Open Dependencies folder and appropriate folder for your machine architecture.
10. Double click and install Microsoft.NET.Native.Framework.1.7 and Microsoft.NET.Native.Runtime.1.7.

11. Finally, double click and install BlueMuse_xxx.appxbundle.

# Versions
#### Latest
* 1.0.6.0 - stable. 
    * Changed timestamp format to Unix epoch **seconds** format.
    * Improved UI - it is now re-sizable and more compact (better for low resolution screens).
    * Added version number to main screen.

#### Older
* 1.0.5.0 - stable. 
    * Corrected timestamps timezone issue (timestamps were meant to be GMT based, but were actually in EST). Timestamps formatted as Unix epoch **milliseconds**.
* 1.0.4.0 - stable. 
    * LSLBridge is auto hidden if no streams active. BlueMuse also polls to keep LSL bridge open if not currently streaming, therefore LSLBridge has proper auto closing mechanism that won't prematurely trigger. This process may seem strange and convoluted but it appears to be the only good method to manage this trusted process with the current Windows UWP API.
* 1.0.3.0 - unstable. 
    * Issues with LSLBridge closing.


# Notes
* **Requires Windows 10 with Fall 2017 Creators Update - Version 10.0.15063 aka Windows 10 (1703).**
* Application requires side loading a Win32 application which does the LSL streaming. This is because UWP apps run in a restricted environment with network isolation. This restricts LSL streams from being seen across the local network if launched from the  UWP app. To get around this issue, the data is shuffled through to the "LSL Bridge", a Win32 application which can run in a normal environment. Note: when you first start a stream, you may need to add a firewall exception for LSLBridge.exe.
* Uses 32-bit binaries for LSL. Acquired from: ftp://sccn.ucsd.edu/pub/software/LSL/SDK/liblsl-All-Languages-1.11.zip
* liblsl32.dll was dependent on MSVCP90.dll and MSVCR90.dll, both of which I included in the project since these may not be available in the System32 folder on your machine (they weren't on mine).
* The full dependencies of liblsl32.dll are: KERNEL32.dll, WINMM.dll, MSVCP90.dll, WS2_32.dll, MSWSOCK.dll, and MSVCR90.dll. Generated with dumpbin utility.

# Troubleshooting
### If your Muse is not showing up after searching for awhile: 
  1. Ensure Muse is removed from "Bluetooth & other devices" list in control panel.
  2. Reset Muse - hold down power button until device turns off then back on.
  3. Make sure Muse is within reasonable range of your computer. Some built in Bluetooth antennas are not very powerful.

### If working on VS Solution - missing references in LSLBridge project:
See https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-enhance
