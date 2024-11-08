# h3 - No strings attached

## Rauta & HostOS

- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Sapphire Radeon RX 7900 XT NITRO+ Vapor-X
- Windows 11 Home

## a) Strings & passtr

Lähdin tarkastelemaan tehtävää ihan puhtaasti kansiosta löytyvästä README.md tiedostosta. Sieltä löytyikin muutamat vinkit tarpeellisiin asioihin. 

        cat README.md

[1](1.png)

Testailin tietenkin alkuun mitä kyseinen passtr tekee.

        ./passtr

[2](2.png)

Okei, mikä salasana on? Tiedossa oli, että salasanaa piti ruveta selvittämään Strings avulla, oli itselle ensin selvennettävä miten ja miksi sitä tarkalleen käytetään. Käytännössä Strings komennolla haetaan binääristä luettavissa olevat tekstit. Lähdin siis suorittamaan strings komentoa seuraavaksi kansiosta löytyvään passtr.

        strings passtr

[3](3.png)

Sieltähän löytyi vaikka mitä tietoa, mutta selvästi myös salasana sala-hakkeri-321. Testataan vielä, että se toimii suorittaessa.

        ./passtr

[4](4.png)

Yes, That's the password. Toimiihan se tosissaan.

Lippu: FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}

## b) passtr.c -ohjelmasta uusi versio

