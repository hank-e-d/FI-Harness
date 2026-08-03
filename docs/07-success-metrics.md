# Success Metrics and Anti-Patterns

## Success metrics

| Metric | Healthy signal |
|---|---|
| Time to first playable | < 1 day from pack fork |
| Gate pass rate on clean pack | 100% before any content |
| Content add cost | New enemy/level = hours not days |
| Agent illegal edits | ~0 (harness guardian catches them) |
| Visual round cap | Strategy change by round 3, not round 6 |
| Creator completion (FI) | Pack exports that pass build-check without blank screens |

## Anti-patterns (do not do early)

| Don't | Why |
|---|---|
| Start with "AI open-world RPG" | Harness surface is infinite |
| Generate art before loop is fun with placeholders | Polish on broken loop wastes credits and time |
| Run visual judges without empties/kit density plan | Atmosphere fails forever |
| One-shot full games | Prefer vertical slices that pass gates |
| Unify all genres into one mega-template | Shared kernel + pack plugins only |
| Treat MCP tools as the harness | Tools are hands; the pack is the body |
| Open mechanics fence mid-art flood | Rate thrash + unreadable playtests |
| Unlimited visual claim rounds | Change strategy, not prompt count |

## Immortal Shores → permanent checklist items

Carry forward into any 3D pack:

- [ ] Mechanics fence intact before visual gauntlet
- [ ] Kit densification on empties (not only lighting tweaks)
- [ ] Mid-field / iso-readable life (workers, river, air)
- [ ] Avoid fog setups that wash ortho near-field
- [ ] Construction / interactive objects remain pickable
- [ ] UI: thin chrome + floating menus over full-bleed world (when that layout is chosen)
- [ ] Production ship: debug cheats off (`NODE_ENV=production` or equivalent)
- [ ] Visual gate closed honestly when below threshold — no claim-only unlock

## Definition of done (single content unit)

A content unit (enemy, room, wave, dialogue chapter) is done when:

1. Schema validates
2. All asset refs resolve
3. Automated play / sim gate passes with recorded seed
4. Visual checklist items that apply are green or explicitly waived with reason
5. No illegal path edits in the PR/diff
