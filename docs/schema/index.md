# Schema Overview

Groupies Commons separates event data into two distinct layers: **canonical data** and **enrichment data**. They share a common identifier and are designed to be joined, but they serve different purposes and carry different reliability guarantees.

---

## Canonical layer

The canonical layer contains only observable, verifiable facts sourced directly from venue and festival websites. A field qualifies for canon if it can be read directly from the source without interpretation.

Canonical data is **stable, permanent, and append-only**. Events are never deleted. Once a `commons_id` is assigned, it does not change. Corrections are versioned, not silent overwrites.

The canonical feed is documented in full in the [Events schema reference](events.md).

---

## Enrichment layer

The enrichment layer contains derived, classified, and interpreted information. This includes matched artist identities, genre assignments, headliner inference, ticket availability, and confidence metadata. These fields require judgment and may be disputed or corrected over time.

The enrichment layer is documented separately. *(Coming soon.)*

---

## Joining the layers

Both layers share the `commons_id` field. A canonical event record and its enrichment record are always a 1:1 relationship on this key.

```
canonical[commons_id] ←→ enrichment[commons_id]
```

---

## Design principles

**Canon records what the venue said. Enrichment interprets what they meant.**

A venue listing "Nick Cave" as a performer is a canonical fact. Resolving that to a specific artist identity with a MusicBrainz ID, linking it to a genre, and inferring headliner status — those are enrichment decisions.

**Explicit uncertainty over assumption.** Fields may be `null` or `"unknown"` indefinitely. Canon does not backfill missing data with guesses.

**Source independence.** Canonical records remain valid even if the original source URL expires. The `event_url` is provenance, not a runtime dependency.

---

See the individual pages for field-level documentation:

- [Events](events.md)
- [Artists](artists.md)
- [Venues](venues.md)
