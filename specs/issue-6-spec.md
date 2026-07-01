# Technical Specification --- Issue #6

## 1. Issue Overview

| Field       | Value                                                                  |
| ----------- | ----------------------------------------------------------------------- |
| Title       | Under footer add message when hovering over "Contact Us"              |
| Description | The footer's "Contact Us" link should display a message on hover.     |
| Labels      | none                                                                     |
| Priority    | Low                                                                      |

## 2. Problem Analysis

The footer (`src/components/Footer.jsx`) renders a set of links — Privacy
Policy, Terms of Service, Cookie Policy, and Contact Us — each wrapped in a
`group` container with a hidden tooltip `div` that becomes visible via
`group-hover:opacity-100 group-hover:visible`.

At the time issue #6 was filed, the "Contact Us" link had no tooltip `div`
at all, so hovering over it produced no visual feedback, unlike its sibling
links which already had descriptive tooltip text.

This was resolved in PR #11 (commit `1bcea48`, merged via `39cf676`), which
added a tooltip block matching the existing pattern used by Privacy Policy /
Terms of Service / Cookie Policy, with descriptive copy: "Have a question or
need help? Reach out to our team and we'll get back to you as soon as
possible."

A subsequent direct commit (`564c1c1`, "fix: update footer contact us hover
tooltip text") replaced that descriptive copy with the plain string
"Contact Us", intentionally shortening the tooltip. This was a deliberate
content change made after the issue was closed, not a regression — the
tooltip element, hover behavior, and markup structure remain intact and
functional.

## 3. Proposed Solution

No structural change is required. The tooltip mechanism (group-hover show/hide,
positioning, arrow indicator) already matches the pattern used by the other
three footer links and needs no rework.

The only open question is copy content: the tooltip currently repeats the
link label ("Contact Us") rather than adding new information, which is
arguably redundant since the link text itself already reads "Contact Us".
If richer messaging is desired, restore or refine descriptive text along the
lines of the original PR #11 copy — this is a content decision, not a
technical one.

## 4. Step-by-Step Implementation

1. Confirm the tooltip element renders on hover — no code change needed,
   behavior already works.
2. (Optional, content-only) Update the tooltip text at
   `src/components/Footer.jsx:181` to a descriptive message if a richer
   hover message is wanted, following the same tone as the Privacy Policy /
   Terms of Service / Cookie Policy tooltips.
3. No other files require changes.

## 5. Verification Strategy

### Unit Tests

- N/A — no component logic changed; tooltip visibility is CSS-driven via
  Tailwind's `group-hover` utility, not component state.

### Integration Tests

- N/A — no route or context behavior involved.

### Manual Checks

- Hover over "Contact Us" in the footer → tooltip becomes visible with
  current text ("Contact Us").
- Hover away → tooltip hides.
- Compare visual consistency (positioning, arrow, styling) against the
  Privacy Policy / Terms of Service / Cookie Policy tooltips.

## 6. Files to Modify

| File Path                       | Nature of Change                                              |
| -------------------------------- | --------------------------------------------------------------- |
| `src/components/Footer.jsx`     | Optional: update tooltip copy at line 181 if richer text desired |

## 7. New Files to Create

None.

## 8. Existing Utilities to Leverage

| Utility                                   | Benefit                                                        |
| ------------------------------------------ | ----------------------------------------------------------------- |
| Existing `group` / `group-hover` Tailwind pattern already used for Privacy Policy, Terms of Service, Cookie Policy tooltips | Keeps Contact Us tooltip visually and behaviorally consistent with siblings |

## 9. Acceptance Criteria

- Hovering over "Contact Us" in the footer displays a visible tooltip. (Already satisfied.)
- Tooltip styling matches sibling links. (Already satisfied.)
- No regressions to other footer links.

## 10. Out of Scope

- Rewriting or expanding tooltip copy for the other footer links (Privacy
  Policy, Terms of Service, Cookie Policy) — those are separate, already-closed
  issues (#5, #3, #2).
- Any redesign of the footer tooltip mechanism.
