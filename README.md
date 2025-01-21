# notes-infrastructure

Este repositorio contiene documentación relacionada con infraestructura.

- [HyperV](/hyperv/README.md)
- [VMware](/vmware/README.md)

```powershell

# install chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# install powershell
choco install powershell-core -y
# install notepad++
choco install notepadplusplus -y
```
