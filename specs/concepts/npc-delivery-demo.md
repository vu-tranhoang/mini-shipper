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
