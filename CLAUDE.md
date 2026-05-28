# H2 2026 Plánovač — #HzM EDU tým

Interaktivní plánovací nástroj pro druhé pololetí 2026. Jeden HTML soubor, žádný build.
Pracuje v něm Lam (vede EDU tým). Komunikuj **česky**.

## Nasazení
- Repo: `h2-planner` (org `holkyzmarketingu`)
- Editovaný soubor je vždy **`index.html`** v kořeni repa — to servíruje GitHub Pages.
- Živá verze: `holkyzmarketingu.github.io/h2-planner/`
- Workflow: uprav `index.html` → zkontroluj lokálně v prohlížeči → `git add index.html && git commit -m "…" && git push`. Nasazení trvá ~minutu; v prohlížeči pak hard refresh (Cmd+Shift+R).

## Technika
- Single-file HTML: veškeré CSS i JS jsou inline v `index.html`. Žádný npm, žádný build, žádné externí JS knihovny.
- Fonty: **Fraunces** (nadpisy, čísla) + **DM Sans** (text), tahané z Google Fonts.
- Vše je čistý vanilla JS. Stav drží JS pole na konci souboru, UI se překresluje funkcemi `recompute()`, `renderTier()`, `renderVertikaly()`, `renderAkademie()`.

## Struktura — 4 taby
1. **Plán scénářů** (`tab-scenario`) — kolik kohort Mini a termínů Školení je potřeba. Editovatelné ceny + posuvníky obsazenosti + presety + rizikové parametry (rezerva, výpadek léta). Funkce `recompute()`, pole `targets` (měsíční cíle Jul–Pro).
2. **Hierarchie témat** (`tab-hierarchie`) — 4 tiery ŠKO témat (A–D), editovatelné počet témat/fill/běhů, dopočítává termíny + kolik nových témat vyrobit. Funkce `renderTier()`, pole `skoTiers`.
3. **Rozdělení dle vertikál** (`tab-vertikaly`) — 18 tematických vertikál, tržby 2025 + 2026, editovatelný H2 cíl. Funkce `renderVertikaly()`, pole `vertikaly`.
4. **Akademie – Podzim 2026** (`tab-akademie`) — tabulka 4 Akademií, editovatelné třídy/studentky/cena. Funkce `renderAkademie()`, pole `akademie`.

## Klíčová čísla a pravidla (NEMĚNIT bez výslovného pokynu)
- Celkový H2 cíl **8,5 M Kč** = Mini 3,64 M + Školení 4,14 M + AI Akademie 0,72 M.
- **Všechny ceny jsou s DPH (gross).** Nikdy nepřidávej koeficient ×1,21.
- Výchozí ceny (editovatelné v UI): Mini 2 875 Kč, Školení 664 Kč, Konzultace 5 000 Kč.
- Hierarchie ŠKO: `SKO_PRICE = 664`, `SKO_TARGET = 3 840 000` (webináře = Škol 4,14 M − 0,3 M konzultace).
- Vertikály: `VERT_TOTAL = 8500`, AI vertikála je pevně 1 393 k a má `highlight: true`.
- Akademie: do 8,5 M cíle se počítá jen AI Akademie (0,72 M). JA, ASS a AMM jsou samostatný příjmový proud mimo plán Mini/Škol.
- Uzávěrka 17. 12. — všechny eventy musí začít do tohoto data; prosinec se modeluje jako poloviční měsíc.

## Vizuální styl tohoto plánovače
Tenhle nástroj má **vlastní editoriální paletu** — teplá krémová (`--bg #FBF8F1`), zelená pro Mini (`--mini #3F6B5E`), rezavá pro Školení (`--skol #B05C40`), fialová pro Konzultace (`--consult #6B5A8C`). **Není to brandová #HzM paleta (fuchsie/víno) — neopravuj barvy na brandové a neměň fonty.** Tohle je záměrné.

## Jazyk a psaní
- Čeština jako od rodilého mluvčího. **Žádné anglicismy** v textu pro uživatele: rezerva (ne buffer), opakování (ne reruny), příjmový proud (ne revenue stream), pesimistický scénář (ne downside), uzávěrka (ne stop date), pozicování (ne positioning), nišový (ne nice). Výjimka: zavedené termíny „fill", „tier", „event" se nechávají.
- Jméno **„Lam" se v češtině neskloňuje** — ve všech pádech zůstává „Lam".
- Tón plánovače: stručný, tykání uživateli (klikni, uprav, posuň).

## Mantinely při editaci
- Po každé změně se ujisti, že JS pořád běží: otevři `index.html` v prohlížeči a koukni, jestli se taby překreslují a čísla počítají. Snadno se rozbije středník nebo uvozovka uvnitř `innerHTML` stringů.
- Neměň `data-mini` / `data-skol` / `data-consult` atributy u tlačítek presetů — drží hodnoty scénářů.
- Když přidáš sloupec do tabulky, uprav i hlavičku (`<th>`) i řádek (`<td>`), ať počty sedí.
- **Přepočty z dat (tržby vertikál, fill rates, klasifikace produktů) nedělej tady** — zdrojová CSV nejsou v repu, žijí v projektu na claude.ai. Tady dělej text, strukturu a logiku modelu. Když je potřeba přepočítat čísla z reálných dat, řekni Lam, ať to zadá v projektu na claude.ai.
