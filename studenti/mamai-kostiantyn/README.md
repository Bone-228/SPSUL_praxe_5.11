# SMS zprávy: phishing a typ podvodu

## Co nástroj dělá

Nástroj z krátké české SMS zprávy určí, zda jde o phishingové nebo jiné podvodné
sdělení. Pokud je zpráva podvodná, přiřadí jí jeden typ podvodu podle obsahu.

## Schéma výstupu

```json
{
  "phishing": true,
  "typ_podvodu": "banka | doruceni | urady | investice | vyhra | ucet | jine | null"
}
```

- `phishing` (`boolean`) — `true`, pokud se zpráva snaží příjemce podvést,
  vylákat přihlašovací údaje, peníze nebo jiné citlivé údaje; jinak `false`.
- `typ_podvodu` (`string nebo null`) — typ podvodu, pokud je zpráva phishingová;
  pro běžnou legitimní zprávu je `null`.

Povolené hodnoty `typ_podvodu`:

- `banka` — zpráva se vydává za banku nebo platební službu;
- `doruceni` — falešné doručení balíku nebo doplatku za dopravu;
- `urady` — vydává se za úřad, policii nebo jinou veřejnou instituci;
- `investice` — slibuje snadný zisk nebo láká na investování;
- `vyhra` — falešná výhra, soutěž nebo odměna;
- `ucet` — krádež přístupu k účtu mimo bankovní zprávy, například k e-mailu;
- `jine` — podvod, který nespadá do žádné z uvedených kategorií.

## Pravidla porovnávání

- Porovnávání JSON hodnot je přesné podle názvů polí a jejich hodnot.
- U textových hodnot ignoruji velikost písmen, nadbytečné mezery a rozdíly mezi
  českou diakritikou a její verzí bez diakritiky.
- `phishing` musí být skutečná logická hodnota `true` nebo `false`, ne text.
- U legitimní zprávy musí být `typ_podvodu` přesně `null`.
- U podvodné zprávy musí `typ_podvodu` obsahovat jednu z povolených hodnot.
- Pokud zpráva obsahuje více podvodných motivů, rozhoduje hlavní účel zprávy;
  pokud jej nelze určit, použije se `jine`.

## Stav práce

Úloha: klasifikace SMS zpráv podle toho, zda jde o phishing, a podle typu
podvodu. Testovací data, prompty, evaluační skript a výsledky budou doplněny
v dalších fázích.

## Spuštění evaluace

Vytvoř soubor `.env` podle `.env.example` a doplň API klíče. Soubor `.env` se
nesmí commitnout do repozitáře.

Z kořene této složky spusť například:

```bash
python src/eval.py --provider groq --model openai/gpt-oss-20b --set dev --prompt v1
python src/eval.py --provider mistral --model mistral-small-latest --set dev --prompt v1
python src/eval.py --provider gemini --model gemini-2.5-flash --set dev --prompt v1
```

Provider, model, sada a prompt se nastavují parametry příkazového řádku.
Odpovědi se ukládají do `cache/`, podrobné výsledky do `results/`, a souhrn
všech běhů do `results/summary.json`.
