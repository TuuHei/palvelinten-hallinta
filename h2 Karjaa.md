## x) Lue ja tiivistä

**Slater 2017: What is the definition of "cattle not pets"?**

* Aikoinaan serveteitä kohdeltiin kuin lemmikkejä. Jos jotain oli vialla, sen korjaaminen oli prioriteettilistan yläpäässä.
* Nykyyän se on verrattavissa enemmän karjaan.
* Serverit on numeroitu, sekä helpommin korvattavissa.

Lähde: https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654

**Karvinen 2017: Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds**

* Kertoo miten asentaa uusi virtuaalikone 31 sekunnissa
* Tarvitsee vain viittä komentoa.
* Kun haluaa poistaa luodun virtuaalikoneen, niin 'vagrant destroy'.

Lähde: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/

**Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves**

* Tutoriaali kolmen virtuaalikoneen hallintaan.
* 

Lähde: https://terokarvinen.com/2023/salt-vagrant/

## a) Asenna Vagrant

Ladataan Vagrantin Windows versio sivulta: https://developer.hashicorp.com/vagrant/downloads.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ca17a1c4-51ee-4026-9fea-db709c46b55e)

Odotetaan, että se on asentunut ja käynnistetään kone uudelleen.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/fc84354f-3272-4278-b614-ed2d2d297875)

## b) Yksi maankiertäjä. Asenna yksi kone Vagrantilla, ota siihen SSH-yhteys, osoita että netti toimii.

Ensin luodaan virtuaalikone. 

    $ vagrant init debian/bullseye64

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ee979c87-3411-425f-b7b3-6091bbe8702a)

Varmistetaan että VagrantFile on luotu.

    $ dir

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/76b1348f-c437-4699-b805-6f9877f5f168)

Tiedosto löytyy, joten käynnistetään virtuaalikone.

    $ vagrant up

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/eae506a6-fc13-45ea-b004-5e40c086ee70)

Uusi virtuaalikone on luotu VirtualBoxiin.

Otetaan seuraavaksi yhteys virtuaalikoneeseen 

    $ vagrant ssh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/f3fb2258-2ec5-4b55-8ea8-4bb25af8e69e)

Osoitetaan vielä, että nettiyhteys toimii. 

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/340dd663-492b-4fbc-a2c9-9891dea9d80d)

Näyttää toimivan.

Lopuksi poistan virtuaalikoneen.

    $ exit
    $vagrant destroy


![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/6df40a23-9f88-4b6b-9132-c51228e29d81)


Apuna käytin sivua: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/

## c) Oma orjansa. Asenna Salt herra ja orja samalle koneelle

Luodaan uusi kone ja otetaan siihen yhteys samalla tavalla, kuin aikaisemmassa tehtävässä.

    $ vagrant init debian/bullseye64
    $ vagrant up
    $ vagrant ssh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/8687bf8a-0a83-419a-ae0b-d432485484cf)

Seuraavaksi luodaan herra ja orja.

    $ sudo apt-get update
    $ sudo apt-get -y install salt-master
    $ sudo apt-get -y install salt-minion

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/76e9641f-f1d4-4013-a7c4-aba8e78fa59f)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/4a541cc6-6cf9-4bea-a125-16637bb407f5)

Tarkastetaan vielä, että molemmat toimivat.

    $ sudo service salt-master status
    $ sudo service salt-minion status

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ff3e1c54-b0c3-4698-bcac-c6b22bbc5439)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/0fb5a488-9a91-471d-99ff-dcd0d687b6a6)

Molemmat ovat aktiivisia.

Apuna käytin: https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/

## d) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli
