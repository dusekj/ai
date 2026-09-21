# Pravidla pro C# a .NET

ID: csharp
Verze: 1.1.0
Aktualizováno: 2026-09-21
Platnost: práce na C#/.NET kódu, projektech a jejich konfiguraci.

## Použití

Používej společně s `general.md` ve stejném adresáři. Při úvodním oznámení načtených pravidel uveď ID a verzi z tohoto souboru podle postupu v obecných pravidlech. Mimo C#/.NET tato technologická pravidla neuplatňuj.

## Základní pravidla

- Před změnou zjisti cílový framework, používané SDK a existující konfiguraci projektu. Respektuj `global.json`, projektové soubory a sdílenou konfiguraci, pokud existují; nepředpokládej nejnovější verzi .NET či C#.
- Používej API a jazykové konstrukce kompatibilní s cílovým projektem. Verze frameworku, SDK a balíčků měň pouze tehdy, když to vyžaduje zadání nebo dohodnuté řešení.
- Navazuj na existující architekturu, pojmenování a konfiguraci analýzy kódu, pokud nejsou v rozporu s platnými pravidly. Historická porušení v upravovaném souboru sjednoť podle postupu v `general.md`. Nezaváděj plošně nový návrhový vzor nebo abstrakci bez konkrétní potřeby úkolu.
- Všechny privátní metody pojmenovávej v PascalCase: první písmeno i začátky dalších slov jsou velké, například `LoadSettings` místo `loadSettings`. Platí to i pro statické a asynchronní privátní metody a metody, které jsou privátní implicitně bez uvedení modifikátoru. V upravovaném souboru přejmenuj i historicky odlišně pojmenované privátní metody a aktualizuj všechna jejich související použití; zachovej funkčnost.
- Při opravě problému neobcházej relevantní kontrolu vypnutím analyzátoru, potlačením varování nebo oslabením testu jen proto, aby kontrola prošla. Oprávněnou změnu takové konfigurace zdůvodni.
- Ověř sestavení dotčeného projektu a relevantní testy podle postupu repozitáře. Pokud ověření blokuje prostředí či chybějící závislost, uveď přesný rozsah neověřeného výsledku.

## Údržba

Při každé změně obsahu zvyš verzi a aktualizuj datum podle konvence popsané v `general.md`. Další pravidla přidávej podle konkrétních opakovaných chyb; tento základ nepředepisuje jednu architekturu všem C# projektům.
