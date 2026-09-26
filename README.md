# 🎹 Piano Buddy

A little browser piano I built for fun — you play it, there's a cute pink blob that reacts when you hit a key, and there's a practice mode with a few classic tunes if you want something to work toward. Everything lives in a single HTML file, so there's nothing to install.

## What it does

- **A real playable piano** — two octaves plus one extra note, and it actually behaves like a piano: hold a key and it sustains, let go and it fades out naturally, instead of just firing a fixed "beep."
- **Keyboard shortcuts** so you don't have to click every note — `A S D F G H J K` for the white keys and `W E T Y U` for the black ones in between, mapped to the first octave.
- **A practice/challenge mode.** Pick one of five songs (Twinkle Twinkle, Mary's Lamb, Hot Cross Buns, Jingle Bells, Ode to Joy), and it shows you the next note to play. Get it right and it turns green, get it wrong and it flashes red without moving on — and it keeps score as you go, so you get a final accuracy percentage at the end.
- **A mascot that just vibes.** It's not doing anything functional — it idles, blinks now and then, and does a little squish when you play a note, mostly so the app feels a bit alive. It never plays on its own.
- **No audio files.** Every note is synthesized on the fly with the Web Audio API, so there's nothing to download or load — it's just math generating a waveform in real time.

## Running it

There's no build step. Clone it and open the file:

```bash
git clone <your-repo-url>
cd <your-repo>
open piano-buddy.html      # macOS
start piano-buddy.html     # Windows
xdg-open piano-buddy.html  # Linux
```

That's genuinely it. No `npm install`, no server, no framework.

## A quick tour of the code, if you're poking around

I tried to keep this readable rather than clever, so a few notes on the parts that might not be obvious at a glance:

- The keyboard isn't hand-typed key by key — the note frequencies are calculated from actual music theory (`440 × 2^(semitones/12)`, standard equal temperament), so adding more octaves later is just a config change, not a rewrite.
- Holding a note down works by tracking currently-sounding notes in a plain object (`activeNotes`), keyed by note name. Press starts an oscillator, release ramps its volume down and stops it — that's basically the whole trick behind it feeling like an instrument instead of a sample player.
- The challenge songs are just arrays of note names. The scoring logic listens in on the same code path that free play already uses, so it wasn't really a separate feature to build — it's just comparing what you played to what was expected.

## Want to add a song?

Find `CHALLENGES` near the bottom of the script and add an entry:

```js
const CHALLENGES = {
  // ...existing songs
  mySong: { label: 'My Song', notes: ['C4','E4','G4','C5'] },
};
```

Any note currently on the keyboard works — white keys `C4`–`C6`, black keys like `C#4` or `F#5`.

## Changing the colors

Everything's driven by a handful of CSS variables at the top of the `<style>` block (`--bg1/2/3`, `--panel`, `--accent`, `--ink`, `--panel-ink`). Swap those and the whole thing reskins itself — I didn't hardcode colors anywhere else.

## Files

```
.
├── piano-buddy.html   # the whole app, HTML/CSS/JS in one place
└── README.md
```

## License

Pick whatever license fits you (MIT is a fine default if you're not sure). The five melodies are old public-domain folk tunes, so no attribution needed there.
