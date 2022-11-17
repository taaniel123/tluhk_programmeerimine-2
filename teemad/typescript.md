# Typescript

## Mis on typescript?

https://www.typescriptlang.org/

Põhimõtteliselt on Typescript tüüpidega Javascript. Kui Javascripti üks suur probleem on see, et muutuja tüüp ei ole määratud, ehk muutuja tüüpi saab igal hetkel muuta (let a = 1 ja järgmisel hetkel saad omistada a = 'one'), siis typescriptis määratakse muutuja deklareerimisel selle tüüp ja seda tüüpi ei saa hiljem enam muuta (let a: number = 1 ning kui püüad omistada a = 'one', siis tekib viga).

Näiteks:

```
let age: number = 30;
let street: string = 'Metsa';

interface Person = {
  firstName: string;
  lastName: string;
  age: number;
}

const person: Person = {
  firstName: 'Juku',
  lastName: 'Juurikas',
  age: 30,
};
```

Typescriptis kirjutatud kood kompileeritakse enne käivitamist tavaliseks javascriptiks ja koodi kirjutamisel võib põhimõtteliselt kasutada ka tavalist javascripti (võid näiteks alustada tavalise javascripti koodi kirjutamisega ja lisada tüübid hiljem).

Typescriptis kasutatavatest tüüpidest saad rohkem lugeda siit: https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html

Typescripti mõte on aidata arendajat koodi kirjutamisel vigu vältida.

Typescripti faili eristab javascripti failist laiend \*.ts

## Node typescript projektiga alustamine

-   Package.json faili loomine: npm init -y
-   Typescripti paigaldamine: npm install typescript
-   tsconfig.json faili (typescripti seaded) loomine: npx tsc --init või loo käsitsi ja kopeeri sinna sisse:

```
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig.json to read more about this file */
    "target": "es6",                                     /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    "module": "CommonJS",                                /* Specify what module code is generated. */
    "rootDirs": ["src/**/*", "./", ".", "src"],          /* Allow multiple folders to be treated as one when resolving modules. */
    "resolveJsonModule": true,                           /* Enable importing .json files */
    "outDir": "./build",                                 /* Specify an output folder for all emitted files. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables `allowSyntheticDefaultImports` for type compatibility. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */
    "strict": true,                                      /* Enable all strict type-checking options. */
    "skipLibCheck": true ,
    "sourceMap": true                                /* Skip type checking all .d.ts files. */
  }
}
```

-   Node tüübidefinitsioonide paigaldamine: npm install @types/node --save-dev

Testime, kas asi töötab:

-   Loo kõigepealt projekti juurkausta kaust nimega src ja tekita sinna uus fail index.ts
-   Ava index.ts fail ja kirjuta sinna sisse: console.log('Hello world!');
-   Terminalis kirjuta npx tsc - selle käsu tagajärjel luuakse uus fail build kausta, mille sisuks on javascripti kompileeritud kood.
-   Nüüd saad kompileeritud koodi käivitada käsuga node ./build/index.js

Selleks, et ei peaks kogu aeg käsitsi koodi kompileerima ja käivitama, saab kasutada eraldi utiliiti, mis jälgib koodis tehtud muudatusi ja koodi salvestamisel taaskäivitab rakenduse iseseisvalt.

-   npm install --save-dev ts-node nodemon
    ts-node võimaldab typescriptis kirjutatud koodi käivitada ilma seda enne javascripti kompileerimata - https://www.npmjs.com/package/ts-node
    nodemon - node monitor - võimaldab jälgida failide muutumist ja node taaskäivitamist
-   Loo projekti juurkausta uus fail nimega nodemon.json ja kopeeri sinna sisse:

```
{
  "watch": ["src"], // Kaust, mille sees olevate failide muudatusi jälgitakse
  "ext": ".ts,.js", // Failide laiendid, mille muudatusi jälgitakse
  "ignore": [],
  "exec": "ts-node ./src/index.ts" // Käivitamise käsk
}
```

Lisa package.json faili "script" jaotisesse:

```
"scripts": {
    "build": "tsc",
    "start": "nodemon"
},
```

Nüüd terminalis käivitades npm start peaks projekt tööle hakkama.
