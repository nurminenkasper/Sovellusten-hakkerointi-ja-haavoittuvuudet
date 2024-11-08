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

![K1](1.png)

Testailin tietenkin alkuun mitä kyseinen passtr tekee.

        ./passtr

![K2](2.png)

Okei, mikä salasana on? Tiedossa oli, että salasanaa piti ruveta selvittämään Strings avulla, oli itselle ensin selvennettävä miten ja miksi sitä tarkalleen käytetään. Käytännössä Strings komennolla haetaan binääristä luettavissa olevat tekstit. Lähdin siis suorittamaan strings komentoa seuraavaksi kansiosta löytyvään passtr.

        strings passtr

![K3](3.png)

Sieltähän löytyi vaikka mitä tietoa, mutta selvästi myös salasana sala-hakkeri-321. Testataan vielä, että se toimii suorittaessa.

        ./passtr

![K4](4.png)

Yes, That's the password. Toimiihan se tosissaan.

Lippu: FLAG{Tero-d75ee66af0a68663f15539ec0f46e3b1}

## b) passtr.c -ohjelmasta uusi versio

DISCLAIMER! Tehtävän ratkaisussa on hyödynnetty lyödettyjen lähteiden lisäksi ChatGPT-4 tekoäly kielimallia.



## Lähteet

Karvinen T. Application Hacking. h3 No strings attached. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/application-hacking/ Luettu 8.11.2024

How To Geek. How to Use the strings Command. Luettavissa: https://www.howtogeek.com/427805/how-to-use-the-strings-command-on-linux/ Luettu: 8.11.2024

Reddit. How to hide characters when doing strings on my compiled C program?. Luettavissa: https://www.reddit.com/r/C_Programming/comments/gri58y/how_to_hide_characters_when_doing_strings_on_my/ Luettu 8.11.2024

Yurisk.info. Binary obfuscation - String obfuscating in C. Luettavissa: https://yurisk.info/2017/06/25/binary-obfuscation-string-obfuscating-in-C/ Luettu 8.11.2024

Tehtävä b) sisällössä ja ratkaisuissa hyödynnetty ChatGPT-4 -kielimallia.

Linux.fi. GCC. Luettavissa: https://www.linux.fi/wiki/GCC Luettu: 8.11.2024


