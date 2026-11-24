---
status: proposed
---

# Acceptance criteria for a new AIS-derived lock KPI method

The evaluation in [ADR-0002](./0002-evaluate-lock-kpis-by-verdict-ambiguity.md)
exists to establish whether the established KPIs suffice; if they do not, a new
method is needed. These criteria are fixed **before** the comparison runs,
because they determine what the comparison has to record — notably a per-method
account of which clock boundaries have no AIS proxy, which would otherwise have
to be retrofitted by re-running the analysis. The method itself is a separate
deliverable; only its acceptance criteria are settled here.

A sufficient method must:

1. **Be computable from AIS position data alone** — no gate, SCADA or operator
   signals — with a documented upgrade path for when ODS or SCADA data becomes
   available.
2. **Decompose** its total into the named Richtlijnen Vaarwegen quantities
   rather than reporting one opaque figure.
3. **Derive its measurement boundaries from observed vessel behaviour** rather
   than from an externally configured line.
4. **Detect waits shorter than 15 minutes**, which Richtlijnen Vaarwegen §4.6.8
   notes do not produce a mooring at all.
5. **Report a distribution, not just a mean** — Richtlijnen Vaarwegen §4.2.5
   uses the 90th percentile of passing duration as its reliability measure.

## Why criterion 3 replaced "stable under configuration differences"

Configuration-stability is not a peer criterion; it is the property that
self-calibration delivers, and it is unachievable while the boundary is a
hand-placed line. It therefore becomes the acceptance *test* for criterion 3 —
the internal-ambiguity experiment in ADR-0002 — rather than a criterion of its
own. Self-calibration also attacks criterion 4, since a vessel that slows
without mooring has a deceleration signature but no stop to detect.

## Known risk to criterion 2

Locking duration is a property of a lockage, not of a vessel, so the
decomposition needs vessels grouped into shared lockages — and without gate
events that must be inferred. The intended approach is co-occupancy clustering
of chamber dwell intervals, validated against an independent exit-burst signal.
Two signals are used because AIS reception inside lock chambers is poor enough
that `detect_polygon_entry_exit()` already needs a `merge_gap_minutes` parameter
to absorb the flicker, so co-occupancy will under-detect membership and needs a
check that does not share that failure mode.

**If lockage inference does not hold up at Volkeraksluizen, criterion 2 is not
satisfiable from AIS and the method falls back to passing duration alone.** This
is the largest single risk to the work.

If inference does hold, an overligger is identifiable as a vessel present in the
waiting area during one lockage that departs in the next — making lay-by
duration measurable, a quantity neither EURIS nor NIS publishes.
