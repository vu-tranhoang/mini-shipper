# Mini Shipper — Gameplay Modes & Party Racing Concept

Document Status: DRAFT — design discussion notes, not an implementation commitment
Discussion date: 2026-10-12
Owner direction: **Mini Shipper prioritizes Party Racing (Option A)**. Scope may be cut aggressively after playtesting.

## 1. Product positioning

### Mini Shipper
- A lightweight, replayable 2D pixel-art courier game; learn/build with Godot.
- **Single-player:** play delivery missions, progress and unlock achievements at one's own pace. Detailed progression and exact ending rules TBD.
- **Multiplayer:** intended **4–8 players**, real-time party racing; compete for orders, earnings and upgrades, with room for socializing.
- Prefer a simple, fun delivery race over a deep life simulator. Initial target of ~20–30-minute sessions is an **idea to test**, not a fixed duration.
- Avoid implementing all brainstormed systems by default.

### Separate future project: Saigon Life
- A distinct **single-player** life-simulation game with deeper narrative, careers, decisions and life systems, inspired by *Nobody – The Turnaround*.
- Do **not** merge Saigon Life's narrative/simulation scope into Mini Shipper. Shared ideas or technical learnings are possible; shared code is not assumed.

## 2. Core Party Racing loop — current preferred direction

1. Players start together (possible communal boarding house / dorm).
2. Find/accept courier orders, collect goods, deliver, earn money.
3. Choose between immediate income and upgrades such as bicycle, gasoline/electric motorbike, carrying capacity.
4. Compete for limited orders; later consider cooperation, shared challenges and social downtime.
5. Reach a match win trigger; other players get a brief last chance before final results.

**Core-first principle:** A minimal version could be only couriers racing to complete deliveries / earn money. Vehicles, health, sleep, education and alternate careers are optional layers, not mandatory for the first playable version.

## 3. World clock and day/night — direction agreed, tuning open

- In-world time flows **continuously in real time** for everyone in a multiplayer session.
- Day/night visibly changes; activities and special events can happen at night. Night play is a legitimate choice.
- Players choose when to work or sleep; no forced sleep or automatic end-of-day pause.
- **Do not fix** minutes per in-game day or match duration yet. Earlier 8-minute-day / 24-minute-match examples were illustrative and felt too compressed for multiple in-world days.
- Keep world-clock rate separate from match duration; test both.
- In single-player, sleeping may advance the world clock to wake-up time. In multiplayer, sleeping does **not** fast-forward everyone else's clock.

## 4. Stamina, health and rest — concept direction, balancing open

- **Stamina / thể lực:** used during movement/work; food/drink restores stamina.
- **Health / sức khỏe:** changes more slowly. Ordinary eating does not directly restore health. Sleep/rest restores health and stamina.
- Sustained overwork / low-health thresholds can reduce delivery speed, increase stamina drain and eventually make rest worthwhile. Avoid punishing ordinary delivery too harshly.
- Sleep can be chosen **at any time**, including daytime. Candidate durations: **2, 4, 6, 8 in-game hours**.
- In multiplayer, character visibly lies in bed while the shared world continues. Recovery may display gradually.
- **Important unresolved design issue:** mapping sleep hours to real waiting time. A literal clock conversion may create excessive downtime in a short party match; consider compressed sleep or lighter rules after tests.
- **Open:** whether players may interrupt sleep early and what recovery penalty/benefit applies.
- These systems are candidates for later experiments, not requirements for the core race.

## 5. Communal boarding house and social play

- Possible shared house / dorm as the initial spawn and sleeping location, to build a sense of community.
- Starting ritual/catchphrase: **“GIẢI TÁN MƯU SINH!”**
- Nearby players can chat or interact while another character is sleeping; players may also use Discord externally.
- In-game proximity voice chat is a **later idea only**, not MVP scope.
- Later multiplayer could include order competition, cooperative missions and casual social hangouts.

## 6. Education / progression — parked for later

- Possible short courses / certificates to improve delivery ability, earnings, or qualify for a different job.
- Learning happens at a location, creating natural social downtime.
- **Agreed behavior if education is implemented:** leaving a class **preserves accumulated progress**; returning resumes it.
- Deep career changes, degree-based wins and business ownership are **not committed** to the Party Racing MVP; consider later or for Saigon Life.

## 7. Match victory, Last Chance and celebration

- Early concepts include racing to a money target, achieving a better career, or other goals. **Final win condition and scoring are not yet decided.**
- When someone first reaches a win trigger, mark them as provisional leader and start a **Last Chance** window, approximately **15–30 seconds** (30 seconds currently favored).
- **All players continue normal gameplay** during this window. Others may finish an objective or achieve something that overturns the result.
- **How to rank competing goals / tie-breaks / comeback scoring remains open.** An earlier points-based idea was liked but explicitly **parked for later**; do not treat sample scores as rules.
- After Last Chance, cut to a **restaurant celebration scene**: winner on a small podium, other players sitting around a table congratulating them, revealing who won. Exact cutscene details/duration TBD.
- If no one meets a win trigger before a time cap, the outcome rule is TBD.

## 8. Vehicle and economy ideas — not yet scoped

- Progression fantasy: walk for nearby light orders -> bicycle -> gasoline/electric motorbike for farther/heavier/higher-paying deliveries.
- Possible fuel/battery, maintenance, carrying weight, vehicle durability and earnings tradeoffs.
- These systems should be modular if implemented, but **do not build them before the basic courier loop is fun**.

## 9. Architecture implications (design guidance only)

- Separate gameplay systems from presentation and game mode, so single/multiplayer can reuse delivery logic.
- Treat orders as entities with unique IDs and an assigned player; avoid assuming only one player in future designs.
- Shared multiplayer clock, order claims and match result should eventually have authoritative resolution (host/server), to avoid contradictory outcomes.
- Do not build networking, session services or speculative abstractions in **MNS-0001**.

## 10. Scope boundaries / immediate next steps

**Now:** Follow existing prototype EPIC and [MNS-0001](../stories/2026/MNS-0001.md): World/Terrain/Grid/Camera foundation; then movement and A-to-B delivery.

**Next proof of fun:** One player can accept, pick up and deliver; then explore a simple competitive race and test whether multiplayer is genuinely fun.

**Only after tests:** Decide day length, session length, order spawn rate, vehicle upgrades, sleep/health complexity, school/careers, exact victory ranking, online architecture and voice features.

## 11. Decision status summary

**Direction selected:** Party Racing (A), 4–8-player multiplayer goal; continuous day/night; sleep whenever; food restores stamina but not health; sleep restores both; 2/4/6/8-hour sleep choices as a concept; preserve course progress on exit if courses exist; Last Chance then restaurant cutscene; communal house idea; Saigon Life separate single-player game.

**Explicitly undecided:** game-day length; match length; exact sleep time conversion/interruption; all numerical stamina/health formulas; mandatory inclusion of education/vehicles; win thresholds, comeback score and tie-breakers; production networking and voice chat.

**Historical note:** Earlier brainstorms considered a deeper multiplayer life-sim (Option B) and multiple career-based win paths. Owner subsequently selected **Option A**, so those remain optional inspiration rather than Mini Shipper's core product promise.
