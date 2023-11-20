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

* 

Lähde: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules

**Karvinen 2018: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port**


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



