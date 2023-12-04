## x) Lue ja tiivistä

**Vapaavalintainen aiemman vuoden kotitehtäväraportti Saltin käytöstä Windowsilla.**

Raportiksi valitsin Johan Lindellin kotisivuilta löytyvän tehtäväraportin. Lähde: https://johanlindell.fi/palvelintenhallinta#h6

* Salt minion ohjelman Windowsille sai ladattua osoitteesta: https://docs.saltproject.io/en/latest/topics/installation/windows.html.
* Nykyään kuitenkin latauslinkki löytyy osoitteesta: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html#install-windows
* Ohjelman asennusvaiheessa täytetään tietoihin Masterin IP-osoite, sekä Minion koneelle nimi.
* Saltin asennus pitää tehdä myös Master koneelle komennolla '$ sudo apt-get install salt-master'.
* Avainten hyväksyminen master koneella '$ sudo salt-key' ja '$ sudo salt-key -A'.
* Tarkistus vielä master-minion yhteyden toimivuudesta. '$ sudo salt '*' cmd.run 'whoami''.

**Halonen, Rajala ja Ollikainen 2023: Installing Windows 10 on a virtual machine**

* Ladataan Windows 10 ISO tiedosto osoitteesta: https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise
* Luodaan uusi virtuaalikone VirtualBoxissa.
* Määritetään aikaisemmin ladattu ISO tiedosto virtuaalikoneelle ja muutetaan loputkin asetukset omaan käyttöön sopiviksi.
* Käynnistetään virtuaalikone ja valitaan levyksi ladattu Windows ISO.
* Käydään Windowsin asennusvaiheet läpi, ja luodaan uusi käyttäjä.

Lähde: https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md

**LSB Workgroup, The Linux Foundation 2015: Filesystem Hierarchy Standard**

* 

Lähde: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

## a) Asenna Windows virtuaalikoneeseen

Koska aikaisemmassa tehtävässä **Halonen, Rajala ja Ollikainen 2023: Installing Windows 10 on a virtual machine** oli tullut niin hyvät ohjeet, niin lähdin tekemään tehtävää sen avulla.

Ensimmäisenä lataan Windows 10 ISO English (Great Britain) 64-bittisen tiedoston virtualboxia varten. Seuraavaksi lähden muokkaamaan virtuaalikoneelle oikeat asetukset. 

Valitaan ladattu ISO tiedosto, sekä Windows versio.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/5330ab66-03b6-4cb8-a517-c66e8fd20b5d)

Valitsin 10GB RAM muistia, sekä 4 prosessoria.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/d05b4a75-a83d-44f7-97d1-a2eddf0a2a33)

Lopuksi vielä varmuuden vuoksi 50GB kovalevytilaa ja muihin asetuksiin en koskenut.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/abb0463b-58a6-4944-9272-743c75272a94)

Seuraavaksi lähden käynnistämään uutta virtuaalikonetta ja pääsen asennusvaiheeseen. Ensin valitaan kieliasetukset halutuksi, jonka jälkeen tuli Install now näkymä.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/62206974-e612-412c-b739-5ca109dc841a)

Lisenssivaiheen hyväksyin ja nyt valitaan asennustapa. Mennään Custom asetuksella, ja valitaan virtuaalikoneelle annettu 50GB kovalevy.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/29192445-0d92-4f46-808b-b36103313721)

Ja nyt annetaan sen asennella rauhassa, jonka jälkeen virtuaalikone käynnistyy uudelleen.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/ee2c412e-eb11-48ef-bf40-cfc0a6762d76)

Sitten päästään vielä vastailemaan kysymyksiin ja luomaan käyttäjä. Valitaan, että olen Suomesta, jonka jälkeen näppäimistön asetukset. Sekin Suomi.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e771c468-0375-4b27-a065-a79a712b85b9)

Nyt pyydetään kirjautumaan sisään. Vasemmassa alakulmassa on "Domain join instead". Painetaan sitä ja valitaan käyttäjälle jokin nimi ja turvallinen salasana.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e8f2e840-08f8-47fe-8d23-a67847d2be74)

Pääsin vielä päättämään haluanko jakaa sijaintini ja muuta turhaa. Kaikkiin Ei.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/473ef23e-cae0-42e1-b511-93f25be016d9)

Ja näin minulla on Windows 10 virtuaalikoneella.

## b) Asenna Salt Windowsille. Osoita 'salt-call --local' komentoa ajamalla, että asennus on onnistunut

Teen testit viime tehtävässä asennetulla virtuaalikoneella.

Menin osoitteeseen: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html#install-windows , josta valitsin tiedoston **Windows downloads** alta. Sieltä sain .exe tiedoston, jolla voin asentaa saltin.

Jatkoin eteenpäin oletusasetuksilla ja painoin 'Install', jonka jälkeen 'Start salt-minion'

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e8c966be-5a89-4116-92b9-5c33980839ed)

Varmistetaan vielä, että asennus on onnistunut.

    salt-call --local --version

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/0bc73efb-46ec-422d-acd1-6301b872823d)
    
## c) Kerää Windows-koneesta tietoa grains.items -toiminnolla.




















