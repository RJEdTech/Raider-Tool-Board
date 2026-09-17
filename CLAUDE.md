# CLAUDE.md: Raider Tool Board (Tool Bingo)

This repo publishes **only the interactive Tool Bingo card** at https://rjedtech.github.io/Raider-Tool-Board/. The card is embedded in the Canvas page `raider-tool-board` in the faculty PD course (course 5414).

Everything else on the Raider Tool Board lives in that Canvas page, behind the login:

- school totals
- the Department Cup
- Power Users
- Using Many Tools
- Steal This
- Rising Star
- Three Minutes of Fame

That page is generated privately by `build_canvas.py`, which is **not** in this repo.

## Privacy rule (non-negotiable)

This repo and its site are **public**, and so is every file in it.

- **No teacher names, no per-person numbers, no school or department usage numbers** anywhere in this repo. That includes commit messages. The only exception is "Jason Beyer" as the contact in the footer.
- Never commit `canvas-page*.html`, `canvas-block*.html` or `build_canvas.py`. They're in `.gitignore`.
- Named colleagues appear in Canvas only after they agree. `build_canvas.py --final` drops anyone who hasn't agreed.
- Nowhere, including Canvas: no bottom of any list, no zero-use accounts, no unpublished-work counts, and nothing about academic-integrity detection.
- Keep `<meta name="robots" content="noindex, nofollow">` and the disallow-all `robots.txt`.

## When this repo changes

Rarely. The monthly refresh happens in Canvas, not here. Edit `index.html` only when:

- the squares change (edit `RTB_CONFIG.squares` **and** bump `RTB_CONFIG.cardId` so saved marks reset)
- the claim survey URL changes (`RTB_CONFIG.claimUrl`)
- the Canvas page URL changes (`RTB_CONFIG.boardUrl`)
- the shared "More RJ Ed Tech tools" footer (between the `<!--RJTOOLS-->` markers) changes across the suite

## Build conventions

- One self-contained `index.html`. It has inline CSS and JS, and the RJ mark is a base64 image. Its only outside load is IBM Plex Mono from Google Fonts. **No `fetch()`, no `data.js`.**
- Look: charcoal `#2a2a2a` bands, RJ red `#c11430`, `#fafafa` cards, uppercase 10px tags, Segoe UI for text and IBM Plex Mono for numbers.
- When framed (`html.framed`), the page hides its own header band, section heading, footer and suite footer, because the Canvas page supplies those.
- The Canvas iframe is 960px tall. The card and How to play sit side by side at 761px and wider, and stack below that.
- The card is always 4×4, including on phones. Four in a **row or column** wins. Diagonals don't count.
- **Subject-specific tools:** a square can carry `variants`, each with `subjects` (`math`, `science`, `english` or `languages`), a short `tag`, `text`, `how` and `url`. Teachers pick "I teach" above the card, and it's saved as `rtb-subject`.
  - A subject-only tool (DeltaMath and Desmos for math, Gizmos for science, NoRedInk for English, VHL Central for languages) must never be a square everyone sees. Put it in a variant, and make the base square something any teacher can do.
  - Marks follow the slot, not the variant.
  - Keep the claim survey's answer list in step with every variant.
- Storage: marks go to `localStorage` key `rtb-bingo-<cardId>`. Every access is wrapped in try/catch, with an in-memory fallback, because third-party iframes and private windows may block storage.
- Deliver complete drop-in files, never patches. Before delivery:
  1. Run `node --check` on every inline `<script>` block.
  2. Run a stubbed-DOM boot test, including a `localStorage` that throws.
  3. Test the bingo logic: rows and columns win, diagonals don't, reset takes two taps, and each subject gets its own variants.
  4. Scan the diff for names.
