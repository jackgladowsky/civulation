# Civulation

Author: Jack Gladowsky
Date: August 2025
Version: 0.1 

---

1. Vision & Goals


Elevator Pitch:
A browser-based toy sandbox where LLM-driven agents live in a 2D world, gather resources, craft tools, build structures, and form emergent civilizations. The simulation is data-driven (ECS), but all agent decisions come from LLM tool calls.

Core Goals:


- Agents are autonomous and make decisions via LLMs.
- The world is systemic: resources, crafting, building, needs.
- The sim is accessible: runs in the browser, shareable link, bring-your-own API key.
- The experience is emergent: civilizations form naturally from agent interactions.
Non-Goals (for MVP):


- No fancy 3D graphics (2D tiles only).
- No multiplayer (single-player sandbox).
- No persistence (save/load later).

---

2. Core Features (MVP Scope)

- 2D Tile World
	- Grid-based map with terrain + resources (trees, rocks, food).
- LLM-Driven Agents
	- Each agent has memory, inventory, needs.
	- Each tick, the agent calls an LLM → outputs a tool call (action).
- Actions (Tool Calls)
	- move(dx, dy)
	- gather(target)
	- craft(inputs → output)
	- eat(item)
	- build(structure)
- Crafting System
	- Recipes defined in JSON.
	- Example: 2 wood + 1 stone → stone axe.
- Needs System
	- Hunger + energy decay over time.
	- Agents must eat to survive.
- Simulation Loop
	- Tick-based updates.
	- Context → LLM → Action → ECS update → Render.
- Visualization
	- PixiJS renders tiles + agents.
	- React UI for controls + agent inspector.
- LLM Integration
	- Users provide API key.
	- Agents use LLMs for decision-making.

---

3. Architecture

ECS (bitECS)

- Entities: Agents, resources, items, structures.
- Components:
	- Position (x, y)
	- Inventory (items)
	- Resource (type, amount)
	- Needs (hunger, energy)
	- Agent (marker)
	- Crafting (ability to craft)
- Systems:
	- ActionSystem (applies LLM tool calls)
	- NeedsSystem (decay hunger/energy)
	- WorldSystem (resource regrowth, environment updates)
	- RenderSystem (PixiJS draw)

LLM Agent Layer

- Context Builder: Summarizes local world state + agent state.
- LLM Call: Sends context → gets tool call JSON.
- Action Queue: Stores actions until applied by ECS.

Rendering/UI

- PixiJS: Draws world + agents.
- React:
	- Controls (pause, step, reset).
	- Agent inspector (inventory, needs, last action).
	- API key input.

---

4. Tool Call Schema (LLM → ECS Contract)

	type ToolCall =
	  | { action: "move"; dx: number; dy: number }
	  | { action: "gather"; target: string }
	  | { action: "craft"; inputs: Record<string, number>; output: string }
	  | { action: "eat"; item: string }
	  | { action: "build"; structure: string };


- LLM must always return valid JSON.
- ECS validates and applies the action.
- Invalid actions are ignored (with feedback logged).

---

5. Development Roadmap

Phase 1: Core ECS World (Week 1–2)

-  Set up project (Vite + React + TypeScript + PixiJS + bitECS).
-  Implement ECS world with Position, Agent, Resource.
-  Add ActionSystem (executes tool calls).
-  Render world + agents in PixiJS.

Phase 2: LLM Integration (Week 3–4)

-  Define tool call schema.
-  Implement contextBuilder(agent, world).
-  Implement callLLM(agent, context) with API key input.
-  Agents move/gather via LLM decisions.

Phase 3: Crafting & Needs (Week 5–6)

-  Add Inventory + Crafting components.
-  Add CraftingSystem with recipes.
-  Add NeedsSystem (hunger/energy).
-  Agents eat food to survive.

Phase 4: Structures & Towns (Week 7–8)

-  Add Structure component (huts, storage).
-  Add BuildSystem.
-  Agents can form simple towns.

Phase 5: Polish & UI (Week 9+)

-  Agent inspector (inventory, needs, last action).
-  Dashboard (population, resources).
-  Controls (pause, step, reset).

---

6. Stretch Goals (Future Ideas)

- Procedural terrain (biomes, rivers, mountains).
- Complex crafting tree (tools → buildings → tech).
- Agent memory + storytelling (LLM narrates history).
- Social systems (trade, conflict, alliances).
- Persistence (save/load worlds).
- Multiplayer (shared world in browser).

---

7. Tech Stack

- Frontend: React + TypeScript.
- Rendering: PixiJS.
- Simulation: bitECS.
- LLM Integration: OpenAI/Anthropic API (bring-your-own-key).
- Build Tool: Vite.

---

8. Guiding Principles

- LLM-first: Agents are always LLM-driven, ECS enforces rules.
- Data-driven: Recipes, items, and resources defined in JSON.
- Separation of concerns:
	- ECS = world state + mechanics.
	- LLM = decision-making.
	- PixiJS = rendering.
	- React = UI.
- Toy first, depth later: Focus on making it fun to watch, then add complexity.

---

9. Idea Backlog (for tracking)

-  Agents form tribes and elect leaders.
-  Agents write journals (LLM-generated logs).
-  Agents invent new recipes (LLM proposes new crafting).
-  Agents trade resources with each other.
-  Agents wage wars or form alliances.