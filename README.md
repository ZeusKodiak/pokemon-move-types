# Sammy's Pokédex

A one-page app with two tabs.

**Pokémon** — pick one from the filterable list (filter by type, or type a name
or Dex number) and it shows

- the region it comes from,
- its category — the "Mouse Pokémon" line the games print,
- its G-Max move, if it is one of the Gigantamax forms,
- whether it is out of the ordinary — legendary, mythical, an Ultra Beast or a
  Paradox Pokémon — which dresses the whole card,
- the Pokémon's own type(s),
- its evolution line, and what triggers each evolution,
- its attacking moves — the highest-power moves it can learn,
- its status moves, each with a one-line summary of what it does,
- the type of every move, plus power, accuracy and PP.

**Type matchups** — pick a type and it shows what that type is strong and weak
against, attacking and defending.

## Using it

It is live at **https://zeuskodiak.github.io/pokemon-move-types/** — open that on
a phone and "Add to Home Screen" for an app-like icon.

To run the local copy instead, open `index.html` in a browser. No install, no
build step, no server. Either way it needs an internet connection, because the
data comes live from [PokéAPI](https://pokeapi.co).

You can deep-link a Pokémon by adding it to the URL, e.g.
`https://zeuskodiak.github.io/pokemon-move-types/#gengar`, and a type's matchups
the same way with `#types/ghost`.

## Publishing changes

This repo is the published copy. GitHub Pages serves the live site straight
from `index.html` in its root on `main`, and it is kept public because Pages on
a private repo needs a paid plan.

The working copy lives in the private `super-duper-octo-happiness` repo under
`projects/pokedex/`; make changes there, copy `index.html` across to this repo's
root, and push to `main`. Pages rebuilds on its own within a minute or two.

The two copies drifted apart once — the evolution line was added here and only
brought back to the working copy later, so for a while a straight copy would
have deleted it. They are identical again, which is what makes the plain copy
safe. Keep it that way: if something ever has to be fixed directly here, bring
it back to the working copy before the next publish.

## How the move lists are picked

PokéAPI doesn't have a notion of a signature move or a competitive moveset, and
most Pokémon can learn well over a hundred moves. So the app takes the moves
learned by levelling up first (that list is the closest thing to a
characteristic moveset), tops it up with TM/tutor/egg moves, and splits the
result in two:

- **Attacking moves** — the twelve strongest by base power.
- **Status moves** — up to ten non-damaging moves, in the order above, since
  they have no power to rank by. Each shows PokéAPI's one-line effect summary,
  because "Swords Dance" on its own tells you nothing.

## Gigantamax forms

Gigantamaxing was the Sword and Shield trick where certain Pokémon grew huge and
changed shape for a few turns. Thirty-two species could do it, and PokéAPI lists
each one as its own entry — `charizard-gmax`, `pikachu-gmax` — so they show up in
the list like any other form.

**Their move tables used to come up empty.** The games keep one list of moves per
species rather than one per form, so PokéAPI has no moves recorded against
`charizard-gmax` at all — the app asked, got nothing, and showed nothing. It now
notices an empty list and falls back to the ordinary form's, which is what the
games mean: a Gigantamax Charizard knows exactly what a Charizard knows. The card
says whose list it is showing, so it doesn't look like the form has its own.

**The G-Max move** is the one thing the Gigantamax form really does have that the
ordinary one doesn't — a signature move replacing every move it knows of that
type. Gigantamax Charizard's fire moves all become G-Max Wildfire. Each card now
shows that move, its type, and what it does beyond damage, in a panel of its own
above the move tables. It isn't in the tables because it can't be taught and has
no fixed power of its own — it borrows the power of whatever move it replaces.

That table is hard-coded, which nothing else in this app is. It has to be:
PokéAPI has the eighteen generic Max Moves (Max Flare, Max Geyser) but **no G-Max
moves at all**, and nothing anywhere linking a Pokémon to its own. Unlike the
Paradox Pokémon further down, there is no clever way to derive it — the data
simply isn't there. It needs no upkeep, though: Gigantamaxing was dropped after
Sword and Shield, so those thirty-four form entries are the whole set, for good.

The table is keyed by form name rather than species because Urshifu's two forms
take different moves — G-Max One Blow for single strike, G-Max Rapid Flow for
rapid strike — while Toxtricity's two share one.

## The evolution line

Every stage of the family is shown left to right, with the one you looked up
outlined. Under each name is what triggers that evolution — a level, a stone, a
trade, high friendship, and so on — and where a Pokémon has more than one route
to the same evolution, both are listed ("Use Leaf Stone, or level up at moss
rock").

Families that branch, like Eevee's eight or Wurmple's two, stack their options
in one column. A long line scrolls sideways rather than squashing.

Tapping any member looks it up, so a whole family can be worked through without
typing. A Pokémon that doesn't evolve says so.

## Picking a Pokémon

The list under the search box holds every Pokémon PokéAPI knows about, so
nothing has to be spelled from memory — scroll it, or type a few letters to
narrow it. Matching ignores case, spaces and hyphens, so `mrmime` finds
`mr-mime`, and a Dex number works too. Names starting with what you typed sort
first, so `char` offers Charmander before Wartortle. Pressing Enter picks the
top match.

The list is capped at 200 rows on screen at a time to keep scrolling smooth on
a phone; the count above it says when there are more.

## Filtering by type

The row of type pills under the search box narrows the list to Pokémon of that
type. Tap a pill to switch it on, tap it again to switch it off. Selected pills
show in their type colour; the rest are greyed out.

**Clear** drops everything narrowing the list — the selected types *and* the
search box — so one tap always gets back to the full list. It appears whenever
either the box or a type is doing something, and hides when there is nothing
left to clear. It deliberately doesn't put the cursor in the search box: that
would open the keyboard on a phone, covering the list the tap was meant to
show.

Picking **two or more** types means "has all of these", not "has any of these" —
so fire + flying gives you Charizard and Moltres rather than every fire Pokémon
plus every flying one. Combinations nothing has, like fire + water, come back
empty.

**Only one type** is the tick box next to the heading. It hides every Pokémon
that has a second type, so fire on its own gives you Charmander and Growlithe
but not Charizard, who is also flying. Ticked with no type chosen, it lists every
one-type Pokémon there is.

While it is ticked the pills work one at a time: tapping a second type swaps to
it rather than adding it, because asking for something that is purely fire *and*
purely water could only ever come back empty. Tapping the lit pill again turns it
off. If two types were already chosen when the box is ticked, the first one
stays.

Typing in the search box works alongside the type filter, and both apply at
once: with fire selected, `char` gives just the Charmander line. The count above
the list names whichever filters are in play, so an empty list never looks like
a fault.

Type membership comes from PokéAPI's `/type` endpoint — one request per type,
which is why the list flickers to "Loading type list…" the first time a pill is
used. Each type is cached for the rest of the session, so switching back is
instant.

Nothing in that data says how many types a Pokémon has — being in the fire list
doesn't rule out also being in the flying one — so "only one type" works it out
by counting across all eighteen lists. Appearing in exactly one of them is what
having one type means. That makes the first tick slower than a pill, since it
needs all eighteen rather than one, and it says "Checking which Pokémon have one
type…" while it does. After that they are cached like any other, shared with both
the pills and the matchups tab, and the answer is worked out once.

## Type matchups

The second tab answers the question the move list raises: a Gengar with a ghost
move — is that any good against what you're facing?

Pick a type from the row of pills and it splits the answer into the two
questions actually being asked:

- **Attacking** — what a move of that type does: super effective (×2), not very
  effective (×½), no effect at all (×0).
- **Defending** — what a Pokémon of that type takes: weak to (×2), resists (×½),
  immune to (×0).

Anything not listed does normal damage, so the rows stay short — listing the ten
or so neutral types every time would bury the ones that matter. A row with
nothing in it says "Nothing", so it never looks like a loading failure.

Every type named in an answer is itself a pill you can tap, which looks that one
up next — so "ghost is weak to dark" leads straight to what dark is weak to,
without scrolling back to the buttons.

The tab opens on the type of whatever Pokémon is showing on the other tab: look
up Charizard, tap across, and it is already on fire.

Two-type Pokémon multiply both of their types together, which is why a
rock/flying one takes ×4 from an electric move. The app says so in a line under
the table rather than trying to work out every pair — the row of pills is for
one type at a time.

The numbers are PokéAPI's `damage_relations`, from the same `/type` request the
filter on the other tab already makes, so the two share a cache: filtering by
fire and then opening fire's matchups costs one request, not two.

## Categories

Under the dex number and region is the Pokémon's category — Pikachu is the
Mouse Pokémon, Bulbasaur the Seed Pokémon. It is the same line the games print
next to the height and weight, and it costs nothing to show: PokéAPI calls it a
*genus* and keeps it on the species record the region and the evolution line
already come from, so no extra request is made for it.

A genus is stored per language, so the app picks the English one. The handful of
entries with no English genus at all just leave the line out rather than showing
an empty one.

Category follows the species, not the form, which is what you want: Alolan
Raichu is a Mouse Pokémon like every other Raichu, even though its region line
says Alola.

## Legendary and mythical

The rare ones look rare. A legendary Pokémon's card turns gold — a gold edge, a
wash of gold fading down from the top, a soft glow around it, and a ✦ Legendary
badge beside the name. A mythical one does the same in violet, with ✧ Mythical.
Everything else looks exactly as it did.

The badge carries a slow sheen across it, once every few seconds rather than
constantly, so it glints instead of flashing. A phone set to reduce motion gets
the badge without the animation.

These are two separate flags on the species record — `is_legendary` and
`is_mythical` — so like the category they cost no extra request. 71 species are
legendary and 23 are mythical, and nothing in PokéAPI's data is both, so a card
never has to choose between the two treatments.

The split is roughly "hard to get" versus "impossible to get normally": the
legendaries are the ones standing at the end of a game (Mewtwo, Lugia, Rayquaza),
while the mythicals are the event-only two dozen — Mew, Celebi, Arceus and the
rest — that needed a download or a ticket to obtain.

This is deliberately not the same thing as *rarity*. PokéAPI has no rarity
field, and the closest alternatives are all worse for the job: `capture_rate`
measures how hard something is to catch rather than how scarce it is, and the
`habitat` field — which does have a value literally called `rare` — is only
filled in for generations I to III, so anything from Sinnoh onwards has none.
The two flags are complete across all 1,025 species, which is why they are what
the card uses.

## Ultra Beasts and Paradox Pokémon

Three more groups get the same treatment in their own colours: cyan for the
eleven **Ultra Beasts**, after the wormhole they come through, and the two
halves of the **Paradox** pairs split warm and cool — rust for the ten ancient
ones, a cold steel for the ten machines. The badges read ◈ Ultra Beast,
✸ Ancient Paradox and ⬡ Future Paradox.

The Paradox pairs are split rather than lumped together because the games split
them: Flutter Mane and Iron Moth are the same Pokémon reimagined in opposite
directions, and one badge covering both would read oddly to anyone who has
played. Nothing extra is needed to tell them apart — see below.

### How they are found without a list of names

PokéAPI records neither idea. There is no `is_paradox` or `is_ultra_beast`, and
the only species flags are legendary, mythical and baby. The obvious fix would
be to hard-code thirty-one names, the way the seven Hisui species are hard-coded
further down — but it isn't needed, because each group has a signature ability
that nothing outside the group has:

| Ability | Marks | Holders |
| --- | --- | --- |
| Beast Boost | Ultra Beast | all 11, and nothing else |
| Protosynthesis | Ancient Paradox | all 10, and nothing else |
| Quark Drive | Future Paradox | all 10, and nothing else |

That was checked against PokéAPI's own data rather than assumed: no strays, none
missing. So the ability *is* the group. Abilities are on the Pokémon record the
app already fetches, so this costs no extra request either — and unlike a list
of names, it needs no maintenance when a new game adds more of them.

It also survives a failure the flags don't: the abilities arrive with the
Pokémon itself rather than with the species, so an Ultra Beast still shows as
one even when the species request fails and the region and category go missing.

**Koraidon and Miraidon** are the exception. They are Paradox Pokémon in the
story, but each carries its own one-off ability instead of the shared two, so
this test doesn't catch them. They are both flagged legendary, and a species
flag deliberately outranks an ability, so they stay gold — being the legendary a
whole game is built around is the more useful thing to say about them than which
end of time they came from.

## Regions

PokéAPI records a *generation*, not a region, so the app maps one to the other —
generation I is Kanto, II is Johto, and so on through to IX being Paldea. That
is the region a species was introduced in, which is not always where you can
catch it in a given game.

Two things that mapping gets wrong on its own, both corrected:

**Regional variants.** A variant is a *form* of an older species, so the
species' generation points at the wrong place — Alolan Raichu would come out as
Kanto. The form's region is in its name (`raichu-alola`, `meowth-galar`,
`growlithe-hisui`, `wooper-paldea`), so the app reads it from there and shows
both: "Alola region · originally Kanto". Two exceptions are handled by name —
Hisuian Basculin is `basculin-white-striped` rather than `-hisui`, and Ash's cap
Pikachu (`pikachu-alola-cap`) is a Kanto Pikachu in a hat, not an Alolan one.

**Generation VIII covers two regions.** Most of it is Galar, but Legends:
Arceus introduced Wyrdeer, Kleavor, Ursaluna, Basculegion, Sneasler, Overqwil
and Enamorus in Hisui. Those seven are listed explicitly; the rest of the
generation stays Galar.

## Notes

- Whichever tab you are on puts itself in the URL, so the browser's Back button
  steps back through the Pokémon you looked up and the tabs you switched
  between, and any point in that is a link you can share.
- Alternate forms need their full name, e.g. `deoxys-normal`, `giratina-origin`.
  The list shows these in full, so picking from it avoids the problem.
- Move details are one HTTP request each, so the first lookup for a Pokémon
  takes a moment. Results are cached for the rest of the session.
- The evolution line costs two more requests (the species, then its chain). The
  pictures in it don't cost anything extra — they're addressed by Dex number.
- Evolution data is PokéAPI's, which flattens every game into one chain, so a
  method that only applies to one game still shows up.
