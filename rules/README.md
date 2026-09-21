# Sdílená pravidla

Společný zdroj pokynů pro Codex a Claude Code:

- [Obecná pravidla](general.md)
- [C# a .NET](csharp.md)

Každý soubor má vlastní verzi přímo v obsahu. Agent má v první odpovědi oznámit skutečně načtené verze i zdrojové cesty a odlišit načtení od použití technologických pravidel.

## Zapojení do jiného projektu

Do jeho načítaného `AGENTS.md` vlož následující blok. Cesty uprav podle prostředí, kde agent běží:

```markdown
## Sdílená pravidla

Před první odpovědí načti a dodržuj pravidla ze souborů:
- `D:/Data/AI/rules/general.md`
- `D:/Data/AI/rules/csharp.md`

V první odpovědi uveď jejich skutečné verze a zdrojové cesty podle
postupu v general.md. C# pravidla používej pouze pro práci v C#/.NET.
Pokud pravidla nelze načíst, oznam to a nevymýšlej jejich verze.
```

Verze do tohoto bloku nekopíruj; agent je musí zjistit z aktuálních souborů. Tento projekt je již napojen relativními cestami ve svém kořenovém AGENTS.md. Globální konfigurace nástrojů ani jiné repozitáře tím nejsou automaticky změněny.

## Kontrola načtení

V novém úkolu očekávej například hlášení tohoto tvaru (zástupné hodnoty musí agent nahradit skutečnými):

```text
Pravidla: general v<verze> — používám (<cesta>);
csharp v<verze> — používám pro C#/.NET (<cesta>).
```

U jiného typu úkolu má u csharp uvést „načteno, pro tento úkol nepoužívám“. Hlášení je viditelné potvrzení agenta, nikoli nezávislý důkaz, že dodrží každé pravidlo. Pokud hlášení chybí, ověř, zda nástroj načetl vstupní AGENTS.md a má přístup k cílovým souborům. Podmínky načítání souborů jednotlivými nástroji shrnují [poznámky](../notes/global-and-shared-instructions.md).

Pro cloud nebo jiný počítač zajisti aktuální dostupnou kopii tohoto repozitáře a uprav cesty. Samotný odkaz obsah neimportuje. Změny pravidel již běžící konverzace automaticky nenačítají; vyžádej jejich opětovné načtení nebo začni novou konverzaci.
