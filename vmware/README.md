# notes-infrastructure / vmware

- [Artículos](#artículos)
- [Comandos](#comandos)

---

## Artículos

- [xyz](https://xyz)

---

## Comandos

### Convertir de VMware a HyperV

- instalar [starwind-v2v-converter](https://www.starwindsoftware.com/starwind-v2v-converter)

- convertir disco en formato vmdk a formato vhdx

```powershell
&"C:\Program Files\StarWind Software\StarWind V2V Converter\V2V_ConverterConsole.exe" convert in_file_name="<disco>.vmdk" out_file_name="<disco>.vhdx" out_file_type=ft_vhdx_thin
```

- desinstalar vmware tools utilizando el [script](https://gist.github.com/luiscasalas16/3484d171b08bfa01de4c2a0d3286db0d)

```powershell
.\Remove_VMwareTools.ps1
```
