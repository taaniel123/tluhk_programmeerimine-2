# Github

Üheks kriteeriumiks hindamisel on koodihalduskeskkonna kasutamine. Vaikimisi on koodihalduskeskkonnaks Github, kuid soovi korral võib kasutada ka mõnda muud keskkonda.
Oma projekti kood tuleks kohe algusest peale laadida üles koodihalduskeskkonda ja seda õppejõuga jagada.

## Alustamine

Selleks, et alustada projekti nii, et see oleks kohe alguses Githubis, siis soovitan teha nii:

1. Logi Githubi sisse
2. Vajuta Repositories nuppu
3. Vajuta rohelist New nuppu
4. Pane repositooriumile nimi (Repository name)
5. Vali Private või Public
6. Pane linnuke Add a README file ette
7. Pane linnuke Add .gitignore ette
8. Vali .gitignore template: Node
9. Vajuta Create Repository nuppu
10. Avanenud lehel vajuta rohelist Code nuppu ja kopeeri sealt repositooriumi link (näit: https://github.com/mrttlu/test.git)
11. Mine oma arvutis terminalis (command promptis) kausta, kuhu soovid projekti kausta tekitada
12. Kirjuta: git clone repositooriumi link (git clone https://github.com/mrttlu/test.git) ja vajuta enter klahvi
13. Võid loodud kausta hakata koodi kirjutama
14. Kirjutatud koodi githubi laadimine:

-   terminalis (command promptis) kirjuta:
-   git add .
-   git commit -m 'Kirjeldus selle kohta, mida vahepeal teinud oled vms'
-   git push
-   .gitignore fail - fail, milles kirjeldatakse failid ja kaustad, mida ei soovita üles laadida koodihalduskeskkondaNode projektide puhul ei ole vaja üles laadida node_modules kausta, kuna see on tihti üsna suuremahuline ja seda on lihtne uuesti taasluua (npm install)

**Githubi poolt loodud node .gitignore faili näidis:** https://github.com/mrttlu/esimene/blob/main/.gitignore

**Career Karma .gitignore Files: A Guide for Beginners:** https://careerkarma.com/blog/gitignore/

## Kuidas oma olemasolev projekt git-i üles laadida

https://docs.github.com/en/free-pro-team@latest/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line

## Git cheat sheet

https://education.github.com/git-cheat-sheet-education.pdf

## Traversy Media 33 minutit pikk video sellest, mis on git ja kuidas sellega alustada jms.

[![Watch the video](https://img.youtube.com/vi/SWYqp7iY_Tc/maxresdefault.jpg)](https://www.youtube.com/watch?v=SWYqp7iY_Tc&t=1s)

## Lisaks

-   Teiste ja oma projekti saab oma arvutisse kloonida käsuga: git clone repositooriumi link (näiteks git clone https://github.com/mrttlu/esimene.git)
-   Giti repositooriumi lingi saab rohelise Code nupu alt:
    ![giti repositooriumi link](/pildid/gitAadress.png)
-   Peale kloonimist pead kindlasti minema kloonitud repositooriumi kausta (näiteks cd esimene)
-   Kui oled repo alla klooninud, siis enne kui selle käivitada saad, tuleb sõltuvused alla laadida käsuga npm install (seda tuleb teha selles kaustas, kus on fail package.json)
