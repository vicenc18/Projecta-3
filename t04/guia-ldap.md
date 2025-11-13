
vicenç Obiol Boter
28/10/2025

# Guia de instalació y utilització de Ldap 

![a](IMG/simpleid.png)

## 1.Actualizar la maquina 

## 1. Actualitzar la màquina  

Abans de tot haurem de posar la màquina al dia.  
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

![yes](IMG/yes.png)

4 A les saguent pantalles aurem de posar: yes yes 

![yes](IMG/yes.png)

![yes](IMG/yes2.png)

5 Per confirmar que tot esta correcte ferem un slapcat

 ```bash
sudo slapcat
```
![captura slapcat](IMG/capturaldap2.png)
 
# creacio de usuaris i grubs

1 En primer lloc haurem de fer els aerchius primer, important fer els archius en ldif 
```bash
sudo nano /etc/OU_users.ldif
sudo nano /etc/OU_groups.ldif
```



![captura de pantalla del archiu](IMG/usuaris.png)

![captura de pantalla del archiu](IMG/groups.png)

# Gestió i Administració (LAM)
1. instalacio de LDAP Acoount manager
   Per instalar el  LDAP Acoount manager haurem de executar:
 ```bash
sudo apt install ldap-account-manager -y
```

![captura de pantalla](IMG/manager.png)    

2 Connectar a LAM des de la màquina física utilitzant l'adreça IP de la interfície Host-Only.

![contraseña](IMG/lamcontraseña.png)

![lam](IMG/lamconfi.png)

Ara configurarem el lam, y lo primer sera configurar el perfil.

![imtege de perfil](IMG/editperfil.png)

![captura de pantalla](IMG/imatgedominilam.png)

Aqui haurem de configurar el domini com es mostra a la seguent imatge:

![ imatge del domini](IMG/dominilam.png)

Despues configurarem la configuracio d'eines:

![captura de ajust d'eines](IMG/capturadeconfiguraciodeeines.png)

## creacio de usuaris i grubs
Hara crearem dos grubs i dos usuaris amb el entorn grafic:

Começem amb la creacio dels grubs que es diran Manager i tech

![creacio dels grubps](IMG/creaciogrubs.png)

![grups creats](IMG/grupslam.png)

Ara aurem de crear el usuaris i asignarols als grubs 

![tech01](IMG/tech01.png)

despues de posar el nom haurem de posar una contrasenya el usuari.

![caprtura de pantalla de contrasenya de usuari](IMG/tech01.png)

Procedim a repetir el proces pro amb mareger01 

![usuaris](IMG/ususariss.png)

i afeigim  una contrasenña en els usuaris


## client (zorin)
### Configuracio del client 
La maquina del client haure de tenir un sol adaptador, que estara en xarxa Nat

![configuracio del client](IMG/xarxanat.png)

Haraconfigurarem el host 

![host](IMG/HOSTSZORIN.PNG)

![host](IMG/HOST2.PNG)

hara instalarem el ldap amb la seguent comande 
```bash
sudo apt install libnss-ldap libpam-ldap ldap-utils nscd -y
```
![instalacio del l'dap](IMG/iclientldap.png)

despues de instalalo l'haurem de configurarlo,

![imatge de la configuracio del l'dap](IMG/conigldap.png)

Despues haurem de posar la contrasenya del administrador, que en el nostre cas sera p@ssw0rd

![contrasenya del administrador](IMG/cna.png)

Be hara haurem de configurar uns archius, el primer sera:

```bash
/etc/pam.d/common-session
```
ha qui haurem de escriure la ultimia linea que surt en la seguent captura:

![captura de pantalla de comoon session](IMG/sessioncommo.png)

El seguent erxiu que executarem sera  el mateix pro cambiant ssesion per pasword:
Aqui lo qe farem sera eliminar la quarta linea comencant per dalt

![archiu](IMG/passwordldap.png)

Avans de de editar el ultim archiu farem un geten  passwd | ail Per que tot estgui correcte 
```bash
sudo geten passwd | ail
```
![gten](IMG/geten.png)

Quan ja em comprobat que tot esta correcta, anem a modificar el ultim arxiu (nsswitch.conf) que serveix per que podem iniciar ssesio amb els usuaris creats amb el entorn grafic

![ultim](IMG/ultim.png)

si tot esta correcte podriem entrar amb el usuaris crats
#fi
