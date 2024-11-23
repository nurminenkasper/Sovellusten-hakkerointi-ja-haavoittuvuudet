# h5 - Se elää!

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home

## a) Lab1
Ennen tehtävän aloittamista, asennetaan GNU Debugger.

![K1](1.png)

Testailin vielä, että mitä itse ohjelma suorittaessa tekee ennen GNU Debuggerilla tarkastelua.

![K2](2.png)

Tulostaa erikoisen kirjainsarjan ja Segmentation Faultin toiselle riville. Tarkastellaan seuraavaksi GNU Debuggerissa. **List** komennolla lähdekoodia tarkemmin näkyviin.

![K3](3.png)

Ajetaan ohjelmaa **Run** komennolla.

![K4](4.png)

Rivillä 7 tulee käsitykseni mukaan Segmentation Fault. Tarkastellaan sitä hieman tarkemmin asettamalla siihen **Breakpoint**.

![K5](5.png)

Uudestaan ohjelmaa ajaessa, huomaan sen miten **"Hello, World."** tekstistä poistuu hiljalleen kirjaimia ja lopulta antaa tulosteen **"Khoor/#zruog1"**. Tarkemmin tarkastellessa molemmissa sama määrä merkkejä, eli ohjelma siis kääntää kirjain kerrallaan alkuperäisen tekstin.

![K6](6.png)

Huomasin myös, miten Segmentation Fault tulee heti kun viimeinen kirjain on käännetty ja tuloste annettu käännökselle.

![K7](7.png)

Tarkastellaanpas korjausta avaamalla alkuperäinen lähdekoodi Microssa ja korjataan viallinen koodi.

Ohjelman tarkoitushan on ilmeisesti siis kääntää teksti "Hello, World." 

**void print_scrambled(char *message)*** Funktio osoitin, eli merkkijono

**register int i = 3;** - Tämän perusteella käytetään ilemisesti ASCII-taulukkoa

**printf("%c", (*message)+i);*** - Tässä haetaan kirjain ja lisätään siihen i taulukossa 3 merkkiä eteepäin vastaava merkki

**while (*++message);*** Siiryminen seuraavaan merkkiin

**printf("\n");** Lopputuloksen tulostus

**int main()
{
  char * bad_message = NULL;
  char * good_message = "Hello, world.";** Muuttujat bad_message & good_message.

**print_scrambled(good_message);** Funktio vastaanottaa osoittimen good_message ja kääntää sen ASCII-taulukon mukaan.

**print_scrambled(bad_message);** Funktiokutsu, mikä on virheellinen, koska NULL viittaa asiaan mikä ei ole kelvollinen ja siitä tulee Segmentation Fault.

No, miten ongelma korjataan? Lisätään koodiin tarkistus, mikä kertoo ohjelmalle olevan tekemättä mitään jos osoitin on NULL.

    if (message == NULL) {
        return;

![K8](8.png)

Testataan vielä, onko ohjelmasta kadonnut segmentointi virhe.

![K9](9.png)

## b) Lab2


## b) Lab3

## Lähteet

Karvinen T. h5 Se elää! Tero Karvisen Verkkosivut. Luettavissa: https://terokarvinen.com/application-hacking/#h5-se-elaa Luettu 23.11.2024

GeeksForGeeks. Segmentation Fault in C++. Luettavissa: https://www.geeksforgeeks.org/segmentation-fault-c-cpp/ Luettu 23.11.2024

Sovellusten hakkerointi ja haavoittuvuudet - ICI012AS3A-3001 Moodle. 01-GDB.pdf. Luettu 23.11.2024

Darkdust. GBD cheatsheet. Luettavissa: https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf Luettu: 23.11.2024

RedHat. The GDB developer's GNU Debugger tutorial, Part 1: Getting started with the debugger. Luettavissa: https://developers.redhat.com/articles/the-gdb-developers-gnu-debugger-tutorial-part-1-getting-started-with-the-debugger# Luettu: 23.11.2024

Tehtävä b) sisällössä yritetty hyödyntää ChatGPT-4 -kielimallia.

