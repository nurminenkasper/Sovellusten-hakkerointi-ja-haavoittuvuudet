# h4 - Kääntöpaikka

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home

## x) Lue/katso/kuuntele ja tiivistä


## a) Asenna Ghidra.
Ghidran asentaminen Debian 12 käyntiin. Asennellaan ensin sitä varten OpenJDK (Open Java Development Kit).

![K1](1.png)

Haetaan wget komennolla GitHubista Ghidra versio 11.1.2. Tässä tapauksessa tämä versio, koska tuoreempi versio tarvitsee Java 21, mitä en ainakaan suoralla apt-get install komennolla vielä repositorystä löytänyt. Puretaan lisäksi paketti.

![K2](2.png)
![K3](3.png)

Suoritetaan Ghidra puretusta kansiosta ja varmistetaan, että se aukeaa oletetulla tavalla.

![K4](4.png)

Toimiihan se!

## b) rever-C
Packd tehtävää varten Ghidra käyntiin ja luodaan sille uusi projekti.

![K5](5.png)

Syötetään packd ohjelma projektiin.

![K6](6.png)

Analysoidaanko? No tietekin. Täysin default valinnoilla mennään.

![K7](7.png)

Itse lähdin etsimään main ohjelmaa aluksi suoraan Listing kohdasta, mutta nopeasti tajusin sen olevan turhan loputon heinäsuo. Selailin hieman Program Trees kohtaa, sieltäkin oli hieman hankala löytää mitään järkevää. Alempaa Symbol Tree kohdasta kuitenkin löytyi suoraan main kohta, mikä oli ilmeisestikkin tarkoitus löytää.

![K8](8.png)

Lähdin Decompile ikkunasta miettimään ratkaisuja vajanainseen C-koodin tynkään.

Ylimmän main(void) kohdan jätin samaksi, en tiedä mitä muutakaan siihen olisi pitänyt syöttää?

**int iVar1; -> int Password**

int, eli integer on ohjelman muuttuja. Ohjemassa viitataan siihen useampaan kertaan.

**char local_28 -> char inputPassword**

char, eli käyttäjän syöte. Siihen viitataan muutamssa eri kohdassa missä sitä verrataan esimerkiksi onko salasana == 0 ja sen perusteella annetaan oikea syöte.

Ohjelmasta löytyy lisäksi tietenkin

**puts**, joka ohjeistaa käyttäjää syöttämään salasanan ja myöhemmin tulostaa oikean tuloksen.

**scanf**, mikä lukee käyttäjän syötteen ja aloittaa vertausprosessin.

## c) Jos väärinpäin


## d) Nora CrackMe


## e) Nora crackme01


## e) Nora crackme01e


## f) Nora crackme02


## g)


## h)


## i)
