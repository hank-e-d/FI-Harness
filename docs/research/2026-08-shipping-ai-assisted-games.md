# Golden Rules for shipping AI-assisted games

- **Window:** June 7 – August 3, 2026  
- **Focus:** Platforms, release ladder, demos, positioning, creators, localization, funnels, browser hosting, copy risk  
- **Evidence style:** Launch postmortems with developer-reported wishlist, player, revenue, retention, or outreach numbers (not independently audited)  
- **Related in this repo:**  
  - [Hosting, publishing & marketing](2026-08-hosting-publishing-marketing.md) (platform culture, disclosure, readiness)  
  - [Four-topic research pass](2026-08-research-pass-four-topics.md)  
  - [Visual & audio asset pipelines](2026-08-visual-audio-asset-pipelines.md) (storefront coherence)  

Most detailed evidence came from Reddit; X was more useful for observing live campaign tactics than for rigorous postmortems.

The central finding is straightforward:

> **AI can shorten production, but it does not shorten the trust-building, positioning, testing, and audience-development work required to sell a game.**

In some cases, conspicuous AI use raises the quality threshold because players scrutinize visual consistency, originality, performance, and disclosure more closely.

---

## 1. Assign each platform one specific job

**Rule:** Do not publish everywhere with the vague goal of “getting exposure.” Decide what each platform is supposed to accomplish.

A practical split emerging from recent releases is:

- **Itch or a browser build:** validate the core loop, remove installation friction, gather feedback, and make the game easy for creators to discover.
- **CrazyGames or similar portals:** reach large free-to-play browser audiences when the game loads quickly and is understandable immediately.
- **Steam:** accumulate wishlists, build durable community infrastructure, run a polished demo, and convert interest into a premium purchase.

This distinction matters because browser popularity does not automatically transfer to Steam. *Bills Must Be Paid* reported roughly 3,000 itch plays, 138,000 CrazyGames plays, and 20,000 Armor Games plays, but believed Steam conversion from those portals—particularly CrazyGames—was low. *Liquid Swarm* similarly found success on CrazyGames without corresponding Steam wishlist traction. By contrast, browser exposure worked extremely well for *Scale the Depths* because YouTubers discovered its itch version and turned it into a creator-driven funnel. ([Reddit][1])

**Action:** Give every release surface a KPI:

- Itch: completed sessions and useful feedback.
- Browser portal: retention, load completion, and revenue per player.
- Steam demo: player-to-wishlist conversion.
- Steam store page: qualified wishlists per traffic source.

---

## 2. Use a release ladder instead of one giant launch

**Rule:** Move through increasingly public and polished releases.

A strong sequence is:

1. Restricted itch or private web build.
2. Public browser prototype.
3. Marketing-ready Steam Coming Soon page.
4. Polished Steam demo.
5. Themed festival or creator campaign.
6. Release-date announcement.
7. Full launch.

*Liquid Swarm* began with an extremely bare itch prototype, moved to CrazyGames, and added features in response to actual player behavior. Its developer reported that the fourth prototype—not the first three—became commercially meaningful. *Scale the Depths* kept its original jam build playable while developing an expanded Steam demo, allowing the free version to continue attracting creators. ([Reddit][2])

**Why it works:** Each stage answers a different question:

- Is the core mechanic interesting?
- Will strangers play without explanation?
- Will creators share it?
- Does the audience want a larger version?
- Will players pay?

Do not add production scope until the previous stage supplies a positive answer.

---

## 3. Publish the Steam page when it is marketing-ready—not merely technically ready

**Rule:** Go live early enough to capture unexpected attention, but not while the storefront still misrepresents the game.

Steam recommends publishing a Coming Soon page when you are ready to talk publicly and have representative branding, screenshots, and description. A recent developer who opened a rough page without prior marketing, strong assets, or a polished demo believed the weak first impression damaged initial momentum. Conversely, *Bills Must Be Paid* benefited from already having a trailer and Steam page when an unexpected Bilibili creator covered the game. ([Steamworks][3])

Before publishing, require:

- A readable capsule at thumbnail size.
- A one-sentence player fantasy.
- Representative gameplay screenshots.
- A trailer or short gameplay clip that shows the hook immediately.
- A clear genre and intended audience.
- A functioning wishlist CTA.
- No placeholder AI imagery that will later establish the wrong expectations.

The correct balance is **early, but credible**.

---

## 4. Treat the demo as a product launch

**Rule:** A demo needs a release freeze, full regression testing, analytics, creator outreach, and an explicit conversion path.

The *Bills Must Be Paid* team made last-minute changes approximately 20 minutes before its demo release and introduced a game-breaking bug that appeared in large creators’ videos. The team’s conclusion was to treat the demo with the same discipline as the full release. The corrected demo later reached more than 80,000 unique players and supplied reviews, Discord activity, discussion feedback, and improvements for the commercial game. ([Reddit][1])

Steam also provides demo-specific conversion tools: the demo can be associated with the base game, the Coming Soon page can collect wishlists, and developers have a limited one-time opportunity to notify wishlisters about the demo’s release. ([Steamworks][4])

A demo launch checklist should include:

- Feature freeze several days before publication.
- Clean-install testing.
- Save migration and restart testing.
- Low-end hardware testing.
- Analytics for start, completion, quit point, replay, and wishlist click.
- A visible in-game “Wishlist the full game” action.
- Stream-safe music.
- Creator keys or direct access before the public date.
- A prepared hotfix branch—but no speculative launch-day refactoring.

---

## 5. Sell the player experience, not the AI workflow

**Rule:** Lead the store page and trailer with the fantasy, mechanic, tension, or transformation the player receives.

“Made with 12 agents,” “fully vibe-coded,” or “generated in three weeks” may interest developers, but it usually does not explain why a player should care. Recent AI-game discussions repeatedly distinguished mandatory disclosure from marketing positioning: developers recommended accurate disclosure while making the game—not the defense of its production process—the headline. ([Reddit][5])

Steam’s current content survey requires developers to describe relevant generative-AI use in shipped or live-generated content. That should be handled accurately. It does not require the entire store identity to revolve around AI. ([Steamworks][6])

A good hierarchy is:

1. **Store headline:** what the player does and why it is compelling.
2. **Trailer:** representative gameplay.
3. **Description:** systems, progression, modes, and content.
4. **Disclosure or FAQ:** clear account of material AI assistance.
5. **Devlog:** deeper technical workflow for interested players and developers.

Transparency and player-centered positioning are compatible.

---

## 6. Replace “AI-looking” inconsistency before scaling traffic

**Rule:** Do not buy traffic for a store page whose capsule, character art, UI, screenshots, and game footage appear to belong to different products.

The developer of the AI-assisted *Ascend from Nine Mountains* reported that approximately 30% of its returned copies that supplied reasons mentioned AI artwork; the remainder cited matters such as difficulty, performance, and translation. That does not prove AI art alone caused returns, but it shows that visible AI use can become part of the purchasing and refund decision. ([Reddit][5])

A useful recent pattern came from *Liquid Swarm*: the developer used temporary AI-generated capsules while validating the concept, then commissioned final storefront art after the game demonstrated traction. ([Reddit][2])

Before a major announcement, audit for:

- Changing character anatomy or costume details.
- Inconsistent perspective and lighting.
- Unreadable logo text.
- Generic key art that does not depict gameplay.
- Screenshots with visibly different UI generations.
- Marketing art that promises fidelity absent from the build.
- AI-generated localization that has not been reviewed in context.

The goal is not to conceal the workflow. It is to ship a coherent product.

---

## 7. Make the hook understandable within seconds

**Rule:** The first capsule, first trailer shot, and first playable interaction should communicate the game’s differentiating action.

The team behind *Ignite the Depths* reported approximately 15,000 wishlists within 72 hours. It spent around 20 days developing the trailer and targeted a specific audience drawn to a retro, dreamlike, nostalgic presentation. Its strongest channels were TikTok, X, and YouTube, where immediate visual comprehension matters. ([Reddit][7])

*Liquid Swarm* attributed part of its browser success to a novel mechanic, an approximately ten-second tutorial, rapid loading, and footage that communicated the experience quickly. ([Reddit][2])

Use this test:

> Show a muted three-second clip to someone unfamiliar with the project. Can they identify what the player controls, what changes, and what is unusual?

If not, revise the footage before increasing outreach.

---

## 8. Contact far more creators than feels reasonable

**Rule:** Creator outreach is a high-variance pipeline. Low response rates are normal; a few successful videos can change the entire launch.

Recent examples:

- *Ignite the Depths* contacted roughly 80 creators and reported only three posts—but those posts contributed substantial reach.
- *Bills Must Be Paid* contacted almost 1,000 creators. Some published coverage without replying to the original email.
- *Horde of Distraction* contacted 138 selected influencers and received 23 videos; the developer still concluded that a larger list would have been useful.
- *Scale the Depths* grew largely because YouTubers organically discovered the browser build, after which additional creators followed. ([Reddit][7])

Prepare a compact creator package:

- One-sentence hook.
- Five-second and thirty-second clips.
- Clean logo and capsule.
- Direct build or demo link.
- Expected playtime.
- Stream-safe audio status.
- Relevant genre comparisons.
- Contact details and bug-report channel.
- No requirement that creators request permission before publishing.

Prioritize creators whose viewers already play the genre. Audience fit matters more than follower count.

---

## 9. Localize outreach where organic traction appears

**Rule:** When an overseas creator or community begins sending traffic, respond immediately with localized materials.

Both *Bills Must Be Paid* and *Scale the Depths* received meaningful attention from Chinese-language creators and platforms. In the former case, the Steam page being live allowed the team to capture an unexpected Bilibili spike rather than losing it. ([Reddit][1])

Prepare lightweight localization before you urgently need it:

- Localized store short description.
- Subtitled trailer.
- Translated creator email.
- Regional press kit.
- Correct game title and search keywords.
- Community moderator or translator contact.
- Clear information about whether the demo supports the language.

Do not fully localize a large game based on one viral clip. First localize the **conversion surface**, then measure wishlists and demo engagement from that region.

---

## 10. Build community around participation, not announcements

**Rule:** Give people something to test, submit, vote on, remix, or discuss.

Recent X campaigns commonly used a single concrete action: request Steam playtest access, enter a Discord screenshot contest, or wishlist after a milestone announcement. That is stronger than repeatedly asking people to “follow development.” ([X (formerly Twitter)][8])

Recent Reddit launch reports also show that community activity improves the product:

- Demo discussions and reviews exposed issues before release.
- Discord feedback influenced balance and feature priorities.
- Replaying creators generated repeated marketing beats.
- The AI-assisted *Ascend from Nine Mountains* shared workflows and enabled Discord members to make custom characters, turning part of the AI pipeline into community-created content. ([Reddit][1])

Useful recurring community activities include:

- Weekly playtest prompt.
- Screenshot or build challenge.
- Vote between two bounded design options.
- Public changelog tied to player reports.
- Mod, character, level, or seed showcase.
- Developer response to the most common demo issue.
- “What confused you first?” feedback thread.

Community should produce evidence and artifacts—not just member counts.

---

## 11. Plan several campaign beats; do not rely on Next Fest alone

**Rule:** Treat festivals as multipliers for an existing campaign, not as automatic discovery engines.

Recent Next Fest reports were highly mixed. Some small games multiplied their wishlist counts, while at least one title entered with approximately 15,000 wishlists and saw little change in its normal wishlist rate despite positive player feedback. *Scale the Depths* achieved a major increase during a highly relevant Fishing Fest, supported by returning creators and a fresh demo. ([Reddit][9])

A stronger sequence is:

1. Store-page announcement.
2. First gameplay reveal.
3. Public demo.
4. Genre-specific festival.
5. Creator update or major demo patch.
6. Release-date trailer.
7. Next Fest or other major showcase.
8. Launch.

Save something genuinely new for each beat. Reposting the same trailer seven times is not seven campaigns.

---

## 12. Measure the funnel, not vanity totals

**Rule:** Downloads, impressions, and views are intermediate signals. Track whether they produce qualified players and purchases.

One recent Steam demo reported 1,416 downloads but only 391 Steam players, a median playtime of roughly 24 minutes, and modest wishlist growth. Its developer concluded that the game still had too much entry friction and insufficient “must share” appeal. ([Reddit][10])

Track:

```text
Impression
→ store-page visit
→ wishlist
→ demo download
→ first launch
→ tutorial completion
→ meaningful session
→ replay
→ purchase
→ retained player
```

Break this down by creator, post, advertisement, country, and platform using campaign tags or UTMs where available.

Paid traffic results were especially inconsistent. *Bills Must Be Paid* reported spending about $200 on Reddit ads for only 19 tracked wishlists, while the AI-assisted *Ascend from Nine Mountains* developer said small Reddit campaigns produced ongoing sales and wishlists for its highly specific niche. ([Reddit][1])

Therefore:

> Test every paid channel with a small capped budget. Scale only after it demonstrates downstream wishlist or purchase conversion.

---

## 13. For browser games, optimize hosting for loading and compatibility

**Rule:** Use the simplest hosting architecture that can deliver the current game reliably.

For a single-player HTML5 prototype, itch supports a ZIP containing `index.html` and its assets, hosts the files itself, and embeds the game on its page. It also recommends uploading files directly rather than depending on external download hosts. ([itch.io][11])

A sensible technical progression is:

- **Static single-player prototype:** itch HTML5 or another static host; no application server.
- **Branded campaign site:** static landing page connected to the itch build or Steam page.
- **Leaderboards/accounts/cloud state:** thin authenticated API with a managed database.
- **Competitive economy or multiplayer:** authoritative server; never expose trusted game logic or secrets in browser code.

For portals, load speed and hardware compatibility are product features. *Liquid Swarm* reported that a WebGL2/mobile upgrade unexpectedly reduced revenue by roughly one-third after more lower-end users bounced, which apparently reduced platform distribution. The same developer improved monetization and engagement through behavioral analytics and A/B tests rather than additional content alone. ([Reddit][2])

Test:

- Cold load time.
- Download size.
- Memory use.
- Integrated-GPU performance.
- Mobile browser compatibility.
- Failure rate before gameplay.
- Time from page open to first meaningful action.

---

## 14. Assume public browser builds can be copied

**Rule:** Publish web prototypes for reach, but manage their ownership and security as public client software.

The *Scale the Depths* developer reported that copies of the jam build’s code and assets appeared in mobile and Chinese-platform games, sometimes modified through AI filters. ([Reddit][12])

Practical safeguards include:

- Keep repository history and original source files.
- Publish under a consistent studio identity.
- Maintain dated store and project pages.
- Add internal build identifiers.
- Keep service credentials and valuable server logic out of the client.
- Make multiplayer economies server-authoritative.
- Preserve source artwork and generation/editing records.
- Prepare a basic evidence package for platform takedown requests.

Obfuscation may increase copying effort, but it cannot make shipped browser code private.

---

## 15. Keep positioning and audience knowledge in-house

**Rule:** A publisher, agency, or freelancer can execute marketing, but the developer must understand the hook, audience, and conversion funnel.

The developer behind the June 2026 full release *Pay 2 Win* reported approximately $35,500 gross revenue over its first 45 days, around 3,092 units, and just under 10,000 wishlists. Its biggest marketing regret was relying on an external marketing team rather than developing that capability internally. ([Reddit][13])

This does not mean publishers are ineffective. *Scale the Depths* credited its publisher with vertical videos and influencer outreach during a campaign that eventually reached a reported 175,000 wishlists. The distinction is whether the developer has already validated the product’s positioning and can evaluate the work being purchased. ([Reddit][12])

Outsource:

- Editing and formatting content.
- Contact-list expansion.
- Translation.
- Paid campaign operations.
- Event coordination.
- Additional creator follow-up.

Retain ownership of:

- The core hook.
- Audience definition.
- Store-page promise.
- Community voice.
- Player-feedback interpretation.
- Decisions about what the next build should prove.

---

# Recommended platform stack

For most small AI-assisted teams:

**Early validation:** A restricted or public itch HTML5 build, basic behavioral analytics, and a tiny Discord or feedback form.

**Commercial validation:** A polished Steam Coming Soon page plus a browser prototype or downloadable demo that creators can access without friction.

**Audience expansion:** Genre-specific creators, translated conversion materials, and selected festivals. Add CrazyGames or another portal only when the experience is fast-loading, free-friendly, and understandable almost immediately.

**Launch preparation:** Steam demo treated as a release, complete analytics funnel, creator build distributed early, full storefront QA, accurate AI disclosure, and multiple campaign beats.

**Post-launch:** Rapid bug fixes, visible changelogs, community participation, refund-reason review, and updates designed around observed player behavior rather than speculative feature generation.

The strongest recent releases did not succeed because they advertised that AI made development faster. They succeeded because they **made the game instantly legible, put a playable version in front of the right people, captured attention with a ready store page, and iterated using real conversion and play data**.

---

## Citations

[1]: https://www.reddit.com/r/gamedev/comments/1va2gtb/releasing_with_61000_wishlists_2_person_what_we/ "Releasing with 61,000+ Wishlists - 2 person - what we did/learnings : r/gamedev"

[2]: https://www.reddit.com/r/gamedev/comments/1uol4p2/six_weeks_on_crazygames_my_incremental_roguelite/ "Six weeks on CrazyGames: my incremental roguelite makes ~€31/day… : r/gamedev"

[3]: https://partner.steamgames.com/doc/store/coming_soon "Coming Soon (Steamworks Documentation)"

[4]: https://partner.steamgames.com/doc/store/application/demos "Demos (Steamworks Documentation)"

[5]: https://www.reddit.com/r/aigamedev/comments/1u8w8kv/has_anyone_had_success_from_their_game_after/ "Has anyone had success from their game after disclosing Ai? : r/aigamedev"

[6]: https://partner.steamgames.com/doc/gettingstarted/contentsurvey "Content Survey (Steamworks Documentation)"

[7]: https://www.reddit.com/r/gamedev/comments/1v7dsys/15k_wishlists_in_72_hours_postmortem_for_our/ "15k wishlists in 72 hours. Postmortem for our announcement! : r/gamedev"

[8]: https://x.com/GardenSouls01/status/2075677042662904291 "Garden Souls on X"

[9]: https://www.reddit.com/r/IndieDev/comments/1ucr6b9/so_how_was_your_next_fest_did_you_reach_your/ "So, how was your Next Fest? Did you reach your targets? : r/IndieDev"

[10]: https://www.reddit.com/r/IndieDev/comments/1uz3liy/steam_demo_results_just_140_wishlists_40_140_is/ "Steam Demo Results: Just 140 Wishlists… : r/IndieDev"

[11]: https://itch.io/docs/creators/html5 "Uploading HTML5 games"

[12]: https://www.reddit.com/r/gamedev/comments/1tq3sv6/our_game_jam_entry_blew_up_and_we_turned_it_into/ "Our game jam entry blew up… 175,000 wishlists… stolen… : r/gamedev"

[13]: https://www.reddit.com/r/IndieDev/comments/1v3qnr0/45_days_ago_we_launched_our_first_game_a/ "45 days ago we launched… Pay 2 Win… post-mortem : r/IndieDev"
