# NodeJS

NodeJS https://nodejs.org/en/

Serveris töötav javascript
Tasuta
Lihtne alustada
Asünkroonne ja sündmustepõhine
Ühelõimeline, kuid hästi skaleeruv
https://www.tutorialspoint.com/nodejs/nodejs_introduction.htm

Lihtne nodejs typescript API näidis:

```
import http from 'http';

const hostname: string = '127.0.0.1';
const port: number = 3000;

const server: http.Server = http.createServer(
(req: http.IncomingMessage, res: http.ServerResponse) => {
res.statusCode = 200;
res.setHeader('Content-type', 'text/plain');
res.end('Hello world!');
},
);

server.listen(port, hostname, () => {
console.log(`Server running at http://${hostname}:${port}`);
});
```
