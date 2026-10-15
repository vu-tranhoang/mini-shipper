# MNS — NPC Delivery Demo & Balance Tool

Document Status: DRAFT / OWNER INTENT
Date: 2026-10-15
Scope: concept and prototype plan only; no gameplay implementation.

## Owner direction
- Prototype first: a small, smooth, playable 2D top-down pixel-art game with square-grid terrain, Stardew Valley-like presentation.
- Defer Isometric 2:1 until the team gains experience and has a solid game loop. See [agent-skills-evaluation.md](agent-skills-evaluation.md).
- Many map locations will initially be **NPCs**. A delivery mission sends the player to **NPC A** to pick up a task/item and then **NPC B** to hand it over and complete the mission.
- Missions should be generated **randomly** using eligible NPCs, not fixed A/B pairs.
- Future extension: houses, restaurants, stores and other locations as pickup sources, with NPCs and buildings modeled separately where appropriate.
- Reward for distant orders must be adjustable through a **table-based configuration tool**, without manually editing gameplay code or hardcoded constants.

## Core gameplay loop
1. Map contains NPC entities with stable IDs and accessible interaction positions.
2. Order generator selects an eligible source NPC A and destination NPC B (distinct; reachable); creates a mission with stable order ID.
3. Order is offered to player; after acceptance, player travels to A.
4. Player interacts with A to pick up the task/item, changing mission state.
5. Player travels to B and interacts to deliver, completing the mission.
6. Reward is calculated from configurable distance bands and future modifiers.

Suggested state machine: OFFERED -> ACCEPTED -> TO_PICKUP -> TO_DELIVERY -> COMPLETED; optional REJECTED/EXPIRED/FAILED. Define exact transition semantics during design.

## Minimal playable demo (proposal, not yet owner-confirmed)
- Small square-grid map, placeholder assets, 5–8 NPCs with distinct IDs and locations.
- Player movement/collision/animation/camera polished enough for a smooth experience.
- Generate random valid A/B pairs; demonstrate several successive missions.
- One active mission at a time for initial test; this does not change the existing concept of max 5 pending notifications.
- Interaction prompts and UI for offer, objective, pickup, completion, payout.
- Debug display for NPC IDs, route distance, chosen price band and mission state.
- No houses/restaurants, multiplayer, procedural world, economy simulation or inventory required in first demo.

## Distance and payout
- Distance should represent a valid traversable route between A and B, not straight-line distance through obstacles. Grid-based pathfinding is an option; method is to be selected when movement architecture is settled.
- Clarify whether pricing is based on A->B only or also includes player->A; initial proposal: A->B for customer-facing delivery distance, optionally separate pickup compensation later.
- Define a currency unit and rounding rules later.
- Example configuration bands (illustrative only):
  | Distance cells | Base reward |
  |---|---:|
  | 0–5 | 100 |
  | 6–10 | 200 |
  | 11–20 | 350 |
  | 21+ | 500 |
- Avoid unreachable routes, same-NPC pairings and ambiguous overlapping bands.

## Delivery Balance Editor — proposed tool
- Godot editor plugin/dock or standalone in-game developer panel (choose after evaluating workflow).
- Table columns: min distance, max distance (nullable for infinity), base reward; later: delivery-time multiplier, difficulty, item category, bonuses.
- Actions: view, add, edit, delete, reorder bands; validate no overlap/gap, nonnegative prices, sensible ranges; save/reload.
- Separate data from game code, version configuration in Git. JSON is a candidate; Godot Resource is an alternative to compare.
- Runtime loads validated config and computes reward through one dedicated pricing service; UI must not directly contain pricing logic.
- Prefer deterministic test cases at boundaries (5/6, 10/11, 20/21), and ensure long-distance orders pay according to config.

## Architecture concepts to evaluate
- NPC Registry: stable IDs, positions, availability, interactions.
- Order Generator: valid randomized A/B pair selection, optional seeded randomness for reproducible tests.
- Mission State Machine: acceptance, pickup, delivery and completion.
- Navigation/Distance Service: reachable path and cost.
- Reward Calculator: data-driven pricing independent from UI.
- Balance Editor: table UI, persistence, validation.
- Future: Location/Building Registry allowing NPCs, restaurants, houses, shops to participate in delivery contracts.

## Existing spec relationships and open decisions
- `specs/vision/project-intent.md` (2026-10-09) currently defines 20x10 playable grid, 2-cell scenery, 4-direction grid-step smooth movement, camera follow, max 5 pending notifications with 10-second response, and timer after Accept. Those are not silently superseded by this concept.
- `specs/epics/2026/initial-prototype.md` currently lists NPCs outside immediate scope. This new Owner direction supersedes **that scope assumption for the upcoming demo**, but the EPIC/story documents require an explicit follow-up alignment before implementation.
- `MNS-0001` remains world/terrain/camera foundation; this document does not create new Story IDs.
- Unresolved: movement mode (grid-step vs free), tile size, Godot version, exact number of NPCs, mission offer cadence, timer semantics, pending/active order limits, distance algorithm, editor UI type, config file format, prices/currency, save behavior.
- Do not mark tests as PASS or claim gameplay implemented until verified in Godot.

## Demo acceptance criteria (proposal)
1. Player can navigate a small top-down map smoothly without collision/camera glitches.
2. NPCs can be interacted with by ID.
3. Multiple generated missions use valid distinct reachable NPC A/B pairs.
4. Picking up at A is required before completion at B.
5. Changing one reward band through the editor persists and affects new mission rewards without modifying game code.
6. Mission completion shows the expected payout and supports starting another mission.

## Follow-up
- Align Owner Intent, EPIC, and Story scope after the owner reviews this demo concept.
- Create implementation stories only according to `gdev-dzu-agents` governance once the workflow is agreed.


## Owner idea update — 2026-10-15: store catalog, NPC roles and cargo-bag puzzle

**Scope:** Preserve as product direction and future-feature backlog. The cargo bag arrangement mini-game is explicitly **not part of the first simple demo**.

### NPC / location capabilities
- A location/NPC can be **sender-only**, **receiver-only**, or **both**.
- Suggested model: `can_send` and `can_receive` capabilities, rather than forcing mutually exclusive classes. A normal customer may only receive; a restaurant can send meals and receive supplies.
- A valid randomly generated mission chooses a source with `can_send=true` and a destination with `can_receive=true`; they must be distinct and reachable.
- Separate NPC identity, business/location identity, role capabilities and map position so these can evolve independently.

### Store-specific merchandise
- Order contents must be consistent with the **sender's catalog/business type**.
- Examples: restaurant -> meals/noodles; milk-tea shop -> milk tea. A restaurant can receive ingredients or goods ordered from another supplier.
- Future data model candidates: `ItemDefinition` (ID, category, footprint), `StoreCatalog` (allowed item IDs), `Order` (source, destination, item lines), `LocationCapabilities`.
- Do not randomly select unrelated items for a shop. Catalog rules, stock and demand generation need later design.

### Future cargo-bag mini-game (DEFERRED)
- Every shipper has a cargo bag with a **grid of cells**; item footprints occupy multiple cells.
- Illustrative owner examples: one bowl of noodles occupies **4 cells**, one milk tea occupies **2 cells**. Exact shapes, orientation and stacking are not decided.
- The player must physically arrange accepted items inside the bag; this is a spatial packing puzzle, not merely a count of free cells.
- Upgrades can expand bag dimensions/capacity, enabling more concurrent deliveries.
- Before accepting/picking up another order, check **current occupied cells, shapes and placement feasibility**, not only maximum cell count.
- Orders offered/accepted should respect the shipper's capacity; distinguish total capacity, current occupancy, pending orders and items already picked up.
- Once cargo is removed/discarded after pickup, the source shop **does not reissue the same goods/order automatically**. Exact penalties, recovery, confirmation and cancellation behavior remain open.
- Do not silently discard cargo or generate an impossible packing state.

### Important open design questions (future)
- Is packing enforced at offer, accept, pickup, or all three stages? How are reserved vs physically carried items represented?
- Are rotations permitted? Can food be stacked, or is each item a rigid 2D shape? What happens with multiple quantities?
- What is the initial bag grid and upgrade progression?
- Can the player rearrange carried goods anytime? What are the consequences of removing an item?
- How does store stock, item availability, destination demand and reward pricing interact?
- How do multiple concurrent deliveries interact with the existing max-five *pending notification* rule (not the same as active-order capacity)?

### Near-term demo boundary
- **Include in the simple demo design:** role-constrained NPC selection and optionally small fixed sender catalogs to demonstrate restaurant vs milk-tea shop.
- **Defer:** bag grid, item placement/rotation, capacity upgrades, cargo discard rules, multi-order packing and economy/stock simulation.
- Do not create implementation stories or modify gameplay code as part of this note.


## Owner update — 2026-10-15: dynamic delivery reward ranges by in-game time

**Owner intent:** Delivery payout must **not** be a single fixed value per distance band. The balance tool must edit **minimum and maximum payout ranges** by **distance interval and in-game time window**. For example, 5–10 cells can yield 15–25đ in one time window, and a different range during peak hours. The values and time bands below are illustrative, not confirmed economy balance.

### Proposed configuration table
| Time window (game clock) | Distance min (cells) | Distance max (cells) | Reward min | Reward max |
|---|---:|---:|---:|---:|
| 06:00–11:00 | 5 | 10 | 15 | 25 |
| 11:00–14:00 | 5 | 10 | 25 | 40 |
| 14:00–18:00 | 5 | 10 | 18 | 30 |
| 18:00–23:00 | 5 | 10 | 30 | 50 |

### Pricing rules to evaluate
1. Determine delivery route distance using the agreed traversable-path distance algorithm.
2. Read **in-game time** at the moment an order is generated/offered (exact pricing moment to be finalized).
3. Find the matching distance interval and time window; sample a random integer amount within `[reward_min, reward_max]` (inclusive, proposed).
4. **Lock the sampled payout into the order** when generated so the displayed offer and eventual reward remain stable even if the clock changes while delivering.
5. Store the pricing rule/version or source values with the order for debug/reproducibility.
6. Avoid gaps, overlaps and invalid bands. Handle cross-midnight windows (e.g. 22:00–02:00), missing config and boundaries explicitly.
7. Define a seedable random generator for reproducible test cases.

### Balance tool requirements
- Editable **table**, not gameplay source edits, with columns for time window, distance range, min/max payout.
- Add/edit/delete rules, validation, save/load and a preview/simulator for selected game time and distance.
- Persist to a versioned configuration file (JSON candidate; Godot Resource alternative).
- Pricing calculation must be a separate service, independent of editor UI.
- Later extensibility: peak-hour bonuses, weather, urgency, cargo size, shop type and economic progression. These are ideas only, not part of the initial demo.

### Demo boundary
- First playable demo can start with a small number of time/distance bands and an editable pricing table; no need to implement advanced economy or cargo-bag packing.
- Previous fixed payout examples in this document are **superseded as the desired model** by the owner decision to use random payout ranges per time window.
- Pending: define game clock speed, complete 24-hour coverage, pricing moment, currency unit, and reward rounding; do not silently assume these are finalized.


## Owner decision — 2026-10-15: initial in-game clock speed

- Use **in-game time**, not the player's real-world wall clock, for pricing and future NPC schedules.
- Initial configurable rate: **1 real minute = 1 game hour**, hence **1 full game day = 24 real minutes** while the simulation clock is advancing.
- This is a **prototype tuning value**, not a permanent balance constraint. Centralize the rate in configuration; no hardcoded 60:1 assumptions throughout gameplay systems.
- Pricing uses the in-game hour at the agreed order-pricing moment, then locks the generated payout into that order.
- Still to decide: starting game hour, pause/menu behavior, offline progression, time display format and persistence.
- Immediate next action: clone `vu-tranhoang/mini-shipper` locally and import existing `project.godot` into Godot; **no gameplay code requested yet**.
