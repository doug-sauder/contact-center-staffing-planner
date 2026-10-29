# Contact Center Staffing Planner

A workforce-management (WFM) tool that helps answer a core contact center question: *how many Agents do we need, and when, to hit our service goals?*

It does this by simulating an inbound voice contact center. Given a staffing schedule and expected call volume, the simulator reports what customers and Agents would experience: Service Level, ASA, abandonment, Queue length, Occupancy, and more.

## Why

Spreadsheet staffing formulas such as Erlang C assume steady-state behavior and ignore real-world effects like abandonment, wrap-up, breaks, and shift ends. Simulation handles these naturally. It also shows the *spread* of outcomes, not only an average, because each Scenario runs many times with different random values.

## Approach

- **Discrete-event simulation (Monte Carlo).** A Scenario defines the call arrivals, Queue settings, and Agent schedules. The simulator runs it as an Ensemble of independent Replications.
- **Event History as the only output.** Each Replication records every Event (call received, queued, routed, answered, abandoned, and so on). All metrics are computed afterward from that record, so you can define a new metric later without re-running the simulation.
- **API-first.** The planner is intended to be driven through an API, with results stored in a form that supports client-defined and ad hoc queries.
- **Compare staffing options.** An Experiment runs several Scenarios side by side, such as 38 to 46 Agents in steps of 2.

## Status

Early stage. The domain model is written and no code exists yet. The model defines the entities, events, state machines, and metric definitions that the implementation will follow.

- [Domain model (v2)](docs/domain-model-v2.md)

The project uses [Spec-Kit](https://github.com/github/spec-kit) for spec-driven development: feature specs, plans, and tasks are derived from the domain model.

## Scope

The MVP (minimum viable product) covers inbound voice calls, one Contact Center, and one Queue. Other channels and the staffing recommendation logic itself are out of scope for the initial simulator.

## License

[MIT](LICENSE)
