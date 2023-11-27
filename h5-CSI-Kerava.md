## x) Lue ja tiivistä

**Karvinen 2018: Apache User Homepages Automatically – Salt Package-File-Service Example**

- Tärkeää on ensiksi tehdä kaikki manuaalisesti, ennen kuin kokeilee automaatiota.
- Tiedostojen muokkaushistorian voi nähdä komennolla 'sudo find -printf "%T+ %p\n"|sort'.
  * %T+ viittaa muokkausaikaan.
  * %p tiedoston nimi ja hakemistopolku
  * \n uutta riviä
- 

Lähde: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/

## a) CSI Kerava. Näytä 'find' avulla viimeisimmäksi muokatut tiedostot /etc/-hakemistosta ja kotihakemistostasi. Selitä kaikki käyttämäsi parametrit ja format string 'man find' avulla.

Testasin ensiksi alla olevaa komentoa kotihakemistossa. 

    $ find -printf '%T+ %p\n'|sort

'find' komentoa käytetään tiedostojen etsimisessä. Se näyttää nykyisessä hakemistossa olevat tiedostot. '-printf '%T+ %p\n'|sort' komennolla voidaan vielä määrittää lisäarvoja. '%T+' näyttää päivämäärän ja ajan, sekä '+' erottaa ne + merkillä. '%p\n' kertoo tiedoston nimen sekä hakemistopolun, ja tekee uuden rivin jokaiselle tiedostolle. '|sort' lajittelee listan aikajärjestykseen. Lähde: 'find' komennon man-sivut.

Kuva komennon testailusta kotihakemistossa:

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/d425d7fa-1c46-4908-aad1-92a057f2f3cb)

Ja sama /etc hakemistossa:

    $ cd /etc
    $ find -printf '%T+ %p\n'|sort

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e7d73295-d7d4-4c8c-9e26-67edeef29971)

## b) Gui2fs. Muokkaa asetuksia jostain graafisen käyttöliittymän (GUI) ohjelmasta käyttäen ohjelman valikoita ja dialogeja. Etsi tämä asetus tiedostojärjestelmästä.

Lähden testaamaan muokkaamalla "Appearance" asetuksia, jonka jälkeen annan '$ find -printf '%T+ %p\n'|sort' komennon, ja katson mitä tiedostoja on muokattu.

![1](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/a08212cc-d224-4b9f-87fb-c6a7580c788d)

Muokkasin tyyliä kello 20:14, joten nyt vertaan sitä komennolla saatuihin muutoksiin.

    $ find -printf '%T+ %p\n'|sort

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/d2299511-c596-43b6-a19a-a289904189f1)

'|sort' avulla muutokset listattiin aikajärjestyksessä, ja pohjalta löytyi ylhäällä näkyvät muutokset kellonajalla 20:14. Oletan siis, että listatut tiedostot ovat vastuussa tyylin muokkaamisesta.

## c) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.

Lähden testailemaan tehtävää aikaisemmin vagrantilla tehdyillä koneilla. Avaan siis Windowsissa PowerShellin ja käynnistän koneet tmaster, t001 ja t002, ja otan yhteyden tmasteriin.

    $ vagrant up
    $ vagrant ssh

Lähden aluksi tekemään uutta komentoa manuaalisesti. Luon uuden .sh tiedoston, jonne keksin yksinkertaisen skriptin.

    $ nano morning.sh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/febd143f-0b32-4a7a-8cde-9ccdd1119114)

Siinä yksinkertainen skripti. Nyt testataan, että kaikki toimii.

    $ bash morning.sh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/29fa8765-8ebc-4455-8827-746139b1291d)

Hyvältä näyttää! Seuraavaksi kopioidaan tiedosto /usr/local/bin/ hakemistoon, jotta komennon saa käyttöön kiakilla käyttäjillä, ja katsotaan onko oikeudet kunnossa.

    $ cd /usr/local/bin/
    $ sudo cp /home/vagrant/morning.sh /usr/local/bin/
    $ ls morning.sh -l

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/0836678b-ac4b-4d7e-8c86-98cdc008c55b)

Nyt rootilla näyttäisi olevan luku- ja kirjoitusoikeudet, ja muilla vain lukuoikeudet. Ei muuteta niitä sen enempää. 

Seuraavaksi testataan tehdä samaa saltilla. Luon salt hakemistoon uuden morning hakemiston, jonne kopioin morning.sh tiedoston, sekä teen init tiedoston.

    $ cd /srv/salt/
    $ sudo mkdir morning
    $ sudo cp /usr/local/bin/morning.sh /srv/salt/morning
    $ sudoedit init.sls

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/6ad527e4-d282-4f9d-994b-b876832dd914)

Seuraavaksi lähden ajamaan tiedoston molemmille orjille.

    $ sudo salt '*' state.apply morning

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/201bdbef-0566-4650-9119-9954809e14bf)

Tiedostot luotiin molemmille orjille. Seuraavaksi testaan vielä komentoa.

    $ sudo salt '*' cmd.run morning

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/a1bb6bce-2099-4aa9-a4c8-b85724c88dbb)

Tuli ilmoitus, että "Permission denied". Huomasin, että orjille luoduilla tiedostoilla oli oikeudet 0644, ja huomasin, että millään ryhmällä ei ollut ajo-oikeuksia ja annoin niille cmd.run, eli ajo komennon. Kävin siis init tiedostossa muokkaamassa oikeuksia.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/bc9e6df9-e14a-45d1-8ef0-9d7ec1f68a65)

Testasin toimiiko komento, jos annan kaikille ryhmille pelkästään ajo-oikeudet. Ensin varmistetaan muutokset ja testataan komentoa uudestaan.

    $ sudo salt '*' state.apply morning
    $ sudo salt '*' cmd.run morning

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/c87e2463-5600-41d7-940e-10143e714b7a)

Sillä näyttäisi ongelma ratkeavan. Nyt orjalle kirjautuessa tiedostoa ei voi ajaa esim. bash komennolla, taikka lukea tai muokata ilman sudoa. Ajaminen kuitenkin onnistuu paikallisesti.

    $ exit
    $ vagrant ssh t001
    $ cd /usr/local/bin/
    $ sudo salt-call --local cmd.run morning

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/17136cc4-0a10-47a8-968c-18f9a4f1cad7)

Apua init tiedostoa varten hain: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.file.html

## d) Apassi. Tee Salt-tila, joka asentaa Apachen näyttämään kotihakemistoja

Lähden seuraamaan ohjeita Tero Karvisen **Apache User Homepages Automatically – Salt Package-File-Service Example** mukaan. Lähde: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/

 Aloitan luomalla /srv/salt hakemistoon apache hakemiston.

     $ cd /srv/salt
     $ sudo mkdir apache

 ![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/5cad8998-6d53-432e-b403-80a05cfd05de)

Sekä luon sinne init.sls tiedoston.

    $ cd apache
    $ sudoedit init.sls

Init tiedostoon kopioin ohjeista kohdan **Better Way with Files** tiedoston sisällön.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/2e78bfa5-7f44-4097-96f2-40cb78d9a92f)

Tässä vaiheessa on hyvä tehdä muokkaukset kotisivuun. Koska laitoin init tiedostoon lähteeksi "salt://apache/default-index.html" niin teen saman nimisen tiedoston /srv/salt/apache hakemistoon.

    $ sudo nano default-index.html
    $ cat default-index.html

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/8610fa05-91fa-42c4-87d0-cde8f6a92056)

Seuraavaksi ajetaan muutokset.

    $ sudo salt '*' state.apply apache

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e596614c-d5a8-49d1-9aa9-2923b4622b93)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/a7fae0b5-124a-41e7-8621-e7d7f19f3abd)

Kaikki vaiheet näyttivät onnistuvan. Seuraavaksi testataan näkyykö muutokset. Teen testin jo aikaisemmin minulle tutulla tavalla, eli w3m selaimella, joka toimii komentokehotteen kautta.

    $ sudo apt install w3m
    $ w3m http://localhost

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/8f582eed-3299-4686-a743-646651d31da4)

Näyttäisi toimivan.
