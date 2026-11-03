# Cerințe - ProiectSDI

## 1. Descriere generală

ProiectSDI este un sistem distribuit de stocare a fișierelor dezvoltat în Java. Sistemul este compus dintr-un coordonator, mai multe noduri de stocare și un client care permite gestionarea fișierelor distribuite în rețea.

Scopul principal al proiectului este asigurarea stocării fișierelor într-un mod fiabil și tolerant la defecte prin replicarea datelor pe mai multe noduri.

---

## 2. Operațiile sistemului

### put

Încarcă un fișier în sistem.

#### Sintaxă

```text
put <cale_fisier>
```

#### Comportament

- Clientul trimite cererea coordonatorului.
- Coordonatorul selectează nodurile de stocare.
- Fișierul este transferat către nodurile selectate.
- Metadatele sunt actualizate.

---

### get

Descarcă un fișier din sistem.

#### Sintaxă

```text
get <nume_fisier>
```

#### Comportament

- Coordonatorul identifică locația fișierului.
- Clientul descarcă fișierul de pe unul dintre nodurile disponibile.

---

### ls

Afișează toate fișierele disponibile în sistem.

#### Sintaxă

```text
ls
```

#### Comportament

- Coordonatorul returnează lista fișierelor cunoscute.

---

### rm

Șterge un fișier din sistem.

#### Sintaxă

```text
rm <nume_fisier>
```

#### Comportament

- Coordonatorul identifică toate replicile fișierului.
- Fișierul este șters de pe toate nodurile.
- Metadatele sunt actualizate.

---

### nodes

Afișează nodurile active din sistem.

#### Sintaxă

```text
nodes
```

#### Comportament

- Coordonatorul returnează lista nodurilor active.
- Sunt afișate informații despre fiecare nod.

---

## 3. Modelul metadatelor

Pentru fiecare fișier se păstrează următoarele informații:

```text
FileMetadata
{
    filename
    size
    uploadTimestamp
    replicas
}
```

### Descriere

- filename – numele fișierului;
- size – dimensiunea fișierului;
- uploadTimestamp – data și ora încărcării;
- replicas – lista nodurilor care conțin replici.

Pentru fiecare nod:

```text
NodeMetadata
{
    nodeId
    address
    port
    status
}
```

### Descriere

- nodeId – identificator unic al nodului;
- address – adresa IP;
- port – portul de comunicație;
- status – ONLINE sau OFFLINE.

---

## 4. Cerințe nefuncționale

### Disponibilitate

Sistemul trebuie să permită accesul la date chiar dacă un nod devine indisponibil.

### Fiabilitate

Fiecare fișier trebuie să fie replicat pe minimum două noduri.

### Scalabilitate

Trebuie să fie posibilă adăugarea de noi noduri fără oprirea sistemului.

### Performanță

Transferul fișierelor trebuie realizat prin conexiuni TCP directe între client și nodurile de stocare.

### Portabilitate

Aplicația trebuie să poată fi executată pe orice sistem care suportă Java 17 sau versiuni mai noi.

---

## 5. Protocolul de comunicație

### Descoperirea nodurilor

Se utilizează UDP Multicast pentru:

- anunțarea nodurilor noi;
- identificarea coordonatorului;
- transmiterea mesajelor heartbeat.

### Transferul fișierelor

Se utilizează TCP pentru:

- upload fișiere;
- download fișiere;
- ștergere fișiere;
- sincronizarea replicilor.

### Alegerea coordonatorului

Se utilizează algoritmul Bully.

Pași:

1. Nodurile detectează lipsa mesajelor heartbeat.
2. Este inițiat procesul de alegere.
3. Nodul cu ID-ul cel mai mare devine noul coordonator.

---

## 6. Scenariul demonstrației

### Pasul 1

Pornirea coordonatorului.

### Pasul 2

Pornirea a trei noduri de stocare.

### Pasul 3

Pornirea clientului.

### Pasul 4

Încărcarea unui fișier:

```text
put test.txt
```

### Pasul 5

Listarea fișierelor disponibile:

```text
ls
```

### Pasul 6

Afișarea nodurilor active:

```text
nodes
```

### Pasul 7

Oprirea unui nod de stocare.

### Pasul 8

Descărcarea fișierului:

```text
get test.txt
```

Se verifică funcționarea mecanismului de replicare.

### Pasul 9

Ștergerea fișierului:

```text
rm test.txt
```

### Pasul 10

Oprirea coordonatorului și alegerea automată a unui nou coord
