# CLAUDE.md — index.html

## Co je tento projekt

`index.html` je single-file webová aplikace (čistý HTML/CSS/JS, žádné závislosti) pro sledování příjmů z rozvozu jídla. Autor je kurýr na platformách **Wolt, Bolt Food a Foodora**, jezdí vlastním autem (Škoda Fabia I 1.4 MPI 44 kW, SPZ Liberec) a bydlí v oblasti Desná/Liberec.

Aplikace je navržena jako **PWA** (manifest + service worker) — přidává se na plochu telefonu a funguje offline.

Data se ukládají výhradně do **localStorage** — žádný backend, žádný server.

---

## Struktura aplikace

Aplikace má tři záložky:

### 1. Hlavní záložka (zadávání dne)
- Výběr platformy (Wolt / Bolt Food / Foodora) s barevnými tématy per platforma
- Zadání tržby za den
- Zadání ujetých km
- Výpočet nákladů na palivo (viz sekce kalkulačka níže)
- Uložení dne do deníku

### 2. Deník
- Seznam uložených dní
- Zobrazení tržby, km, nákladů, zisku pro každý den
- Možnost mazání záznamů

### 3. Statistiky
- Souhrnné přehledy (celková tržba, celkové km, průměry...)
- Spojnicový graf tržeb

---

## Kalkulačka nákladů na palivo

Kalkulačka je integrovaná přímo v `index.html`. Klíčové parametry:

### Spotřeba — Fabia I 1.4 MPI 44 kW
- **Rozvoz ve městě:** 9,5 l/100 km (preset "Rozvoz")
  - Tovární NEDC městský cyklus udává 8,9–10,7 l/100 km
  - Reálná data majitelů: 6,9–8,6 l/100 km v hustém provozu
  - 9,5 l/100 km je kompromis zohledňující studené starty a krátké jízdy typické pro rozvoz
- **Mimoměstská jízda (okresky):** 6,5 l/100 km (preset "Mimo město")

### Preset "Desná–Liberec"
- Checkbox (defaultně zapnutý): připočítá cestu Desná–Liberec a zpět (~50 km)
- Tato trasa má **vlastní fixní spotřebu 6,5 l/100 km** (períoky, ustálená rychlost)
- Počítá se **odděleně** od rozvozové spotřeby a výsledky se sečtou

### Logika výpočtu
```
náklady_rozvoz = (km_rozvoz / 100) * spotřeba_rozvoz * cena_benzinu
náklady_desna  = (50 / 100) * 6.5 * cena_benzinu   // pokud zapnuto
celkem = náklady_rozvoz + náklady_desna
```

---

## Technické poznámky

- Soubor je **single HTML file** — veškerý CSS a JS inline
- Žádné externí knihovny ani CDN
- Verze se označuje ve formátu `2026.0.0.x` (rok.major.minor.patch)
- Aktuální verze: **2026.0.0.4**
- Desetinná čárka i tečka musí fungovat v číselných inputech (lokalizační problém)
- Presetová tlačítka spotřeby musí hodnotu přednastavit **automaticky při načtení** (ne až po kliknutí)
- Cena PHM se zadává přes vlastní wheel picker (bottom sheet, dva sloupce Kč/haléře se scroll-snapem, styl iOS time pickeru); interně se ukládá jako celé haléře, naposledy zadaná hodnota se pamatuje v `localStorage` a je defaultní příště

---

## Historie vývoje (zkráceno)

- Základní kalkulačka tržby + paliva
- Přidány platformy (Wolt/Bolt Food/Foodora) s barevnými tématy
- PWA podpora (manifest, service worker, offline)
- localStorage persistence
- Záložky: hlavní / deník / statistiky
- Spojnicový graf v statistikách
- Kalkulačka paliva integrovaná do `index.html`
  - Preset "Rozvoz" (9,5 l/100km)
  - Preset "Mimo město" (6,5 l/100km)
  - Checkbox Desná–Liberec s oddělenou fixní spotřebou
- Cena PHM: wheel picker (Kč/haléře) s haptickou odezvou při scrollování a zapamatováním poslední hodnoty

---

## Kontext autora

- Rozvoz autem (ne na kole), platformy Wolt / Bolt Food / Foodora
- Bydliště: Desná (Jizerské hory), rozvoz probíhá v Liberci
- Cesta Desná–Liberec: ~25 km jedním směrem, celá po okresních silnicích
- Auto: Škoda Fabia I (6Y), 1.4 MPI, 44 kW, motor AUA/BBY (Bosch Motronic ME7.5.10)
- Provozní charakter: hodně krátkých jízd, studené starty, semaforový provoz bez dlouhých kolon
