
vicenç Obiol Boter
28/10/2025

# Guia de instalació y utilització de Ldap 

![a](IMG/simpleid.png)

## 1.Actualizar la maquina 
Avans de tot  Haurem de posar la maquina al dia.
```bash
sudo apt update && sudo apt upgrade -y
```
![captura de upgrade](IMG/captura.upgrade.png)

##2. configuracio del server (hostname) 
Per configurar el Host name de la maquina haurem de entrar en el archiu.
```bash
sudo nano /etc/hosts
```
Dindte del harchiu haurem de modificar les dues primeras lineas canviant el nom,
guardem amb control+o i sortim amb control+x

![captura de el hostname](IMG/capturahostname.png)

Per asegurarnos que esta be farem:
```bash
sudo hostname -f
```
![hostname -f](IMG/hostname-f.png)
