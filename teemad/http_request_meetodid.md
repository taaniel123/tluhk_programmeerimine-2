# HTTP request meetodid

HTTP request meetodid https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods

-   GET - määratud ressursi pärimine (näiteks GET /api/excuses tagastab vabanduste nimekirja)
-   POST - määratud ressursile üksuse edastamine (näiteks POST /api/excuses päringuga saadetakse vabanduste andmed uue vabanduse lisamiseks andmebaasi)
-   PATCH - määratud ressursile üksuse edastamine olemasoleva üksuse muutmiseks (näiteks PATCH /api/excuses/:id päringuga saadetakse kaasa andmed määratud id-ga vabanduse andmete muutmiseks)
-   DELETE - kustutab määratud ressursi (näiteks DELETE /api/excuses/:id kustutab vabanduse määratud id-ga)

PUT ja PATCH päringud on mõlemad mõeldud olemasoleva ressursi muutmiseks, vahe on selles, et PUT-päringuga kirjutatakse üle kogu ressurss (väärtused, mida kaasa ei saadeta, asendatakse olemasolevas ressursis null-iga), PATCH-päringuga muudetakse ainult need ressursi väärtused, mis päringuga kaasa saadetakse.

Näiteks, kui on kasutaja ressurss, mis sisaldab eesnime ja perekonnanime ({"eesnimi": "Juku", "perekonnanimi": "Juurikas"}), siis:

PUT {"eesnimi": "Juhan"} tulemus on {"eesnimi": "Juhan", "perekonnanimi": null}

PATCH {"eesnimi": "Juhan"} tulemus on {"eesnimi": "Juhan", "perekonnanimi": "Juurikas"}
