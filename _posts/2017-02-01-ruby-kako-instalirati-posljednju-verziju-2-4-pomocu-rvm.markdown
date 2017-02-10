---
layout: post
title:  "Ruby: Kako instalirati posljednju verziju 2.4 pomocu RVM"
author: nedim
date:   2017-02-01 10:27:26 +0100
categories: ruby
---

Nedavno je izasla 2.4 verzija Ruby programskog jezika. Vise mozete procitati na ovom [linku](https://www.ruby-lang.org/en/news/2016/12/25/ruby-2-4-0-released/).

U nastavku je opisan nacin kako instalirati te podesiti 2.4 verziju kao trenutnu i default verziju.

## Pripreme

Instalirati potrebne pakete za Debian/Ubuntu distribuciju.

{% highlight shell %}
apt-get install curl sudo
{% endhighlight %}


Slijede upute kako dodati novog korisnika sa usernameom `ruby` te unuta home direktorija navedenog korisnika instalirati posljednju ruby verziju.


Dodajemo korinsika ruby

{% highlight shell %}
adduser ruby
{% endhighlight %}

Dodajemo novog clana grupi sudo

{% highlight shell %}
usermod --append --groups sudo ruby
{% endhighlight %}

Sljedeci korak je logovanje kao korisnik `ruby`.

{% highlight shell %}
su ruby -l
{% endhighlight %}



## Instaliranje RVM

Sljedeci koraci se izvrsavaju dok ste prijavljeni kao `ruby` korisnik.

{% highlight shell %}
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable  --ruby=2.4.0
{% endhighlight %}

U slucaju da key ne radi novi mozete pokupiti na ovom [linku](https://rvm.io/). Potrebno je sacekati da download i kompajliranje zavrsi. Zatim je potrebno izvrsiti


{% highlight shell %}
`source ~/.rvm/scripts/rvm`
{% endhighlight %}

Provjeriti da li je dodan zapis koji automatski loada RVM prilikom logozanja korisnika

{% highlight shell %}
grep rvm ~/.bashrc
{% endhighlight %}

U slucaju da grep ne vrati rezultate potrebno je ozvrsiti komandu ispod koja dodaje potrebno za automatsko zivrsavanje RVMa.

{% highlight shell %}
echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
{% endhighlight %}

Sada je potrebno ucitati nove postavke

{% highlight shell %}
source ~/.bashrc
{% endhighlight %}

## Podesavanje i testiranje

Provjeriti instaliranu verziju ruby.

{% highlight shell %}
ruby -v
{% endhighlight %}

U slucaju da imate vec instaliran RVM komande za instaalciju 2.4 verzije te postavljanje iste kao trenutnu i default verziju su.

{% highlight shell %}
rvm install ruby-2.4.0
rvm --default use 2.4.0
rvm use 2.4.0
{% endhighlight %}


### Koje postavke kontrolise RVM?

RVM postavlja nove `environment` varijable od kojih su najvaznije `MY_RUBY_HOME`, `GEM_HOME`, `RUBY_VERSION` te modificira postojecu varijablu `PATH`. Trenutno postavljene vrijednosti navedenih varijabli mozemo provjeriti sa komandama u nastavku.

{% highlight shell %}
echo $RUBY_VERSION
{% endhighlight %}

{% highlight shell %}
echo $GEM_HOME
{% endhighlight %}

{% highlight shell %}
echo $MY_RUBY_HOME
{% endhighlight %}

Ako zelimo provjeriti sve `environment` varijable koje sadrze string `ruby` mozemo koristiti komandu ispod.

{% highlight shell %}
printenv | grep ruby
{% endhighlight %}
