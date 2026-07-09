# Final Project – Multi-Agent AI Travel Planning System


### 

Group Members :

- Alaa Alammar
- Rimas Alqahtani
- Amjad Aldosari


---

##  Problem Definition

**Problem.** Planning a trip involves several distinct cognitive tasks:
understanding the traveler's constraints, researching up-to-date attraction
information, composing a coherent day plan, and checking that plan against
the traveler's budget and preferences. Doing all of this well in a single
LLM call tends to produce itineraries that are generic, ignore the budget, or
hallucinate attractions that don't fit the destination.

**Why agents are suitable.** Each of the sub-tasks above benefits from a
different responsibility, a different prompt, and in one case a different
capability (live web search). Splitting the problem across cooperating
agents lets each one specialize, lets us insert a **review/retry loop** that
catches poor itineraries before they reach the user, and lets us insert
**guardrails** at the system boundary where untrusted user input enters the
system — none of which is possible with one monolithic prompt.

**Target users.** Individual travelers who want a personalized, one-day
itinerary for a destination, plus (secondarily) travel agencies that want a
scriptable planning assistant they can embed in a larger booking flow.



## Documentation & Presentation — Summary

- **Architecture diagram:** rendered above from the compiled LangGraph graph
  (Mermaid), and reproduced as text in Section 3.
- **Workflow explanation:** Section 3; decision points at
  `route_after_guardrail` (reject vs. proceed) and `route_after_review`
  (approve vs. revise, capped retry loop).
- **Agent roles:**
  - *Guardrail Agent* — input validation, prompt-injection detection, PII
    masking.
  - *Coordinator Agent* — plans and routes the pipeline.
  - *Research Agent* — grounds the plan in real data via the Tavily tool,
    with a graceful LLM fallback on tool failure.
  - *Itinerary Agent* — Plan-and-Execute drafting/revision of the itinerary.
  - *Reviewer Agent* — Reflection/self-critique gate before finalization.
- **Monitoring:** every node's execution is timed and logged to
  `state["logs"]`, printed after each test run above.
- **Testing:** normal, invalid-input, security-attack, and tool-failure
  scenarios are all exercised in Section 8.

