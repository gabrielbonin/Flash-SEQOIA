# Flash Courier / Sequoia — Project Specification

> **Status:** Production-ready 100% static site. No build tools, no npm dependencies. HTML + CSS + vanilla JavaScript.

---

## Project Overview

**Flash Courier** (branded as **Sequoia** for Investor Relations) is an institutional website for a Brazilian logistics company. The project consists of two fully static pages:

- **index.html** — Main landing page with company story, operations, partnerships, recognition
- **investidores.html** — Investor Relations portal with stock quotes, financials, governance, CVM filings

**Key characteristics:**
- 100% frontend (HTML + CSS + vanilla ES6+ JavaScript)
- No build tools, transpilers, or npm dependencies
- Only external dependency: **Google Fonts** (Inter, JetBrains Mono)
- Immediately deployable to any static host (S3, Netlify, Vercel, Nginx, Apache)
- ~4,847 total lines of code (~28 MB with assets)
- Responsive design (mobile-first, breakpoints at 880px, 780px, etc.)
- Bilingual support (Portuguese/English on investidores.html only)

---

## Architecture & File Structure

```
Flash-SEQOIA/
├── index.html                              (Landing page — 2,276 lines)
├── investidores.html                       (RI page — 2,571 lines)
├── README.md                               (Deployment & integration points)
├── assets/                                 (~28 MB total)
│   ├── Flash_Modelo1_didatico_compacto.mp4 (~15 MB hero video, MP4)
│   ├── flash-logo-transparent.png
│   ├── logo-v1-oval.png
│   ├── logo-v2-mid.png
│   ├── sef-2026.png                        (ABF award seal)
│   ├── sef-esg-2026.png                    (ABF ESG award seal)
│   ├── sorter-*.jpeg                       (Hub facility photos)
│   ├── hub-*.jpg
│   └── [10+ additional images]
└── CLAUDE.md                               (This file)
```

**Page Structure Pattern:**

1. `<head>` — Meta tags, font preconnects, inline `<style>`
2. Multiple `<section>` elements (feature by feature)
3. Single/multiple `<script>` blocks (all inline, no external JS files)
4. Data attributes for JavaScript hooks and i18n

---

## Technology Stack

### Frontend

| Layer | Tech | Details |
|-------|------|---------|
| **HTML** | HTML5 | Semantic structure, SVG, form inputs |
| **CSS** | CSS3 | Custom properties, Flexbox, Grid, animations, transforms |
| **JavaScript** | ES6+ vanilla | No libraries. Uses: Fetch API, IntersectionObserver, requestAnimationFrame, Dataset API |
| **Fonts** | Google Fonts | Inter (wght 300–800), JetBrains Mono (wght 400–600) |
| **Media** | MP4 + PNG/JPEG | Video with autoplay/muted/loop, responsive images |
| **Build** | None | Serve as-is; no webpack, babel, postcss, or npm |

### Browser Support

- Chrome 58+
- Safari 12+
- Firefox 55+
- Edge 79+
- (No polyfills needed; uses native ES6+ and modern APIs)

### External Dependencies

Only **Google Fonts** via CDN:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

---

## Design System & CSS Tokens

### Color Palette

**index.html:**
```css
:root {
  --navy: #1B2470;              /* Primary brand */
  --navy-deep: #0F1547;         /* Darker navy for sections */
  --orange: #F37021;            /* Primary accent, CTAs */
  --orange-dark: #D85A11;       /* Hover state */
  --white: #FFFFFF;
  --off-white: #FBFBFD;
  --gray-text: #6E6E73;
  --gray-darker: #1D1D1F;
  --gray-bg: #F5F5F7;
  --gray-line: #D2D2D7;
}
```

**investidores.html (Extended):**
```css
:root {
  --navy: #1b2470;
  --navy-deep: #0f1547;
  --orange: #f37021;
  --orange-dark: #d85a11;
  --orange-light: #ffab6b;      /* Light accent for text */
  --bg: #fbfbfd;
  --bg-soft: #f5f5f7;
  --ink: #1d1d1f;               /* Main text */
  --ink-soft: #3a3a3d;          /* Secondary text */
  --muted: #6e6e73;             /* Tertiary text */
  --muted-2: #86868b;
  --line: #e5e5ea;              /* Borders */
  --line-soft: #efeff2;
  --green: #16a34a;             /* Status positive */
  --red: #dc2626;               /* Status negative */
}
```

### Layout Tokens

```css
--max: 1240px;            /* Wide container */
--max-narrow: 880px;      /* Narrow container */
--pad-x: 24px;            /* Horizontal padding */
```

### Typography

**Fonts:**
- **Inter** — Body text, UI elements (wght 300, 400, 500, 600, 700)
- **JetBrains Mono** — Code-like text, numbers, labels (wght 400, 500, 600)

**Font Sizes (clamp for fluid scaling):**
```css
/* Desktop 108px, mobile 48px, scales smoothly */
h1 { font-size: clamp(48px, 7.2vw, 108px); }

/* Desktop 22px, mobile 18px */
.lede { font-size: clamp(18px, 1.6vw, 22px); }
```

---

## Key Features & Sections

### index.html — Landing Page

#### 1. **Navigation** (Sticky)
- Logo + menu items (hidden on mobile < 880px)
- Login link + CTA button
- Glassmorphism backdrop: `backdrop-filter: saturate(180%) blur(20px)`

#### 2. **Hero Carousel**
- 2 main slides (extensible)
- Slide 1: Hero image/video + animated text
- Slide 2: "A Trajetória" (33-year timeline)
- Dot navigation + arrow buttons
- Auto-advance: 7s per slide
- Pause on hover: animated progress bar

#### 3. **Tracking Section** (#track)
- Shipment code lookup input + button
- **3 states:**
  1. Loading spinner (simulated 700ms delay)
  2. 6-stage journey visualization
  3. Not-found message
- **Test code:** `FC8472193BR` (demo)
- **Stages:** Gráfica → Coleta → Hub → Transferência → Distribuição → Entrega
- Animated package icon + progress bar

#### 4. **Stats Band**
- 5-column KPI grid
- Animated counters on scroll (cubic easing, ~1.6s)
- Responsive: 5 cols → 3 cols (1180px) → 2 cols (780px)

#### 5. **About Poster**
- Configurable background (dark/navy/gray/white)
- Logo evolution: 3 versions with hover effects
- Timeline marquee: infinite scrolling milestones
- Optical sizing adjustments per variant

#### 6. **Hub Tour Carousel**
- 3+ images with captions
- Cloning strategy for seamless infinite loop
- Active slide wider; sides narrower
- Auto-advance: 6.5s
- Smooth easing: cubic-bezier(0.4, 0, 0.2, 1)

#### 7. **Brazil Network Map**
- Dynamically generated SVG (state boundaries)
- Color-coded by franchise tier (t1, t2, t3, t4)
- **Interactive tooltips** on hover:
  - State name + UF code
  - Franchise count (orange-highlighted)
  - Cities/total coverage
  - Population percentage
- Offset labels for micro-states (DF, AL, SE)

#### 8. **ABF Recognition Section**
- Award seals (2026 SEF, SEF ESG)
- Tier ladder visualization (Pleno → Sênior → Máster → Mega)
- 17-seal timeline (2006–2026)
- Reveal animations on scroll

#### 9. **Crossbelt Sorter Cards**
- 21 payment/banking providers
- Animated "conveyor belt" motion
- Random rotation per card (CSS variable `--r`)
- Custom gradients per brand
- Seamless infinite loop

#### 10. **Quote Simulator**
- 3 groups: Kind (doc/card/parcel) × Speed (eco/std/exp)
- Dynamic price calculation: `base[kind] * mult[speed]`
- ETA display per speed tier
- Pill button UI with active states

#### 11. **Footer**
- Contact info
- Navigation links
- Copyright

---

### investidores.html — Investor Relations Portal

#### 1. **Navigation**
- Sticky top bar with logo + ticker mini widget
- Language toggle (PT / EN)
- Sub-nav tabs: Cotações, Kit, Comunicados, Governança, etc.

#### 2. **Hero**
- Title with gradient text effect
- Emphasize "em progresso"
- CTAs (dark + ghost buttons)

#### 3. **Stock Section** (#cotacao)
- Quote display (SEQL3 · B3)
- Metadata: segment, ISIN, sector, auditor
- Stock card with ticker-style display (SVG chart, stats grid)
- Disclaimer footer

#### 4. **Investor Kit** (#kit)
- 3 downloadable documents
- ITR (Quarterly Information)
- Earnings Release
- Reference Form
- Featured card styling for ITR

#### 5. **Updates/News** (#updates)
- Material facts, announcements, results feed
- Tagged categories (color-coded badges)
- Scrollable list

#### 6. **Upcoming Events** (#eventos)
- Event cards: date, title, description, location, time
- 3-column grid (responsive to 1 col)

#### 7. **Governance** (#governanca)
- Board of Directors: 5 members
- Statutory Board (Diretoria): 2 executives
- **Per member:**
  - Avatar (initials in gradient circle)
  - Name, role, term dates
  - Detailed biography
  - Mandate/term info

#### 8. **CVM Filings** (#documentos)
- 9 document categories
- Material Facts (42 docs)
- Announcements (128 docs)
- Reference Form, Bylaws, Policies, Meetings, Shareholding, Trading, Governance Report
- **Per doc:** name, metadata (count/date), click to open

#### 9. **Contact IR** (#contato)
- Dark section with white text
- Grid layout (left: content, right: contact info)
- **Info rows:**
  - Email: ri@sequoialog.com.br
  - Address: Av. Yojiro Takaoka, 4.384, Alphaville, SP
  - Whistleblower channel: contatoseguro.com.br/grupomove3sequoia
  - Shareholder portal link
- CTA buttons: "Send message" (orange) + "Mailing list" (ghost)

#### 10. **Footer**
- Brand description + links
- Investors column
- Governance column
- Company column
- Bottom: copyright + legal links

---

## Internationalization (i18n)

**Scope:** investidores.html only (index.html is Portuguese-only)

### System Architecture

```javascript
const i18n = {
  pt: {},    // Auto-populated from DOM on first load
  en: {      // ~140+ English translations
    brand: "Sequoia",
    nav_site: "← Corporate site",
    hero_t1: "Transformation",
    hero_t2: "in progress.",
    // ... 130+ more keys ...
  }
};
```

### Implementation

1. **HTML markup** uses `data-i18n="key"` on translatable elements
2. **Initialization:** Page loads with Portuguese text
3. **On script execution:** Capture all PT strings into `i18n.pt` from DOM
4. **Language toggle:** Two buttons (PT, EN) with `data-lang` attribute
5. **Switch function:** `applyLang(lang)` updates:
   - `document.documentElement.lang` (pt-BR or en)
   - All `[data-i18n]` element `.innerHTML`
   - Button active states (`.on` class)

### Translation Coverage

- Navigation (6 items)
- Subnavigation (8 sections)
- Hero (4 keys)
- Stock quote (12 keys)
- Investor Kit (8 keys)
- Highlights (6 keys)
- Updates (5 keys)
- Governance (20 keys — board names, roles, bios)
- CVM Filings (13 keys)
- Contact (7 keys)
- Footer (12 keys)

**Total:** ~140 translation keys for full PT ↔ EN support

---

## JavaScript Functionality

### Reveal on Scroll (Both Pages)

```javascript
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('in');
      io.unobserve(e.target);
    }
  });
}, { threshold: 0.12, rootMargin: '0px 0px -60px 0px' });

document.querySelectorAll('.reveal').forEach(el => io.observe(el));
```

- **Trigger:** When 12% of element is visible
- **Effect:** Adds `.in` class → CSS transition from `opacity: 0; transform: translateY(20px)` to full visibility
- **Performance:** Unobserves after firing (one-time per element)

### Counter Animation (index.html)

```javascript
const cIo = new IntersectionObserver((entries) => {
  // Animates from 0 to data-to value
  // Duration: 1600ms
  // Easing: cubic out
  // Format: pt-BR locale (thousand separators)
}, { threshold: 0.4 });
```

### Carousel: Hero (index.html)

- **Manual:** Dot navigation + arrow buttons
- **Auto-advance:** 7 seconds per slide
- **Animation:** Smooth track translation via `requestAnimationFrame`
- **Pause on hover:** Progress bar animation pauses
- **Infinite loop:** Slides wrap around

### Carousel: Hub Tour (index.html)

- **Layout:** Active slide wider (62% desktop, 78% mobile), sides narrower
- **Infinite loop:** Clone slides before/after for seamless wrapping
- **Auto-advance:** 6.5 seconds
- **Easing:** cubic-in-out
- **Responsive:** Adjusts active/side widths at 880px breakpoint

### Tracking Feature (index.html #track)

**Demo Database:**
```javascript
const DB = {
  'FC8472193BR': {
    stage: 3,
    from: 'São Paulo, SP',
    to: 'Rio de Janeiro, RJ',
    eta: 'previsão TER, 14:00',
    status: 'Em transferência'
  }
};
```

**UI States:**
1. **Input + button** — Ready for user input
2. **Loading spinner** — 700ms simulated delay
3. **Result card** — Journey stages animate in sequence (450ms intervals)
4. **Not found message** — If code not in DB

**Test interaction:**
- Click `<code data-fill="FC8472193BR">` chips to autofill input
- Press Enter or click "Rastrear" button
- Watch stages animate: Gráfica → Coleta → Hub → Transferência → Distribuição → Entrega

### Quote Simulator (index.html)

```javascript
const state = { kind: 'doc', speed: 'std' };
const base = { doc: 14, card: 18, parcel: 32 };
const mult = { eco: 0.8, std: 1.4, exp: 2.4 };
// Price = base[kind] * mult[speed]
```

**Groups:**
- **Kind:** doc (document) / card / parcel
- **Speed:** eco (1-2 days) / std (1 day) / exp (same-day)

**Interaction:**
- Click pill buttons to toggle kind/speed
- Price updates dynamically
- ETA text changes per speed

### Map Tooltip (index.html)

```javascript
const tip = document.getElementById('map-tip');
document.querySelectorAll('.br-state').forEach(s => {
  s.addEventListener('mouseenter', (e) => {
    // Show tooltip with state data
    tip.style.left = e.pageX + 'px';
    tip.style.top = e.pageY + 'px';
    tip.innerHTML = `<strong>${state.name}</strong>...`;
  });
});
```

**Tooltip content:**
- State name + UF code
- Franchise count (orange-highlighted)
- Cities/total coverage
- Population percentage

### Crossbelt Cards (index.html)

- **Data:** 21 payment/banking providers with gradients
- **Animation:** Seamless conveyor belt loop
- **Rotation:** Random `-1` to `1` degree per card (CSS var `--r`)
- **Cloning:** Banks array doubled for infinite appearance
- **Performance:** `will-change: transform` for smooth animation

### SVG Map Generation (index.html)

```javascript
const NS = 'http://www.w3.org/2000/svg';
data.forEach(s => {
  const p = document.createElementNS(NS, 'path');
  p.setAttribute('d', s.d);  // SVG path data for state boundary
  p.setAttribute('class', `br-state ${tier(s.fr)}`);
  // Set dataset attributes for interactive tooltip
  p.dataset.fr = s.fr;
  p.dataset.name = s.name;
  p.dataset.uf = s.uf;
  sg.appendChild(p);
});
```

**Tiering:** `tier(franchises)` → CSS classes `t1`, `t2`, `t3`, `t4` with different colors

---

## Integration Points (Backend Required)

### 1. Tracking API (index.html #track)

**Current:** Simulated with hardcoded DB (`FC8472193BR` only)

**To integrate:**

Replace the simulated `go()` function with:
```javascript
async function go() {
  loading.classList.add('show');
  try {
    const response = await fetch('/api/track', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ code: inp.value })
    });
    if (!response.ok) throw new Error('Not found');
    const rec = await response.json();
    // Render result...
  } catch {
    notfound.classList.add('show');
  } finally {
    loading.classList.remove('show');
  }
}
```

**Expected API Response:**
```json
{
  "code": "FC8472193BR",
  "stage": 3,
  "from": "São Paulo, SP",
  "to": "Rio de Janeiro, RJ",
  "eta": "previsão TER, 14:00",
  "status": "Em transferência",
  "events": [
    {
      "date": "2026-05-19",
      "time": "10:30",
      "stage": 0,
      "location": "São Paulo, SP",
      "description": "Objeto enviado à gráfica"
    },
    // ... more events ...
  ]
}
```

### 2. Login Portal (index.html nav)

**Current:** Hash link to `#track` (placeholder)

**To integrate:**
- Link to real auth service
- Remove placeholder: update `href` in `.nav-actions a`
- Example: `href="https://portal.sequoia.com.br/login"`

### 3. Contact Form (index.html footer + investidores.html #contato)

**Current:** `mailto:` links (informational only)

**To integrate:**
1. Add form element with email, name, message fields
2. POST to `/api/contact` or third-party email service (SendGrid, Mailgun)
3. Add validation + error handling
4. Show success confirmation message
5. Optional: subscribe to mailing list

### 4. RI Data (investidores.html)

**Current:** Static example data (dated 2026)

**To integrate:**
- **Stock quote:** Fetch from B3 API or data provider (update every 15 min)
- **Board/directors:** Update text in PT/EN translations (quarterly)
- **CVM documents:** Fetch links from repository or CMS
- **Financial metrics:** Update quarterly (earnings, margins, etc.)
- **News feed:** Fetch from internal CMS or API (weekly updates)
- **Events calendar:** Sync with internal calendar service

### 5. Email Services (investidores.html #contato)

**Current:** `href="mailto:ri@sequoialog.com.br"`

**To integrate:**
- Form submission → email service (SendGrid, Mailgun, etc.)
- Mailing list signup → email provider (Klaviyo, ConvertKit, etc.)
- Confirmation email to user
- Notification email to RI team

### 6. Whistleblower Channel (investidores.html)

**Current:** Static external link to contatoseguro.com.br

**To integrate:**
- Update URL if service provider changes
- Ensure HTTPS and external link security
- Verify accessibility from Brazil (firewall/VPN requirements)

---

## Code Patterns & Conventions

### Module Pattern (IIFE)

All major features use self-executing function expressions for scope isolation:

```javascript
(function() {
  // Private scope
  const state = { /* ... */ };
  const selectors = { el: document.querySelector('...'), /* ... */ };
  
  function init() {
    // Setup event listeners, initial state
  }
  
  function cleanup() {
    // Remove listeners, unobserve, etc.
  }
  
  // Event listeners
  selectors.btn.addEventListener('click', handleClick);
  
  // Auto-run on DOM ready
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init, { once: true });
  } else {
    init();
  }
})();
```

**Benefits:**
- No global namespace pollution
- Private variables and functions
- Clear initialization logic
- Easy to add cleanup/teardown

### Data Attributes for Configuration

```html
<!-- Internationalization -->
<h1 data-i18n="hero_title">Transformação em curso</h1>

<!-- Counter animation target -->
<div data-to="1000" class="counter">0</div>

<!-- Interactive state -->
<div class="q-pill" data-group="kind" data-v="doc">Documento</div>

<!-- Analytics/labeling -->
<section data-screen-label="01 Hero">
```

### DOM Querying Patterns

```javascript
// Single element by ID
const inp = document.getElementById('track-input');

// Single element by selector
const btn = document.querySelector('.btn-primary');

// Multiple elements with forEach
document.querySelectorAll('.reveal').forEach(el => io.observe(el));

// Closest parent
const card = el.closest('.kit-card');

// Data attribute lookup
el.dataset.i18n  // Equivalent to: el.getAttribute('data-i18n')
el.dataset.to = '500';  // Sets data-to="500"
```

### Class Manipulation

```javascript
// Add class
el.classList.add('active');

// Remove class
el.classList.remove('show');

// Toggle class (conditional)
el.classList.toggle('on', condition);

// Check if has class
if (el.classList.contains('active')) { /* ... */ }
```

### Event Listeners

```javascript
// Direct binding (preferred for specific elements)
btn.addEventListener('click', handleClick);
inp.addEventListener('keydown', e => {
  if (e.key === 'Enter') handleSubmit();
});

// Cleanup (important for scroll listeners, observers)
io.unobserve(el);
el.removeEventListener('click', handler);
```

---

## CSS Animation Patterns

### Reveal on Scroll

```css
.reveal {
  opacity: 0;
  transform: translateY(20px);
  transition:
    opacity 0.8s cubic-bezier(0.2, 0.7, 0.3, 1),
    transform 0.8s cubic-bezier(0.2, 0.7, 0.3, 1);
}

.reveal.in {
  opacity: 1;
  transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
  .reveal {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

### Fade-up (Hero Titles)

```css
@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero-title {
  animation: fadeUp 1.2s 0.5s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}
```

### Carousel Dot Progress

```css
.hc-dot.active::after {
  animation: hcProg 7s linear forwards;
}

@keyframes hcProg {
  from { width: 0; }
  to { width: 100%; }
}

.hero-carousel.paused .hc-dot.active::after {
  animation-play-state: paused;
}
```

### SVG Stroke Draw

```css
.trj-ribbon-back {
  stroke-dasharray: 3000;
  stroke-dashoffset: 3000;
  animation: trjDraw 2.4s 0.35s cubic-bezier(0.6, 0.05, 0.2, 1) forwards;
}

@keyframes trjDraw {
  to { stroke-dashoffset: 0; }
}
```

### Infinite Marquee (Scroll)

```css
.timeline-track {
  animation: tl-marquee 72s linear infinite;
  will-change: transform;
}

.timeline-wrap:hover .timeline-track {
  animation-play-state: paused;
}

@keyframes tl-marquee {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}
```

---

## Performance & Optimization

### Current Optimizations

1. **No dependencies:** Zero npm overhead, instant load
2. **Single file:** One HTTP request (minus fonts + media)
3. **Inline CSS/JS:** Eliminates render-blocking resources
4. **Font optimization:**
   - `rel="preconnect"` for early DNS resolution
   - `display=swap` for FOUT (Flash of Unstyled Text)
5. **RequestAnimationFrame:** Smooth 60fps animations
6. **IntersectionObserver:** Native scroll detection (no polling)
7. **Unobserve pattern:** Remove elements from observer after triggering
8. **will-change:** Applied to frequently-animated elements (`.timeline-track`, carousel slides)
9. **Media queries:** Responsive design without JavaScript

### Potential Improvements

1. **Hero video:** ~15 MB MP4
   - Ensure server supports HTTP Range requests (standard)
   - Consider WebM/AV1 variant for smaller file size
   - Add `poster="image.jpg"` to `<video>` tag

2. **Asset images:** ~13 MB combined
   - Add `loading="lazy"` to off-screen images
   - Consider WebP with fallback to JPEG

3. **SVG map:** Dynamically generated from JSON
   - Consider pre-rendering if 50+ paths cause jank
   - Alternative: split into multiple SVGs

4. **Cache-busting:**
   - Append hash to filenames: `image.abc123.jpg`
   - Set far-future expires headers for assets

5. **Critical CSS:**
   - Extract above-fold styles (if splitting becomes necessary)
   - Inline critical path for faster FCP

### Recommended Server Headers

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'unsafe-inline';
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src https://fonts.gstatic.com;
  img-src 'self' data:;
  media-src 'self';

X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Cache-Control: max-age=0, must-revalidate

# For assets (images, fonts)
Cache-Control: public, max-age=31536000, immutable
```

---

## Responsive Design

### Breakpoints

| Breakpoint | Device | Changes |
|------------|--------|---------|
| Max-width: 1400px | Large desktop | 90% zoom for density |
| Max-width: 1240px | Desktop | Standard width |
| Max-width: 880px | Tablet | Nav menu hides, sections stack |
| Max-width: 780px | Large mobile | Stats grid 2 cols → stacked |
| Max-width: 680px | Mobile | Font scaling, flex-wrap |

### Mobile-First Approach

- **Base:** Mobile layout (stacked, single column)
- **Enhance:** Add media queries for larger screens
- **Font sizes:** Use `clamp()` for fluid scaling
- **Flexbox/Grid:** Natural responsive behavior
- **Touch targets:** 44px minimum for mobile

### Fluid Typography

```css
/* Scales smoothly from 48px (mobile) to 108px (desktop) */
h1 {
  font-size: clamp(48px, 7.2vw, 108px);
  line-height: 1.1;
}

/* Example: 18px to 22px */
.lede {
  font-size: clamp(18px, 1.6vw, 22px);
}
```

---

## Deployment & Hosting

### Static Hosting Options

- **AWS S3 + CloudFront** — Most cost-effective, global CDN
- **Netlify** — Simple git-based deploys, auto-HTTPS
- **Vercel** — Optimized for static sites, instant deploys
- **GitHub Pages** — Free, git-based
- **Traditional VPS** — Nginx/Apache on own server

### Deployment Process

```bash
# No build step needed — serve files as-is

# Example: AWS S3 + CloudFront
aws s3 sync . s3://bucket-name/
aws cloudfront create-invalidation --distribution-id XXXXX --paths "/*"

# Example: Netlify
netlify deploy --prod --dir .

# Example: Vercel (requires vercel.json config)
vercel --prod
```

### Environment Setup

```bash
# No npm install needed
# Just upload files to hosting service

# Server should:
# 1. Serve index.html for / (root)
# 2. Support HTTP Range requests for video streaming
# 3. Set far-future expires for /assets/*
# 4. Set appropriate CSP headers
# 5. Enable gzip compression
```

---

## Development Guidelines

### Adding New Features

#### For animations:
- Use CSS animations/transitions (`.reveal` pattern)
- Apply `data-screen-label` for analytics
- Test `prefers-reduced-motion` media query
- Use `cubic-bezier()` for smooth easing

#### For interactive elements:
- Wrap in IIFE for scope isolation
- Use `querySelector` for single elements, `querySelectorAll` for groups
- Add `data-*` attributes for configuration
- Implement proper event cleanup (unobserve, removeEventListener)

#### For i18n (investidores.html only):
- Add `data-i18n="key"` to HTML
- Use Portuguese text as DOM content
- Add English translation to `i18n.en` object
- Test both languages (PT and EN buttons)

#### For styling:
- Use CSS custom properties for consistency
- Follow mobile-first responsive approach
- Use `clamp()` for fluid typography
- Prefer flexbox for layout, grid for complex grids
- Use `transition` for state changes, `animation` for looping effects

### Testing Checklist

- [ ] **Responsive:** Test on mobile (<768px), tablet (768–1024px), desktop (>1024px)
- [ ] **Browsers:** Chrome, Firefox, Safari, Edge (last 2 versions)
- [ ] **Accessibility:** Keyboard navigation, screen reader (NVDA/JAWS), color contrast
- [ ] **Performance:** Slow network (3G throttling), CPU throttling
- [ ] **Media:** Video loads and autoplays, images render
- [ ] **Tracking:** Form submits with valid/invalid codes
- [ ] **i18n (RI only):** Language toggle works, all text translates
- [ ] **Animations:** Reveal animations trigger on scroll, carousels navigate
- [ ] **Forms:** Submission works (once backend integrated)
- [ ] **Links:** All internal/external links functional

### Common Edits

**Update contact email:**
```html
<!-- index.html footer -->
<a href="mailto:new@email.com">new@email.com</a>

<!-- investidores.html #contato -->
<a href="mailto:ri@newemail.com">ri@newemail.com</a>
```

**Add board member (investidores.html):**
```html
<div class="board-member">
  <div class="board-avatar">JD</div>
  <div class="board-role" data-i18n="b6_role">New Role</div>
  <div class="board-name">Jane Doe</div>
  <div class="board-term mono">
    <span data-i18n="term">Mandato</span> 01/2026 — AGO 2028
  </div>
  <div class="board-bio" data-i18n="b6_bio">Biography...</div>
</div>
```

Then add English translation:
```javascript
i18n.en.b6_role = "New Role";
i18n.en.b6_bio = "Biography...";
```

**Add hero carousel slide:**
```html
<div class="hc-slide" data-screen-label="01c New Slide">
  <!-- Content -->
</div>

<!-- Add corresponding navigation dot -->
<button class="hc-dot"></button>
```

**Change brand colors:**
```css
:root {
  --navy: #NEW_HEX;
  --orange: #NEW_HEX;
  /* ... update other colors ... */
}
```

---

## Project Statistics

| Metric | Value |
|--------|-------|
| **Pages** | 2 (index.html, investidores.html) |
| **Total Code Lines** | ~4,847 |
| **Assets Size** | ~28 MB |
| **External Dependencies** | Google Fonts only |
| **Frameworks/Libraries** | None (vanilla JS) |
| **CSS Preprocessor** | None (native CSS3) |
| **Build Tools** | None |
| **Package Manager** | None (no npm) |
| **i18n Support** | PT/EN (investidores.html only) |
| **Browser Target** | Modern (ES6+, no polyfills) |
| **Responsive** | Yes (mobile-first) |
| **Animations** | CSS + RequestAnimationFrame |
| **Interactive Features** | 9+ (tracking, carousels, map, forms, etc.) |
| **Backend Integration Points** | 6 (tracking, login, contact, RI data, email, whistleblower) |
| **Deployment** | Any static host (S3, Netlify, Vercel, Nginx) |

---

## Summary

**Flash-SEQOIA** is a production-ready, zero-dependency static website perfect for institutional sites requiring high performance, maintainability, and instant deployment. The codebase is well-organized with clear conventions, comprehensive i18n support, and strategic integration points for backend services.

**Next steps:**
1. Deploy to preferred static host
2. Implement backend integrations (tracking, contact forms, RI data sync)
3. Set up CDN with appropriate cache headers
4. Monitor performance and accessibility metrics
5. Update content quarterly (board changes, financial results, events)

---

**Last updated:** 2026-06-22  
**Maintained by:** Gabriel Bonin  
**Status:** Production-ready
