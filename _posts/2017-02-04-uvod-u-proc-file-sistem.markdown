---
layout: post
title:  "/proc : Uvod u process information file system"
author: nedim
date:   2017-02-04 12:27:26 +0100
categories: linux
tags:
- linux
- proc
---

## Sta je to /proc?

Proc je virtuelni file sistem koji je mountiran u `/proc` direktorij te sadrzi poddirektorije
i fajlove u kojima se nalaze informacije o samom operativnom sistemu, kao sto su pokrenuti procesi, te hardveru.
Proc folder i fajlovi nisu snimljeni na disku nego se nalaze u  RAM memoriji. Interesantno je da velicina fajlova i direktorija u `/proc` iznosi `0`.
Ovo omogucuje `VHS` (Virtual File System) sto znaci da fajl ne sadrzi informacije sve do trenutka kada mu korisnik pristupi.
U tom trenutku sadrzaj se dinamicki ucita iz kernela i napuni sadrzajem. 
Ovo mozemo lako provjeriti koristeci komandu `file` koja analizira fajlove te ispise infrmacije o tipu fajla. 

Primjer za datoteku iz `/proc`:

{% highlight shell %}
file /proc/meminfo 
/proc/meminfo: empty
{% endhighlight %}

Primjer iznad jasno govori da je datoteka prazna. Medjutim, kada pokusamo pristupiti samom sadrzaju fajla komanda ce biti uspjesna:

{% highlight shell %}
cat /proc/meminfo 
{% endhighlight %}

Proc mozemo smatrati kao interfejs prema razlicitim informacija koje se nalaze u kernelu.
Neke informacije takodjer mozemo mijanjti dok je racunar upaljen - `sysctl`. 

Dodatne informacije se nalaze na `man` stranici kojoj mozemo pristupiti pomocu sljedece komande.

{% highlight shell %}
man proc
{% endhighlight %}

## Pregled informacija o procesima

Pokrenuti proces ima jedinstveni `PID` ili `Proces ID` odnosno jedinstveni broj koji je snimljen u  `/proc` direktoriju.
Prvi proces koji se boota jer init process i kao prvi ima dodijeljen broj `1`. Ako zelimo da vidimo informacije
o initu kao sto je status procesa pokrecemo sljedecu komandu.

{% highlight shell %}
cat /proc/1/status
{% endhighlight %}

#### Pregled `/proc` fajlva koji otkrivaju dodatne informaicije o zadanim procesima. 

Napmena: U primjerima ispod potrebno je PID zamijeniti sa stvarnim brojem procesa.

* `cat /proc/PID/status` - vraca status procesa te ostale korisne informacija kao ko je pokrenuo process te zauzece memorije.
* `cat /proc/PID/cmdline` - komanda koje je pokrenula porces
* `ls /proc/PID/cwd` - simbolicki link do `current working directory`
* `cat /proc/PID/environ` - sadrzi environment varijable koje formiraju okruzenje pod kojim je pokrenut proces
* `ls /proc/PID/exe` - simbolicki link do samog programa, ako jos postoji na disku
* `ls -l /proc/PID/fd` - direktorij koji sadrzi simbolicke linkove do svih `file descriptora` koje je otvorio proces kao sto su fajlovi, mrezni socketi itd.
* `ls /proc/PID/fdinfo` - direktorij sadrzi dodatne informacije o `file descriptors` 
* `cat /proc/PID/maps` - datoteka koja sadrzi informacije o mapiranim fajlovima i blokovima kao sto su heap i stack
* `ls /proc/PID/root` - simbolicki link do / odnosno root putanje onako kako ga vidi taj proces. 
Za vecinu procesa to ce biti / putanja osim ako se process ne pokrene preko chroot programa ili `change root` cime
proces mozemo zavarati da je putanja do root foldra negdje drugo.
* `ls /proc/PID/task` - direktorij koji sadrzi hard linkove do taskova koji su pokrenuti od strane ovog procesa - parent proces.





## Mountiranje proc-a

U ovom dijelu cemo opisati gdje se nalazi defaultna sistemska okacija za proc te kako proc mountirati u direktorij po zelji.

### Defaultna mount lokacija

Proc se automatski mounta prilikom bootanja sistema na lokaciju `/proc` pomocu zapisa koji se nalazi u konfiguracijskom fajlu `/etc/fstab`. Sadrzaj te datoteke mozemo jednostavno izlistati pomocu grep komande.

{% highlight shell %}
grep proc /etc/fstab
{% endhighlight %}

### Mountanje u proc u direktorij po zelji

Proc iz RAMA mozemo mountati u bilo koji direktorij na hard disku. To cinimo na sljedeci nacin.

* Napravite direktorij u koji cemo mountati proc

{% highlight shell %}
 mkdir /mnt/proc
{% endhighlight %}

* Izvrisite mount proc-a iz RAMA u `/mnt/proc`

{% highlight shell %}
mount -t proc none /mnt/proc
{% endhighlight %}

* Sadrzaj `/mnt/proc` direktorija sada cemo provjeriti sa komandom `ls` te porediti sa defaultnim `proc` direktorijem

{% highlight shell %}
ls /mnt/proc
ls /proc
{% endhighlight %}

* Nakon zavrsene vjezbe potrebno je uraditi `unmount`

{% highlight shell %}
unmount /mnt/proc
{% endhighlight %}

## Primjeri

#### Kako saznati informacije o procesoru?

{% highlight shell %}
cat /proc/cpuinfo
{% endhighlight %}

#### Kako saznati informacije o RAM memoriji?

{% highlight shell %}
cat /proc/meminfo
{% endhighlight %}

#### Kako saznati koliko dugo je sistem upaljen?

{% highlight shell %}
cat /proc/uptime
{% endhighlight %}

Prvi broj je zbroj sekundi od zadnjeg restarta, drugi broj u sekundama govori koliko dugo je sistem `idle`.

#### Kako saznati koji kernel se boota i sa kojim parametrima?

{% highlight shell %}
cat /proc/cmdline
{% endhighlight %}

#### Kako saznati verziju kernela?

{% highlight shell %}
cat /proc/version
{% endhighlight %}


#### Kako saznati listu particija?

{% highlight shell %}
cat /proc/partitions 
{% endhighlight %}

#### Kako saznati informacije o swap particiji?

{% highlight shell %}
cat /proc/swaps 
{% endhighlight %}

