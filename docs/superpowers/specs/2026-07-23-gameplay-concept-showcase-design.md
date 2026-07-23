# Wire Runner Gameplay Concept Showcase

## Goal

Turn the construction teaser into a believable advertisement for the future Wire Runner simulator. Add gameplay imagery that shows how the program is expected to feel while clearly presenting its camera and companion states.

## Visual thesis

A grounded, cinematic conversion-delivery simulator: wet roads, restrained Transamerica-adjacent navy and red, operational stakes, and realistic gameplay framing rather than arcade spectacle.

## Content plan

The existing hero remains the opening poster. A new `Gameplay` section follows it with one dominant 16:9 stage, a short explanatory line, and four selectable views:

1. **Driver / Solo** — first-person cab view with the road, mirrors, dashboard, and next route checkpoint.
2. **COM Co-pilot** — first-person cab view with a COM passenger and coordination or handoff cues.
3. **TC Co-pilot** — first-person cab view with a TC passenger and setup or audit cues.
4. **Chase Camera** — third-person view behind the same delivery vehicle on the same route.

The page will identify these as gameplay concepts so the visuals advertise the intended experience without claiming they are production captures.

## Asset direction

Generate four separate landscape images rather than one collage. Every image must preserve a common world:

- the same red-and-navy utility vehicle;
- the same wet, winding mountain road approaching a distant city;
- consistent overcast sunset lighting and realistic materials;
- grounded high-end simulator rendering;
- no weapons, racing competitors, arcade pickups, scores, logos, watermarks, or embedded interface text.

The passenger images should show a credible adult coworker in the front passenger seat without making a real person's identity part of the concept. COM and TC identity will be communicated by the page's accurate interface labels and role-specific prompts, not costume stereotypes.

Generated art stays free of HUD text. The precise gameplay interface will be rendered as HTML and CSS on top of each image, avoiding garbled generated lettering and keeping the future UI editable.

## Interface design

The showcase uses one large active image and four compact selectors. Selecting a state updates the artwork, title, short role description, camera label, and HUD details.

The HUD remains restrained and functional:

- route: `D1 -> EFF -> WIRE`;
- current checkpoint and next objective;
- camera or companion state;
- a small route-progress treatment derived from the existing route card;
- role-specific prompts for COM coordination and TC setup/audit work.

The section should inherit the current navy, paper, red, amber, condensed display type, thin-line mapping language, and film-grain atmosphere. It should feel like a continuation of the existing page, not a separate dashboard.

## Interaction thesis

- Crossfade and subtly scale the active image when the selected view changes.
- Animate HUD labels with a short stagger so the new state reads immediately.
- Add a restrained hover/focus treatment to the four view selectors.

All motion will honor `prefers-reduced-motion`. Controls must be keyboard operable, expose their selected state, and retain readable contrast over every image.

## Responsive behavior

On desktop, the active image is the dominant full-width visual with selectors in a single row below it. On small screens, the stage keeps a wide crop, selectors become a two-column grid, and decorative HUD details reduce before essential labels do. The existing hero remains intact and scrolls naturally into the new section.

## Implementation boundaries

- Keep the project as a self-contained static site.
- Preserve the existing hero, branding, route preview, and pointer movement unless a small height adjustment is required for natural scrolling.
- Save all four generated images under `assets/` with descriptive, non-overwriting filenames.
- Use semantic buttons and progressive enhancement; all four images remain meaningful if JavaScript is unavailable.
- Do not add a framework or remote runtime dependency.

## Verification

- Extend the existing Node validation to require all four assets, gameplay labels, accessible selector semantics, and reduced-motion support.
- Check desktop and mobile layouts for cropping, readable HUD contrast, keyboard selection, and no horizontal overflow.
- Verify every local image reference exists.
- Confirm the live hosted page loads the new assets after deployment.
