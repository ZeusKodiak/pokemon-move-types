# Pokémon Move Types

A one-page app: type a Pokémon's name (or Dex number) and it shows

- the Pokémon's own type(s),
- its attacking moves — the highest-power moves it can learn,
- its status moves, each with a one-line summary of what it does,
- the type of every move, plus power, accuracy and PP.

## Running it

Open `index.html` in a browser. That's it — no install, no build step, no
server. It needs an internet connection because the data comes live from
[PokéAPI](https://pokeapi.co).

You can deep-link a Pokémon by adding it to the URL: `index.html#gengar`.

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

## Notes

- Alternate forms need their full name, e.g. `deoxys-normal`, `giratina-origin`.
- Move details are one HTTP request each, so the first lookup for a Pokémon
  takes a moment. Results are cached for the rest of the session.

## About this repo

This is the published copy, kept public so GitHub Pages can serve it for free.
The working copy lives in the private `super-duper-octo-happiness` repo under
`projects/pokedex/`; changes should be made there and copied across.
