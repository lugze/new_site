---
layout: post
title:  "Koristenje dig alata iz shella "
author: admeer
date:   2017-01-31 17:48:07 +0100
categories: linux
---
### DNS upiti pomocu dig alata.

Koristiti cemo dig alat za upit dns domenskih postavki.

###### Instalacija.

{% highlight shell %}
apt-get install dnsutils
{% endhighlight %}

##### Procedura za dobijanje liste Name Servera za domenu:

*    dobijanje liste svih root servera
*    upit na bilo koji root server za AUTHORITATIVE GTLD(Generic Top Level Domain) dns servere
*    upit na GTLD server


#### 1.  Dobijanje liste svih root servera
Dig bez argumenata dat ce listu root servera

{% highlight shell %}
$ dig +short
j.root-servers.net.
g.root-servers.net.
l.root-servers.net.
c.root-servers.net.
e.root-servers.net.
f.root-servers.net.
h.root-servers.net.
i.root-servers.net.
m.root-servers.net.
d.root-servers.net.
a.root-servers.net.
b.root-servers.net.
k.root-servers.net.

{% endhighlight %}

#### 2.  Upit na bilo koji root server za AUTHORITATIVE GTLD(Generic Top Level Domain) dns servere

###### Uzmite bilo koji root server i postavite upit za GTLD server


{% highlight shell %}
$ dig @c.root-servers.net debian.org

; <<>> DiG 9.10.3-P4-Ubuntu <<>> @c.root-servers.net debian.org
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15573
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 6, ADDITIONAL: 13
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;debian.org.			IN	A

;; AUTHORITY SECTION:
org.			172800	IN	NS	a2.org.afilias-nst.info.
org.			172800	IN	NS	a0.org.afilias-nst.info.
org.			172800	IN	NS	d0.org.afilias-nst.org.
org.			172800	IN	NS	b0.org.afilias-nst.org.
org.			172800	IN	NS	b2.org.afilias-nst.org.
org.			172800	IN	NS	c0.org.afilias-nst.info.

;; ADDITIONAL SECTION:
a0.org.afilias-nst.info. 172800	IN	A	199.19.56.1
a2.org.afilias-nst.info. 172800	IN	A	199.249.112.1
b0.org.afilias-nst.org.	172800	IN	A	199.19.54.1
b2.org.afilias-nst.org.	172800	IN	A	199.249.120.1
c0.org.afilias-nst.info. 172800	IN	A	199.19.53.1
d0.org.afilias-nst.org.	172800	IN	A	199.19.57.1
a0.org.afilias-nst.info. 172800	IN	AAAA	2001:500:e::1
a2.org.afilias-nst.info. 172800	IN	AAAA	2001:500:40::1
b0.org.afilias-nst.org.	172800	IN	AAAA	2001:500:c::1
b2.org.afilias-nst.org.	172800	IN	AAAA	2001:500:48::1
c0.org.afilias-nst.info. 172800	IN	AAAA	2001:500:b::1
d0.org.afilias-nst.org.	172800	IN	AAAA	2001:500:f::1

;; Query time: 53 msec
;; SERVER: 192.33.4.12#53(192.33.4.12)
;; WHEN: Tue Jan 31 18:06:04 CET 2017
;; MSG SIZE  rcvd: 441

{% endhighlight %}

#### 3. Upit na GTLD server

{% highlight shell %}
$ dig @b0.org.afilias-nst.org debian.org

; <<>> DiG 9.10.3-P4-Ubuntu <<>> @b0.org.afilias-nst.org debian.org
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16453
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 3, ADDITIONAL: 3
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;debian.org.			IN	A

;; AUTHORITY SECTION:
debian.org.		86400	IN	NS	dnsnode.debian.org.
debian.org.		86400	IN	NS	sec2.rcode0.net.
debian.org.		86400	IN	NS	sec1.rcode0.net.

;; ADDITIONAL SECTION:
dnsnode.debian.org.	86400	IN	A	194.146.106.126
dnsnode.debian.org.	86400	IN	AAAA	2001:67c:1010:32::53

;; Query time: 52 msec
;; SERVER: 199.19.54.1#53(199.19.54.1)
;; WHEN: Tue Jan 31 18:06:36 CET 2017
;; MSG SIZE  rcvd: 153

{% endhighlight %}

###### Dig ce dati odgovor sa **AUTHORITY SECTION** gdje se moze naci AUTHORITY name server

##### Generic dig upit
###### Dobijanje bilo kojih postavki

{% highlight shell %}
$ dig +short any debian.org
sec1.rcode0.net.
dnsnode.debian.org.
sec2.rcode0.net.
{% endhighlight %}

###### Dobijanje MX recorda za debian.org

{% highlight shell %}
$ dig +short mx debian.org
0 mailly.debian.org.
0 muffat.debian.org.
{% endhighlight %}

**OBJASNJENJE**

###### Koristenje komande dig bez *+short* opcije
###### Kada se korisit +short opcija dobija se samo dio iz ANSWER SECTION

*QUESTION SECTION*

Mozemo vidjeti koji smo upit postavili.

*ANSWER SECTION*

Unutar odjeljka mozemo vidjeti odgovor za trazeni upit.

##### Dobijanje NS servera za debian.org

{% highlight shell %}
$ dig +short ns debian.org
dnsnode.debian.org.
sec1.rcode0.net.
sec2.rcode0.net.
{% endhighlight %}

##### Dobijanje A recorda za www.debian.org

{% highlight shell %}
$ dig +short a debian.org
5.153.231.4
140.211.15.34
128.31.0.62
130.89.148.14
{% endhighlight %}



