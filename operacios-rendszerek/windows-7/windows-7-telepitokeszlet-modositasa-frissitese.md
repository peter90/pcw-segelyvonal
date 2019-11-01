# Windows 7 telepítőkészlet módosítása, frissítése

## Telepítőkészletek

Nem mindenki rendelkezik (tisztességes) telepítőkészlettel annak ellenére, hogy megvette a Windows 7-t. Semmi gond, töltsön le egy, a licencének megfelelőt bárhonnan, ami fontos, hogy az ellenőrzőkódja egyezzen a http://msdn.microsoft.com/hu-hu/subscriptions/downloads/default(en-us).aspx lévőkkel (Operating Systems --> Windows 7). A letöltött lemezkép ellenőrzőkódjait a http://www.marxio-tools.net/en/download.php-rel állíthatjuk elő. Ha nem fognak egyezni a kódok, akkor a lehető leghamarabb szabaduljunk meg a lemezképtől!

## Online telepítőkészletmódosítás

### Amire szükségünk lesz:
* Olvasás képessége
* Windows 7 telepítőlemez (DVD, vagy képfájl)
* Az, amivel frissíteni, bővíteni szeretnénk (Service Pack, .NET, friss DirectX, stb.)
* Virtuális vagy fizikai gép, amit használatba vehetünk
* http://dl.dropbox.com/u/15497521/pcw/windows/imagex_oscdimg.zip
* Ha virtuális gépen dolgozunk, akkor egy olyan programra, ami képes felcsatolni a virtuális meghajtónkat Windows alá (WinMount [shareware])

1. Kezdjük el telepíteni az operációs rendszert ugyanúgy, ahogyan szoktuk.
2. A második újraindítást követően, mikor adatokat kér rólunk a rendszer, ne kezdjük el konfigurálni, hanem nyomjunk **Ctrl + Shift + F3**-at!

Újra fog indulni a 7, betöltésnél azonnal Rendszergazdaként léptet be bennünket, méghozzá audit módban, ebben az üzemben leszünk képesek "manipulálni" a telepítőképet.
**Meg fog jelenni a Rendszer-előkészítő eszköz ablaka, ne zárjuk be, csak toljuk félre!**

3. Remélhetőleg szépen összekészítettük azokat a szoftvereket, frissítéseket, amelyeket bele szeretnénk pakolni a telepítőkészletbe. Nem tudom, ki mit fog belerakni, de itt egy útmutatás a sorrendet illetően:

    1. SP1 (Ha már benne van, nyilván nem kell, telepítés után futtatni KELL a takarítót.)
    2. Internet Explorer 11
    3. .NET-keretrendszer 4.8
    4. Más Windows kiegészítések (pl. nyelv)
    5. Egyebek (Ha fizikai gépen dolgozunk, akkor drivereket is telepíthetünk, NEM fogja befolyásolni, ha másik gépre telepítünk ezzel a készlettel.)

> Ha valami kéri, akkor nyugodtan indítsuk újra a gépet, ugyanúgy audit módban fogunk belépni, a kis ablak is megjelenik majd, szintén toljuk félre addig, amíg nincs rá szükségünk.

Ha végeztünk a frissítéssel, bővítéssel, akkor szedjük elő a Rendszer-előkészítő eszköz ablakát, majd válasszuk a képen látható lehetőségeket (kezdő élmény mutatása, leállítás):

< hiányzik >

Ha valaki fizikai gépen dolgozik, akkor választhatja az Újraindítást is a Leállítás helyett, de figyeljen rá, hogy ne ez a rendszer induljon el!
Tulajdonképpen ezzel érjük azt el, hogy bármilyen gépre fel lehessen telepíteni.

### install.wim legyártása

1. Első lépésben csatoljuk fel a partíció (virtuális lemezkép) megfelelő részét (az a bizonyos 100 MB-os partíció ne érdekeljen bennünket), ami a 7-t tartalmazza!
     
     nálam a betűjele: **Z**

2. Start menü > Minden program > Kellékek, jobb klikk a Parancssorra, Futtatás rendszergazdaként
3. Navigáljunk el az ImageX-t tartalmazó mappánkhoz!
4. Adjuk ki a következő parancsot:

    ```imagex /compress maximum /flags "Professional" /capture z: e:\install.wim "Windows 7 Professional"```

    * **imagex:** maga a program
    * **/compress maximum:** tömörítésű szint
    * **Professional és Windows 7 Professional:** kinek milyen van
    * **/capture z:** melyik partíción van a menteni kívánt 7
    * **e:\install.wim:** cél helye és neve, TILOS átnevezni (ha jót akarunk magunknak, akkor külön merevlemezre pakoljuk)

## Offline driver-integrálás

**A Windows 7 beépítve tartalmazza a dism-et, Vista-hoz fel kell telepíteni a Windows 7 WAIK-ot!**

Egyszerűen bonyolult vagy mi. Ahhoz, hogy sikeresen tudjunk meghajtókat integrálni az kell, hogy jobban ismerjük a driverek terjesztését, mint az ábécét. Persze ezen felül az sem árt, ha tudjuk milyen driverre van szükségünk.

Vannak olyan csomagok, amelyekkel nem sok gond van. Letöltjük a ZIP-et, majd kimásoljuk belőle a win7 mappában található *.sys, *.inf és a többi fájlt, meg is van a driver.

Hát, az ATI csomagjai nem ilyenek, nézzük a Mobility csomagokat, ezekben van egy plusz kanyar. Az http://amd.com oldalán talán mindenki ki tudja választani a neki megfelelő pakkot. A mobil GPU-khoz adott fájl azonban nem tartalmaz drivert, csak egy letöltőalkalmatosságot kapunk. El kell indítanunk a letöltést, majd a letöltött állományt kell megnyitnunk (majdnem telepítenünk). Ekkor rá fog kérdezni, hova is csomagolhatja ki a fájljait, keressünk valami egyszerű mappát! Miután végzett nem szabad elindítanunk a telepítést, lépjünk ki. Elő kell vennünk egy újabb segédeszközt: 7ZIP. Azért kell, mert a kicsomagolt driver is tömörített állományokat tartalmaz, amelyeket a dism nem bír lenyelni. Ezeket a "Packages\Drivers\Display\W7_INF\B100667\" (vagy hasonló) mappában találjuk, különös ismertetőjelük a kiterjesztésükben szereplő '_' jel. Csomagoljuk ki ezeket ugyanebbe a mappába! Ezzel elő is készítettük az ATI meghajtónkat. Egyszerű volt, ugye? Helyes.

Azért a Realtek sem kutya! Hasonlóan kell megkapnunk a meghajtó fájljait, mint az ATI pakkok esetében, eltekintve a '_' fájlok kicsomagolásától. A Realtek telepítő automatikusan a %temp% mappába rakja az állományokat, innen tudjuk előbányászni őket. Nyilván nem kell az x86-os és x64-es mappa, mindig csak a megfelelőre van szükségünk. Látható, hogy ~40 inf fájl van a könyvtárban, ezek közül nekünk csak egyre van szükségünk. Honnan tudjuk, hogy melyikre?

1. Indítsunk adminisztrátori jogokkal egy parancssort!
2. ```dism /online /Get-Drivers```
3. Miután végzett, kapunk egy listát, amiben ki kell keresnünk a megfelelő inf állományt.

Miutána megtaláltuk, töröljük az összes többit a mappából, mert különben a dism lesz kedves, és mindegyiket egyesével be fogja építeni a lemezképünkbe.
Akármilyen hihetetlen, de meg is voltnánk.

A fenti dism-es módszert más helyzetben is használhatjuk. Az esetemben, a következő képpen vadásztam le jó pár driveremet, amit alapesetben a WU tölt le. Dism-mel megnéztem, milyen inf fájlnevekre (nem az oem...) kell figyelnem, majd benéztem a "c:\Windows\System32\DriverStore\FileRepository\" mappába, ahonnan ki tudtam menteni a szükséges anyagokat.

Ha megvan minden, akkor tegyük őket egy helyre, nálam a C:\drivers mappa kapta a megtiszteltetést, valahogy így néz ki a tartalma:

* 10.6\
* acrzun32z\
* broadcom\
* HDAATI\
* HDARt\
* netathr\
* o2mddisk\
* o2media\
* tcwbfadv\

A 10.6 mappa a komplett ATI drivert tartalmazza, nincs belőle törölve semmi se!

A mese után nézzük meg, hogyan kell belegyógyítani mindezt az install.wim-be.

1. Indítsunk adminisztrátori jogokkal egy parancssort!
2. ```dism /Mount-Wim /wimfile:e:\install.wim /index:1 /MountDir:h:\img```
     * dism - a program, amivel dolgozunk
     * /Mount-Wim - képfájl csatlakoztatása
     * /wimfile:e:\install.wim - képfájl megnevezése teljes elérési úttal
     * /index:1 - módosítani kívánt Windows kiadás (Lekérdezése (nyilván még a mount előtt): ```dism /Get-WimInfo /WimFile:d:\sources\install.wim```)
     * /MountDir:h:\img - melyik mappába csatlakoztassa a képfájlt (nem hozza létre magának, helyette hibát dob, így ezt először létre kell hoznunk, 10 GB hely nem árt, ha van)
3. Miután végzett:
   ```dism /image:h:\img /add-driver /driver:C:\driver /recurse```
     * dism - a program, amivel dolgozunk
     * /image:h:\img - ide csatoltuk a képet
     * /add-driver - drivert szeretnénk beletenni
     * /driver:C:\driver - hol van a driver
     * /recurse - almappákban is megkeresi az inf fájlokat

    Figyeljük a kimenetet, írni fogja, hogy sikerült-e a művelet.
4. Amennyiben sikerült: ```dism /Unmount-Wim /MountDir:h:\img /commit```
     * dism - a program, amivel dolgozunk
     * /Unmount-Wim - lecsatlakoztatjuk a lemezképet
     * /MountDir:h:\img - ide csatlakoztattuk, megmondjuk, hogy ez kell
     * /commit - mentjük a változásokat

    Ha nem sikerült: ```dism /Unmount-Wim /MountDir:h:\img /discard```
     * dism - a program, amivel dolgozunk
     * /Unmount-Wim - lecsatlakoztatjuk a lemezképet
     * /MountDir:h:\img - ide csatlakoztattuk, megmondjuk, hogy ez kell
     * /discard - visszavonjuk a változtatásokat

    Nem lenne egyébként kötelező unmountolni a képet, de nem árt, ha egyszerre egy dolgot csinálunk meg, mert így lehetőség van (biztonsági) mentésre.

Fontos megjegyezni, hogy ezzel nem fogjuk összebarmolni a rendszert. Szimplán bekerülnek az előre beállított mappákba a driverek, aztán telepítésnél, ha jelen van a megfelelő hardver, akkor használja őket a Windows, ha nincs, akkor nem. Egyébként el is lehet távolítani az ilyen módon hozzáadott drivereket.

## Offline hotfix-integrálás

Készítsük össze az integrálni kívánt frissítéseket egy mappába, nálam ez a "C:\update" lesz. Csak azok a frissítések lesznek jók, amelyek *.msu vagy *.cab fájlok. Kipróbáltam a DirectX *.cab csomagjait, de azokat nem tudta használni a dism.

1. Indítsunk adminisztrátori jogokkal egy parancssort!
2. ```dism /Mount-Wim /wimfile:e:\install.wim /index:1 /MountDir:h:\img```

    * dism - a program, amivel dolgozunk
    * /Mount-Wim - képfájl csatlakoztatása
    * /wimfile:e:\install.wim - képfájl megnevezése teljes elérési úttal
    * /index:1 - módosítani kívánt Windows kiadás (```dism /Get-WimInfo /WimFile:d:\sources\install.wim```)
    * /MountDir:h:\img - melyik mappába csatlakoztassa a képfájlt (nem hozza létre magának, helyette hibát dob, így ezt először létre kell hoznunk, 10 GB hely nem árt, ha van)
3. Hotfixek integrálása: ```dism /image:h:\img /Add-Package /PackagePath:C:\update /IgnoreCheck```
    * dism - a program, amit használunk
    * /image:h:\img - aktuális mount
    * /Add-Package - hotfixeket vagy más csomagokat szeretnénk hozzáadni
    * /PackagePath:C:\update - megmondjuk, honnan
    * /IgnoreCheck - ha nem használható 1-1 csomag, kihagyja
4. Ha készen vagyunk: ```dism /Unmount-Wim /MountDir:h:\img /commit```
   * dism - a program, amivel dolgozunk
   * /Unmount-Wim - lecsatlakoztatjuk a lemezképet
   * /MountDir:h:\img - ide csatlakoztattuk, megmondjuk, hogy ez kell
   * /commit - mentjük a változásokat

    Amennyiben meggondoltuk magunkat: ```dism /Unmount-Wim /MountDir:h:\img /discard```
    * dism - a program, amivel dolgozunk
    * /Unmount-Wim - lecsatlakoztatjuk a lemezképet
    * /MountDir:h:\img - ide csatlakoztattuk, megmondjuk, hogy ez kell
    * /discard - visszavonjuk a változtatásokat

## Offline rendszerösszetevő-letiltás

1. Indítsunk adminisztrátori jogokkal egy parancssort!
2. ```dism /Mount-Wim /wimfile:e:\install.wim /index:1 /MountDir:h:\img```

    * dism - a program, amivel dolgozunk
    * /Mount-Wim - képfájl csatlakoztatása
    * /wimfile:e:\install.wim - képfájl megnevezése teljes elérési úttal
    * /index:1 - módosítani kívánt Windows kiadás (```dism /Get-WimInfo /WimFile:d:\sources\install.wim```)
    * /MountDir:h:\img - melyik mappába csatlakoztassa a képfájlt (nem hozza létre magának, helyette hibát dob, így ezt először létre kell hoznunk, 10 GB hely nem árt, ha van)
3. Jó lenne tudni, milyen szolgáltatásokat tudunk egyáltalán engedélyezni,     * tiltani: ```dism /image:h:\img /Get-Features > c:\features.txt```
    * dism - a program, amivel dolgozunk
    * /image:h:\img - megmondjuk neki, melyik mounttal dolgozzon
    * /Get-Features - lekérdezzük az elérhető rendszerösszetevőket
    * > c:\features.txt - a kimenetet átirányítjuk egy fájlba, mert úgy könnyebb lesz böngészni
4. Ha megvan a kiszemelt áldozat, azt a következőképpen tudjuk tiltani: ```dism /image:h:\img /Disable-Feature:MediaCenter```
    * dism - a program, amivel dolgozunk
    * /image:h:\img - megmondjuk neki, melyik mounttal dolgozzon
    * /Disable-Feature: - tiltani szeretnénk a : utáni szolgáltatást

    Engedélyezés: ```dism /image:h:\img /Enable-Feature:TFTP```
    * dism - a program, amivel dolgozunk
    * /image:h:\img - megmondjuk neki, melyik mounttal dolgozzon
    * /Enable-Feature: - engedélyezni szeretnénk a : utáni szolgáltatást
5. Ha készen vagyunk: ```dism /Unmount-Wim /MountDir:h:\img /commit```
    * dism - a program, amivel dolgozunk
    * /Unmount-Wim - lecsatlakoztatjuk a lemezképet
    * /MountDir:h:\img - ide csatlakoztattuk, megmondjuk, hogy ez kell
    * /commit - mentjük a változásokat
  
    Amennyiben meggondoltuk magunkat: ```dism /Unmount-Wim /MountDir:h:\img /discard```
    * dism - a program, amivel dolgozunk
    * /Unmount-Wim - lecsatlakoztatjuk a lemezképet
    * /MountDir:h:\img - ide csatlakoztattuk, megmondjuk, hogy ez kell
    * /discard - visszavonjuk a változtatásokat

Azt hiszem, látható, hogy a dism egy igen erőteljes szoftver, a leírtaknál még többet is tud (pl. javítások integrálása). Vannak ilyen csili-vili módosító eszközök, SOHA ne használjuk őket.

## ISO készítése

Nem tudhatom, ki hogyan pakolgatta az install.wim, de most a kiindulási alapunk az lesz, hogy a komplett Windows 7 telepítőlemez tartalma a módosított vagy érintetlen install.wim-mel bent van a H:\win7 mappában (maga az install.wim a souces könyvtárba kerül).
http://windows7center.com/news/how-to-install-any-version-or-sku-of-windows-7/
Azt sem tudom, ki mivel ír, de az ImgBurn kiváló alkalmazás, ingyen van, így senkinek sem lesz kárára, ha megismerkedik vele.

### Parancssoros ISO-készítés (Windows 7 boot sector)

A használt oscdimg programot a Windows AIK-ban találhatjuk meg.

```oscdimg -u2 -bE:\win7\boot\etfsboot.com -h -lWIN7UP_201006 E:\win7 H:\win7_update.iso```

* -u2 - UDF file system
* -bE:\win7\boot\etfsboot.com - boot sector, nincs szóköz
* -h - rejtett fájlokat is másolja
* -lWIN7UP_201006 - lemezcímke, nincs szóköz
* E:\win7 - a mappa, amiben a Windows 7 telepítőfájljai vannak
* H:\win7_update.iso - kimeneti fájl

### ImgBurn Windows 7 boot szektorával történő ISO készítés

* Indítsuk el az ImgBurn-t!
* Kattintsunk a "Create image file from files/folders" gombra!
* Kattintsunk a kis mappa ikonra, jelöljük ki a H:\win7 mappát (kinek mit, ugye)!
* Advanced fül a jobb oldalon, majd Bootable Disc!
    * Make Image Bootable
    * Emulation Type: None (Custom)
    * Boot Image: jelöljük ki a H:\win7\boot\etfsboot.com fájlt
    * Developer ID: Microsoft Corporation
    * Load Segment: 07C0
    * Sectors To Load: 8
* Build gomra kell ezután nyomnunk, hogy elinduljon a folyamat.
* Utána mindenre nyomjunk Yes-t, OK-t!

> Ismert probléma a Cannot boot from CD - Code: 5, most (megint) leküzdjük!
Ugyanazt kell csinálnunk, mint fent, csak:
* A Vista etfsboot.com fájlját jelöljük ki (igen, kell hozzá a Vista lemeze, vagy csak a boot mappa belőle),
* Sectors To Load értékét pedig 4-re állítjuk!

Innentől az ISO-t már tudjuk használni akár DVD-re írásra vagy pendrive előkészítésére telepítéshez.