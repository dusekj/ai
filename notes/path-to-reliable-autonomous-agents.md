# Cesta k přesnějším a autonomnějším AI agentům

Vytvořeno: 2026-09-18
Aktualizováno: 2026-09-25
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

## Kdy agenta použít a proč výsledky stále opravuji

Jakub uvedl (2026-09-23), že zatím vždy musel výsledek agenta opravit, ručně nebo dalším příkazem, a plně autonomní vývoj kódu mu proto zatím připadá nereálný.

### Realistický cíl

Cílem není nulový počet oprav, ale **levnější opravy**: méně, dříve odhalené a menší. I pull request zkušeného vývojáře obvykle dostane připomínky. Rozumná meta je „agent samostatně dokončí vymezený úkol, člověk dělá review“, ne „agent vyvíjí bez dohledu“. (Interpretace asistenta.)

### Vhodné a nevhodné scénáře

Agent se hodí, když je úkol **dobře zadaný, ověřitelný, lokální a s nízkým rizikem**:

- oprava chyby, kterou lze reprodukovat testem,
- doplnění testů k existujícímu kódu,
- mechanické změny: přejmenování, migrace API nebo verze knihovny, sjednocení podle pravidel,
- nový kód podle existujícího vzoru v projektu (další endpoint, další entita),
- průzkum a vysvětlení neznámého kódu, hledání příčiny chyby, analýza logů,
- jednorázové skripty a nástroje, dokumentace,
- druhý pohled při code review.

Méně vhodné: nejasné zadání, architektonická rozhodnutí, úkoly závislé na nezapsaných znalostech (doménová pravidla, zvyklosti týmu), vizuální vkus v UI, rozsáhlé průřezové změny a bezpečnostně kritický kód bez důkladného review.

### Diagnostika oprav

U každé opravy je užitečné zapsat, **proč** byla nutná. Každá příčina má jiný lék:

| Příčina opravy | Lék |
| --- | --- |
| Agent špatně pochopil zadání | Nejprve plán (plan mode) a jeho schválení před psaním kódu; nechat agenta klást otázky; konkrétní příklady a kritéria přijetí |
| Porušil konvence a styl | Převést opakovanou připomínku do pravidla (`rules/`, AGENTS.md), případně do analyzátoru nebo hooku |
| Funkční chyba | Dát agentovi možnost ověření: test reprodukující chybu, build, spuštění aplikace; hotovo až po úspěšné kontrole |
| Nedotažená práce | Explicitní definice hotového stavu (definition of done) a kontrolní seznam |
| Chybějící kontext | Odkázat na vzorový soubor, dokumentaci a doménová pravidla |
| Příliš velký úkol | Rozdělit na menší kroky s mezikontrolou |

Zásada: **každá opakovaná ruční oprava by se měla změnit v pravidlo, test nebo hook**, aby se neopakovala. Tímto způsobem vznikla i sdílená C# pravidla v [globálních pokynech](global-and-shared-instructions.md).

### Pomůže více agentů, kteří si připomínkují práci?

Částečně. Oddělený reviewer s čerstvým kontextem a konkrétním zadáním (hledej chyby vůči zadání, kontroluj pravidla) odhalí část chyb, které autor přehlédl. Ale:

- Agenti na stejném modelu mívají **stejná slepá místa**. Chybí-li nebo je-li nejasná specifikace, shodnou se na stejné chybné interpretaci; review nenahradí chybějící zadání.
- Volná „diskuse“ agentů zvyšuje cenu a šum a nemá deterministický výsledek.
- Deterministická kontrola (test, build, analyzátor) je spolehlivější zpětná vazba než názor dalšího modelu.

Doporučené pořadí: jeden agent + plán + automatické ověření → potom jeden krok review (např. `/code-review` v Claude Code) → více agentů až pro skutečně paralelní nezávislé úlohy nebo široký průzkum. Odpovídá to krokům 7 a 9 v tabulce výše. Související: [Harness u AI agentů](agent-harness.md).

### Lokální paralelní běh vs. agent na pozadí

Jakubův současný postup (2026-09-23): zadání ladí v několika iteracích, pak úkol předá agentovi, ten běží přibližně 30–40 minut a Jakub mezitím pracuje na jiném úkolu, případně souběžně s dalšími lokálními relacemi Claude Code. Poté výsledek kontroluje.

Závěr diskuse: tento postup už je **ohraničená autonomie** (krok 8 v tabulce výše). Přesun agenta na server nebo do cloudu sám o sobě kvalitu ani rychlost nezvýší. Úzkým hrdlem je čas na kontrolu výsledků, ne místo, kde agent běží.

Agent běžící mimo vlastní počítač dává smysl hlavně tehdy, když:

- práci spouští **událost bez účasti člověka**: plán (cron), pád CI, nové issue, komentář v review,
- má běžet, i když je počítač vypnutý nebo uživatel pryč,
- je potřeba **izolace**: sandbox, kde lze agentovi povolit víc bez rizika pro vlastní stroj,
- lokální výkon nebo počet souběžných úloh nestačí,
- s agentem pracuje celý tým (sdílené spouštění a výsledky).

U lokálního paralelního běhu se vyplatí:

- oddělit souběžné úlohy do **git worktree** nebo samostatných větví, aby si agenti nepřepisovali soubory,
- chtít na konci **podklady pro rychlou kontrolu**: co se změnilo a proč, které kontroly proběhly s jakým výsledkem, rizika a otevřené otázky,
- nechat agenta před předáním udělat vlastní review (např. `/code-review`),
- nastavit notifikaci o dokončení,
- počet paralelních úloh přizpůsobit tomu, kolik jich člověk stihne kvalitně zkontrolovat.

### Další směr s největším přínosem (vyhodnocení 2026-09-25)

Otázka: který směr rozvoje přinese teď nejvíc? Stav podle poznámek: pravidla a teorie jsou zpracované do šířky (sdílená pravidla, harness, hooky, grafy a workflow), ale zatím nejsou zaznamenaná **data z reálné práce** a žádná kontrola není vynucená strojově. Úzkým hrdlem je podle Jakubova popisu čas na kontrolu a opravy výsledků.

Doporučení asistenta (interpretace, ne měřený výsledek): na 2–3 týdny přestat přidávat nová témata a zaměřit se na **zpětnovazební smyčku na jednom reálném C# projektu**.

1. **Deník oprav.** U každého úkolu předaného agentovi zapsat 1 řádek: úkol, zda byla nutná oprava, příčina podle tabulky *Diagnostika oprav*, čas kontroly. Po ~10 úkolech je vidět, která příčina dominuje, a tedy kam investovat. Bez toho je výběr dalšího kroku odhad.
2. **Mechanická pravidla převést z textu do buildu.** Co jde zkontrolovat nástrojem, nemá hlídat model ani reviewer. Build chybu ukáže agentovi hned, funguje stejně v Claude Code i Codexu i v CI a zmenšuje objem review:
   - PascalCase privátních metod: pravidla pojmenování v `.editorconfig` (diagnostika IDE1006) se závažností `error` a `<EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>` v projektu.
   - XML dokumentace: `GenerateDocumentationFile` s varováním CS1591 pokrývá jen veřejně viditelné členy. Pro privátní metody a vlastnosti je potřeba analyzátor, např. StyleCop.Analyzers (SA1600) s volbou dokumentace privátních prvků ve `stylecop.json`.
   - **Neověřeno v této konverzaci** — přesné názvy voleb a chování ověřit v aktuální dokumentaci a na konkrétním projektu.
3. **Definice hotového stavu vynucená Stop hookem:** build a testy musí projít, agent předá krátký report pro kontrolu (co a proč změnil, jaké kontroly proběhly, rizika). Navazuje na náčrt v [Harness u AI agentů](agent-harness.md).
4. **Až podle deníku** rozhodnout o dalším: dominuje-li nepochopení zadání → plan mode a šablona zadání (loop specification); funkční chyby → testy reprodukující chybu; opakovaný proces → skill nebo uložené workflow.

Proč ne jiné směry teď: více agentů, grafy a běh na pozadí zvyšují objem výstupu, který je potřeba kontrolovat — tedy zatěžují právě úzké hrdlo. Dávají smysl, až bude kontrola levnější. RAG, fine-tuning nebo vlastní orchestrace přes SDK by měly vycházet z konkrétního nedostatku v deníku.

Upřesnění od Jakuba (2026-09-25): postup už používá u 2–3 zákazníků a problémy zapisuje průběžně. Deník tedy existuje, jen není v tomto repozitáři. Přehled dalších technologií podle řešeného problému je v [Technologie pro agentní vývoj v .NET](agent-tooling-for-dotnet.md).

Alternativa, pokud je cílem hlubší porozumění AI, ne produktivita ve vývoji: postavit malou vlastní aplikaci nad API modelu (tool use, agent loop, jednoduchý eval). Tuto preferenci Jakub zatím neupřesnil.

## Doporučený první experiment

Na jednom existujícím vývojovém projektu zkusit kroky 1–3: instrukce projektu, konkrétní kritéria přijetí jedné malé změny a funkční kontrolu výsledku. Zaznamenat, kolikrát bylo nutné zasáhnout, z jaké příčiny podle tabulky výše a jaké chyby unikly kontrolám. Teprve opakující se úspěšný postup převést do skillu a automatizovat.

Vlastní vektorovou databázi/RAG, fine-tuning ani složitý multiagentní framework zatím nedoporučuji jako první investici. Jde o doporučení pro popsanou situaci, ne obecné odmítnutí těchto technik. Jejich potřeba by měla vycházet z konkrétního nedostatku a měření.

## Otevřené otázky

- Jaký typ aplikací a technologický stack Jakub nejčastěji vyvíjí?
- Kde nyní vzniká nejvíce oprav: nepochopení zadání, chybný kód, chybějící kontext, nebo nedotažené ověření?
- Jaké testy a automatické kontroly už jeho vývojové projekty mají?
- Jak dlouhé samostatné úkoly chce zadávat a podle čeho bude posuzovat úspěch?
