Vicenç Obiol Boter
6/11/25

# T07 Guia servei DNS 

![imatge dns ](IMG/servidoresDNS.jpg)

## configuracio inizial 
### 1 configuracio del domini 
Lo primer que hurem de fer és entrar el Aechiu per canviar el domini.
``` bash
sudo nano /etc/hosts
```
![captura de hosts](IMG/hosts.png)

El nom del domini es la linea que esta avaig de localhost, en el nostre cas posarem server.digicore-18.test server.
Hara haurem de sortir del archiu pro avans guardarem amb control + o i sortim amb control+x

Per copmprobar que ho tenom ve domes executem
``` bash
sudo hostname -f
```
![hostname -f](IMG/Hostname.png)

### 2 configuracio de la xarxa 
Lo primer sera tenir dos port activat en la MV un en NAT i l'altra en PONT


![nat](IMG/nat.png)

![pont](IMG/PONT.png)
