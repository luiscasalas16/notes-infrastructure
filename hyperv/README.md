# notes-infrastructure / hyperv

- [Artículos](#artículos)
- [Comandos](#comandos)

---

## Artículos

- [How to Resize a Hyper-V Virtual Disk](https://www.altaro.com/hyper-v/resize-virtual-hard-disks-hyper-v-2016/)
- [How to Shrink a Hyper-V Virtual Disk](https://www.altaro.com/hyper-v/shrink-hyper-v-virtual-disk-vhd-vhdx/)
- [Why You Should Compact Your Hyper-V Virtual Disk](https://www.altaro.com/hyper-v/compact-hyper-v-virtual-disks-vhdx)

---

## Comandos

### Migrar una máquina virtual de HyperV a Azure

- referencias

  - [Prepare a Virtual Disk to upload to Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image)
  - [Upload a Virtual Disk to Azure](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/disks-upload-vhd-to-managed-disk-powershell)
  - [Create a Virtual Machine from a Virtual Disk](https://learn.microsoft.com/en-us/azure/virtual-machines/attach-os-disk)

- 1. ajustar tamaño de disco de máquina virtual

```powershell
#montar el disco
Mount-VHD -Path "<disco>.vhdx"
#obtener numero de disco virtual (2 normalmente)
Get-Disk
#obtener numero partición de "Recovery" (4 normalmente) y partición "Basic" (3 normalmente)
Get-Partition -DiskNumber 2
#remover partición "Recovery"
Remove-Partition -DiskNumber 2 -PartitionNumber 4
#ajustar partición de "Basic" a su tamaño mínimo
Resize-Partition -DiskNumber 2 -PartitionNumber 3 -Size (Get-PartitionSupportedSize -DiskNumber 2 -PartitionNumber 3).SizeMin
#ajustar disco a tamaño requerido
Resize-VHD -Path "<disco>.vhdx" -SizeBytes 32GB
#ajustar partición de "Basic" a su tamaño máximo
Resize-Partition -DiskNumber 2 -PartitionNumber 3 -Size (Get-PartitionSupportedSize -DiskNumber 2 -PartitionNumber 3).SizeMax
#desmontar disco
Dismount-VHD -Path "<disco>.vhdx"
#verificar disco
Get-VHD -Path "<disco>.vhdx"
```

- 2. ajustar máquina virtual en hyperv, ejecutar powershell como administrador

```powershell
#Run Windows System File Checker
sfc.exe /scannow
#Remove  WinHTTP Proxy
netsh.exe winhttp reset proxy
#Set the disk SAN policy to Onlineall:
diskpart.exe
san policy=onlineall
exit
#Set (UTC) to Windows
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation -Name RealTimeIsUniversal -Value 1 -Type DWord -Force
Set-Service -Name w32time -StartupType Automatic
#Set the power profile to high performance
powercfg.exe /setactive SCHEME_MIN
powercfg /setacvalueindex SCHEME_CURRENT SUB_VIDEO VIDEOIDLE 0
#Set environmental variables TEMP and TMP to default values
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name TEMP -Value "%SystemRoot%\TEMP" -Type ExpandString -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name TMP -Value "%SystemRoot%\TEMP" -Type ExpandString -Force
#Check the Windows services is set to the Windows default value
Get-Service -Name BFE, Dhcp, Dnscache, IKEEXT, iphlpsvc, nsi, mpssvc, RemoteRegistry |
  Where-Object StartType -ne Automatic |
    Set-Service -StartupType Automatic
Get-Service -Name Netlogon, Netman, TermService |
  Where-Object StartType -ne Manual |
    Set-Service -StartupType Manual
#Check the remote desktop registry settings
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -Name fDenyTSConnections -Value 0 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name fDenyTSConnections -Value 0 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name PortNumber -Value 3389 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name LanAdapter -Value 0 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name UserAuthentication -Value 1 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name KeepAliveEnable -Value 1  -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name KeepAliveInterval -Value 1  -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name KeepAliveTimeout -Value 1 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name fDisableAutoReconnect -Value 0 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name fInheritReconnectSame -Value 1 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name fReconnectSame -Value 0 -Type DWord -Force
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name MaxInstanceCount -Value 4294967295 -Type DWord -Force
if ((Get-Item -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp').Property -contains 'SSLCertificateSHA1Hash')
{
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name SSLCertificateSHA1Hash -Force
}
#Configure Windows Firewall rules
Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled True
Enable-PSRemoting -Force
Get-NetFirewallRule -DisplayGroup 'Remote Desktop' | Set-NetFirewallRule -Enabled True
Set-NetFirewallRule -Name FPS-ICMP4-ERQ-In -Enabled True
New-NetFirewallRule -DisplayName AzurePlatform -Direction Inbound -RemoteAddress 168.63.129.16 -Profile Any -Action Allow -EdgeTraversalPolicy Allow
New-NetFirewallRule -DisplayName AzurePlatform -Direction Outbound -RemoteAddress 168.63.129.16 -Profile Any -Action Allow
#Check the disk healthy
chkdsk.exe /f
#Restart
#Check (BCD) settings
cmd
bcdedit.exe /set "{bootmgr}" integrityservices enable
bcdedit.exe /set "{default}" device partition=C:
bcdedit.exe /set "{default}" integrityservices enable
bcdedit.exe /set "{default}" recoveryenabled Off
bcdedit.exe /set "{default}" osdevice partition=C:
bcdedit.exe /set "{default}" bootstatuspolicy IgnoreAllFailures
bcdedit.exe /set "{bootmgr}" displaybootmenu yes
bcdedit.exe /set "{bootmgr}" timeout 5
bcdedit.exe /set "{bootmgr}" bootems yes
bcdedit.exe /ems "{current}" ON
bcdedit.exe /emssettings EMSPORT:1 EMSBAUDRATE:115200
exit
#Enable the dump log collection
# Set up the guest OS to collect a kernel dump on an OS crash event
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name CrashDumpEnabled -Type DWord -Force -Value 2
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name DumpFile -Type ExpandString -Force -Value "%SystemRoot%\MEMORY.DMP"
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name NMICrashDump -Type DWord -Force -Value 1
# Set up the guest OS to collect user mode dumps on a service crash event
$key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
New-ItemProperty -Path $key -Name DumpFolder -Type ExpandString -Force -Value 'C:\CrashDumps'
New-ItemProperty -Path $key -Name CrashCount -Type DWord -Force -Value 10
New-ItemProperty -Path $key -Name DumpType -Type DWord -Force -Value 2
Set-Service -Name WerSvc -StartupType Manual
#Verify that  WMI is consistent
winmgmt.exe /verifyrepository
#Install the Azure Virtual Machine Agent
#https://go.microsoft.com/fwlink/?LinkID=394789
```

- 3. subir disco de máquina virtual en hyperv a azure

```powershell
# Generación 1
Add-AzVhd -Verbose -LocalFilePath "C:\...\virtual machine.vhdx" -ResourceGroupName "lcs16-rg" -Location "eastus" -DiskName "virtual_machine_disk" -Zone 1 -DiskSku "StandardSSD_LRS" -DiskOsType "Windows" -DiskHyperVGeneration "V1"
# Generación 2
Add-AzVhd -Verbose -LocalFilePath "C:\...\virtual machine.vhdx" -ResourceGroupName "lcs16-rg" -Location "eastus" -DiskName "virtual_machine_disk" -Zone 1 -DiskSku "StandardSSD_LRS" -DiskOsType "Windows" -DiskHyperVGeneration "V2"
```

- 4. hacer máquina virtual en azure utilizando disco en portal

- 5. ajustar máquina virtual en azure, ejecutar powershell como administrador

```powershell
#Put the page file on the temporal drive volume
Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -Name PagingFiles -Value 'D:\pagefile.sys' -Type MultiString -Force
```
