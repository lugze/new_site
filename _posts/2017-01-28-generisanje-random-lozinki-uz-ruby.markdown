---
layout: post
title:  "Ruby: Generisanje random lozinki"
author: nedim
date:   2017-01-28 21:07:26 +0100
categories: ruby
---

Otkriti novi nacin generisanja nasumicne lozinke mi je uvijek bio od interesa.

## Random Metoda

Primjeri iz ovog clanka koriste Random metodu/funkciju koja o kojoj mozete vise procitati [ovdje](https://ruby-doc.org/core-2.4.0/Random.html). Ovu metodu mozemo testirati tako
sto cemo generisati nasumicni broj. Pokrenite IRB pomocu komande `irb` zatim pozovite `rand` metodu.

{% highlight ruby %}
rand
 => 0.8270045384707297 
{% endhighlight %}

Po defaultu rand vraca decimale a ako metodi dodijelimo argument u vidu broja onda ce rand generisati broj od nule sve do tog broja, ne ukljucujuci broj koji smo naveli kao argument.

{% highlight ruby %}
rand 200
 => 187 
{% endhighlight %}

U navedenom primjeru rand je vratio broj izmedju `0` i `200` odnosno broj `187`.  Navedeni primjer mozemo dodatno zakomplikovati ako saberemo broj nasumicno generisani broj sa brojem po zelji.

{% highlight ruby %}
65 + rand(200)
 => 103 
{% endhighlight %}

U ovom primjeru sabiramo sa brojem `65.`


Da generisemo broj odredjenog raspona `rand` koristimo na sljedeci nacin

{% highlight ruby %}
rand 100..400
 => 295 
{% endhighlight %}

U ovom primjeru `rand` je generisao nasumicni broj `295`.

## Sample Metoda

Ovu metodu mozemo koristiti ako zelimo da izaberemo nasumicno broj iz zadane liste. U ovom primjeru `sample` metoda ce izabrati nasumicno broj iz liste `[2, 5, 15, 40, 20, 11]`.

{% highlight ruby %}
[2, 5, 15, 40, 20, 11].sample
 => 20 
{% endhighlight %}

Jos jedan nacin da izaberemo nasumicni broj iz liste je vezivanjem metoda `shuffle`, `first` ili `last`.

{% highlight ruby %}
[2, 5, 15, 40, 20, 11].shuffle.first
 => 11 
{% endhighlight %}

Nakon sto `shuffle` pomijesa brojeve iz liste metoda `first` prikaze prvi clan liste.

{% highlight ruby %}
[2, 5, 15, 40, 20, 11].shuffle.last
 => 15 
{% endhighlight %}

Nakon sto `shuffle` pomijesa brojeve iz liste metoda `last` prikaze zadnji clan liste.

#### Opseg i Sample metoda

Metodu `sample` mozemo vezati na niz iz nekog opsega. Recimo kada zelimo generisati nasumicno slovo iz abecede.

{% highlight ruby %}
('a'..'z').to_a.sample
 => "l"
{% endhighlight %}

Slican primjer ali ovaj put sa opsegom brojeva od `0` do `99`.

{% highlight ruby %}
(0..99).to_a.sample
 => 88 
{% endhighlight %}

### Nekoliko primjera kako pomocu `Ruby` generisati random lozinke


#### Prvi primjer

Pokrenute IRB pomocu komande `irb` zatim prekucajte kod ispod

{% highlight ruby %}
(0...8).map { (65 + rand(26)).chr }.join
{% endhighlight %}

Takodjer isti kod mozete pokrenuti direktno iz CLI 

{% highlight bash %}
ruby -e "puts (0...8).map { (65 + rand(26)).chr }.join"
{% endhighlight %}

#### Drugi primjer

{% highlight ruby %}
(0...50).map { ('a'..'z').to_a[rand(26)] }.join
{% endhighlight %}


#### Treci primjer

{% highlight ruby %}
charset = [*'a'..'z', *'A'..'Z'].sample(30).join
{% endhighlight %}
