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

