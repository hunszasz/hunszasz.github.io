---
layout: post
title: Google Analytics kezdeti lépések
date: 2023-02-20 08:53 +0100
categories: [Működés, Hasznos]
---
# Cél
Ismerkedés elősegítése a Google Analytics eszközökkel.

# Fiók
Ha nincs Google fiókod, készíts egyet. Ezt követően a Google fiókoddal már be tudsz lépni a [Google Analytics weboldalára](https://analytics.google.com/analytics/web/).
Illetve telefonra az app boltokban elérhető az alkalmazás is:
* [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.giant&hl=en_US&gl=US&pli=1)
* [IOS](https://apps.apple.com/us/app/google-analytics/id881599038)

# Google Analytics Tracking Id vs Google Tag
Utánajárás után lett világos, hogy a két azonosító különböző, egyes eszközök csak a ```Google Analytics Tracking Id``` UA-######## formátumot használja, mások pedig mindkettőt illetve csak a ```Google Tag``` G-######## formátumot.
Mivel az "új világ - Google Analytics 4" már a Google Tag-et vezette be, így annak használatát preferáltam.

# Tulajdon felvétele
* Menüsor alján látható adminisztrálás \
  ![1_admin](/assets/1_admin.webp)
* új tulajdon felvétele \
  ![plusz_tulajdon](/assets/plusz_tulajdon.webp)
* tulajdon adatainak megadása
  * név
  * időzóna
  * pénznem
    ![Tulajdon_beallitas](/assets/Tulajdon_beallitas.webp)
* kísérleti előkonfiguráció
  * cég típus
  * cég méret
  * mire használnánk a Google Analytics eszközt \
    ![kiserleti_szemelyreszabas](/assets/kiserleti_szemelyreszabas.webp)
* Adatstreamek-en belül új adatfolyam rögzítése - a bloghoz én platformnak a web-et választottam \
   ![adatstream](/assets/adatstream.webp)
* Mérési azonosító alatt megtalálható a Google Tag \
  ![meresi_azonosito](/assets/meresi_azonosito.webp)

# Ismerkedés
Az analizált oldal beállítása után néhány napot várva vállik láthatóva a mérések.


