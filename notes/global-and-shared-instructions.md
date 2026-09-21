# Globální a sdílené pokyny pro AI nástroje

Vytvořeno: 2026-09-21
Aktualizováno: 2026-09-21
Zdroje ověřeny: 2026-09-21

## Potřeba

Jakub chce definovat opakující se pravidla jednou a používat je ve všech projektech, například při práci v C#. Preferuje společný obsah přímo v tomto repozitáři a odkazy z instrukčních souborů ostatních projektů. Konkrétní opakované chyby zatím nevyjmenoval.

## Globální pokyny v Codexu

Codex načítá osobní pokyny z `~/.codex/AGENTS.md`, případně z `CODEX_HOME`, je-li nastaveno. Globální `AGENTS.override.md` má přednost před globálním `AGENTS.md`. Projektové a bližší adresářové pokyny doplňují globální pravidla a při konfliktu je mohou přebít. Soubor nad kořenem Git repozitáře není náhradou globální konfigurace. Nové nastavení ověřit v nové relaci.

Lokální kontrola: proměnná `CODEX_HOME` v použitém prostředí nebyla nastavena. V `C:/Users/JakubDusek/.codex/` nebyl nalezen ani `AGENTS.md`, ani `AGENTS.override.md`. Globální konfigurace v této konverzaci nebyla vytvořena.

Jde o nastavení příslušného uživatelského prostředí; přenos na jiný počítač či vzdálené prostředí je potřeba zajistit samostatně. [OpenAI: AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)

## Doporučené rozdělení

Návrh asistenta:

| Vrstva | Obsah |
| --- | --- |
| Osobní globální pokyny | Obecné preference platné napříč projekty |
| Sekce pro C#/.NET | Pravidla s výslovnou podmínkou, že se použijí při práci v C#/.NET |
| Projektové pokyny | Architektura, výjimky a konkrétní příkazy daného projektu |

Společný obsah je nyní vytvořen v [rules/general.md](../rules/general.md) a [rules/csharp.md](../rules/csharp.md), každý s vlastní počáteční verzí 1.0.0. Kořenové AGENTS.md tohoto repozitáře na ně odkazuje. Jeho pravidla pro studijní poznámky a automatický push zůstávají specifická tomuto projektu.

Na žádost Jakuba pravidla vyžadují oznámení skutečně načtených verzí a cest v první odpovědi nové konverzace. Agent načte oba soubory a odliší použití C# pravidel podle typu úkolu. Při změně obsahu se zvyšuje verze daného souboru. Hlášení je potvrzení agenta, nikoli záruka dodržení pravidel. [Návod k zapojení](../rules/README.md) obsahuje blok pro další projekty. Globální konfigurace a jiné repozitáře nebyly upraveny; načítání v nové relaci Codexu ani Claude Code zatím nebylo samostatným během ověřeno.

V AGENTS.md ostatních projektů nebo v osobních globálních pokynech použít výslovný pokyn: „Před zahájením práce přečti a dodržuj společná pravidla v <cesta>. Při práci v C#/.NET přečti také <cesta k pravidlům C#>.“ Samotný Markdown odkaz není automatický import; agent musí cílový soubor skutečně otevřít. Nedostupnost souboru má oznámit. Osobní skill může být vhodný pro delší opakovaný postup.

Absolutní cesta jako `D:/Data/AI/rules/csharp.md` funguje pouze v prostředí, které tuto cestu má a dovoluje čtení. Pro další počítače či cloud zajistit dostupnou kopii repozitáře a odpovídající cestu, případně při přípravě prostředí vložit aktuální pravidla do instrukčního souboru. Odkaz na GitHub sám obsah nenačítá ani nesynchronizuje. Při použití lokální kopie je potřeba zajistit také její aktualizaci.

Příklady formulace, nikoli dosud přijatá uživatelova pravidla: před změnou zjistit cílový framework a používané konvence; neprovádět nesouvisející změny závislostí; konkrétní opakovanou chybu popsat spolu s požadovanou alternativou a způsobem ověření.

## Sdílení s Claude Code a mezi počítači

Claude Code má osobní soubor `~/.claude/CLAUDE.md` a podporuje importy pomocí `@cesta`. Společný obsah proto může žít v samostatném Git repozitáři pravidel. Do globálního souboru Codexu jej lze při instalaci/synchronizaci kopírovat nebo vytvořit vhodný odkaz na soubor; Claude může tentýž obsah importovat ze svého osobního souboru. Je třeba zajistit existenci cest v každém prostředí.

Claude Code nově podporuje přímo projektové AGENTS.md (dle aktuální dokumentace od verze 2.1.277). Výchozí chování je načítat AGENTS.md, pokud v pracovním adresáři nebo nad ním není projektový CLAUDE.md či CLAUDE.local.md. Existují omezení podle prostředí a lze nastavit načítání obou formátů. Podpora AGENTS.md neznamená automatické načtení globálního souboru z adresáře Codexu ani automatický import všech jeho odkazů. Pro podrobnosti a výjimky viz [Claude Code: paměť a instrukční soubory](https://code.claude.com/docs/en/memory).

## Omezení a další krok

Textová pravidla nezaručují bezchybné dodržení. Pro opakované chyby je vhodné podle jejich povahy doplnit automatickou kontrolu. Nejprve sepsat konkrétní příklady chyb, potom rozhodnout, co patří mezi globální preference a co do projektu či kontrol.

## Kolik omezení dává smysl

Sdílené pokyny jsou vhodný směr pro zachování zkušeností napříč projekty. Cílem je přesně popsat důležité požadavky a kontext; maximalizovat počet omezení není užitečný cíl. Stručné instrukce založené na skutečných opakovaných problémech doporučují [OpenAI](https://learn.chatgpt.com/guides/best-practices) i [Anthropic](https://code.claude.com/docs/en/best-practices). Dlouhé, rozporné a nerelevantní instrukce mohou snižovat jejich účinnost.

Doporučení asistenta pro Jakubův postup:

- Do společných pravidel ukládat stabilní preference a skutečně opakované chyby, které mají stejný význam ve více projektech.
- Projektová rozhodnutí, například konkrétní architekturu, ponechat v projektu. U pravidel závislých na situaci uvést podmínku a případné výjimky.
- U jednotlivého úkolu definovat očekávané chování, hranice změny a způsob ověření. Volbu běžných implementačních detailů nechat agentovi, pokud pro omezení není konkrétní důvod.
- Rozlišovat instrukci, kontrolu a oprávnění: text říká, co má agent dělat; test či analyzátor ověřuje vybranou vlastnost; oprávnění nástroje skutečně omezují dostupné akce.
- Pravidla přidávat a upravovat podle výsledků. U opakované chyby nejprve určit, zda chybělo zadání, kontext, ověření, nebo zda nástroj ignoroval již existující instrukci.

| Potřeba | Vhodné místo |
| --- | --- |
| Trvalá preference, například jazyk komunikace | Sdílené instrukce |
| Specifické omezení projektu | Projektové instrukce |
| Očekávané chování funkce | Zadání a odpovídající test |
| Mechanicky zjistitelná chyba | Automatická kontrola, případně stručné vysvětlení v instrukcích |
| Opakovaný pracovní postup | Skill nebo skript |

Příklad navrženého pravidla: „Při opravě chyby zachovej veřejné rozhraní, pokud jeho změna není součástí zadání; ověř původně selhávající scénář.“ Naopak požadavek vytvářet rozhraní pro každou třídu bez ohledu na projekt by zbytečně předepisoval implementaci. Jde o ilustrační návrhy, nikoli nově zavedená pravidla.

První praktický krok: zpracovat několik konkrétních chyb z minulých C# úkolů a pro každou vybrat nejmenší účinné opatření. Úspěch posuzovat podle menšího počtu opakovaných chyb a lidských oprav, nikoli podle délky instrukčního souboru.

Související: [Cesta k přesnějším a autonomnějším agentům](path-to-reliable-autonomous-agents.md).
