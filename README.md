# NeuraTiM FrameOsu — the music library

384 tracks in 47 genres, 1901.6 MB, MP3 at 44.1 and 48.0 kHz, 0:41 to 7:59.

## Adding new music — this is the whole of it

**1. Drop the files in `unassigned/`.** It is git-ignored and is the only place new music
goes. Which of its two folders you use is what decides where each file ends up:

| Put it in | It becomes |
| --- | --- |
| `unassigned/app/` | **the app's own music** — ships inside the build, plays with no connection, needs a release |
| `unassigned/library/` | **this library** — downloaded on demand, live for everybody within a day of a push |

Anything dropped loose, or in a folder named anything else, is treated as `library/`, because
that is where a mistake is cheap. **Do not rename the files and do not strip their tags** —
the genre and the title are read out of the tags the generator wrote into them.

**2. Run this, in Claude Code, from anywhere in the workspace:**

```
/music-intake
```

No arguments, no questions. It reads every file's tags, works out the genre, the title and
the destination, renames and moves them, rewrites this README and both catalogues, runs the
tests, and reports what it did — including anything it could not identify, which it leaves in
`unassigned/` rather than guessing at. The procedure it follows is
`packages/frameosu/app/MUSIC_INTAKE.md`.

**3. Commit and push. It deliberately does not**, because the two halves land in two
repositories:

```sh
git -C packages/frameosu       add -A && git -C packages/frameosu       commit -m "music"
git -C packages/frameosu/music add -A && git -C packages/frameosu/music commit -m "music"
git -C packages/frameosu/music push
```

Pushing **this** repository is what makes a track live: the app re-reads `catalogue.json`
once a day, so it reaches every install within a day, and immediately for anybody who taps
*Check for new music*. The app's own music needs a build instead — it is in the bundle, and
nothing else could carry it.

*Doing it without the command: see* **Adding a track by hand** *at the end.*

**None of it ships inside the app, and that is the point.** NeuraTiM FrameOsu
carries music of its own — 22 pieces, 91.2 MB, committed with the app — so a
reel can be cut with sound before anything is downloaded. This repository is the
*other* library: everything here can be previewed immediately through one
replaceable temporary slot. An explicit download makes it an ordinary track that
can go under a reel with no connection at all.

**And the app reads `catalogue.json` once a day, so this list is the app's
list.** Add a track here and every installed copy of FrameOsu offers it within a
day; no release, no store review. Remove one and it stops being offered — without
ever taking away audio somebody has already downloaded. See *The catalogue*
below.

## Licence

Public domain — CC0 1.0. Composed for NeuraTiM with generative AI. No
attribution required, commercial use included. See `LICENSE`.

## Layout

```
tracks/<genre>/<genre>__some-words.mp3   — the words are the title
tracks/<genre>/<genre>_NN.mp3            — for a track that has no name
catalogue.json
```

## The catalogue

`catalogue.json` is what the app fetches, and it is **generated** — nothing in it
is typed by hand. The title comes out of the file's own name (the words after
`__`, title-cased), the length is read out of the MPEG frame headers, and the
byte count is the file's own. Do not edit it: add or remove a file and re-run the
generator (`app/tool/prepare_music.dart`).

```jsonc
{
  "schema": 1,          // stays 1 — see below
  "revision": 7,        // computed; only ever goes up, and only when something changed
  "generatedAt": "…",   // for a person reading the file; nothing decides by it
  "downloadBase": "…",  // every track's audio hangs off this
  "licence": "…",       // what a track carries unless it states its own
  "genres": [ { "id": "worldchill", "name": "World Chill", "category": "organicAndWorld" } ],
  "tracks": [ {
    "id": "bundled:worldchill__desert-breath",  // never changes — a saved reel stores it
    "title": "Desert Breath",
    "genre": "worldchill",                      // one of the genres above
    "path": "tracks/worldchill/worldchill__desert-breath.mp3",
    "seconds": 173,                             // required, and > 0
    "bytes": 4284358
  } ]
}
```

Four rules the app relies on, so a change here does not break an install:

1. **an id never changes.** A project stores it, so re-titling a track is fine
   and renaming its file is not — that is a new id and a new track. Changing a
   track's *audio* under the same id is likewise a new track, not an edit;
2. **`seconds` is required and greater than zero.** The length is what the Music
   screen filters by and the only thing somebody has to go on before spending the
   download, so a track that cannot state one is a measurement that failed. The
   generator refuses to publish it;
3. **`revision` only goes up.** The app refuses a document older than the one it
   holds, so a stale mirror cannot walk a device backwards. Publishing a rollback
   means bumping the revision on the way back — which the generator does for you,
   because it computes the number by comparing against the file it wrote last;
4. **`schema` stays 1.** Anything new is an *optional* key, which an older app
   ignores and a newer one reads, so a build from a year ago keeps getting
   today's tracks. A change that genuinely cannot be made that way is published at
   a new path (`catalogue-v2.json`) with this file left in place.

**Removing a track is safe.** It stops being offered; a copy already downloaded
keeps working, keeps its title and length, and is labelled as no longer in the
library. Nothing is deleted off anybody's device.

## Genres

| Genre | Folder | Tracks | Category in the app |
| --- | --- | --- | --- |
| Acoustic Folk | `acousticfolk` | 11 | organicAndWorld |
| Ambient | `ambient` | 20 | calmAndFocus |
| Big Beat | `bigbeat` | 6 | highVelocity |
| Boom Bap | `boombap` | 4 | urbanRhythm |
| Celtic Folk | `celtic` | 6 | organicAndWorld |
| Chillout | `chillout` | 5 | brandAndJourney |
| Chiptune | `chiptune` | 8 | electronicMotion |
| Cinematic | `cinematic` | 15 | screenAndScore |
| Dark Ambient | `darkambient` | 20 | calmAndFocus |
| Dark Folk | `darkfolk` | 7 | organicAndWorld |
| Darksynth | `darksynth` | 2 | modernSongs |
| Dark Techno | `darktechno` | 8 | electronicMotion |
| Deep House | `deephouse` | 4 | electronicMotion |
| Drum & Bass | `dnb` | 8 | highVelocity |
| Dream Pop | `dreampop` | 7 | modernSongs |
| EDM | `edm` | 6 | electronicMotion |
| Electro Pulse | `electropulse` | 2 | electronicMotion |
| Electro Swing | `electroswing` | 5 | globalGroove |
| Epic | `epic` | 8 | screenAndScore |
| Epic Vocal | `epicvocal` | 1 | modernSongs |
| Funk | `funk` | 18 | globalGroove |
| Future Garage | `futuregarage` | 6 | modernSongs |
| General | `general` | 19 | screenAndScore |
| Glitch Hop | `glitchhop` | 3 | electronicMotion |
| House | `house` | 12 | electronicMotion |
| Hybrid Trailer | `hybrid` | 7 | screenAndScore |
| IDM | `idm` | 4 | electronicMotion |
| Indie Electronic | `indieelectronic` | 11 | modernSongs |
| Indie Rock | `indierock` | 12 | modernSongs |
| Jazz | `jazz` | 21 | calmAndFocus |
| Lo-Fi | `lofi` | 10 | calmAndFocus |
| Medieval | `medieval` | 4 | screenAndScore |
| Melodic Techno | `melodictechno` | 2 | electronicMotion |
| Minimal Techno | `minimaltechno` | 8 | electronicMotion |
| Mono | `mono` | 4 | electronicMotion |
| Neoclassical | `neoclassical` | 18 | screenAndScore |
| Organic House | `organichouse` | 8 | globalGroove |
| Phonk | `phonk` | 2 | urbanRhythm |
| Post-Rock | `postrock` | 10 | screenAndScore |
| Soul | `soul` | 6 | modernSongs |
| Sport Rock | `sportrock` | 4 | highVelocity |
| Synthwave | `synthwave` | 8 | modernSongs |
| Trance | `trance` | 4 | electronicMotion |
| Trap | `trap` | 1 | urbanRhythm |
| Trip Hop | `triphop` | 2 | organicAndWorld |
| Western | `western` | 10 | organicAndWorld |
| World Chill | `worldchill` | 17 | organicAndWorld |

## What the app ships instead

The app's own music lives in the app's repository, at
`packages/frameosu/app/assets/music/`, and is **not** part of this library: it
is 91.2 MB of pieces cut for FrameOsu's own reels rather than a shelf anybody
browses, and putting it here would make every clone of this repository pay for
it. Its catalogue entries state an `asset` and no `path` — there is nowhere to
fetch them from, because they are already in the build.

| Style | Tracks | Category in the app |
| --- | --- | --- |
| Celtic Folk | 5 | organicAndWorld |
| Chillout | 2 | brandAndJourney |
| Cinematic | 2 | screenAndScore |
| Electro Pulse | 2 | electronicMotion |
| Epic | 2 | screenAndScore |
| Epic Vocal | 1 | modernSongs |
| Hybrid Trailer | 4 | screenAndScore |
| Medieval | 3 | screenAndScore |
| Neoclassical | 1 | screenAndScore |

## Adding a track by hand

What `/music-intake` does, if you would rather do it yourself. Put the file in
`tracks/<genre>/` for this library or in `app/assets/music/` for the app's own music, named
`<genre>__some-words.mp3` — the words become its title, and the whole name is the id, so
renaming it later is a new track — and re-run the generator from `packages/frameosu/app`:

```sh
dart run tool/prepare_music.dart ../music
```

It rewrites `catalogue.json`, this README and the app's own copy of the
catalogue, and bumps the revision if anything actually changed. It moves no audio:
both directories are sources. Five things are an error rather than a silent
default — a genre the app has no category for (add it to `_categories` in the
generator first), a file whose name does not start with its folder's genre, a
file whose name states no title, a file whose length cannot be read, and two
files anywhere that reduce to the same id.

Then **commit and push this repository** — the app fetches `main`, so a track is
live for everybody within a day of the push, and immediately for anybody who taps
*Check for new music*.

https://github.com/neuratim/frameosu_music
