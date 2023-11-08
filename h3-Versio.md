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




