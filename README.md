# proverb-masher

Real proverbs, chopped in half and glued back wrong -- in Swedish, English,
Thai, Russian, Dutch, Greek, Polish, French, German, Portuguese and Finnish.
The grammar stays right; the meaning gets mangled. That's the whole point.

> A dear friend is better than two new ones. · Don't say "hop" -- then love
> hauling the sled uphill too. · New brooms run deep.

**Live:** https://leifclaesson.github.io/proverb-masher/

Hit `[X] Auto` to leave it cycling on a monitor. `?auto=1` arms that from the
URL and `?auto=20` also sets the dwell in seconds -- the pause after a line
lands, so a long one is never cut short. The default dwell is 10 seconds. For
a screen with no keyboard in front of it. `?auto=` is not limited to the
Interval bar's 5-30 range: ask for `?auto=60` and the bar simply grows to show
it, rather than pinning at 30 while the timer runs a minute.

The `☼` button beside it opens the autocycle's settings. **Interval** sets the
dwell from 5 to 30 seconds. A switch decides
whether the cycle stays in one language or changes language as well, and a bar
per language sets how often each one comes up -- so a screen can run mostly
Swedish with the occasional Thai line, or an even shuffle across the lot. The
weights are **relative, not percentages**: every edition at 50 is the same even
shuffle as every edition at 100, and a language at zero simply never comes up.
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

German is the only edition whose decoy needed neither transposing nor
respelling *and* carries both meanings. *dyr*, *duur* and *teuer* are one
Germanic word in three spellings, but the first two mean only costly, while
*teuer* also means beloved -- *mein teurer Freund* -- the way `дорогой`,
*drogi* and *cher* do. English *dear* is the same word a fourth time with the
same two senses, so this is the one edition whose English mirror keeps the
decoy word untranslated: it says **dear**, not *expensive*, because there is
nothing to choose between. Its four nominative shapes also expose an axis no
earlier edition could show, since none of them had articles -- the ending
depends on the **determiner**, not just the noun: *der teure Apfel* against
*ein teurer Krug*, same word, same case, different ending. And *teuer* drops
its own *e* the moment an ending arrives.

German also runs every seam the marker was invented for, in one file: an
ending (`mach(t/en)`), a stem umlaut (`f(ällt/allen)`), an *e*→*i* raising
(`br(icht/echen)` -- the same verb in the same proverb as Dutch
`br(eekt/eken)`, cut in a different place for a different reason), and two
verbs with no shared letter to cut at (`(ist/sind)`, `(isst/essen)`).

Two things are new with German, and both are about **where the seam may
fall** rather than what the marker holds. First, the verb bracket: German puts
its finite verb second and shoves the rest of the verb to the end of the
clause, so a cut anywhere in the middle strands half a verb. The seam
therefore has exactly two legal positions -- a clause boundary, or immediately
after the one constituent before the finite verb. That is why its "don't do
this" group fronts the object (`Den Tag|soll man nicht vor dem Abend loben`)
instead of splitting the modal from its infinitive. Second, **case crosses the
seam**: the verb in the tail governs the case of the head, and the `+` marker
holds only number. No marker could fix it either -- the head would have to
ship in four cases, article and adjective included, which is a declension
table rather than a bracket. So the fix is the 1996 section itself: **every
group is a case class.** German's data file has six groups where every other
edition has five, and the sixth is shape-identical to the fifth -- it exists
only because those verbs take the dative.

And German is the first edition to hit something the machinery genuinely
**cannot** express, which its data file says out loud rather than
half-shipping. A German possessive stem agrees with the gender of the
*possessor*, and the possessor is in the head: *Jedes Ding hat **seine**
Zeit*, but *Die Hoffnung hat **ihre** Zeit*. `(seine/ihre)` carries number
only, so a glued line would be wrong half the time. English `(its/their)`,
Dutch `(zijn/hun)` and French all escape this, French for free -- there the
possessive agrees with the thing possessed. German doesn't escape, so no
German tail carries a possessive at all and the English mirror drops its
`(its/their)` to match. Two proverbs were cut for it. A gender axis was
possible and not worth a resolver change for two lines.

Portuguese shares French's decoy outright -- *caro* is *cher*, the same Latin
word under the same rule of position, so a *caro amigo* is dear and an *amigo
caro* is expensive, and its decoys stand behind the noun for the same reason
French's do. What it adds is the exact mirror of the French finding: *caro,
cara, caros, caras* are four different **sounds**, so this is the edition
where the agreement French writes and never says is finally audible. Two of
those four are also nouns, in the same spelling and in two different groups --
*Quem vê caras não vê corações* is faces, *As paredes caras têm ouvidos* is
the price. French has nothing like it; *chères* is not a word for anything.

It is written in the Portuguese of Portugal, and that is structural rather
than a matter of taste. The file is full of pronouns hung on the back of the
verb -- *acrescenta-lhe*, *avia-se*, *diz-me* -- which is not where Brazil
puts them. Negation drags the pronoun back in front (*não te rias*, never
*não ris-te*), and that needed no teaching: the 1996 groups are grammatical
equivalence classes, so the two placements sorted themselves into different
groups. Group 5 goes further and puts a pronoun *inside* a verb --
*dir-te-ei*, that is *dizer* + *te* + the future ending. Mesoclisis exists in
no other language on the site.

And Portuguese makes the strongest case in the whole project for a marker that
carries both forms rather than deriving one from the other, because **its seam
chases an accent** -- the Dutch doubled-vowel problem in a shape none of the
other eight editions has. What changes with the number is the *diacritic*, and
no suffix rule in the world reaches a diacritic: `d(á/ão)`, `v(ai/ão)`,
`v(ê/eem)`, `sa(i/em)`, and then `t(em/êm)` and `v(em/êm)`, where the two
forms differ by a circumflex and nothing else and are still pronounced
differently.

Finnish is the first edition from outside the Indo-European family, and the
decoy survives the jump: **`kallis` means both expensive and dear** -- *kallis
ystävä* is a dear friend -- exactly like *dyr*, *duur*, *teuer*, *dear*,
«дорогой», *drogi*, *cher* and *caro*, and this time it cannot be the same word
inherited. It is the same picture arrived at twice. It also needs the fewest
decoy forms of any edition, two against Greek's six, because Finnish has
neither gender nor articles.

What it does need is a marker that leaves the verb. Dutch and German let the
nominal half collide on purpose -- *Holle vaten zijn koning* -- because a wrong
noun reads as the joke where a wrong verb reads as a bug. Finnish gets no such
choice: **predicate adjectives agree obligatorily**, so *ovat sokea* looks like
a typo rather than a punchline, and the bracket moves one word to the right --
`(on/ovat) soke(a/ita)`, `(on/ovat) aina hankal(a/ia)`, `(on/ovat) puoliksi
teh(ty/tyjä)`. Nine editions had kept it inside the verb. The engine needed no
change, because `agree()` was always replacing every bracket rather than the
first.

Its seam is chosen by **vowel harmony**: `lakaise(e/vat)` against
`teke(e/vät)`, the same cut in the same place with a different suffix vowel,
decided by the vowels already sitting in that word. That is the Dutch
doubled-vowel argument for carrying both forms, made again from an unrelated
language family -- and unlike German case, harmony never crosses the seam,
because a suffix always attaches to its own word. It looks more frightening
than case and costs exactly nothing.

Two more things fall out of Finnish being Finnish. **Negation is a verb that
conjugates** (*ei* / *eivät*, with the main verb frozen in a connegative form),
so the same word needs a marker or doesn't depending on which side of the seam
it lands: group 1 carries `(ei/eivät)` in the tail, while group 5 puts *Ei* at
the front of the head and needs no marker at all. And **there is no verb "to
have"** -- possession is *seinillä on korvat*, "on the walls are ears" -- so
that whole family got its own group, the one group in the project whose head is
a *place* rather than a subject. It is the exact spot where German gave up: a
German possessive stem agrees with the possessor's gender, so no German tail
carries one and two proverbs were cut for it. Finnish `-nsa/-nsä` is identical
in singular and plural, so *aikansa* and *ristinsä* simply work, and only the
English mirror has to carry `ha(s/ve)` and `(its/their)`.

Case, the thing you would expect to break a language with fifteen of them, stays
out of the seam by itself. Every tail in the *joka* group opens with a resumptive
pronoun that takes whatever case the tail's own verb wants -- *se*, *sitä*,
*sen* -- while *joka* is nominative in the head because it is its own clause's
subject. And Finnish is the one edition where the word every other language
borrowed went somewhere else: the autocycle button cannot say *Auto*, because in
Finnish that is a car.

## Provenance

`COOKIE.BAS`, written by Leif Claesson (**Liket of Goto10**) around 1996 in
PowerBASIC for DOS, where it mashed Swedish proverbs on every boot. Revived
2026 as Python and Rust console ports plus this page, which reads the same
data format the DOS original did -- the Swedish data file is still CP437,
decoded by a hand-spelled 128-entry table.

No frameworks, no build step, no backend: one HTML file and a handful of text
files.
