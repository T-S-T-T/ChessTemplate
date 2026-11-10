# Open Questions and Suggestions

Record answers in the GDD decisions log, then mark them answered here.

## Questions

| # | Question | Why it matters | Answer |
| --- | --- | --- | --- |
| 1 | Platform: Unity chosen. Which Unity version and target (PC, mobile, WebGL)? | Affects input, UI scaling, build setup | Unity, version/target open |
| 2 | Are alterations mostly rule changes (variants), visual changes, or both? | Decides where the most flexibility is built | Open |
| 3 | Which variants do you already have in mind? | Shapes the rules data format | Open |
| 4 | Players: local two-player only, vs computer, online later, or all three? | Online changes the architecture early | Open |
| 5 | Art style: 2D classic, pixel, minimalist, themed? | Decides asset pipeline and theme format | Open |
| 6 | Audience: just you, friends, or public release? | Sets polish and platform requirements | Open |
| 7 | Time controls (blitz, increment) in v1? | Clocks touch game flow and UI | Open |
| 8 | Import and export games (PGN/FEN)? | Affects the persistence module | Open |

## Suggestions

1. **Keep rules as data.** Describe pieces, boards and variants in ScriptableObjects or config files so changes are cheap.
2. **Build a rules test suite first.** Known positions and edge cases (castling through check, en passant pins) stop alterations silently breaking chess. Unity Test Framework fits (EditMode tests).
3. **Add a sandbox board early.** Setting up odd positions is the fastest way to test a new rule or piece.
4. **Support undo and replay from day one.** Store games as a start position plus a move list.
5. **Pick the first three variants** (Chess960, King of the Hill, Three-check) to prove the template is flexible.
6. **Keep game logic free of Unity types.** Plain C# for rules and state makes testing fast and keeps the engine swappable; MonoBehaviours only for presentation and input.
