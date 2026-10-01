# Harmonicon lessons

The lesson curriculum for [Harmonicon](https://github.com/tcanabrava/harmonicon),
a rhythm game you play with a real harmonica. The game downloads this
repository on first start (only the latest commit) and offers to update it
from its Options page whenever there are new commits.

Lessons live here, not in the game, so they can change without waiting for a
game release. Anyone can publish lessons the same way: a git repository laid
out like this one can be added to the game from Options.

## Layout

```
pack.json                       what this is and which game it needs
locales/<lang>.ftl              names shared across lessons: units, tracks
<unit>/<lesson>/lesson.json     one lesson (schema: assets/lesson_schema.dtd.json
                                in the game repository)
<unit>/<lesson>/locales/<lang>.ftl   that lesson's text
<unit>/<lesson>/song/*.harpchart     its drill, if it has one
scripts/                        tools that generate some of the charts
```

The `01_`/`02_` prefixes on unit and lesson folders are the curriculum order.
A lesson's `id` is how the game records progress and how other lessons name it
as a prerequisite, so an id that has shipped must never change.

## Translations

Text is [Fluent](https://projectfluent.org/). `lesson.json` holds message
*keys* (`title_key`, `body_key`, a widget's `title_key`), defined in the
`locales/` folder beside the lesson; unit and track names
(`lesson-unit-<unit>`, `lesson-track-<track>`) are defined at the root.
`en-US` is required everywhere, since it is what a player whose language is
missing falls back to, and every other language must define every key `en-US`
does. Prefix a lesson's keys with its id so they cannot collide with another
lesson's or the game's.

## Compatibility

`pack.json`'s `requires` says which game can read this pack:

- `harmonicon`: a semver range on the game's version.
- `lesson_format`: the newest lesson format a lesson here uses. When a lesson
  starts using something new in the game (a widget or pass criterion added in
  a later release), raise this to that release's `LESSON_FORMAT_VERSION`, so
  older games refuse the pack with a clear message instead of failing on it.

## Checking a change

```sh
# from a checkout of the game
cargo run --bin validate-pack -- ../harmonicon-lessons
cargo run --bin validate-pack -- --choice-report ../harmonicon-lessons
```

CI runs the first on every push and pull request. To play a change before
publishing it, point the game at this folder instead of the published
repository: in the game's `settings.json`, set

```json
"content_sources": { "lessons": [{ "path": "/path/to/harmonicon-lessons" }] }
```

A local folder is read in place, so edits show up without committing.

## License

MIT, like the game; see `LICENSE`.
