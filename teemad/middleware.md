# Middleware

-   Kirjutamine: https://expressjs.com/en/guide/writing-middleware.html
-   Kasutamine: https://expressjs.com/en/guide/using-middleware.html
-   Milleks middleware'i kasutada?
-   Järjekord on oluline

## Mis on middleware?

-Middleware funktsioonid on funktsioonid, millel on juurdepääs päringuobjektile (req), vastuseobjektile (res) ja järgmisele funktsioonile rakenduse päringu-vastuse tsüklis. Next funktsioon on Express-ruuteri funktsioon, mis käivitamisel käivitab middleware praeguse middleware’i järel.

-   Middleware saab:

1. Käivitada koodi
2. Teha muudatusi request ja response objektides
3. Lõpetada request-response tsüklit
4. Kutsuda välja järjekorrast järgmine middleware

-   Meeles peab pidama, et kui middleware ei lõpeta päringu-vastuse tsüklit (näiteks res.status(200).json…), siis peab middleware kutsuma välja next() funktsiooni, muidu jääb rakendus ‘rippuma’
    Logimise middleware näide:

```
// Http päringute konsooli logimise middleware
import { Request, Response, NextFunction } from 'express';

export default (req: Request, res: Response, next: NextFunction) => {
  // Väljastatakse päringu sihtaadress, meetod ja aeg
  console.log(req.url, req.method, new Date().toISOString());
  // Next funktsiooni käivitamine annab järjekorra üle järgmisele middleware'le
  next();
}
```

Selleks, et middleware't kasutada saaks, peab selle registreerima, seejuures on oluline see, et seda tuleb teha enne route'de defineerimist.

```
...
// Middleware importimine
import logger from './middlewares/logger';

...

// Middleware registreerimine
app.use(logger);

...
```

Skeemi peal näeb middleware kasutamine välja nii:

![Middleware](/pildid/Middleware.jpeg)