# h6-vagrant-oma-projekti-tila
**palvelinten hallinta s22, läksy 6 = vagrantin asennus ja siinä orja-herra arkkitehtuurin teko sekä oman tilan/moduulin teon aloitus**

a) Hello Vagrant. Asenna virtuaalikone Vagrantilla.

Tosiaan asentelen tuota Vagranttia Windows11. Ensimmäisenä latasin Hashicorpin sivuilta (https://developer.hashicorp.com/vagrant/downloads) tuon tiedoston AMD64, version 2.3.3. Sen latautumisen jälkeen avasin Windows Powershellin (as a administrator) ja laitoin komennon ```vagrant init debian/bullseye64``` , jolla asentelin tuon "perusboxin". 

Tämän latautumisen jälkeen komennolla ```vagrant up``` käynnistin virtuaaliboksin, jonka jälkeen komennolla ```vagrant ssh``` menin ns. sisälle. 

Tässä vaiheessa näytti tältä: 

![bullseye](https://user-images.githubusercontent.com/118457367/205927753-f38a407c-edf8-4863-9f57-31f9e608e376.jpg)

