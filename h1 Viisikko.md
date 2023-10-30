## x) Lue ja tiivistä

 Karvinen 2023: Create a Web Page Using Github

- GitHub on nopea ja ja helppo tapa tehdä esimerkiksi nettisivuja ja raportteja.
- Ongelmien välttämiseksi on hyvä olla README tiedosto
- MarkDown tehdään kirjoittamalla .md tiedoston perään.

Lähde: https://terokarvinen.com/2023/create-a-web-page-using-github/

Karvinen 2023: Run Salt Command Locally

- Salt komentojen ajaminen paikallisesti on hyvä tapa harjoitella.
- Tärkeimmät tilafunktiot ovat pkg, file, service, user ja cmd.
- Salt:ia yleensä käytetään useiden koneiden hallintaan verkon välityksellä.

Lähde: https://terokarvinen.com/2021/salt-run-command-locally/

## a) Asenna Salt (salt-minion) koneellesi.

Käytin Salt:in asennukseen komentoja:


$ sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg https://repo.saltproject.io/salt/py3/debian/11/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg

$ echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/11/amd64/latest bullseye main" | sudo tee /etc/apt/sources.list.d/salt.list
$ sudo apt-get update

$ sudo apt-get install salt-minion

jotka löytyivät kurssin sivuilta. https://terokarvinen.com/2023/configuration-management-2023-autumn/

<img width="392" alt="Näyttökuva 2023-10-29 211746" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/b5b0b678-d333-44e0-a9c9-a66fd974942c">

<img width="392" alt="Näyttökuva 2023-10-29 212355" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/fb67a69e-d07c-4823-be7e-f8d0d313adcc">

## b) Viisi tärkeintä. Näytä esimerkit viidestä tärkeimmästä Saltin tilafunktiosta: pkg, file, service, user, cmd. Analysoi ja selitä tulokset.

pkg.installed: Sen mukaan sovelluksen pitäisi olla asennettuna tai poistettuna koneelta. Kun pkg.installed ajetaan ensimmäisen kerran, eikä koneella ole sille annettua sovellusta, se ladataan. pkg.removed on sama, mutta toisinpäin.

Käytin komentoa "$ sudo salt-call --local -l info state.single pkg.installed tree", joka tarkastaa onko tree asennettuna. 

<img width="329" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/f0087c3d-d029-4459-a617-c8130b62f18f">

Se ei ollut koneellani, joten se asennettiin, ja samalla tila muuttui (changed=1)

file.managed: Varmistaa, että koneelta löytyy komennolle annettu tiedosto.

ajoin komennon $ sudo salt-call --local -l info state.single file.managed /tmp/filetesti , joka katsoo, onko koneella /tmp/filetesti tiedostoa.

<img width="233" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/4146b19c-dd06-4218-95d8-0d078d1059d5">

Kun etsii kyseisen tiedoston, niin se löytyy

 $ less /tmp/filetesti

 <img width="389" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ac28e23e-0f76-4003-ae40-3608c24177cf">

Tiedoston sisältöä voi myös muuttaa lisäämällä file.managed komennon perään "contents="muokkaus"

Kun ajaa komennon $ sudo salt-call --local -l info state.single file.managed /tmp/filetesti contents="muokkaus", niin tiedosto näyttää tältä.

<img width="393" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/78877f99-011d-4a23-bcb3-562ece2df995">

$ sudo salt-call --local -l info state.single file.absent /tmp/filetesti , jos kyseistä tiedostoa ei pidä olla enään olemassa.

service.running: Käytetään, kun halutaan varmistaa palvelun pysyminen käynnissä tai suljettuna.

Toimii komennoila 

 $ sudo salt-call --local -l info state.single service.running apache2 enable=True

$ sudo salt-call --local -l info state.single service.dead apache2 enable=False

user.present: Varmistaa, että komennolle annettu käyttäjä on olemassa. Jos ei ole, se luodaan.

$ sudo salt-call --local -l info state.single user.present akkuut
 
Koska koneella ei ole akkuut käyttäjää, sellainen tehtiin.

<img width="391" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/184e4157-3423-4fb8-a4d6-57d6c3c22259">

<img width="196" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/47a094be-1553-442a-9e3c-dba5ca260744">

$ sudo salt-call --local -l info state.single user.absent akkuut

Varmistaa, että akkuut käyttäjää ei ole olemassa. Ei kuitenkaan poista akkuut:tia pois kotihakemistosta.

cmd.run: Ajaa komentoja. Käytetään, kun halutaan tehdä muutoksia. Testasing itse:

 $ sudo salt-call --local -l info state.single cmd.run 'touch /tmp/kukko' creates="/tmp/kukko"

Nyt /tmp/kukko tiedosto pitää tehdä.

<img width="251" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/54d1abf9-e0fa-43a8-988c-2896e5bb608e">

<img width="266" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/f1feeb20-2c10-48bc-a1c6-bafa8b812b32">

## c) Idempotentti. Anna esimerkki idempotenssista. Aja 'salt-call --local' komentoja, analysoi tulokset, selitä miten idempotenssi ilmenee.

Käytän idempotenssi esimerkkinä aikaisemmassa tehtävässä ladattua tree:tä. Idempotenssiin päästään, kun komento ajetaan, eikä muutoksia tarvitse tehdä. 

Esimerkiksi, jos ajan jo aikaisemmin ajetun komennon:

 $ sudo salt-call --local -l info state.single pkg.installed tree

 Muutoksia ei pitäisi tulla.

 <img width="305" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/31059bac-34a3-4992-9f5f-cc3575ad759d">

Kuten kuvassa näkyy, tree oli ladattuna jo koneelle, eikä muutoksia tarvittu. Jos kuitenkin ajaa komennon, jossa tree pitää olla poistettuna, niin joudutaan taas pois idempotenssista.

 $ sudo salt-call --local -l info state.single pkg.removed tree

 <img width="246" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/87a0b999-3af0-46bb-a8fd-9f42ddc2280e">

Muutoksia piti tehdä, joten ei oltu enään idempotenssisessa tilassa. Jos komennon ajaa uudelleen, pästään taas idempotenssiin.

<img width="290" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/713a5423-f3b8-49e0-b81c-e4f443d788a2">

d) Tietoa koneesta. Kerää tietoja koneesta Saltin grains.items -tekniikalla. Poimi kolme kiinnostavaa kohtaa, näytä tulokset ('grains.item osfinger virtual') ja analysoi ne.

Ajamalla komennon "$ sudo salt-call --local grains.items" sain pitkän listan koneen tiedoista.

<img width="400" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/fa290ee9-7d94-49c0-9c25-b6587a2f080e">

En ollut aikaisemmin juuri tutkinut oman kannettavan komponentteja, joten prosessori kohta pisti silmään.

<img width="238" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/477eed66-1684-4c42-8763-ea2c5e9c07e8">

Sitä kautta löytyi myös virtuaalikoneelleni antamat asetukset:

<img width="121" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/fd61cc4d-bbf3-4e49-b1dc-31f35dbb1a8c">

Sekä käyttöjärjestelmän tiedot:

<img width="117" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/5383fd94-1d59-4813-8851-786deb5b5620">

Kaikenkaikkiaan mukavasti pakattu lista järjestelmän tiedoista.
