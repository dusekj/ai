# Obecná pravidla pro práci agenta

ID: general
Verze: 1.0.0
Aktualizováno: 2026-09-21
Platnost: všechny projekty, které tato pravidla načítají.

## Načtení a oznámení verze

- Na začátku nové konverzace přečti tento soubor a sourozenecký `csharp.md`. Verze zjisti ze skutečného obsahu souborů; nepřebírej je z příkladu, paměti ani předchozí konverzace.
- V první uživateli viditelné odpovědi uveď načtená ID, verze, zdrojové cesty a která pravidla používáš. Stačí jeden krátký řádek. Pokud musíš nejprve odeslat zprávu před použitím nástrojů, oznam v ní, že verze teprve ověříš, a skutečné verze doplň hned po načtení.
- Obecná pravidla používej vždy. C# pravidla používej pouze při práci v C#/.NET; jinak je označ jako načtená, ale pro aktuální úkol nepoužitelná. Pokud zatím rozsah neznáš, řekni, že jejich použití závisí na průzkumu projektu.
- Pokud soubor chybí, nelze jej přečíst nebo neobsahuje verzi, oznam konkrétní problém a netvrď, že tato pravidla používáš. Před změnami vyžadujícími chybějící pravidla vyřeš jejich dostupnost.
- Při změně pravidel během konverzace je znovu načti a oznam nové verze. Při běžných dalších odpovědích hlášení neopakuj.

## Práce na úkolu

- Před úpravami prozkoumej relevantní kód, projektové pokyny a stav Gitu. Zachovej nesouvisející uživatelské změny.
- Ujasni si požadované chování a způsob ověření. Na nejasnosti se ptej, pokud by odpověď podstatně změnila výsledek; běžné implementační volby řeš samostatně.
- Dodržuj zavedené konvence projektu. Měň pouze to, co souvisí s úkolem; nepřidávej nesouvisející refaktoring, závislosti ani změny veřejného rozhraní.
- Při opravě chyby ověř původně selhávající scénář. Vol kontroly přiměřené dopadu změny a používej existující projektové postupy.
- Rozlišuj provedené kontroly od předpokladů. V závěru stručně popiš výsledek, ověření a případné nevyřešené překážky; netvrď úspěch bez důkazu.
- Tato sdílená pravidla jsou výchozí. Konkrétní projektové a aktuální uživatelské pokyny je mohou upřesnit; respektuj hierarchii instrukcí nástroje.
- Commitování, push, jazyk obsahu a projektové názvosloví řiď pokyny konkrétního projektu nebo uživatele. Tento soubor sám nezavádí automatický commit ani push.

## Údržba

Při každé změně obsahu tohoto souboru zvyš jeho verzi a aktualizuj datum. Oprava formulace zvyšuje PATCH, nové slučitelné pravidlo MINOR, neslučitelná změna MAJOR. Verze druhého souboru se mění pouze při změně jeho obsahu. Udržuj pravidla stručná a založená na skutečných potřebách.
