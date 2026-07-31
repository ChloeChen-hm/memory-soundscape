# memory-soundscape
<div align="center">

# Memory Soundscape

### Turn meaningful photographs into editable, AI-composed soundscapes.

Memory Soundscape combines an image, a few words, and a curated audio library to create an atmosphere you can hear—not just see.

<br />

<!-- Replace this file with your main product screenshot or banner. -->
<img src="./docs/images/hero.png" alt="Memory Soundscape gallery and sound mixer" width="900" />

<br />

[Features](#features) · [How it works](#how-it-works) · [Getting started](#getting-started) · [Tech stack](#tech-stack)

</div>

---

## What is Memory Soundscape?

Photographs preserve how a moment looked. Memory Soundscape explores how that moment might have **felt and sounded**.

Upload a photograph, describe the memory in your own words, and the app uses Google Gemini to interpret its possible mood and atmosphere. It then searches a curated sound library and assembles a layered mix of ambience, texture, music, and accents.

The generated result is not fixed. You can preview individual sounds, enable or remove layers, adjust volume and timing, play the complete arrangement, and save the finished memory to a personal gallery.

> Memory Soundscape creates an artistic interpretation—not a claim about exactly what happened or what was originally audible.

<!-- Replace this GIF with a short recording of the complete flow. -->
<p align="center">
  <img src="./docs/images/demo.gif" alt="Creating and playing a memory soundscape" width="900" />
</p>

## Why it is interesting

Most photo applications organize memories visually. Memory Soundscape adds another dimension: **sound as a way to revisit atmosphere**.

It is useful as:

- A reflective tool for personal storytelling and digital journaling
- A creative prototype for multisensory photo albums
- An accessible way to experiment with AI-assisted audio composition
- A demonstration of structured multimodal AI working with a real, editable media system

Rather than asking an AI model to produce a vague description, the app turns its analysis into a practical arrangement: selected audio files, roles, start times, durations, fades, loop behavior, and volume levels.

## Features

### Multimodal memory analysis

Gemini considers both the uploaded photograph and the user's written description to suggest a cautious title, summary, mood, energy level, search tags, and target mix direction.

### Curated sound matching

The app searches a local catalogue of tagged audio files instead of relying on arbitrary generated audio. Candidate sounds include metadata such as category, duration, BPM, beat positions, loudness, and loopability.

### Layered AI arrangement

A soundscape is assembled from three or four complementary roles:

- **Bed** — the continuous environmental foundation
- **Texture** — subtle detail that gives the scene character
- **Music** — an optional emotional or rhythmic layer
- **Accent** — a smaller event that adds movement or focus

### Editable sound mixer

Users remain in control after generation. Each layer can be previewed, enabled or disabled, repositioned, and adjusted in volume before the full soundscape is played or saved.

<!-- Replace with a close-up screenshot of the mixer. -->
<p align="center">
  <img src="./docs/images/mixer.png" alt="Editable Memory Soundscape audio mixer" width="760" />
</p>

### Persistent memory gallery

Saved memories are stored in SQLite with their photograph, story, AI interpretation, and complete audio arrangement. Each gallery entry can be reopened and replayed later.

### Timing-aware playback

The browser audio engine supports scheduled entrances, source offsets, playback durations, looping, fades, and multi-track playback on a shared timeline.

## How it works

```text
Photograph + written memory
            │
            ▼
Gemini scene interpretation
            │
            ▼
Local sound-catalogue search
            │
            ▼
Gemini structured mix plan
            │
            ▼
Validation and timing adjustments
            │
            ▼
Editable browser sound mixer
            │
            ▼
Saved SQLite gallery entry
```

1. The user uploads a JPEG, PNG, or WebP image and adds a short description.
2. Gemini returns structured scene information without inventing specific events.
3. The app searches the local sound catalogue using the model's tags, categories, mood, and energy.
4. Gemini chooses a small set of candidates and proposes a structured arrangement.
5. The server validates the mix plan and connects every selection to a real audio file.
6. The user edits and previews the soundscape in the browser.
7. The photograph, description, interpretation, and final track settings are saved to SQLite.

## Screenshots

Add your images to `docs/images/` using these names, or update the paths in this README.

| File | Suggested content |
| --- | --- |
| `hero.png` | A wide screenshot showing the strongest overall view of the app |
| `demo.gif` | A 15–30 second flow: upload → describe → generate → edit → play |
| `mixer.png` | A close-up of the editable track controls |
| `gallery.png` | Several saved memories displayed in the gallery |
| `memory-detail.png` | A saved memory with its image, story, and playable soundscape |

A simple recording sequence for the GIF:

1. Open the create page.
2. Upload a visually interesting photograph.
3. Enter a short memory description.
4. Generate the soundscape.
5. Preview one layer and adjust another.
6. Play the complete mix.
7. Save it and open the gallery entry.

## Getting started

### Prerequisites

- Node.js 20 or later
- npm
- A Google Gemini API key

### Installation

```bash
git clone <your-repository-url>
cd memory-soundscape
npm install
```

Create a `.env.local` file in the project root:

```bash
GEMINI_API_KEY=your_api_key_here
GEMINI_MODEL=gemini-3.1-flash-lite
```

The model setting is optional; the application uses `gemini-3.1-flash-lite` by default.

Start the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Sound catalogue

Audio files live in `public/audio/`, while metadata is maintained in `data/sounds.csv` and compiled into `data/sounds.json`.

To rebuild the JSON catalogue after adding or editing sounds:

```bash
npm run build:sounds
```

To import or update those sounds in the SQLite database:

```bash
npm run import:sounds
```

Each catalogue entry can include:

```text
id, duration, tags, bpm, beatOffset, beatTimes,
loudnessLufs, category, loopable, file
```

The corresponding MP3 filename must match the sound ID—for example, a sound with the ID `rain-window` should be stored as `public/audio/rain-window.mp3`.

## Available scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the Next.js development server |
| `npm run build` | Create a production build |
| `npm run start` | Run the production server |
| `npm run lint` | Run ESLint |
| `npm run build:sounds` | Validate the CSV and generate `data/sounds.json` |
| `npm run import:sounds` | Import or update the sound catalogue in SQLite |

## Tech stack

- **Next.js 16** and the App Router
- **React 19**
- **Google Gemini** through `@google/genai`
- **Web Audio API** for layered, scheduled playback
- **SQLite** with `better-sqlite3`
- **Tailwind CSS 4**
- **JavaScript / JSX**

## Project structure

```text
memory-soundscape/
├── app/
│   ├── api/analyze/       # Gemini scene analysis and mix planning
│   ├── api/memories/      # Save-memory endpoint
│   ├── create/            # Memory creation experience
│   ├── memories/[id]/     # Saved memory detail page
│   └── page.js            # Memory gallery
├── components/
│   ├── CreateMemoryForm.js
│   ├── Header.js
│   └── SoundMixer.js
├── data/
│   ├── sounds.csv
│   └── sounds.json
├── database/              # Local SQLite database
├── lib/
│   ├── candidateSearch.js
│   ├── memories.server.js
│   ├── mixValidation.js
│   ├── soundCatalog.server.js
│   └── soundscapeEngine.js
├── public/
│   └── audio/             # Curated MP3 library
└── scripts/               # Catalogue build and import tools
```

## Privacy and responsible use

When a soundscape is generated, the selected photograph and written description are sent to the configured Google Gemini service for analysis. Do not upload sensitive images unless you are comfortable with that processing and have reviewed the applicable provider terms.

The AI-generated title, summary, and sound arrangement should be treated as creative suggestions. They may be inaccurate and are deliberately presented as interpretations rather than factual reconstructions.

Saved application data is stored in the project's local SQLite database in the current prototype.

## Future ideas

- User accounts and private cloud galleries
- Sharing a memory through a public link
- Exporting a soundscape as an audio file or video
- Recording a spoken reflection alongside the generated ambience
- Searching memories by mood, place, person, or sound
- Alternative soundscape versions for the same photograph
- Accessibility controls and reduced-motion playback views

## Acknowledgements

Memory Soundscape is inspired by the idea that memories are multisensory. A single image can carry traces of weather, movement, distance, rhythm, and emotion—and sound can offer a new way to explore those traces.

---

<p align="center">
  Built as an experiment in memory, storytelling, and human-directed AI creativity.
</p>
