# API

API - Application Programming Interface

Selle kursuse kontekstis on mõeldud üle http töötavat API-t.

Liides, mille kaudu süsteemid saavad omavahel infot jagada
Backend - frontend
Backend - backend
Andmed, mida vahetatakse on sageli JSON kujul
Endpoint - aadress, millele pöördudes saab API-le päringuid teha.
Näiteks Githubi API endpoindid: https://docs.github.com/en/rest/overview/endpoints-available-for-github-apps

Põhimõtteliselt on tegemist päringu saatmisega mingile aadressile, kust saab vastuseks mingi info. Näiteks https://dog-facts-api.herokuapp.com/api/v1/resources/dogs/all aadressile pöördumisel saab vastuseks nimekirja suvalistest faktidest koerte kohta.
Enamasti saab API poole pöördudes saata API-le ka täiendavat informatsiooni, mille abil on võimalik piirata vastusena saadavate tulemuste arvu (näiteks otsing, sorteerimine jne). Eelnevalt näidisena toodud API-le on võimalik lisada nn query parameetrina number, mis piirab tagastatavate vastuste arvu: https://dog-facts-api.herokuapp.com/api/v1/resources/dogs?number=2

Valikut tasuta kasutatavatest API-dest näeb näiteks siit: https://github.com/public-apis/public-apis
