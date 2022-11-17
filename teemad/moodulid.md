# Moodulid

Moodulite abil on võimalik jagada kood eraldi failidesse ja vajaduse korral neid importida.

Mooduli tegemiseks tuleb selle sisu eksportida, näiteks:

```
/**
* Http response codes
*/
export default {
  ok: 200,
  created: 201,
  noContent: 204,
  badRequest: 400,
  notFound: 404,
  serverError: 500,
};
```

või

```
const port: number = 3000;
const databaseName: string = 'excuses';
const environment: string = 'development';

export { port, databaseName, environment };
```

Teises failis saab seda faili nüüd kasutada nii:

```

...
import responseCodes from './general/responseCodes';
import { databaseName } from './general/config';

...
console.log(`Connecting to database ${databaseName}`);
...
res.status(responseCodes.noContent).json({});
```

Samast kataloogist importides pannakse faili nime ette './', näiteks: import responseCodes = from './responseCodes';

Kataloogist, mis on samm ülevalpool pannakse nime ette '../', näiteks import responseCodes = from '../responseCodes';
