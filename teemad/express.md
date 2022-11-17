# Express

Väga populaarne Node.js veebirakenduste raamistik https://expressjs.com/

Express-i allalaadimiste võrdlus teiste sarnaste raamistikega.

![SKAALA](/pildid/skaala.png)

https://www.npmtrends.com/express-vs-koa

Lihtne Express rakendus

**Järgnev juhend eeldab, et on läbitud sammud, mis on kirjeldatud eelnevalt Typescripti peatükis.**

1. Expressi paigaldus: npm install --save express

-   Expressi tüüpide definitsioonid: https://www.npmjs.com/package/@types/express

2. Expressi tüüpide definitsioonide paigaldamine: npm install --save @types/express

3. Loo uus fail (näiteks index.ts)

4. Kirjuta või kopeeri loodud faili sisse järgmised read:

```
import express, { Request, Response, Express } from 'express';

const app: Express = express();
const port: number = 3000;

app.get('/', (req: Request, res: Response) => {
  res.status(200).json({
    success: true,
    message: 'Hello world!',
  });
});

app.listen(port, () => {
  // eslint-disable-next-line no-console
  console.log(`App is running on port ${port}`);
});
```

5. Salvesta fail

6. Käivita loodud rakendus terminalist käsuga npm start

7. Mine veebilehitsejas aadressile http://localhost:3000/hello

8. Peaksid nägema järgmist vastust: {"success": true,"message": "Hello world!"}

## Põhilised meetodid

https://expressjs.com/en/4x/api.html#app.METHOD

-   app.get()
-   app.post()
-   app.delete()
-   app.put()
-   app.use()
-   app.listen()

## Request objekti struktuur (req)

https://expressjs.com/en/api.html#req

-   req.body
-   req.params
-   req.query
-   req.method
-   req.baseUrl

## Response objekti meetodid

https://expressjs.com/en/api.html#res

-   res.end()
-   res.json({key: value})
-   res.status(code)
