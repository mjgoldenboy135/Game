# Momo's Whistling Garden

**An offline Android game for children aged 5–7**

*Design document by a cross-functional team: child psychologist, Montessori educator, game designer, UI/UX designer, and Flutter developer.*

---

## The Concept in One Paragraph

Momo is a small, cheerful axolotl who lives in a floating garden made of little islands. The garden creatures — sleepy plant-buddies called **Sprouts** — have dozed off, and Momo needs the child's help to wake them up through short, playful activities: matching singing flowers, sorting pattern stones, guiding water through bamboo pipes, and cheering up moody weather-clouds. Every activity ends with a Sprout waking up, giggling, and giving the child a **Sunny Seed**. Seeds are planted in the child's own **My Garden**, a free-play sandbox that grows into a living, personal record of everything they've done. There is no losing, no timer pressure, no score comparison — only a garden that gets more alive the more you play.

**Fun first, learning second.** The child thinks they're waking up silly plant creatures. The psychologist knows they're training working memory, attention, logic, emotional recognition, and executive function.

---

## 1. Core Gameplay Loop

### The 2–5 minute session loop

```
┌─────────────────────────────────────────────────────────┐
│  1. OPEN   → Momo waves hello, garden hub appears        │
│  2. CHOOSE → Child taps one of 6 activity islands        │
│  3. PLAY   → One activity round (90–180 seconds)         │
│  4. WAKE   → A Sprout wakes up, celebrates the child     │
│  5. EARN   → Sunny Seed drops into the child's pouch     │
│  6. PLANT  → Optional: plant/decorate in My Garden       │
│  7. CLOSE  → Natural stopping point; Momo yawns if the   │
│              screen-time limit is near ("Momo is sleepy")│
└─────────────────────────────────────────────────────────┘
```

Every loop iteration is a complete, satisfying arc. The child can stop after one island (≈2 min) or play two or three (≈5 min). The design deliberately creates **natural exit points** rather than hooks that resist stopping — the opposite of a dark pattern.

### The six activity islands

| Island | Activity | Primary skill |
|---|---|---|
| 🌸 Melody Meadow | Singing flowers play a short tune-and-light sequence; child replays it by tapping | Working memory, auditory memory |
| 🪨 Pebble Beach | Pattern stones (shape/color sequences); child drags the stone that comes next | Logic, pattern recognition |
| 💧 Bamboo Falls | Rotate bamboo pipe pieces to route water to a thirsty Sprout | Problem-solving, planning |
| ☁️ Cloud Hill | Weather-clouds show feelings (happy, sad, grumpy, scared, surprised); child matches the face, then picks what helps ("a hug", "a song", "a snack") | Emotional recognition, empathy |
| 🐞 Bug Burrow | Find-and-tap: spot the hiding critters in a busy, gently animated scene | Selective attention, visual search |
| 🎨 My Garden | Free-play sandbox: plant seeds, mix flower colors, arrange decorations, dress up Sprouts | Creativity, autonomy |

My Garden is always unlocked and never gated — creativity must never be a reward you can fail to earn.

---

## 2. Learning Objectives

Mapped to the requested developmental targets, ages 5–7 (Piaget's late preoperational → early concrete operational stage):

| Domain | Objective | Where it's trained |
|---|---|---|
| **Memory** | Hold and reproduce sequences of 2→6 items; recall object locations | Melody Meadow; "who was hiding where" bonus in Bug Burrow |
| **Attention** | Sustain focus 60–120s; filter distractors; shift attention on cue | Bug Burrow; all activities use gentle ambient motion as calibrated distraction |
| **Logic** | Extend AB/ABB/ABC patterns; classify by one, then two attributes; seriation (small→big) | Pebble Beach; sorting variants |
| **Creativity** | Open-ended construction with no "correct" outcome; divergent color/shape mixing | My Garden |
| **Emotional recognition** | Name 5 basic emotions from face + posture + sound; connect feeling → helpful response | Cloud Hill |
| **Problem-solving** | Plan 2–4 steps ahead; test-and-revise without frustration | Bamboo Falls |
| **Executive function** | Inhibition ("only tap the *blue* bugs"), cognitive flexibility (rule switches mid-round), working memory | Cross-cutting: rule-switch variants appear in every island as difficulty rises |
| **Fine motor** | Precise tap, drag-and-drop, rotation gestures | All controls |

Montessori alignment: each activity is **self-correcting** — the material itself shows whether it worked (water flows or doesn't; the flower song sounds right or a flower gently shakes its head). No adult judgment, no red X.

---

## 3. The Psychology Behind Each Mechanic

### Piaget — concrete operations through concrete objects
Children 5–7 learn by manipulating tangible things, not abstract symbols. Every puzzle is physical and visible: real water flowing, real stones being placed, real flowers singing. Pattern logic is taught with objects (stones, petals) rather than symbols or numbers. Bamboo Falls externalizes cause-and-effect: rotate a pipe, immediately see water change course — a concrete lab for hypothesis testing.

### Vygotsky — the Zone of Proximal Development, with Momo as scaffold
Momo is the "more knowledgeable other." When a child hesitates (~8s of no input) or makes a second consecutive error, Momo offers a **graduated hint**: first a glance toward the answer, then a pointing gesture, then a gentle demonstration. Scaffolding fades automatically as competence grows — hints arrive later and less explicitly at higher adaptive levels. The child is always working *just* beyond what they could do alone, never far beyond.

### Montessori — freedom within structure, self-correction, real choice
- The hub is an open shelf: all islands visible, child chooses freely (autonomy).
- Materials are self-correcting (see §2) — the environment teaches, not an authority figure.
- One activity, one skill focus, minimal decoration *inside* the task area (isolation of difficulty).
- No extrinsic grades. Progress is embodied in the garden itself, like a Montessori child's growing tray of completed works.

### Erikson — Industry vs. Inferiority (and late Initiative vs. Guilt)
Ages 5–7 straddle Erikson's stages 3–4: children need to feel *capable* and that their initiative is welcomed. Every session ends with visible evidence of competence (a Sprout awake, a garden grown). Mistakes never produce shame cues — no buzzer, no red flash, no character disappointment. Wrong answers get a warm "almost!" animation: the stone wiggles and hops back, Momo giggles encouragingly. The child's initiative (choosing islands, decorating freely) is always honored.

### Positive reinforcement — reward effort, never punish
- **Effort is rewarded**: attempting a round earns warmth (Momo cheers a try); completing it earns the Seed. There is no fail state — a hard round gently self-simplifies (see §4) until the child succeeds.
- **Variable delight, fixed rewards**: the Seed is guaranteed (predictable safety), but the Sprout's wake-up animation is drawn from a pool of surprises (sneezes, somersaults, silly hats) — variety without gambling mechanics. No loot boxes, no near-miss manipulation.
- **Praise targets process, not person**: Momo's voice lines say "You kept trying!" and "You found a way!" — never "You're so smart!" (per Dweck's growth-mindset findings, ability-praise makes children avoid challenge).

### Intrinsic motivation — Self-Determination Theory (autonomy, competence, relatedness)
- **Autonomy**: free island choice; My Garden has no goals at all.
- **Competence**: adaptive difficulty keeps success rate ~70–85%, the sweet spot where children feel challenged but capable.
- **Relatedness**: Momo and the Sprouts remember the child ("You woke me up yesterday!"), creating a caring relationship. The child is the *helper*, not the performer — helping others is intrinsically motivating for this age.
- **No extrinsic undermining**: no daily-login bribes, no streaks, no countdown scarcity. Rewards are woven into the fiction (seeds → garden), which research shows preserves intrinsic interest better than points/badges.

### Flow — Csíkszentmihályi, tuned for small children
Flow needs: clear goals (one Sprout to wake), immediate feedback (every tap responds within 100ms with sound + motion), and challenge≈skill balance (adaptive engine). Sessions are short because sustained attention at this age is ~10–15 minutes maximum; we target the *front* half of that curve and end on a high note, so the memory of play is always positive (peak-end rule).

### Executive function — deliberate, disguised training
Inhibition and switching tasks are the strongest predictors of school readiness. We disguise them: "Only tap bugs wearing hats!" (inhibition), then mid-round Momo announces "Now only *sleeping* bugs!" (set-shifting). These rule-switch rounds only appear once the adaptive engine sees stable performance, so they feel like a fun twist, not a trick.

---

## 4. Level Progression & Adaptive Difficulty

### No levels — growth stages instead

There is no "Level 12" and no locked content wall. Each island has an invisible **skill rating** per child (0–100). The garden world itself visibly matures through 5 **Garden Seasons** (Sprout → Bud → Bloom → Grove → Festival) based on total Sprouts woken, giving a sense of journey without gating play.

### The adaptive engine (per island, per child profile)

```
target success rate: 70–85%

after each round:
  clean success (no hints)        → rating += 4
  success with hints              → rating += 2
  round self-simplified mid-play  → rating -= 3   (never shown to child)

difficulty parameters derived from rating, e.g. Melody Meadow:
  rating  0–20 : 2-note sequences, slow playback, big flowers
  rating 21–40 : 3 notes, normal speed
  rating 41–60 : 4 notes, one visual distractor flower
  rating 61–80 : 5 notes, playback only (flowers don't light on replay)
  rating 81–100: 6 notes + rule-switch variants ("play it backwards!")
```

**Graceful in-round easing** (the anti-frustration system): after 3 unsuccessful attempts in one round, the round quietly simplifies — a pipe pre-rotates itself, one sequence note lights up longer — until the child succeeds. The child never sees a "difficulty lowered" message; success is always ultimately reached. Ratings also decay slightly toward the child's 2-week average, so a bad day doesn't feel like lost progress.

### Session pacing
- First round of a session is pitched slightly *below* the child's rating (warm-up, guaranteed early win).
- Rule-switch/EF variants never appear in the first round of a session.

---

## 5. Reward System

### Currency and celebration — all effort-based, nothing punitive

| Reward | Earned by | Used for |
|---|---|---|
| **Sunny Seed** | Completing any activity round (guaranteed) | Planting flowers/plants in My Garden |
| **Sparkle** | Trying — awarded per round *attempted*, success or not | Cosmetic dust: makes garden items shimmer; watering-can trails |
| **Sprout Friend** | Each woken Sprout moves into My Garden permanently | Living population; Sprouts play idle animations, wave at the child |
| **Wardrobe Bits** | Garden Season milestones (total Sprouts woken) | Hats/scarves for Momo & Sprouts — pure cosmetics |

### Hard rules (child-safety by design)
- **Nothing can ever be lost.** No decay, no wilting-if-you-don't-return, no streaks to break.
- **No comparison.** No leaderboards, no "best score," no stars-out-of-three. The only progress display is the garden itself.
- **No gambling shapes.** Reward pools are variety, not rarity tiers; no spinning wheels, no "rare" pressure.
- **Celebration is capped** at ~5 seconds and skippable with a tap — respects the child's momentum and avoids overjustification.
- **Praise lines rotate through process-focused phrases**: "You listened so carefully!", "You didn't give up!", "What a creative garden!"

---

## 6. UI/UX Guidelines

### Interaction rules
- **Touch targets ≥ 76dp** (well above the 48dp adult minimum — 5-year-old fingers are imprecise and often tap with the pad, not the tip).
- **Two gestures only: tap and drag.** Rotation in Bamboo Falls is tap-to-rotate (90° per tap), never a twist gesture. No pinch, no long-press, no swipe-dependent navigation.
- **Forgiving drag**: generous drop zones (1.5× the visual target); a released item animates smoothly to the nearest valid slot or glides home — it never "falls" or disappears.
- **Immediate feedback** on every touch: ≤100ms visual response + a soft sound. Even tapping the background makes grass rustle or a butterfly flutter — the world always responds.
- **Zero reading required.** All instruction is Momo's demonstration + voice-over + iconography. Any text on screen is decorative or parent-facing.
- **No back-stack traps**: a single persistent "home leaf" button (bottom-left) always returns to the hub in one tap; leaving mid-round loses nothing (Sparkle for the attempt is already banked).

### Visual design
- **Palette**: warm, bright, low-saturation-anchored — soft mint greens, sky blues, sunflower yellows, coral pinks. High contrast between interactive and background elements (interactive items get a subtle white outline + gentle idle bobbing).
- **Shapes**: everything rounded; no sharp corners anywhere in the world or UI.
- **Motion**: slow, springy, physical (ease-out curves, 200–400ms). Nothing flashes faster than 3Hz (photosensitivity safety). Ambient motion is calm — drifting clouds, swaying grass.
- **Typography** (parent screens only): a rounded humanist sans (e.g. Quicksand/Nunito), generous sizing.

### Audio design
- **Music**: gentle pentatonic loops (pentatonic scales can't sound "wrong," which matters when flower notes are child-triggered), ~60–70 BPM, lullaby-adjacent but bright.
- **SFX**: soft mallets, water drops, hand-drum taps — no harsh buzzers, ever. "Incorrect" sounds are curious ("hmm?" marimba wobble), not negative.
- **Voice**: Momo speaks in warm gibberish ("Simlish"-style) + emotive tone, so no localization burden and no language dependency; meaning is carried by animation and demonstration.
- Full **mute/music/SFX toggles** in the parent area; game is 100% playable silently (all audio cues have visual twins — important for hearing-impaired children and quiet households).

### Parent controls & screen-time
- **Parent Gate**: parent area locked behind an age-appropriate gate (e.g. "touch and hold the three leaves in the corners at the same time" — a multi-touch pattern young children can't reliably perform, better than a math question a 7-year-old can solve).
- **Parent dashboard**: session-length setting (5/10/15/20 min or off), daily play summary, per-skill progress descriptions in plain language ("Maya is now remembering 4-note melodies"), audio toggles, profile management (up to 4 children), data export/delete.
- **Screen-time reminders in-fiction**: when the limit nears, the garden shifts to golden "sunset" light and Momo yawns; at the limit, Momo curls up to sleep and waves goodnight — the session ends warmly, not with an alarm or a slammed door. The child can always finish the current round.
- **No exit punishment**: the goodnight scene is identical whether the child stops voluntarily or via the timer.

### Safety & compliance checklist
- No ads, no IAP, no external links, no analytics SDKs, no network permission at all (the manifest simply doesn't request INTERNET — the strongest possible privacy guarantee).
- COPPA/GDPR-K aligned by architecture: all data stays on-device; parent-initiated export/delete.
- Designed For Families (Google Play) compliant.

---

## 7. Character & World Design

### Momo the Axolotl — the guide
A palm-sized pink-coral axolotl with big friendly eyes, feathery gills that wiggle when excited, and a tiny leaf backpack for Sunny Seeds. Axolotls regenerate — a quiet metaphor for "mistakes heal, you can always try again," and a distinctive silhouette children haven't seen a hundred times. Momo never instructs from authority; Momo *asks for help* ("the Sprouts are so sleepy!"), casting the child as the capable one (Erikson's industry).

**Animation personality**: bouncy idle breathing, celebratory somersault, thoughtful chin-tap when hinting, exaggerated cozy yawn at session end.

### The Sprouts — the friends you wake
Round-bodied plant creatures, each themed to its island: musical bell-flowers (Melody Meadow), pebble-shelled tortoise-buds (Pebble Beach), droopy fern-pups (Bamboo Falls), fluffy cloud-lambs (Cloud Hill), lantern-beetles (Bug Burrow). Waking Sprouts have distinct silly personalities revealed in their wake-up animation. Woken Sprouts live in My Garden and remember the child.

### The world — the Whistling Garden
Floating garden islands connected by petal bridges, drifting in a soft pastel sky. Called "Whistling" because wind through the flowers makes gentle pentatonic tones — the world itself is an instrument. Each island has a distinct color key and landmark for pre-reader navigation (children navigate by color + landmark, not labels). Day/night tint follows the real device clock subtly, supporting the sunset screen-time fiction.

---

## 8. Asset List

### Characters & animation (Rive or sprite-sheet, see §9)
- Momo: idle, walk, point/hint (3 escalation poses), cheer, somersault, yawn/sleep, wave — ~9 animation states
- Sprouts: 5 families × 3 variants = 15 Sprouts; each: sleeping, stir, wake-celebration (2 variants), idle-in-garden, wave — ~6 states each
- Ambient critters: butterfly, ladybug, snail, bee (2-frame loops)

### Environments
- Hub: sky background (day/sunset/night tints), 6 island bases, petal bridges, animated clouds (3), grass/foliage overlays
- Per-island activity backdrops (6) + interaction props:
  - Melody Meadow: 6 singing flowers (lit/unlit/wiggle states)
  - Pebble Beach: 12 pattern stones (4 shapes × 3 colors), sand slots
  - Bamboo Falls: pipe pieces (straight, elbow, T), water-flow shader/frames, thirsty Sprout pot
  - Cloud Hill: 5 emotion cloud faces × 2 sizes, "helper" icon cards (hug/song/snack/blanket/friend)
  - Bug Burrow: busy burrow scene (3 density layers), 10 hideable critters, accessory variants (hats, glasses) for inhibition rounds
- My Garden: 20 plantable flowers/plants (each with 3 growth stages), 15 decorations (fences, ponds, lanterns, swings), 12 wardrobe items

### UI
- Home leaf button, seed pouch, parent-gate leaves, parent dashboard components (charts drawn in Flutter, no images needed), celebration particles (petals, sparkles), hint-glow shader

### Audio
- Music: hub loop, 6 island loops, sunset/goodnight lullaby (~8 tracks, 60–90s loops, OGG)
- SFX: ~40 one-shots (taps, drops, water, pops, chimes, wake-up fanfares ×5, "hmm?" wobble)
- Momo voice: ~25 gibberish emotive barks (greeting, encouragement ×6, hint ×3, celebration ×5, yawn, goodnight…)
- Flower notes: pentatonic set C-D-E-G-A across 2 octaves (10 samples)

### Budget note (low-end support)
All art vector-first (SVG/Rive) rendered to resolution-appropriate rasters at install; total asset footprint target **< 90 MB**, runtime memory target < 250 MB, no texture atlases beyond 2048×2048.

---

## 9. Flutter Architecture

### Stack decisions

| Concern | Choice | Why |
|---|---|---|
| Engine | **Pure Flutter + `flame`** (Flame only for activity scenes) | Hub/menus/garden are widget-friendly; Flame's game loop suits the water sim & particles. Both run well on low-end devices. |
| Animation | **Rive** for characters, implicit/`AnimationController` for UI | Rive state machines map perfectly to Momo's hint-escalation logic; tiny file sizes |
| State management | **Riverpod** | Compile-safe, testable, no BuildContext plumbing in game logic |
| Local DB | **sqflite** + lightweight DAO layer | Requirement; mature, works on Android 5.0+ |
| Audio | **flame_audio / audioplayers** with a pooled SFX service | Low-latency one-shots + looping music channels |
| DI / services | Riverpod providers | Single mechanism, fewer concepts |
| Min SDK | Android 5.0 (API 21), target 60fps with graceful 30fps fallback | Low-end reach |

### Layered architecture (offline-first, no data layer for network — there is no network)

```
┌────────────────────────────────────────────────────────┐
│ PRESENTATION   Widgets, Flame Games, Rive controllers  │
│                (screens/, games/, widgets/)            │
├────────────────────────────────────────────────────────┤
│ APPLICATION    Riverpod controllers & services:        │
│                SessionController, AdaptiveEngine,      │
│                RewardService, ScreenTimeService,       │
│                AudioService, HintScheduler             │
├────────────────────────────────────────────────────────┤
│ DOMAIN         Pure Dart entities & rules:             │
│                ChildProfile, SkillRating, Round,       │
│                GardenItem, Sprout, DifficultyParams    │
│                (no Flutter imports — 100% unit-testable)│
├────────────────────────────────────────────────────────┤
│ DATA           sqflite DAOs, migrations,               │
│                SharedPreferences (settings), asset     │
│                manifest loader                         │
└────────────────────────────────────────────────────────┘
```

### Key components

- **`AdaptiveEngine`** (pure Dart): consumes `RoundResult`, updates `SkillRating`, emits `DifficultyParams` per island. Deterministic and fully unit-tested — the pedagogical core must never regress.
- **`HintScheduler`**: per-round timer + error counter → emits `HintLevel(0–3)`; presentation layer maps it to Momo's Rive state machine inputs.
- **`ScreenTimeService`**: accumulates foreground play seconds per profile per day (persisted every 30s and on lifecycle pause); drives sunset tint at 80% of limit and the goodnight sequence at 100%.
- **`RewardService`**: transactional grant of seeds/sparkles/sprouts on round completion — DB write happens *before* the celebration animation, so a crash can't eat a reward.
- **Each activity = one `FlameGame` subclass** implementing a common `ActivityGame` interface (`configure(DifficultyParams)`, `Stream<RoundEvent>`), so islands are pluggable — this is the future-expansion seam.
- **Performance for low-end**: pre-warmed Rive artboards, pooled particle systems, capped concurrent SFX (4 voices), `RepaintBoundary` around ambient layers, all heavy assets loaded per-island (hub stays < 120 MB resident).

---

## 10. SQLite Database Schema

```sql
-- schema_version managed by a migrations table
CREATE TABLE profiles (
  id            INTEGER PRIMARY KEY AUTOINCREMENT,
  name          TEXT    NOT NULL,            -- child's first name / nickname
  avatar_id     INTEGER NOT NULL DEFAULT 0,  -- picked from preset avatars
  birth_year    INTEGER,                     -- optional, coarse (adaptive priors)
  created_at    INTEGER NOT NULL,            -- epoch millis
  is_active     INTEGER NOT NULL DEFAULT 1
);

CREATE TABLE skill_ratings (
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  island        TEXT    NOT NULL,            -- 'melody','pebble','bamboo','cloud','bug'
  rating        REAL    NOT NULL DEFAULT 10, -- 0..100
  rounds_played INTEGER NOT NULL DEFAULT 0,
  updated_at    INTEGER NOT NULL,
  PRIMARY KEY (profile_id, island)
);

CREATE TABLE rounds (
  id            INTEGER PRIMARY KEY AUTOINCREMENT,
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  island        TEXT    NOT NULL,
  started_at    INTEGER NOT NULL,
  duration_ms   INTEGER NOT NULL,
  difficulty    REAL    NOT NULL,            -- rating snapshot at round start
  outcome       TEXT    NOT NULL,            -- 'clean','hinted','eased','left'
  hints_used    INTEGER NOT NULL DEFAULT 0,
  errors        INTEGER NOT NULL DEFAULT 0
);
CREATE INDEX idx_rounds_profile_day ON rounds(profile_id, started_at);

CREATE TABLE inventory (
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  item_type     TEXT    NOT NULL,            -- 'seed','sparkle','wardrobe'
  item_id       TEXT    NOT NULL,            -- catalog key ('seed.tulip','hat.strawberry')
  quantity      INTEGER NOT NULL DEFAULT 0,
  PRIMARY KEY (profile_id, item_type, item_id)
);

CREATE TABLE garden_items (                  -- placed things in My Garden
  id            INTEGER PRIMARY KEY AUTOINCREMENT,
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  item_id       TEXT    NOT NULL,            -- catalog key
  pos_x         REAL    NOT NULL,
  pos_y         REAL    NOT NULL,
  growth_stage  INTEGER NOT NULL DEFAULT 0,  -- 0..2 for plants
  placed_at     INTEGER NOT NULL
);

CREATE TABLE sprouts (                       -- woken sprout friends
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  sprout_id     TEXT    NOT NULL,            -- 'melody.bell_01' ...
  woken_at      INTEGER NOT NULL,
  wardrobe_json TEXT,                        -- equipped cosmetics
  PRIMARY KEY (profile_id, sprout_id)
);

CREATE TABLE play_time (                     -- screen-time accounting
  profile_id    INTEGER NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  day           TEXT    NOT NULL,            -- 'YYYY-MM-DD' local
  seconds       INTEGER NOT NULL DEFAULT 0,
  PRIMARY KEY (profile_id, day)
);

CREATE TABLE settings (                      -- parent settings, app-global + per-profile
  scope         TEXT    NOT NULL,            -- 'app' or profile id as text
  key           TEXT    NOT NULL,            -- 'daily_limit_min','music_on','sfx_on',...
  value         TEXT    NOT NULL,
  PRIMARY KEY (scope, key)
);
```

Notes: no personal data beyond an optional nickname; `rounds` powers both the adaptive engine and the parent dashboard; everything cascades on profile delete so "delete my child's data" is one statement.

---

## 11. Folder Structure

```
momo_garden/
├── android/                        # min SDK 21; NO internet permission in manifest
├── assets/
│   ├── audio/{music,sfx,voice,notes}/
│   ├── rive/                       # momo.riv, sprouts_*.riv
│   ├── images/{hub,islands,garden,ui}/
│   └── catalog/items.json          # seed/decoration/wardrobe catalog
├── lib/
│   ├── main.dart
│   ├── app.dart                    # MaterialApp, theming, routing
│   ├── core/
│   │   ├── constants.dart          # touch sizes, timings, palette
│   │   ├── theme.dart
│   │   └── audio/audio_service.dart
│   ├── domain/                     # pure Dart, zero Flutter imports
│   │   ├── entities/               # child_profile.dart, skill_rating.dart,
│   │   │                           # round.dart, sprout.dart, garden_item.dart
│   │   ├── adaptive/adaptive_engine.dart
│   │   ├── adaptive/difficulty_params.dart
│   │   └── rewards/reward_rules.dart
│   ├── data/
│   │   ├── db/database.dart        # open/migrate
│   │   ├── db/migrations/
│   │   └── dao/                    # profile_dao.dart, round_dao.dart, ...
│   ├── application/                # Riverpod providers/controllers
│   │   ├── session_controller.dart
│   │   ├── screen_time_service.dart
│   │   ├── hint_scheduler.dart
│   │   └── reward_service.dart
│   ├── games/                      # Flame activity games
│   │   ├── activity_game.dart      # shared interface
│   │   ├── melody/  pebble/  bamboo/  cloud/  bug/
│   │   └── shared/                 # particles, drag helpers, feedback fx
│   ├── screens/
│   │   ├── hub/                    # garden hub + island select
│   │   ├── garden/                 # My Garden sandbox
│   │   ├── parent/                 # gate, dashboard, settings, profiles
│   │   └── shared/                 # celebration overlay, goodnight scene
│   └── widgets/                    # big_touch_button.dart, momo_rive.dart, ...
├── test/
│   ├── domain/                     # adaptive engine + reward rules (highest value)
│   ├── data/                       # DAO + migration tests (sqflite_ffi)
│   └── application/
└── pubspec.yaml
```

---

## 12. Future Expansion Ideas

**New islands (pluggable via the `ActivityGame` seam)**
- 🔢 **Counting Cove** — subitizing and 1–20 number sense with hatching turtle eggs
- 📖 **Story Stones** — sequence picture-stones to retell a story (narrative logic, early literacy)
- 🎵 **Rhythm Roots** — tap-along drumming (auditory processing, timing)
- 🧩 **Mirror Pond** — symmetry puzzles (spatial reasoning)

**Depth on existing systems**
- Seasonal garden events tied to real calendar (winter lanterns, spring blossoms) — cosmetic only, never FOMO-gated (past items return every year)
- Sprout "care" micro-interactions in My Garden (watering, tickling) for calm, goal-free play
- Photo mode: child composes a garden postcard; saved to device gallery via parent-gated share
- Sibling mode: two profiles' gardens visible side by side on one device (relatedness without networking)

**Accessibility & reach**
- Colorblind-safe pattern variants (shapes always redundant with color — partially in place, formalize it)
- Switch-access / single-tap-only mode for motor impairments
- Optional narrated instructions in local languages (voice packs as offline expansion downloads)

**Parent value**
- Weekly plain-language growth notes ("This week Maya practiced holding 5 things in mind")
- Printable offline activity sheets that extend each island's skill into the physical world (Montessori bridge — the strongest "learning transfer" feature on this list)

---

*Fun first, learning second. Nothing to lose, everything to grow.* 🌱
