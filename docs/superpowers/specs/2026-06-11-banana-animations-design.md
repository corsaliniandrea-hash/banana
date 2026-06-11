# Design: Animazioni CSS Poster Banana

**Data:** 2026-06-11  
**Argomento:** Animazioni CSS per il poster "Siamo alla Frutta"  
**Status:** Approvato

---

## 1. Obiettivo

Aggiungere animazioni CSS al poster esistente composto da due file SVG (`POSTER BANANA DEF.svg` e `TESTO.svg`) senza alterare layout, proporzioni, contenuto grafico o colori.

---

## 2. Elementi animati

1. **Testo** — 3 righe: “SIAMO”, “ALLA”, “FRUTTA”.
2. **Banana** — gruppo della banana sul monociclo.
3. **Icone** — 6 elementi decorativi nel gruppo `icone_che_girano`.

---

## 3. Architettura

- **File principale:** `index.html` che sovrappone i due SVG all’interno dello stesso contenitore con `viewBox="0 0 1080 1920"`.
- **Posizionamento:** entrambi gli SVG sono in `position: absolute; top: 0; left: 0; width: 100%; height: 100%` per mantenere allineamento perfetto.
- **Stile:** un unico blocco `<style>` nel `<head>` di `index.html`. Nessuna libreria esterna.

---

## 4. Modifiche minime agli SVG

Nessun tracciato, colore o coordinata verrà modificata. Solo aggiunta di classi semantiche.

### 4.1 `TESTO.svg`
- Wrappare ogni `<tspan>` (riga di testo) in un `<g class="poster-text-line">`.
- Aggiungere `<g class="poster-text-line">` come contenitore con `overflow: hidden` gestito dal CSS.

### 4.2 `POSTER BANANA DEF.svg`
- Aggiungere `class="poster-banana"` al gruppo `<g id="banana">`.
- Aggiungere `class="poster-icon"` a ciascuno dei 6 elementi figli nel gruppo `<g id="icone_che_girano">`.

---

## 5. Animazioni

### 5.1 Testo — `slideRevealLine`

| Proprietà | Valore |
|-----------|--------|
| `@keyframes` | `slideRevealLine` |
| `0%` | `translateX(-100%)`, `opacity: 0` |
| `100%` | `translateX(0)`, `opacity: 1` |
| Durata | `0.6s` |
| Easing | `ease-out` |
| `animation-fill-mode` | `both` |
| Delay righe | 1ª: `0s`, 2ª: `0.15s`, 3ª: `0.3s` |
| `mix-blend-mode` | `multiply` (già presente nello stile originale) |

Il contenitore di ogni riga avrà `overflow: hidden` per mascherare la comparsa.

### 5.2 Banana — `bananaBalance`

| Proprietà | Valore |
|-----------|--------|
| `@keyframes` | `bananaBalance` |
| `0%` | `rotate(0deg) translateX(0)` |
| `25%` | `rotate(-4deg) translateX(-3px)` |
| `50%` | `rotate(3deg) translateX(2px)` |
| `75%` | `rotate(-2deg) translateX(-1px)` |
| `100%` | `rotate(0deg) translateX(0)` |
| `transform-origin` | `50% 85%` |
| Durata | `2.4s` |
| Iterazione | `infinite` |
| Easing | `ease-in-out` |

### 5.3 Icone — `iconHalfRotate`

| Proprietà | Valore |
|-----------|--------|
| `@keyframes` | `iconHalfRotate` |
| `0%` | `rotate(-18deg)` |
| `50%` | `rotate(18deg)` |
| `100%` | `rotate(-18deg)` |
| `transform-origin` | `center` |
| Durata | `1.6s` – `2.2s` (variabile per icona) |
| Iterazione | `infinite` |
| Easing | `ease-in-out` |
| Delay | sfalsato via `nth-child` su `.poster-icon` |

---

## 6. Cosa NON cambia

- Layout, proporzioni e coordinate SVG originali.
- Colori, stroke, fill o altri attributi di stile.
- Contenuto testuale o grafico.
- Nessuna libreria esterna aggiunta.

---

## 7. File di output

| File | Scopo |
|------|-------|
| `index.html` | Contenitore HTML/CSS che carica i due SVG e applica le animazioni |
| `POSTER BANANA DEF.svg` | Aggiunta classi `poster-banana` e `poster-icon` |
| `TESTO.svg` | Aggiunta classi `poster-text-line` su ogni riga |

---

## 8. Note per l’implementazione

- Gli SVG verranno inclusi inline (`<svg>...</svg>`) in `index.html` oppure referenziati come `<img>` o `<object>` se si preferisce separazione. Per garantire che il CSS esterno possa selezionare elementi interni, la scelta consigliata è **inline**.
- Il CSS degli SVG originali (definizioni in `<defs><style>`) rimane invariato; il nuovo CSS nel `<head>` di `index.html` sovrascrive/aggiunge solo le proprietà di animazione.
- `overflow: hidden` sul contenitore della riga di testo serve a evitare artefatti visivi durante l’animazione `translateX`.
