
![logo de la escola](IMG/logopia.png)

# T06: Fundamentos del servicio DNS

**Autores:**  
Vicenç Obiol  

**Fecha:**  
23/10/2025  

![portad de la activitat](IMG/dnsimg1.png)

---

## Comando 1: Consulta Básica de Registro A

**Ejecuta:**  
```bash
dig xtec.cat A
```
![captura de la execucio de la comande](IMG/captura1.png)

## Anàlisi

1. **IP de resposta:**
   - La IP associada al domini **xtec.cat** és:
     - `83.247.151.214`

2. **Valor TTL:**
   - El TTL (Time To Live) és **3249 segons**.
     - Això indica que la resposta es mantindrà en la memòria cau durant **3249 segons** abans de ser considerada obsoleta i es necessiti una nova consulta per actualitzar la informació.

3. **Servidor que ha respost a la consulta:**
   - El servidor que ha respost a la consulta és:
     - `127.0.0.53#53` (Servidor DNS local)
   - Aquesta IP `127.0.0.53` és una adreça local (normalment utilitzada per systemd-resolved en sistemes Linux per a la resolució DNS local), i utilitza el port **53** (el port estàndard per a DNS).

## Comando 2: Consulta de Servidors de Noms (NS)
**Ejecuta:**  
```bash
 dig tecnocampus.cat NS

```
![captura de pantalla de la segona comande](IMG/captura2.png)

## Anàlisi
A la secció “ANSWER SECTION” apareixen els servidors de noms autoritatius per al domini tecnocampus.cat. Són aquests quatre:

1. ns-535.awsdns-02.net.

2. ns-1071.awsdns-05.org.

3. ns-130.awsdns-16.com.

4.ns-1689.awsdns-19.co.uk.
