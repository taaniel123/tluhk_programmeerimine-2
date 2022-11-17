# JSON Web Token

![Allikas](/pildid/jsonwebtoken.png)

-   JSON Web Token - https://jwt.io/introduction/

-   JSON WEB TOKEN (JWT) on avatud standard, mis määratleb kompaktse ja iseseisva viisi teabe turvaliseks edastamiseks osapoolte vahel JSON-objektina. Seda teavet saab kontrollida ja usaldada, kuna see on digitaalselt allkirjastatud. JWT-sid saab allkirjastada parooliga või avaliku / privaatse võtmepaari abil RSA või ECDSA abil.

## JWT struktuur

Header

-   Päis koosneb tavaliselt kahest osast: loa tüübist, milleks on JWT, ja kasutatavast allkirjastamise algoritmist, näiteks HMAC SHA256 või RSA.

Payload

-   Tokeni teine osa on kasulik koormus, mis sisaldab nõudeid.

Signature

-   Allkirjaosa loomiseks peate võtma kodeeritud päise, kodeeritud kasuliku koormuse, parooli, päises määratud algoritmi ja selle allkirjastama.

Millal JWT-d kasutada?

-   Autoriseerimine: see on kõige tavalisem stsenaarium JWT kasutamiseks. Kui kasutaja on sisse logitud, sisaldab iga järgmine taotlus JWT-d, mis võimaldab kasutajal juurde pääseda selle märgiga lubatud marsruutidele, teenustele ja ressurssidele.

-   Teabevahetus: JSON-i veebimärgid on hea viis turvaliselt osapoolte vahel teavet edastada. Kuna JWT-sid saab allkirjastada - näiteks kasutades avaliku / privaatse võtme paare -, võite olla kindel, et saatjad on need, kes nad end ütlevad. Lisaks, kuna allkiri arvutatakse päise ja kasuliku koormuse abil, saate ka kontrollida, kas sisu ei ole muudetud

Selleks, et saaksime API-s JWT-d kasutada, peame tegema järgmisi tegevusi:

-   Lisama oma projektile jsonwebtoken npm paki: npm install jsonwebtoken - https://www.npmjs.com/package/jsonwebtoken
-   Lisama oma API-le sisselogimise võimaluse
-   Kontroller: https://github.com/mrttlu/esimene/blob/main/api/controllers/authController.j
-   Teenus: https://github.com/mrttlu/esimene/blob/main/api/services/authService.js
-   Route: app.post('/api/login', authController.login);
-   Sisse logimiseks saadab klient API-le oma kasutajanime (siin kasutusel e-mail) ja parooli.
-   API kontrollib, kas sellise e-mailiga kasutaja on olemas ja kas kasutaja poolt saadetud parool klapib andmebaasis oleva hashiga.

```
const user = await usersService.readByEmail(email);
if (user) {
  const match = await hashService.compare(password, user.password);
  ...
```

-   Selleks kasutatakse Bcrypti match funktsiooni, mis võtab parooli ja võrdleb seda hashiga.

```
hashService.compare = async (password, hash) => {
  const match = await bcrypt.compare(password, hash);
  return match;
}
```

-   Kui e-mail või parool ei sobi, saadetakse kliendile veateade.
-   Kui e-mail ja parool olid korrektsed, siis luuakse JWT token, mille payloadi lisatakse kasutaja andmebaasi id (selle abil saame hiljem andmebaasist päringuid teha) - token luuakse parooli abil, mida teab ainult API.

```
const token = jwt.sign({ id: user.id }, config.jwtSecret, { expiresIn: 60 * 60 });
```

-   Token saadetakse tagasi kliendile, et klient saaks selle lisada edaspidi päringuid tehes headerisse Authorization Bearer tokenina.

```
res.status(200).json({
  success: true,
  token: token
});
```

-   Selle API vastus koos tokeniga näeb välja selline:

```
{
    "success": true,
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiaWF0IjoxNjA2ODkxMjE5LCJleHAiOjE2MDY4OTQ4MTl9.ZE4XPyZhY2kJvktoIlDNowKvAdKHdxlKchrG_f_cABE"
}
```

-   Kui kasutame API testimiseks Postmani, siis saab siit vastusest võtta tokeni ja kopeerida selle Postmani:

![Token Postmanis](/pildid/postmanToken.png)

-   Lisame API-le middleware isLoggedIn, mis registreeritakse enne neid endpointe, mille puhul kasutaja peab olema sisse logitud.

```
...
app.post('/api/login', authController.login);
app.post('/api/users', usersController.create);

app.use(isLoggedIn);

app.get('/api/users', usersController.read);
app.get('/api/users/:id', usersController.readById);
...
```

Enne kui request objekt jõuab kontrollerisse, kontrollib middleware:

-   Kas request objekti headeris on Authorization Bearer kirje
-   Kui on, siis võtab sealt tokeni, vastasel juhul tagastab kliendile veateate.
-   Kontrollib, kas token on korras (parooli abil, millega token on loodud).
-   Kui token on korras, siis võetakse tokeni payloadist kasutaja id ja lisatakse see request objektile ja antakse tegevus üle kontrollerile.
-   Kui token ei olnud korras, tagastatakse kliendile veateade.

```
const isLoggedIn = (req, res, next) => {
  try {
    // Kontrollime kas token on headeris olemas
    const token = req.headers.authorization.substring(7);
    // Kontrollime kas token on valiidne
    const verified = jwt.verify(token, config.jwtSecret);
    if (verified) {
      // Kirjutame tokeni payloadis oleva id request objekti sisse
      req.user = verified.id;
      // Anname tegevuse üle järgmisele middlewarele (kontrollerile)
      next();
    }
  } catch (error) {
    // Lõpetame vea puhul request-response tsükli ja saadame kliendile veateate
    res.status(401).json({
      success: false,
      message: 'Invalid token'
    });
  }
}
```
