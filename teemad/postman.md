# Postman

API arendamisel on kindlasti vaja testida, kas kõik töötab nii, nagu plaanitud. Selleks soovime saata API-le erinevaid päringuid (GET, POST, DELETE jne). Kui GET- päringut saame saata lihtsalt kasvõi veebilehitseja aadressiribale API ja endpoindi aadressi kirjutades, siis muude päringutega nii lihtne ei ole. Seetõttu ei ole mõistlik API testimiseks ainult veebilehitsejat kasutada (välja arvatud vast juhul, kui frontend juba olemas on). Selleks ongi loodud erinevad tööriistad, millega on võimalik lihtslat API-le erinevaid päringuid saata, nagu näiteks Postman, Curl jms. Selle kursuse raames on näidetes kasutatud Postmani, mis on graafilise kasutajaliidesega populaarne API testimise platvorm.

Postman https://www.postman.com/

![postman](/pildid/postman.png)

Märkused:

-   Postmanist body saatmisel (POST, PUT, DELETE päringute tegemisel) märgi 'raw' ja siis 'JSON'
-   Postmanist JSON-i saatmisel peavad olema nii key-d, kui valued olema topeltjutumärkide vahel: { "fistName": "Juku" }
