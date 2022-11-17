# API struktuur

API struktuur

-   Controller
-   Service
-   Routes
-   Helpers

Praeguseks on API-l kokku 4 endpointi, milles saab teha päringuid andmete lugemiseks, kirjutamiseks, muutmiseks ja kustutamiseks. Programmikoodi on kokku üle 600 rea, kusjuures loogikat ja kontrolle on tehtud väga minimaalselt. Mis siis, kui endpointe oleks rohkem? näiteks 20 või 40 vi 100? Kokkuvõttes läheks kood väga raskelt loetavaks. Struktureerimisega saame jagada koodi vastavalt selle eesmärgile väiksemateks tükkideks, mis annab koodile parema loetavuse, taaskasutatavuse ja ka testimise.

Algselt näeb sellest API-st päringu tegemine välja selliselt:

![api enne](/pildid/api1.jpeg)

Struktureerimise eesmärk on jagada kood tükkideks vastavalt ülesannetele.

Kui API-le tuleb päring, siis:

Router saadab päringu vastavale kontrollerile

1. Kontroller teeb:

-   Esmase kontrolli (kas vajalikud andmed on olemas)
-   Vaatab, mida on vaja vastuse tegemiseks
-   Küsib vastavatelt teenustelt vajalikud andmed
-   Moodustab vastuse kliendile
-   Saadab vastuse kliendile tagasi
-   Vajadusel kirjutab logisse, jms.

2. Teenused teevad päringuid andmebaasidesse ja saadavad vastuse kontrollerile
3. Lisaks võivad olla veel erinevad abiprogrammid, middlewared jms, mida meie koodis hetkel veel ei ole

Teises loengus tehtud struktuuri tagajärjel näeb kasutaja pärimine välja selline:

![api pärast](/pildid/api2.jpeg)
