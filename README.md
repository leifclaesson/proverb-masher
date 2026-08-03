# proverb-masher

Real proverbs, chopped in half and glued back wrong -- in Swedish, English,
Thai, Russian, Dutch, Greek, Polish and French. The grammar stays right; the
meaning gets mangled. That's the whole point.

> A dear friend is better than two new ones. · Don't say "hop" -- then love
> hauling the sled uphill too. · New brooms run deep.

**Live:** https://leifclaesson.github.io/proverb-masher/

Hit `[X] Auto` to leave it cycling on a monitor. `?auto=1` arms that from the
URL and `?auto=20` also sets the dwell in seconds -- the pause after a line
lands, so a long one is never cut short. The default dwell is 10 seconds. For
a screen with no keyboard in front of it.

The `☼` button beside it opens the autocycle's settings. A switch decides
whether the cycle stays in one language or changes language as well, and a bar
per language sets how often each one comes up -- so a screen can run mostly
Swedish with the occasional Thai line, or an even eight-way shuffle. The
weights are **relative, not percentages**: all eight at 50 is the same even
shuffle as all eight at 100, and a language at zero simply never comes up.
`?autolang=1` arms language-changing from the URL, and the weights are
remembered per browser. Only the timer ever changes language -- `One more` and
the spacebar stay in the language you are reading.

Those bars are Leif's own **ProgressAdjust** slider, the same control his audio
software uses: no handle, drag anywhere on it, and it accelerates as you move
so one gesture covers both the coarse and the fine end. Double-click undoes a
drag, right-click cancels one, and the tick at 50 marks the default.

The line types itself on at Leif's own measured keyboard cadence, always --
the OS reduced-motion flag is for window transitions and doesn't get a say in
whether a terminal prints its line. `?type=0` turns it off for anyone who wants
that, and the choice sticks.

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

*Biased*, not incapable, and building the Greek edition is what finally got it
measured. The rule guarantees the two **indices** differ, not the two
**strings** -- and each decoy shares its tail character-for-character with the
real proverb it shadows, so gluing the real head onto the decoy's tail puts the
real line back together. The 1996 Swedish data does exactly that about **one
roll in 92**; Russian, with the tightest decoy set, manages one in 51, Polish
one in 129 and French one in 199. Thai is the only edition that never does it.
Nothing worth fixing -- a masher that occasionally tells the truth by accident
is funnier than one that can't.

The Russian edition's decoy word «дорогой» means both *expensive* and *dear*
-- a pun the Swedish original (*dyr*) and English (*dear*, as in "it cost him
dear") turn out to share, since they're the same Germanic word. Polish
*drogi* has it too and takes it furthest: it is the word you open a letter
with, so the masculine-personal plural decoy lands exactly on **Drodzy
przyjaciele**, *Dear Friends*. Its feminine *droga* is also the noun *road*,
which puts the path back inside a word game about proverbs. The Dutch
edition is the one where the decoys didn't need transposing at all, only
respelling: *duur* **is** *dyr*, and it even inflects the same way (*duur*
before a *het*-word, *dure* before a *de*-word or a plural, exactly as Swedish
picks between *dyrt* and *dyra*).

French carries the same *dear/expensive* pun and is the only edition where the
language itself keeps the two apart: **`cher` means one or the other depending
on which side of the noun it stands.** *Un cher ami* is a dear friend, *un ami
cher* is an expensive one. Every other edition puts its decoy adjective in
front of the noun; French had to move it behind, or the fake proverbs would
have read as declarations of friendship. Its four forms -- *cher, chère, chers,
chères* -- are also four spellings of **one sound**, so this is the first
edition whose agreement is invisible the moment you read it aloud.

Greek needs the most decoy forms of any edition -- **six**, because `ακριβός`
agrees in gender as well as number, and drags the article along with it (*ο
ακριβός σκύλος*, *η ακριβή γλώσσα*, *το ακριβό μήλο*, and three more in the
plural). It also shows that the 1996 "section" was quietly doing more work than
it looked: Greek predicate adjectives agree with the subject's gender, and a
section is precisely a gender-and-number equivalence class. That needed no new
code in 2026 because it was already there in 1996.

Polish takes five agreement slots but only four distinct shapes, since neuter
singular and non-virile plural are the same word (*drogie*) -- and it has no
article at all, so the adjective carries the whole agreement alone. One of its
forms isn't a suffix swap: *drog-i* becomes *dro**dz**y*, a stem alternation.
Polish also has a gender that exists only in the plural (virile *drodzy
przyjaciele* against non-virile *drogie ściany*), and the present tense
collapses it -- so the distinction is fully visible in the head, where it's
baked in, and fully invisible in the tail, where the engine would have to
resolve it. The 1996 `+` marker needed no change.

English, Dutch, Greek and Polish are the editions whose verbs conjugate for
the number of the subject, so they're the ones that carry inflection markers in
the data -- `+Empty vessels|make(s) the most noise`, `+Holle vaten|klink(t/en)
het hardst`, `+Οι τοίχοι|έχ(ει/ουν) αυτιά`. The marker holds **both** forms
rather than deriving one from the other, and Dutch is why that's not
over-engineering: an open syllable drops its doubled vowel, so the seam lands
mid-stem -- `ma(akt/ken)`, `ve(egt/gen)`. No suffix rule reaches that. Greek
adds the opposite argument from the other end -- its copula does *not* inflect
(*αυτός είναι* / *αυτοί είναι*), so the verb that is `(is/are)` and `(is/zijn)`
everywhere else carries no marker there at all. A derive rule would have had to
be taught that; a file carrying both forms simply never mentions it. Polish
closes the argument from the other side: `(jest/są)` is **suppletive** -- the
two forms share no letter at all, so there is nothing to derive from.

French settles it. Five verb classes in one file put the seam in five different
places -- `tomb(e/ent)`, `pourri(t/ssent)`, `crai(nt/gnent)`, `va(ut/lent)`,
`sui(t/vent)` -- and `faire` has no seam to find, since *fait* and *font* share
nothing. It also inverts the Greek finding exactly: there the copula was the
one silent verb in a language you could otherwise hear, while in French the
copula is the loudest thing in the file and the entire first conjugation, most
of the language, is marked on the page and inaudible in the ear.

## Provenance

`COOKIE.BAS`, written by Leif Claesson (**Liket of Goto10**) around 1996 in
PowerBASIC for DOS, where it mashed Swedish proverbs on every boot. Revived
2026 as Python and Rust console ports plus this page, which reads the same
data format the DOS original did -- the Swedish data file is still CP437,
decoded by a hand-spelled 128-entry table.

No frameworks, no build step, no backend: one HTML file and a handful of text
files.
