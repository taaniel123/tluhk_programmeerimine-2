# Kasutajad

## Autentimine ja autoriseerimine

**Autentimine** on protsess, millega üks kasutaja, süsteem või muu olem (objekt) saab kontrollida teise olemi **väidetava identiteedi tõesust**, tavaliselt mingit tüüpi **identsustõendi** alusel:

-   miski, mida **Sa tead** (näiteks parool, PIN-kood, robotilõks, turvaküsimus);
-   miski, mis **Sul on** (näiteks ID-kaart, pangakaart, telefoni number, e-mail, riistvarapääsmik, paroolikaart, sertifikaat) või
-   miski, mis **Sa oled** (sõrmejälg, näo pilt või topograafia, iirise struktuur, ...). (Veri, 2016)

**Autoriseerimine ehk volitamine** on protsess, mis annab (või keelab) õiguse ligi pääseda (võrgu)ressurssidele. Näiteks enamik e-kaubanduse turvasüsteeme põhineb kaheastmelisel protsessil. Esiteks toimub **autentimine**, kus kontrollitakse, et kasutaja on tõesti see, kellena ta esineb, seejärel toimub **autoriseerimine**, mis lubab kasutajal juurde pääseda temale ette nähtud ressurssidele.

[Allikas](https://sisu.ut.ee/autentimine/m%C3%B5isted)

Selleks, et me saaksime oma API-le tekitada autentimise ja autoriseerimise, on meil vaja kõigepealt luua kasutajate jaoks andmebaasi tabel, kasutajate loomise/muutmise/kustutamise võimalus ja sisselogimise võimalus.

Selleks teeme oma API-le kasutajate tabeli, kuhu salvestatakse kasutaja eesnimi, perekonnanimi, email (kasutajanimi), kasutaja roll ja parool.

Lisaks oleks vaja võimalust kasutajaid luua, muuta, kustutada ja ka võimalust sisse logimiseks.

Sisselogimisel on vaja kontrollida kasutajanime ja parooli õigsust ja vastavalt sellele kasutajat edasi lubada või mitte. Lisaks peaks olema võimalik kontrollida kasutaja rolli (näiteks tavakasutaja või administraator) ja selle alusel otsustada, kas kasutaja lubada mingite ressursside/tegevuste juurde või mitte.

Edaspidi peab otsustama milliste ressursside juurde keegi pääseb - näiteks kas kasutaja näeb ainult oma loodud ressursse, kes saab loodud ressursse muuta jne.

Näitena tehtud API-s on plaanis teha:

-   Kasutaja saab lisada vabandusi, millele saab määrata, kas need on avalikud või mitte. Avalikke vabandusi näevad kõik, kuid privaatseid vabandusi näeb ainult nende looja.
-   Kasutaja andmeid saab muuta ainult kasutaja ise või administraator.
-   Kasutajate nimekirja näha ainult administraator.
-   jne
