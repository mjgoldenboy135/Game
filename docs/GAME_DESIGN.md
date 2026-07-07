# Momo's Whistling Garden — Game Design & Technical Specification

| | |
|---|---|
| **Document status** | Draft v2.0 — Approved for prototyping |
| **Product** | Offline Android game, children aged 5–7 |
| **Platform** | Android 5.0+ (API 21), Flutter |
| **Last updated** | 2026-07-07 |
| **Owners** | Child psychology · Montessori pedagogy · Game design · UI/UX · Flutter engineering |

**Revision history**

| Version | Date | Changes |
|---|---|---|
| 1.0 | 2026-07-07 | Initial concept document |
| 2.0 | 2026-07-07 | Professional revision: numbered requirements, formal adaptive-engine spec, session state machine, failure-mode handling, performance budgets, test strategy, delivery plan, risk register, compliance matrix |

---

## Table of Contents

- [Part A — Product & Design](#part-a--product--design)
  - [1. Vision, Pillars, and Success Criteria](#1-vision-pillars-and-success-criteria)
  - [2. Core Gameplay Loop](#2-core-gameplay-loop)
  - [3. Learning Objectives](#3-learning-objectives)
  - [4. Psychological Rationale per Mechanic](#4-psychological-rationale-per-mechanic)
  - [5. Progression & Adaptive Difficulty (formal spec)](#5-progression--adaptive-difficulty)
  - [6. Reward System](#6-reward-system)
  - [7. UI/UX Specification](#7-uiux-specification)
  - [8. Character & World Design](#8-character--world-design)
  - [9. Asset List & Budgets](#9-asset-list--budgets)
- [Part B — Engineering Specification](#part-b--engineering-specification)
  - [10. Requirements (FR/NFR)](#10-requirements)
  - [11. Flutter Architecture](#11-flutter-architecture)
  - [12. Session & Round State Machines](#12-session--round-state-machines)
  - [13. Data Layer & SQLite Schema](#13-data-layer--sqlite-schema)
  - [14. Reliability, Failure Modes & Edge Cases](#14-reliability-failure-modes--edge-cases)
  - [15. Performance Engineering for Low-End Devices](#15-performance-engineering-for-low-end-devices)
  - [16. Folder Structure](#16-folder-structure)
- [Part C — Quality, Compliance & Delivery](#part-c--quality-compliance--delivery)
  - [17. Testing Strategy](#17-testing-strategy)
  - [18. Privacy, Safety & Compliance Matrix](#18-privacy-safety--compliance-matrix)
  - [19. Accessibility](#19-accessibility)
  - [20. Delivery Plan & Milestones](#20-delivery-plan--milestones)
  - [21. Risk Register](#21-risk-register)
  - [22. Future Expansion](#22-future-expansion)

---

# Part A — Product & Design

## 1. Vision, Pillars, and Success Criteria

### 1.1 Concept

Momo is a small, cheerful axolotl who lives in a floating garden of six islands. The garden's plant creatures — the **Sprouts** — have dozed off, and Momo asks the child to help wake them through short, playful activities: matching singing flowers, sorting pattern stones, routing water through bamboo pipes, and cheering up moody weather-clouds. Every completed activity wakes a Sprout, who gives the child a **Sunny Seed** and moves into the child's own **My Garden** — a free-play sandbox that grows into a living, personal record of everything they've done.

The child experiences a warm world of silly creatures. The design underneath trains working memory, attention, logic, creativity, emotional recognition, problem-solving, and executive function.

### 1.2 Design pillars

Every design and engineering decision is tested against these four pillars, in priority order:

1. **Fun first, learning second.** If a mechanic is pedagogically ideal but not delightful, it is redesigned or cut.
2. **Nothing can be lost, no one can fail.** No fail states, no decay, no comparison, no punishment. Difficulty bends; the child does not break.
3. **The child is capable.** The child is cast as the helper the world needs. All feedback targets effort and process.
4. **Calm by default.** Short sessions with natural exits, gentle audio, no urgency mechanics, no dark patterns.

### 1.3 Success criteria (non-exploitative by design)

We deliberately exclude retention/engagement-maximization metrics. Success is measured by:

| Metric | Target | Rationale |
|---|---|---|
| Median session length | 3–6 min | Matches attention span; validates natural exit points |
| Round completion rate | ≥ 95% | Anti-frustration easing is working |
| Success rate within adaptive band | 70–85% of rounds | Flow channel is correctly tuned |
| Voluntary stop at sunset cue | ≥ 60% of limited sessions end without hard stop | Screen-time fiction is respected, not fought |
| Crash-free sessions | ≥ 99.7% | Reliability floor for a child audience |
| Parent-reported distress at session end | ~0 in playtesting | The goodnight sequence must end sessions warmly |

## 2. Core Gameplay Loop

### 2.1 The 2–5 minute session loop

```
1. OPEN    Momo waves hello; garden hub appears (< 3.5 s cold start)
2. CHOOSE  Child taps one of six activity islands (free choice, none locked)
3. PLAY    One activity round (90–180 s, adaptively tuned)
4. WAKE    A Sprout wakes and celebrates the child (≤ 5 s, tap-skippable)
5. EARN    Sunny Seed drops into the child's pouch (guaranteed)
6. PLANT   Optional: plant/decorate in My Garden
7. CLOSE   Natural stopping point; sunset lighting and Momo's yawn
           signal an approaching screen-time limit
```

Each iteration is a complete, satisfying arc. A child can stop after one island (~2 min) or chain two or three (~5 min). The loop creates **natural exit points** rather than continuation hooks — the deliberate inverse of dark-pattern session design.

### 2.2 The six activity islands

| Island | Activity | Primary skill | Controls |
|---|---|---|---|
| Melody Meadow | Singing flowers play a tune-and-light sequence; child replays it by tapping | Working/auditory memory | Tap |
| Pebble Beach | Shape/color pattern sequences; child drags the stone that comes next | Logic, pattern recognition | Drag |
| Bamboo Falls | Rotate bamboo pipe segments to route water to a thirsty Sprout | Problem-solving, planning | Tap (90° per tap) |
| Cloud Hill | Weather-clouds display feelings; child matches the emotion, then chooses what would help | Emotional recognition, empathy | Tap |
| Bug Burrow | Find-and-tap hidden critters in a busy, gently animated scene | Selective attention, visual search | Tap |
| My Garden | Free-play sandbox: plant seeds, mix flower colors, arrange decorations, dress Sprouts | Creativity, autonomy | Tap + drag |

**Invariant:** My Garden is always available and never gated. Creativity must never be a reward that can fail to be earned.

## 3. Learning Objectives

Mapped to developmental targets for ages 5–7 (Piaget's late preoperational → early concrete operational transition):

| Domain | Objective (observable) | Trained in |
|---|---|---|
| Memory | Reproduce sequences of 2→6 items; recall object locations | Melody Meadow; Bug Burrow bonus rounds |
| Attention | Sustain focus 60–120 s; filter calibrated distractors; shift attention on cue | Bug Burrow; ambient-distraction tuning in all islands |
| Logic | Extend AB/ABB/ABC patterns; classify by one, then two attributes; seriation | Pebble Beach and sorting variants |
| Creativity | Open-ended construction with no correct outcome; divergent color/shape mixing | My Garden |
| Emotional recognition | Identify 5 basic emotions from face, posture, and sound; map feeling → helpful response | Cloud Hill |
| Problem-solving | Plan 2–4 steps ahead; test-and-revise without frustration | Bamboo Falls |
| Executive function | Inhibition ("only tap the blue bugs"), set-shifting (mid-round rule switches), working memory | Cross-cutting rule-switch variants, gated by the adaptive engine |
| Fine motor | Precise tap, drag-and-drop, discrete rotation | All controls |

**Montessori alignment:** every activity is *self-correcting* — the material itself reveals the outcome (water flows or it doesn't; the melody sounds right or a flower gently shakes its head). No authority figure judges; no red X exists anywhere in the product.

## 4. Psychological Rationale per Mechanic

### 4.1 Piaget — concrete operations through concrete objects
Children in this window learn by manipulating tangible things, not abstract symbols. Every puzzle is physical and visible: real water flowing, real stones placed, real flowers singing. Bamboo Falls externalizes hypothesis testing — rotate a pipe, immediately see the water's course change — a concrete cause-and-effect laboratory.

### 4.2 Vygotsky — Zone of Proximal Development, with Momo as fading scaffold
Momo is the "more knowledgeable other." On hesitation (~8 s idle) or a second consecutive error, Momo offers a **graduated hint**: (1) glance toward the answer, (2) point, (3) demonstrate. Scaffolding fades automatically with competence — at higher ratings, hints arrive later and less explicitly. The child always works just beyond independent ability, never far beyond. (Formal trigger spec in §5.4.)

### 4.3 Montessori — freedom within structure
- The hub is an open shelf: all islands visible and choosable (autonomy).
- Materials are self-correcting; the environment teaches.
- One activity isolates one difficulty; task areas are visually minimal.
- No extrinsic grades — progress is embodied in the garden, like a Montessori child's tray of completed works.

### 4.4 Erikson — Industry vs. Inferiority
Ages 5–7 straddle Erikson's stages 3–4: children need to feel capable and to have initiative welcomed. Every session ends with visible evidence of competence. Mistakes never produce shame cues — no buzzer, no red flash, no disappointed character. Errors yield a warm "almost!" response: the stone wiggles and hops back; Momo giggles encouragingly.

### 4.5 Positive reinforcement — reward effort, never punish
- Attempting a round earns Sparkles; completing it earns the Seed. There is no fail state — a hard round self-simplifies (§5.5) until the child succeeds.
- **Variable delight, fixed rewards:** the Seed is guaranteed (predictable safety); only the celebratory animation varies. Variety, not rarity — no gambling shapes, no near-miss manipulation.
- **Process praise only** (per Dweck's growth-mindset findings): "You kept trying!", "You listened so carefully!" — never "You're so smart!" Ability-praise measurably increases challenge avoidance in this age group.

### 4.6 Intrinsic motivation — Self-Determination Theory
- **Autonomy:** free island choice; My Garden is goal-free.
- **Competence:** the adaptive engine holds success at 70–85%, the band where children feel challenged but capable.
- **Relatedness:** Momo and woken Sprouts remember the child; the child is the *helper*, a powerfully intrinsic role at this age.
- **No extrinsic undermining:** no login bribes, no streaks, no countdown scarcity. Rewards live inside the fiction (seeds → garden), which preserves intrinsic interest better than points and badges.

### 4.7 Flow — Csíkszentmihályi, tuned for young children
Flow requires clear goals (one Sprout to wake), immediate feedback (≤ 100 ms response to every touch), and challenge ≈ skill (adaptive engine). Sessions target the front half of the 10–15 minute attention curve and end on a high note — by the peak-end rule, the memory of play is always positive.

### 4.8 Executive function — deliberate, disguised training
Inhibition and set-shifting are among the strongest predictors of school readiness. They are disguised as play: "Only tap bugs wearing hats!" (inhibition), then mid-round, "Now only sleeping bugs!" (shifting). Rule-switch rounds unlock only after stable performance (§5.3), so they land as a fun twist, never a trick.

## 5. Progression & Adaptive Difficulty

### 5.1 Model

There are no numbered levels and no locked content. Each (profile, island) pair carries an invisible **skill rating** `r ∈ [0, 100]`. The world matures through five cosmetic **Garden Seasons** (Sprout → Bud → Bloom → Grove → Festival) driven by total Sprouts woken — a sense of journey without gating.

### 5.2 Rating update rule (deterministic, unit-tested)

```
constants:
  GAIN_CLEAN  = +4      # success, no hints
  GAIN_HINTED = +2      # success with hints
  LOSS_EASED  = -3      # round required in-round easing
  NEUTRAL     =  0      # round abandoned ('left') — never penalized
  R_MIN, R_MAX = 0, 100
  R_START      = 10     # cold-start prior; +5 if birth_year implies age 7

on round end:
  r ← clamp(r + Δ(outcome), R_MIN, R_MAX)

daily regression toward recent form (applied once per profile per day):
  r ← r + 0.1 × (mean(r over last 14 days of rounds) − r)
```

The regression term means a single bad day cannot feel like lost progress, and a long absence gently re-eases entry. Abandoning a round is explicitly neutral: leaving must never cost anything.

### 5.3 Difficulty parameter mapping

Each island defines a pure function `DifficultyParams f(r)`. Example, Melody Meadow:

| Rating band | Sequence | Playback | Distractors | EF variants |
|---|---|---|---|---|
| 0–20 | 2 notes | Slow, flowers stay lit | None | — |
| 21–40 | 3 notes | Normal | None | — |
| 41–60 | 4 notes | Normal | 1 non-singing flower | — |
| 61–80 | 5 notes | Audio-only replay | 2 | Unlocked |
| 81–100 | 6 notes | Audio-only | 2 | Includes "play it backwards" |

Rules that hold across all islands:
- The **first round of every session** is pitched one band below current rating (guaranteed warm-up win).
- **EF/rule-switch variants never appear** in a session's first round, and only at rating ≥ 61.
- Band boundaries are data, not code (`assets/catalog/difficulty.json`), so tuning never requires an engine change.

### 5.4 Hint scheduler (Vygotskian scaffolding, formalized)

```
triggers (whichever first):  idle ≥ 8 s  OR  2 consecutive errors
escalation:  L1 glance → (repeat trigger) → L2 point → L3 demonstrate
fading:      idle threshold scales with rating: 8 s + (r / 100) × 6 s
reset:       any correct action resets escalation to L0
```

Hint usage is recorded per round (`hints_used`) and feeds the rating update (§5.2).

### 5.5 In-round easing (the anti-frustration system)

After **3 unsuccessful attempts within one round**, the round quietly simplifies one notch — a pipe pre-rotates, one melody note lights longer, one distractor bug wanders off-screen — repeating as needed until the child succeeds. No message is shown; the outcome is recorded as `eased`. Success is always ultimately reached. This is the invariant behind pillar 2.

## 6. Reward System

### 6.1 Currencies and grants

| Reward | Earned by | Used for |
|---|---|---|
| Sunny Seed | Completing any round (guaranteed) | Planting flowers/plants in My Garden |
| Sparkle | Attempting a round (success or not) | Cosmetic shimmer, watering-can trails |
| Sprout Friend | Each woken Sprout (permanent) | Living garden population; idle animations, greets the child |
| Wardrobe Bits | Garden Season milestones | Cosmetic hats/scarves for Momo and Sprouts |

### 6.2 Hard rules (enforced in `RewardRules`, unit-tested)

1. Nothing can ever be lost — no decay, no wilting, no streaks to break.
2. No comparison — no leaderboards, no best scores, no star ratings. The garden is the only progress display.
3. No gambling shapes — reward pools are variety pools, not rarity tiers; no wheels, no odds.
4. Celebration ≤ 5 s and tap-skippable (respects momentum; avoids overjustification).
5. Reward persistence commits **before** the celebration renders (§14.2) — a crash can never eat a reward.
6. Praise lines rotate through process-focused phrases only; the line pool is reviewed by the child-psychology owner.

## 7. UI/UX Specification

### 7.1 Interaction rules

| Rule | Specification |
|---|---|
| Touch targets | ≥ 76 dp (5-year-olds tap with the finger pad; 48 dp adult minimum is insufficient) |
| Gesture vocabulary | Tap and drag only. Rotation = tap-to-rotate 90°. No pinch, long-press, or swipe-dependent navigation |
| Drag forgiveness | Drop zones 1.5× visual target; released items glide to the nearest valid slot or float home — nothing ever drops or vanishes |
| Feedback latency | ≤ 100 ms visual + audio response to every touch, including background touches (grass rustles, butterfly flutters) |
| Reading | Zero reading required for the child. Instruction = Momo's demonstration + voice tone + iconography |
| Navigation | One persistent "home leaf" button (bottom-left); hub is always one tap away; leaving mid-round banks the attempt Sparkle |
| Accidental-exit guard | Home leaf requires a 600 ms press-and-hold with a filling ring (visible affordance) to prevent accidental hub exits during drags |

### 7.2 Visual design

- **Palette:** warm and bright with low-saturation anchors — soft mint, sky blue, sunflower, coral. Interactive elements carry a subtle white outline plus gentle idle bobbing; backgrounds never do.
- **Shape language:** everything rounded; no sharp corners in world or UI.
- **Motion:** springy, physical, 200–400 ms ease-out. Nothing flashes above 3 Hz (photosensitivity safety).
- **Typography** (parent surfaces only): rounded humanist sans (Nunito/Quicksand), minimum 16 sp.

### 7.3 Audio design

- **Music:** pentatonic loops (child-triggered flower notes can never sound "wrong"), 60–70 BPM, bright but calm. Volume auto-ducks 50% under voice barks.
- **SFX:** soft mallets, water drops, hand-drum taps. The "incorrect" cue is a curious marimba wobble — never a buzzer.
- **Voice:** Momo speaks warm emotive gibberish — no localization burden, no language dependency; meaning is carried by animation and demonstration.
- **Audio focus:** the game respects Android audio focus (pauses music for calls/other apps, resumes gracefully) and is 100% playable muted — every audio cue has a visual twin.

### 7.4 Parent controls & screen time

- **Parent gate:** simultaneous press-and-hold of three corner leaves for 2 s — a multi-touch span+hold pattern young children cannot reliably perform. (Deliberately not an arithmetic gate, which a 7-year-old defeats.) Gate re-locks on app background.
- **Dashboard:** daily limit (5/10/15/20 min/off), per-day play summary, plain-language skill notes ("Maya now remembers 4-note melodies"), audio toggles, up to 4 profiles, data export (JSON) and one-tap full deletion.
- **Screen-time in fiction:** at 80% of the limit the garden shifts to golden sunset light and Momo yawns; at 100% Momo curls up and waves goodnight. The current round always completes. The goodnight scene is identical whether the child stops voluntarily or by timer — stopping is never framed as loss.

## 8. Character & World Design

### 8.1 Momo the Axolotl
A palm-sized coral-pink axolotl: big friendly eyes, feathery gills that wiggle when excited, a tiny leaf backpack for seeds. Axolotls regenerate — a quiet metaphor for "mistakes heal; you can always try again" — and offer a distinctive, uncrowded silhouette. Momo never instructs from authority; Momo *asks for help*, casting the child as the capable one.

Animation states: idle-breathe, walk, hint ×3 (glance/point/demonstrate), cheer, somersault, yawn, sleep, wave.

### 8.2 The Sprouts
Round-bodied plant creatures, one family per island: bell-flowers (Melody), pebble-shelled tortoise-buds (Pebble), droopy fern-pups (Bamboo), cloud-lambs (Cloud), lantern-beetles (Bug). Each Sprout's personality shows in its wake-up animation; woken Sprouts live in My Garden and greet the child by animation on return.

### 8.3 The world
Floating garden islands joined by petal bridges in a pastel sky. "Whistling" because wind through the flowers plays soft pentatonic tones — the world itself is an instrument. Each island has a distinct color key and landmark for pre-reader navigation. A subtle day/sunset tint follows the device clock, reinforcing the screen-time fiction.

## 9. Asset List & Budgets

### 9.1 Characters & animation (Rive)
- Momo: 9 animation states (one artboard, one state machine)
- Sprouts: 5 families × 3 variants = 15; each with sleep, stir, wake ×2, garden-idle, wave
- Ambient critters: butterfly, ladybug, snail, bee (2-frame loops)

### 9.2 Environments & props
- Hub: sky (day/sunset/night tints), 6 island bases, petal bridges, 3 cloud layers, foliage overlays
- Melody Meadow: 6 flowers × 3 states — Pebble Beach: 12 stones (4 shapes × 3 colors) + slots — Bamboo Falls: 3 pipe types + water flow frames — Cloud Hill: 5 emotion faces × 2 sizes + 5 helper cards — Bug Burrow: layered scene + 10 critters + accessory variants
- My Garden: 20 plants × 3 growth stages, 15 decorations, 12 wardrobe items

### 9.3 UI & audio
- UI: home leaf, seed pouch, parent-gate leaves, celebration particles, hint glow. Parent dashboard is drawn in Flutter (no image assets).
- Music: hub + 6 island loops + goodnight lullaby (8 OGG tracks, 60–90 s)
- SFX: ~40 one-shots. Voice: ~25 gibberish barks. Notes: pentatonic C-D-E-G-A × 2 octaves (10 samples)

### 9.4 Budgets (hard limits, enforced in CI where measurable)

| Budget | Limit |
|---|---|
| Install size | ≤ 90 MB |
| Runtime memory (PSS) | ≤ 250 MB |
| Largest texture | 2048 × 2048 |
| Concurrent SFX voices | 4 |
| Audio format | OGG Vorbis, mono SFX @ 44.1 kHz |

---

# Part B — Engineering Specification

## 10. Requirements

### 10.1 Functional requirements

| ID | Requirement |
|---|---|
| FR-1 | All gameplay MUST function with no network connectivity; the manifest MUST NOT declare `INTERNET` permission |
| FR-2 | The app MUST support up to 4 local child profiles with independent progress, ratings, inventory, and screen time |
| FR-3 | Every activity round MUST be completable by every child (in-round easing, §5.5) |
| FR-4 | Rewards MUST be persisted transactionally before any celebration UI renders |
| FR-5 | The parent area MUST be protected by a multi-touch hold gate that re-locks on app background |
| FR-6 | Daily screen-time accounting MUST survive process death, device reboot, and day rollover |
| FR-7 | The adaptive engine MUST be deterministic given (rating, outcome history) — no hidden randomness in difficulty |
| FR-8 | A parent MUST be able to export all of a profile's data as JSON and delete a profile (with all data) in one action |
| FR-9 | The game MUST be fully playable with all audio muted |
| FR-10 | Interrupting the app at any point (call, home button, battery death) MUST lose at most the current round's progress — never inventory, ratings, or garden state |

### 10.2 Non-functional requirements

| ID | Requirement | Target |
|---|---|---|
| NFR-1 | Cold start to interactive hub | ≤ 3.5 s on baseline device (§15.1); ≤ 2 s mid-tier |
| NFR-2 | Frame time | 16.6 ms target; automatic reduced-effects mode holds ≤ 33 ms on baseline |
| NFR-3 | Touch-to-feedback latency | ≤ 100 ms |
| NFR-4 | Crash-free sessions | ≥ 99.7% (measured in playtesting; no crash-reporting SDK ships in release — see §18) |
| NFR-5 | Battery | ≤ 6% drain per 15-minute session on baseline |
| NFR-6 | Storage resilience | App remains usable (play + warn) with < 10 MB free; no data corruption on disk-full |
| NFR-7 | Locale/clock robustness | Correct behavior across timezone changes, DST, and manual clock changes (§14.4) |

## 11. Flutter Architecture

### 11.1 Stack decisions

| Concern | Choice | Rationale |
|---|---|---|
| Rendering | Flutter widgets for hub/menus/garden; **Flame** for the five activity scenes | Widgets excel at UI-like screens; Flame's game loop suits water simulation and particles; both proven on low-end hardware |
| Character animation | **Rive** state machines; `AnimationController` for UI | Rive inputs map 1:1 to the hint scheduler's levels; tiny files; GPU-friendly |
| State management | **Riverpod** | Compile-safe DI + state in one mechanism; game logic testable without `BuildContext` |
| Persistence | **sqflite** + DAO layer; `SharedPreferences` only for non-critical toggles | Requirement; mature on API 21+ |
| Audio | `flame_audio`/`audioplayers` behind a pooled `AudioService` | Pre-loaded pools give low-latency one-shots; capped voices |
| Errors | `runZonedGuarded` + `FlutterError.onError` → local ring-buffer log | No network, so diagnostics are local and parent-exportable |

### 11.2 Layering

```
PRESENTATION   Screens, Flame games, Rive controllers          (Flutter)
APPLICATION    SessionController, AdaptiveEngine facade,
               RewardService, ScreenTimeService, HintScheduler,
               AudioService                                     (Riverpod)
DOMAIN         Entities + rules: ChildProfile, SkillRating,
               Round, DifficultyParams, RewardRules             (pure Dart, zero Flutter imports)
DATA           sqflite DAOs, migrations, catalog loader,
               settings store                                   (sqflite / prefs)
```

Dependency rule: arrows point downward only. The domain layer has no Flutter imports and is 100% unit-testable; the pedagogical core (adaptive engine, hint scheduler, reward rules) lives there and must never regress.

### 11.3 Key contracts

```dart
/// Every island implements this; islands are pluggable (expansion seam).
abstract class ActivityGame {
  void configure(DifficultyParams params);
  Stream<RoundEvent> get events;   // attempts, errors, hints, completion
  Future<void> easeOneNotch();     // in-round easing hook (§5.5)
}

/// Pure, deterministic, golden-tested.
class AdaptiveEngine {
  SkillRating update(SkillRating current, RoundOutcome outcome);
  DifficultyParams paramsFor(Island island, SkillRating rating,
      {required bool isFirstRoundOfSession});
}
```

- **`RewardService.grantForRound()`** runs a single DB transaction (seed + sparkle + sprout + rounds row + rating update), then completes a `Future` the celebration overlay awaits (FR-4).
- **`ScreenTimeService`** ticks on a monotonic `Stopwatch`, persists accumulated seconds every 30 s and on every `AppLifecycleState.paused` (FR-6).
- **`HintScheduler`** exposes `Stream<HintLevel>`; the presentation layer forwards levels into Momo's Rive state machine inputs.

## 12. Session & Round State Machines

### 12.1 Session

```
BOOT ──► PROFILE_SELECT ──► HUB ◄─────────────────┐
                             │ island tap          │
                             ▼                     │
                          ACTIVITY ── complete ────┤
                             │                     │
                     limit reached (80%)           │
                             ▼                     │
                          SUNSET (tint + yawn) ────┘  (play continues)
                             │ limit reached (100%) AND current round finished
                             ▼
                        GOODNIGHT (Momo sleeps; input → wave; session ends)
```

### 12.2 Round (inside an activity)

```
INTRO (Momo demonstrates, skippable)
  └─► PLAY ──success──► CELEBRATE (≤5 s, skippable) ─► reward txn already committed ─► exit
        │ error ─► warm "almost" feedback ─► PLAY  (error counter++)
        │ 3 failed attempts ─► easeOneNotch() ─► PLAY  (outcome := eased)
        │ idle/errors ─► HintScheduler escalates L1→L3
        └ home-leaf hold ─► outcome := left (Sparkle banked; no penalty) ─► HUB
```

Both machines are implemented as explicit enums with assertion-guarded transitions — no implicit state via booleans.

## 13. Data Layer & SQLite Schema

### 13.1 Connection policy

- Single database instance, opened at boot with:
  `PRAGMA foreign_keys = ON; PRAGMA journal_mode = WAL; PRAGMA synchronous = NORMAL; PRAGMA busy_timeout = 5000;`
- WAL gives crash-safe atomic commits with good low-end write latency; `synchronous=NORMAL` under WAL is the standard durability/speed balance.
- All multi-row grants (rewards, profile deletes, migrations) run inside explicit transactions.
- Versioned migrations table; migrations are forward-only, each wrapped in a transaction, and covered by tests that upgrade a fixture DB from every prior version (§17.2).

### 13.2 Schema

```sql
CREATE TABLE schema_migrations (
  version     INTEGER PRIMARY KEY,
  applied_at  INTEGER NOT NULL                      -- epoch ms
);

CREATE TABLE profiles (
  id          INTEGER PRIMARY KEY AUTOINCREMENT,
  name        TEXT    NOT NULL CHECK (length(name) <= 24),
  avatar_id   INTEGER NOT NULL DEFAULT 0,
  birth_year  INTEGER,                              -- optional; coarse adaptive prior only
  created_at  INTEGER NOT NULL,
  is_active   INTEGER NOT NULL DEFAULT 1 CHECK (is_active IN (0,1))
);

CREATE TABLE skill_ratings (
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  island        TEXT    NOT NULL CHECK (island IN
                  ('melody','pebble','bamboo','cloud','bug')),
  rating        REAL    NOT NULL DEFAULT 10 CHECK (rating BETWEEN 0 AND 100),
  rounds_played INTEGER NOT NULL DEFAULT 0,
  updated_at    INTEGER NOT NULL,
  PRIMARY KEY (profile_id, island)
);

CREATE TABLE rounds (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  profile_id   INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  island       TEXT    NOT NULL,
  started_at   INTEGER NOT NULL,
  duration_ms  INTEGER NOT NULL,
  difficulty   REAL    NOT NULL,                    -- rating snapshot at start
  outcome      TEXT    NOT NULL CHECK (outcome IN
                 ('clean','hinted','eased','left')),
  hints_used   INTEGER NOT NULL DEFAULT 0,
  errors       INTEGER NOT NULL DEFAULT 0
);
CREATE INDEX idx_rounds_profile_time ON rounds(profile_id, started_at);

CREATE TABLE inventory (
  profile_id INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  item_type  TEXT    NOT NULL CHECK (item_type IN ('seed','sparkle','wardrobe')),
  item_id    TEXT    NOT NULL,                      -- catalog key, e.g. 'seed.tulip'
  quantity   INTEGER NOT NULL DEFAULT 0 CHECK (quantity >= 0),
  PRIMARY KEY (profile_id, item_type, item_id)
);

CREATE TABLE garden_items (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  profile_id   INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  item_id      TEXT    NOT NULL,
  pos_x        REAL    NOT NULL,
  pos_y        REAL    NOT NULL,
  growth_stage INTEGER NOT NULL DEFAULT 0 CHECK (growth_stage BETWEEN 0 AND 2),
  placed_at    INTEGER NOT NULL
);
CREATE INDEX idx_garden_profile ON garden_items(profile_id);

CREATE TABLE sprouts (
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  sprout_id     TEXT    NOT NULL,                   -- 'melody.bell_01' …
  woken_at      INTEGER NOT NULL,
  wardrobe_json TEXT,                               -- equipped cosmetics
  PRIMARY KEY (profile_id, sprout_id)
);

CREATE TABLE play_time (
  profile_id INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  day        TEXT    NOT NULL,                      -- 'YYYY-MM-DD', device-local
  seconds    INTEGER NOT NULL DEFAULT 0 CHECK (seconds >= 0),
  PRIMARY KEY (profile_id, day)
);

CREATE TABLE settings (
  scope TEXT NOT NULL,                              -- 'app' or profile id as text
  key   TEXT NOT NULL,
  value TEXT NOT NULL,
  PRIMARY KEY (scope, key)
);
```

### 13.3 Data lifecycle

- **Retention:** raw `rounds` older than 90 days are pruned after being folded into the 14-day rating aggregates the engine needs and the per-day summaries the dashboard shows — the DB cannot grow unboundedly.
- **Export (FR-8):** one JSON document per profile covering every table, written to app documents and shared via the system share sheet from the gated parent area.
- **Delete:** `DELETE FROM profiles WHERE id = ?` — cascades reach every child-related row; wrapped in a transaction with a confirmation step.
- **Privacy posture:** the only free-text field is the ≤ 24-char nickname; no photos, no contacts, no identifiers.

## 14. Reliability, Failure Modes & Edge Cases

### 14.1 Principles
A 6-year-old cannot troubleshoot. The app must therefore *never* show an error dialog to a child. Recoverable faults degrade silently; unrecoverable faults surface only in the parent area.

### 14.2 Failure-mode table

| Failure | Handling |
|---|---|
| Process killed mid-round | Round is simply lost (FR-10). Rewards for prior rounds are already committed (§6.2 rule 5). On next launch the hub opens normally |
| Crash during celebration | Reward transaction committed before the overlay rendered — nothing lost; celebration simply doesn't replay |
| DB corruption detected on open | Attempt `PRAGMA integrity_check`; on failure, move the corrupt file aside, recreate the schema, and show a parent-area notice with the export path of the damaged file. The child sees a fresh-but-working garden rather than a broken app |
| Disk full on write | Writes retried once; on failure the app enters read-mostly mode (play continues, grants queue in memory and flush when space allows); parent-area storage warning (NFR-6) |
| Audio focus lost (call, another app) | Music pauses, game auto-pauses in activities, resumes on focus return |
| App backgrounded mid-drag | Drag item glides home; round pauses; parent gate re-locks |
| Rive asset fails to load | Static fallback sprite per character ships in the bundle; animation degrades, play continues |
| OOM pressure (`onTrimMemory`) | Drop non-visible island assets and particle pools; hub always remains loadable |

### 14.3 Reward integrity (FR-4 detail)

```
1. Round completes (domain event)
2. BEGIN TRANSACTION
     INSERT round row
     UPDATE skill_ratings
     UPSERT inventory (+seed, +sparkle)
     INSERT sprout (if newly woken)
   COMMIT
3. Only then: celebration overlay plays
```

If step 2 fails, the round result is re-queued in memory and retried; the celebration still plays (the child's experience is never held hostage to I/O), and the queue flushes on the next successful write.

### 14.4 Clock & calendar edge cases (NFR-7)

- Play seconds accumulate from a **monotonic `Stopwatch`**, never wall-clock deltas — manual clock changes cannot inflate or erase today's screen time.
- Day rollover uses device-local midnight; a session spanning midnight splits its seconds across both `play_time` rows.
- If the wall clock moves *backwards* across an existing `play_time` day key, the existing row is reused (never reset) — setting the clock back cannot grant extra play time.
- Timezone/DST changes at most shift the day boundary; accumulated seconds are never recomputed retroactively.

## 15. Performance Engineering for Low-End Devices

### 15.1 Device tiers

| Tier | Reference | Targets |
|---|---|---|
| Baseline | 2 GB RAM, quad-core A53-class, 720p (typical surviving API 21–23 device) | 30 fps stable, reduced effects |
| Mid | 3–4 GB RAM, 1080p | 60 fps, full effects |

Tier is detected at first launch (RAM + a 2 s render benchmark during the splash) and stored; the parent area can override it.

### 15.2 Reduced-effects mode (automatic on baseline or sustained jank)
Halves particle counts, disables ambient critters beyond two, swaps the water shader for frame animation, and caps Rive artboards at one active state machine per screen. A rolling frame-time monitor (last 120 frames) downgrades effects at runtime if p95 frame time exceeds 33 ms — the child never sees a settings screen.

### 15.3 Standing tactics
- Per-island asset loading with an explicit unload on exit; hub resident set ≤ 120 MB.
- Pre-warmed Rive artboards and pooled particles/SFX players (max 4 voices) — zero allocation spikes during play.
- `RepaintBoundary` around ambient layers; ambient animations pause when occluded by overlays.
- All timings derive from the frame clock, not `Timer`s, so background/foreground transitions cannot drift game state.
- CI perf smoke test: scripted 3-round session on an API 21 emulator profile asserting frame-time and memory budgets (§17.3).

## 16. Folder Structure

```
momo_garden/
├── android/                        # minSdk 21; manifest declares NO internet permission
├── assets/
│   ├── audio/{music,sfx,voice,notes}/
│   ├── rive/                       # momo.riv, sprouts_*.riv (+ static fallbacks)
│   ├── images/{hub,islands,garden,ui}/
│   └── catalog/
│       ├── items.json              # seeds, decorations, wardrobe
│       └── difficulty.json         # rating-band → params tables (§5.3)
├── lib/
│   ├── main.dart                   # runZonedGuarded bootstrap
│   ├── app.dart
│   ├── core/
│   │   ├── constants.dart          # touch sizes, timings, palette, budgets
│   │   ├── theme.dart
│   │   ├── logging/ring_log.dart   # local diagnostics ring buffer
│   │   └── audio/audio_service.dart
│   ├── domain/                     # pure Dart — zero Flutter imports
│   │   ├── entities/               # child_profile.dart, skill_rating.dart, round.dart, …
│   │   ├── adaptive/               # adaptive_engine.dart, difficulty_params.dart,
│   │   │                           # hint_policy.dart
│   │   └── rewards/reward_rules.dart
│   ├── data/
│   │   ├── db/database.dart        # open, PRAGMAs, integrity check, recovery
│   │   ├── db/migrations/          # v1.dart, v2.dart, …
│   │   ├── dao/                    # one DAO per aggregate
│   │   └── export/profile_exporter.dart
│   ├── application/
│   │   ├── session_controller.dart # session state machine (§12.1)
│   │   ├── screen_time_service.dart
│   │   ├── hint_scheduler.dart
│   │   ├── reward_service.dart
│   │   └── device_tier.dart
│   ├── games/
│   │   ├── activity_game.dart      # shared contract (§11.3)
│   │   ├── melody/  pebble/  bamboo/  cloud/  bug/
│   │   └── shared/                 # particles, drag helpers, feedback fx
│   ├── screens/
│   │   ├── hub/  garden/  parent/  shared/
│   └── widgets/                    # big_touch_button.dart, momo_rive.dart, …
├── test/
│   ├── domain/                     # engine goldens, reward rules, hint policy
│   ├── data/                       # DAO + migration-chain tests (sqflite_ffi)
│   └── application/                # state machines, screen-time clock cases
├── integration_test/               # full-session flows on emulator
├── .github/workflows/ci.yaml
└── pubspec.yaml
```

---

# Part C — Quality, Compliance & Delivery

## 17. Testing Strategy

### 17.1 Test pyramid

| Layer | Scope | Tooling |
|---|---|---|
| Unit (majority) | Adaptive engine, reward rules, hint policy, state machines, screen-time clock math | `flutter test`; deterministic seeds |
| Data | Every DAO; migration chain from each historical schema version to current | `sqflite_common_ffi` fixture DBs |
| Widget | Parent gate, dashboard, celebration overlay, touch-target sizes | Widget tests + golden images |
| Integration | Full session: boot → profile → activity → reward → garden → goodnight | `integration_test` on API 21 & 34 emulators |
| Performance | 3-round scripted session asserting frame-time and memory budgets | CI perf smoke (§15.3) |

### 17.2 Pedagogy-critical tests (never allowed to regress)
- **Engine goldens:** simulated learner profiles (fast, average, struggling, sporadic) replayed through the engine; asserted rating curves stay within the 70–85% success band and never oscillate more than one difficulty band between consecutive rounds.
- **Invariant tests:** no input sequence can reduce inventory; `left` outcome never lowers rating; first session round is always one band down; EF variants never appear at rating < 61 or in a session's first round.
- **Clock property tests:** randomized clock jumps/timezone changes can never increase remaining screen time or corrupt `play_time`.

### 17.3 CI (GitHub Actions)
`flutter analyze` (zero warnings) → `dart format --set-exit-if-changed` → unit+data+widget tests with coverage gate (domain ≥ 90%) → integration tests on two emulator profiles → perf smoke → asset-budget check (install size, texture caps) → manifest check asserting **no INTERNET permission** (a one-line guard that turns the strongest privacy promise into a build failure).

### 17.4 Child playtesting protocol (ethics-first)
Moderated sessions with parent present and written consent; observation only (no recordings of the child); tasks framed as "help us make Momo better." We measure: unaided task discovery, hint effectiveness, emotional response at errors and at the goodnight sequence, and voluntary-stop behavior. Any observed distress at any mechanic is a release blocker.

## 18. Privacy, Safety & Compliance Matrix

| Area | Commitment | Enforcement |
|---|---|---|
| Network | No connectivity of any kind | No `INTERNET` permission; CI manifest guard (§17.3) |
| Ads / IAP / links | None | No SDKs present; Play listing declares none |
| Analytics / crash reporting | No third-party SDKs; diagnostics stay in a local ring buffer exportable only via parent area | Dependency allowlist reviewed each release |
| COPPA | No collection of personal information; no disclosure possible (no network) | Architecture; nickname-only data model (§13.3) |
| GDPR-K | On-device data; parent-initiated export & erasure | FR-8 |
| Google Play Families | Designed-for-Families program compliance; content rating "Everyone" | Pre-launch checklist per release |
| Photosensitivity | No flashing above 3 Hz | Design rule + review checklist |
| Dark patterns | No streaks, FOMO, countdowns, gated creativity, loss framing, or comparison | Pillar 2 invariant tests (§17.2) + design review |

## 19. Accessibility

- **Motor:** ≥ 76 dp targets, forgiving drops, no timed inputs anywhere, tap-only alternative for every drag interaction (planned as a settings toggle, M3).
- **Hearing:** every audio cue has a visual twin; fully playable muted (FR-9).
- **Vision:** shape is always redundant with color (colorblind-safe patterns); interactive elements outlined + motion-cued, never color-cued alone; UI contrast ≥ 4.5:1 on parent surfaces.
- **Cognition:** one instruction at a time, always demonstrated; no reading required; consistent spatial layout (home leaf never moves).
- **Vestibular:** no screen shake, no parallax beyond gentle cloud drift.

## 20. Delivery Plan & Milestones

| Milestone | Scope | Exit criteria |
|---|---|---|
| **M0 — Prototype** | Hub + Melody Meadow + Pebble Beach, placeholder art, adaptive engine v1 | Engine goldens pass; 5 playtest children complete rounds unaided |
| **M1 — Vertical slice** | One island fully polished (final art/audio/Rive), reward txn path, My Garden basic planting | FR-3/4/10 demonstrated; baseline-device budgets met |
| **M2 — Content complete** | All 6 islands, parent area, screen time, profiles, export/delete | All FRs implemented; migration chain tested |
| **M3 — Polish & beta** | Reduced-effects mode, accessibility toggles, family beta (10–15 households) | NFR budgets green in CI; zero distress observations; crash-free ≥ 99.7% |
| **M4 — Release** | Play Families review, store listing, final compliance pass | §18 checklist signed off by all five disciplines |

Definition of Done (every feature): tests per §17, budgets green, no new warnings, reviewed against the four pillars, and — for anything child-facing — sign-off from the psychology and pedagogy owners.

## 21. Risk Register

| # | Risk | L | I | Mitigation |
|---|---|---|---|---|
| 1 | Rive performance on baseline devices | M | H | M1 spike on real API 21 hardware; static-sprite fallback path already specified (§14.2) |
| 2 | Adaptive engine mistuned → boredom or frustration | M | H | Golden simulations pre-launch (§17.2); band tables are data (§5.3) so tuning ships without code change; playtest telemetry on paper forms |
| 3 | Asset scope creep (15 Sprouts × 6 states, 20 plants × 3 stages) | H | M | Vertical slice (M1) locks the per-asset cost before batch production; budgets in CI |
| 4 | Gibberish voice tests poorly with children | L | M | A/B in M0 playtests against chime-only feedback; barks are cheap to replace |
| 5 | sqflite corruption on low-end flash | L | H | WAL + integrity check + recovery path (§14.2); export encourages parent backups |
| 6 | Parent gate too hard for some parents / too easy for dexterous 7-year-olds | M | M | Playtest both populations at M2; gate pattern is configurable behind a constant |
| 7 | 2–5 min sessions judged "too small" for store featuring | L | L | Positioning: session length is the child-safety feature; parent messaging leads with it |
| 8 | Sunset/goodnight mechanic circumvented by profile switching | M | L | Screen-time limits are per-profile but a device-level daily cap option is available to parents |

*(L = likelihood, I = impact; H/M/L)*

## 22. Future Expansion

**New islands** (pluggable via `ActivityGame`, §11.3): Counting Cove (subitizing, 1–20 number sense), Story Stones (narrative sequencing, early literacy), Rhythm Roots (auditory timing), Mirror Pond (symmetry/spatial reasoning).

**Systems depth:** real-calendar seasonal garden cosmetics (never FOMO-gated — every item returns yearly); goal-free Sprout care interactions; parent-gated photo/postcard mode saving to gallery; side-by-side sibling gardens on one device (relatedness without networking).

**Accessibility & reach:** formalized tap-only mode; switch-access support; offline downloadable narrated-instruction voice packs per language.

**Parent value:** weekly plain-language growth notes; printable offline activity sheets extending each island's skill into the physical world — the Montessori bridge, and the strongest learning-transfer feature on this list.

---

*Fun first, learning second. Nothing to lose, everything to grow.*
