# Golden Rules: Hosting, publishing & marketing AI-assisted games

- **Window:** June – early August 2026  
- **Focus:** Platforms, technical hosting, store strategy, community, what recent releasers actually did  
- **Sources:** r/aigamedev, r/IndieDev, X builders  
- **Related:** [Four-topic research pass](2026-08-research-pass-four-topics.md)

---

## Thesis

> **itch is for players and learning; Steam is for customers.** AI does not lower the polish bar for paid storefronts — it only floods the funnel with unfinished builds. Ship where the **audience expectation** matches your **finish level**.

---

## Golden Rule 1: Match platform to readiness

**Rule:**

| Stage | Default platform |
|-------|------------------|
| Friends / closed alpha | Private link, Discord, personal host |
| Public playtest, hobby, jam | **itch.io** (free, minutes to upload) |
| Paid / “real product” | **Steam** (after polish + store craft) |
| Instant play demos | Web build (GitHub Pages, itch web, custom CDN) |
| Mobile-first loops | App stores (also common in aigamedev) |

**Why it works:** AI Game Festival form (~30 entries, late Jun 2026): vast majority on itch, only ~2 on Steam. Consensus reasons: **$0 vs $100**, multi-week Steam checklist vs “name + zip + done,” fear of AI-hate review bombs, and many projects simply **not customer-ready**. Aphorism from thread: *“Release on Steam when ready for customers; on itch when ready for players.”*

**Evidence:** Detailed Steam vs itch checklist comparison (page review loops, build review, demo checkbox footguns, achievements, antivirus/depot setup).

**Confidence:** High

---

## Golden Rule 2: Don’t confuse “can upload” with “should publish”

**Rule:** Do not put half-hour concept demos on Steam (or even public itch if you care about reputation) just because AI made them possible.

**Why it works:** Experienced voices warn platforms are flooding with AI slop; Steam’s historical asset-flip crackdown is the analogy. Uploading unfinished AI clones trains audiences to hate the category and hardens future policy. Prefer friends/family play before storefronts.

**Confidence:** High (cultural + platform risk)

---

## Golden Rule 3: Disclose AI per platform rules; sell the game, not the tool chain

**Rule:** Use required AI disclosures (itch and Steam both have them). Marketing copy leads with **fantasy, loop, trailer** — not “100% vibe coded.”

**Why it works:** r/aigamedev marketing thread: most players don’t care if it’s not slop; they care if it’s fun. Steam comments *can* tank ratings for AI tags — some shippers still go Steam with disclosure; some believe peers under-disclose code assist. Policy drift reported (Steam eased pure-code disclosure per some comments) — **verify current Steam/itch rules before ship**, don’t rely on forum rumor alone.

**Confidence:** High on disclose-and-focus-on-fun; Medium on exact legal checklist (check primary sources at ship time)

---

## Golden Rule 4: Budget calendar time for Steam, not just tokens

**Rule:** Treat Steam page + build review as a **multi-week project**: capsules, copy length rules, hardware tags, depots, demo flags, payment identity.

**Why it works:** Community repeatedly underestimates Steam friction vs itch. $100 fee is not the main cost — iteration on failed reviews is.

**Practical sequence:**

1. Steamworks account + payment  
2. Store page draft → review  
3. Capsules / trailer (see Rule 6)  
4. Build + depots → review  
5. Optional demo app (separate checklist)  
6. Wishlist campaign **before** release date  
7. Launch  

**Confidence:** High

---

## Golden Rule 5: Host web builds for demos; native for “product”

**Rule:** Browser builds (WebGL/WebGPU) are excellent for r/aigamedev proof and itch “play in browser.” Ship desktop/mobile packages when you need performance, filesystem, or Steam features.

**Why it works:** Many AI prototypes are Three.js/Godot web — zero install converts curiosity. Performance-sensitive or GPU-heavy work needs real packages (and real GPU testing — see harness notes). Capybara-class claim: multi-target (web/mobile/desktop) from consistent asset approach.

**Technical tips heard:**
- itch: drag zip or butler CLI for updates  
- Web: cache-bust assets; watch mobile Safari GPU limits  
- Steam: sign builds to reduce antivirus false positives  

**Confidence:** High

---

## Golden Rule 6: Short vertical video is the marketing unit

**Rule:** Prioritize 15–45s gameplay clips (Reels/TikTok/X/YouTube Shorts) over long blogs. Show the loop in the first 3 seconds.

**Why it works:** Marketing thread: TikTok/IG Reels recommended for discovery; TikTok noted as often anti-AI in comments — still works for **gameplay** if the clip is strong. Pixel/narrative indies on X still do **story trailer + Steam page launch day** as a beat.

**Avoid:** Talking-head “I used Opus 5” without gameplay.

**Confidence:** High for indie discovery generally

---

## Golden Rule 7: Build wishlists and community before “launch day surprise”

**Rule:** Steam page up early; post progress in public; collect wishlists; soft-launch demo on itch; then paid launch.

**Why it works:** Classic indie — unchanged by AI. AI only makes empty pages faster. First-time releasers on X still ask “what worked for marketing?” — answers cluster on **consistent progress posts**, not one viral AI flex.

**Channels:**
- X/Reddit progress (r/aigamedev for process, genre subs carefully)  
- Discord for playtesters  
- Streamers: hard for pure “AI games”; pitch **fun genre pitch**, not AI angle  

**Confidence:** High

---

## Golden Rule 8: Store page craft is a design system

**Rule:** Capsules, screenshots, and description follow platform visual rules. Screenshots = **readable gameplay**, not AI hero renders with no UI.

**Why it works:** Steam review fails often on capsule/text rules (community checklist). AI can draft copy; human must verify length, tags, age rating, and “AI content” questionnaire accuracy.

**Screenshot set (minimum):**
1. Core loop moment  
2. Readable UI  
3. Failure/success state  
4. Variety (not 5 crops of same frame)  
5. Optional: mood/key art  

**Confidence:** High

---

## Golden Rule 9: Price and positioning for AI-assisted games

**Rule:** Price like the **playtime and polish**, not the token bill. Free/PWYW on itch for feedback; paid Steam when session length and retention justify it.

**Why it works:** Flood of free AI demos devalues paid unpolished clones. One X take: half-price launch + strong single-player emotional hook before service features — genre-specific, but “earn goodwill first” is durable.

**Confidence:** Medium (pricing always situational)

---

## Golden Rule 10: Protect reputation; measure what matters

**Rule:** Track wishlists, playtime, bounce on demo, and qualitative playtest notes — not just upvote on r/aigamedev.

**Why it works:** Subreddit praise ≠ Steam conversion. Alpha itch posts often conclude **discovery on itch is weak without external traffic** — you must bring audience.

**Metrics:**
- Wishlist velocity  
- Demo completion / 5-min retention  
- Crash-free sessions  
- Review text themes (fun vs AI-hate vs bugs)  

**Confidence:** High

---

## Platform cheat sheet (2026 mid-year community view)

| Platform | Cost to list | Friction | AI culture | Best for |
|----------|--------------|----------|------------|----------|
| itch.io | $0 (+ optional share) | Very low | Disclose; hobby-friendly | Demos, jams, feedback |
| Steam | ~$100/app | High (weeks) | Disclosure + mixed hate risk | Real products, wishlists |
| Own site / web | Hosting cost | Medium | You own narrative | Prototypes, web-native |
| Mobile stores | Dev accounts | High | Policy varies | Hypercasual / simple loops |
| Glitch.fun (mentioned) | — | Medium | Marketed as mid-ground | Niche alternative |

---

## Failure modes

| Failure | Why it hurts | Fix |
|---------|--------------|-----|
| Steam with unpolished AI clone | Review bombs, refunds | itch first; polish bar |
| No disclosure when required | Policy risk, trust | Follow platform forms |
| Marketing “made with AI” as pitch | Attracts hate, not players | Pitch fantasy + trailer |
| Upload and pray on itch | Zero discovery | External clips + community |
| Skip store craft | Failed Steam reviews | Capsule checklist early |
| Infinite demo, never ship | Reputation as vapor | Calendar + scope freeze |

---

## Minimal ship checklist (actionable)

```text
[ ] Vertical slice fun for 10+ minutes without excuses
[ ] AI disclosure text drafted per platform
[ ] Trailer/clips: loop visible in 3s
[ ] itch page: web or zip, clear controls, version notes
[ ] Playtest with 5 strangers; fix top 3 friction points
[ ] If Steam: page + capsules in review while still polishing
[ ] Wishlist goal number before setting launch date
[ ] Crash/perf smoke on target hardware
[ ] Post-launch: patch plan, not radio silence
```
