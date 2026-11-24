# EuRIS passage lines: what the bulk export actually contains

Findings from the EuRIS NetworkData bulk export for the Netherlands,
`NL_NetworkData_20260224_v2.4.zip`. These close several questions left open in
[ADR-0001](./adr/0001-two-layer-reference-frame-for-lock-kpis.md) and
[ADR-0002](./adr/0002-evaluate-lock-kpis-by-verdict-ambiguity.md). Source and
licensing in [ATTRIBUTION.md](../ATTRIBUTION.md).

Re-fetch:

```bash
curl -s 'https://www.eurisportal.eu/AWFImportData/api/ExportFile/GetFiles?countryCode=NL'
curl -L -o NL_NetworkData.zip \
  'https://www.eurisportal.eu/AWFImportData/api/ExportFile/DownloadFile?fileName=NL_NetworkData_20260224_v2.4.zip'
```

Exports are published irregularly — the NL series runs Feb, May, Aug and Dec
2025, then Feb 2026. Only two dataset families exist per country:
`NetworkData` and `RisIndex`.

## Passage lines are published

`PassageLine_NL_20260224.geojson`, 6.9 MB, **7,327 LineString features**, with
properties: `PassageId`, `Type`, `ISRS`, `ObjectName`, `SectionRef`,
`FairwayName`, `Offset`, `Angle`, `Length`, `Active`, `TravelDirection`,
`PermitNeeded`, `PermitDelay`, `PermitEnd`, `RelatedISRS`, `RelatedObjectName`.

This is EuRIS's own measurement geometry, so its passage-duration method can be
re-implemented exactly rather than approximated.

### Line types

| `Type` | Count | Offsets observed | What it is |
|---|---|---|---|
| 1 | 4,342 | 0, 300 | Fairway junctions (`ObjectName` is always `junction : A - B`) |
| 2 | 2,021 | 100, 200, null | The structure itself (locks, bridges) |
| 3 | 964 | **500 only** | Lock queue lines |

Type 3 is the "Lock Queue" type seen in the EuRIS configuration UI, and its
offset is **uniformly 500 m across every one of the 964 lines**.

## They attach to complexes, not chambers

The 964 Type-3 lines cover **482 distinct objects, two lines each (one per
travel direction)**. All 482 match a `LockComplex` locode; **none** match a
`LockChamber`. There are 482 lock complexes and 548 chambers in the NL export,
so the mapping is exactly one queue-line pair per complex.

**But EuRIS publishes average passage duration per chamber**, not per complex —
its own chart separates Westkolk, Middenkolk and Oostkolk Volkeraksluizen. The
passage lines cannot make that distinction. Chamber attribution must therefore
come from somewhere else, presumably by locating the vessel inside a
`LockChamberArea` polygon, and those exist for only **405 of 548** chambers
nationally. This seam between a complex-level clock and a chamber-level report
is not documented anywhere we have found, and any re-implementation has to make
the same join explicitly.

## Volkeraksluizen

EuRIS models Volkerak as **two** complexes: `Volkeraksluizen`
(`NLWIS001430956600021`, three chambers) and `Jachtensluis Volkeraksluizen`
(`NLWIS0143A0957300023`, one chamber). Each has its own Type-3 pair at 500 m.

| Chamber | locode | Length (cm) | Width (cm) | `otimemin` | Area polygon |
|---|---|---|---|---|---|
| Westkolk | NLWIS001430956700021 | 32,700 | 2,400 | 30 | yes |
| Middenkolk | NLWIS001430956800021 | 30,890 | 2,410 | 30 | yes |
| Oostkolk | NLWIS001430956900021 | 33,150 | 2,410 | 30 | yes |
| Sluiskolk Jachtensluis | NLWIS0143A0957400024 | 13,500 | 1,600 | 30 | yes |

Chambers link to their complex via `slslocode`.

## `otimemin` is nominal, not measured

Every chamber carries `otimemin`, an operating time in minutes. Nationally it
takes five values: 30 min (343 chambers), 0 (141), 20 (59), 10 (4), 40 (1).
That distribution is a set of defaults, not measurements.

**This produces an internal inconsistency worth pursuing.** All four Volkerak
chambers are listed at 30 minutes, yet EuRIS's own published average passage
duration for those chambers is roughly 21–23 minutes. A passage duration below
the nominal operating time of the same chamber suggests the passage-duration
clock does not span a full locking cycle — which would make it substantially
narrower than Richtlijnen Vaarwegen's `passeertijd`, and would explain part of
the gap to the NIS-sourced figure of about 51 minutes for the same lock.

## Consequences for the evaluation

- **ADR-0002's open question about configuration consistency is answered for
  the Netherlands: it is consistent.** All 482 complexes are operated by a
  single authority (`wwauthorit = RIS-NL`) and every queue line sits at 500 m.
  The per-organisation variation seen in the configuration UI does not vary
  within NL. It remains open for BE, DE, FR and the Danube countries, which is
  now a cheap check since their exports use the same schema.
- The internal-ambiguity test changes shape. It is no longer "does the
  configuration vary between locks" — it does not — but "is the bottleneck
  verdict sensitive to the 500 m choice". That is still a real sensitivity
  analysis and still worth running, but it tests the *designers'* single choice
  rather than inconsistency between locks.
- An earlier inference that passage lines were probably absent from the bulk
  export was **wrong**. It rested on `RWS-NL/fis-crawler` reading only the lock,
  bridge, terminal and berth layers; that crawler simply does not consume the
  `PassageLine` layer.
