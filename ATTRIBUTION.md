# Attribution

The lock performance work in this repository builds on methods, definitions and
datasets authored by others. **No authorship of any method listed here is
claimed.** They are the work of the organisations and authors named below, and
are referenced, re-implemented or approximated here under their own terms. Where
this repository derives its own quantities, it says so explicitly and does not
present them as any of the methods below.

Verification status is recorded per source, because it affects how much weight a
claim can carry. "Verified" means the primary document was read directly and is
publicly retrievable; "primary, non-public" means the primary source was
consulted directly but is not publicly available, so a reader cannot check it
without obtaining it from the publisher; "secondary" means the claim rests on
third-party summaries and has not been confirmed against the primary source.

## Definitions and norms

**Richtlijnen Vaarwegen 2020** — Rijkswaterstaat, Water, Verkeer en Leefomgeving
(RWS WVL), 2e gewijzigde druk, 20 November 2020, ISBN 9789090338781.
*Verified.* Source of the duration decomposition used throughout
[`GLOSSARY.md`](./GLOSSARY.md): passeertijd, wachttijd, overligtijd, totale
wachttijd, schuttijd, bedieningstijd (§4.2.2, §10.3); the 30-minute norm on
total waiting duration (§4.2.3); the 90th-percentile reliability measure
(§4.2.5); and the forming-up/waiting area design dimensions (§4.6.7–4.6.9).
<https://open.rijkswaterstaat.nl/publish/pages/138463/richtlijnen_vaarwegen_2020_2e_gewijzigde_druk.pdf>

**SIVAK** (Simulatie Verkeersafwikkeling Kunstwerken) — Rijkswaterstaat.
The Richtlijnen Vaarwegen definitions above are stated there to be "ontleend aan
SIVAK" (§4.2.2), so the decomposition originates with SIVAK rather than with the
guideline. SIVAK itself has not been consulted.

**Nota Mobiliteit** — Ministerie van Verkeer en Waterstaat, Den Haag, 2004.
*Secondary, via Richtlijnen Vaarwegen ref. 15.* Origin of the 30-minute average
total waiting time norm for commercial navigation on main waterways.

**Varen voor een vitale economie** — Den Haag, 2007. *Secondary, via
Richtlijnen Vaarwegen ref. 16.* Origin of the 90% value of passing duration as
the reliability measure for lock passage.

## Measurement methods and data sources

**NIS / IVS** (Nationale Informatievoorziening Scheepvaart; Informatie- en
Volgsysteem voor de Scheepvaart) — Rijkswaterstaat. *Primary, non-public.*
The waiting duration definition used in this project's comparison —
`Wachttijd = DatumTijdInvaarGroen − DatumTijdToerbeurt`, duration in seconds —
is quoted from the NIS data dictionary, field 34 (`Wachttijd`), which is
obtainable from Rijkswaterstaat and is not published openly.
The field naming convention matches the IVS export fields handled by
`RWS-NL/fis-crawler`. This project does not re-implement the NIS method; it
compares against NIS published outputs only.

**EuRIS** (European River Information Services) — a product of the RIS COMEX
project, operated by a partnership of European waterway authorities.
*Portal output observed directly; definition secondary.* Source of the average
passage duration metric and of the passage-line mechanism (typed lines at a
configured offset per lock and direction, reported per chamber) that this
project's operational frame approximates. EuRIS requests the following attribution for use of its data or
services, which applies to any downstream service built on it:

> API/Service [name of API/service] incorporated from EuRIS (eurisportal.eu)

Portal: <https://www.eurisportal.eu> · API documentation:
<https://developer.eurisportal.eu/docs>

**Rijkswaterstaat FIS VNDS** — Fairway Information Services, Vaarweg Netwerk
Dienst. ArcGIS MapServer layers consumed by `src/ais_shader/rws.py`, including
lock chamber polygons (`sluiskolk_v`, layer 65), fairway sections, fairways and
kilometre markers.
<https://geo.rijkswaterstaat.nl/arcgis/rest/services/GDR/fis_vnds/MapServer>

**RWS-NL/fis-crawler** — Rijkswaterstaat. Crawler extracting information from
the Dutch Fairway Information System and from EuRIS; the reference for how the
EuRIS bulk export is obtained and for the EuRIS structure entity model
(`LockComplex`, `LockChamber`, `LockChamberArea`, `BridgeArea`,
`BridgeOpening`, `Terminal`, `Berth`).
<https://github.com/RWS-NL/fis-crawler>

**Kooman method** — lock capacity and cycle time analysis. *Not verified.*
Referenced here only because `RWS-NL/fis-crawler` contains an implementation
(`notebooks/kooman-method.ipynb`). The primary publication has not been
consulted and no citation is asserted; anyone building on it should establish
the original reference first.

**Compendium voor de Leefomgeving**, indicator 2143, "Passeertijd sluizen en
ongevallen binnenvaart" (last revised 22 December 2022, no longer updated).
*Secondary.* Source of the statement that passing duration is published as a
proxy because waiting time data is unavailable.
<https://www.clo.nl/indicatoren/nl2143-passeertijd-sluizen-en-ongevallen-binnenvaart>

## Previous work in this repository

See the *References & Previous Work* section of [`README.md`](./README.md) for
the AIS visualization research this project builds on.
