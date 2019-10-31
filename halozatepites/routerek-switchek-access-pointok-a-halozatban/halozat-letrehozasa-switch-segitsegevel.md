# Hálózat létrehozása switch segítségével

### Hálózati elemek:
* Switch vagy switchet tartalmazó router
* LAN kapcsolódású számítógépek
* Hálózati kábelek (CAT5e patch kábelek)
* Ha szükséges, akkor kiegészítő LAN hálózati kártyák

Nagyon egyszerű a dolgunk, csupán össze kell kötnünk az összes számítógépet a switchcsel (lehetőleg Cat5e kábellel), futtatnunk a hálózatbeállító varázslót, majd fix IP-címeket kiadnunk (pl.: 10.0.0.1-255).
Azt is megcsinálhatjuk, hogy a switchbe csatlakoztatunk egy routert, amin keresztül jön a net, így a switchen lógó gépek is kapnak internetet, persze ilyenkor használható a router DHCP-szervere. Ezzel a módszerrel gyors helyi hálózatot is létrehozhatunk, ha gigabites switchet alkalmazunk.
Itt is érvényes, hogy a tűzfalat megfelelően állítsuk be (megosztások ne legyenek tiltva, ha szükségesek)!

Ha nekünk csak egy switchet tartalmazó routerünk van, akkor sem kell pánikolni, hiszen 1:1-ben használható lesz. DHCP maradhat bekapcsolva is, de ha mi nem szeretnénk használni, akkor kapcsoljuk ki, majd a fent leírt módon adjunk kézzel minden gépnek 1-1 megfelelő IP-t, aztán lehet is pingelni a másikat.