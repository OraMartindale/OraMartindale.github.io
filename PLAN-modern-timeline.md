# Plan: Modern Timeline Redesign (Issue #6)

## Context

The timeline section on martindale.org shows the family's life events from 1998-2017. The current design uses a basic alternating left/right layout with plain bordered boxes and muted icons. It also lacks responsive styles entirely, breaking on mobile. This redesign modernizes the visual treatment while keeping the alternating timeline pattern on desktop.

## Files to Modify

- `css/style.css` — Replace lines 366-441 (old timeline CSS) with ~170 lines of new styles
- `index.html` — Replace lines 111-256 (the `<ul>` timeline) with new `<div>`-based structure
- `js/custom.js` — Add ~15 lines for scroll-driven progress line animation

## Design Approach

### Visual Changes
- **Cards**: Rounded corners (12px), subtle shadow, `border-top: 3px solid #BE2525` accent strip, slight lift on hover
- **Icon dots**: Solid `#BE2525` background with white icons, double-ring shadow effect (`box-shadow` with white + red glow), scale on hover
- **Center line**: Two-layer — a beige "track" line (CSS `::before`) with a red "progress" line that fills as you scroll (JS-driven)
- **Section background**: Warm gradient (`#faf9f6` to `#f5f0eb`)
- **Typography**: Year in `Satisfy` cursive font at 28px in `#BE2525`; description in `Roboto` 300-weight
- **Future entry** (last item, `fa-question`): Dashed border dot with infinite pulse animation
- **Scroll animations**: WOW.js `fadeInLeft`/`fadeInRight` alternating per card

### HTML Structure Change
Switch from `<ul><li>` to semantic `<div>` structure:
```html
<div class="timeline">
  <div class="timeline-entry wow fadeInLeft">
    <div class="timeline-dot"><i class="fa fa-heart"></i></div>
    <div class="timeline-card">
      <span class="timeline-year">1998</span>
      <p class="timeline-text">Girl meets Boy.</p>
    </div>
  </div>
  <!-- alternating fadeInLeft / fadeInRight for odd/even -->
</div>
```

### Responsive Design (currently missing)
- **Desktop (992px+)**: Alternating left/right with centered line and dots
- **Tablet (768-991px)**: Single-column, line and dots on left at 30px
- **Mobile (<768px)**: Single-column, line and dots on left at 24px, smaller dots (44px), all cards animate from left

### JavaScript Addition
Scroll-driven progress line: a `<div class="timeline-progress">` inserted by JS that grows in height as the user scrolls through the timeline section, creating a "filling in" effect over the beige track line.

## Implementation Steps

1. **style.css**: Delete old timeline CSS (lines 366-441). Append new timeline styles at end of file covering: container, `::before` track line, entry layout (flexbox + alternating), dot styling, card styling, arrow pointers (`::after`), typography, future entry, progress line, responsive breakpoints (767px, 768-991px)
2. **index.html**: Replace `<ul class="timeline">...</ul>` with new `<div class="timeline">` structure; 16 entries with alternating `wow fadeInLeft`/`fadeInRight`; last entry gets `timeline-entry--future` class; remove `wow fadeInUp delay-05s` from the `<section>` tag
3. **js/custom.js**: Add scroll listener that calculates scroll progress through `.timeline` and sets height on `.timeline-progress` div; ensure WOW init includes `mobile: true`

## Verification
- Open `index.html` in browser at various widths: 320px, 768px, 992px, 1200px+
- Confirm alternating layout on desktop, single-column on mobile/tablet
- Scroll through to see WOW.js card animations fire
- Scroll to verify red progress line fills the timeline
- Hover dots and cards to confirm interactive effects
- Check that the final "future" entry pulses
