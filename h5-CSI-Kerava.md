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

