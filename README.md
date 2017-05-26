# New-NanoServerVHD

Creates a bootable VHD/VHDx containing Windows Server Nano 2016 RTM.

_Note: As of Windows Server 2016 Technical Preview 3, the NanoServer folder in the ISO contains a new-nanoserverimage.ps1 PowerShell script that can also be used to create new Nano Server VHD/VHDx files. This script is the official one provided by Microsoft and so it should be used in any new scripts. I have updated the new-nanoservervhd.ps1 script to support TP3 so that if you have already got scripts using it then you don't have to rewrite them to use the official one (although you probably should)._

## Overview

Creates a bootable VHD/VHDx containing Windows Server Nano 2016 using the publicly available Windows Server 2016 RTM ISO.

Windows Server 2016 RTM ISO can be downloaded from the Microsoft Evaluation Center here: [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016)

This script needs the Convert-WindowsImage.ps1 script to be in the same folder.
**It will be automatically downloaded from GitHub if it is not found:** [https://raw.githubusercontent.com/PlagueHO/New-NanoServerVHD/master/Convert-WindowsImage.ps1](https://raw.githubusercontent.com/PlagueHO/New-NanoServerVHD/master/Convert-WindowsImage.ps1)

This function turns the instructions on the following link into a repeatable script: [https://technet.microsoft.com/en-us/library/mt126167.aspx](https://technet.microsoft.com/en-us/library/mt126167.aspx)

Please see the link for additional information.

This script can be found:

- Github Repo: [https://github.com/PlagueHO/New-NanoServerVHD](https://github.com/PlagueHO/New-NanoServerVHD)
- Script Center: [https://gallery.technet.microsoft.com/scriptcenter/Create-a-New-Nano-Server-61f674f1](https://gallery.technet.microsoft.com/scriptcenter/Create-a-New-Nano-Server-61f674f1)

## Change Log

- 2017-05-27: Updated to support new version of Convert-WindowsImage.ps1.
- 2017-05-27: Corrected readme.md markdown.
- 2016-10-14: Updated to support Windows Server 2016.
- 2016-05-14: Updated to support Windows Server 2016 TP5.
- 2015-12-01: Added WorkFolder parameter to override default work folder path.
- 2015-11-21: Offline Domain Join support added. Fix to adding SCVMM packages.
- 2015-11-20: Ability to cache base NanoServer.VHD/VHDx file to speed up creation of multiple VHD files with different packages/settings.
- 2015-11-20: Added support for Windows Server 2016 TP4.
- 2015-11-13: Added Optional Timezone Parameter. Defaults to 'Pacific Standard Time'.
- 2015-09-18: Added support for setting IP Subnet Mask, Default Gateway and DNS Settings on first boot.
- 2015-08-20: Updated to support packages available in Windows Server 2016 TP3.
- 2015-07-24: Updated setup complete script to create a task that shows the IP Address of the Nano Server in the console window 30 seconds after boot.
- 2015-06-19: Updated to support changes in Convert-WindowsImage on 2015-06-16.
- 2015-06-19: Added VHDFormat parameter to allow VHDx files to be created.
- 2015-06-19: Added Edition parameter (defaults to CORESYSTEMSERVER_INSTALL) so that the Name of the edition in the NanoServer.WIM can be specified.
- 2015-06-19: Because of changes in Convert-WindowsImage, VHDx files are always created using the GPT partition format. VHD files are still created using MBR partition format.
- 2015-06-05: Fix to Unattend.xml to correctly set Server Name in OfflineServicing phase.

## Minimum requirements

- PowerShell 4.0


## License and Copyright

The MIT License (MIT)

Copyright (c) 2016 Daniel Scott-Raynsford

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


## Example Usage

```powershell
.\New-NanoServerVHD.ps1 `
    -ServerISO 'D:\ISOs\Windows Server 2016\14393.0.160715-1616.RS1_RELEASE_SERVER_EVAL_X64FRE_EN-US.ISO' `
    -DestVHD D:\Temp\NanoServer01.vhd `
    -ComputerName NANOTEST01 `
    -AdministratorPassword 'P@ssword!1' `
    -Packages 'Compute','OEM-Drivers','Guest' `
    -Verbose
```

This command will create a new VHD containing a Nano Server machine with the name NANOTEST01. It will contain only the Compute, OEM-Drivers and Guest packages. The IP Address will be configured using DHCP.

```powershell
.\New-NanoServerVHD.ps1 `
    -ServerISO 'D:\ISOs\Windows Server 2016\14393.0.160715-1616.RS1_RELEASE_SERVER_EVAL_X64FRE_EN-US.ISO' `
    -DestVHD D:\Temp\NanoServer01.vhd `
    -ComputerName NANOTEST01 `
    -AdministratorPassword 'P@ssword!1' `
    -Packages 'Storage','OEM-Drivers','Guest' `
    -IPAddress '10.0.0.20' `
    -SubnetMask '255.0.0.0' `
    -GatewayAddress '10.0.0.1' `
    -DNSAddresses '10.0.0.2','10,0,0,3' `
    -Verbose
```

This command will create a new VHD containing a Nano Server machine with the name NANOTEST01. It will contain only the Storage, OEM-Drivers and Guest packages. It will set the Administrator password to P@ssword!1 and set the IP address of the first ethernet NIC to 10.0.0.20/255.0.0.0 with gateway of 10.0.0.1 and DNS set to '10.0.0.2','10,0,0,3'. It will also set the timezone to 'Russian Standard Time'.

```powershell
.\New-NanoServerVHD.ps1 `
    -ServerISO 'D:\ISOs\Windows Server 2016\14393.0.160715-1616.RS1_RELEASE_SERVER_EVAL_X64FRE_EN-US.ISO' `
    -DestVHD D:\Temp\NanoServer02.vhdx `
    -VHDFormat VHDX `
    -ComputerName NANOTEST02 `
    -AdministratorPassword 'P@ssword!1' `
    -Packages 'Storage','OEM-Drivers','Guest' `
    -IPAddress '192.168.1.66' `
    -Timezone 'Russian Standard Time'
    -Verbose
```

This command will create a new VHDx (for Generation 2 VMs) containing a Nano Server machine with the name NANOTEST02. It will contain only the Storage, OEM-Drivers and Guest packages. It will set the Administrator password to P@ssword!1 and set the IP address of the first ethernet NIC to 192.168.1.66/255.255.255.0 with no Gateway or DNS.

```powershell
.\New-NanoServerVHD.ps1 `
    -ServerISO 'D:\ISOs\Windows Server 2016\14393.0.160715-1616.RS1_RELEASE_SERVER_EVAL_X64FRE_EN-US.ISO' `
    -DestVHD D:\Temp\NanoServer03.vhdx `
    -VHDFormat VHDX `
    -ComputerName NANOTEST03 `
    -AdministratorPassword 'P@ssword!1' `
    -Packages 'Compute','OEM-Drivers','Guest','Containers','ReverseForwarders' `
    -IPAddress '192.168.1.66' `
    -DJoinFile 'D:\Temp\DJOIN_NANOTEST03.TXT' `
    -Verbose
```

This command will create a new VHDx (for Generation 2 VMs) containing a Nano Server machine with the name NANOTEST03. It will contain be configured to be a container host. It will set the Administrator password to P@ssword!1 and set the IP address of the first ethernet NIC to 192.168.1.66/255.255.255.0 with no Gateway or DNS. It will also be joined to a domain using the Offline Domain Join file D:\Temp\DJOIN_NANOTEST03.TXT.
