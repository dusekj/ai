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

## Vynucení code review v Claude Code i Codexu

Otázka (2026-09-23): lze automatické review vynutit přes AGENTS.md tak, aby fungovalo v obou nástrojích?

Tři úrovně, od nejslabší po nejspolehlivější:

1. **Pokyn v AGENTS.md** — funguje v obou nástrojích, ale jen jako instrukce; model ho obvykle dodrží, nikoli vždy. Claude Code nativně čte `CLAUDE.md`, ne `AGENTS.md`; sdílení řeší [globální pokyny](global-and-shared-instructions.md). Slash příkazy se liší: Claude Code má skill `code-review`, který umí spustit i model sám; Codex má uživatelský `/review` a samostatný příkaz `codex review --uncommitted` (případně `--base <větev>`, `--commit <SHA>`).
2. **Stop hook** — skutečné vynucení. Oba nástroje mají událost `Stop`, která se spustí při ukončení tahu a může agenta vrátit k další práci: exit code `2` s důvodem na `stderr`, nebo JSON `{"decision": "block", "reason": "..."}`. Oba posílají pole `stop_hook_active`, podle kterého hook pozná, že už jednou pokračování vynutil, a zabrání nekonečné smyčce. Jeden skript tak může sloužit oběma; liší se jen místo konfigurace: `.claude/settings.json` vs. `.codex/hooks.json` (nebo `config.toml`). Codex vyžaduje, aby uživatel projektový hook nejdřív schválil (trust).
3. **Git hook nebo CI** — nezávislé na nástroji, poslední brána před sloučením (např. `codex review --base main` nebo Claude Code v CI).

Náčrt společného Stop hooku (neotestováno):

```bash
#!/usr/bin/env bash
# Stop hook pro Claude Code i Codex: při neuložených změnách jednou vynutí review.
input=$(cat)
if echo "$input" | grep -Eq '"stop_hook_active"[[:space:]]*:[[:space:]]*true'; then
  exit 0
fi
if [ -z "$(git status --porcelain)" ]; then
  exit 0
fi
echo "Před dokončením zkontroluj svůj diff vůči zadání a pravidlům projektu (v Claude Code použij skill code-review), oprav nalezené chyby a shrň výsledek review." >&2
exit 2
```

Omezení náčrtu: kontroluje jen necommitnuté změny (v tomto studijním repozitáři agent commituje před koncem, takže by se nespustil); review provádí stejný agent se stejným kontextem, ne nezávislý reviewer. Nezávislejší varianta: hook sám spustí `codex review --uncommitted` nebo `claude -p` a nálezy předá jako `reason` — pomalejší a dražší.

## Evaluation harness

Druhý význam: rámec, který nad modelem nebo agentem opakovaně spouští sadu úloh a měří výsledky (úspěšnost, cenu, čas). Příkladem je open-source projekt EleutherAI `lm-evaluation-harness`. U agentů se tím ověřuje, zda změna promptu, pravidel nebo nástrojů skutečně zlepšila výsledky — navazuje na téma ověřování v [cestě k přesnějším agentům](path-to-reliable-autonomous-agents.md).

## Omezení a nejasnosti

- Pojem „harness“ nemá jednu normovanou definici; někdy se jím míní jen agent loop, jindy celý produkt včetně UI.
- Konkrétní funkce harnessů (názvy nastavení, typy hooků) se rychle mění. Tabulka odpovídá stavu Claude Code k 2026-09-23 podle chování aktuální relace; odkazy na dokumentaci níže nebyly v tento den znovu ověřeny.

## Zdroje a související poznámky

- Dokumentace Claude Code: https://docs.anthropic.com/en/docs/claude-code (neověřeno k 2026-09-23)
- Claude Code hooks: https://code.claude.com/docs/en/hooks (ověřeno 2026-09-23)
- Codex hooks: https://learn.chatgpt.com/docs/hooks (ověřeno 2026-09-23)
- Codex příkazy včetně `codex review`: https://learn.chatgpt.com/docs/developer-commands?surface=cli (ověřeno 2026-09-23)
- EleutherAI lm-evaluation-harness: https://github.com/EleutherAI/lm-evaluation-harness (neověřeno k 2026-09-23)
- [Globální a sdílené pokyny](global-and-shared-instructions.md)
- [Cesta k přesnějším a autonomnějším AI agentům](path-to-reliable-autonomous-agents.md)
