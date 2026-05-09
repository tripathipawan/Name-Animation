# Name Animation

A pure CSS hover interaction that reveals a hidden acrostic poem — built with only HTML and CSS. No JavaScript, no animation library, no external dependency of any kind. The entire effect is driven by two CSS properties: `letter-spacing` and `overflow: hidden`, toggled on hover using sibling selectors.

---

## What This Project Does

Five `<h2>` lines sit stacked on screen, displaying only their first letter. Each first letter is the initial of a word that together spells **N · A · W · A · P** — an acrostic for **Pawan** (read bottom to top, since the body uses `flex-direction: column-reverse`). When you hover over any line, that line and every line below it expand to reveal their full words simultaneously.

The 5 lines spell out the phrase:

```
N — Nature
A — As
W — Wise
A — And
P — Peaceful
```

Reading the initials bottom to top: **P · A · W · A · N → PAWAN**.

---

## How the HTML Is Structured

Each line is one `<h2>` containing exactly 2 `<span>` elements:

```html
<h2><span>N</span><span>ature</span></h2>
<h2><span>A</span><span>s</span></h2>
<h2><span>W</span><span>ise</span></h2>
<h2><span>A</span><span>nd</span></h2>
<h2><span>P</span><span>eaceful</span></h2>
```

- **First `<span>` (`:nth-child(odd)`)** — holds the initial capital letter. Always visible.
- **Second `<span>` (`:nth-child(even)`)** — holds the rest of the word. This is the span that hides and reveals.

The body has `flex-direction: column-reverse`, so the `<P>eaceful` line renders at the bottom of the screen and `<N>ature` at the top — making the acrostic read **PAWAN** from bottom to top visually.

---

## How the CSS Works — Full Breakdown

**Body layout:**
```css
body {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column-reverse;
  min-height: 100vh;
  background: #001401;
}
```
`column-reverse` reverses the DOM order visually — the last `<h2>` (`<P>eaceful`) appears at the bottom. This is what makes PAWAN readable from bottom to top.

**`<h2>` base style:**
```css
h2 {
  font-size: 4em;
  color: transparent;
  -webkit-text-stroke: 2px #1aff00;
  line-height: 1.5em;
  text-transform: uppercase;
  cursor: default;
}
```
`color: transparent` removes any fill from the letters. `-webkit-text-stroke: 2px #1aff00` draws a 2px neon green (#1aff00) stroke outline around every letter. The result is hollow, outlined lettering — just the border of each character is visible, no fill.

**Both `<span>` elements:**
```css
h2 span {
  display: inline-flex;
  transition: 0.5s ease-in-out;
  filter: drop-shadow(0 0 15px #1aff00);
}
```
`transition: 0.5s ease-in-out` means any CSS property change on these spans animates smoothly over 0.5 seconds. `filter: drop-shadow(0 0 15px #1aff00)` adds a 15px neon green glow around every character — giving the text a lit-up, glowing appearance on the dark background.

**The hidden state — even spans collapsed:**
```css
h2 span:nth-child(even) {
  letter-spacing: -1em;
  overflow: hidden;
}
```
This is the core of the entire effect. By default, the second span (the rest of the word after the initial) has `letter-spacing: -1em`. Since each character in a 4em font is approximately 1em wide, `-1em` letter-spacing pushes every character of the word directly underneath the one before it — collapsing the visible width to effectively zero. `overflow: hidden` then clips anything that overflows, making the hidden word completely invisible. The word is technically present in the DOM and takes up no visual space.

**The revealed state — hover expands both this line and all lines below:**
```css
h2:hover ~ h2 span:nth-child(even),
h2:hover span:nth-child(even) {
  letter-spacing: 0;
  color: #1aff00;
}
```
Two selectors work together:

- `h2:hover span:nth-child(even)` — targets the even span inside the hovered `<h2>` itself, expanding its own word.
- `h2:hover ~ h2 span:nth-child(even)` — the `~` is the **general sibling combinator**. It selects every `<h2>` that comes after the hovered `<h2>` in DOM order. Because the flex body is `column-reverse`, DOM-later elements appear visually higher on screen — so hovering a line expands that line and everything above it visually (which is everything that comes after it in the DOM).

When hover fires, `letter-spacing` jumps from `-1em` back to `0`, and the `transition: 0.5s ease-in-out` on the span animates this change smoothly — making the word appear to slide or expand outward from the initial letter. `color: #1aff00` simultaneously fills the previously transparent text with solid neon green, making the revealed word fully opaque while the initial letter remains just an outline.

**Summary of the state change on hover:**

| Property | Default (hidden) | On hover (revealed) |
|---|---|---|
| `letter-spacing` | `-1em` (collapsed) | `0` (full width) |
| `overflow` | `hidden` (clipped) | `hidden` (still set, but no overflow occurs) |
| `color` | `transparent` (only stroke visible) | `#1aff00` (solid neon green fill) |
| Transition | — | `0.5s ease-in-out` on both changes |

---

## Tech Stack

| Technology | Role |
|---|---|
| HTML5 | 5 `<h2>` elements, each containing 2 `<span>` tags — initial letter and remaining word |
| CSS3 | `letter-spacing` collapse/expand trick, `overflow: hidden`, `-webkit-text-stroke`, `flex-direction: column-reverse`, general sibling combinator (`~`), `drop-shadow` filter, `transition` |

No JavaScript. No external fonts. No CDN. The entire interaction is 37 lines of CSS.

---

## Project Structure

```
Name-Animation/
├── Name-animation.html    # 5 h2 elements, each with 2 spans — initial letter + rest of word
└── Name-animation.css     # All layout, hover logic, letter-spacing trick, and neon styling
```

---

## How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/tripathipawan/Name-Animation.git
   ```
2. Open `Name-animation.html` directly in any modern browser — hover over any line to see the effect. No server or build step needed.

---

## Repository

[https://github.com/tripathipawan/Name-Animation](https://github.com/tripathipawan/Name-Animation)
