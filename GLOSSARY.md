# ais-shader

Deriving waterway traffic measures from AIS vessel position data. This glossary
covers the lock performance domain: the quantities that describe how long a
vessel takes to get through a lock, and the terms the Dutch and European
sources use for them.

The definitions below are drawn from sources authored by others, credited in
[ATTRIBUTION.md](./ATTRIBUTION.md); no authorship of them is claimed here.

Canonical terms are English where an accepted English term exists. Where no
accepted English term exists, the Dutch term is canonical and is used as a
loanword; inventing an English translation would invite drift from the
normative source. Every term records its Dutch original.

## Lock structure

**Lock complex**:
The whole structure at one location, comprising every chamber that serves it.
Dutch: _sluizencomplex_. Volkeraksluizen is one complex of four chambers.
_Avoid_: lock (ambiguous between complex and chamber), lock site

**Lock chamber**:
The enclosed basin, with a gate at each end, that a vessel is raised or lowered
in. Dutch: _kolk_ / _sluiskolk_. A lock complex may have several.
_Avoid_: lock (which means the whole structure), lockage basin, sublock, kolk

**Lock approach**:
The protected water at one end of a lock, between the open fairway and the lock
head, containing the forming-up area and any waiting area. Dutch: _voorhaven_.
_Avoid_: forebay, outer harbour

**Forming-up area**:
Mooring space at a lock where vessels assemble for the *next* lockage. Dutch:
_opstelruimte_. Richtlijnen Vaarwegen 2020 §4.6.7 sizes it at a minimum of 1.1 ×
chamber length.
_Avoid_: waiting area (that is a different, adjacent thing), staging area

**Waiting area**:
Mooring space at a lock where vessels that cannot make the next lockage wait.
Dutch: _wachtruimte_. Distinct from the forming-up area: occupying it means a
vessel has already missed a cycle.
_Avoid_: queue area, holding area

**Mooring structure**:
The fixed structure along the forming-up and waiting areas that vessels moor
against. Dutch: _remmingwerk_. It is the physical referent in the Richtlijnen
Vaarwegen waiting-time trigger ("arrives at the lock and/or moors at the
_remmingwerk_").
_Avoid_: dolphin, fender, guide works

## Lock operation

**Lockage**:
One levelling operation: the gates close, the chamber's water level is changed,
the far gates open, and the vessels inside leave. Dutch: _schutting_. A lockage
carries a group of vessels, so it is the natural unit for any measure of lock
throughput, and it is not observable in AIS position data on its own.
_Avoid_: lock cycle, locking, transit, schutting

**Overligger**:
A vessel that cannot join the first lockage after it arrives and must wait a
further cycle. No accepted English term exists; the Dutch word is canonical.
Richtlijnen Vaarwegen 2020 §10.3 treats it as a first-class concept because the
delay it suffers is a named quantity (lay-by duration) rather than ordinary
waiting.
_Avoid_: queued vessel, bumped vessel, waiter

**Levelling**:
Changing the water level in the chamber to match the far side. Dutch:
_nivelleren_. One component of the operating duration.
_Avoid_: filling, emptying, equalising

## Duration measures

These five quantities form a decomposition: passing duration = waiting duration
+ lay-by duration + locking duration. The decomposition is from Richtlijnen
Vaarwegen 2020 §4.2.2, which inherits it from SIVAK, Rijkswaterstaat's lock
simulation program. Each measurement method operationalises these differently —
see *Measurement methods* below — so a bare duration name is ambiguous unless
the method is stated with it.

**Passing duration**:
The total time a vessel needs to get past a lock. Dutch: _passeertijd_.
_Avoid_: transit time, passage time, lock delay

**Waiting duration**:
The time a vessel spends waiting for the lockage it will join. Dutch:
_wachttijd_. Under Richtlijnen Vaarwegen it runs from arrival at the lock until
locking or lay-by begins, and so includes the time spent entering the chamber.
_Avoid_: delay, queue time, dwell time

**Lay-by duration**:
The extra time an overligger waits because it missed a lockage: from the entry
gates closing in front of it until the lockage it does join begins. Dutch:
_overligtijd_. Richtlijnen Vaarwegen is the only one of the three sources that
names this quantity separately.
_Avoid_: extra waiting, bump time, overlay time

**Total waiting duration**:
Waiting duration plus lay-by duration. Dutch: _totale wachttijd_. This, not
passing duration, is the quantity the Dutch 30-minute lock service norm applies
to (Richtlijnen Vaarwegen 2020 §4.2.3, sourced to Nota Mobiliteit 2004).
_Avoid_: total delay, aggregate waiting

**Locking duration**:
The time one lockage takes: from all vessels being aboard and the gates able to
close, until the last vessel's stern clears the exit-side gate. Dutch:
_schuttijd_. It is a property of the lockage, not of a vessel, so every vessel
in a lockage shares one value.
_Avoid_: cycle time, lock time, levelling time

**Operating duration**:
The time to close the gates, level the chamber and open the far gates. Dutch:
_bedieningstijd_. Equivalently _SNO-tijd_ (sluiten, nivelleren, openen) — the
locking duration excluding the time vessels take to leave.
_Avoid_: machinery time, gate cycle

## Measurement methods

A duration measure is only defined relative to a method, because the three
established methods bound the clocks by different kinds of observable event.

**Richtlijnen Vaarwegen method**:
The Dutch waterway design guideline's definitions (RVW 2020 §4.2.2), inherited
from the SIVAK simulation model. Clocks are bounded by physical events: arrival
or mooring, the gates closing, all vessels being aboard, the last stern clearing
the gate. It is the only method that decomposes passing duration completely.
_Avoid_: RWS definition, the official definition, SIVAK

**NIS method**:
Rijkswaterstaat's Nationale Informatievoorziening Scheepvaart. Clocks are
bounded by lock operating signals rather than vessel movement: waiting duration
is the interval from turn assignment to the entry signal going green.
_Avoid_: IVS, the Rijkswaterstaat KPI

**EURIS method**:
The European River Information Services portal's published average passage
duration. Clocks are bounded by geometry: a vessel crossing configured passage
lines placed at an offset from the lock, reported per chamber and per direction.
_Avoid_: EuRIS passage time, the European definition

**Passage line**:
A line across the fairway, configured per lock and per direction, whose crossing
starts or stops a clock in the EURIS method. Lines carry a type (for example
*Lock Queue*) and an offset in metres from the lock. They are configured per
waterway authority, so whether they are placed consistently across locks is an
open question rather than an assumption.
_Avoid_: gate line, detection line, cordon

**Passage**:
Reserved here for a vessel's act of getting past a lock. Note the collision:
Rijkswaterstaat's fis-crawler uses "passage" for an edge through a structure in
a routing graph, which is an unrelated sense. Where the routing sense is meant,
say *network edge*.
_Avoid_: transit, passing (as a noun), lockage (a different concept entirely)

**Turn assignment**:
The moment a lock operator allocates a vessel its place in the order of
lockages. Dutch: _toerbeurt_. It starts the NIS waiting clock, and has no
counterpart in the other two methods.
_Avoid_: queue position, booking, slot

**Entry signal green**:
The moment the lock's inbound traffic light turns green for a vessel. Dutch:
_invaargroen_. It ends the NIS waiting clock.
_Avoid_: green light, entry permission

**ISRS code**:
The international identifier for a waterway object such as a lock or chamber,
used by EURIS and carried by Dutch lock chamber records. It is the key that
makes a given lock's measures comparable across the methods.
_Avoid_: lock id, ISRS location code, object id
