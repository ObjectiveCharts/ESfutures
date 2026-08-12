# Visual tokens (draft — fill before UI)

Lock these before any layout or graphics work. Agents may not invent new colors or fonts.

## Color

| Token | Hex | Use |
|---|---|---|
| `--bg` | `#TODO` | Page background |
| `--surface` | `#TODO` | Panels / player chrome |
| `--text` | `#TODO` | Primary text |
| `--text-muted` | `#TODO` | Support copy |
| `--accent` | `#TODO` | Primary CTA |
| `--accent-2` | `#TODO` | Optional secondary |

Max 2 accents. No extra “decorative” greys outside this table.

## Typography

| Role | Family | Fallback | Notes |
|---|---|---|---|
| Display | TODO (expressive) | serif/sans of choice | Brand / H1 |
| Body | TODO | — | Long copy, UI |

Do **not** use Inter, Roboto, Arial, or system-ui as the designed look.

### Type scale (rem)

| Step | Mobile | Desktop | Use |
|---|---|---|---|
| display | 2.25 | 3.5 | Brand / hero |
| h1 | 1.75 | 2.5 | Section titles |
| h2 | 1.35 | 1.75 | Subsections |
| body | 1 | 1.125 | Copy |
| small | 0.875 | 0.875 | Meta / legal |

## Space scale

`4 / 8 / 16 / 24 / 40 / 64` only. No one-off `13px` gaps.

## Radius & borders

| Token | Value | Use |
|---|---|---|
| `--radius` | TODO | Controls / inputs only |
| `--border` | TODO | Hairlines if needed |

## Motion

2–3 intentional motions max (e.g. fade-in hero copy, player pulse when live). No ambient particle noise.
