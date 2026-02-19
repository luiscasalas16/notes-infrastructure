# notes-infrastructure / windows

## Configuraciones de Windows

## Personalizaciones Básicas de Windows

- display -> dark mode
- windows explorer -> show -> file name extensions
- windows explorer -> options -> general -> privacy -> clear
- windows explorer -> options -> view -> expand to open folder
- windows explorer -> options -> view -> show this pc

## Peronalizaciones Avanzadas de Windows

Personalizar barra de tareas. <https://github.com/valinet/ExplorerPatcher>

### Custom Folders View

Personalizar el comportamiento de las vistas de archivos en el Windows Explorer. <https://lesferch.github.io/WinSetView>

![image](https://github.com/user-attachments/assets/aca1780d-2f18-4429-be47-d214a2cb9c9b)

![image](https://github.com/user-attachments/assets/1c6fd6a1-98a5-4836-bfed-e7de3b32bc88)

### Disable show more options context menu

```shell
reg add HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32 /ve /d "" /f
```

### Disable show recommended files in start, recent files in file Explorer, and items in jump lists

```shell
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" /v NoRecentDocsHistory /t REG_DWORD /d 1 /f
```

### Disable Zip Folders

```powershell
#Disable ZIP Folders
Remove-Item -Path 'Registry::HKEY_CLASSES_ROOT\CompressedFolder\CLSID' -Recurse -Force
Remove-Item -Path 'Registry::HKEY_CLASSES_ROOT\SystemFileAssociations\.zip\CLSID' -Recurse -Force
#Disable CAB Folders
Remove-Item -Path 'Registry::HKEY_CLASSES_ROOT\CABFolder\CLSID' -Recurse -Force
Remove-Item -Path 'Registry::HKEY_CLASSES_ROOT\SystemFileAssociations\.cab\CLSID' -Recurse -Force
```

### Allow store credentials on remote desktop

```powershell
New-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\LSA" -Name "LsaCfgFlags" -PropertyType "DWORD" -Value 0 -Force
```
