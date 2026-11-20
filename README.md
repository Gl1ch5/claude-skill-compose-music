# compose-music — Claude Code skill for writing piano music

A Claude Code / agent skill that composes original piano pieces in the style of classical
composers and delivers them as **MIDI + rendered MP3** (sampled grand piano).

Claude cannot emit audio directly, so the skill works the long way around: the agent writes
the notes as Python code → saves a MIDI file → renders it with a sampled piano SoundFont →
gives you the `.mid`, the `.mp3` and (optionally) a LilyPond score PDF.

## What's inside

```
.claude/skills/compose-music/
├── SKILL.md                 # the skill prompt: workflow + per-style recipes
└── scripts/
    ├── music.py             # core DSL: Piece.bar(chord, melody, accomp=...)
    ├── dsl_piano.py         # text DSL: chords, runs, trills, accelerando/ritardando
    ├── engine.py            # two-hand helpers (bass + chords, melody, octaves, runs)
    ├── render.py            # MIDI → MP3 via fluidsynth + Salamander Grand Piano
    ├── score.py             # MIDI → LilyPond → PDF piano score
    └── <piece>.py           # ready examples, one per style
```

Example pieces rendered by the skill are committed in the style folders
(`concerto/`, `etude/`, `fantasy/`, `improv/`, `mozart/`, `nocturne/`, `nocturne_eb/`,
`rachmaninoff/`, `tango/`, `vivaldi/`, `waltz/`, `eminem/`) — each as `.mid` and `.mp3`.

## Styles covered

Mozart / Classical · Chopin nocturne · Chopin revolutionary etude · Vivaldi / Baroque ·
Rachmaninoff · tango (Piazzolla spirit) · Shostakovich waltz · Eminem-style piano ostinato ·
programmatic multi-movement concert pieces.

## Requirements

```bash
apt-get update && apt-get install -y fluidsynth fluid-soundfont-gm lilypond ffmpeg
pip install pretty_midi music21 mido
```

`render.py` downloads the Salamander Grand Piano (CC-BY, ~300 MB) once and caches it,
falling back to FluidR3_GM if the download fails.

## Usage

```
compose a Chopin-style nocturne in E-flat, 2 minutes
```

The agent picks form and style, writes the piece with `scripts/music.py`,
runs `python scripts/render.py piece.mid piece.mp3`, checks the output is not silent
and hands back the files. Sheet music: `python scripts/score.py piece.mid score.pdf "Title"`.

All melodies are original — the styles are imitated, not copied.