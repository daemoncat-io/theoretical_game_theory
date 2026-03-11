# theoretical_game_theory

A CTF simulation with emergent team AI. No behavior trees. No scripts. No AI.

Just objects perceiving other objects.

**[Live Demo](https://daemoncat-io.github.io/theoretical_game_theory)**

---

## The Claim

Game AI doesn't need to be complex. It needs to be *honest*.

The industry has spent thirty years building elaborate planning systems, behavior trees, NavMesh bakers, and GOAP planners on top of a simpler truth: if you give an object accurate perception of other objects, rational behavior falls out naturally. You don't script it. You don't plan it. You don't train it. It just happens.

This is a proof of that claim. 30 agents. 7 emergent roles. Coherent team tactics. One HTML file.

---

## How It Works

Every agent is an object with:
- A position
- A perception radius
- A shoot cooldown

That's it for constraints. Everything else — role, target, movement, cover-seeking — is derived from conditions at runtime.

Each tick, an agent asks:
1. **What can I see?** *(perception — raycasted, radius-limited)*
2. **What is true right now?** *(conditions — is the flag taken, is the carrier threatened, am I crowded)*
3. **What should I be?** *(role selection — falls out of 2)*
4. **Where should I go?** *(target selection — falls out of 3)*

No state machine. No planner. No global blackboard. The flag object has a `carrier` field. The agent checks it. That's the extent of the "world state."

The complexity lives in the conditions, not the agents.

---

## Object in World Orientation

This is not Object Oriented Programming in the classical sense and it is not world-state-driven planning in the GOAP sense.

It's simpler than both.

Objects exist. Objects have state. Other objects perceive that state directly through the physics of the simulation — line of sight, proximity, geometry. No messages. No events. No registry. The agent looks at the world and decides. Every frame. Independently.

The result is that coordination emerges without a coordinator. Escorts screen carriers because the conditions make that the rational choice, not because a squad manager issued an order. Retrievers chase enemy flag carriers because the boolean stack resolves that way, not because a designer scripted the scenario.

---

## Roles

Roles are not assigned. They are computed per-frame from conditions.

| Role | Condition | Target |
|------|-----------|--------|
| `carrier` | holding enemy flag | own base |
| `retriever` | friendly flag taken | enemy carrier |
| `defender` | agent index ≥ 12 | own flag |
| `escort` | agent index 9–11 | screen carrier |
| `healer` | agent index 7–8 | downed friendly spawn |
| `support` | enemy flag approach crowded | mid-field |
| `attacker` | default | enemy flag |

An agent is not *an* attacker. It *becomes* an attacker this tick, given these conditions. When both flags are simultaneously in play, almost every agent on the field independently evaluates to `retriever`. No special case. No designer intervention. The condition just resolves that way.

---

## The Map

Asymmetric by design. Red has open terrain and minimal cover. Blue has a fortress with a chokepoint entry, inner defenders, and mid-field retreat cover.

Identical code runs on both teams. The map produces structurally different emergent behavior. That's the point.

---

## Lineage

This isn't a new idea. Jeff Orkin shipped GOAP in F.E.A.R. in 2005 — three FSM states, A* over a world-state graph, squad behaviors falling out of individual agents reading shared conditions. Critics called it the best game AI they'd ever faced. The industry proceeded to mostly ignore the underlying philosophy.

Orkin's insight was that you could replace hundreds of scripted FSM transitions with a planner that reads world state at runtime. The insight underneath that insight — which this repo is about — is that you don't need the planner either.

F.E.A.R. GDC 2006 paper: [Three States and a Plan](https://alumni.media.mit.edu/~jorkin/gdc2006_orkin_jeff_fear.pdf)

STRIPS, the academic planning system F.E.A.R. adapted, was published by Stanford in 1971.

The thought is not new. The execution keeps getting more complicated. It shouldn't.

---

## Running It

```
open index.html
```

No build step. No dependencies. No server. The file has no external calls and never will. It will run identically in any browser in any year.

**Controls**
- `click` — inspect agent (shows live condition panel)
- `space` — pause
- `r` — reset

---

## What This Is Not

- Not a game engine
- Not a framework
- Not a library
- Not AI

It's a proof of concept. A baseline. The argument that the starting point for game NPC behavior should be this, not behavior trees.

View source. Copy paste. That's the point.

---

*[DaemonCat LLC](https://daemoncat.io)*
