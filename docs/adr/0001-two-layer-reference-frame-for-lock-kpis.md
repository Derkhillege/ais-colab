---
status: accepted
---

# A two-layer reference frame for comparing lock KPIs

Three established methods define lock passing, waiting and locking durations,
and each bounds its clocks with a different kind of observable: Richtlijnen
Vaarwegen 2020 §4.2.2 uses physical events (arrival or mooring, gates closing,
all vessels aboard, the last stern clearing the exit gate); NIS uses lock
operating signals (`Wachttijd = DatumTijdInvaarGroen - DatumTijdToerbeurt`);
EURIS uses geometry (vessels crossing configured passage lines at an offset
from the lock). We compare them in a **two-layer frame**: Richtlijnen Vaarwegen
supplies the *conceptual* decomposition — which quantities exist and what each
method omits — while line-crossing geometry supplies the *operational* frame,
because it is the only one of the three that AIS position data can actually
reproduce. NIS sits outside the operational layer and is compared on published
outputs only.

## Considered options

- **Richtlijnen Vaarwegen as the single frame.** It is the only complete
  decomposition (`passeertijd = wachttijd + overligtijd + schuttijd`) and the
  only normative one. Rejected because it cannot be operationalised: every one
  of its boundaries is a gate or mooring event, and gate/SCADA data is out of
  scope for now. Adopting it alone would mean a frame in which nothing is
  measurable.
- **Three peers, compared pairwise.** Rejected because it yields three pairwise
  divergences with no shared ontology, and therefore no way to say *what* each
  method is missing rather than merely *that* it differs.

## Consequences

- Quantities that the conceptual layer names but the operational layer cannot
  yet measure — lay-by duration, and locking duration if lockage inference
  fails — become an explicit backlog item gated on ODS or SCADA access, rather
  than silently disappearing from the analysis.
- Richtlijnen Vaarwegen's definitions are inherited from SIVAK, Rijkswaterstaat's
  lock *simulation* program ("ontleend aan SIVAK", §4.2.2). The reference frame
  is therefore a model, not ground truth, and results must be stated that way.
- NIS cannot be re-implemented over AIS at all, so any NIS comparison is a
  comparison of published outputs and inherits their aggregation. Note also
  that NIS's own data dictionary records a known disagreement between the
  registered and calculated waiting-time fields (issue 292).
- The claim that NIS norms passing duration as locking duration plus 30 minutes
  of waiting is **unverified**. The 30 minutes is Richtlijnen Vaarwegen's norm
  on *total waiting duration* (§4.2.3, sourced to Nota Mobiliteit 2004). Per-lock
  norm times are set "op basis van wachttijd en schuttijd", which is not an
  equation. Do not build on the arithmetic without the per-lock norm table.
