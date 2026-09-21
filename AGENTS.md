# Pokyny pro práci v projektu AI

## Sdílená pravidla

Před první odpovědí načti a dodržuj pravidla ze souborů (cesty jsou relativní ke kořeni tohoto repozitáře, tedy k adresáři s tímto `AGENTS.md`):

- `./rules/general.md` — obecná pravidla
- `./rules/csharp.md` — pravidla C#/.NET

V první odpovědi oznam skutečné verze a zdrojové cesty podle postupu v `./rules/general.md`. C# pravidla používej pouze při práci v C#/.NET; jinak je označ jako načtená, ale nepoužitelná pro aktuální úkol. Pokud soubor nelze načíst, oznam to a nevymýšlej jeho verzi.

## Účel projektu

Tento repozitář slouží Jakubovi ke studiu umělé inteligence a průběžného vývoje v oboru. Konverzace mohou pokrývat teorii, modely, nástroje, programování, praktické experimenty i širší souvislosti AI. Výsledkem má být postupně rostoucí, přehledná znalostní báze v Markdownu verzovaná v Gitu.

## Trvalá dohoda s uživatelem

- Komunikuj a piš poznámky česky, s diakritikou. Běžné anglické odborné termíny zachovej a podle potřeby vysvětli.
- Automaticky ukládej podstatné poznatky ze studijních konverzací do souborů `.md`. Uživatel nemusí o zápis zvlášť žádat.
- Před závěrečnou odpovědí vytvoř nebo aktualizuj příslušné poznámky, automaticky commitni změny z daného úkolu a ihned po každém commitu proveď push. Toto je výslovné trvalé svolení ke commitování i pushování; nevyžaduj opakované potvrzení.
- Čistě organizační zprávy či potvrzení nevyžadují studijní poznámku. Pokud nevznikla žádná změna, nevytvářej prázdný commit.
- Novější výslovné pokyny uživatele mají přednost před těmito pravidly.

## Organizace poznámek

- `README.md`: základní popis projektu a odkaz na přehled poznámek.
- `notes/README.md`: průběžně aktualizovaný tematický rozcestník.
- `notes/<topic>.md`: poznámky k jednotlivým tématům; podadresáře zaváděj až podle potřeby.
- Názvy všech adresářů a souborů piš vždy anglicky. Používej malá písmena bez diakritiky a pomlčky; standardní názvy jako `AGENTS.md` a `README.md` zachovej. Komunikace i obsah poznámek zůstávají v češtině. Soubory ukládej v UTF-8.
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
- Po každém commitu automaticky proveď push aktuální větve do jejího nakonfigurovaného vzdáleného repozitáře. Pokud chybí upstream a cíl je jednoznačný, nastav jej při pushi.
- Nepřepisuj historii ani nemaž nesouvisející práci. Neukládej přístupové údaje a tajné klíče.
- Pokud commit selže, pokus se odstranit běžnou příčinu bez destruktivních zásahů. Nevymýšlej identitu autora; chybějící konfiguraci pravdivě oznam a poznámky ponech uložené.
- Pokud push selže, pokus se odstranit běžnou příčinu bez přepisování historie a bez force push. Nevyřešenou překážku pravdivě oznam; lokální commit zachovej.
- V závěrečné odpovědi stručně uveď, kam byly poznámky uloženy, hash vytvořeného commitu a výsledek pushe, případně důvod, proč commit nebo push neproběhl.
