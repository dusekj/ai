# Technologie pro rozšíření agentního vývoje v .NET

Vytvořeno: 2026-09-25
Aktualizováno: 2026-09-25
Zdroje ověřeny: 2026-09-25 (jen položky s odkazem, viz níže)

## Výchozí stav

Jakub uvedl (2026-09-25), že současný postup (sdílená pravidla, Claude Code a Codex nad Git repozitářem) už používá u 2–3 zákazníků a problémy s výstupem agenta zapisuje průběžně. Hlavní vývojový stack je C#/.NET. Chce přehled dalších technologií, které může nasadit.

## Shrnutí

- Technologie je nejlepší vybírat podle **příčiny oprav**, kterou ukazuje deník problémů. Každá skupina níže řeší jinou příčinu (tabulka *Diagnostika oprav* v [cestě k přesnějším agentům](path-to-reliable-autonomous-agents.md)).
- U více zákazníků je největší páka **sdílení**: jeden balíček pravidel a analyzátorů a jeden plugin se skills a hooky pro všechny projekty místo kopírování.
- Doporučené pořadí (návrh asistenta): 1) analyzátory ve sdíleném NuGet balíčku, 2) C# LSP plugin, 3) silnější testy (Testcontainers, Verify, Playwright), 4) review v CI, 5) MCP pro dokumentaci a issue tracker, 6) vlastní plugin, 7) měření nákladů.

## Přehled podle řešeného problému

### 1. Porušení konvencí → deterministické kontroly v buildu

| Technologie | Co přináší |
| --- | --- |
| `.editorconfig` + `EnforceCodeStyleInBuild` | Pojmenování a styl jako chyba buildu (IDE1006 apod.) |
| StyleCop.Analyzers | Dokumentace (SA1600) včetně privátních prvků podle `stylecop.json`, pořadí a formát |
| Meziantou.Analyzer, SonarAnalyzer.CSharp, Roslynator | Běžné chyby, výkon, async, bezpečnost |
| `Directory.Build.props` | Jednotné nastavení analyzátorů pro všechny projekty v repozitáři |
| **Vlastní NuGet balíček** (např. `Corexpert.CodeStyle`) | `.editorconfig`, analyzátory a jejich závažnosti na jednom místě pro všechny zákazníky; aktualizace změnou verze balíčku |
| `dotnet format` v hooku `PostToolUse` | Formát opraví automaticky, agent ho nemusí hlídat |

Textová pravidla v `rules/csharp.md` pak vysvětlují záměr a build slouží jako důkaz. Názvy a volby analyzátorů v této tabulce nebyly v této konverzaci ověřeny v dokumentaci. Než je zavedeš, ověř je na konkrétním projektu.

### 2. Chybějící kontext a chybná navigace v kódu → code intelligence a dokumentace

- **C# LSP plugin pro Claude Code** (oficiální `csharp-lsp`, využívá language server `csharp-ls`, instalace `dotnet tool install --global csharp-ls`). Agent místo textového hledání používá přechod na definici, hledání referencí a diagnostiku. Velký přínos ve velkých solution. [Plugin](https://claude.com/plugins/csharp-lsp), [zdroj](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/csharp-lsp) (ověřeno 2026-09-25)
- **Microsoft Learn MCP Server**: oficiální, bezplatný, bez přihlášení. Agent si dohledá aktuální dokumentaci .NET/Azure a ukázky kódu místo spoléhání na znalosti z tréninku. [Přehled](https://learn.microsoft.com/en-us/training/support/mcp) (ověřeno 2026-09-25)
- **MCP nebo CLI k issue trackeru** zákazníka (GitHub `gh`, Azure DevOps, Jira): agent čte zadání a akceptační kritéria přímo z úkolu. Nejdřív zvaž existující CLI, bývá jednodušší než MCP.
- **Read-only přístup k testovací databázi** (MCP nebo `sqlcmd`/skript): schéma a data bez rizika zápisu. Nikdy ne k produkčním datům zákazníka.

### 3. Funkční chyby → silnější automatické ověření

| Technologie | Co přináší |
| --- | --- |
| `WebApplicationFactory` (ASP.NET Core) | Integrační testy API v paměti, agent je spustí sám |
| Testcontainers for .NET | Skutečná databáze nebo fronta v Dockeru pro testy místo mocků |
| Verify (snapshot testing) | Rychlé zachycení změn výstupu (JSON, HTML, SQL); změny agent předloží jako diff |
| Playwright for .NET / Playwright MCP | End-to-end testy UI; agent vidí skutečný výsledek v prohlížeči |
| Stryker.NET (mutation testing) | Ověří, zda testy napsané agentem skutečně odhalí chybu. Řeší riziko, že testy jen opakují chybný předpoklad |

### 4. Nedotažená práce a review → automatizace v CI

- **Claude Code GitHub Actions** (`anthropics/claude-code-action`): review každého PR, reakce na `@claude` v issue nebo komentáři. Pro jiné CI lze použít neinteraktivní režim `claude -p`. [Dokumentace](https://code.claude.com/docs/en/github-actions) (ověřeno 2026-09-25)
- **`codex review --base main`** jako druhý, nezávislý reviewer s jiným modelem. Méně sdílených slepých míst než u dvou instancí stejného modelu (viz [Harness u AI agentů](agent-harness.md)).
- Povinné kontroly (build, analyzátory, testy) nastavit jako podmínku sloučení PR.

### 5. Opakované postupy napříč zákazníky → vlastní plugin

Claude Code umí distribuovat skills, hooky, subagenty, MCP konfiguraci i uložená workflow jako **plugin** z vlastního marketplace (Git repozitáře). Může nahradit současné odkazy na `rules/` absolutní cestou a zajistit stejnou verzi na více počítačích. Kandidáti: skill „oprava chyby s reprodukujícím testem“, Stop hook s definicí hotového stavu, reviewer subagent s kontrolou pravidel. Mechanismus pluginů jsem v této konverzaci podrobně neověřoval. Ekvivalent pro Codex je potřeba řešit zvlášť.

### 6. Izolace a bezpečnost u více zákazníků

- **Dev Containers** (`.devcontainer`) nebo sandbox: každý zákazník má oddělené prostředí, agentovi lze povolit víc bez rizika pro vlastní stroj a data jiných zákazníků.
- **Oddělená oprávnění a tajné klíče** pro každého zákazníka (`settings.json` v projektu, `permissions.deny` pro citlivé soubory jako `appsettings.Production.json`).
- **Smluvní stránka:** ověřit, že smlouva se zákazníkem dovoluje zpracování kódu cloudovým AI modelem a za jakých podmínek (retence dat, firemní nebo zero-retention tarif). Nejde o technologii, ale u práce pro zákazníky jde o podmínku použití.

### 7. Měření → rozhodování podle dat

- Claude Code podporuje export metrik přes **OpenTelemetry** (tokeny, cena, relace). Ve spojení s deníkem problémů to dává cenu a přínos na zákazníka nebo typ úkolu. Podrobnosti v této konverzaci neověřeny.
- Z deníku problémů později vznikne jednoduchá **sada evaluačních úloh**: opakované porovnání pravidel, modelů nebo postupů na stejných úlohách.

## Omezení a otevřené otázky

- Pořadí je interpretace asistenta. Správná priorita závisí na tom, jaká příčina v Jakubově deníku převažuje. Ten zatím v tomto repozitáři není.
- Jaké CI a issue tracker používají jednotliví zákazníci (GitHub, Azure DevOps, GitLab)? Určuje to volbu u bodů 2 a 4.
- Kódy analyzátorů, volby StyleCop, pluginy a OpenTelemetry je potřeba před zavedením ověřit v aktuální dokumentaci.

## Zdroje a související poznámky

- [C# LSP Plugin – Claude](https://claude.com/plugins/csharp-lsp) (ověřeno 2026-09-25)
- [anthropics/claude-plugins-official: csharp-lsp](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/csharp-lsp) (ověřeno 2026-09-25)
- [Microsoft Learn MCP Server overview](https://learn.microsoft.com/en-us/training/support/mcp) (ověřeno 2026-09-25)
- [Claude Code GitHub Actions](https://code.claude.com/docs/en/github-actions) (ověřeno 2026-09-25)
- [Cesta k přesnějším a autonomnějším AI agentům](path-to-reliable-autonomous-agents.md)
- [Harness u AI agentů](agent-harness.md)
- [Grafy a workflow místo klasického promptování](agent-graphs-and-workflows.md)
- [Globální a sdílené pokyny](global-and-shared-instructions.md)
