# Sammy's Pokédex

A one-page app: pick a Pokémon from the filterable list — or type a name or Dex
number — and it shows

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

The live site is served by GitHub Pages from the separate public repo
[`ZeusKodiak/pokemon-move-types`](https://github.com/ZeusKodiak/pokemon-move-types),
because Pages on a private repo needs a paid plan. This folder is the working
copy; to publish an edit, copy `index.html` across to that repo's root and push
to its `main`. Pages rebuilds on its own within a minute or two.

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

## Regions

PokéAPI records a *generation*, not a region, so the app maps one to the other —
generation I is Kanto, II is Johto, and so on through to IX being Paldea. That
is the region a species was introduced in, which is not always where you can
catch it in a given game.

## Notes

- Alternate forms need their full name, e.g. `deoxys-normal`, `giratina-origin`.
  The list shows these in full, so picking from it avoids the problem.
- Move details are one HTTP request each, so the first lookup for a Pokémon
  takes a moment. Results are cached for the rest of the session.
- The evolution line costs two more requests (the species, then its chain). The
  pictures in it don't cost anything extra — they're addressed by Dex number.
- Evolution data is PokéAPI's, which flattens every game into one chain, so a
  method that only applies to one game still shows up.

## About this repo

This is the published copy, kept public so GitHub Pages can serve it for free.
The working copy lives in the private `super-duper-octo-happiness` repo under
`projects/pokedex/`; changes should be made there and copied across.
