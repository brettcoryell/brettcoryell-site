# Theme Rename Exceptions — brettcoryell-site

Scope: `refactor: rename CSS tokens to semantic roles (theme-arch step 1)`

## Tailwind key names preserved

`index.html` has no Tailwind; N/A for this repo.

## Semantic mismatches

| Location | Old token | Usage | Issue | Recommended future token |
|---|---|---|---|---|
| `index.html` footer `.site-footer` | `--color-text-primary` (was `--site-ink`) | `background-color` | `--color-text-primary` = `#1a2332` is correct value but semantically wrong name for a bg | `--color-bg-footer` |

## Deferred — dedicated session required

- None in this repo.

## Notes

All `--site-*` references in source replaced. Primitive block renamed to `--primitive-*` and kept in `:root`. Semantic `--color-*` tokens imported from `themes/default.css`.
