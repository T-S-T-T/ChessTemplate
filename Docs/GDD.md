# Chess Template: Game Design Document

Living copy of the GDD. The editable master is the shared doc "Chess Template GDD"; keep this file in sync when decisions change.

A modular chess base: standard chess works out of the box, and rules, pieces, boards and presentation can each be changed without breaking the others.

## Vision and scope

The template is a complete, playable game of standard chess built so that changing one thing (a rule, a piece, the board, the look) means editing one place.

- **In scope (v1):** standard 8x8 chess, legal move validation, check/checkmate/stalemate, all draw rules, local two-player play, move history, undo, and a basic computer opponent.
- **Out of scope (v1):** online multiplayer, accounts and ratings, tournaments, monetisation.
- **Primary user:** the designer, altering the game over time. Players come second, so clarity of structure beats polish.

## Design pillars

1. **Rules are data, not code paths.** Piece movement, board size, start position and win conditions live in config, so a new piece or variant is a new entry, not a rewrite.
2. **Separation of layers.** Game state, rules, input, rendering and AI never reach into each other directly; they talk through a small set of events.
3. **Standard chess is the default preset.** Every alteration is a named preset layered on top, so the original is always one switch away.
4. **Everything is replayable.** A game is a start position plus a move list, which makes testing, undo, saving and AI all use the same record.
5. **Test the rules first.** A suite of known positions (perft counts, famous mates, edge-case draws) protects the base every time something is altered.

## Core rules (each is a toggle with a default)

| Rule | Default | Why it is a toggle |
| --- | --- | --- |
| Board size | 8x8 | Variants use other sizes or shapes |
| Piece set | K Q R B N P | New or removed pieces |
| Castling | On | Some variants drop it |
| En passant | On | Some variants drop it |
| Pawn promotion | Q, R, B, N on last rank | Different targets or ranks |
| Check enforcement | Must escape check | Variants where the king can be captured |
| Win condition | Checkmate | King of the Hill, capture all, race to a square |
| Stalemate | Draw | Some variants count it as a win or loss |
| Draw rules | 50-move, threefold, insufficient material | Individually switchable |
| Turn order | Alternate | Multi-move or simultaneous turns |

## Alteration points (each future change lands in one module)

| Module | Owns | Example alteration | Project folder |
| --- | --- | --- | --- |
| Board definition | Size, shape, blocked squares, zones | 10x8 board, holes, hexagonal grid | Scripts/Boards, Data/Boards |
| Piece definitions | Move patterns, capture rules, value, art | Add an Archbishop or a one-way rook | Scripts/Pieces, Data/Pieces |
| Setup / presets | Starting layout, active rule toggles | Chess960, handicap starts | Scripts/Presets, Data/Presets |
| Rules engine | Move generation, legality, check logic | Capture-the-king, extra-turn rules | Scripts/Rules |
| End conditions | Win, loss, draw checks | Reach the centre, lose all pieces | Scripts/Rules |
| Game flow | Turns, clocks, timers | Increment, bonus moves, fog of war | Scripts/Core |
| Input | Click, drag, keyboard, touch | Gamepad or voice control | Scripts/Input |
| Presentation | Board and piece art, animation, sound | Themes, 3D board | Scripts/UI, Art, Audio, Data/Themes |
| Opponent | Move chooser | Random, minimax, external engine | Scripts/AI |
| Persistence | Save, load, move notation | PGN/FEN import and export | Scripts/Persistence |

## Modes and variant ideas

- **Modes (v1):** local two-player, player vs computer, sandbox board for setting up positions.
- **Variants to host without core changes:** Chess960, King of the Hill, Three-check, Atomic, Racing Kings, fairy pieces (Archbishop, Chancellor, Amazon), custom boards (larger, smaller, holes).

## UX and presentation

- **Board:** coordinates shown, last-move highlight, check highlight, flip board option.
- **Moving:** click-to-select with legal-move dots, plus drag and drop.
- **Feedback:** capture and check sounds, short move animation, promotion picker.
- **Info panel:** move list, captured pieces, turn indicator, clocks when enabled.
- **Controls:** new game, undo, redo, resign, offer draw, flip, settings.
- **Accessibility:** colour-blind-safe highlights, high-contrast theme, keyboard-only play, scalable UI.
- **Theming:** a theme is a bundle of board colours, piece art and sounds, selectable in settings.

## Opponent AI

The AI is a swappable module that receives the game state and returns one legal move, so it works with any rule preset.

| Level | Approach | Notes |
| --- | --- | --- |
| Beginner | Random legal move, prefers captures | Good for testing |
| Intermediate | Shallow search with material scoring | Few moves ahead |
| Advanced | Deeper search with position scoring | Needs a time limit |
| External | Plug in an outside engine | Only for standard chess |

The AI must ask the rules engine for legal moves rather than assume standard chess. Search-based AI will be weaker on exotic variants until tuned for them.

## Roadmap

1. **Phase 1, Standard chess:** board, pieces, legal moves, all end conditions, local two-player, undo, move list.
2. **Phase 2, Template structure:** move rules, board and presets moved into config; rule toggles; automated rule tests.
3. **Phase 3, Opponent and polish:** beginner and intermediate AI, themes, sounds, save and load.
4. **Phase 4, Variant toolkit:** first three variants as presets (Chess960, King of the Hill, Three-check), plus a sandbox editor.

Each phase ends with a playable build and a passing rules test suite.

## Decisions log

| Topic | Decision | Status |
| --- | --- | --- |
| Default game | Standard 8x8 chess as the base preset | Assumed |
| Architecture | Rules, pieces and boards defined as data | Assumed |
| Platform and engine | Unity (2D URP project, Input System package present) | Decided |
| Visual style | Not chosen | Open |
| Variants first | Chess960, King of the Hill, Three-check | Proposed |
| Multiplayer | Out of scope for v1 | Proposed |
