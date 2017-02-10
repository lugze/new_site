---
layout: post
title:  "/sys : Uvod u sysfs"
author: nedim
date:   2017-02-09 19:41:10 +0100
categories: linux sysfs 101
tags: [linux, proc]
---

* Sysfs je spoj `proc`, `devfs`, and `devpty` virtuelnih file sistema te i sam je virtuelni file sistem.
* Osnovna svrha mu je pružanje informacija o hardverskoj komponenti sistema userspace programima kao sto je `udev`.
* Ovaj virtuelni sistem je aktuelan od Linux 2.6 kernel verzije čime su virtuelni fajlovi vezani za hardver izmjesteni iz `/proc` direktorija.
* Pristup sysfs-u se moze izvrsiti pristupum `/sys/`` direktoriju iz shella.
* Informacije koje se nalaze u sysfs-u su zapisane u `plain text` formatu tako da im mogu pristupiti kako sistem administratori tako i programi.
* `sysfs` je baziran na `ramfs` programskom kodu iz kernel verzije 2.4.
* Originalni naziv za sysfs bio je `ddfs` (Device Driver Filesystem).
* `ddfs` u kernelu 2.5 je preimenovan u `driverfs` a proširenjem funkcionalnosti postao je `sysfs`.

Sysfs je virtuelni file sistem koji dinamički kreira i puni sadržajem Linux kernel. Sadržaj se odnosi na informacije i drivere vezane za hardver.
Na ovaj način pristupamo iz user space-a preko virtuelnih fajlova do informacija koje se nalaze u kernel space-u.
Osim što ove virtuelne fajlove možemo koristiti za čitanje informacija o hardveru neke od njih možemo koristiti za konfigurisanje određenog hardvera.


## Lokacija

Sysfs file sistem je mountiran na putanji `/sys/` i sadrži direktorije pomoću kojih možemo saznati informacije o uređajima koji su na različite načine spojeni na računar.


## Vježba: Mountiranje `sysfs`

`Sysfs`, kao i bilo koji virtuelni file sistem smješten u memoriji, može se mountati iz userspace-a pomoću `mount` komande.
Defaultna `/sys/` lokacija se mounta automatski prilikom bootanja. Kao dio vježbe mount `sysfs` ćemo izvršiti u dodatni direktorij.

* 1. Napraviti direktorij u koji ćemo mountiati `sysfs`.
* 2. Mount `sysfs` izvršiti u direktorij na `/mnt/sysfs` lokaciji.
* 3. Nakon toga ćemo porediti da li je isti sadržaj u defaultnoj `/sys/` i našoj `/mnt/sysfs` lokaciji.

* Napravite direktorij u koji cemo mountati sysfs

```
 mkdir /mnt/sysfs
```

* Izvrišite mount sysfs-a iz RAMA u `/mnt/sysfs`

```
mount -t sysfs sysfs /mnt/sysfs/
```

* Provjeriti da li postoji sadržaj nakon mount-a

```
ls /mnt/sysfs/
```
* Trebali bi dobiti nešto slično sadržaju ispod

```
block  bus  class  dev  devices  firmware  fs  hypervisor  kernel  module  power
```

* Sada je potrebno porediti da li isti sadržaj ima defaultna `sysfs` lokacija kao i naš direktorij iz vježbe
 * listing defaultne sysfs lokacije

```
ls /sys/
```

 * listing našeg direktorija

```
ls /mnt/sysfs/
```

sadržaj je identičan.

Nakon ove vježbe potrebno je uraditi `unmount`

* ```umount /mnt/sysfs```

Te provjeriti da li je unmountirani direktorij prazan

* ```ls /mnt/sysfs/```


## Opis poddirektorija unutar `/sys/`


## Tree prikaz

Koristan alat za prikaz strukture `/sys/` direktorija je `tree` komanda.

Komanda za tree prikaza `/sys`.

```
tree -L 1 /sys/
```

Dobijemo sljedeći ispis.

```
/sys/
├── block
├── bus
├── class
├── dev
├── devices
├── firmware
├── fs
├── hypervisor
├── kernel
├── module
└── power
```

## Opis osnovnih poddirektorija `/sys/`

### /sys/block poddirektorij

Block direktorij sadrži poddirektorije za sve `block` uređaje koji su otkriveni od strane sistema.
Block uređaji su svi uređaji koji mogu biti služeni za skladištenje podataka kao što su diskovi, ram memorija itd.
Svaki uređaj sadrži svoj poddirektorij koji sadrži atribute uređaja kao što je veličina itd.

#### Primjer

Recimo za disk na sistemu koji je prijavljen kao `sda` i pokretanjem komande u nastavku možemo saznati sve prijavljene informacije o disku..

```
ls /sys/block/sda/
```

U navedenom direktoriju se nalaze fajlovi i poddirektoriji koji sadrže sve potrebne informacije. Recimo veličinu diska možemo saznati pokretanjem sljedeće komande

```
cat /sys/block/sda/size
```

### `/sys/bus/` poddirektorij

`Bus' direktorij sadrži poddirektorije za svaku fizicku sabirnicu odnosno bus koja je prijavljena u kernelu. Primjer tree prikaza poddirektorija unutar bus direktorija:

```
/sys/bus/
├── acpi
├── clockevents
├── clocksource
├── container
├── cpu
├── edac
├── event_source
├── i2c
├── iscsi_flashnode
├── machinecheck
├── mc0
├── mdio_bus
├── memory
├── mipi-dsi
├── mmc
├── nd
├── node
├── pci
├── pci_express
├── platform
├── pnp
├── rapidio
├── scsi
├── sdio
├── serio
├── spi
├── usb
├── virtio
├── vme
├── workqueue
├── xen
└── xen-backend

```

Direktorij koji odgovara svakom pojedinacnom bus-u u sys direktoriju sadrži dva poddirektorija i to `devices` i `drivers`.  

#### /sys/bus/*/devices/ direktorij

Direktorij `devices` sadrži sve uređaje tog tipa koji su otkriveni na cijelom sistemu.

Primjer tree prikaza devices poddirektorija za pci bus

```
tree -L 1 /sys/bus/pci/devices/
/sys/bus/pci/devices/
├── 0000:00:00.0 -> ../../../devices/pci0000:00/0000:00:00.0
├── 0000:00:01.0 -> ../../../devices/pci0000:00/0000:00:01.0
├── 0000:00:02.0 -> ../../../devices/pci0000:00/0000:00:02.0
├── 0000:00:06.0 -> ../../../devices/pci0000:00/0000:00:06.0
├── 0000:00:16.0 -> ../../../devices/pci0000:00/0000:00:16.0
├── 0000:00:1a.0 -> ../../../devices/pci0000:00/0000:00:1a.0
├── 0000:00:1c.0 -> ../../../devices/pci0000:00/0000:00:1c.0
├── 0000:00:1c.5 -> ../../../devices/pci0000:00/0000:00:1c.5
├── 0000:00:1c.7 -> ../../../devices/pci0000:00/0000:00:1c.7
├── 0000:00:1d.0 -> ../../../devices/pci0000:00/0000:00:1d.0
├── 0000:00:1e.0 -> ../../../devices/pci0000:00/0000:00:1e.0
├── 0000:00:1f.0 -> ../../../devices/pci0000:00/0000:00:1f.0
├── 0000:00:1f.2 -> ../../../devices/pci0000:00/0000:00:1f.2
├── 0000:00:1f.3 -> ../../../devices/pci0000:00/0000:00:1f.3
├── 0000:01:00.0 -> ../../../devices/pci0000:00/0000:00:01.0/0000:01:00.0
├── 0000:04:00.0 -> ../../../devices/pci0000:00/0000:00:1c.5/0000:04:00.0
└── 0000:05:00.0 -> ../../../devices/pci0000:00/0000:00:1c.7/0000:05:00.0
```

#### drivers dir

Ovaj direktorij sadrži poddirektorije za svaki device driver koji je prijavljen za taj tip sabirnice.
Unutar tih direktorija se nalaze atributi pomoću kojim možemo vidjeti postavke drivera te ih modifikovati.


### /sys/class direktorij

Kao što ime samo govori class direktorij sadrži klase koje logički opisuju hardver.
Klase predstavljaju nivo apstrakcije koji na intuitivan način daju imena hardverskim komponentama.

Primjeri:

#### Svi mrežni uređaji se nalaze u podrektoriju

```ls /sys/class/net```

#### Svi input uređaji se nalaze u poddirektoriju

```ls /sys/class/input```

#### Serijski uredajaji se nalaze

```ls /sys/class/tty```

#### Svi block uređaji se nalaze

```ls /sys/class/block/```


Svaki uređaji odredjene klase sadrži fajlove i direktorije koje sadrže dodatne informacije o navedenom urađaju.

#### Primjeri vezani za mrežnu karticu

##### Kako saznati da li je kabl uključen u mrežnu karticu?

```
cat /sys/class/net/eth0/carrier
```
* 0 - isključeno
* 1 - uključeno


## /sys/firmware direktorij

firmware direktorij sadrži interfejse za pregled i manipulisanje atributa koji su specificni za firmware kao što je BIOS.

Tree prikaz firmwware direktorija

```
/sys/firmware/
├── acpi
├── dmi
└── memmap
```

## /sys/module direktorij

Ovaj direktorij sadrži poddirektorije za svaki modul koji je ukljucen u kernel. Module možemo izlistati pomoću komande

```
ls /sys/module/
```

ili komande

```
lsmod
```
