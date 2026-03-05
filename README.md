# cljd_mongol_hardware_keyboard

Mongolian IME (input method) for Flutter with **hardware keyboard** support, written in [ClojureDart](https://github.com/tensegritics/ClojureDart).

- **Latin → Mongol**: Type Latin letters (a–z), get live Mongol script preview and word candidates from an FST dictionary.
- **Next-word prediction**: After committing a word, show next-word candidates (from `assets/next.zip`).
- **Suffix suggestions**: After a Mongol word, get grammatical suffix candidates (e.g. case, possessive).
- **Single shared engine**: One global IME state; the active text field registers as client and receives input.

## Features

- Inline preview: Mongol text appears as you type (atomic replace, no dropped characters).
- Candidate list overlay: 5 candidates per page, select with **1–5** or **Space** for first.
- Page navigation: **=** / **+** next page, **-** previous page.
- Shortcuts: **Ctrl+Space** toggles IME on/off; **Shift+-** (or **_**) triggers suffix suggestions.
- **ESC** cancels current buffer and closes the overlay.

## Prerequisites

- [Flutter](https://flutter.dev) SDK
- [Clojure](https://clojure.org) (for `clj` / `deps.edn`)
- [ClojureDart](https://github.com/tensegritics/ClojureDart) (pulled via `deps.edn`)

## Project layout

```
m-h-k/
├── src/shared_ime/          # Shared IME library
│   ├── controller.cljd     # Key handling, state, commit/preview
│   ├── overlay_view.cljd   # Candidate overlay UI
│   ├── fst_reader.cljd     # FST dict load & query (LRU cache)
│   ├── mapping.cljd        # Latin → Mongol character mapping
│   ├── suffix.cljd         # Suffix suggestions (mongol_code)
│   ├── next.cljd           # Next-word prediction (from next.zip)
│   └── zip_utils.cljd      # Load JSON from asset ZIP + cache
├── example/                 # Flutter app using the IME
│   ├── src/mongol_ime_test/main.cljd
│   └── assets/
│       ├── fst/            # FST assets: token_table, dict.*, lists.*, keys.json, words.fst
│       ├── next.zip        # Next-word dictionary (JSON packed into a zip)
│       └── fonts/          # Mongolian fonts (Oyun*)
├── deps.edn
└── README.md
```

## Run the example app

From the repo root:

```bash
# Build Dart from ClojureDart (from project root)
clj -M:cljd compile

# Run Flutter app (example is the Flutter project)
cd example && flutter run
```

Ensure `example/assets/` contains:

- **FST**: `assets/fst/token_table.bin`, `dict.bin`, `dict.idx`, `lists.bin`, `lists.idx`, `keys.json`, `words.fst`
- **Next-word**: `assets/next.zip`
- **Fonts**: `assets/fonts/Oyun*.ttf`

The mapping in `mapping.cljd` should match your FST key encoding.

## Keyboard reference

| Key / combo        | Action                          |
|--------------------|---------------------------------|
| **a–z**            | Type latin → Mongol preview    |
| **Space**          | Commit first candidate         |
| **1–5**            | Select candidate by number     |
| **=** or **+**     | Next page of candidates        |
| **-** (no Shift)   | Previous page                  |
| **Shift + -** / **_** | Trigger suffix suggestions  |
| **Backspace**      | Delete last latin buffer char or pass through |
| **ESC**            | Cancel input, clear overlay    |
| **Ctrl + Space**   | Toggle IME on/off              |

## Dependencies (example app)

- **[mongol](https://github.com/suragch/mongol)** – Mongol script text field and layout.
- **[mongol_code](https://github.com/suragch/mongol_code)** – Mongolian suffix/grammar (e.g. case endings).

## License

MIT. See [LICENSE](LICENSE).
