# DOM (Document Object Model) — Simple Understanding

## One-line idea

The DOM is how JavaScript "sees" and changes your web page.

## The analogy — think of a house

- Your HTML file is the **blueprint**.
- The browser reads that blueprint and builds an actual **house** in memory. That house is the DOM.
- JavaScript is the **handyman** who can walk into the house, repaint walls, add a room, or remove a door — while people are living in it. You never touch the blueprint again; you work on the live house.

## Why it matters

- HTML alone is static — it can't react to clicks or change.
- The DOM turns your page into **objects** you can read and modify with code. That's what makes pages interactive.

## The mental picture: a family tree

```
document
  └── html
       ├── head
       │     └── title
       └── body
             ├── h1
             └── p
```

- `document` is the great-grandparent at the top.
- Everything below is a **node** (an element, or just text).
- You use selectors to "find a person" in that tree, then change them.

## The 3 things you do 99% of the time

1. **Find** it → `document.querySelector("#myButton")`
2. **Change** it → `element.textContent = "Hi";`
3. **React** to it → `element.addEventListener("click", ...)`

## Simple article / reference

### What the DOM actually is

- The DOM is a **programming interface for web documents** — a live, in-memory, object-oriented **tree** the browser builds after parsing your HTML.
- It is **not part of JavaScript**; it's a **Web API** that JavaScript (and other languages) use to talk to the page.

### The tree mental model

- Every part of the page is a **node**: element nodes (`<p>`, `<div>`), text nodes, attribute nodes, comment nodes.
- `document` is the root → `Element` inherits from `Node` → specific elements get specialized interfaces (`HTMLTableElement`, etc.).
- `window` ≈ the browser/tab; `document` ≈ the page.

### What you can do with it

- **Find:** `querySelector()` / `querySelectorAll()` (CSS selectors), `getElementById`, `getElementsByTagName`.
- **Read/write:** `textContent` (text only), `innerHTML`, attributes via `.getAttribute()`.
- **Structure:** `createElement()`, `createTextNode()`, `appendChild()`, `removeChild()`.
- **React to events:** `addEventListener()`.

### Event propagation (the part that trips people up)

- Events flow in three phases: **capture** (down from root) → **target** → **bubble** (back up). Listeners default to the bubble phase.
- `event.stopPropagation()` halts that flow — which is why a click on a child sometimes never reaches the parent handler that "should" fire.
- The real DOM tree **preserves whitespace**, so `childNodes` often includes text nodes you didn't expect — use `children` when you only want elements.

## Simple takeaways

- **DOM = a live, editable model of your page**, built from HTML but separate from it.
- **Nodes, not just tags** — text and attributes are nodes too.
- **It's not JavaScript** — it's an API the browser gives JavaScript to talk to the page.
- **Dynamic ≠ source** — what you see in DevTools is the DOM *after* JS changed things, not your original HTML.

## Why this matters for Playwright

- Every `page.locator()` / selector strategy is you traversing this tree.
- Locators find nodes in the DOM, so understanding **find → change → react** is exactly the skill your tests will use.
- Playwright sees the same DOM the browser's JavaScript sees.
