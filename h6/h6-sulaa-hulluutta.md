# h6 - Sulaa hulluutta

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home 24H2

## a) Tutki tiedostoa h1.jpg jo opituilla työkaluilla.
Tehtävän aloittamista varten oli luonnollsiesti ladattava h1.jpg, mitä tutkia. Käytin Teron sivuille wget komentoa hakeakseni sen.

![K1](1.png)

Päätin lähteä tutkimaan **file** komennolla, mutta mitään erityistä silmään pistävää en tästä löytänyt.

![K2](2.png)

Tarkastellaan hieman esimerkiksi tiedoston kokoa **ls -l** komennolla

![K3](3.png)

Seuraavaksi ajattelin, että kokeilen **Strings** komentoa kuvalle.

![K4](4.png)

Syöte oli niin pitkä, että en sitä kokonaan edes tähän viitsinyt sisällyttää. Mitään relevanttia en löytänyt, epämääräistä tekstisarjaa. Siirryinkin näin ollen seuraavan tehtävän pariin, koska en enää keksinyt mitään lisättävää.

## b) Tutki tiedostoa h1.jpg binwalk:lla
Piti hieman perehtyä, miten binwalk asennetaa. Näytti kuitenkin löytyvän suoraan Debianin repositorystä.

![K5](5.png)

**Binwalk** komennolla pääsi tarkastelemaan, mitä eri lisäkomennot perään tekevät.

![K6](6.png)

Ajoin kuitenkin kuvan suoraan pelkällä **Binwalk h1.jpg** komennolla. 

![K7](7.png)

Tuosta pisti heti silmään se, miten h1.jpg on ilmeisesti pakattuja tietoja? Binwalkin ohjeista löyty erilliset ohjeet Extraction Optionseille.

![K8](8.png)

Ajetaan näin ollen **binwalk -e** komennolla, ja katsotaan mitä saadaan aikaan.

![K9](9.png)
![K10](10.png)

Uusi kansio rakennettuna, perehdytäänpä tarkemmin sisältöön!

![K11](11.png)

Yllätti, miten monta eri kansiota ja tiedostoa ohjelma purki kuvasta! 494F5.zip tiedosto pisti ainakin heti silmään, mutta ajattelin tutkia hieman muita kansioita ensin.

![K12](12.png)

Näissä kansioissa ei nyt mitään erityisen silmään pistävää, joten tutkitaan vielä hieman tarkemmin .zip tiedostoa mikä pisti alunperin silmään.

![K13](13.png)

Tutkin alkuun kansioiden ja tiedostojen kokoja. Mitäpä jos yrittäisi purkaa unzipillä 494F5.zip tiedoston.

![K14](14.png)

Mielenkiintoista, ei onnistu lainkaan. Mitä **file** komento näyttää.

![K15](15.png)

Microsoft Word 2007+? Okei. Ei nyt älyttömästi apua, siirryin tutkimaan hieman muita kansioita ja niiden tiedostoja.

![K16](16.png)

Näistä .xml tiedostoista ei mitään erityistä pistä omaan silmään. Ajattelin kuitenkin vielä kokeilla cat komentoa ja lukea mitä ne sisältää.

![K17](17.png)
![K18](18.png)

Ehkä päälimmäisenä pisti silmään, että sieltä löytyi esimerkiksi opettajan "Lari Iso-Anttila" nimi, mutta muuten ei mitään silmään pistävää. Entäpä **Strings** komento?

![K19](19.png)

Ei mitään uutta. Tutkitaan hieman muita kansioita ja tiedostoja vielä.

![K20](20.png)

Ei mitään uutta silmään pistävää. Perehdyin vielä binwalk komentoihin ja yritin ajaa esimerkisi **binwalk -A** mikä tutkii tiedostosta "Scan target file(s) for common executable opcode signatures"

![K21](21.png)

Ei tuloksia. Entä **binwalk -M**, millä tutkitaan tiedoston purettuja tiedostoja rekursiivisesti.

![K22](22.png)

Ei vieläkään mitään kovin ihmeellistä omaan silmään. Viimeisimpänä tarkastelin vielä tunnillakin tutkittua Entropyä **binwalk -E** komennolla

![K23](23.png)

Tapahtuuhan siinä asioita, mutta ei tämä avaa itselleni tapahtumia yhtään tarkemmin. Mitään muuta tutkittavaa en Biwalkilla enää keksinyt, joten kohti seuraavaa tehtävää.

## c) FOSS (Free Android OpenSource)
Valitsin FOSS GitHubista Bitwarden sovelluksen, koska sitä tulee itsekkin käytettyä aktiivisesti niin ajattelin sen olevan mielenkiintoinen kohde tutkia. Haetaan sen .apk wget komennolla GitHubista.

![K24](24.png)

### ZIP
Alkuun purin ladatun .apk sen unzipillä.

![K25](25.png)

Perehdytäänpä tarkemmin, mitä sieltä löytyy.

![K26](26.png)

Melkoinen määrä tiedostoja ja kansioita.

![K27](27.png)

Perehdyin hieman kansioiden sisältöihin, mutta en ollut varma mitä muuta ZIP kohdassa olisi pitänyt suorittaa, joten siirryin seuraaviin ohjelmiin.

### JADX
Latasin JADX ohjelman wgetillä GitHubista, sieltä löytyi myös hyvät ohjeet miten ohjelma suoritetaan.

![K28](28.png)

Puretaan ladattu paketti.

![K29](29.png)

Ohjeistuksessa oli vaihtoehto ajaa joko terminalissa tai gui versiona, ajattelin tällä kertaa mennä gui versiolla joten avataan se.

![K30](30.png)
![K31](31.png)

Tadaa, ohjelma käynnistyy! Syötetään sille ohjeen mukaan Open File kohdasta ladattu .apk

![K32](32.png)
![K33](33.png)

.apk alta löytyy muutamia kansiorakenteita ja polkuja, tutkitaan sisältöä ohjelmalla hieman tarkemmin.

![K34](34.png)

Resources alta löytyy ainakin vaikka mitä tutkittavaa. Availin hieman eri kohtia ja sieltä löytyi tuloksia binääristä itse koodiin.

![K35](35.png)
![K36](36.png)

Siinäpä se, siirrytään seuraavaan ohjelmaan.

### Bytecode Viewer
Bytecode Viewerin hain vastaavasti wget komennolla GitHubista.

![K37](37.png)

GitHub ohjeistukseta löytyi ohje, miten ladattu ohjelma suoritetaan. Sitä ei tarvinnut purkaa ollenkaan, vaan .jar tiedosto ajetaan suoraan Javalla **Java -jar** komennoola

![K38](38.png)
![K39](39.png)

Toimiihan se! Tiputetaan kansiosta sille APK, kuten ohjelma pyytää.

![K40](40.png)

Tutkitaan, miltä tällä ohjelmalla eri sisältö näyttää.

![K41](41.png)
![K42](42.png)

Melko samalta se näyttää. Itse ehkä pidin JADX mielyttävämpänä käyttää. Yritin skaalata Bytecode Vieweriä isommaksi, mutta se tuntui jotenkin tunkkaisen pieneltä ja ahtaalta.

## Lähteet

Karvinen T. h6 Sulaa hulluutta. Tero Karvisen Verkkosivut. Luettavissa: https://terokarvinen.com/application-hacking/#h6-sulaa-hulluutta Luettu: 29.11.2024

Kali.org Tool Documentation. Binwalk Usage Example. Luettavissa: https://www.kali.org/tools/binwalk/ Luettu: 29.11.2024

Android FOSS GitHub. Luettavissa: https://github.com/offa/android-foss Luettu: 29.11.2024

Bitwarden GitHub. Luettavissa: https://github.com/bitwarden/android Luettu: 29.11.2024

JADX GitHub. Luettavissa: https://github.com/skylot/jadx Luettu: 29.11.2024

Bytecode Viewer. Luettavissa: https://github.com/Konloch/bytecode-viewer/ Luettu: 29.11.2024
