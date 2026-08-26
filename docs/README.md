# The phone remote page

Static export of the KillerDarts phone controller, served by GitHub Pages so a player can
scan a board's QR and play **without joining the venue's Wi-Fi**. Built by
`npm run remote:export` in the private source repo; do not edit these files by hand.

## Why it is hosted here and not on Supabase Storage

Supabase Storage serves `.html` as `Content-Type: text/plain` — deliberately, so
`supabase.co` cannot be used to host arbitrary pages. The browser then shows the source
instead of rendering it. Its sibling `manifest.webmanifest` comes back with the correct
`application/manifest+json`, which is what proves the HTML case is a policy and not a
mistake. Measured 2026-08-26; `docs/relay-setup.md` used to recommend Storage.

## What is in these files, and what is not

**In:** the page, the Supabase project URL, and the **publishable** key — which is
RLS-constrained by design and is meant to be downloadable. The export refuses to build with
a key that bypasses row-level security.

**Not in:** the 160-bit join token (it rides in the QR and is minted fresh on every board
restart), the unit secret, any database credential, and any player data.

The join token IS the access control for the relay. A copy of this page without one reaches
no board.

## Files

`index.html` picks a language from the phone — not the board, because the venue sets one and
the reader may want another — then forwards to `en|fr|de|es|nl.html`, preserving the query
string so the join token survives the hop. `.nojekyll` stops GitHub Pages processing them.
