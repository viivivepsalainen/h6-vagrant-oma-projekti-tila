# h6-vagrant
**palvelinten hallinta s22, läksy 6 = vagrantin asennus ja siinä orja-herra arkkitehtuurin teko**

x) Lue ja tiivistä (muutamalla ranskalaisella viivalla per artikkeli, poimi esim itsellesi keskeisimmät komennot)

Karvinen 2017: Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds (Suosittelen käyttämään tässä koneena 'vagrant init debian/bullseye64')

Kyseinen sivu kertoo kuinka ladata virtuaaliboxi ja avata se, itselle tärkeimmät komennot ovat ```vagrant up``` ja ```vagrant ssh```, ensimmäisellä käynnistetään/uudelleenkäynnistetään vagrantti ja toisella mennään ns. sisälle/avataan yksi taikka tietty kone. 

Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant

Tämä sivu kertoo kuinka ladata kaksi virtuaalikonetta samaan verkkoon; luomalla niille yhteisen kansion, jossa muokataan tuota vagrantfilea, jonka valmis koodi on myöskin kyseisellä sivulla (jonne pitää vain muokata omat nimet jne.) Sivu myöskin kertoo, kuinka testata koneiden välistä yhteyttä ```ping -c 1 "ip-osoite"``` komennolla sekä kuinka tuhota virtuaalikoneet ```vagrant destory``` komennolla, jotta voi esimerkiksi aloittaa puhtaalta pöydältä. Viimeisenä sivulta löytyy ratkaisu virheeseen, jos virtuaalikoneita käynnistäessä command promptti ilmoittaakin, ettei ip-osoitenumerot ole sallituja, tällöin vain vaihdetaan parin numeron paikkaa vagrantfilessa konffatessa: ip-osoite "192.168.88.101" ei kelpaa, vaihdetaan 88 tilalle 60.

Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux

Tämä sivu kertoo, kuinka rakennetaan herra-orja arkkitehtuuri Linuxin ubuntulle. Tärkeimmät komennot itselleni ovat: ```sudo apt-get -y install salt-master```, jolla ladataan masteri/herra, sekä ```sudo apt-get -y install salt-minion``` jolla ladataan minioni/orja. Komennolla ```sudoedit /etc/salt/minion``` pitää muokata minionin tiedostoa, jotta orja tietää missä herra on (master ja id kohtaan vaihdetaan omat tiedot). Tämän jälkeen ```sudo systemctl restart salt-minion.service```, jotta tiedot päivittyvät. 

Sitten vielä pitää hyväksyä orjan avain herralle, eli sudo salt-key -A, jonka jälkeen kokeillaan vielä että ```sudo salt '*' cmd.run 'whoami'``` , jotta orja vastaa. 

a) Hello Vagrant. Asenna virtuaalikone Vagrantilla.

Tosiaan asentelen tuota Vagranttia Windows11. Ensimmäisenä latasin Hashicorpin sivuilta (https://developer.hashicorp.com/vagrant/downloads) tuon tiedoston AMD64, version 2.3.3. Sen latautumisen jälkeen avasin Windows Powershellin (as a administrator) jolla jatkoin tehtävän tekoa. 

b) Yksityisverkko. Asenna kaksi virtuaalikonetta samaan verkkoon Vagrantilla. Laita toisen koneen nimeksi "isanta" ja toisen "renki1". Kokeile, että "renki1" saa yhteyden koneeseen "isanta" (esim. ping tai nc). Tehtävä tulee siis tehdä alusta, vaikka olisit ehtinyt kokeilla tätä tunnilla.

Nyt kun vagrantin tiedosto oli ladattu, loin ensin kansion twohosts ```mkdir twohosts```, jonne siirryin ```cd twohosts```. Tämän jälkeen kommennolla ```vagrant init debian/bullseye64``` latailin oikeaa boksia. 

Tämän jälkeen muokkasin vagrantfilea, jotta saan oikeat nimet ja lisättyä kaksi konetta samaan verkkoon. 

Eli komennolla ```notepad.exe vagrantfile``` avasin tiedoston, josta poistin valmiiksi konfiguroidut osat, ja lisäsin tilalle: 

```
$tscript = <<TSCRIPT
set -o verbose
apt-get update
apt-get -y install tree
echo "Done - set up test environment - https://terokarvinen.com/search/?q=vagrant"
TSCRIPT

Vagrant.configure("2") do |config|
	config.vm.synced_folder ".", "/vagrant", disabled: true
	config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
	config.vm.provision "shell", inline: $tscript
	config.vm.box = "debian/bullseye64"

	config.vm.define "isanta" do |isanta|
		isanta.vm.hostname = "isanta"
		isanta.vm.network "private_network", ip: "192.168.88.101"
	end

	config.vm.define "renki1", primary: true do |renki1|
		renki1.vm.hostname = "renki1"
		renki1.vm.network "private_network", ip: "192.168.88.102"
	end
	
end
```
Ylläoleva koodi on melkein valmiiksi kopsattu sivulta: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ , vaihdoin vain oikeat nimet oikeisiin paikkoihin. 

Tämän jälkeen uudelleenkäynnistin vagrantin komennolla ```vagrant up```. 

Sitten testailin että konffaus toimii, komennolla vagrant ssh isanta/renki1 ja kumpaankin päästiin sisälle, kuten alla näkyykin. 

![toimiii](https://user-images.githubusercontent.com/118457367/205957504-d157644d-4dec-47c9-b26f-8a7ab9239be3.jpg)

Tämän jälkeen katselin vielä, että kummatkin koneet saavat yhteyden toisiinsa ping komennolla: 

![pingrenki](https://user-images.githubusercontent.com/118457367/205958313-562a564f-0d5c-4065-a902-7c58b8b3cb2d.jpg)

![isantaping](https://user-images.githubusercontent.com/118457367/205958365-a14f6284-b763-4820-b7bb-67df56a680b7.jpg)

c) Salt master-slave. Toteuta Salt master-slave -arkkitehtuuri verkon yli. Aseta edellisen kohdan kone renki1 orjaksi koneelle isanta.

Ekana avasin tuon isanta vagrantin komennolla ```vagrant ssh``` , jonne aloin asentamaan tuota salt-masteria komennolla ```sudo apt-get -y install salt-master```. 

Tämän jälkeen avasin renki1 vagrantin, jonne asensin salt-minionin komennolla ```sudo apt-get -y install salt-minion```. 

Sitten tsekkailin masterin ip-osoitteen komennolla ```hostname -I``` (192.168.88.101), jotta voin kertoa orjalle/minionille oikeat tiedot. 

Tämän jälkeen siirryin takaisin renki1 vagranttiin, jossa availin tiedostoa ```sudoedit salt/etc/minion```, jotta pääsen laittamaan sinne masterin ip-osoitteen ja orjan/minionin nimen: 

![IEDOT](https://user-images.githubusercontent.com/118457367/205973399-ae30b699-3d41-44ed-8058-a3110ff45367.jpg)

Tämän jälkeen käynnistelin renki1 uudelleen ```sudo systemctl restart salt-minion.service```, jotta tiedot päivittyvät. 

Sitten siirryin isanta vagranttiin, jossa komennolla ```sudo salt-key -A``` pystyin hyväksymään renki1 avaimen.

![saltkey](https://user-images.githubusercontent.com/118457367/205973683-12ed86b6-8fef-4928-bcfb-6119a854810b.jpg)

Lopputulos oli: "Key for minion renki1 accepted". 

Sitten testailin vielä, että renki1 vastailee isannalle:

![toimii2](https://user-images.githubusercontent.com/118457367/205974203-66fdc71c-ccc2-4797-81f6-896b0c950b80.jpg)

Vastasi. 
