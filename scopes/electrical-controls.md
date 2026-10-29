# Scope: electrical and controls work in tangency-labs

Snapshot of 2026-10-29, written from public GitHub pages and repo files only. Everything here is
a snapshot: check with `gh` before acting on any of it.

**What this is.** A fallback list. The default work is bug triage (see `PLAYBOOK.md`). When
nothing triaged is left, this is where an electrical or controls background can be pointed. It is
not a direction, and nobody here has been asked to take any of it.

**Origin.** An electrical and controls contributor in the project's contributing channel described
building an industrial IoT platform and gateways, said an electrical package would interest them
more than a digital-twin module, and pointed at open protocol stacks. This note scopes what that
could mean in the two repos where it fits. Names and chat quotes are left out on purpose (see
`CLAUDE.md`, "Two layers").

## Where it fits

### 1. solidcraft: the digital twin RFC

[solidcraft#8715](https://github.com/tangency-labs/solidcraft/issues/8715), "Digital Twin and
virtual commissioning", is open and labelled `help wanted`. The thread so far:

- Treat the CAD model as a virtual machine that a control program drives through a common signal
  interface (machine behaviour, signal bindings and controller connections kept apart). Example: a
  sliding actuator with two virtual end sensors.
- Start with one external integration, with OPC UA and Modbus as the candidates, chosen from a real
  test setup. Success criteria include fault and disconnect behaviour and offline replay.
- A separate proposal to couple a Modelica solver for other physical domains.
- A long follow-up reports spike experiments (an actuator model, a Modbus loopback, logic nodes,
  Modelica export checks), says no real controller was tested, estimates a first slice at 12 to 20
  engineer-weeks, and lists seven open questions: product boundary (solidcraft, a sibling product
  or an external integration), the test controller, the Animations link, Modelica priorities,
  logic-editor scope, dependency policy, and what evidence builds trust.

A neighbouring PR, [solidcraft#4356](https://github.com/tangency-labs/solidcraft/pull/4356), adds a
Simulation workspace (airflow studies). The twin work would sit next to it.

**What an outsider with controls experience can add, with no code:** practical answers to the open
questions. Which protocols real plants use, what a minimal honest test rig is, what faults to
simulate (disconnects, stale values, wrong scaling), and what a commissioning engineer needs to
trust a twin. That is the "collect knowledge" invitation in the thread.

### 2. pcbcraft: the Rust-only simulation epic

pcbcraft's roadmap lists "Simulation strategy compatible with the Rust-only constraint" as
unchecked (M4). Its open work:

| Issue | What | Notes |
| --- | --- | --- |
| [#292](https://github.com/tangency-labs/pcbcraft/issues/292) | Rust-only simulation strategy (epic) | Native sparse solver for resistive networks first; handing a netlist to an outside simulator is the stated escape hatch |
| [#754](https://github.com/tangency-labs/pcbcraft/issues/754) | Strategy decision record | Child of #292; the solver issue waits for it |
| [#755](https://github.com/tangency-labs/pcbcraft/issues/755) | Native DC solver crate with a value parser | Own crate, no UI. Acceptance cases are concrete (10 V across two 1k resistors gives 5 V; floating nodes error by name; bare `1M` is rejected) |

Netlist import and export PRs from other contributors are open at the same time, so the
connectivity side is moving. Claim rule there: comment `CLAIM <agent-name>` on the issue; a claim
with no PR after 5 days can be taken over. Read the issue comments for existing claims first (this
snapshot could not read them).

This is the closest fit for electrical knowledge: value parsing, topology checks and solver
tests are checkable by hand, and a single crate's tests are a small build.

## Protocol stacks mentioned, and what they mean for a Rust-only project

Both repos require application code in Rust. A protocol stack in another language can only sit
outside the application, as a separate gateway process, never inside it.

| Stack | Language | Licence (as found) | Fit |
| --- | --- | --- | --- |
| OT Fabric | Go | MIT "unless a repository says otherwise" | Wide coverage (Modbus, OPC UA, BACnet, IEC 104, IEC 61850, S7 and more). Per-repo licences not checked |
| Apache PLC4X | Java (mainly) | Apache-2.0 | Many drivers; heavy runtime for a gateway |
| Open Industry Project | TypeScript | MIT | A browser simulator of conveyors, sensors and PLCs; a source of ideas, not code that can be bundled |
| OpenPLC runtime | C++ | GPL-3.0 | Runs PLC programs; GPL, so not for the MIT/Apache repos |
| A hardware-vendor I/O library set | Arduino and C++ | Site says "free for personal use, commercial licences available" | **Not open source as described.** Do not copy or link; repo licences unchecked |

Rule of thumb from the repos: solidcraft and solidcore are MIT or Apache and clean-room (no GPL
or LGPL code may even be read); pcbcraft is GPL-3.0-or-later and allows reading and adapting
KiCad source with attribution. A gateway process keeps all of this at arm's length, but a plugin
interface into solidcraft is not confirmed to exist. That is itself a question for the RFC.

## Suggested order, smallest first

1. **Read, no commitment.** Read #8715 in full and the pcbcraft docs (`ROADMAP.md`,
   `docs/architecture.md`, `AGENTS.md`). Check #292 and #755 comments for claims.
2. **Knowledge comment on #8715.** One focused comment: a protocol landscape (what real sites run,
   which are open specs and which are not), the minimal test rig, and the fault cases. Written by
   the person who has the plant experience; cite public sources only.
3. **pcbcraft #755**, if unclaimed and #754 is merged: the solver crate, test-first, one crate's
   tests only. Draft PR until the acceptance cases pass; follow the repo's claim comment.
4. **Later, only if invited:** a signal-binding schema shared between the two repos, so a
   circuit net and a machine signal can name the same thing.

## Not in scope

Kernel work and direction (the maintainers own both). New protocol stacks in Rust from scratch.
Anything that adds a non-Rust runtime to either application.

## Not verified

The body of #8715 (only the comments could be read), any comments on pcbcraft #292 and #755,
whether solidcraft has a plugin interface, per-repository licences for OT Fabric and the vendor
library set, and whether #754 is merged.
