## x) Lue ja tiivistä

**Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves**

* .sls tiedostoihin voi määrittää erilaisia tiloja, ja ne voidaan ajaa yhdellä komennolla.
* top.sls tiedostoilla voidaan määrittää, mille koneille tilat halutaan ajaa.

Lähde: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file

**Salt contributors: Salt overview**

* YAML "renderöiröijän" tehtävä on muuttaa YAML tietorakenne Python muotoon Salt:ia varten. 
* Kommentit alkavat "#" merkillä.
* Tab kielletty, käytetään vain välilyöntejä.
* Koostuu skalaareista, listoista ja hakemistoista.

Lähde: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml

**Salt contributors: Salt states**

* state tree eli tila puu pitäisi olla selkeä, jotta se on helposti luettava myös muille.
* Sisältää komponentit:
  - Identifier: tilan tunniste.
  - State: Funktion sisältävän tila moduulin nimi, esimerkiksi pkg.
  - Function: Funktio jota kutsutaan, esim installed.
  - Name: ajettavan tilan nimi, esim tiedosto tai ladattava paketti.
  - Agruments: mitä tilafunktio hyväksyy

Lähde: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules

**Karvinen 2018: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port**

* Hyvä ohje SSH server portin vaihtamiseksi.
* Vaihdetaan muokkaamalla sshd_config tiedostoa.

Lähde: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh

## a) Hello SLS! Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.

Käytin aikaisemmin h2 tehtävässä luotuja koneita tässäkin tehtävässä. Käynnistin koneet ja otin yhteyden master koneeseen.

    $ vagrant up
    $ vagrant ssh

Tehtävässä pyydettiin tehdä Hei maailma -tila esimerkiksi /srv/salt/hello/init.sls tiedostoon, joka minulta löytyy jo, joten lähdin muokkaamaan sitä.

    $ sudoedit /srv/salt/hello/init.sls

Tutkittuani läksymateriaaleja en aivan saanut kiinni mitä haettiin, joten kävin katsomassa mallia Joonas Hautaviitan raportista. Lähde: https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md

Oma tiedostoni näyttää sen jälkeen tältä:

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/74c28e4c-dda6-4f35-a5ca-7f6862eff3ae)

Seuraavaksi lähden testailemaan toimivuutta. Koska kyseessä on sama tiedosto, kuin h2 tehtävässä, käytän silloin annettua komentoa.

    $ sudo salt '*' state.apply hello

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/25cfeea3-7a65-450a-857d-6057ec562099)

Kaikki näyttää toimivan. ...

## b) Top. Tee top.sls niin, että tilat ajetaan automaattisesti

 Aloitan tekemällä top.sls tiedoston.

     $ sudoedit /srv/salt/top.sls

Otin mallia tiedoston sisältöön Tero Karvisen **top.sls - What Slave Runs What States**. Lähde: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/69453226-1f42-4b47-8128-a213c68f7069)

Ja nyt kaikki orjat '*' ajavat aikaisemmassa tehtävässä luodun Hei maailma -tilan.

    $ sudo salt '*' state.apply

Saatiin sama lopputulos kuin aikaisemmin.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/2864f967-f179-4a04-8dfb-30b4bbe761e8)

## c) Apache. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy

Lähdetään aluksi asentamaan apache ja katsotaan, että se toimii.

        $ sudo apt-get install apache2
        $ sudo systemctl status apache2

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/cef7ad76-18b7-4168-8e8a-44f1216fff93)

Tehtävässä pyydetään korvaamaan testisivu.

        $ cd /var/www/html
        $ sudo rm index.html
        $ sudoedit korvaus.html
        $ cat korvaus.html

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/419aa51a-0681-4bee-a9c3-13319f0552db)

Testataan vielä apachen toimivuus.

        $ sudo systemctl status apache2

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/2e544551-9847-4436-bdeb-8f3d7102109f)

Lopuksi poistan vielä apachen.

        $ sudo service apache2 stop
        $ sudo apt-get purge apache2

Seuraavaksi testataan automatisointia. Luon /srv/salt/ hakemistoon uuden tiedoston, johon lähden testailemaan.

        $ sudo mkdir apatest
        $ sudoedit init.sls

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/73e26cf0-5a8b-481c-ba7b-f4bae068fab3)

Lähdin ottamaan aluksi mallia saltprojektin **Create the Apache state** ohjeesta. Lähde: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules

Sain apachen asennusksen keksittyä itsenäisesti, mutta muut kohdat eivät tulleet yhtä luonnollisesti, joten kävin hakemassa taas apua Joonaksen raportista. https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md

Lopuksi tiedostoni näyttää tältä:

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/d2f305dc-3d4f-446e-94c6-2f92886438df)

Nyt testataan lopputulos sormet ristissä.

        $ sudo salt '*' state.apply apatest

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/c5fdf71e-523c-484e-b047-db9da605b07a)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/2b6c96f2-f928-4b73-829e-880dc16e2b3d)

Molemmilla orjilla tuli sama tulos, joten kaikki näyttää toimivan niinkuin pitääkin.

## d) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.

Vaihdoin virtuaalikoneeni Debianiin, jossa SSH oli jo ladattuna.

Päivitettyäni koneen, menin ssh:n config tiedostoon.

        $ sudoedit /etc/ssh/sshd_config

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ecbf74ee-8518-47fc-b402-6a8d4a74d75c)

Heti alussa tuli vastaan portti, joka nyt näyttää olevan 22.

Testasin yksinkertaisesti mennä ohjeilla, jotka on annettu sivulla: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh

Siellä lukee, että poistetaan "#" ja vaihdetaan porttinumero ja käynnistetään demoni, jotta saadaan uudet asetukset käyttöön.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/4534dba8-d493-448d-8643-e156010094db)

Tallennan tiedoston ja käynnistän ssh:n uudelleen ja yritän päästä localhostille sisään.

        $ sudo systemctl restart sshd.service
        $ ssh -p 1234 tuukkah@tuukka-virtualbox

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/4ba29f8c-23e8-4a87-8788-35bf79efd03a)

Kaikki näyttää toimivan hyvin.

Vinkit tehtävään katsoin osoitteesta https://terokarvinen.com/2023/configuration-management-2023-autumn/.
