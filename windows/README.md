# notes-infrastructure / windows

## Allow store credentials on remote desktop.

```powershell
New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\LSA" -Name "LsaCfgFlags" -PropertyType "DWORD" -Value 0 -Force
```

## Disable show more options context menu

```shell
reg add HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32 /ve /d "" /f
```

## Disable show recommended files in start, recent files in file Explorer, and items in jump lists

```shell
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoRecentDocsHistory /t REG_DWORD /d 1 /f
```

## Custom Folders View

Personalizar el comportamiento de las vistas de archivos en el Windows Explorer. <https://lesferch.github.io/WinSetView>

![image](https://github.com/user-attachments/assets/aca1780d-2f18-4429-be47-d214a2cb9c9b)

![image](https://github.com/user-attachments/assets/1c6fd6a1-98a5-4836-bfed-e7de3b32bc88)

## Custom Taskbar

Personalizar barra de tareas. <https://github.com/valinet/ExplorerPatcher>

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
