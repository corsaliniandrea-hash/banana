# Banana Poster CSS Animations Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add CSS animations to the existing banana poster SVGs without altering layout, proportions, or content.

**Architecture:** Create a single `index.html` that overlays the two modified SVGs (poster and text) and applies all animations via an internal `<style>` block. The SVG files receive only semantic class additions.

**Tech Stack:** HTML5, CSS3 (no external libraries)

---

## File Structure

| File | Action | Responsibility |
|------|--------|----------------|
| `TESTO.svg` | Modify | Split single `<text>` into three independent `<text>` elements with class `poster-text-line` |
| `POSTER BANANA DEF.svg` | Modify | Add class `poster-banana` to banana group; add class `poster-icon` to every path inside `icone_che_girano` group |
| `index.html` | Create | Overlay both SVGs inline and host all CSS `@keyframes` and animation rules |

---

### Task 1: Modify `TESTO.svg` to separate text lines and add animation classes

**Files:**
- Modify: `TESTO.svg`

- [ ] **Step 1: Replace the single `<text>` element with three separate `<text>` elements**

Find:
```xml
    <g id="Livello_10" data-name="Livello 10">
      <text class="cls-1" transform="translate(44.81 839.94)"><tspan x="0" y="0">SIAMO</tspan><tspan x="0" y="281">ALLA</tspan><tspan x="0" y="562">FRUTTA</tspan></text>
    </g>
```

Replace with:
```xml
    <g id="Livello_10" data-name="Livello 10">
      <text class="cls-1 poster-text-line" transform="translate(44.81 839.94)">SIAMO</text>
      <text class="cls-1 poster-text-line" transform="translate(44.81 1120.94)">ALLA</text>
      <text class="cls-1 poster-text-line" transform="translate(44.81 1401.94)">FRUTTA</text>
    </g>
```

- [ ] **Step 2: Verify no layout change**

Open `TESTO.svg` in a browser and visually confirm the three lines appear in exactly the same positions as before.

---

### Task 2: Modify `POSTER BANANA DEF.svg` to add semantic classes

**Files:**
- Modify: `POSTER BANANA DEF.svg`

- [ ] **Step 1: Add `poster-banana` class to the banana group**

Find:
```xml
  <g id="banana">
```

Replace with:
```xml
  <g id="banana" class="poster-banana">
```

- [ ] **Step 2: Add `poster-icon` class to every icon path**

Find the group:
```xml
  <g id="icone_che_girano" data-name="icone che girano">
```

Inside this group, every child `<path>` or `<polygon>` must receive the additional class `poster-icon`. For example, the first path currently reads:
```xml
    <path class="cls-5" d="M395.44,906.66l-6.44,1.93 ...">
```
Change it to:
```xml
    <path class="cls-5 poster-icon" d="M395.44,906.66l-6.44,1.93 ...">
```

Repeat this for **all** child elements inside `<g id="icone_che_girano">` (7 total paths/polygons).

- [ ] **Step 3: Verify no visual change**

Open `POSTER BANANA DEF.svg` in a browser and confirm the poster looks identical to the original.

---

### Task 3: Create `index.html` with inline SVGs and CSS animations

**Files:**
- Create: `index.html`

- [ ] **Step 1: Build the HTML shell**

Create `index.html` with the following structure:

```html
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Poster Banana — Animazioni</title>
  <style>
    /* Container */
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      overflow: hidden;
      background: #fff;
    }

    .poster-stage {
      position: relative;
      width: 100vw;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .poster-stage svg {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }

    /* ── Text animation ── */
    @keyframes slideRevealLine {
      0% {
        transform: translateX(-100%);
        opacity: 0;
      }
      100% {
        transform: translateX(0);
        opacity: 1;
      }
    }

    .poster-text-line {
      animation: slideRevealLine 0.6s ease-out both;
    }

    .poster-text-line:nth-of-type(1) { animation-delay: 0s; }
    .poster-text-line:nth-of-type(2) { animation-delay: 0.15s; }
    .poster-text-line:nth-of-type(3) { animation-delay: 0.3s; }

    /* ── Banana animation ── */
    @keyframes bananaBalance {
      0%   { transform: rotate(0deg) translateX(0); }
      25%  { transform: rotate(-4deg) translateX(-3px); }
      50%  { transform: rotate(3deg) translateX(2px); }
      75%  { transform: rotate(-2deg) translateX(-1px); }
      100% { transform: rotate(0deg) translateX(0); }
    }

    .poster-banana {
      transform-origin: 50% 85%;
      animation: bananaBalance 2.4s ease-in-out infinite;
    }

    /* ── Icons animation ── */
    @keyframes iconHalfRotate {
      0%   { transform: rotate(-18deg); }
      50%  { transform: rotate(18deg); }
      100% { transform: rotate(-18deg); }
    }

    .poster-icon {
      transform-origin: center;
      animation: iconHalfRotate ease-in-out infinite;
    }

    .poster-icon:nth-child(1) { animation-duration: 1.6s; animation-delay: 0s; }
    .poster-icon:nth-child(2) { animation-duration: 1.8s; animation-delay: 0.2s; }
    .poster-icon:nth-child(3) { animation-duration: 2.0s; animation-delay: 0.4s; }
    .poster-icon:nth-child(4) { animation-duration: 2.2s; animation-delay: 0.6s; }
    .poster-icon:nth-child(5) { animation-duration: 1.7s; animation-delay: 0.1s; }
    .poster-icon:nth-child(6) { animation-duration: 1.9s; animation-delay: 0.3s; }
    .poster-icon:nth-child(7) { animation-duration: 2.1s; animation-delay: 0.5s; }
  </style>
</head>
<body>
  <div class="poster-stage">
    <!-- POSTER SVG will be inlined here -->
    <!-- TEXT SVG will be inlined here -->
  </div>
</body>
</html>
```

- [ ] **Step 2: Inline `POSTER BANANA DEF.svg`**

Copy the **complete, modified** content of `POSTER BANANA DEF.svg` (the file already updated in Task 2) and paste it inside the `poster-stage` `<div>`, replacing the comment `<!-- POSTER SVG will be inlined here -->`.

Keep only the inner `<svg>` element (from `<svg xmlns="..."` to `</svg>`); do **not** include the XML declaration `<?xml version="1.0" encoding="UTF-8"?>`.

- [ ] **Step 3: Inline `TESTO.svg`**

Copy the **complete, modified** content of `TESTO.svg` (the file already updated in Task 1) and paste it inside the same `poster-stage` `<div>`, replacing the comment `<!-- TEXT SVG will be inlined here -->`.

Again, keep only the inner `<svg>` element, discarding the XML declaration.

- [ ] **Step 4: Ensure both SVGs share the same viewBox**

Confirm that both inlined `<svg>` tags declare `viewBox="0 0 1080 1920"`. This guarantees pixel-perfect overlay.

---

### Task 4: Verification

**Files:**
- Test: `index.html`

- [ ] **Step 1: Check text reveal animation**

Open `index.html` in a browser. Observe that:
- The text lines slide in from the left.
- Each line appears with the correct staggered delay (0s, 0.15s, 0.3s).
- `mix-blend-mode: multiply` remains active (defined in the original SVG class `.cls-1`).

- [ ] **Step 2: Check banana balance animation**

The banana group should oscillate gently and infinitely with the specified rotation and translation values.

- [ ] **Step 3: Check icons rotation animation**

All icons in the `icone_che_girano` group should oscillate between `-18deg` and `+18deg` with different durations and delays.

- [ ] **Step 4: Check no layout shifts**

Confirm that:
- Poster and text overlay perfectly.
- No elements are cropped unexpectedly.
- Original proportions and colors are preserved.

---

### Task 5: Commit

- [ ] **Step 1: Stage changes**

```bash
git add index.html "POSTER BANANA DEF.svg" TESTO.svg
```

- [ ] **Step 2: Commit**

```bash
git commit -m "feat: add CSS animations to banana poster"
```

---

## Plan Self-Review

1. **Spec coverage:**
   - Text separation with `poster-text-line` → Task 1
   - `slideRevealLine` with delays → Task 3 CSS
   - `bananaBalance` with `transform-origin` → Task 2 + Task 3 CSS
   - `iconHalfRotate` with `nth-child` delays → Task 2 + Task 3 CSS
   - No layout/content alteration → enforced by minimal SVG edits and inline overlay

2. **Placeholder scan:** None found. All code blocks contain exact markup/CSS.

3. **Type consistency:** Class names (`poster-text-line`, `poster-banana`, `poster-icon`) match between SVG modifications and CSS selectors.
