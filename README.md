# h6-vagrant-oma-projekti-tila
**palvelinten hallinta s22, läksy 6 = vagrantin asennus ja siinä orja-herra arkkitehtuurin teko sekä oman tilan/moduulin teon aloitus**

a) Hello Vagrant. Asenna virtuaalikone Vagrantilla.

Tosiaan asentelen tuota Vagranttia Windows11. Ensimmäisenä latasin Hashicorpin sivuilta (https://developer.hashicorp.com/vagrant/downloads) tuon tiedoston AMD64, version 2.3.3. Sen latautumisen jälkeen avasin Windows Powershellin (as a administrator) jolla jatkoin tehtävän tekoa. 

b) Yksityisverkko. Asenna kaksi virtuaalikonetta samaan verkkoon Vagrantilla. Laita toisen koneen nimeksi "isanta" ja toisen "renki1". Kokeile, että "renki1" saa yhteyden koneeseen "isanta" (esim. ping tai nc). Tehtävä tulee siis tehdä alusta, vaikka olisit ehtinyt kokeilla tätä tunnilla.

Nyt kun vagrantin tiedosto oli ladattu, loin ensin kansion isanta ```mkdir isanta```, jonne siirryin ```cd isanta```ja jonne asensin tuon ```vagrant init bento/ubuntu-16.04``` ja käynnistin tuolla komennolla ```vagrant up``` kuten allaolevassa kuvassa näkyy: 

![h62](https://user-images.githubusercontent.com/118457367/205930303-3ac32e70-b1c9-46d5-a37b-dd0b8321c96b.jpg)

