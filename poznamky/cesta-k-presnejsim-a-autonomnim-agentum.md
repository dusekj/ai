# Cesta k přesnějším a autonomnějším AI agentům

Vytvořeno: 2026-09-18
Aktualizováno: 2026-09-18
Aktuálnost zdrojů ověřena: 2026-09-18

## Výchozí stav

Jakub dlouhodobě používá ChatGPT a Claude na dotazy a Claude Code nebo Codex na vývoj. Nástroj připojí ke Git repozitáři a snaží se co nejpřesněji formulovat prompt. Další rozšíření podle svého popisu zatím nepoužívá. V tomto studijním repozitáři jsme již zavedli AGENTS.md a automatické ukládání poznámek, commit a push.

## Hlavní závěr

Doporučený další směr je budovat prostředí, v němž má agent správný kontext, dostupné nástroje a ověřitelná kritéria dokončení. Přesnost a autonomie jsou samostatné vlastnosti: více samostatnosti ani více agentů samo o sobě nezaručuje správný výsledek.

Následující pořadí je doporučení asistenta pro tento výchozí stav, nikoli univerzální žebříček doložený benchmarkem. Jednotlivé kroky na sebe navazují; pozdější krok nemusí mít větší přínos než předchozí.

## Doporučené pořadí

| Krok | Směr | Praktický první krok | Hlavní přínos |
| --- | --- | --- | --- |
| 1 | Kontext projektu | Stručné AGENTS.md pro Codex a CLAUDE.md pro Claude Code: účel, omezení, příkazy, důležitá rozhodnutí | Méně opakovaného vysvětlování a chybných předpokladů |
| 2 | Specifikace a kritéria přijetí | U jedné funkce popsat očekávané chování, konkrétní příklady a hranice změny | Správně zvolený cíl |
| 3 | Ověřování výsledků | Zprovoznit testy, build a podle aplikace kontrolu v prohlížeči; agent je spouští a opravuje zjištěné chyby | Zpětná vazba pro samostatné opravy |
| 4 | Skills | Zabalit opakovaný postup do znovupoužitelného návodu, případně se skripty | Konzistentnější provádění známých úloh |
| 5 | Nástroje a zdroje přes CLI/API/MCP | Zpřístupnit konkrétní chybějící zdroj, např. issue tracker, logy nebo testovací databázi | Agent si dohledá potřebné informace a provede práci |
| 6 | Hooks a CI | Vynucovat vybrané kontroly programově a před sloučením vyžadovat jejich úspěch | Menší závislost na dodržení textového pokynu |
| 7 | Oddělené review a měření | Druhý průchod hledá chyby podle zadání; na sadě úloh sledovat úspěšnost a lidské zásahy | Odhalení chyb a měřitelný výběr postupu |
| 8 | Omezené autonomní workflow | Zadání → větev → implementace → kontroly → opravy → pull request | Dokončení vymezeného úkolu s menším dohledem |
| 9 | Více agentů a vlastní orchestrace | Oddělit skutečně nezávislé úlohy; vlastní řízení přes API/SDK až při konkrétní potřebě | Vyšší kapacita a specializace za cenu složitější koordinace |

### Kontext, specifikace a ověřování

Trvalé instrukce mají být stručné. Rozsáhlejší znalosti patří do tematické dokumentace, kterou agent načte podle potřeby. Nesouvisející úkoly je vhodné oddělovat do samostatných konverzací. Pro oba nástroje udržovat společná pravidla bez protichůdných kopií; načítání instrukcí každým nástrojem ověřit.

Příklad kritéria přijetí: export CSV obsahuje pouze vyfiltrované záznamy, správně zachází s diakritikou a funguje i pro prázdný výsledek. Takové zadání lze převést na kontroly. Samotný úspěšný build neprokazuje správnou funkčnost a testy vytvořené agentem mohou opakovat jeho chybný předpoklad. Očekávané chování proto odvozovat ze zadání a reálných scénářů. [Claude Code: doporučené postupy](https://code.claude.com/docs/en/best-practices)

### Rozdíly mezi rozšířeními

- **Skill:** znovupoužitelný postup a související znalosti, například diagnostika chyby nebo příprava vydání.
- **MCP:** rozhraní pro připojení nástrojů a zdrojů dat. Samo o sobě nezvyšuje schopnosti uvažování modelu. Existující CLI nebo API může stejnou potřebu pokrýt jednodušeji.
- **Hook:** program spouštěný při konkrétní události. Přesouvá vybrané povinnosti z textové instrukce do vykonávaného kódu. Jeho účinnost závisí na konfiguraci a možnostech konkrétního nástroje.
- **CI:** automatické kontroly repozitáře, například po pushi; pro blokování sloučení musí být odpovídajícím způsobem nastaveny povinné kontroly.
- **Eval:** opakovatelné hodnocení agenta na reprezentativních úlohách, včetně kvality výsledku, ceny, času a počtu lidských zásahů.

Praktický rozdíl pro tento projekt: pravidlo „vždy commit a push“ v AGENTS.md je instrukce agentovi. Pokud je potřeba strojově ověřené dokončení, lze později doplnit kontrolu stavu Gitu a synchronizace se vzdálenou větví. [Hooks](https://code.claude.com/docs/en/hooks-guide), [evaluace skills](https://developers.openai.com/de-DE/blog/eval-skills).

### Autonomie a více agentů

Pro první samostatný běh doporučuji malou opravu s reprodukovatelnou chybou a jasným výsledkem. Vymezit rozsah, oprávnění, rozpočet nebo časový limit a situace, kdy má agent předat problém člověku. Výsledek má obsahovat důkazy o provedených kontrolách.

Oddělený reviewer může mít čerstvý kontext, ale stále může sdílet stejné omyly jako autor. Více agentů je vhodné tam, kde jde práci rozdělit a výsledky spolehlivě ověřit. Zvyšuje nároky na koordinaci i cenu. Začínat jednoduchým workflow a přidávat složitost až podle pozorovaných nedostatků odpovídá doporučení Anthropic. [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)

## Doporučený první experiment

Na jednom existujícím vývojovém projektu zkusit kroky 1–3: instrukce projektu, konkrétní kritéria přijetí jedné malé změny a funkční kontrolu výsledku. Zaznamenat, kolikrát bylo nutné zasáhnout a jaké chyby unikly kontrolám. Teprve opakující se úspěšný postup převést do skillu a automatizovat.

Vlastní vektorovou databázi/RAG, fine-tuning ani složitý multiagentní framework zatím nedoporučuji jako první investici. Jde o doporučení pro popsanou situaci, ne obecné odmítnutí těchto technik. Jejich potřeba by měla vycházet z konkrétního nedostatku a měření.

## Otevřené otázky

- Jaký typ aplikací a technologický stack Jakub nejčastěji vyvíjí?
- Kde nyní vzniká nejvíce oprav: nepochopení zadání, chybný kód, chybějící kontext, nebo nedotažené ověření?
- Jaké testy a automatické kontroly už jeho vývojové projekty mají?
- Jak dlouhé samostatné úkoly chce zadávat a podle čeho bude posuzovat úspěch?
