---
layout: post
title:  "Linux: Kako prikazati trenutnu potrosnju RAM memorije?"
author: nedim
date:   2017-01-28 18:58:26 +0100
categories: linux nedim
---

## Prikaz preko pseudo datoteke `/proc/meminfo`

Jedan od nacina je koristiti grep alat za filter kljucnih rijeci `MemFree` iz sistemskog fajla `/proc/meminfo`

{% highlight shell %}
grep Mem /proc/meminfo
{% endhighlight %}

ispis
{% highlight shell %}

MemTotal:       131917312 kB
MemFree:         3970444 kB
MemCommitted:   354429952 kB

{% endhighlight %}

Za prikaz ukupe RAM memorije
{% highlight shell %}
grep MemTotal /proc/meminfo
{% endhighlight %}

Za prikaz slobodne memorije

{% highlight shell %}
grep MemFree /proc/meminfo
{% endhighlight %}

## FREE KOMANDA

Komanda cesto koristena za prikazivanje trenutne potrosnje RAM memorije je komanda free. 

Primjeri upotrebe:

- Prikaz potrosnje u Gigabajtima

{% highlight shell %}
free -g
{% endhighlight %}

ispis komande iznad

{% highlight shell %}
             total       used       free     shared    buffers     cached
Mem:           125        122          3          0          5         63
-/+ buffers/cache:         53         72
Swap:           20         11          9
{% endhighlight %}


- Prikazm potrosnje u formatu lakse citljivom za covjeka tzv human 

{% highlight shell %}
free -g -h
{% endhighlight %}

ispis

{% highlight shell %}
             total       used       free     shared    buffers     cached
Mem:          125G       122G       3.7G         0B       5.6G        63G
-/+ buffers/cache:        53G        72G
Swap:          20G        11G       9.1G
{% endhighlight %}


- Total odnosno zbirni prikaz kolicine RAM memorije i velcine swap particije

{% highlight shell %}
free -g -h -t
{% endhighlight %}

ispis

{% highlight shell %}
             total       used       free     shared    buffers     cached
Mem:          125G       122G       3.6G         0B       5.6G        63G
-/+ buffers/cache:        53G        72G
Swap:          20G        11G       9.1G
Total:        146G       133G        12G{% endhighlight %}


- Opcija -o izbacuje prikaz linije "buffer adjusted"

{% highlight shell %}
free -g -h -o
{% endhighlight %}

{% highlight shell %}
             total       used       free     shared    buffers     cached
Mem:          125G       122G       3.6G         0B       5.6G        63G
Swap:          20G        11G       9.1G
{% endhighlight %}

- Zbog lakseg koristenja format pisanja se moze pojednostaviti u sljedeci

{% highlight shell %}
free -ghto
{% endhighlight %}

ispis

{% highlight shell %}
             total       used       free     shared    buffers     cached
Mem:          125G       122G       3.6G         0B       5.6G        63G
Swap:          20G        11G       9.1G
Total:        146G       133G        12G
{% endhighlight %}

Help stranice za free komandu.

{% highlight shell %}
free --help
{% endhighlight %}

ispis

{% highlight shell %}
Usage:
 free [options]

Options:
 -b, --bytes         show output in bytes
 -k, --kilo          show output in kilobytes
 -m, --mega          show output in megabytes
 -g, --giga          show output in gigabytes
     --tera          show output in terabytes
 -h, --human         show human readable output
     --si            use powers of 1000 not 1024
 -l, --lohi          show detailed low and high memory statistics
 -o, --old           use old format (no -/+buffers/cache line)
 -t, --total         show total for RAM + swap
 -s N, --seconds N   repeat printing every N seconds
 -c N, --count N     repeat printing N times

      --help    display this help text
 -V, --version  output version information and exit

For more details see free(1).
{% endhighlight %}
