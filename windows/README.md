# notes-infrastructure / windows

## Disable Credential Guard

Permitir almacenar credenciales en remote desktop.

```powershell
New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\LSA" -Name "LsaCfgFlags" -PropertyType "DWORD" -Value 0 -Force
```

## Enable or Disable "show more options" context menu

Habilitar o deshabilitar menú contextual de "show more options".

- Disable

  ```powershell
  cmd.exe /c 'reg add HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32 /ve /d "" /f'
  ```

- Enable

  ```powershell
  cmd.exe /c 'reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f​'
  ```

## Custom Folders View

Personalizar el comportamiento de las vistas de archivos en el Windows Explorer. <https://lesferch.github.io/WinSetView>

## Enable or Disable Hyper-V

- Disable

  ```powershell
  cmd.exe /c 'bcdedit /set hypervisorlaunchtype off'
  ```

- Enable

  ```powershell
  cmd.exe /c 'bcdedit /set hypervisorlaunchtype auto'
  ```

## Configuraciones de Windows

- display -> dark mode
- windows explorer -> show -> file name extensions
- windows explorer -> options -> general -> privacy -> clear
- windows explorer -> options -> view -> expand to open folder
