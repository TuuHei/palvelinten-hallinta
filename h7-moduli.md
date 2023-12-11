Tämän modulin tarkoituksena on lähteä yrittämään tekemään komentoa, jolla voi muokata Debian virtuaalikoneen teemaa.

Aloitan etsimällä tiedoston, joka on vastuussa teeman muokkaamisessa. Käytän apuna aiempaa raporttiani [h5-CSI-Kerava.md](https://github.com/TuuHei/palvelinten-hallinta/blob/main/h5-CSI-Kerava.md). Kävin muokkaamassa teemaa, jonka jälkeen katsoin komennolla mitä tiedostoa muokattiin.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/2e3c2015-9a52-411d-953c-229f3a82352f)

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/28715e19-e2b3-4ae4-9b07-0d9fde59f032)

    $ find -printf '%T+ %p\n'|sort

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/5838cc1e-95f3-4681-b6a1-d8050867d105)

Tiedosto näyttäisi olevan **./.config/xfce4/xfcong/xfce-perchannel-xml/xsettings.xml**. Käyn katsomassa miltä se näyttää.

    $ nano ./.config/xfce4/xfcong/xfce-perchannel-xml/xsettings.xml

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/2e30f51b-62af-4469-8566-dfb948f8c858)

Tässä tiedosto kaikessa kauneudessaan. Tiedostosta näkee samalla rivillä "ThemeName" sekä nykyinen käytössä oleva teema "Adwaita-dark". Tarkoituksena olisi siis luoda komento, joka tunnistaa nykyisen teeman ja pystyy muokkaamaan sen haluttuun teemaan. 

Löysin komennon, jolla voi korvata tekstiä tiedostossa. Lähde: https://www.cyberciti.biz/faq/how-to-use-sed-to-find-and-replace-text-in-files-in-linux-unix-shell/

Katsotaan mitä käy, kun ajan komennon.

    $ sed -i 's/Adwaita-dark/HighContrast/g' ./.config/xfce4/xfconf/xfce-perchannel-xml/xsettings.xml
    $ nano ./.config/xfce4/xfcong/xfce-perchannel-xml/xsettings.xml

Ensimmäinen Adwaita-dark, on nykyinen tiedostosta löytyvä teema ja sen jälkeen tuleva HighContrast on korvaava teema.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/9f0a8adf-edcf-4ad7-b59b-01868380d4db)

Tiedostossa teema näyttäisi muuttuvan niinkuin pitääkin. Huomasin kuitenkin, että muutokset eivät tule heti voimaan, tulevat mutta viimeistään koneen uudelleen käynnistyessä. En ole löytänyt tähän vielä ratkaisua. Jatkan kuitenkin näillä eteenpäin.

Luin netistä, että asetukset, jotka varastoidaan .xml tiedostoihin eivät muutu heti... Ikävää mutta tällä on pakko jatkaa. Lähde: https://wiki.archlinux.org/title/xfce **3 Configuration**

Jatketaan luomalla skripti. 

    $ nano mustatila.sh
    
![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/07fdc123-daa0-477c-9ab4-0a123b3d75c4)

Yritin luoda if else ratkaisulla komennon, joka vaihtaisi teemaan "Adwaita-dark" teemaan, jos se ei ole tiedostossa. 

Koska en löytänyt tietoa miten saisin muutokset päivitettyä heti, niin "nerokkaasti" komento käynnistää koneen uudelleen :D

Varmasti skriptin saisi tehtyä nerokkaamminkin, mutta tähän pääsin nykyisellä ajalla.

Pääsin aloittamaan työhön panostamisen liian myöhään, ja olisin mielelläni halunnut käyttää enemmän aikaa järkevän ja toimivan projektin tekemiseen. Aion työstää työtä vielä eteenpäin ja etsin mahdollisia ratkaisuja ja parempia menetelmiä.

Tässä vielä kuvat testailuista. Aluksi minulla on jo darkmode käytössä.

    $ bash mustatila.sh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/67403557-45cb-40c6-b3d2-e6059d28220c)

Tehdään testi muuttamalla **HighContrast** teema käyttöön.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/72cbc89e-08a8-4a05-a5fa-c0808ba34119)

    $ bash mustatila.sh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/06d8add9-7ca2-45f2-93a1-4aaeb5b35b88)

Ja lopuksi vielä **Adwaita** teema.

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/cc8a76e8-822a-48fd-a340-24a164eeab08)

    $ bash mustatila.sh

![kuva](https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/a4087469-0667-4d94-a9e4-beaa6825e89c)


Ainakin toimivat...

## Lähteet

https://www.cyberciti.biz/faq/how-to-use-sed-to-find-and-replace-text-in-files-in-linux-unix-shell/

https://wiki.archlinux.org/title/xfce

https://github.com/TuuHei/palvelinten-hallinta/blob/main/h5-CSI-Kerava.md
