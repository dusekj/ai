# Grafy a workflow místo klasického promptování

- Vytvořeno: 2026-09-23
- Aktualizováno: 2026-09-23

## Shrnutí

- „Graf“ v kontextu agentů znamená popsat práci jako **uzly** (agent nebo krok s jedním úkolem), **hrany** (pravidla, co běží dál) a **sdílený stav** (data předávaná mezi kroky). Místo toho, aby člověk v chatu krok za krokem říkal, co dělat dál, je postup zapsaný jednou a spouští se opakovaně.
- Prompt tím nezaniká. Přesouvá se **dovnitř uzlů** (zadání jednotlivého subagenta) a o pořadí kroků, větvení a ověřování rozhoduje struktura, ne ruční psaní dalších promptů.
- V Claude Code je nejpřímější cestou funkce **dynamic workflows** (od 2026-05-28): Claude napíše JavaScriptový skript, který orchestruje subagenty; skript lze uložit do `.claude/workflows/` a spouštět jako `/<název>`. Ověřeno v dokumentaci 2026-09-23.
- Doporučený přechod je postupný: nejdřív najít opakované prompty, pak je převést na skilly a subagenty, povinné přechody vynutit hooky a teprve ustálené, opakované procesy převést na uložené workflow.

## Klíčové pojmy

### Graf agentů

| Prvek grafu | Význam | Realizace v Claude Code |
| :-- | :-- | :-- |
| Uzel | Krok s jedním úkolem, vlastním kontextem a omezenými nástroji | Subagent v `.claude/agents/`, volání `agent()` ve workflow |
| Hrana | Kdo nebo co rozhodne o dalším kroku | Hlavní konverzace (model rozhoduje průběžně), hook (deterministicky), skript workflow (kód) |
| Stav | Data tekoucí mezi uzly | Kontext hlavní konverzace, soubory v repozitáři, proměnné skriptu, JSON výstup podle schématu |

Podstatný rozdíl mezi přístupy je, **kdo drží plán**. Dokumentace Claude Code to shrnuje takto:

| | Subagenti | Skilly | Agent teams | Workflows |
| :-- | :-- | :-- | :-- | :-- |
| Kdo rozhoduje o dalším kroku | Claude, tah po tahu | Claude podle instrukcí | Vedoucí agent | Skript |
| Kde žijí mezivýsledky | Kontext Clauda | Kontext Clauda | Sdílený seznam úkolů | Proměnné skriptu |
| Co je opakovatelné | Definice pracovníka | Instrukce | Definice týmu | Celá orchestrace |
| Rozsah | Několik delegací | Jako subagenti | Několik dlouhých peerů | Desítky až stovky agentů |

### Loop specification

Související myšlenka z článku S. Macedo (arXiv, 2026-06): místo krokového promptování předat agentovi znovupoužitelnou specifikaci smyčky s pěti částmi:

1. **Trigger** — co běh spouští (příkaz, plán, PR, hook).
2. **Goal** — jasně ověřitelný cíl.
3. **Verification** — jak se výsledek kontroluje (testy, build, nezávislý reviewer).
4. **Stopping rule** — kdy skončit (úspěch, žádný pokrok ve dvou kolech, limit).
5. **Memory** — co přetrvá mezi běhy (soubory, poznámky, uložené výsledky).

Autor sám uvádí, že loop engineering **doplňuje**, nenahrazuje prompt engineering. Tuto kostru lze použít jako šablonu pro každý proces, který chceme převést z chatu do grafu.

## Dynamic workflows v Claude Code

Ověřeno v dokumentaci 2026-09-23.

- **Spuštění:** v promptu napsat „use a workflow…“ nebo klíčové slovo `ultracode`. Nastavení `/effort ultracode` (xhigh effort + automatické workflow) nechá Clauda plánovat workflow pro každý podstatný úkol; vyžaduje Claude Code v2.1.203+.
- **Vestavěný příklad:** `/deep-research <otázka>` — paralelní hledání, křížové ověření zdrojů a report s citacemi; neověřitelná tvrzení označí.
- **Sledování:** `/workflows` ukazuje fáze, agenty, tokeny a čas; běh lze pozastavit, zastavit, restartovat agenta a klávesou `s` uložit skript.
- **Uložení:** `.claude/workflows/` v projektu (sdílené přes Git) nebo `~/.claude/workflows/` (osobní, všechny projekty). Uložené workflow se spouští jako `/<název>` a přijímá vstup přes globální `args`. Lze je distribuovat i v pluginu.
- **Podoba skriptu:** `export const meta = { name, description }` a tělo s `agent()`, `pipeline()` (agent pro každou položku), `parallel()`, `phase()` a `log()`. Volitelné `schema` vynutí JSON výstup subagenta.
- **Omezení:** během běhu nelze zadávat vstup (schvalování mezi fázemi = více workflow), skript sám nemá přístup k souborům ani shellu (jen agenti), nelze `import()`, výchozí limit 16 souběžných agentů a 1 000 agentů na běh. `Date.now()` a `Math.random()` ve skriptu vyhazují chybu kvůli deterministickému obnovení.
- **Obnovení:** běh lze obnovit ve stejné relaci; dokončení agenti vrátí uložený výsledek. Selhání uprostřed fan-outu ale znovu spustí i všechny později spuštěné agenty.
- **Cena:** výrazně více tokenů než běžná konverzace. Doporučení: nejdřív malý výsek, sledovat `/workflows`, nastavit `workflowSizeGuideline` (`small` < 5 agentů, `medium` < 10, `large` < 50) a pro jednodušší fáze zvolit menší model.

## Praktický postup přechodu

Postupný „žebřík“ od promptu ke grafu. Každý stupeň má smysl až tehdy, když předchozí opakovaně funguje:

| Stupeň | Co udělat | Signál, že je čas |
| :-- | :-- | :-- |
| 1. Prompt | Psát zadání s cílem, kontextem a kritériem ověření | Výchozí stav |
| 2. Skill / příkaz | Opakovaný prompt uložit jako skill | Stejný prompt píšu podruhé či potřetí |
| 3. Subagent | Úzce vymezená role (reviewer, výzkumník) s omezenými nástroji | Úkol zahlcuje hlavní kontext nebo potřebuje čerstvý pohled |
| 4. Hook | Povinný krok (testy, review) jako deterministická hrana | Model krok občas vynechá |
| 5. Uložené workflow | Ustálený vícekrokový proces jako `/<název>` | Proces je stabilní, opakovaný a má jasné ověření |
| 6. Běh bez dozoru | Plánované úlohy, Agent SDK, CI | Workflow spolehlivě funguje s dohledem |

Doporučené první kroky:

1. **Inventura promptů.** Projít poslední konverzace a sepsat, které typy zadání se opakují (review větve, oprava chyby, studijní rešerše).
2. **Pro jeden proces sepsat loop specification** (trigger, cíl, ověření, stop, paměť).
3. **Vyzkoušet `/deep-research`** na studijní otázce — jde o hotový graf, na kterém je vidět struktura fází, cena a kvalita výstupu.
4. **Nechat Clauda napsat workflow pro konkrétní úkol** (např. „use a workflow to review every file changed on this branch for correctness issues, then merge the findings into one ranked summary“), prohlédnout skript a při úspěchu jej uložit do `.claude/workflows/`.
5. **Porovnat s běžným postupem** — kvalitu, čas a spotřebu tokenů. Bez tohoto srovnání nelze tvrdit, že graf je lepší.

Kandidáti na workflow v tomto repozitáři (návrh, zatím neověřeno): rešerše tématu → ověření zdrojů → návrh poznámky → kontrola odkazů a Markdownu. Commit a push je vhodné nechat mimo workflow, protože jde o nevratnou akci po kontrole.

## Omezení a otevřené otázky

- **Graf se nevyplatí pro malé úkoly.** Režie, cena tokenů a složitost ladění převýší přínos. Anthropic obecně doporučuje začít jednoduše a složitost přidávat podle pozorovaných nedostatků (viz [cesta k přesnějším agentům](path-to-reliable-autonomous-agents.md)).
- **Comprehension debt:** vygenerovaný skript je potřeba číst a chápat; jinak hrozí slepé spoléhání na automatizaci.
- **Workflow neumí interaktivní schvalování** uprostřed běhu; lidský checkpoint znamená rozdělit proces na více workflow.
- **Codex:** ekvivalent uložených workflow jsem k 2026-09-23 neověřoval.
- **Jiné významy slova „graph“:** kódový graf (indexace závislostí v repozitáři přes MCP nástroje, např. CodeGraph) šetří kontext při průzkumu kódu, ale neřeší orchestraci. Frameworky jako LangGraph definují graf agentů programově mimo Claude Code. Obojí jsem zde neověřoval.

## Zdroje a související poznámky

- [Orchestrate subagents at scale with dynamic workflows — Claude Code Docs](https://code.claude.com/docs/en/workflows) (ověřeno 2026-09-23)
- [Introducing dynamic workflows — Claude blog, 2026-05-28](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)
- [Create custom subagents — Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [Building effective agents — Anthropic](https://www.anthropic.com/engineering/building-effective-agents)
- [S. Macedo: Stop Hand-Holding Your Coding Agent (arXiv 2607.00038)](https://arxiv.org/abs/2607.00038) — preprint, nerecenzováno
- [Graph Engineering with Claude Code](https://www.aibuilderclub.com/blog/graph-engineering-with-claude-code) — sekundární zdroj, mapování uzly/hrany/stav
- Související: [Harness u AI agentů](agent-harness.md), [Cesta k přesnějším a autonomnějším AI agentům](path-to-reliable-autonomous-agents.md)
