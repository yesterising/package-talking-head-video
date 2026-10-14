## User editing workflow precedence

Read the installed `video-editing-playbook` skill, located through the current host skill directory first. The visual rules below may guide authoring. Screenshots, blocking gates and manual acceptance checks run only on explicit user request; they are not completion criteria for ordinary editing. The user performs acceptance and export.

# Blocking visual quality gates

These checks apply to every real invocation. Passing unit tests, typecheck, build, or render is not visual acceptance.

## 1. Presentation mode

Every beat declares exactly one visual mode:

- **Presenter-safe:** the live presenter remains visible. Foreground content is restricted to x=5%–32% or x=68%–95%; center x=35%–65% is completely clear; the lower 18% is reserved for burned-in captions.
- **Opaque full-screen:** the graphic covers the full 1920×1080 canvas with an opaque background. It may use the center because the presenter is intentionally replaced for that beat.
- **Behind-presenter:** text/animation may cross the center beneath a synchronized subject matte. Review according to the installed `koubo-cut` reference `references/large-type-and-occlusion.md`; if unavailable, keep essential text outside the presenter area and use behind-presenter layers only with a usable synchronized matte, including full-motion playback and readable copy after occlusion.

Reject foreground graphics covering the live face. Center-spanning graphics require verified behind-presenter layering or an opaque full-screen scene.

## 2. Side-card density

Presenter-safe side surfaces must be content-fit.

- Do not stretch a card from a fixed top to a fixed bottom.
- Maximum card height is 64% of the frame.
- Do not leave a decorative empty lower half.
- Use one rail when there is one idea. Use two rails only when the narration genuinely contains two complementary groups.
- Headings, items, and takeaway must participate in normal document flow; do not pin a footer far below sparse content.

## 3. Full-screen composition

- The background must be fully opaque from edge to edge; no transparent final gradient stop.
- The visual hierarchy needs one clear primary element, one supporting structure, and one result or source label.
- Process routes and illustrations may use the center in this mode; behind-presenter beats must satisfy the separate subject-matte checks.
- Critical content stays out of the lower 18% caption reserve unless the source is explicitly caption-free.

## 4. Stable review frames

Representative stills are selected at 72% of each beat, after the main entry animation but before the exit. A midpoint screenshot is not sufficient because it can capture a half-built layout.

Gate C must include:

1. eight frames from eight distinct semantic structures when the video semantics support them;
2. the exact timestamp, structure, presentation mode, and SHA-256 for every frame;
3. a 4×2 contact sheet built from those exact frames;
4. manual frame review, recorded as pass/fail for face safety, density, clipping, contrast, semantic fit, and subtitle clearance.

## 5. Blocking decision

Any of the following blocks Gate D:

- foreground graphics covering the presenter, or a behind-presenter matte with visible flicker, missing body/hand edges, misalignment, or unreadable key text;
- opaque scene not actually opaque;
- side card visibly padded with unused space;
- content clipped, overlapping, or too small to read;
- screenshot captured before the composition reaches a stable readable state;
- structure selected for visual variety rather than spoken meaning;
- evidence treated as decoration or an illustration treated as proof;
- any manual review item left unchecked.

After fixes, regenerate the real screenshots and review the new images. Do not reuse screenshots from a rejected render.
