# Instalación Windows

## Paso 1

Para instalar sin cuenta de correo:

- desconectar red
- shift + f10
- oobe\bypassnro

## Paso 2

- activar windows
- cambiar computer name
- cambiar power plan
- cambiar task bar
- cambiar dark mode
- cambiar background
- deshailitar transparency effects
- deshailitar animation effects
- cambiar performance options

## Paso 3

- ejecutar windows update

## Paso 4

```powershell
# install chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

```powershell
choco install adobereader -y
choco install brave -y
choco install drawio -y
choco install lightshot -y
choco install lockhunter -y
choco install notepadplusplus -y
choco install notion -y
choco install paint.net -y
choco install powershell-core -y
choco install spotify -y
choco install telegram -y
choco install vlc -y
```

```powershell
choco install azure-cli -y
choco install dbeaver -y
choco install git -y
choco install postman -y
choco install vscode -y
choco install nodejs --version="20.18.2" -y
npm install -g @angular/cli@18.2.0
```
