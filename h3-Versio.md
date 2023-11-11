## a) Online. Tee uusi varasto GitHubiin

Uuden varaston voi luoda GitHubissa menemällä **Create new** --> **New repository**. Seuraavaksi täytetään tarvittavat tiedot.

<img width="365" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/770440ed-ec5b-43d9-b9b3-a4f6ff7ba6b3">

Näillä tiedoilla se on valmis luotavaksi.

<img width="460" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/27bf9564-7d78-4a3f-bd58-3611190dc15c">

## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.

Aloitan luomalla Debianin puolella avainparin, jotta voin antaa julkisen avaimen GitHubiin.

    $ ssh-keygen

<img width="381" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/f9acc025-5e19-48ac-ac60-428f51c3a2fb">

Haetaan avain 

        $ cat $HOME/.ssh/id_rsa.pub

<img width="400" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/7be4915a-16da-4085-8154-c08fa940368c">

Ja annetaan se GitHubiin.

Setttings --> 

<img width="272" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/7c194731-26e6-4e4d-b9be-37329bb2d7c2">

Seuraavaksi koitetaan kloonata varasto linuxiin. Ensin pitää hakea linkki GitHubista.

<img width="199" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/e2951455-eb49-4ca4-a23c-d9c0b999795e">

Seuraavaksi

        $ git clone git@github.com:TuuHei/winter-tasks.git

Tässä vaiheessa on hyvä huomata, että Gittiä ei ole ladattu koneelleni, joten hoidetaan senkin asennus pois alta.

        $ sudo apt-get install git-all

<img width="302" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/93b51448-74b6-4698-ad34-205c9fc9a433">

Yritetään kloonata varasto uudelleen.

        $ git clone git@github.com:TuuHei/winter-tasks.git

<img width="395" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/3eaa6e82-f5ac-4542-a3c2-6569201bf8cc">

Katsotaan löytyykö varasto.

        $ cd winter-tasks

<img width="232" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/5c13646f-999b-48dc-b21a-cc162be342d0">

Hyvältä näyttää. Seuraavaksi yritetään tehdä muutoksia johonkin tiedostoon ja pusketaan ne palvelimelle. Testaan vaikka README tiedostoa.

        $ nano README.md

<img width="253" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/86aa4a2e-85eb-467d-9571-5f80dff0a17a">

Kun muutokset on tehty, niin annoin komennot samassa järjestyksessä kuin ohjeissa. https://terokarvinen.com/2023/configuration-management-2023-autumn/

        $ git pull
        $ git add .
        $ git commit
        $ git pull
        $ git push
        $ git log --patch

<img width="394" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/de3fe2e6-5c9b-4ac3-af76-f95949d3bf8e">

Ja GitHub näyttää tältä

<img width="308" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/0f0fba1d-7c0f-439c-88e3-ea1829c8954a">

Muutokset siis onnistuivat.

## c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.

Aloitin tekemällä uuden tiedoston /winter-tasks hakemistoon. Tyhmänä muutoksena tein normaalin tekstitiedoston, jonne kirjoitin salasanan ja toisen tiedoston nimeltä paivakirja.txt

    $ nano paivakirja.txt
    $ nano salasanat.txt
    $ cat salasanat.txt

<img width="310" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/4f0d161d-8d68-44d2-b417-d5d6dc0be19f">

Seuraavaksi valmistelen niiden lähettämistä gittiin.

    $ git pull
    $ git add .

Nyt kuitenkin tajuan virheeni, ja haluan poistaa tekemäni muutokset.

    $ git reset --hard

<img width="298" alt="image" src="https://github.com/TuuHei/palvelinten-hallinta/assets/122973223/52693fc3-84d4-4bde-9cd2-cf34aaa80ad5">

Tiedostot ovat poistuneet.




