# Globální a sdílené pokyny pro AI nástroje

Vytvořeno: 2026-09-21
Aktualizováno: 2026-09-21
Zdroje ověřeny: 2026-09-21

## Potřeba

Jakub chce definovat opakující se pravidla jednou a používat je ve všech projektech, například při práci v C#. Konkrétní opakované chyby zatím nevyjmenoval.

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

Začít krátkou podmíněnou sekcí v globálním souboru. Rozsáhlejší pravidla lze později oddělit do společného dokumentu a globálně uložit pokyn k jeho přečtení před prací v C#. V Codexu takový textový odkaz sám o sobě není automatický import souboru; agent musí soubor skutečně otevřít. Osobní skill může být vhodný pro delší opakovaný postup.

Příklady formulace, nikoli dosud přijatá uživatelova pravidla: před změnou zjistit cílový framework a používané konvence; neprovádět nesouvisející změny závislostí; konkrétní opakovanou chybu popsat spolu s požadovanou alternativou a způsobem ověření.

## Sdílení s Claude Code a mezi počítači

Claude Code má osobní soubor `~/.claude/CLAUDE.md` a podporuje importy pomocí `@cesta`. Společný obsah proto může žít v samostatném Git repozitáři pravidel. Do globálního souboru Codexu jej lze při instalaci/synchronizaci kopírovat nebo vytvořit vhodný odkaz na soubor; Claude může tentýž obsah importovat ze svého osobního souboru. Je třeba zajistit existenci cest v každém prostředí.

Aktuální Claude Code umí za určitých podmínek načítat také projektové AGENTS.md; to není důvod předpokládat, že automaticky načítá globální soubor z adresáře Codexu. [Claude Code: paměť a instrukční soubory](https://code.claude.com/docs/en/memory)

## Omezení a další krok

Textová pravidla nezaručují bezchybné dodržení. Pro opakované chyby je vhodné podle jejich povahy doplnit automatickou kontrolu. Nejprve sepsat konkrétní příklady chyb, potom rozhodnout, co patří mezi globální preference a co do projektu či kontrol.

Související: [Cesta k přesnějším a autonomnějším agentům](cesta-k-presnejsim-a-autonomnim-agentum.md).
