# h6-vagrant-oma-projekti-tila
**palvelinten hallinta s22, läksy 6 = vagrantin asennus ja siinä orja-herra arkkitehtuurin teko sekä oman tilan/moduulin teon aloitus**

a) Hello Vagrant. Asenna virtuaalikone Vagrantilla.

Tosiaan asentelen tuota Vagranttia Windows11. Ensimmäisenä latasin Hashicorpin sivuilta (https://developer.hashicorp.com/vagrant/downloads) tuon tiedoston AMD64, version 2.3.3. Sen latautumisen jälkeen avasin Windows Powershellin (as a administrator) jolla jatkoin tehtävän tekoa. 

b) Yksityisverkko. Asenna kaksi virtuaalikonetta samaan verkkoon Vagrantilla. Laita toisen koneen nimeksi "isanta" ja toisen "renki1". Kokeile, että "renki1" saa yhteyden koneeseen "isanta" (esim. ping tai nc). Tehtävä tulee siis tehdä alusta, vaikka olisit ehtinyt kokeilla tätä tunnilla.

Nyt kun vagrantin tiedosto oli ladattu, loin ensin kansion isanta ```mkdir isanta```, jonne siirryin ```cd isanta```ja jonne asensin tuon ```vagrant init bento/ubuntu-16.04``` ja käynnistin tuolla komennolla ```vagrant up``` kuten allaolevassa kuvassa näkyy: 

![h62](https://user-images.githubusercontent.com/118457367/205931191-07040d69-34d9-49a7-bdef-e8b0e2c1e8f5.jpg)

Tämän jälkeen avasin/siirryin virtuaalikoneeseen komennolla ```vagrant ssh```, josta poistuin tämän jälkeen komennola ```exit vagrant```.

Sitten samalla tavalla loin kansion renki1, jonne siirryin ja asensin sinne tuon bento/ubuntu-16.04. Tämän jälkeen samalla tavalla komento ```vagrant up``` koneen käynnistymiseksi, ja ```vagrant ssh``` koneelle sisään pääsemiseksi. 

Tämän jälkeen tsekkasin isännän ip osoitteen ```hostname -I``` komennolla, jonka pingasin renki1 koneessa, tuloksena:

![testaush6](https://user-images.githubusercontent.com/118457367/205940837-1d9cc38d-5e62-4662-80d6-382873416cc7.jpg)

