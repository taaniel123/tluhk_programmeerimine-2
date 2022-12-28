# Andmete saatmine Express API-le

Kui tahame Express API-le saata mingit infot (uue ressursi loomiseks, või päringutele lisainfo lisamiseks), saab seda teha mitut moodi:

-   request body-s 
-   route parameetrina
-   päringuparameetrina (query parameter)
-   päringu päises

## Request body-s

Selleks, et Express API-le saaks saata request bodys andmeid, peab kõigepealt olema Express API-s registreeritud app.json() middleware, mis tekitab express req-objekti sisse omakorda body objekti, mis sisaldab kliendist body-s kaasa saadetud JSON objekti (vastasel korral on req-objekti sees body lihtsalt ei ole)

```
const express = require('express');
const app = express();
const port = 3000;

// Middleware for creating req.body in express app
app.use(express.json());
...
```

Kui nüüd Postmaniga näiteks saata selline POST päring:

![post päring postmanis](/pildid/post_paring.png)

ja vaadata Expressi saabunud req-objekti sisse:

![req objekt](/pildid/req_objekt.png)

siis on seal näha body objekti sees nii categoryId, kui description, mille saame req-objekti seest kätte näiteks nii:

```
const description = req.body.description;
const categoryId = req.body.categoryId;

// või

const { description, categoryId } = req.body;
```

## Route parameetrina

Kui soovime määrata näiteks soovitava ressursi id-d, siis on seda võimalik teha route parameetrina, ehk endpoindi aadressi sees mingi muutujana. Näiteks GET päring http://localhost:3000/excuses/1 - kus 1 on soovitava vabanduse id.

Selleks peab olema express API-s määratud route teekonnas id parameeter:

```
app.get('/excuses/:id', (req, res) => {
  //
});
```

Teekonnas kirjeldatud id on muutuja nimi, kuhu saadetud parameeter salvestatakse ja selleks, et anda expressile teada, et tegemist on muutujaga, peab see algama kooloniga (muidu arvab express, et tegemist on lihtsalt route teekonna nimega).

Kui nüüd saata API-le päring näiteks endpoindile: http://localhost:3000/excuses/2, siis näeme req-objekti sisse vaadates params objekti:

![params objekt](/pildid/params_objekt.png)

params objekti sees on näha id väärtusega '2'. Tasub tähele panna, et kuigi päringus on tegemist justkui arvuga, siis API-sse jõudes on tegemist stringiga.

Ja lugeda saame seda id-d järgmiselt:

```
const id = req.params.id;

// või

const { id } = req.params;
```

## Päringuparameetrina (query parameter)

Päringuparameetrid on veebiaadressi lõppu lisatavad parameetrid, mis eraldatakse aadressist ? - märgiga, näiteks:

http://localhost:3000/excuses?lang=ee

Mitme päringuparameetri saatmisel eraldatakse need omavahel & - märgiga, näiteks:

http://localhost:3000/excuses?lang=ee&order=desc

Eelnevalt kirjeldatud päringu saatmisel näeme API-s req-objekti sisse vaadates query objekti:

![params objekt](/pildid/query_objekt.png)

Ja kätte saab need parameetrid sarnaselt eelmistele variantidele nii:

```
const lang = req.query.lang;
const order = req.query.order;

// või

const { lang, order } = req.query;
```
