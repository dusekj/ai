# Harness u AI agentů

Vytvořeno: 2026-09-23
Aktualizováno: 2026-09-23

## Shrnutí

- **Harness** (doslova „postroj, výstroj“) je software kolem jazykového modelu, který z něj dělá použitelného agenta. Model sám jen přijme text a vrátí text; harness zajišťuje vše ostatní.
- Zjednodušeně: **agent = model + harness**. Stejný model se v různých harnessech chová výrazně jinak.
- Claude Code, Codex CLI nebo Cursor agent jsou příklady harnessů. Uživatel je používá hlavně konfigurací: pokyny (`AGENTS.md`, `CLAUDE.md`), oprávnění, hooky, nástroje (MCP), skills a subagenty.
- Pojem se používá i v jiném významu: **evaluation harness** (test harness) je rámec pro opakované spouštění testů či benchmarků nad modelem.

## Co harness dělá

1. **Sestavuje kontext** — systémový prompt, projektové pokyny, historii konverzace, výsledky nástrojů, případně paměť.
2. **Nabízí nástroje** — popisuje modelu dostupné nástroje (čtení a zápis souborů, shell, prohlížeč, MCP servery).
3. **Řídí smyčku agenta** (agent loop): pošle kontext modelu → model odpoví textem nebo žádostí o volání nástroje → harness nástroj skutečně spustí → výsledek vrátí modelu → opakuje, dokud model nedokončí úkol.
4. **Hlídá bezpečnost a oprávnění** — co smí proběhnout bez potvrzení, co vyžaduje souhlas uživatele, sandbox.
5. **Spravuje kontextové okno** — zkracuje či shrnuje dlouhé konverzace, odkládá málo používané nástroje.
6. **Spouští deterministickou logiku** — hooky reagující na události (např. před voláním nástroje, po úpravě souboru).
7. **Zobrazuje výsledek** uživateli (terminál, desktopová aplikace, IDE).

Klíčový princip: model nic sám nespouští. Jen *navrhne* akci; provede ji harness a ten také rozhoduje, zda ji vůbec povolí.

## Jak se harness používá v praxi

| Páka | K čemu slouží | Příklad v Claude Code |
| --- | --- | --- |
| Pokyny | Trvalý kontext a pravidla projektu | `CLAUDE.md`, `AGENTS.md`, sdílené `rules/` |
| Oprávnění | Co agent smí dělat bez ptaní | `settings.json` → `permissions` (allow/deny) |
| Hooky | Deterministické akce, které nezávisí na „ochotě“ modelu | spustit formátovač po každé úpravě souboru |
| Nástroje | Rozšíření schopností | MCP servery (databáze, prohlížeč, dokumenty) |
| Skills | Znovupoužitelné postupy načítané podle potřeby | skill pro code review nebo tvorbu `.docx` |
| Subagenti | Oddělený kontext pro dílčí úkol | průzkumný agent, který vrátí jen závěr |

Praktické pravidlo: **co musí proběhnout vždy, patří do harnessu (hook, oprávnění), ne jen do promptu.** Pokyn v `CLAUDE.md` model obvykle dodrží, hook proběhne vždy.

Příklad z tohoto repozitáře: automatický commit a push po úpravě poznámek je zatím jen pokyn v [AGENTS.md](../AGENTS.md). Pokud by bylo potřeba to vynutit, šlo by to převést na hook (vlastní interpretace, nevyzkoušeno).

## Evaluation harness

Druhý význam: rámec, který nad modelem nebo agentem opakovaně spouští sadu úloh a měří výsledky (úspěšnost, cenu, čas). Příkladem je open-source projekt EleutherAI `lm-evaluation-harness`. U agentů se tím ověřuje, zda změna promptu, pravidel nebo nástrojů skutečně zlepšila výsledky — navazuje na téma ověřování v [cestě k přesnějším agentům](path-to-reliable-autonomous-agents.md).

## Omezení a nejasnosti

- Pojem „harness“ nemá jednu normovanou definici; někdy se jím míní jen agent loop, jindy celý produkt včetně UI.
- Konkrétní funkce harnessů (názvy nastavení, typy hooků) se rychle mění. Tabulka odpovídá stavu Claude Code k 2026-09-23 podle chování aktuální relace; odkazy na dokumentaci níže nebyly v tento den znovu ověřeny.

## Zdroje a související poznámky

- Dokumentace Claude Code: https://docs.anthropic.com/en/docs/claude-code (neověřeno k 2026-09-23)
- EleutherAI lm-evaluation-harness: https://github.com/EleutherAI/lm-evaluation-harness (neověřeno k 2026-09-23)
- [Globální a sdílené pokyny](global-and-shared-instructions.md)
- [Cesta k přesnějším a autonomnějším AI agentům](path-to-reliable-autonomous-agents.md)
