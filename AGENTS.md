# Pokyny pro práci v projektu AI

## Účel projektu

Tento repozitář slouží Jakubovi ke studiu umělé inteligence a průběžného vývoje v oboru. Konverzace mohou pokrývat teorii, modely, nástroje, programování, praktické experimenty i širší souvislosti AI. Výsledkem má být postupně rostoucí, přehledná znalostní báze v Markdownu verzovaná v Gitu.

## Trvalá dohoda s uživatelem

- Komunikuj a piš poznámky česky, s diakritikou. Běžné anglické odborné termíny zachovej a podle potřeby vysvětli.
- Automaticky ukládej podstatné poznatky ze studijních konverzací do souborů `.md`. Uživatel nemusí o zápis zvlášť žádat.
- Před závěrečnou odpovědí vytvoř nebo aktualizuj příslušné poznámky a automaticky commitni změny z daného úkolu. Toto je výslovné trvalé svolení ke commitování; nevyžaduj opakované potvrzení.
- Čistě organizační zprávy či potvrzení nevyžadují studijní poznámku. Pokud nevznikla žádná změna, nevytvářej prázdný commit.
- Novější výslovné pokyny uživatele mají přednost před těmito pravidly.

## Organizace poznámek

- `README.md`: základní popis projektu a odkaz na přehled poznámek.
- `poznamky/README.md`: průběžně aktualizovaný tematický rozcestník.
- `poznamky/<tema>.md`: poznámky k jednotlivým tématům; podadresáře zaváděj až podle potřeby.
- Názvy souborů piš malými písmeny bez diakritiky a s pomlčkami. Soubory ukládej v UTF-8.
- Před zápisem prohlédni rozcestník a relevantní existující poznámky. Navazuj na ně a omez duplicity; související témata propojuj relativními odkazy.

## Podoba studijní poznámky

Použij následující strukturu přiměřeně tématu; prázdné a nerelevantní oddíly vynech:

1. Název tématu.
2. Datum vytvoření a poslední aktualizace ve formátu `YYYY-MM-DD`.
3. Stručné shrnutí hlavních poznatků.
4. Klíčové pojmy a srozumitelné vysvětlení principů.
5. Praktické příklady, použití nebo experimenty, pokud byly probírány.
6. Omezení, nejistoty a otevřené otázky.
7. Zdroje a související poznámky.

Zapisuj užitečnou syntézu, ne doslovný přepis chatu. Zachovej důležité otázky, vysvětlení a závěry. Rozlišuj ověřená fakta, hypotézy a vlastní interpretace. Nevymýšlej zdroje ani výsledky experimentů. U rychle se měnících informací ověřuj aktuálnost a uváděj datum ověření a konkrétní odkazy, přednostně na primární zdroje. Pokud něco zůstává neověřené, označ to. Neuváděj, že uživatel něčemu rozumí nebo něco vyzkoušel, pokud to nepotvrdil.

## Git a dokončení práce

- Na začátku zkontroluj stav repozitáře. Zachovej existující uživatelské změny a nezahrnuj nesouvisející úpravy do svého commitu.
- Před commitem zkontroluj obsah změn, Markdown, relativní odkazy a případné nechtěně zahrnuté citlivé údaje. Přidávej do commitu konkrétní soubory nebo změny související s úkolem.
- Vytvářej stručné, výstižné commit zprávy, například `docs: doplnit poznámky k transformerům`.
- Automatické commitování znamená lokální commit. Push do vzdáleného repozitáře prováděj pouze na pokyn uživatele.
- Nepřepisuj historii ani nemaž nesouvisející práci. Neukládej přístupové údaje a tajné klíče.
- Pokud commit selže, pokus se odstranit běžnou příčinu bez destruktivních zásahů. Nevymýšlej identitu autora; chybějící konfiguraci pravdivě oznam a poznámky ponech uložené.
- V závěrečné odpovědi stručně uveď, kam byly poznámky uloženy a hash vytvořeného commitu, případně důvod, proč commit nevznikl.
