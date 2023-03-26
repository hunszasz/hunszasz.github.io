---
layout: post
title: Hogy telepíts Fedora 37 linuxot?
date: 2023-03-23 07:53 +0100
categories: [Fejlesztői környezet, Linux]
seo.type: BlogPosting
image: /assets/linux.webp
description: A poszt célja, hogy ismertesse a Fedora 37 Cinnamon telepítését valamint egy felülnézeti rálátást biztosítson a Linux disztribúciókra. 
---

# Cél

A poszt célja, hogy ismertesse a Fedora 37 Cinnamon telepítését
valamint egy felülnézeti rálátást biztosítson a Linux disztribúciókra.

A blogban szereplő posztok alapjául ez a rendszer fog szolgálni. 

# Linux előnyei

* Ingyenes: Az egyik legnagyobb előnye a Linuxnak az, hogy ingyenes. Bármilyen változatát letöltheted, telepítheted és használhatod anélkül, hogy fizetned kellene érte.
* Testreszabhatóság: A Linux rendszerek rendkívül testreszabhatóak, lehetővé téve a felhasználók számára, hogy az igényeiknek megfelelően alakítsák ki a rendszert. Választhatsz különböző asztali környezeteket, megváltoztathatod az ablakkezelőt, és testreszabhatod a különböző alkalmazások megjelenését is.
* Biztonság: A Linuxra kevés vírus és kártevő létezik, így kevesebb az esélye annak, hogy a számítógépedet meg fertőzzék.
* Stabilitás: A Linux stabilitása és megbízhatósága a csomagkezelők használatában is rejlik.
* Szabad szoftver: A Linux sok szabad szoftvert tartalmaz, például a LibreOffice-t, amely egy ingyenes irodai csomag, és a GIMP-et, amely egy ingyenes képszerkesztő program.

# Disztribúciók

![Linux](/assets/linux.webp)

A következő áttekintés nem tekinthető teljes körűnek, nem célom átfogó képet adni. 
A választásom hátterét írom le, amely jó kiindulási alap lehet Linux-al ismerkedőknek.

A Linux egy nyílt forráskódú operációs rendszer, amely lehetővé teszi a felhasználók 
számára, hogy saját igényeikhez igazítsák a rendszert. Számos különböző Linux-disztribúció áll rendelkezésre, 
amelyek számos eltérést és hasonlóságot is mutatnak.

Az Ubuntu egy népszerű Debian-disztribúció, amelynek célja, hogy felhasználóbarát 
és könnyen használható legyen a kezdő felhasználók számára is. 
Az Ubuntu rengeteg előre telepített szoftvert tartalmaz, így azoknak, 
akik nem szeretnének sokat konfigurálni az operációs rendszerüket, ez lehet az 
ideális választás. Számos alkalmazás elérhető Ubuntu alatt 
a hosszú LTS (Long-Term Support) időszakai miatt. Egy nagy hátránya van, nem lehet telepítés nélkül 
a következő verzióra portolni.

Az Arch Linux egy olyan disztribúció, amelynek középpontjában a minimalizmus áll, 
és a felhasználóknak lehetőségük van a rendszer részletes testreszabására. 
Az Arch Linux fejlesztése úgynevezett "rolling release" modellt használ, ami azt 
jelenti, hogy a rendszer folyamatosan frissül, így mindig a legújabb verziókat 
használhatjuk.

A Fedora Linux egy másik nagyon népszerű RedHat-disztribúció, amelynek fejlesztése 
során központi helyen szerepel a stabilitás és biztonság. 
A Fedora Linux nem teljesen "rolling release", mint az Arch Linux, azonban megoldást ad 
az upgrade, így mindig a legfrissebb technológiákat használhatjuk. Azt gondolom, hogy a 
Fedora Linux nagyon jó választás azon programozók számára, akik szeretnének 
stabil környezetet a munkájuk során.

# Fedora Spin-ek

A Fedora spin-ek olyan előre összeállított változatai a Fedora Linux disztribúciónak, 
amelyek konkrét célcsoportokat céloznak meg. Ezek a spin-ek lehetővé teszik a 
felhasználók számára, hogy a választott rendszer már előre konfigurált legyen
és olyan előre telepített szoftverekkel rendelkeznek, amelyek kielégítik az adott 
felhasználók igényeit. 

Például, a Fedora Robotics spin azoknak a felhasználóknak készült, akik robotikai 
projekteken dolgoznak, míg a Fedora Workstation spin a fejlesztők és kreatív 
szakemberek számára készült. 

# Asztali környezetek

Számos asztali környezet választható a Fedora Linux disztribúció alatt. Mindnek megvan
a saját jellege és előnye, érdemes utánaolvasás után, de még telepítés előtt a live-os
módban kipróbálni telepítés előtt. Így biztosan olyat választunk, amely a  
megfelelő Fedora asztali környezetet biztosítja számunkra. Néhányat ízelítőnek leírok.

A GNOME - a Workstation spin alapból ezt használja - egy modern és elegáns asztali 
környezet, amelynek középpontjában a felhasználói élmény áll. 
A GNOME-ban az alkalmazások a képernyő bal oldalán találhatók, amelyeket az 
"Activities" (tevékenységek) nézetből érhetünk el. 
A GNOME számos előre telepített alkalmazással rendelkezik, többek között az Evince PDF-olvasót, 
a GNOME Terminált, a Nautilust fájlkezelőt.

Az Xfce egy könnyű és gyors asztali környezet, amelynek célja az egyszerűség és a 
hatékonyság. Az Xfce alkalmazásai közül meg lehet említeni, a Thunar fájlkezelőt, 
a Mousepad szövegszerkesztőt és a Terminál emulátort.

A Cinnamon egy modern és könnyen használható asztali környezet, amely hasonlít a 
Windowsra, de sokkal testre szabhatóbb. Az előzőekben leírtaktól eltérően ezt a 
környezetet választottam saját intuíciók után.

# Telepítés

[Fedora 37](https://getfedora.org/hu/workstation/download/) leírás alapján a minimum rendszerkövetelmény
20GB tárhely és legalább 2GB memória, az ajánlott érték pedig ezek duplája.

Első lépésként egy telepítőt kell készíteni, ezt számos módon el lehet végezni. Egyik
legegyszerűbb módja a **Fedora Media Writer** használata. 
* [Windows](https://getfedora.org/fmw/FedoraMediaWriter-win32-latest.exe)
* [Mac](https://getfedora.org/fmw/FedoraMediaWriter-osx-latest.dmg)
* Linux: liveusb-creator 

Következő lépésben már lehet boot-olni az elkészült telepítőt tartalmazó pendrive-ról.
Ebben segítség lehet ez a [lista](https://www.disk-image.com/faq-bootmenu.htm).

## Lépések

* Install to Hard Drive \
  A telepítést az alábbi ikonra kattintva lehet elkezdeni. \
  ![Install to Hard Drive Icon](/assets/fedora_install/install_to_hard_drive.webp)
* Nyelv kiválasztása \
  ![Telepítő nyelvének kiválasztása](/assets/fedora_install/nyelv_valaszto.webp) \
  A telepítő lépéseit az alábbi képen láthatjuk, ahol piros felirat jelzi a teendőinket. \
  ![Telepítő lépései](/assets/fedora_install/lepesek.webp)
* Telepítési cél
  * hova települjön \
    A partíciókat automatikusan és kézzel is létre lehet hozni.
    A meglévő partíciók törlését a **szabadítson fel helyet** jelölővel érhetjük el.
    Titkosíthatjuk rendszerünket, boot során a rendszer kérni fogja az itt megadott jelszót. \
    ![Telepítés helye](/assets/fedora_install/telepitesi_cel.webp)
  * titkosítás (opcionális) \
    Adjuk meg kétszer a kívánt jelszót. \
    ![Titkosítás jelszava](/assets/fedora_install/titkositas.webp)
  * hely felszabadítás (opcionális) \
    Amennyiben a teljes rendelkezésre álló terület felszabadítható, - nincs másik op rendszer a gépen, egyéb megtartandó partíció - úgy kattintsunk az **Összes törlése** gombra. 
    ![Partíciók törlése](/assets/fedora_install/torles.webp)
* Rendszergazdai fiók \
  ![Rendszergazdai fiók](/assets/fedora_install/rendszergazda.webp)
* Felhasználói fiók \
  ![Felhasználói fiók](/assets/fedora_install/felhasznalo.webp)
* Konfiguráció befejezése \
  Az alábbi gombbal tudjuk befejezni a telepítést. A következő boot-nál már az új rendszer fog indulni.
  ![Konfiguráció befejezése](/assets/fedora_install/kesz.webp)
