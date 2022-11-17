# Testimine - Typescript

![testimise meem](/pildid/testing_meme.png)

## Testimisest

Majandus- ja kommunikatsiooniminsteeriumi Veebilehe WCAG 2.0 juhendi nõuetele vastavuse väljaselgitamise testimise juhend: https://www.mkm.ee/et/eesmargid-tegevused/wcag-20-rakendusjuhised/testimine
Majandus- ja kommunikatsiooniminsteeriumi tarkvara testimise rakendusjuhis: https://www.mkm.ee/sites/default/files/tarkvara_testimise_juhis_-_koopia.doc

https://martinfowler.com/articles/practical-test-pyramid.html

## API testimine

Selle kursuse raames räägime põhiliselt enpointide testimisest, mis sisaldab suures plaanis kahte erinevat testimise tüüpi:

-   Turvalisuse testimist - kas kasutaja saab/ei saa ligi mingitele endpointidele, millele on tal luba/ei ole ligipääsuks
-   Funktsionaalsuse testimist - kas endpoindid töötavad nii, nagu me eeldame (tagastavad andmeid kujul, mida ootame, tagastavad HTTP vastuskoodid kujul mida ootame, veateated jne.)

## Mocha

Mocha on javascripti testiraamistik, mis töötab nii node.js-is, kui veebilehitsejas. Võimaldab testide kirjutamist.

https://mochajs.org/#getting-started

npm install --save-dev mocha

npm install --save-dev @types/mocha

Testid salvestatakse test kausta, mis peab olema root kaustas.

package.json failis muuta test skripti sektsiooni:

```
"scripts": {
"start": "nodemon",
"test": "mocha -r ts-node/register 'tests/\*_/_.ts'"
},
```

Testide käivitamine käsuga: npm test

Kui testide käivitamine jääb "rippuma", siis lisa package.json faili mocha järel --exit võti:

```
"scripts": {
"start": "nodemon",
"test": "mocha -r ts-node/register 'tests/\*_/_.ts' --exit"
},
```

### Esimene test:

Tee test kausta fail test.ts ja kopeeri sinna sisse:

```
import assert from 'assert';
import { describe, it } from 'mocha';

describe('Array', () => {
  describe('#indexOf()', () => {
    it('should return -1 when the value is not present', () => {
      assert.equal([1, 2, 3].indexOf(4), -1);
    });
  });
});
```

Proovi nüüd test käivitada käsuga: npm test

Eeldatavalt peaks olema väljund midagi sarnast:

```
Array

    #indexOf()


      ✓ should return -1 when the value is not present
```

## Mocha hooks:

_Hook'e_ kasutatakse juhul, kui on vaja teha teatud toiminguid enne või peale teste/testi - nagu näiteks luua andmebaasi kirjed või kustutada hiljem andmebaasist testide poolt tekitatud andmed jne.

```
import {
  describe, before, after, beforeEach, afterEach,
} from 'mocha';


describe('hooks', () => {
  before(() => {
    // käivitatakse ühe korra enne esimest testi selles blokis
  });

  after(() => {
    // käivitatakse ühe korra pärast viimast testi selles blokis
  });

  beforeEach(() => {
    // käivitatakse enne igat testi selles blokis
  });

  afterEach(() => {
    // käivitatakse peale igat testi selles blokis
  });

  // testid
});
```

## Chai

Assertion library - tööriist, mis võimaldab kontrollida, kas saadud tulemused vastavad eeldustele, näiteks kas tagastatud andmed on objekti kujul: expect(response.body).to.be.a('object');

https://www.chaijs.com/

https://www.npmjs.com/package/chai

npm install --save-dev chai

npm install --save-dev @types/chai

Chai cheatsheet: https://devhints.io/chai

## Supertest

API endpointide testimine - võimaldab saata päringuid API endpointidele ja lugeda saabunud vastust (response)

https://github.com/visionmedia/supertest

npm install --save-dev supertest

npm install --save-dev @types/supertest

Enne endpointide testide kirjutamist on vaja teha väike muudatus API struktuurist. Nimelt tuleb express app-i loomine ja selle käivitamine panna eraldi failidesse, kuna testimisel käivitatakse api testi enda poolt ja vastasel juhul võib tekkida olukord, kus kasutatav api port on juba hõivatud.

Selleks peab tõstma index failist kõik, mis ei puuduta api käivitamist (app.listen), eraldi faili, mille nimeks võib panna näiteks app.js. Lisaks tuleb loodud express app ka eksportida, et seda oleks võimalik testimisel ja API käivitamisel kasutada.

Tulemus võiks olla midagi sellist:

app.ts:

```
/* eslint-disable no-console */
import express, { Application } from 'express';
import swaggerUi from 'swagger-ui-express';
import cors from 'cors';
import openapi from './openapi.json';

import loggerMiddleware from './components/general/middlewares/middlewares';
import postsRouter from './components/posts/routes';
import pingRouter from './components/ping/routes';
import usersRouter from './components/users/routes';

import { login } from './components/users/controller';
import isLoggedIn from './components/general/middlewares/isLoggedIn';

const app: Application = express();

app.use(cors());
app.use(express.json());
app.use(loggerMiddleware);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(openapi));

// Register ping routes
app.use('/ping', pingRouter);
// Register posts routes
app.use('/posts', isLoggedIn, postsRouter);
// Register users routes
app.use('/users', usersRouter);

app.post('/login', login);

export default app;
```

index.ts:

```
import app from './app';

const port: number = 3000;

app.listen(port, () => {
console.log(`Server is running on port: ${port}`);
});
```

Järgmisena saab juba kirjutada esimese testi, näiteks proovida küsida postituste nimekirja ilma sisse logimata:

```
import request from 'supertest';
import { expect } from 'chai';
import { describe, it } from 'mocha';
import app from '../src/app';

describe('Posts controller', () => {
  describe('GET /posts', () => {
    it('responds with error message and status 401', async () => {
      const response = await request(app).get('/posts');
      expect(response.body).to.be.a('object');
      expect(response.statusCode).to.equal(401);
      expect(response.body.error).to.equal('No token provided');
    });
  });
});

```

Testidega kaetus: nyc
https://www.npmjs.com/package/nyc
