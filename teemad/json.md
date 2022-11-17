# JSON

JSON - JavaScript Object Notation https://www.json.org/json-en.html

-   Lihtne ja loetav andmevahetusformaat
-   JSON objekt on ümbritsetud loogeliste sulgudega - {"description" : "Ei tahtnud teha"}
-   JSON objekt koosneb key-value (võti-väärtus) paaridest, mis on eraldatud kooloniga - { "key": "value" }
-   Võtmeks saab olla string ja väärtuseks string, arv, objekt, massiiv, tõeväärtus või null
-   Key-value paarid on omavahel eraldatud komaga - {"id": "1", "description", "Ei tahtnud teha"}

```
{
    "excuses": [
        {
            "id": 1,
            "description": "Ei tahtnud teha"
        },
        {
            "id": 2,
            "description": "Ei osanud"
        },
        {
            "id": 3,
            "description": "Ei viitsinud"
        },
        {
            "id": 4,
            "description": "Ei teadnud, et oleks vaja midagi teha"
        }
    ]
}
```
