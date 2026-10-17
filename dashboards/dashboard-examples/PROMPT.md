Build a curated dashboard reference page as a single HTML file: eleven dashboard examples (open-source kits + live demos) as filterable cards with license tags.

THE PAGE
- Dark GitHub-style theme. Hero with title + one-line intro. Filter chips: All / UI kits / Live demos / Charts & maps. Result count line. Responsive card grid (auto-fill, min 300px).
- Each card: name, one-paragraph description, license/type tags (color-coded: MIT green, Apache blue, AGPL orange, live demo purple), outbound link.
- One inline script: chip filtering + count update.

STRUCTURE
- Tokens in :root custom properties. Sections: hero · filters · grid. No assets, no libraries — text and CSS only, works from file://.

WHAT BREAKS
- Cards are hardcoded HTML, not data-driven — adding one means copying a card block, not pushing to an array.
- The filter keys live in data-k attributes (kit / live / charts); a new category needs a chip button too.
