---
layout: post
title:  "Generisanje random lozinki iz shella "
author: admeer
date:   2017-01-28 20:05:26 +0100
categories: linux
---

Generisanje random lozinki iz shella se moze uradit na vise nacina. Ovdje cemo navesti jedan nacin.

{% highlight shell %}
head --bytes 22 /dev/urandom | base64
output: HnaDkOg1bz761p02KAWCECh5hO+LmA==
{% endhighlight %}
ili
{% highlight shell %}
head -c 12 /dev/urandom | base64
output: tnS/GSlJVHVjuX61
{% endhighlight %}

komanda cita prvih nekoliko linija bilo kojeg datog teksta i ispisuje ih kao standardni output u konzolu
{% highlight shell %}
head          
{% endhighlight %}


opcija limitira output na 12 bytes
{% highlight shell %}
-c 12         
{% endhighlight %}

omogucuje interfejs prema kernelovom random generatoru brojeva
{% highlight shell %}
/dev/urandom     
{% endhighlight %}

znak pipe prosljedjuje karaktere koje ispise head komanda na standardni output komandi base64
{% highlight shell %}
znak |        
{% endhighlight %}

sluzi za kodiranje outputa prethodnog coda komande sa base64, da se dekodira output koristimo base64 -d
{% highlight shell %}
base64         
{% endhighlight %}
