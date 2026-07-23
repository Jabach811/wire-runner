# Wire Runner Gameplay Concept Showcase Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a realistic four-state gameplay showcase to the Wire Runner landing page and publish the verified result.

**Architecture:** Keep the project framework-free and extend the existing `index.html` with a semantic gameplay section, four local scene assets, CSS-rendered HUDs, and a small progressive-enhancement controller. Generated imagery supplies only the world and characters; editable HTML supplies all exact interface text and state.

**Tech Stack:** Static HTML, CSS, vanilla JavaScript, SVG/CSS HUD elements, built-in image generation, Node validation, Sites hosting.

## Global Constraints

- Preserve the existing hero, branding, route preview, and pointer movement.
- Save four separate landscape assets under `assets/`; do not overwrite the existing road image.
- Use the same red-and-navy utility vehicle, wet mountain route, distant city, and overcast sunset world across all images.
- Keep generated images free of logos, watermarks, weapons, racing elements, arcade collectibles, scores, and embedded interface text.
- Render all exact HUD copy in HTML/CSS.
- Keep the site self-contained with no framework or remote runtime dependency.
- Honor `prefers-reduced-motion`, keyboard operation, selected-state semantics, contrast, and small-screen layout.
- Label the images as gameplay concepts rather than production captures.

---

### Task 1: Define the gameplay contract in validation

**Files:**
- Modify: `tests/validate-wire-runner.js`
- Test: `tests/validate-wire-runner.js`

**Interfaces:**
- Consumes: existing static HTML and local-asset validation.
- Produces: required asset names and markup tokens that later tasks must satisfy.

- [ ] **Step 1: Add failing requirements for the showcase**

Add these entries to the existing `required` array:

```js
'id="gameplay"',
'data-gameplay-view="driver"',
'data-gameplay-view="com"',
'data-gameplay-view="tc"',
'data-gameplay-view="chase"',
'aria-pressed="true"',
'Gameplay concept — representative of the intended experience',
'assets/gameplay-driver-solo.png',
'assets/gameplay-com-copilot.png',
'assets/gameplay-tc-copilot.png',
'assets/gameplay-chase-camera.png'
```

Add assertions for four selector buttons and four gameplay frames:

```js
const selectorCount = (html.match(/class="gameplay-selector/g) || []).length;
const frameCount = (html.match(/class="gameplay-frame/g) || []).length;

if (selectorCount !== 4) {
  throw new Error(`Expected 4 gameplay selectors, found ${selectorCount}`);
}

if (frameCount !== 4) {
  throw new Error(`Expected 4 gameplay frames, found ${frameCount}`);
}
```

- [ ] **Step 2: Run validation and confirm the contract fails**

Run: `node tests/validate-wire-runner.js`

Expected: FAIL on `Missing required token: id="gameplay"`.

- [ ] **Step 3: Commit the failing contract**

```powershell
git add -- tests/validate-wire-runner.js
git commit -m "test: define gameplay showcase contract"
```

---

### Task 2: Generate and validate the matched gameplay scenes

**Files:**
- Create: `assets/gameplay-driver-solo.png`
- Create: `assets/gameplay-com-copilot.png`
- Create: `assets/gameplay-tc-copilot.png`
- Create: `assets/gameplay-chase-camera.png`

**Interfaces:**
- Consumes: the approved asset direction and the existing `assets/wire-runner-road.webp` world reference.
- Produces: four 16:9 scene images consumed by the gameplay frames in `index.html`.

- [ ] **Step 1: Generate the driver scene**

Use the built-in image-generation path with this production prompt:

```text
Use case: stylized-concept
Asset type: realistic 16:9 gameplay screenshot for a conversion-delivery training simulator advertisement
Primary request: first-person driver view inside a modern red-and-navy utility delivery vehicle descending a wet winding mountain road toward a distant city
Scene/backdrop: overcast sunset after rain, reflective asphalt, pine-covered hills, guardrails, distant city skyline
Subject: steering wheel, dashboard, windshield, side mirrors, and open road; no passenger
Style/medium: grounded high-end real-time game rendering, believable simulator gameplay, not a photograph and not cinematic key art
Composition/framing: 16:9 driver eye level with clear road visibility and safe negative space near edges for an HTML HUD overlay
Constraints: no embedded UI, no readable text, no logos, no watermark; preserve realistic vehicle proportions
Avoid: weapons, racing opponents, arcade pickups, speed effects, fantasy vehicles
```

Copy the selected output to `assets/gameplay-driver-solo.png`.

- [ ] **Step 2: Generate the COM co-pilot scene**

Use the driver scene as the visual continuity reference and request the same cab, route, weather, and lighting. Add one professional adult coworker in the front passenger seat, visible naturally in peripheral view, engaged with a tablet or route folder. Keep the passenger generic and non-identifiable. Copy the output to `assets/gameplay-com-copilot.png`.

- [ ] **Step 3: Generate the TC co-pilot scene**

Use the established cab scene as the continuity reference. Add one different professional adult coworker in the front passenger seat reviewing a compact setup checklist or tablet. Preserve the exact camera height, vehicle interior, route, weather, and lighting. Copy the output to `assets/gameplay-tc-copilot.png`.

- [ ] **Step 4: Generate the chase-camera scene**

Use the established vehicle/world reference and request a third-person chase camera approximately six meters behind and slightly above the same red-and-navy utility vehicle on the wet mountain curve, with the distant city ahead. Keep clear image edges for the HTML HUD. Copy the output to `assets/gameplay-chase-camera.png`.

- [ ] **Step 5: Inspect all four assets**

Open each asset and verify: 16:9 landscape composition, common world, consistent vehicle/interior, correct passenger count, no embedded text or logos, and no arcade or racing cues. Regenerate any asset that breaks continuity or looks like promotional key art instead of gameplay.

- [ ] **Step 6: Commit the approved assets**

```powershell
git add -- assets/gameplay-driver-solo.png assets/gameplay-com-copilot.png assets/gameplay-tc-copilot.png assets/gameplay-chase-camera.png
git commit -m "feat: add matched gameplay concept scenes"
```

---

### Task 3: Build the gameplay showcase and real HUD overlays

**Files:**
- Modify: `index.html`
- Test: `tests/validate-wire-runner.js`

**Interfaces:**
- Consumes: the four gameplay PNG files and the selector tokens from Task 1.
- Produces: `selectGameplayView(viewId)` behavior over `driver`, `com`, `tc`, and `chase` states.

- [ ] **Step 1: Add the semantic section after the existing hero scene**

Add a `<section id="gameplay" class="gameplay-showcase">` containing:

```html
<header class="gameplay-intro">
  <p class="eyebrow">Gameplay</p>
  <h2>One route. Four seats in the mission.</h2>
  <p>Switch perspectives to see how driving, coordination, setup, and delivery come together.</p>
  <span class="concept-label">Gameplay concept — representative of the intended experience</span>
</header>
```

Create four `.gameplay-frame` figures with unique `data-gameplay-frame` values and accurate `img` alt text. Each figure contains a CSS/HTML HUD with:

```html
<span class="hud-route">D1 <i></i> EFF <i></i> WIRE</span>
<span class="hud-camera">Driver / Solo</span>
<strong class="hud-objective">Hold the route</strong>
<span class="hud-detail">Next checkpoint · EFF</span>
```

Use these state-specific labels:

| View | Camera label | Objective | Detail |
|---|---|---|---|
| `driver` | `Driver / Solo` | `Hold the route` | `Next checkpoint · EFF` |
| `com` | `COM / Co-pilot` | `Confirm the handoff` | `Coordination channel · Active` |
| `tc` | `TC / Co-pilot` | `Clear the setup gate` | `Setup and audit checks · In review` |
| `chase` | `Chase Camera` | `Carry the conversion` | `Destination · WIRE` |

- [ ] **Step 2: Add the four selectors**

Add four native buttons using this exact interface:

```html
<button class="gameplay-selector is-active" type="button" data-gameplay-view="driver" aria-pressed="true">
  <span>01</span><strong>Driver</strong><small>Solo route</small>
</button>
```

Repeat for `com`, `tc`, and `chase`, setting their initial `aria-pressed` values to `false` and labels to `COM`, `TC`, and `Chase`.

- [ ] **Step 3: Add the responsive visual system**

Add styles that:

```css
.gameplay-showcase { position: relative; padding: clamp(5rem, 10vw, 9rem) 0; }
.gameplay-stage { position: relative; aspect-ratio: 16 / 9; overflow: hidden; background: #08111d; }
.gameplay-frame { margin: 0; min-height: 100%; }
.js .gameplay-frame { position: absolute; inset: 0; opacity: 0; pointer-events: none; transform: scale(1.015); }
.js .gameplay-frame.is-active { opacity: 1; pointer-events: auto; transform: scale(1); }
.gameplay-frame img { width: 100%; height: 100%; object-fit: cover; }
.gameplay-selectors { display: grid; grid-template-columns: repeat(4, 1fr); }
@media (max-width: 700px) { .gameplay-selectors { grid-template-columns: repeat(2, 1fr); } }
```

Use the existing navy, red, amber, paper, thin-line, and condensed-type tokens. Keep the stage visually dominant and avoid turning each selector into an ornamental card.

- [ ] **Step 4: Add progressive-enhancement selection behavior**

Place `document.documentElement.classList.add('js')` before the page renders interactive state. Implement:

```js
const selectors = [...document.querySelectorAll('[data-gameplay-view]')];
const frames = [...document.querySelectorAll('[data-gameplay-frame]')];

function selectGameplayView(viewId) {
  selectors.forEach((button) => {
    const selected = button.dataset.gameplayView === viewId;
    button.classList.toggle('is-active', selected);
    button.setAttribute('aria-pressed', String(selected));
  });

  frames.forEach((frame) => {
    frame.classList.toggle('is-active', frame.dataset.gameplayFrame === viewId);
  });
}
```

Bind click plus left/right arrow movement between selectors. On arrow movement, focus the next button and call `selectGameplayView(next.dataset.gameplayView)`.

- [ ] **Step 5: Run the automated validation**

Run: `node tests/validate-wire-runner.js`

Expected: `Wire Runner validation passed.`

- [ ] **Step 6: Commit the showcase implementation**

```powershell
git add -- index.html
git commit -m "feat: add interactive gameplay showcase"
```

---

### Task 4: Verify the experience and publish it

**Files:**
- Modify only if verification finds a scoped defect: `index.html`, `tests/validate-wire-runner.js`, or one of the four gameplay assets.
- Read: `.openai/hosting.json`

**Interfaces:**
- Consumes: completed static page and `project_id` from Sites hosting configuration.
- Produces: verified local result and refreshed live deployment.

- [ ] **Step 1: Run static checks**

Run:

```powershell
node tests/validate-wire-runner.js
git diff --check
```

Expected: validation passes and `git diff --check` emits no errors.

- [ ] **Step 2: Verify rendered behavior at desktop and mobile sizes**

Check 1440×900 and 390×844 renders. Confirm all four controls switch the active art and HUD, arrow keys work, focus is visible, no horizontal overflow appears, image crops keep the passenger/vehicle readable, and reduced motion removes the crossfade/scale animation.

- [ ] **Step 3: Publish with the existing Sites project**

Deploy the workspace through the configured Sites project `appgprj_6a62700f4f6881918547c7d4438a3283`. Do not replace the hosting configuration or create a second site.

- [ ] **Step 4: Verify the live URL**

Open `https://wire-runner.jabach0811.chatgpt.site/` and confirm the new Gameplay section, four assets, and selector behavior are present on the served build.

- [ ] **Step 5: Record any final scoped fix**

If verification required a correction, rerun Step 1 and commit only the affected files:

```powershell
git add -- index.html tests/validate-wire-runner.js assets/gameplay-driver-solo.png assets/gameplay-com-copilot.png assets/gameplay-tc-copilot.png assets/gameplay-chase-camera.png
git commit -m "fix: polish gameplay showcase verification issues"
```
