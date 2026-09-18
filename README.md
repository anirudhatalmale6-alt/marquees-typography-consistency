# Marquees.com - homepage typography consistency

## What I found

The homepage text sizes are set by `wp-content/themes/marquees-spacing-child/style.css`
(15,726 lines). It contains **177 separate `font-size` rules**, almost all marked
`!important`, each with its own hand-written `clamp()`.

That is why things don't match:

* Some section titles have a custom clamp, others have **no rule at all** and fall
  back to Divi's 34px preset. Measured on the live page at 1440px wide:

  | Section title | Current size | Where it comes from |
  |---|---|---|
  | Creating Special Events (hero) | 51.2px | `clamp(2.15rem, 4.5vw, 3.2rem)` (line 4570) |
  | Marquee Style | 46.4px | `clamp(2rem, 3.4vw, 2.9rem)` (line 6955) |
  | From Our Blog | 33.6px | `clamp(1.6rem, 2.5vw, 2.1rem)` (line 3348) |
  | Event Type | 34px | no rule - Divi preset |
  | Our Team | 34px | no rule - Divi preset |
  | Recent Projects | 34px | no rule - Divi preset |

* One element, three conflicting rules. `Marquees for Every Event, Every Season`
  is set three times - lines 6362, 6426 and 15679 - to three different clamps.

* Body copy runs at 16px, 16.8px, 17px, 18px, 20.88px and 24.8px depending on
  which block it sits in.

* On mobile the hierarchy collapses: the hero drops 51.2px -> 34.4px but the
  section titles don't scale at all, so they end up the same size as the hero.

* Card titles use two different golds - `#b9ab85` in the Event Type grid,
  `#bdb7aa` in the Marquee Style grid. In the Recent Projects carousel the two
  are mixed within the same row.

## What this file does

Replaces the one-off values with a single scale, declared once at the top:

| Role | Mobile | Desktop |
|---|---|---|
| Hero headline | 34.4px | 51.2px |
| Section titles (all five) | 28px | 36px |
| Hero eyebrow | 11.8px | 13.4px |
| Section sub-headings | 20px | 24px |
| Lead paragraph | 18.4px | 21.6px |
| Body copy | 16px | 17px |
| Card titles | 16.8px | 18.4px |
| Card body | 16px | 16px |

Change one number at the top of the file and it changes everywhere.

## Install

Append to `wp-content/themes/marquees-spacing-child/style.css`, or paste into
**Divi > Theme Options > Custom CSS**. Nothing else needs to change.

To undo, delete the block. No existing rule is edited or removed.

## Verified

Measured with a headless Chromium against the live page at 1440px, 900px and
390px wide, before and after. Result:

* section titles: 4 different sizes -> 1
* sub-headings: 3 different sizes -> 1 (+ the hero eyebrow, which is a separate role)
* paragraphs: 6 different sizes -> 3 (lead / body / card body)
* no horizontal overflow introduced at any of the three widths

## Note

`Recent Projects`, `From Our Blog` and `Get a Quote` live in the Divi **footer
template**, so those three rules apply on every page, not just the homepage.
That is what you want for consistency, but worth knowing before it goes live.
