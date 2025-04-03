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
- cambiar transparency effects
- cambiar background

## Paso 3

- ejecutar windows update

## Paso 4

- instalar winrar
- instalar office, 2024, x64x, en-us

## Paso 5

```powershell
# install chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# brave
choco install brave -y
# lightshot
choco install lightshot -y
# paint.net
choco install paint.net -y

# drawio
choco install drawio -y
# notepad++
choco install notepadplusplus -y

# powershell
choco install powershell-core -y
# git
choco install git -y
# azure-cli
choco install azure-cli -y
# postman
choco install postman -y
# node
choco install nodejs --version="20.18.2" -y
# angular
npm install -g @angular/cli@18.2.0
```

## Paso 6

- configurar brave
