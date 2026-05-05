# Instalación Windows

## Paso 1

Bypass all Windows 11 requeriments: [https://www.starwindsoftware.com/blog/bypass-tpm-and-install-windows-11-on-unsupported-hardware/]

## Paso 2

- ejecutar windows update

## Paso 3

- activar windows
- ajustar power options, high performance, turn off the display
- ajustar task bar
- ajustar languajes (english, méxico) y keyboard (us y latin america)
- ajustar mode dark, transparency effects, animation effects, background
- ajustar performance options
- ajustar windows explorer
  - show -> file name extensions
  - options -> general -> privacy -> clear
  - options -> view -> expand to open folder
  - options -> view -> show this pc

## Paso 4

```powershell
# install chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

```powershell
# dev software
choco install adobereader -y
choco install azure-cli -y
choco install dbeaver -y
choco install git -y
choco install postman -y
choco install vscode -y
choco install drawio -y
choco install notepadplusplus -y
choco install paint.net -y
choco install powershell-core -y
choco install winmerge -y
choco install winrar -y
choco install lightshot -y
choco install lockhunter -y

# power desginer
# firma digital

Install-Module -Name Az -Repository PSGallery -Force
```

```powershell
# admin software
choco install brave -y
choco install notion -y
choco install spotify -y
choco install telegram -y
choco install vlc -y
```
