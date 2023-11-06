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

* Tutoriaali kolmen virtuaalikoneen luomiseen.
* Ohjeessa luodaan yksi master ja kaksi orjaa.
* Ohjeesta löytyy myös komennot niiden ohjaamiseen.

Lähde: https://terokarvinen.com/2023/salt-vagrant/

## a) Asenna Vagrant

Ladataan Vagrantin Windows versio sivulta: https://developer.hashicorp.com/vagrant/downloads.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ca17a1c4-51ee-4026-9fea-db709c46b55e)

Odotetaan, että se on asentunut ja käynnistetään kone uudelleen.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/fc84354f-3272-4278-b614-ed2d2d297875)

## b) Yksi maankiertäjä. Asenna yksi kone Vagrantilla, ota siihen SSH-yhteys, osoita että netti toimii.

Ensin luodaan virtuaalikone. 

    vagrant init debian/bullseye64

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ee979c87-3411-425f-b7b3-6091bbe8702a)

Varmistetaan että VagrantFile on luotu.

    dir

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/76b1348f-c437-4699-b805-6f9877f5f168)

Tiedosto löytyy, joten käynnistetään virtuaalikone.

    vagrant up

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/eae506a6-fc13-45ea-b004-5e40c086ee70)

Uusi virtuaalikone on luotu VirtualBoxiin.

Otetaan seuraavaksi yhteys virtuaalikoneeseen 

    vagrant ssh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/f3fb2258-2ec5-4b55-8ea8-4bb25af8e69e)

Osoitetaan vielä, että nettiyhteys toimii. 

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/340dd663-492b-4fbc-a2c9-9891dea9d80d)

Näyttää toimivan.

Lopuksi poistan virtuaalikoneen.

    $ exit
    vagrant destroy


![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/6df40a23-9f88-4b6b-9132-c51228e29d81)


Apuna käytin sivua: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/

## c) Oma orjansa. Asenna Salt herra ja orja samalle koneelle

Luodaan uusi kone ja otetaan siihen yhteys samalla tavalla, kuin aikaisemmassa tehtävässä.

    vagrant init debian/bullseye64
    vagrant up
    vagrant ssh

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

Lähdin testaamaan tätä ohjeilla, jotka löytyvät näiltä sivulta: https://terokarvinen.com/2023/salt-vagrant/

Koska käytän Windowsia, niin tiedoston tekeminen ja sen täyttäminen valmiilla Vagrantfile pohjalla eroaa ohjeista. Ensimmäiseksi yritin netistä katsoa miten tehdä tekstitiedosto PowerShellin kautta. Löysin ohjeen, josta löytyi komennot:

    notepad filename.extension
    notepad SampleDoc.txt

Lähde: https://techpp.com/2021/08/22/create-file-using-command-prompt-guide/

Testasin PowerShellissä 
      
    notepad Vagrantfile

Sain luotua uuden tekstitiedoston, johon kopioin valmiin Vagrantfile pohjan, joka löytyy tehtävän alussa mainitusta linkistä.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/61062cce-baf7-44d8-85e7-8a3954fca2a4)

Seuraavaksi jatkoin annetuilla ohjeilla, eli

    vagrant up

Kaikki sujui hyvin ja sain virtuaalikoneet tehtyä.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/6f4383e9-d3aa-48b3-a4ba-9b5c3591d258)

Seuraavaksi otetaan ssh yhteys masteriin.

    vagrant ssh tmaster

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/bdae34b8-3ca4-4a62-8fa2-6c44ca2bfd24)

Hyväksytään orjakoneiden avaimet.

    $ sudo salt-key -A

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/66e8e21c-b10f-49c6-9039-72f02922ab0b)

Ja kokeillaan, että yhteys toimii.

    $ sudo salt '*' test.ping

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/44deabf6-17ef-4f8f-9afa-f2d0aa08486a)

Kaikki näyttää olevan kunnossa. Seuraavassa tehtävässä käytän samoja koneita komentojen ajamiseen verkon yli.

## e) Aja useita idempotentteja (state.single) komentoja verkon yli

Ensimmäisenä annoin komennon

    $ sudo salt '*' state.single pkg.installed apache2

Eli apache2 pitää olla asennettuna. 

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/7311ec10-48ab-4004-b19c-49160e76f753)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/71d3796a-70ef-4483-aa3b-aa47a5eb0e3c)

Kummallakaan koneella niitä ei ollut vielä asennettuna, joten asennus tehtiin. Jos ajan saman komennon uudestaan, niin muutoksia ei pitäisi enään tulla, joten idempotentti pitäisi olla saavutettuna.

    $ sudo salt '*' state.single pkg.installed apache2

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/6c19809e-c276-41e6-83e7-c188165ae097)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/3aef6369-bae7-4638-bf93-78657619a4c9)

Kummassakaan koneessa ei muutoksia. Idempotentti saavutettu.

Seuraavaksi teen samanlaisen testin, mutta nyt apache2 pitää olla käynnissä.

    $ sudo salt '*' state.single service.running apache2

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/98cc15f8-439a-4c6f-86eb-9a6f27bd0dda)

Muutoksia ei tarvittu tehtä. Minä en kuitenkaan halua, että ne ovat päällä.

     $ sudo salt '*' state.single service.dead apache2 

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/9f99fe37-2cfa-411c-adcc-711c6e365337)

Tila muuttui. Tehdään sama uudestaan, jotta saavutettaisiin idempotentti.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/936ace1c-07f6-4184-9b58-08bed847886b)

Ei muutoksia.

## f) Kerää teknistä tietoa orjista verkon yli (grains.item)

Testataan aluksi, että saan komennot toimimaan. Otin viime viikon tehtävästä komennon ja muokkasin sitä tähän tehtävään sopivaksi.

    $ sudo salt-call --local grains.item osfinger
    $ sudo salt '*' grains.item osfinger

Näytti toimivan, sillä sain vastaukseksi: 

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/4e78149b-36f9-4c54-9876-a2babc82cd22)

Testataan vielä vaikka tietoa kernelistä.

    $ sudo salt '*' grains.item kernel kernelrelease

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e4cbe6b7-f10e-4384-85bc-0e6b5484401a)

Lähteet: https://terokarvinen.com/2023/configuration-management-2023-autumn/

https://terokarvinen.com/2023/salt-vagrant/

## g) Aja shell-komento orjalla verkon yli

Lähden testaamaan tekemällä orjakoneisiin yksinkertaiset tiedostot.

    $ sudo salt '*' state.single cmd.run 'cat > stesti.txt'

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/685d12e8-4fa9-4e7c-8457-82dbf38e8c9f)

Lähteet: https://terokarvinen.com/2023/salt-vagrant/

## h) Hello, IaC

Lähdin seuraamaan https://terokarvinen.com/2023/salt-vagrant/ sivulla annettua ohjetta "Infra as Code - Your wishes as a text file". Aluksi luon hakemiston, ja sinne .sls tiedoston.

    $ sudo mkdir -p /srv/salt/hello
    $ sudoedit /srv/salt/hello/init.sls

Kopioin ohjeista init.sls tiedoston sisällön ja liitän ne omaan tiedostoon.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/de8dd3fa-bc6c-41f4-a8c6-f42bd894645b)

Seuraavaksi ajoin tiedoston.

    $ sudo salt '*' state.apply hello

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/8ad5bd31-c243-47d4-9b9d-604546a6d913)

Tarkastetaan, että tiedostot löytyvät.

    $ sudo salt '*' state.single file.managed '/tmp/infra-as-code'

Laitoin tiedoston samaksi mikä luotiin aikaisemmin, joten jos tiedosto tehtiin muutoksia ei pitäisi tulla.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/140d6a8e-d436-49b0-a0d2-3ccd79cae20a)

Muutoksia ei tullut.
    
