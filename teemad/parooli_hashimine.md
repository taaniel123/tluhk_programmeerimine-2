# Parooli hashimine

-   Parooli hashimine. Praegu on meil 'andmebaasis' kasutaja parool salvestatud lihtsalt tavalise stringina. Kui pärime kasutajate andmeid, on andmete hulgas parool kenasti näha ja loetav. See ei ole hea praktika. Paroolid peavad olema andmebaasis salvestatud sellisel kujul, et neid ei oleks võimalik lugeda nii, et on aru saada, mis parooliga tegemist on. Selleks kasutatakse parooli hashimist. Nodes saame kasutada sellist npm pakki, nagu bcrypt (https://www.npmjs.com/package/bcrypt).

-   Bcrypt on funktsioon andmete krüpteerimiseks. Sellest, mis bcrypt on ja kuidas see täpsemalt toimib, saate lugeda näiteks Dan Arias'e artiklist siin: https://auth0.com/blog/hashing-in-action-understanding-bcrypt/ Meie API-s peaks bcrypti kasutamine käima nii, et kui me loome uue kasutaja, siis enne, kui kasutaja andmed 'andmebaasi' salvestatakse, hashitakse parool ära ja tavalise stringi asemel salvestatakse 'andmebaasi' hashitud parool. Selle jaoks lisame oma API-le uue teenuse 'hashService.ts' ja sinna lisame kaks funktsiooni, millest üks on parooli hashimiseks ja teine on hiljem parooli võrdlemiseks hashiga. Lisaks paigaldame bcrypt'i käsuga npm install bcrypt. Samuti lisame bcrypti tüübikirjeldused käsuga npm install --save-dev @types/bcrypt. hashService.ts fail näeb välja umbes selline:

```
// Impordime bcrypt'i
import bcrypt from 'bcrypt';
// Järgnev muutuja määrab ära, kui palju peab tööd tegema parooli
// hashimiseks (mida suurem number, seda rohkem on vaja vaeva näha)
const saltRounds = 10;

const hashService = {
  // Funktsioon parooli hashimiseks - funktsioon tagastab hashitud parooli
  hash: (password: string): Promise<string> => {
    const hash = bcrypt.hash(password, saltRounds);
    return hash;
  },
  // Funktsioon parooli võrdlemiseks hashiga - funktsioon tagastab true või false vastavalt võrdlemise tulemusele
  compare: (password: string, hash: string): Promise<boolean> => {
    const match = bcrypt.compare(password, hash);
    return match;
  },
};

// Ekspordime loodud objekti, et saaksime seda teenust mujal kasutada
export default hashService;
```

Kuna parooli hashimine on tegevus, mis võtab aega, siis node eripärast lähtudes (asünkroonsus) võib juhtuda olukord, kus parooli hashimine võtab aega nii kaua, et koodi täitmine läheb enne edasi, kui parool hashitud saab. Seetõttu peame ütlema programmi koodile, et ära enne järgmise koodirea juurde liigu, kui hashimine on tehtud. Selleks saame kasutada async/await funktsionaalsust - [Making asynchronous programming easier with async and await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises).

```
// Kui teame, et mingi funktsiooni sees toimub aeganõudev tegevus, siis peame kasutama selle funktsiooni deklareerimise juures async märksõna
const timeConsumingFunction = async () => {
  // Kui tahame ära oodata vastuse, enne kui kood edasi läheb, kasutame await märksõna
  const result = await someFunction();
  // Vastus tagastatakse alles siis, kui result on käes
  return result;
}
```

Async/await muutub eriti oluliseks siis, kui hakkame oma API-t andmebaasiga liidestama, sest andmete pärimine võtab tihtipeale aega.

Funktsioonid hashService faili sees tuleb teha selliseks:

```
// Impordime bcrypt'i
import bcrypt from 'bcrypt';
// Järgnev muutuja määrab ära, kui palju peab tööd tegema parooli
// hashimiseks (mida suurem number, seda rohkem on vaja vaeva näha)
const saltRounds = 10;

const hashService = {
  // Funktsioon parooli hashimiseks - funktsioon tagastab hashitud parooli
  hash: async (password: string): Promise<string> => {
    const hash = await bcrypt.hash(password, saltRounds);
    return hash;
  },
  // Funktsioon parooli võrdlemiseks hashiga - funktsioon tagastab true või false vastavalt võrdlemise tulemusele
  compare: async (password: string, hash: string): Promise<boolean> => {
    const match = await bcrypt.compare(password, hash);
    return match;
  },
};

// Ekspordime loodud objekti, et saaksime seda teenust mujal kasutada
export default hashService;

```

Meeles peab seejuures pidama seda, et async/await märksõnu on vaja kasutada nüüd ka nendes funktsioonides, mis hashimise funktsioone välja kutsuma hakkavad (users/service ja users/controller) Nüüd, kui meil on olemas teenud, mis oskab parooli hashida, saame seda kasutada kasutaja andmete salvestamisel. Andmete salvestamisega tegeleb usersService funktsioon 'createUser'

```
import db from '../../db';
import { User, UpdateUser, NewUser } from './interfaces';
import hashService from '../general/services/hashService';

const usersService = {
  getAllUsers: (): User[] => {
    const { users } = db;
    return users;
  },
  /**
   * Returns user or undefined
   */
  getUserById: (id: number): User | undefined => {
    const user = db.users.find((element) => element.id === id);
    return user;
  },
  removeUser: (id: number): boolean => {
    const index = db.users.findIndex((element) => element.id === id);
    db.users.splice(index, 1);
    return true;
  },
  // Uue kasutaja lisamise funktsioon
  createUser: async (newUser: NewUser): Promise<number> => {
    const id = db.users.length + 1;
    // Hashime loodava kasutaja parooli
    const hashedPassword = await hashService.hash(newUser.password);
    // Lisame kasutaja 'andmebaasi' koos hashitud parooliga
    db.users.push({
      id,
      ...newUser,
      password: hashedPassword,
    });
    return id;
  },
  updateUser: (user: UpdateUser): boolean => {
    const { id, firstName, lastName } = user;
    const index = db.users.findIndex((element) => element.id === id);
    if (firstName) {
      db.users[index].firstName = firstName;
    }
    if (lastName) {
      db.users[index].lastName = lastName;
    }
    return true;
  },
  getUeserByEmail: async (email: string): Promise <User | undefined> => {
    const user = db.users.find((element) => element.email === email);
    return user;
  },
};

export default usersService;
```

Tähelepanu peab pöörama sellele, et kuna peame ootama ära hashimise teenusest tagastatava tulemuse, siis peame ka siin kasutama async/await'i Samuti peab lisama async/await ka users/controllerisse, mis eelneva teenuse välja kutsub:

```
...
createUser: async (req: Request, res: Response) => {
  const {
    firstName, lastName, password, email,
  } = req.body;
  if (!firstName) {
    return res.status(responseCodes.badRequest).json({
      error: 'First name is required',
    });
  }
  if (!lastName) {
    return res.status(responseCodes.badRequest).json({
      error: 'Last name is required',
    });
  }
  if (!email) {
    return res.status(responseCodes.badRequest).json({
      error: 'E-mail is required',
    });
  }
  if (!password) {
    return res.status(responseCodes.badRequest).json({
      error: 'Password is required',
    });
  }
  const newUser: NewUser = {
    firstName,
    lastName,
    email,
    password,
  };
  const id = await usersService.createUser(newUser);
  return res.status(responseCodes.created).json({
    id,
  });
},
...
```
