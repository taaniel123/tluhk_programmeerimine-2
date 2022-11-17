# API Liidestamine andmebaasiga (MySQL) TypeScript

-   Kõigepealt tuleks arvutisse installeerida MySQL-i server ( https://dev.mysql.com/downloads/mysql/ ) või kasutada mingit olemasolevat varianti
-   Kes oskab, sellel soovitan kasutada MySQLi Dockeri konteinerit ( https://hub.docker.com/_/mysql )
-   Seejärel tuleks luua andmebaasi mudel, luua andmebaas ja tabelid.

Minu loodud andmebaasi mudel näeb välja selline:

![andmebaasimudel](/pildid/andmebaasi_mudel.png)

-   Mudeli loomiseks on kasutatud MySQL Workbench tööriista (https://dev.mysql.com/downloads/workbench/), mille abil on väga mugav võtta otse andmebaasiga ühendust, teha tabeleid, luua nende vahel seoseid, teha päringuid jne.
-   Andmebaasi tabelite loomise ja esimeste andmete sisestamise päringud on siin: https://github.com/mrttlu/programmeerimine2_2021_sygis/blob/mysql/docs/modelAndSeed.sql
-   Paigalda oma API-le mysql2 npm pakk (https://www.npmjs.com/package/mysql2) - npm install mysql2
-   MySQL2 põhjalikum dokumentatsioon on leitav siit: https://github.com/sidorares/node-mysql2

-   Nagu ikka, teeme andmebaasi ühenduse jaoks eraldi faili database.ts - sinna impordime mysql2 teegi, loome andmebaasi ühenduse ja ekspordime selle, et seda oleks võimalik eraldi teenuste all kasutada.

database.ts fail võiks välja näha siis selline:

```
import mysql from 'mysql2';
import path from 'path';
import fs from 'fs';
import config from './config';

const pool = mysql.createPool({
  host: config.db.host,
  user: config.db.user,
  password: config.db.password,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
  multipleStatements: true,
}).promise();

export default pool;
```

-   Nagu näha, siis eeldab ühenduse loomine config failis config.db kirje olemasolu. Need ühenduseks vajalikud parameetrid peaksid olema jällegi eraldi failis, mida githubi ei salvestata, sest muu hulgas sisaldab näiteks parooli. Selle, kuidas config.ts fail välja näeb, saab vaadata config.sample.ts failist. config.sample.ts faili sisu:

```
const config = {
  jwtSecret: 'jwtsecret',
  db: {
    host: 'localhost',
    user: 'root',
    password: 'testpassword',
    database: 'blog',
  },
};

export default config;
```

-   Nüüd on võimalik hakata andmebaasi kasutama teenuste all.
-   Selleks tuleb teenuse all kõigepealt importida andmebaasi ühendus ja siis saab hakata juba päringuid kirjutama

```
/* eslint-disable no-console */
import { FieldPacket, ResultSetHeader } from 'mysql2';
import pool from '../../database';
import { IUpdateUser, INewUser, IUser } from './interfaces';
import hashService from '../general/services/hashService';

const usersService = {
  getAllUsers: async (): Promise<IUser[] | false> => {
    try {
      const [users]: [IUser[], FieldPacket[]] = await pool.query('SELECT id, firstName, lastName, email, dateCreated, role FROM users WHERE dateDeleted IS NULL');
      return users;
    } catch (error) {
      console.log(error);
      return false;
    }
  },
};
```

-   SQL-päringute käivitamisel peab meeles pidama seda, et kui päringud sisaldavad kasutaja poolt saadetud infot (näiteks e-mail, parool, ükskõik mida klient API-sse saadab), siis annab see võimaluse kasutada SQL injection rünnakut. (Selle kohta saab lugeda näiteks PortSwigger'i artiklist siit: https://portswigger.net/web-security/sql-injection).
-   SQL injectioni vältimiseks tuleb kasutada parameetrilisi päringuid, kus kasutaja poolt saadetud muutujaid ei kirjutata otse päringusse sisse.

```
// Selle asemel, et kirjutada kasutaja poolt saadetud infot otse päringusse kujul:
const [user]: [IUser[], FieldPacket[]] = await pool.query('SELECT * FROM users WHERE email = email AND dateDeleted IS NULL;');

// Tuleks seda teha sellisel kujul:
const [user]: [IUser[], FieldPacket[]] = await pool.query('SELECT * FROM users WHERE email = ? AND dateDeleted IS NULL', [email]);

// Kui muutujaid on rohkem siis tuleb ikkagi muutujate asemele päringusse kirjutada ? ja pärast [] sisse lihtsalt muutujad komadega eraldatult
const [result]: [ResultSetHeader, FieldPacket[]] = await pool.query(`INSERT INTO users SET firstName = ?, lastName = ?, email = ?, password = ?`, [user.firstName, user.lastName, user.email, user.password]);
```
