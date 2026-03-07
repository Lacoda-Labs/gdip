# GDIP-007: Party Comparison and Tract/County Party Calculations — Optional

**Status**: Draft  
**Type**: Optional  
**Created**: 2026-03-06  
**Updated**: 2026-03-06

## Summary

Specifies how to compute and aggregate **party** (e.g. Democratic/Republican share) at census tract and county level, and how district group party at any step is derived from persisted tract totals. This GDIP defines the **tract/county party calculation** that any party data provider must support, and lists **options** for the source of party data (election results, voter registration, KIN). Implementations that report partisan balance (e.g. GDIP-006 comparison) SHOULD follow these rules so that party percentages are consistent and two-party totals sum to 100%.

## Motivation

Stakeholders need comparable party metrics across implementations and across steps (not only the final step). By specifying tract-level persistence (once per VEST or data refresh) and district group aggregation as a sum of tract totals, the protocol ensures fast, consistent party percentages for every district group at every step. Listing data-source options allows implementers to choose or combine sources while adhering to the same tract/county calculation layer.

## Specification

### 1. Tract/county party (required for any party data provider)

- **Tract-level**: Party data is computed **once** per (state, year) when the underlying source (e.g. VEST) is refreshed or the election year changes, and **stored per census tract** (keyed by tract GEOID). When only county-level data is available, each tract is assigned the same party percentages (or proportional vote counts) as its parent county (by state + county FIPS from GEOID). In rare cases where a tract spans counties, implementations MAY use a ratio or area-weighted allocation; such cases SHOULD be documented.
- **Two-party consistency**: Stored tract values (e.g. `votesDem`, `votesRep`, `totalVotes`) SHOULD be two-party consistent: percentages are computed using a denominator of `votesDem + votesRep` so that D% + R% = 100%. If the raw source uses full ballot totals, implementations SHOULD derive two-party totals (e.g. set tract `totalVotes = votesDem + votesRep`) so that district aggregation does not inflate totals or distort percentages.
- **District group at any step**: District group party percentages and vote totals are obtained by **summing the stored tract-level party totals** over the tracts in that group. Formula: `pctDem = sum(votesDem) / (sum(votesDem) + sum(votesRep))`, and similarly for `pctRep`. This is a fast aggregation (single pass over tract GEOIDs in the group) and applies at **any step** (0, 1, 2, … final), not only the final step.

### 2. Options for source of party data

The protocol does not mandate a single source. Implementations MAY use one or more of the following; each can feed the same tract/county calculation layer above. These options may be specified in more detail in separate GDIPs.

- **Past election results (per election, per county or tract)**: e.g. VEST (Voting and Election Science Team) tract- or county-level files. When tract-level results exist, use them directly; when only county-level exists, allocate to tracts using parent county percentages (or ratio where tracts span counties). This is an approximation; actual tract-level results can differ (e.g. urban vs rural within a county).
- **Voter registration (by county, zip, or tract)**: Where available, registration by geography often gives a better signal than a single election. Aggregate to tract then to district group using the same summation rules. Implementations SHOULD document geography (county/zip/tract) and any allocation method.
- **KIN (Known Identity Network) or similar**: Multi-factor authentication, human-in-the-loop vouching, and AI-assisted faction/fraud detection for self-registration of party. Such systems can be most accurate but typically start with a small percentage of the citizenry; the protocol MAY reference them as a future or optional source.

### 3. Protocol advocacy

The protocol SHOULD advocate that election results be reported at **census tract level** for tracts with population above a threshold (e.g. 100) or another value that preserves anonymity, so that implementations can use tract-level data instead of county approximation where available.

### 4. No impact on boundaries

- This GDIP does NOT change the core algorithm (GDIP-004). Party is computed **after** boundaries are fixed. The protocol does not use party data to draw district boundaries.

### Required vs Optional

**Optional**: Implementations MAY implement this GDIP. Conforming implementations that do not implement it do not report party metrics; boundary output and core protocol are unchanged. Implementations that do report party (e.g. for GDIP-006 comparison) SHOULD follow the tract/county and two-party rules above for consistency.

### Data model impact

- Optional: tract-level party cache keyed by (state, year); per-tract fields such as `pctDem`, `pctRep`, `votesDem`, `votesRep`, `totalVotes`. District group party can be computed on demand from tract totals or cached per (state, step, …). Schema can be specified in the reference implementation or a future GDIP.

### Backward Compatibility

- Fully backward compatible; additive only.

## Reference Implementation

- [backend/services/tract-party-persistence.js](https://github.com/Lacoda-Labs/geodistricts/blob/main/backend/services/tract-party-persistence.js) — tract party once per (state, year); county→tract when needed.
- [backend/services/vest-data-loader.js](https://github.com/Lacoda-Labs/geodistricts/blob/main/backend/services/vest-data-loader.js) — `buildTractDataFromCountyVEST`, `allocateCountyVotesToTract` (two-party tract totals).
- [backend/index.js](https://github.com/Lacoda-Labs/geodistricts/blob/main/backend/index.js) — `computeDistrictPartyForStep`, `runDistrictPartyJob` (aggregate tract totals per group); GET district-party for any step.
- [doc/pages/STATE_ELECTION_DATA.md](https://github.com/Lacoda-Labs/geodistricts/blob/main/doc/pages/STATE_ELECTION_DATA.md), [STATE_DATA_SOURCES.md](https://github.com/Lacoda-Labs/geodistricts/blob/main/doc/pages/STATE_DATA_SOURCES.md).

## References

- [GDIP-002: Data Model](gdip-002-data-model.md)  
- [GDIP-004: Core Algorithm](gdip-004-core-algorithm.md)  
- [GDIP-005: Demographics for New Geodistricts](gdip-005-demographics.md) (optional)  
- [GDIP-006: Comparison Metrics](gdip-006-comparison-metrics.md) (optional; uses party for partisan balance comparison)
