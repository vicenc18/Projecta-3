# Guia Completa de LVM amb Discos Virtuals a VirtualBox (Markdown)

Aquesta guia està totalment adaptada al format **Markdown**.

---

## 📌 1. Creació dels Discos Virtuals de 10 GB a VirtualBox

### **1.1. Pantalla inicial d'emmagatzematge**

Aquesta imatge mostra els discs existents assignats al controlador SATA:

![Controlador SATA](IMG/volgrup3.png)

(Jo no tinc res a veure amb l'ho de Carlitos)

---

### **1.2. Creació d'un nou disc virtual de 10 GB

Assegura't que la mida és exactament **10 GB**.

![Creació VDI 10GB](IMG/25.png)

---

---

## 📌 2. Procediment Correcte per Crear els 4 Discos de 10 GB

1. Obrir: Configuració → Emmagatzematge.
2. Seleccionar **Controlador SATA**.
3. Clicar: **Afegeix disc dur → Crea**.
4. Configurar:

   * Tipus: VDI
   * Mode: Dynamically allocated
   * Mida: **10 GB**
5. Repetir el procés fins tenir **4 discos de 10 GB**.

---

## 📌 3. Configuració Inicial d'LVM (PV + VG + LV)

### **3.1. Crear particions LVM**
bash
sudo fdisk /dev/sdb
sudo fdisk /dev/sdc
Funció: Obrir l’eina de particionat per crear particions noves.
Acció important: Cal assignar el tipus 8e (Linux LVM).


### **3.2. Crear volums físics (PV)**

![volgrub](IMG/pvcrate.png)
bash
sudo pvcreate /dev/sdb1 /dev/sdc1
Funció: Crear Physical Volumes (PV), és a dir, marcar les particions com a compatibles amb LVM.

### **3.3. Crear grup de volums (VG)**
bash
sudo vgcreate vg_dades /dev/sdb1 /dev/sdc1
Funció: Crear un Volume Group (VG), que combina diversos PV en un sol pool d’emmagatzematge.

### **3.4. Crear volum lògic (LV)**
![l](img/L.png)
bash
sudo lvcreate -L 5G -n lv_dades vg_dades
Funció: Crear un Logical Volume (LV) dins del VG.
És com crear una “unitat virtual” amb la mida desitjada.

### **3.5. Formatar i muntar**
sudo mkfs.ext4 /dev/vg_dades/lv_dades
Funció: Donar format ext4 al volum lògic.
sudo mkdir /mnt/dades
sudo mount /dev/vg_dades/lv_dades /mnt/dades
Funció: Crear punt de muntatge i muntar el LV al sistema.

---

## 📌 4. Alta Disponibilitat: Mirall LVM


bash
sudo lvconvert --type mirror -m1 vg_dades/lv_dades
Funció: Convertir un LV normal en un LV mirall (redundat).
-m1 = 1 còpia extra (mirall doble).

---

## 📌 5. Instantànies (Snapshots)

### **5.1. Afegir dos discos nous i ampliar el VG**
sudo pvcreate /dev/sdd1 /dev/sde1
Crea PV nous sobre nous discos.
sudo vgextend vg_dades /dev/sdd1 /dev/sde1
Afegeix els PV al VG ja existent.

### **5.2. Crear un LV per dades**
sudo lvcreate -L 8G -n lvm_dades vg_dades
Crea un volum lògic nou de 8 GB.
sudo mkfs.ext4 /dev/vg_dades/lvm_dades
Formata el volum.
sudo mkdir /mnt/lvm_dades
sudo mount /dev/vg_dades/lvm_dades /mnt/lvm_dades
Crea el punt de muntatge i el munta.


### **5.3. Afegir arxius**
wget https://picsum.photos/200/300 -O /mnt/lvm_dades/foto1.jpg
wget https://picsum.photos/400/500 -O /mnt/lvm_dades/foto2.jpg
Descarrega imatges i les desa dins el LV.

### **5.4. Crear snapshot**
bash
sudo lvcreate -L 2G -s -n lv_snapshot /dev/vg_dades/lvm_dades
Funció: Crear un snapshot (còpia temporal) del LV.

### **5.5. Restaurar snapshot**
sudo umount /mnt/lvm_dades
sudo lvconvert --merge /dev/vg_dades/lv_snapshot
Funció: Fusiona el snapshot per tornar l’LV a l’estat anterior.

---

## 📌 6. Escalabilitat: Ampliar LV

### **6.1. Ampliar volum**
sudo lvextend -L +2G /dev/vg_dades/lvm_dades
Funció: Afegir 2 GB extra al volum lògic.


### **6.2. Redimensionar FS**
bash
sudo resize2fs /dev/vg_dades/lvm_dades
Funció: Ampliar el sistema de fitxers perquè ocupi el nou espai del LV.

# **Guia: Gestió d’emmagatzematge amb Storage Spaces a Windows 11**

---

## 2.1 Creació del Pool d’emmagatzematge

### **Objectiu**
Crear un *Storage Pool* utilitzant tres discos virtuals de 10 GB de manera inicial com a base per a la resta de configuracions.

### **Procediment Creació Maquina**
1. Crear la maquina amb **4 GB de RAM** i **2 processadors**.

<img src="IMG/hardware.png" width="600">

3. Anem a l'apartat **d'Emmagatzematge** i a l'opcio de **Controlador: SATA** seleccionem l'opcio de **Afegeix disc dur** hi ha **Crea**.

<img src="IMG/controladorSATA.png" width="600">

<img src="IMG/creadiscdur.png" width="600">
 
4. Canviem el nom a disc01 o similar i assignem **10 GB d'espai**.

<img src="IMG/espaidiscos.png" width="600">
   
6. Creem i l'escollim. I repetim aquest proces dos cops mes per tenir el pool inicial.

<img src="IMG/pool.png" width="600">

### **Captura de pantalla resultat final**

<img src="IMG/maquina3.png" width="600">

### **Procediment Creació d'un Grup**

#### Objectiu
Apendre a com crear un grup d'emmagatzematge

1. Entrem a la maquina, obrim **Tauler de control → Sistema i seguretat → Espais d’emmagatzematge**.

<img src="IMG/taulerdecontrol.png" width="600">

<img src="IMG/sistemaiseguredad.png" width="600">

<img src="IMG/configuracioalmacenamiento.png" width="600">

2. Seleccionem **Crea un grup nou i un espai d’emmagatzematge nou**.

<img src="IMG/creargrup.png" width="600">

3. Seleccionar els discos necesaris de **10 GB cadascun** i creem el grup.

<img src="IMG/selecciomirroging.png" width="600">

---

## 2.2 Resiliència de mirall doble (Two-Way Mirror)

### **Objectiu**
Configurar un espai amb **mirall doble** per garantir la disponibilitat de les dades en cas de fallada d’un disc.

### **Procediment**
Discos necesaris:

<img src="IMG/A.png" width="600">


#### 1. Entrem al **"Administrador de discos"** e inicialitzarem un disc seleccionan els 3 discos que hem creat anteriorment utilitzant l'estil de particio **GPT**.

<img src="IMG/B.png" width="600">

#### 2. Ara entrem a **"Espacios de almacenamiento"** i crearem un nou grup i espai d'emmagetzematge

<img src="IMG/C.png" width="600">

#### 3. Seleccionem dos discos i creem el grup.

<img src="IMG/D.png" width="600">

#### 4. Configurar de la següent manera:
   - Tipus de resiliència: `Mirall doble (Reflejo doble)`  
   - Mida: `10 GB`  
   - Lletra d’unitat: `E:`

Com a de quedar:

<img src="IMG/E.png" width="600">

Un cop el tinguem configurat creem el grup.

<img src="IMG/F.png" width="600">


#### 5. Copiar alguns fitxers de prova a la unitat `E:`.

<img src="IMG/G.png" width="600">

#### 6. Deshabilitar un dels discos del pool per **simular una fallada**, per aixo tancarem la maquina i eliminarem un disc del pool

<img src="IMG/H.png" width="600">

<img src="IMG/J.png" width="600">


#### 8. Verifiquem que els fitxers continuen accessibles.

<img src="IMG/I.png" width="600">

#### 10. Tornar a habilitar el disc i comprovar la **reconstrucció automàtica**, per aixo anem a **"Espacios de almacenamiento"**, anem al grup i seleccionem l'opcio de afegir un disc al grup.


<img src="IMG/K.i.png" width="600">


<img src="IMG/L.png" width="600">

- Eliminem del grup el disc que te la fallada, per aixo tindrem que esperar un rato fins que hens sorti l'opcio de **"Quitar"**.

<img src="IMG/M.png" width="600">

<img src="IMG/N.png" width="600">

<img src="IMG/O.png" width="600">

- I finalment comprovem que la recontruccio s'aixi fet correctament, que es pot veure que s'ha fet correctament, ja que al afegir el tercer disc aquest a copiat tot el fitxers que tenia el primer, utilitzant el mateix espai de disc que el primer.

<img src="IMG/P.png" width="600">


---

## 2.3 Resiliència de paritat

### **Objectiu**
Crear un espai amb **resiliència de paritat** per mantenir les dades amb menys espai dedicat a còpies.

### **Procediment**
Discos que utilitzarem:

<img src="IMG/AP.png" width="600">

#### 1. Igual que abans anem a l'apartat de **"Espacios de almacenamiento"**, creem un nou grup i ara seleccionarem 3 discos.

<img src="IMG/BP.png" width="600">

#### 2. Configurem el grup de la següent manera:  
   - Tipus: `Paritat`  
   - Mida: `18 GB`  
   - Lletra d’unitat: `E:`

Com a de quedar: 

<img src="IMG/CP.png" width="600">

Un cop tenim a configuracio correcta creem el grup.

<img src="IMG/DP.png" width="600">

#### 3. Copiar fitxers de prova a la unitat.

<img src="IMG/EP.png" width="600">

#### 4. Deshabilitar un disc per simular una fallada, per aixo tanquem la maquina i eliminem un dels discos del pool.

<img src="IMG/FP.png" width="600">

<img src="IMG/HP.png" width="600">


#### 6. Verificar que les dades segueixen accessibles tot i la fallada.

<img src="IMG/GP.png" width="600">

#### 8. Reactivar el disc i comprovar la reconstrucció. Tanquem la maquina, afegim un nou disc al pool, obrim la maquina, anem a l'apartat **"Espacios de almacenamiento"**, al grup de paritat i seleccionem l'opcio de afegir un disc al grup.

<img src="IMG/IP.png" width="600">

<img src="IMG/JP.png" width="600">

- En aquesta captura es pot veure com el disc que acabem d'afegir ja s'esta utilitzant, recronstuint la informació que tenia el disc de la fallada:

<img src="IMG/KP.png" width="600">

- Eliminem el disc amb la fallada:

<img src="IMG/LP.png" width="600">

<img src="IMG/MP.png" width="600">

- I comprovem que la informació esta correcta:

<img src="IMG/NP.png" width="600">

---

## 2.4 Resiliència de mirall triple (Three-Way Mirror)

### **Objectiu**
Configurar un espai amb **mirall triple** per protegir les dades davant la fallada simultània de dos discos.

### **Procediment**
Discos que utilitzarem:

<img src="IMG/AT.png" width="600">

#### 1. Creem un nou grup en el qual seleccionarem 5 discos.

<img src="IMG/BT.png" width="600">

#### 3. La configurem de la seguent manera:  
   - Tipus: `Mirall de tres vies (Three-way mirror)`  
   - Mida: `10 GB`  
   - Lletra d’unitat: `E:`
Com a de quedar:

<img src="IMG/CT.png" width="600">

<img src="IMG/DT.png" width="600">

#### 2. Copiar fitxers de prova a la unitat.

<img src="IMG/ET.png" width="600">
  
#### 4. Deshabilitar un disc per simular una fallada (aquest podria soportar fins a dues fallades de disc). El proces es el mateix que els dos anteriors, apagar maquina, eliminem disc del pool.

<img src="IMG/GT.png" width="600">

#### 6. Verificar que els fitxers continuen accessibles.

<img src="IMG/FT.png" width="600">

#### 8. Reactivar els discos i comprovar la reconstrucció. apaguem la maquina, afegim un disc nou al pool, obrim la maquina, anem al grup i afegim el disc al grup.

<img src="IMG/HT.png" width="600">

<img src="IMG/IT.png" width="600">

<img src="IMG/JT.png" width="600">

<img src="IMG/KT.png" width="600">

---

## Conclusió

Els resultats mostren que:
- El **mirall doble** ofereix protecció davant una fallada individual.  
- La **paritat** és més eficient en ús d’espai, tot i que més lenta en reconstrucció.  
- El **mirall triple** proporciona el màxim nivell de seguretat davant múltiples fallades.

En conclusió, **Storage Spaces** és una eina molt útil per a la gestió d’emmagatzematge amb redundància dins d’un entorn Windows professional.

---

Click aqui per anar a [README](README.md)

Click aqui per anar a [HOME](..)






