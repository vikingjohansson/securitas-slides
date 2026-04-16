# HTML Presentation Template

Reference architecture for generating Securitas-branded slide presentations. This file is the source of truth for both structure and brand behavior.

## Securitas Brand Defaults

Use these defaults unless the user explicitly asks to break brand.

### Brand Intent

- Calm authority
- Operational clarity
- Premium restraint
- Direct business communication over visual novelty

### Visual Anchors

- Deep marine background, typically kept flat on standard content slides
- Crisp white typography with strong left alignment
- Red used sparingly as a signal color
- Three-dot marker at the top-right as the default content-slide brand cue
- Do not add a top-left Securitas wordmark or full logotype on standard content slides unless the user specifically asks for that treatment
- Thin dividers and quiet panels used only when they help the content breathe

### Approved Header Asset

Use this three-dot asset when remote assets are acceptable:

`https://brand.securitas.com/wp-content/uploads/2021/02/SEC_Logotype_3-1536x768.png`

On standard content slides, this should usually be the only persistent corner brand element.

If the remote asset is undesirable, download it into a local `assets/` folder and update the `src`. If it fails to load, a simple CSS-dot fallback is acceptable.

### Palette

```css
:root {
  --bg-primary: #062b3c;
  --bg-secondary: #041f2c;
  --surface: #0d3749;
  --text-primary: #f7fbfd;
  --text-secondary: #cad7de;
  --accent: #ff3657;
  --accent-dark: #d72646;
  --accent-soft: rgba(255, 54, 87, 0.18);
  --line: rgba(255, 255, 255, 0.16);
}
```

### Typography

- Prefer `Securitas Pro Office` only if licensed webfont files are actually available
- Safe web fallback: `Manrope`
- Keep hierarchy simple: eyebrow, strong heading, restrained body
- Avoid condensed, ornamental, or playful type treatments

### Layout Rules

- Default to 16:9 slides with large negative space
- Favor left-aligned composition for standard slides, but allow centered title-slide compositions when the reference calls for it
- On standard one-column slides, let the working text area span broadly across the slide instead of defaulting to a narrow left rail
- Use two-column layouts for explanatory slides, and let both columns occupy most of the slide width with a clear gutter
- Treat screenshots as utility panels, not decorative hero images
- Reserve red circular callouts for special cases rather than using them as a default slide ingredient
- For closing slides, reduce the composition to a centered brand sign-off and strip away interface-like chrome
- On standard content slides, prefer the enlarged top-right dot asset as the only corner branding

### Motion Rules

- Use subtle fast reveals around 180-320ms
- Prefer fade plus slight upward motion
- Avoid bounce, theatrical zooms, glow pulses, particles, parallax, and playful effects
- Preserve `prefers-reduced-motion`

### Off-Brand Signals To Avoid

- Pastel or bright non-brand canvases
- Neon glows
- Glassmorphism
- Consumer-app rounded UI language
- Centered one-word hero slides unless explicitly requested
- generic section-kicker labels above every content-slide heading
- automatic top-left Securitas wordmarks on standard content slides

## Base HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Presentation Title</title>

    <!-- Fonts: use licensed brand fonts when available; otherwise a clean web font -->
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;500;700;800&display=swap"
    />

    <style>
      /* ===========================================
           CSS CUSTOM PROPERTIES (SECURITAS DEFAULTS)
           Adjust only within brand guardrails.
           =========================================== */
      :root {
        /* Colors — Securitas brand palette */
        --bg-primary: #062b3c;
        --bg-secondary: #041f2c;
        --surface: #0d3749;
        --text-primary: #f7fbfd;
        --text-secondary: #cad7de;
        --accent: #ff3657;
        --accent-dark: #d72646;
        --accent-soft: rgba(255, 54, 87, 0.18);
        --line: rgba(255, 255, 255, 0.16);

        /* Typography — MUST use clamp() */
        --font-display: "Manrope", sans-serif;
        --font-body: "Manrope", sans-serif;
        --title-size: clamp(2rem, 6vw, 5rem);
        --subtitle-size: clamp(0.875rem, 2vw, 1.25rem);
        --body-size: clamp(0.75rem, 1.2vw, 1rem);

        /* Spacing — MUST use clamp() */
        --slide-padding: clamp(1.5rem, 4vw, 4rem);
        --content-gap: clamp(1rem, 2vw, 2rem);

        /* Animation */
        --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
        --duration-normal: 240ms;
      }

      /* ===========================================
           BASE STYLES
           =========================================== */
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      /* --- PASTE viewport-base.css CONTENTS HERE --- */

      /* ===========================================
           ANIMATIONS
           Trigger via .visible class (added by JS on scroll)
           =========================================== */
      .reveal {
        opacity: 0;
        transform: translateY(16px);
        transition:
          opacity var(--duration-normal) var(--ease-out-expo),
          transform var(--duration-normal) var(--ease-out-expo);
      }

      .slide.visible .reveal {
        opacity: 1;
        transform: translateY(0);
      }

      /* ===========================================
           BRAND CHROME (SECURITAS DEFAULT SHELL)
           =========================================== */
      body {
        background: var(--bg-primary);
        color: var(--text-primary);
        font-family: var(--font-body);
      }

      .brand-header {
        position: fixed;
        top: clamp(1rem, 2vw, 1.75rem);
        right: clamp(1rem, 2vw, 1.75rem);
        display: flex;
        align-items: center;
        justify-content: flex-end;
        z-index: 20;
        pointer-events: none;
      }

      .brand-dots-image {
        width: auto;
        height: clamp(0.9rem, 1.35vw, 1.25rem);
        display: block;
      }

      .brand-dots {
        display: inline-flex;
        gap: clamp(0.3rem, 0.5vw, 0.5rem);
      }

      .brand-dots[hidden] {
        display: none;
      }

      .brand-dots span {
        width: clamp(0.5rem, 0.8vw, 0.8rem);
        height: clamp(0.5rem, 0.8vw, 0.8rem);
        border-radius: 999px;
        background: var(--accent);
      }

      .brand-wordmark {
        position: fixed;
        top: clamp(1rem, 2vw, 1.75rem);
        left: clamp(1rem, 2vw, 1.75rem);
        z-index: 20;
        font-size: clamp(0.7rem, 0.95vw, 0.9rem);
        font-weight: 700;
        letter-spacing: -0.01em;
      }

      .closing-slide {
        background: var(--bg-primary);
      }

      .closing-logo {
        width: min(34rem, 52vw);
        max-width: 100%;
        height: auto;
        max-height: min(38vh, 320px);
      }

      /* Stagger children for sequential reveal */
      .reveal:nth-child(1) {
        transition-delay: 60ms;
      }
      .reveal:nth-child(2) {
        transition-delay: 120ms;
      }
      .reveal:nth-child(3) {
        transition-delay: 180ms;
      }
      .reveal:nth-child(4) {
        transition-delay: 240ms;
      }

      /* Add deck-specific layouts here, but stay inside the Securitas system. */
    </style>
  </head>
  <body>
    <script id="deck-data" type="application/json">
      {
        "title": {
          "title": "Presentation Title",
          "subtitle": "Subtitle or author"
        },
        "content": {
          "header": "Slide Title",
          "body": "Content..."
        }
      }
    </script>

    <div class="brand-wordmark" aria-label="Presentation branding">Securitas</div>
    <header class="brand-header" aria-label="Presentation branding">
      <img
        class="brand-dots-image"
        src="https://brand.securitas.com/wp-content/uploads/2021/02/SEC_Logotype_3-1536x768.png"
        alt=""
        aria-hidden="true"
      />
      <div class="brand-dots" aria-hidden="true" hidden>
        <span></span>
        <span></span>
        <span></span>
      </div>
    </header>

    <!-- Optional: Progress bar -->
    <div class="progress-bar"></div>

    <!-- Optional: Navigation dots -->
    <nav class="nav-dots"><!-- Generated by JS --></nav>

    <!-- Slides -->
    <section class="slide title-slide">
      <div class="slide-content">
        <h1 class="reveal" data-field="title.title">Presentation Title</h1>
        <p class="reveal" data-field="title.subtitle">Subtitle or author</p>
      </div>
    </section>

    <section class="slide">
      <div class="slide-content">
        <h2 class="reveal" data-field="content.header">Slide Title</h2>
        <p class="reveal" data-field="content.body">Content...</p>
      </div>
    </section>

    <section class="slide closing-slide">
      <div class="slide-content" style="align-items:center; justify-content:center;">
        <img
          class="closing-logo reveal"
          src="https://brand.securitas.com/wp-content/themes/securitas-brand-portal/assets/Securitas_Logotype.svg"
          alt="Securitas"
        />
      </div>
    </section>

    <!-- More slides... -->

    <script>
      function getByPath(object, path) {
        return path.split(".").reduce((value, key) => value?.[key], object);
      }

      function populateDeckContent() {
        const contentNode = document.getElementById("deck-data");
        if (!contentNode) return;

        const deckData = JSON.parse(contentNode.textContent);

        document.querySelectorAll("[data-field]").forEach((element) => {
          const value = getByPath(deckData, element.dataset.field);
          if (typeof value === "string") {
            element.textContent = value;
          }
        });
      }

      /* ===========================================
           SLIDE PRESENTATION CONTROLLER
           =========================================== */
      class SlidePresentation {
        constructor() {
          this.slides = document.querySelectorAll(".slide");
          this.currentSlide = 0;
          this.setupIntersectionObserver();
          this.setupKeyboardNav();
          this.setupTouchNav();
          this.setupProgressBar();
          this.setupNavDots();
        }

        setupIntersectionObserver() {
          // Add .visible class when slides enter viewport
          // Triggers CSS animations efficiently
        }

        setupKeyboardNav() {
          // Arrow keys, Space, Page Up/Down
        }

        setupTouchNav() {
          // Touch/swipe support for mobile
        }

        setupProgressBar() {
          // Update progress bar on scroll
        }

        setupNavDots() {
          // IMPORTANT: Always clear before building — if outerHTML was
          // captured while dots were rendered, re-opening the file would
          // append a duplicate set on top of the existing ones.
          this.navDotsContainer.innerHTML = "";
          // Generate and manage navigation dots
        }
      }

      populateDeckContent();
      new SlidePresentation();

      /* Keep a graceful fallback if the remote dots asset is unavailable. */
      document.querySelectorAll(".brand-dots-image").forEach((dotsImage) => {
        dotsImage.addEventListener("error", () => {
          dotsImage.hidden = true;
          document.querySelector(".brand-dots")?.removeAttribute("hidden");
        });
      });
    </script>
  </body>
</html>
```

## Editable Content Default

By default, generated decks should keep editable copy in one inline JSON block near the top of the file instead of scattering text throughout the slide markup.

Preferred pattern:

- use `<script id="deck-data" type="application/json">`
- group content by slide or section
- bind values into the DOM using `data-field` attributes
- keep the HTML body primarily structural so users can edit copy without touching layout code

Prefer this inline JSON approach over external JSON files for ordinary local decks because direct browser `fetch()` can fail from `file://` URLs.

## Required JavaScript Features

For branded corporate decks, keep the JavaScript behavior polished but invisible. Navigation and progress UI should feel precise and dependable, not playful.

Every presentation must include:

1. **SlidePresentation Class** — Main controller with:
   - Keyboard navigation (arrows, space, page up/down)
   - Touch/swipe support
   - Mouse wheel navigation
   - Progress bar updates
   - Navigation dots

2. **Intersection Observer** — For scroll-triggered animations:
   - Add `.visible` class when slides enter viewport
   - Trigger CSS transitions efficiently

3. **Optional Enhancements** (brand-safe only):
   - Counter animations for metrics
   - Gentle panel emphasis on hover
   - Small state transitions for navigation or interactive case cards

4. **Inline Editing** (only if user opted in during Phase 1 — skip entirely if they said No):
   - Edit toggle button (hidden by default, revealed via hover hotzone or `E` key)
   - Auto-save to localStorage
   - Export/save file functionality
   - See "Inline Editing Implementation" section below

## Inline Editing Implementation (Opt-In Only)

**If the user chose "No" for inline editing in Phase 1, do NOT generate any edit-related HTML, CSS, or JS.**

**Do NOT use CSS `~` sibling selector for hover-based show/hide.** The CSS-only approach (`edit-hotzone:hover ~ .edit-toggle`) fails because `pointer-events: none` on the toggle button breaks the hover chain: user hovers hotzone -> button becomes visible -> mouse moves toward button -> leaves hotzone -> button disappears before click.

**Required approach: JS-based hover with 400ms delay timeout.**

HTML:

```html
<div class="edit-hotzone"></div>
<button class="edit-toggle" id="editToggle" title="Edit mode (E)">✏️</button>
```

CSS (visibility controlled by JS classes only):

```css
/* Do NOT use CSS ~ sibling selector for this!
   pointer-events: none breaks the hover chain.
   Must use JS with delay timeout. */
.edit-hotzone {
  position: fixed;
  top: 0;
  left: 0;
  width: 80px;
  height: 80px;
  z-index: 10000;
  cursor: pointer;
}
.edit-toggle {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  z-index: 10001;
}
.edit-toggle.show,
.edit-toggle.active {
  opacity: 1;
  pointer-events: auto;
}
```

JS (three interaction methods):

```javascript
// 1. Click handler on the toggle button
document.getElementById("editToggle").addEventListener("click", () => {
  editor.toggleEditMode();
});

// 2. Hotzone hover with 400ms grace period
const hotzone = document.querySelector(".edit-hotzone");
const editToggle = document.getElementById("editToggle");
let hideTimeout = null;

hotzone.addEventListener("mouseenter", () => {
  clearTimeout(hideTimeout);
  editToggle.classList.add("show");
});
hotzone.addEventListener("mouseleave", () => {
  hideTimeout = setTimeout(() => {
    if (!editor.isActive) editToggle.classList.remove("show");
  }, 400);
});
editToggle.addEventListener("mouseenter", () => {
  clearTimeout(hideTimeout);
});
editToggle.addEventListener("mouseleave", () => {
  hideTimeout = setTimeout(() => {
    if (!editor.isActive) editToggle.classList.remove("show");
  }, 400);
});

// 3. Hotzone direct click
hotzone.addEventListener("click", () => {
  editor.toggleEditMode();
});

// 4. Keyboard shortcut (E key, skip when editing text)
document.addEventListener("keydown", (e) => {
  if (
    (e.key === "e" || e.key === "E") &&
    !e.target.getAttribute("contenteditable")
  ) {
    editor.toggleEditMode();
  }
});
```

**CRITICAL: `exportFile()` must strip edit state before capturing outerHTML.**

When the user presses Ctrl+S in edit mode, `document.documentElement.outerHTML` captures the live DOM —
including `body.edit-active`, `contenteditable="true"` on every text element, and `.active`/`.show` classes on
the toggle button and banner. Anyone opening the saved file sees dashed outlines, a checkmark button, and an
edit banner, as if permanently stuck in edit mode.

Always implement `exportFile()` like this:

```javascript
exportFile() {
    // Temporarily strip edit state so the saved file opens cleanly
    const editableEls = Array.from(document.querySelectorAll('[contenteditable]'));
    editableEls.forEach(el => el.removeAttribute('contenteditable'));
    document.body.classList.remove('edit-active');

    // Also strip UI classes from toggle button and banner
    const editToggle = document.getElementById('editToggle');
    const editBanner = document.querySelector('.edit-banner');
    editToggle?.classList.remove('active', 'show');
    editBanner?.classList.remove('active', 'show');

    const html = '<!DOCTYPE html>\n' + document.documentElement.outerHTML;

    // Restore edit state so the user can keep editing
    document.body.classList.add('edit-active');
    editableEls.forEach(el => el.setAttribute('contenteditable', 'true'));
    editToggle?.classList.add('active');
    editBanner?.classList.add('active');

    const blob = new Blob([html], { type: 'text/html' });
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = 'presentation.html';
    a.click();
    URL.revokeObjectURL(a.href);
}
```

## Image Pipeline (Skip If No Images)

If user chose "No images" in Phase 1, skip this entirely. If images were provided, process them before generating HTML.

**Dependency:** `pip install Pillow`

### Image Processing

```python
from PIL import Image, ImageDraw

# Circular crop only if a provided asset truly needs that treatment
def crop_circle(input_path, output_path):
    img = Image.open(input_path).convert('RGBA')
    w, h = img.size
    size = min(w, h)
    left, top = (w - size) // 2, (h - size) // 2
    img = img.crop((left, top, left + size, top + size))
    mask = Image.new('L', (size, size), 0)
    ImageDraw.Draw(mask).ellipse([0, 0, size, size], fill=255)
    img.putalpha(mask)
    img.save(output_path, 'PNG')

# Resize (for oversized images that inflate HTML)
def resize_max(input_path, output_path, max_dim=1200):
    img = Image.open(input_path)
    img.thumbnail((max_dim, max_dim), Image.LANCZOS)
    img.save(output_path, quality=85)
```

| Situation                        | Operation                     |
| -------------------------------- | ----------------------------- |
| Square logo on rounded aesthetic | `crop_circle()`               |
| Image > 1MB                      | `resize_max(max_dim=1200)`    |
| Wrong aspect ratio               | Manual crop with `img.crop()` |

Save processed images with `_processed` suffix. Never overwrite originals.

### Image Placement

**Use direct file paths** (not base64) — presentations are viewed locally:

```html
<img src="assets/logo_round.png" alt="Logo" class="slide-image logo" />
<img
  src="assets/screenshot.png"
  alt="Screenshot"
  class="slide-image screenshot"
/>
```

```css
.slide-image {
  max-width: 100%;
  max-height: min(50vh, 400px);
  object-fit: contain;
  border-radius: 8px;
}
.slide-image.screenshot {
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}
.slide-image.logo {
  max-height: min(30vh, 200px);
}
```

**Adapt border/shadow colors to the Securitas palette.** Never repeat the same image on multiple slides unless it serves a clear communication purpose.

**Placement patterns:** Screenshots in two-column layouts with text. Diagrams and utility visuals inside framed panels. Avoid full-bleed decorative imagery and avoid logo-led hero compositions by default.

---

## Code Quality

**Comments:** Every section needs clear comments explaining what it does and how to modify it.

**Accessibility:**

- Semantic HTML (`<section>`, `<nav>`, `<main>`)
- Keyboard navigation works fully
- ARIA labels where needed
- `prefers-reduced-motion` support (included in viewport-base.css)

## File Structure

Single presentations:

```
presentation.html    # Self-contained, all CSS/JS inline
assets/              # Images only, if any
```

Multiple presentations in one project:

```
[name].html
[name]-assets/
```
