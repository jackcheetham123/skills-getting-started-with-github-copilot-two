# Dark and Light Mode Toggle — Feature Design

## Summary

Add a user-facing control fixed to the bottom-right of the Mergington High School Activities page. It lets visitors switch between light and dark appearance. The selected theme should remain in effect as they use the page and be remembered on their next visit.

## Context

The project serves a single static page from `src/static/index.html`, styles it with `src/static/styles.css`, and initializes browser behavior in `src/static/app.js`. The page contains a header, activity cards, a sign-up form, and a footer. The current light appearance is defined directly in CSS, and no frontend framework or theme service is present. This feature can therefore remain entirely client-side; no API or server changes are needed.

## User experience

- Place a compact, clearly labeled theme toggle at the bottom-right of the viewport. Keep it visible while scrolling, without covering important page content or form controls.
- Show the current mode in the control's accessible name and visible text or icon-plus-text treatment (for example, “Dark mode” when dark mode is active, with wording that makes the available action clear).
- Activating the control switches the page between light and dark themes immediately, without a reload.
- On a visitor's first visit, use the operating system/browser color-scheme preference. After the visitor explicitly chooses a theme, remember that choice in browser storage and restore it on later visits.
- If browser storage is unavailable, the page must still work; apply the current-session choice without throwing an error.

## Design and behavior

### Theme application

- Represent the active theme centrally on the document (for example, through a theme attribute or class on the root element) so all page styling follows the same state.
- Define light and dark color values as theme-level design tokens in the stylesheet rather than duplicating component rules. Cover the page background, primary text, header, sections/cards, form fields, borders, buttons, links, status messages, and focus indicators.
- Preserve the existing light appearance as the light theme baseline. Choose dark-theme colors with sufficient contrast and avoid relying on color alone to convey success or error states.
- Apply the restored theme as early as practical during page initialization to minimize a flash of the wrong theme. Theme setup must not interfere with activity fetching or sign-up form behavior.
- Respect `prefers-reduced-motion` by avoiding or reducing animated color transitions for visitors who request reduced motion. Keep any transition subtle and nonessential.

### Toggle placement and accessibility

- Anchor the control to the bottom-right with safe spacing from viewport edges, including on narrow screens and when device safe-area insets apply.
- Ensure it remains reachable and operable by keyboard, has a visible focus style, and exposes its state and purpose to assistive technology (use appropriate button or switch semantics and an accessible label).
- Maintain adequate target size and text/background contrast in both themes. Check that the fixed control does not obstruct the sign-up form, success/error feedback, or footer on small screens; adjust its layout or spacing if needed.

### Persistence and preference precedence

1. If a valid saved light/dark choice exists, use it.
2. Otherwise, follow the system color-scheme preference.
3. If the system preference cannot be determined, use light mode.
4. When a visitor changes the toggle, save that explicit choice and apply it immediately.

A future enhancement could add a “system” option; it is not part of this two-state toggle's initial scope.

## Scope

**Included:** the static page's theme control, theme styling for existing page elements, client-side preference detection and persistence, and responsive/accessibility behavior.

**Not included:** changes to API endpoints or activity data, account-level theme synchronization, a third “system” setting, or a redesign of the page layout.

## Acceptance criteria

- A theme toggle is visible at the bottom-right on desktop and mobile layouts.
- The user can switch between light and dark mode without reloading, and all existing page regions remain legible and styled consistently.
- The selected mode survives a page reload and subsequent visit in the same browser.
- A first-time visitor with no saved selection receives the operating-system theme; if unavailable, light mode is used.
- Keyboard and assistive-technology users can identify and operate the control, see its current state, and identify focus.
- The control does not block key content or actions at narrow viewport sizes.
- Activity loading, activity selection, and sign-up behavior continue to work in both themes.
- If local storage is unavailable, theme switching continues to work for the current page session without a runtime failure.

## Verification plan

- Manually inspect the page in both themes, including activity cards, form controls, buttons, footer, and success/error feedback.
- Test initial rendering with system preference set to light and dark, with and without a saved choice, and verify that an explicit saved choice takes precedence.
- Toggle themes, reload, and confirm the choice is restored.
- Navigate the control using keyboard only and verify its accessible name/state and visible focus.
- Check narrow/mobile widths and browser zoom to ensure the fixed control does not obscure the form or feedback.
- Simulate unavailable storage and confirm the page remains functional.
- Verify activity fetching and sign-up flows in both themes.
