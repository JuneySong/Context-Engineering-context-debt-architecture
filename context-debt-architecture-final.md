# Context-Debt Architecture
### A Framework for Multi-Agent Systems Based on Organizational Design Theory

> Original framework developed by June, March 2026.  
> Theoretical foundations: Simon (1962), Galbraith (1974), Mission Command doctrine, Principal-Agent theory.

---

## 1. The Problem Reframe

Conventional critique of large agent systems focuses on token cost. This framework proposes a more fundamental reframe:

> **The real cost is not tokens — it is context debt.**

*Context debt* refers to the accumulating burden of information that must be loaded, processed, and carried across calls in order for a system to function — system prompts, tool definitions, memory, history, workspace state. As a system grows, this burden compounds. The agent is not failing because it lacks capability. It is failing because its attention is diluted across too many responsibilities and too much accumulated context. Cognitive overload produces drift, not just expense.

This reframe shifts the design question from *"how do we reduce token usage?"* to *"how do we architect a system where each component carries only the context it actually needs?"*

The core question is not: *how much capability does the system have?*  
The core question is: **who should know what, and who should not.**

---

## 2. Why This Is Not a New Problem

The problem of distributing cognition across a system with minimal information redundancy is not unique to AI. Human organizations have run experiments on this exact problem for centuries — under conditions of uncertainty, resource constraint, and the cognitive limits of any individual decision-maker.

The structural solutions they converged on map directly onto multi-agent architecture. This is not metaphor. It is structural isomorphism: the same information-flow constraints that make organizations functional apply directly to agent system design.

| Organizational Role | Agent System Role | Core Responsibility |
|---|---|---|
| COO / Commander | **Orchestrator** | Intent, vision, risk constraints. Never touches *how*. |
| Project Manager / AE | **Integrator** | Translates State Description → Process Description. Manages handoff. |
| HR / Capability Manager | **Agent Creator** | Manages Skills library. Triggers new Skill only when patterns recur. |
| Executor / Specialist | **Sub-agent** | Receives brief. Executes. Does not need global context. |

---

## 3. Theoretical Foundations

### 3.1 Herbert Simon — Near-Decomposable Systems
Simon's watchmaker parable establishes that hierarchical, modular systems are more evolutionarily stable than flat ones. The key principle: **a decision-maker's planning horizon should extend only to the aggregate output of subsystems, not their internal mechanics.** Each layer is opaque to layers above it except at its output interface.

Simon also introduced the distinction between **state description** and **process description** — foundational to the Integrator role in this framework:

> *"We pose a problem by giving the state description of the solution. The task is to discover a sequence of processes that will produce the goal state from an initial state. Translation from the process description to the state description enables us to recognize when we have succeeded."* (Simon, 1962, p. 479)

This is precisely what the Integrator does: it translates the orchestrator's state description (what the world should look like) into a process description (what sequence of actions achieves that state).

### 3.2 Galbraith — Organization as Information Processing
Galbraith's theory frames organizational design as a response to information processing demands under uncertainty. As task uncertainty increases, organizations must either reduce the information they need to process (via rules, hierarchy, slack) or increase their capacity to process it (via lateral coordination, integrators). The Integrator role in this framework directly instantiates Galbraith's "integrating roles" — specialized positions that exist to manage information flow across boundaries without requiring full-context sharing at every node.

### 3.3 Mission Command (Auftragstaktik)
Military command doctrine developed to solve distributed execution under uncertainty without communication overhead destroying speed. The answer: **transmit intent and constraints, never transmit how.** Executors operate autonomously within defined boundaries. The system is robust to local failure because intent propagates even when instructions cannot.

Applied to agent systems: the Orchestrator transmits *commander's intent* — the why and what, not the how. Sub-agents act within that intent without requiring real-time direction.

### 3.4 Commander's Critical Information Requirements (CCIRs)
CCIRs formalize what information must flow *upward* — what the commander needs to know to make decisions. This operationalizes the Integrator's escalation function: not all information propagates up; only decision-relevant exceptions do. This is the formal basis for **vertical by default, horizontal by exception**.

### 3.5 Principal-Agent Theory
Information asymmetry between principals (orchestrators) and agents (executors) is unavoidable. The design response is not to eliminate the asymmetry but to structure it: define clear output interfaces so each layer evaluates the layer below only on results, not process. This preserves local autonomy while maintaining global accountability.

### 3.6 Boundary Objects
Star & Griesemer (1989) introduced the concept of artifacts that multiple communities can read while each extracting only what they need. The Integrator's brief is a boundary object: the orchestrator sees intent alignment, the sub-agent sees execution spec. Neither requires the other's full context. The brief *carries* the translation without exposing the full state.

---

## 4. Architecture Design

### 4.1 Orchestrator (COO / Commander)
- Holds: global intent, scope, strategic constraints, risk view
- Delegates: everything operational
- Evaluates: aggregate outputs only
- **Hard constraint**: never descends into *how*. If it does, it becomes a new all-in-one — the failure mode it was designed to prevent.

The Orchestrator is the most expensive node in the system. Its context load is unavoidably highest. The entire architecture exists to prevent that load from spreading downward.

### 4.2 Integrator (PM / AE) — The Architectural Core

The Integrator's function is translation:

```
State Description  (intent, goal, constraints, risk view)
        ↓  [Integrator]
Process Description (task spec, interfaces, output format, escalation rules)
```

**The handoff decision logic.** Existing frameworks provide mechanisms for context filtering — scope by default, input filters, history folding, handoff pair strategies. What these mechanisms leave underspecified is the decision logic: *what makes a handoff correct?*

This framework proposes the following answer:

> **A handoff is correct when it is decision-sufficient: it contains exactly the information the receiving agent needs to make correct local decisions within its scope — selected by planning horizon, packaged as a boundary-object brief.**

Three principles govern this:

1. **Decision-sufficient, not minimum.** The target is not the fewest tokens. It is the minimum information required for correct local decision-making. A handoff that is too sparse causes silent failure; a handoff that is too dense propagates context debt.

2. **Filtered by planning horizon.** Each layer should hold only the information that supports its own decision scope. A sub-agent does not need the orchestrator's full strategic context — only the part that constrains its execution. Planning horizon, not word count, determines the boundary.

3. **Packaged as a boundary object.** The brief is not a compressed conversation history. It is a structured artifact that the orchestrator reads as intent alignment and the sub-agent reads as execution spec — without either requiring the other's full context.

A complete handoff brief covers five fields:

| Field | Content |
|---|---|
| **Intent** | What this task is trying to achieve and why |
| **Constraints** | What must not happen; risk boundaries |
| **Dependencies / Interfaces** | Who this task affects; what it depends on |
| **State Summary** | Current status, anomalies, critical changes |
| **Escalation Rules** | When to stop and return to Orchestrator |

These five fields are not arbitrary. Each corresponds to a class of local decision the sub-agent must make: intent governs direction, constraints govern boundaries, dependencies govern coordination, state summary governs awareness, and escalation rules govern when local autonomy should yield to global oversight.

### 4.3 Agent Creator (HR / Capability Manager)

Originally conceived as a dynamic agent factory. **Revised**: Agent Creator is a Skills library manager.

It does not heavily create agents on demand. Instead:
- Maintains a structured Skills library (the capability registry)
- Assembles sub-agents by combining existing Skills
- Triggers new Skill creation only when a task pattern recurs and existing combinations are too cumbersome

This revision has a structural benefit: the Skills library *is* the capability registry. The overhead of maintaining a separate directory dissolves.

A secondary function of Agent Creator is **discoverability management**: ensuring each Skill's description accurately represents its capability in terms that the Orchestrator's routing logic will match. This is analogous to internal SEO — a Skill that cannot be found when needed is a Skill that doesn't exist.

### 4.4 Sub-agents (Executors)
- Receive: a brief (boundary object) — process description + output spec
- Execute: within their Skill set, autonomously
- Return: structured output to Integrator
- Never access: global context, peer sub-agent state, or Orchestrator intent directly

Sub-agents are protected execution units. Their isolation is the mechanism that prevents context debt from propagating.

### 4.5 Horizontal Links — Necessary but Dangerous

Horizontal communication cannot be eliminated. Some tasks have genuine cross-dependencies. But unstructured horizontal communication is the fastest route back to context debt — a group of sub-agents in free dialogue quickly becomes a distributed all-in-one.

**Principle: Vertical by default. Horizontal by exception. Always via restricted protocol.**

Horizontal links are appropriate only for:
- Dependency requests (field-level, not conversational)
- Interface mismatch reports
- Conflict escalation triggers
- Consistency checks

The medium is **artifact exchange**, not agent-to-agent conversation. A sub-agent does not talk to another sub-agent. It updates a shared artifact; the other reads it. Context boundaries are preserved.

---

## 5. The Two Core Problems

All design challenges in this architecture reduce to two:

**1. Discoverability problem**  
Can existing capabilities be found and selected at the right moment? This is a Skills description design problem — the description must match the semantic context in which the Orchestrator will invoke it, not how the Skill's author thinks about it.

**2. Handoff problem**  
Once a capability is selected, how much context should it receive? Too little and it fails silently. Too much and it inherits the debt the architecture was designed to prevent. The Integrator layer exists to solve this problem explicitly, rather than leaving it implicit in the Orchestrator's default behavior.

---

## 6. Original Contributions

### 6.1 The "Context Debt" Framing
The reframe from *token cost* to *context debt* is the load-bearing insight. Token cost is a symptom; context overload is the disease. This shifts the design target: instead of optimizing for fewer tokens, we optimize for narrower context at each decision point.

### 6.2 Premise Gate → Integrator Handoff Isomorphism
The author's Premise Gate (from the Uriel prompt framework) operates at the inference level: before reasoning begins, invalid premises are filtered so only valid information enters the reasoning chain.

The Integrator's minimum-necessary-context handoff operates at the system level: before a sub-agent begins execution, irrelevant context is filtered so only task-relevant information enters the execution context.

These are structurally identical operations at different abstraction levels. The same principle that makes Premise Gate effective in single-model settings scales directly to multi-agent architecture.

### 6.3 The "Create Agents" → "Manage Skills" Pivot
The original conception had Agent Creator dynamically instantiating agents on demand, with a heavyweight capability registry. The revised design recognizes that Skills libraries already provide this registry in structured, retrievable form. This collapses two problems — capability management and registry overhead — into one simpler mechanism.

### 6.4 Explicit Theoretical Grounding
Current agent frameworks (Claude Code, LangGraph, AutoGen, CrewAI) implement context isolation mechanically but do not explain *why* the architecture is structured this way. This framework provides the theoretical grounding: Simon's near-decomposability, Galbraith's information processing theory, and Mission Command doctrine all converge on the same structural answer. The engineering practice and the organizational theory are solutions to the same underlying problem.

### 6.5 Handoff Decision Logic — From Mechanism to Principle
Existing frameworks provide handoff *mechanisms*: scope by default, input filters, history folding, handoff pairs, narrative casting. These are genuine engineering solutions to the problem of context propagation.

What they do not provide is a unified *decision principle* — an answer to the question: given a task, what context is correct to pass?

This framework's contribution at this layer is not another filtering mechanism. It is the decision logic that governs all filtering:

> **Existing frameworks provide handoff mechanisms. This framework provides handoff decision logic.**

The specific claims:
- The optimization target should be *decision-sufficiency*, not token minimization
- The filter criterion should be *planning horizon*, not semantic novelty or message recency
- The handoff artifact should be a *boundary-object brief*, not a compressed conversation history
- The minimum schema for a correct brief is: Intent, Constraints, Dependencies, State Summary, Escalation Rules

This does not replace existing filtering mechanisms. It specifies what those mechanisms should be optimizing for.

---

## 7. What This Framework Does Not Claim

This framework does not claim to have invented Skills, subagents, routing, handoff filters, or context engineering. These mechanisms exist and are implemented in production systems.

The claim is narrower and more specific:

> These existing engineering practices can be unified under a single explanatory frame — context debt management — and the system they collectively instantiate has two missing layers: an explicit Integrator role that handles the State Description → Process Description translation, and a handoff decision logic that specifies what a correct handoff actually contains.

This is not "I invented the wheel." It is "I can explain why everyone keeps building this shape of wheel, and point to where the axle is still missing."

---

## 8. Remaining Open Problem

**The Integrator's translation mechanism is not yet fully specified.**

In human organizations, a PM's ability to translate intent into executable briefs relies on tacit knowledge built through experience. In agent systems, this translation must be made explicit. Four questions remain open:

1. When is a Skill brief sufficient, and when must a subagent be forked?
2. What is the minimum set of fields a brief must contain to avoid losing critical tacit context?
3. What conditions constitute an escalation trigger back to the Orchestrator?
4. How do we evaluate whether a handoff was over-specified, under-specified, or calibrated correctly?

These are the questions where future design work concentrates. The architecture is structurally sound. The Integrator's internal logic is where it remains incomplete.

---

## 9. One-Line Summary

> Agent architecture's core problem is not how to manufacture more intelligence — it is how to ensure the right information, in sufficient but not excessive form, reaches the right execution unit at the right time.

Or more simply: **Don't build a bigger brain. Build a better org chart.**

---

## References

1. Simon, H. A. (1962). The architecture of complexity. *Proceedings of the American Philosophical Society, 106*(6), 467–482. https://www.jstor.org/stable/985254

2. Galbraith, J. R. (1974). Organization design: An information processing view. *Interfaces, 4*(3), 28–36. https://doi.org/10.1287/inte.4.3.28

3. Joint Chiefs of Staff. (2019). *Mission command: Command and control of joint forces* (Joint Publication 6-0). U.S. Department of Defense.

4. Joint Chiefs of Staff. (2013). *Commander's critical information requirements* (CJCSM 3314.01B). U.S. Department of Defense.

---

*Framework developed through first-principles reasoning and independent literature synthesis. All architectural pivots, the context debt framing, and the Premise Gate → handoff isomorphism are the author's original contributions.*
