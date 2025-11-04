
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

##3. Interfície de Xarxa Pública

NAT (Per accés a Internet i descàrrega de paquets).

![captura de pantalla del adaptador point ](IMG/capturaxarxanat.png)

##3.1 Interfície de Xarxa Privada.

Adaptador point nomes en anfritio (Per a comunicació privada amb el Client virtual  i la màquina física).

![Adaptador point nomes en anfritio](IMG/nomesenanfritio.png)

# Instalació i configuracio ldap

## Instalació 
1. Per Instal-lar aquest programa haurem de executar la seguent comande
 ```bash
sudo apt install sldap ldap-untils -y
```

![captura intalar ldap](IMG/sldap.png)

1.2 verificar amb slapcat

 ```bash
sudo slapcat
```

![captura slapcat](IMG/capturaldap2.png)
 
## configuracio 
1 Haurem de afeguir una contrasenya que sera p@ssw0rd

![captura contraseña](IMG/capturacontraseña.png)

2.Posar el nom del domini dns: el enl nostre poserem innovatech18

![captura del domini](IMG/capturadominidnd.png)

3 Nom de la organitzacio: sera el mateix que el domini 

![captura de la organitzacio](IMG/nombreorganizazion.png)

4 A les saguent pantalles aurem de posar: yes yes 
