# Sammy's Pokédex

A one-page app: pick a Pokémon from the filterable list — filter by type, or
type a name or Dex number — and it shows

- the region it comes from,
- the Pokémon's own type(s),
- its evolution line, and what triggers each evolution,
- its attacking moves — the highest-power moves it can learn,
- its status moves, each with a one-line summary of what it does,
- the type of every move, plus power, accuracy and PP.

## Using it

It is live at **https://zeuskodiak.github.io/pokemon-move-types/** — open that on
a phone and "Add to Home Screen" for an app-like icon.

To run the local copy instead, open `index.html` in a browser. No install, no
build step, no server. Either way it needs an internet connection, because the
data comes live from [PokéAPI](https://pokeapi.co).

You can deep-link a Pokémon by adding it to the URL, e.g.
`https://zeuskodiak.github.io/pokemon-move-types/#gengar`.

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
search box — so one tap always gets back to the full list, and the cursor lands
back in the box ready to type again. It appears whenever either the box or a
type is doing something, and hides when there is nothing left to clear.

Picking **two or more** types means "has all of these", not "has any of these" —
so fire + flying gives you Charizard and Moltres rather than every fire Pokémon
plus every flying one. Combinations nothing has, like fire + water, come back
empty.

Typing in the search box works alongside the type filter, and both apply at
once: with fire selected, `char` gives just the Charmander line. The count above
the list names whichever filters are in play, so an empty list never looks like
a fault.

Type membership comes from PokéAPI's `/type` endpoint — one request per type,
which is why the list flickers to "Loading type list…" the first time a pill is
used. Each type is cached for the rest of the session, so switching back is
instant.

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

- Alternate forms need their full name, e.g. `deoxys-normal`, `giratina-origin`.
  The list shows these in full, so picking from it avoids the problem.
- Move details are one HTTP request each, so the first lookup for a Pokémon
  takes a moment. Results are cached for the rest of the session.
- The evolution line costs two more requests (the species, then its chain). The
  pictures in it don't cost anything extra — they're addressed by Dex number.
- Evolution data is PokéAPI's, which flattens every game into one chain, so a
  method that only applies to one game still shows up.
