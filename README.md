# proverb-masher

Real proverbs, chopped in half and glued back wrong -- in Swedish, English,
Thai and Russian. The grammar stays right; the meaning gets mangled. That's
the whole point.

> A dear friend is better than two new ones. · Don't say "hop" -- then love
> hauling the sled uphill too. · New brooms run deep.

**Live:** https://leifclaesson.github.io/proverb-masher/

## How it works

Each proverb in the [data files](data/) is split by a pipe into a HEAD and a
TAIL, grouped into sections of grammatically interchangeable halves. The
engine prints one proverb's HEAD glued to a *different* proverb's TAIL. Two
details lift it above a dumb word-masher:

1. **Population-weighted section choice** -- one slot per proverb, so a fat
   section is picked in proportion to its size. Section 1 of every edition is
   deliberately stuffed with fake "expensive" decoys, so the weighting makes
   them dominate the output.
2. **The retry rule** -- head and tail must come from different lines, so the
   engine is structurally biased against ever handing you an intact proverb.
   In the 1996 original this retry label was named after a Swedish curse
   (`fanheller:`), and every edition since has translated the swearing along
   with the data.

The Russian edition's decoy word «дорогой» means both *expensive* and *dear*
-- a pun the Swedish original (*dyr*) and English (*dear*, as in "it cost him
dear") turn out to share, since they're the same Germanic word.

## Provenance

`COOKIE.BAS`, written by Leif Claesson (**Liket of Goto10**) around 1996 in
PowerBASIC for DOS, where it mashed Swedish proverbs on every boot. Revived
2026 as Python and Rust console ports plus this page, which reads the same
data format the DOS original did -- the Swedish data file is still CP437,
decoded by a hand-spelled 128-entry table.

No frameworks, no build step, no backend: one HTML file and four text files.
