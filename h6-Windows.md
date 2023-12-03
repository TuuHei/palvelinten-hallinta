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
