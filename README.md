# NeuraTiM FrameOsu — the music library

141 tracks in 22 genres, 875.8 MB, all
192 kbit/s MP3 at 44.1 kHz.

This is the whole library. **NeuraTiM FrameOsu ships 16 of them**
and fetches any of the rest on demand: pressing play on a track the app has not
got downloads it from here and then plays it, and from that moment it is an
ordinary track that can be put under a reel. The app never needs this repository
to run — everything already downloaded, imported or bundled works with no
connection at all.

## Licence

Public domain — CC0 1.0. Composed for NeuraTiM with Fable AI. No attribution
required, commercial use included. See `LICENSE`.

## Layout

```
tracks/<genre>/<genre>_NN.mp3
tracks/<genre>/<genre>_NN__some-words.mp3   — a track that has a name
manifest.json
```

`manifest.json` is generated and is what the app's own catalogue is built from.
Nothing in it is typed by hand: the title comes out of the file's own name — the
genre and the track's number inside it, or the words after `__` for the few
tracks that have a name of their own — the duration is read out of the MPEG frame
headers, and the byte count is the file's own. Do not edit it — add or remove a
file and re-run the generator (`app/tool/prepare_music.dart`).

## Genres

| Genre | Folder | Tracks | Category in the app |
| --- | --- | --- | --- |
| Ambient | `ambient` | 7 | calmAndFocus |
| Big Beat | `bigbeat` | 3 | highVelocity |
| Chillout | `chillout` | 9 | brandAndJourney |
| Cinematic | `cinematic` | 9 | screenAndScore |
| Darksynth | `darksynth` | 5 | modernSongs |
| Deep House | `deephouse` | 9 | electronicMotion |
| Drum & Bass | `dnb` | 8 | highVelocity |
| Dubstep | `dubstep` | 3 | highVelocity |
| EDM | `edm` | 11 | electronicMotion |
| Epic | `epic` | 6 | screenAndScore |
| Future Garage | `futuregarage` | 6 | modernSongs |
| Hard Techno | `hardtechno` | 3 | highVelocity |
| House | `house` | 7 | electronicMotion |
| Lo-Fi | `lofi` | 9 | calmAndFocus |
| Melodic Techno | `melodictechno` | 6 | electronicMotion |
| Phonk | `phonk` | 3 | urbanRhythm |
| Reggaeton | `reggaeton` | 6 | globalGroove |
| Sport Rock | `sportrock` | 3 | highVelocity |
| Synthwave | `synthwave` | 9 | modernSongs |
| Trap | `trap` | 9 | urbanRhythm |
| Trip Hop | `triphop` | 4 | organicAndWorld |
| World Chill | `worldchill` | 6 | organicAndWorld |

## What the app ships

One track per genre for fifteen genres, chosen to cover all nine of the app's
categories, and the shortest track in each — the smallest bundle for the same
coverage. Anything after those fifteen ships because it was asked for by name
(`_bundledTracks` in the generator), not as a genre's representative.

| Track | File | Length |
| --- | --- | --- |
| Ambient 02 | `tracks/ambient/ambient_02.mp3` | 204 s |
| Chillout 05 | `tracks/chillout/chillout_05.mp3` | 196 s |
| Cinematic 06 | `tracks/cinematic/cinematic_06.mp3` | 218 s |
| Deep House 09 | `tracks/deephouse/deephouse_09.mp3` | 184 s |
| Drum & Bass 08 | `tracks/dnb/dnb_08.mp3` | 182 s |
| EDM 08 | `tracks/edm/edm_08.mp3` | 183 s |
| Epic 03 | `tracks/epic/epic_03.mp3` | 182 s |
| Future Garage 04 | `tracks/futuregarage/futuregarage_04.mp3` | 266 s |
| House 02 | `tracks/house/house_02.mp3` | 193 s |
| Lo-Fi 08 | `tracks/lofi/lofi_08.mp3` | 204 s |
| Reggaeton 06 | `tracks/reggaeton/reggaeton_06.mp3` | 176 s |
| Sport Rock 01 | `tracks/sportrock/sportrock_01.mp3` | 201 s |
| Synthwave 05 | `tracks/synthwave/synthwave_05.mp3` | 233 s |
| Trap 07 | `tracks/trap/trap_07.mp3` | 209 s |
| World Chill 01 | `tracks/worldchill/worldchill_01.mp3` | 212 s |
| First Cut | `tracks/house/house_07__first-cut.mp3` | 180 s |

## Adding a track

Drop the file in `tracks/<genre>/`, named `<genre>_NN.mp3` with the next free
number — or `<genre>_NN__some-words.mp3` if it has a name of its own, which
becomes its title — and re-run the generator from `packages/frameosu/app`:

```sh
dart run tool/prepare_music.dart ../music
```

It rewrites `manifest.json`, this README and the app's catalogue. A genre folder
the app has no category for is an error rather than a silent default — add it to
`_categories` in the generator first.

https://github.com/neuratim/frameosu_music
