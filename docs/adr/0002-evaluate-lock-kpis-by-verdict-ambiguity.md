---
status: accepted
---

# Evaluate lock KPIs by verdict ambiguity, not value divergence

The question this evaluation answers is whether the established lock KPIs are
sufficient to support an unambiguous judgment about lock performance, and if
not, what method would be. Showing that two methods report different numbers
for the same lock does not answer it — differing numbers are explained away as
differing definitions, both valid. We therefore evaluate against a **decision
rule**: a KPI is insufficient when it fails to determine the verdict, and we
test two separate ways that can happen.

## What we test

- **External ambiguity**: two methods yield different verdicts for the same
  lock over the same period.
- **Internal ambiguity**: a single method's verdict is unstable under its own
  configuration choices. Concretely, re-running the EURIS-style method with the
  passage line at several offsets (its observed value is 500 m) and checking
  whether the verdict flips.

The verdicts in scope, in priority order: is this lock a capacity bottleneck
against a norm; how do locks rank nationally by severity; and has a lock's
performance changed over time.

## Considered options

- **Compare values only** (distributions of passing duration per method).
  Retained as the substrate, rejected as the conclusion: it measures divergence
  without establishing whether that divergence matters.
- **External ambiguity only.** Rejected because internal instability is the
  stronger result: if a method's verdict moves when you move its own
  measurement line, the KPI cannot support the judgment regardless of whose
  definition is preferred.

## Consequences

- The comparison must record, per method, which clock boundaries have no AIS
  proxy, and must separate *proxy bias* from *definitional divergence* — a
  divergence attributable to our approximation is not a finding about the
  method.
- Scope follows from this: published-value comparison runs nationally (NIS
  publishes per lock), while AIS re-implementation runs deep at
  Volkeraksluizen first, with 2–4 contrasting locks added afterwards. The
  selection criteria for those locks are deferred until the Volkerak run shows
  which divergences are real, so the choice is driven by evidence rather than
  by a guess about what varies.
- Change detection over time requires long, consistent AIS history.
  `scripts/crawl_euris.py` collects live and cannot backfill, so this verdict
  lands last and carries a data dependency the other two do not.
- Whether EURIS passage lines are placed consistently across locks is an open
  question, not an assumption: the configuration UI scopes them per
  organisation. It must be established by inspection before any cross-lock
  claim rests on it.
