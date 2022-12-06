# h6-vagrant-oma-projekti-tila
**palvelinten hallinta s22, läksy 6 = vagrantin asennus ja siinä orja-herra arkkitehtuurin teko sekä oman tilan/moduulin teon aloitus**

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

c) Salt master-slave. Toteuta Salt master-slave -arkkitehtuuri verkon yli. Aseta edellisen kohdan kone renki1 orjaksi koneelle isanta.

