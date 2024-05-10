# notes-infrastructure / windows

## Disable Credential Guard

Permitir almacenar credenciales en remote desktop.

```powershell
New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\LSA" -Name "LsaCfgFlags" -PropertyType "DWORD" -Value 0 -Force
```

## Enable or Disable "show more options" context menu

Habilitar o deshabilitar menú contextual de "show more options".

Disable

```cmd
reg add HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32 /ve /d "" /f
```

Enable

```cmd
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f​
```
