---
layout: post
title:  "Sta je sticky bit i kako ga koristiti"
date:   Sun 06 Nov 2022 08:35:24 AM CET
categories: blog
---
Sticky bit setuje na folderu i sadrzaju foldera koji se koristi od strane vise usera, u cilju prevencije brisanje od strane bilo kojeg usera koji nije kreator foldera/sadrzaja foldera ili root.
Primjer toga `/tmp` folder kreiran od strane Linux sistema, koristen od strane vise Linux usera za kreiranje privremenih dokumenata.
Sta ako bi neko slucajno izbrisao ili promijenio ime dokumenta kreiranog od strane nekog drugog korisnika u ovom direktoriju?
U cilju prevencije ovakve situacije,postoji koncept sticky bit nad direktorijem.
To je permisija setovana nad dokumentom ili direktorijem koji omogucava jedino kreatoru istih ili root-u da obrise ili promijeni ime.

## Primjer kreiranje sticky bit foldera
- Napravi folder i dokument u folderu
```bash
$ mkdir test_folder && touch test_folder/file
```
- Pregled permisija nad folderom
```bash
 ls -ld test_folder/
drwxr-xr-x 2 anel dell 4096 Nov  6 08:39 test_folder/
```
Kreator (owner) ima permisije nad folderom `d` prikazane kao `rwx` (int 7), grupa `r-x` (int 5), te ostali useri `r-x` (int 5).
Zadatak kakve su permisije file-a ?
```bash
$ ls -ld test_folder/file 
-rw-r--r-- 1 anel dell 0 Nov  6 08:39 test_folder/file
```
- Setuj sticky bit nad folderom i prikazi permisije
```bash
$ chmod +t test_folder/
$ ls -ld test_folder/
drwxr-xr-t 2 anel dell 4096 Nov  6 08:39 test_folder/
```
Setovanje sticky bit-a se radi komandom `chmod` sa setovanjem `+t` flaga.
Primijetiti da je dodan `t` znak u permisijama foldera.
### Zadatak
1. Kreirati folder  `/tmp/user_1_folder` i file u direktoriju `/tmp` kao user1 i setovati sticky bit nad folderom.
2. Kreirati usera 2. i logovati se
3. Pokusati kreirati file u `/tmp` direktoriju. Radi li? Trebalo bi.
4. Pokusati obrisati `/tmp/user_1_folder` kao user 2. Radi li ? Ne bi trebalo.

### Nastavak ucenja
Pored sticky bit postoje `setuid` i `setgid` koje omogucavaju pokretanje izvrsnih dokumenata korisniku sa permisijama iskljucivo kreatora dokumenta ili sa permisijama grupe.
