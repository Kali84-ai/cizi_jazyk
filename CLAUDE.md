# CLAUDE.md

Pokyny pro Claude Code při dalším rozvoji projektu **English Quest** – interaktivní aplikace pro učení angličtiny (pro české uživatele).

## Projekt v kostce
- Celá aplikace je jeden soubor `index.html` (HTML + CSS + vanilla JS, žádné závislosti, žádný build).
- Hostuje se přes **GitHub Pages** z větve `main` – soubor se proto musí jmenovat `index.html` a ležet v kořeni repa.
- Repozitář: https://github.com/Kali84-ai/cizi_jazyk
- Jazyk UI: **čeština**; učený jazyk: **angličtina**.

## Spuštění a test
- Stačí otevřít `index.html` v prohlížeči (Chrome/Edge doporučeno kvůli rozpoznávání řeči).
- Testy zatím nejsou; po změně ručně projdi: onboarding → rozřazovací test → každý režim → výsledková obrazovka, a zkontroluj konzoli bez chyb.

## Architektura (`index.html`, sekce `<script>`)
| Sekce | Obsah |
|---|---|
| `DATA` | `LEVELS` (A1–C2), `VOCAB` – slovíčka `[en, cz, příkladová věta]`, `MODES`, `BADGES` |
| `STATE` | objekt `S` v `localStorage` pod klíčem `englishQuest.v1` (`load`/`save` vždy v try/catch) |
| `HELPERS` | `pickWords` (Leitnerův systém, přihrádky 0–5, ≥3 = zvládnuto), `say` (Web Speech TTS), `lev` (Levenshtein) |
| `SCREENS` | `renderOnboard`, `renderHome`, přepínání obrazovek přes `show(id)` |
| `LESSON ENGINE` | `startLesson` → `next` → `RENDER[type]` → `answer(ok, w)` → `finish` |
| `EXERCISE RENDERERS` | `RENDER.quiz/gap/listen/build/flash/speak` – každý dostane slovo `w` a kořenový element |
| `PLACEMENT TEST` | 2 otázky z každé úrovně, doporučí nejvyšší souvisle zvládnutou úroveň |

## Konvence
- Barvy pouze přes CSS proměnné v `:root` (světlý i tmavý motiv – při přidání barvy ji doplň do obou).
- Uživatelský text vkládaný do HTML vždy přes `esc()`.
- Nový režim = záznam v `MODES` + funkce v `RENDER`, která na konci volá `answer(ok, w, poznámka)`.
- Nová slovíčka přidávej do `VOCAB` jako `[en, cz, věta]`; věta musí obsahovat dané slovo (využívá ji „Doplň slovo“ a „Skládání vět“).
- Při změně struktury stavu zvyš verzi klíče `STORE_KEY` nebo doplň migraci.
- Mobilní layout musí fungovat od šířky 360 px.

## Nápady na rozvoj
- Více slovíček a tematické balíčky (cestování, práce, jídlo).
- Gramatická cvičení (časy, členy, předložky).
- Export/import pokroku, nastavitelný denní cíl.
- PWA (offline režim, instalace na mobil).
