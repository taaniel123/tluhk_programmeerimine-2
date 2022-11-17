# HTTP response koodid

Allikad:

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html

Enamkasutatavad koodid

**Edukad**:

-   200 - OK - Päring õnnestus ja tagastatakse sisu.
-   201 - Created - Uus ressurss on loodud (vastus saadetakse pärast seda, kui uus ressurss on loodud ja vastus peaks sisaldama infot uue ressursi kohta - näiteks loodud ressursi id)
-   204 - No Content - Päring õnnestus, server on teinud oma toimingud, kuid ei ole vaja midagi tagastada.

**Vead**

-   400 - Bad request - Server ei saa päringut töödelda või ei hakka seda töötama kliendi veana peetava asja tõttu.
-   401 - Unauthorized - Päring nõuab kasutaja autentimist.
-   403 - Forbidden - Server mõistis taotlust, kuid keeldub seda täitmast.
-   404 - Not Found - Server ei leia soovitud ressurssi.

**Serveri vead**

-   500 - Internal Server Error - Serveril tekkis ootamatu probleem, mis takistas tal päringut täitmast.
