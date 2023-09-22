---
layout: post
title: A Java21 elérkezett! Így kezd használni 4 lépésben.
date: 2023-09-22 21:06 +0200
categories: [Hasznos, Test, Trend]
seo.type: BlogPosting
image: /assets/ideogram_hello_java21.webp
description: megtudhatod, hogyan tudsz több java verziót használni
---

# Cél
Ha elolvasod a bejegyzést,
* megtudhatod, hogyan tudsz több java verziót használni
* ezek között hogyan lehet magabiztosan váltani
* milyen egyszerűen telepíthetsz java21-et Fedora OS-re
* tanulhatsz a java21 újdonságairól

# Itt a Java21 
![java21](/assets/ideogram_hello_java21.webp)

Az Oracle bejelentette a java21 általános elérhetőségét 2023.09.19-én!
Számos fejlesztés és javítás kapott helyett a java21-ben. Teljes listáért itt a [release notes](https://www.oracle.com/java/technologies/javase/21-relnote-issues.html)
Ez a verzió a korábban bejelentett 6 hónapos kiadási ütem 12-ik időben release-ált tagja. 
A fejlesztők számára ebből az következik, hogy több java verzió is lesz a gépükön.

# Több java vesződés nélkül
A JDK használata első látásra a használt IDE szempontjából lehet fontos. Azonban a build-ek során szükség lehet egy segítő kézre, ami összeszedi a függőségeket, futtatja a build plan-t és így tovább.
Spring esetében - és amúgy is érdemes - a Maven veszi át ezeket. Ahhoz, hogy az integrált IDE és a Maven is ugyanazt a java verziót használja
szükséges néhány beállítás.

A következő megoldás az alábbi szempontrendszert teljesíti:
- kapcsolható java verzió
- rendszerszintű hatás
- újraindítás nélkül
- minimalizált lépés

## 1. lépés telepítsd fel a legújabb java JDK-t
Ez a parancs a legújabb fejlesztői kit-et fogja feltelepíteni a gépre, ez jelenleg a 21-es 2024 márciusáig.
```shell
sudo dnf install java-latest-openjdk-devel
```

## 2. alternatives használata
Az **alternatives** egy parancssori eszköz Linux rendszereken, amely segít kezelni és váltogatni az alkalmazásokhoz
kapcsolódó szimbolikus linkeket. Ez akkor lehet hasznos, ha több verziójú vagy alternatív alkalmazásokat szeretnél 
használni egy adott parancsra a rendszereden. Például, ha több java JDK-t vagy JRE-t szeretnél használni, 
az "alternatives" segítségével könnyen váltogathatsz közöttük.

A különböző verziók feltöltésére használhatsz segítséget, töltsd le külön a [java_alternatives.sh](https://github.com/hunszasz/java21/blob/main/src/sh/java_alternatives.sh)
vagy a clone-ozd le a teljes git repo-t.

**A fájlban szükséges a JAVA_HOMES nevű asszociatív tömböt módosítani a telepített JDK-k függvényében.**

```shell
git clone https://github.com/hunszasz/java21.git
cd java21/src/sh
chmod +x java_alternatives.sh
sudo ./java_alternatives.sh
```

A script telepíti az a java, javac és jar parancsokat a megadott JDK verziókhoz.

## 3. Válts JDK verziót
A váltáshoz használható [switch_java.sh](https://github.com/hunszasz/java21/blob/main/src/sh/switch_java.sh) első körben beállítja a kiválasztott alternatives bejegyzést, amelyet argumentumként vár a script.

Létrehozza az /etc/profile.d/java_home.sh fájlt a JAVA_HOME jelenleg kiválasztott JDK verziójához, amellyel így újraindítás után is megőrizzük a választásunkat. 

Ezt követően exportálja a helyes JAVA_HOME attribútumot, illetve ellenőrzés miatt kiírja a java, javac és mvn verzióit a JAVA_HOME értéke mellett.

```shell
chmod +x switch_java.sh
sudo ./switch_java.sh 1
source /etc/profile.d/java_home.sh
```

## 4. mehet a build vagy futtatás a java21 kódodra natívan, IDE-ből vagy maven-nel

### Natív
Ellenőrizhetjük a verziót az alábbi paranccsal.
```shell
java -version
```

### Maven
Ellenőrizhetjük a verziót illetve futtathatjuk a teszteket alábbi paranccsal.
```shell
mvn -v
mvn clean test
```

### IDEA
Idea-ban első lépésként hozzá kell adni az új JDK-t.
Ehhez válasszuk a "file/project structure" menüt.
![project_setting](/assets/project_structure.webp)

Az IDEA jó esetben automatikusan detektálja az új JDK-t.
![auto_detect](/assets/auto_detect.webp)

Az IDEA-ban delegáljunk a build/run műveleteket a maven-re - amely a hot deploy esetében is működni fog. 
![delegate](/assets/delegate_runner.webp)

# Hasznos linkek
- https://dev.to/khmarbaise/using-jdk21-preview-features-andor-incubator-classes-3dc1
- https://www.oracle.com/java/technologies/javase/21-relnote-issues.html
- https://github.com/hunszasz/java21