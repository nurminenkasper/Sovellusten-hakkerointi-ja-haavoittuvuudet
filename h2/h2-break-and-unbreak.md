# h2 - Break & unbreak

## x) Lue/katso/kuuntele ja tiivistä

### OWASP Top 10: A01 Broken Access Control
- Broken Access Control, eli rikkinäiset käyttöoikeudet on yleisin haavoittuvuus OWASP Top 10 listalla
- Broken Access Control johtaa siihen, että käyttäjät pääsevät käsiksi sovelluksen sisältöön, joihin heillä ei ole oikeutta päästä
- Toimintoihin liittyy olennaisesti tietojen luvaton näkeminen, paljastaminen, muuttaminen sekä poistaminen.
- Parhaimmat tavat estää on: Luotettavan palvelinpuolen koodin käyttäminen, estä oletuksena pääsy kaikkiin resursseihin paitsi julkisiin sekä toteuttaen pääsynhallinta kerran ja käytäen sitä uudelleen koko sovelluksessa, samalla minimoiden CORS-käyttö.
### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf
- Fuff on joohoi:n kehittämät web fuzzer
- Fuff avulla voi löytää web-palvelimilta piilotettuja hakemistoja, otsikoita tai POST-parametrejä
- Fuffia voi testata paikallisesti ilman internet-yhteyttä. Tärkeää onkin muistaa, että penetraatiotestauksen teknkiikoiden käyttö tulee suorittaa lain sekä eettisten perjaatteiden puitteissa.
### PortSwigger: Access control vulnerabilities and privilege escalation
- Artikkelissa käsitellään pääsynhallintaa ja se on jaettu kahteen eri pystysuoraan & vaakasuoraan pääsynhallintaan.
- Pystysuora pääsynhallinta rajoittaa pääsyä tiettyihin toimintoihin käyttäjätyypin mukaan, esimerkiksi jaottelua ylläpitäjien ja tavallisten käyttäjien välillä
- Vaakasuora pääsynhallinta rajoittaa pääsyä resursseihin tiettyjen käyttäjien välillä, kuten esimerkiksi käyttäjän omiin pankkitileihin ja siirtoihin liittyviin tietoihin
- Pääsynhallintaa voidaan rikkoa useilla eri tavoilla, näistä artikkeleissa on muutamia esimerkkejä.
- Käyttäjä pääsee käsiksi toimintoihin, joihin hänellä ei ole oikeutta kuten esimeriksi tavallinen käyttäjä pääsee admininstrator-sivulle. Tätä kutsutaan pystysuoran pääsynhallinnan rikkomiseksi.
- Suojaamaton toiminnallisuus on hyvin yleinen tapa rikkoa pääsynhallintaa. Tässä tavassa esimerkiksi administrator sivuja ei ole suojattu pääsyltä ja niihin pääsee käsiksi kuka vain syöttämällä oikean osoitteen
### Karvinen 2006: Raportin kirjoittaminen
- Raportin tulisi olla täsmällinen ja toistettava. Raportissa tulisi atkiivisesti kertoa mitä teit ja mitä tapahtui. Raportin kirjoittaminen työtä tehdessä on suositeltavaa.
- Raportissa olisi hyvä myös sisällyttää ympäristö, jossa työ tehtiin, koska monet asiat ja mahdolliset ongelmat toimivat sekä ilmenevät eri tavalla tietokoneissa ja verkoissa.
- Kirjoita menneessä aikamuodossa ja kerro tarkasti, mitä komentoja annoit tai mitä klikkasit. Raportoi kaikki tulokset ja mahdolliset viat ohjelmissa, työkaluissa ja laitteissa.
- Raportin tulee olla helppolukuinen. Käytä väliotsikoita, kirjoita selkeää kieltä ja viittaa mahdollisiin lähteisiin.
- Vältä sepittämistä, plagiointia ja kuvien luvatonta käyttöä.

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home

## a) Murtaudu 010-staff-only
Tehtävää suoritin Debian 12 virtuaalikoneen puolella. Tehtävää edeltävästi asensin tarvittavat python3-flask & python3-flask-sqlalchemy paketit.

      sudo apt-get -y install python3-flask python3-flask-sqlalchemy

Purettuani tehtäväpaketin kansioon, suoritin staff-only.py ohjelman ja avasin sen Firefox selaimessa localhost osoitteesta.

Testailin alkuun eri numerosarjoja, mutta niillä ei kovin pitkälle päässyt. Yritin heti perään syöttää SQL tekstiä, mutta tekstikenttään pystyi syöttämään vain numeroita.

Lähdin taklaamaan ongelmaa avaamalla F12 näppäimellä Inspector näkymän ja valitsin syötekentän. Muokkasin input type number -> text. Tällä tavalla pystyis syöttämään myös tekstiä kenttään. 

![K1](1.png)

Yritin ensimmäisenä syöttää kenttään:

      0'+OR+1=1 --

![K2](2.png)

Mutta palautteena sain vain Internal Server Erroria, joten paluu mietintämyssyyn edessä. Muokkailin hieman syötteen rakennetta ja yllätyksekseni sain vastaukseksi foo.

      0' OR '1=1'--

![K3](3.png)

Foo, mikä ihmeen foo? Pitkän tovin mietiskelin, mutta en keksinyt mitään jatkoa lauseelle millä paljastaisin muita salasanoja. Teron vinkeistä löysin kuitenkin ratkaisun mietiskelyyn ja rupesin kokeilemaan lisäämällä LIMIT loppuun.

      0' OR '1=1' LIMIT 2,1 --

![K4](4.png)

BINGO. Sieltähän se SUPERADMIN paljastui ruudulle toisella yrittämällä. Jäin vielä miettimään itsekseni tuota LIMIT käyttöä, koska se oli itselle aivan uusi asia. Pohdintoja salasanan murtavasta lauseesta:

      SELECT password FROM pins WHERE pin='0' OR '1=1' LIMIT 2,1 --'; 

Alkuperäinen ehto etsii SQL taulusta salasanaa, jonka pin on 0. OR '1=1' lisää ehdon, joka palauttaa kaikki salasanat joiden PIN on 0 tai tosi. Minkä seurauksena saadaan myös foo, mikäli ymmärrän oikein. Tämän jälkeen, kun lisätään LIMIT 2,1, se palauttaa vain tietyn rivin tuloksia, mistä tällä kertaa löytyykin SUPERADMIN.

Hieman vielä hakusessa, mutta tästä se lähtee rullaamaan.

## b) Korjaa 010-staff-only haavoittuvuus lähdekoodista
Pyörittelin tehtävän lähdekoodia pidemmän tovin microlla auki. Mahdollisesti löysin oikean rivin mitä muokata, mutta edes Hack-n-Fix sivuston vinkeillä en osaa lähteä parantamaan koodia niin, että siitä saisi toimivan.

## c) Ratkaise dirfuzt-1
Asentelin Githubista tuoreimman version 2.1.0 ffufista.

      wget https://github.com/ffuf/ffuf/releases/download/v2.1.0/ffuf_2.1.0_linux_amd64.tar.gz
      tar -zf ffuf_2.1.0_linux_amd64.tar.gz
      ./ffuf

![K5](5.png)

Lisäksi latasin Teron ohjeistukseta löytyvän suoran kirjaston, Seclists by Daniel Miessler

      wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
      head -3 common.txt 
      wc -l common.txt 

![K6](6.png)

Testailin ensin löytää salatun sivun dirfutz-0 tiedostosta Tero sivuilta löytyvän ohjeistuksen mukaan, että tajuan miten homma toimii. Ffufilla kun ajetaan ladatut kirjastot läpi, saadaan hieman lisätietoa etsimästämme.

      ./ffuf -w common.txt -u http://127.0.0.2:8000/.bash_history
      ./ffuf -w common.txt -u http://127.0.0.2:8000/.bashrc

![K7](7.png)

Sieltähän me löydetään vastauksia, mutta vielä pitäisi filteröidä ne vastaukset mitä halutaan. Käytetään Teron ohjeistuksen mukaista -fs filteröintiä, millä voidaan filteöidä koon (bytes) mukaan.

      ./ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 132

![K8](8.png)

Bingo. Admin sivusto löydetty ja kun testataan se vielä selaimessa toimivaksi, saadaan homma maaliin.

Seuraavaksi siirryin dirfuzt-1 tehtävän pariin. Käytin täysin samoja methodeja, mitä yläpuolella on kuvailtuna ja sieltä löytyi wp-admin & .git sivustot. 

![K9](9.png)
![K10](10.png)

## d) Murtaudu 020-your-eyes-only
Tehtävä käyntiin Debianilla asentamalla paketit ja riippuvuudet, kuten Virtualenv, että saadaan Djangolla testi serveri käyntiin, missä tehtävä suoritetaan.

      cd challenges/020-your-eyes-only/
      sudo apt-get -y install virtualenv
      virtualenv virtualenv/ -p python3 --system-site-packages
      source virtualenv/bin/activate
      cat requirements.txt
      pip install -r requirements.txt
      cd logtin/
      ./manage.py makemigrations; ./manage.py migrate
      ./manage.py runserver

![K11](11.png)

Lähdin heti ratkomaan tehtävää etsimällä ffufilla piilotettuja sivuja. Tämän tarpeesta olikin jo alustava tieto tunnilla opitusta ja käytin samaa metodia suorittamiseen, mitä tehtävässä C.

      ./ffuf -w common.txt -u http://127.0.0.1:8000/FUZZ

![K12](12.png)

Tästä saatiikin saaliiksi se, että sivustolta löytyy admin-console. Suunnistin sivulle http://127.0.0.1:8000/admin-console/ ja rupesin miettimään, miten kirjautuminen ratkaistaan. Lähestymistapaa yritin hakea pidemmän tovin SQL injektiona.

      Username: admin '--
      Password: ' '

![K13](13.png)

Tämä ei kuitenkaan tuottanut mitään tulosta, joten oli pakko turvautua Teron sivustolta löytyviin tehtävävinkkeihin. Heti kun näin vinkin "Did you try as logged-in user, too?" rupesin soimaan hälytyskellot, tuli jopa ehkä vähän tyhmä olo, miksen tajunnut asiaa. Suuntasin tekemään tunnukset etusivun kautta löytyvästä register napista. 

![K14](14.png)
![K15](15.png)

Hackerman tunnukset tulille ja kirjautuminen sisään onnistuneesti. Tämän jälkeen uudestaan ffufilla etsitylle admin-console sivustolle http://127.0.0.1:8000/admin-console/

![K16](16.png)

Ja näin homma pakettiin. Ihan hölmistynyt olo, miten simppeli ratkaisu lopulta oli. 

## e) Korjaa 020-your-eyes-only haavoittuvuus
Löysin kyllä Teron vinkkien kautta views.py tiedoston ja avasin sen microlla, mutta hetken aikaan pähkäiltyä ei ollut mitään hajua mistä edes aloittaa korjaamista.

## g) Ratkaise Portswigger Academyn "Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data".
Tämän ratkaisinkin jo kurssin tunnin aikana ja tämän tehtävän oppeja lähdinkin soveltamaan Teron tuottamaan 010 - Staff Only tehtävään. Portswiggerin tehtävässä valittiin category Gifts ja URL perään syötettiin SQL koodia näyttämään myös piilotetut tuotteet.

      '+OR+1=1--
      
![K17](17.png)

## h) Ratkaise Portswigger Academyn "Lab: SQL injection vulnerability allowing login bypass"
Tämänkin kerkesin ratkaista jo kurssin tunnin aikana, mutta ratkoin uudestaan vielä. Yritinkin 020 - Your Eyes Only tehtävässä ratkoa kirjautumista admin-console sivustolle juuri tätä tehtävää vastaavalla suorituksella, missä admin käyttäjälle yritetään hakea kannasta sopivaa salasanaa.

      Username: administrator'--
      Passowrd: ' '

![K18](18.png)
![K19](19.png)

## Lähteet
OWASP Top 10:2021. A01:2021 – Broken Access Control. Luettavissa: https://owasp.org/Top10/A01_2021-Broken_Access_Control/ Luettu: 31.10.2024

Karvinen T. Raportin kirjoittaminen 2016. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ Luettu: 31.10.2024

Nurminen Kasper GitHub. Linux Kurssin viikkotehtävä. Luettavissa: https://github.com/nurminenkasper/Linux-palvelimet/blob/main/h1-oma-linux.md Luettu: 31.10.2024

Karvinen T. Find Hidden Web Directories - Fuzz URLs with ffuf. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/ Luettu: 31.10.2024

Portswigger Access control vulnerabilities and privilege escalation. Luettavissa: https://portswigger.net/web-security/access-control Luettu: 31.10.2024

Karvinen T. Hack'n Fix. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/hack-n-fix/ Luettu 1.11.2024

Karvinen T. Find Hidden Web Directories - Fuzz URLs with ffuf. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/ Luettu 1.11.2024

Portswigger, Examples of SQL injection. Luettavissa: https://portswigger.net/web-security/sql-injection#sql-injection-examples Luettu 1.11.2024

Portswigger, Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. Luettavissa: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data Luettu 1.11.2024 

Portswigger, Lab: SQL injection vulnerability allowing login bypass. Luettavissa: https://portswigger.net/web-security/sql-injection/lab-login-bypass Luettu 1.11.2024 
