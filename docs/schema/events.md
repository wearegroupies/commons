# Events Schema

Full field reference for canonical event records. The canonical layer contains 19 fields — all observable facts, no interpretation.

---

## Identifiers

### `commons_id`

Type: `string` — Required

The stable public identifier for a canonical event. Format: `GC-{integer}` (e.g. `GC-00042381`).

Assigned once at publication time. Permanent. Never reassigned. If a record is corrected, the correction is versioned — the `commons_id` does not change.

### `source_event_id`

Type: `string | null` — Optional

The identifier the original source (venue or festival website) assigned to this event, if one is exposed. Preserved verbatim. Never fabricated — if the source does not provide one, this field is `null`.

### `source`

Type: `string` — Required

Identifies which venue or festival feed this event originated from (e.g. `"melkweg"`, `"paradiso"`). Observable origin, not interpreted.

---

## Venue

### `venue_id`

Type: `string` — Required

Canonical identifier for the venue in the Groupies registry. Stable slug format (e.g. `"melkweg"`, `"tivolivredenburg"`).

### `venue_name`

Type: `string` — Required

The official name of the venue as verified against the source. Always identifies the *programming* venue — the organisation that listed the event.

### `city`

Type: `string` — Required

The city where the event takes place. Geographic fact.

### `room`

Type: `string | null` — Optional

A named space within the same physical building as the venue (e.g. `"Grote Zaal"`, `"Ronda"`). The venue and room share the same address. `null` when not applicable.

### `location`

Type: `string | null` — Optional

The name of an external venue at a different physical address where the event takes place, when different from the programming venue's own address. For example: Paradiso programmes an event at Tolhuistuin — `venue_name` is `"Paradiso"`, `location` is `"Tolhuistuin"`.

`null` when the event takes place at the venue's own address.

!!! note "room vs location"
    `room` = same building, different space. `location` = different building, different address. Both can coexist but this is rare.

---

## Event details

### `title`

Type: `string` — Required

The event title as provided by the source, after Title Case normalisation. Never fabricated.

### `subtitle`

Type: `string | null` — Optional

Secondary title if provided by the source. `null` when absent.

### `date`

Type: `string (YYYY-MM-DD)` — Required

The event date, or start date for multi-day events. ISO 8601 format.

### `date_end`

Type: `string (YYYY-MM-DD) | null` — Optional

End date for multi-day events. Must be `>= date`. `null` for single-day events.

### `event_url`

Type: `string (URL)` — Required

Link to the original event listing on the source website. Recorded as provenance — canonical records remain valid even if this URL later expires.

### `performers`

Type: `string[]` — Optional

Artist names as listed on the source, or canonical names from the matching pipeline. May be an empty array `[]` when the source provides no performer information.

### `performers_source`

Type: `string | null` — Optional

Tracks how the `performers` array was populated.

| Value | Meaning |
|-------|---------|
| `"scraper"` | Raw text from the venue website. Names are unverified. |
| `"matched"` | Verified through the artist matching pipeline. Names are canonical. |
| `null` | No performer data. `performers` is `[]`. |

---

## Status

### `status`

Type: `string` — Required

The planned state of the event.

| Value | Meaning |
|-------|---------|
| `scheduled` | Event is or was planned to occur. Default. |
| `cancelled` | Event will not or did not occur. Source confirmed. |
| `postponed` | Event delayed, new date not yet known. |
| `rescheduled` | Event was postponed and has a confirmed new date. The original entry keeps this status; the new date creates a new record. |
| `banned` | Event was prohibited by an authority. |
| `unknown` | Status cannot be determined from available sources. |
| `removed` | Event no longer appears in the source feed. May indicate it was delisted, sold out, or removed. |

### `occurred`

Type: `boolean | null` — Required

Whether the event verifiably took place. Applies to past events only.

| Value | Meaning |
|-------|---------|
| `true` | Event verifiably occurred. Evidence exists (venue confirmation, published photos, independent reviews). |
| `false` | Event verifiably did not occur. |
| `null` | Occurrence cannot be confirmed. Default for all future events, and for past events without evidence. **Preferred over guessing.** |

`occurred: null` is valid and expected. Canon does not infer occurrence from the date passing or the absence of a cancellation notice.

**Valid status / occurred combinations:**

| Status | occurred | Valid |
|--------|----------|-------|
| `scheduled` | `null` | ✓ Future event, or past event unverified |
| `scheduled` | `true` | ✓ Past event, verified occurred |
| `scheduled` | `false` | ✗ Contradiction |
| `cancelled` | `false` | ✓ Consistent |
| `cancelled` | `true` | ✗ Contradiction |
| `cancelled` | `null` | ✓ Cancelled, occurrence not verified |
| `postponed` | `null` | ✓ Normal |
| `rescheduled` | `null` | ✓ Original entry, new event exists elsewhere |

---

## Provenance

### `scraped_at`

Type: `string (ISO 8601)` — Required

Timestamp of the most recent data capture from the source. Updated on each scrape.

### `first_seen_at`

Type: `string (ISO 8601)` — Required

Timestamp of the initial data capture. Immutable after creation.

---

## Archival guarantees

1. **Permanence.** Events are never deleted. Past events remain accessible indefinitely.
2. **ID stability.** `commons_id` does not change for the lifetime of a record, regardless of title corrections, status updates, or source changes.
3. **Provenance.** Every record retains `first_seen_at` (immutable) and `scraped_at` (updated), providing full temporal provenance.
4. **Uncertainty preservation.** Fields may be `null` or `"unknown"` indefinitely. Canon does not backfill assumptions.
5. **No retroactive inference.** Past events are not enriched with guessed data. `occurred: null` may persist permanently if the event cannot be verified.
6. **Append-only semantics.** New events are added; existing events are updated in place. No bulk deletions or rewrites.
7. **Source independence.** Canon remains valid even if original source URLs expire.

---

## Example record

```json
{
  "commons_id": "GC-00012847",
  "source_event_id": "18423",
  "source": "paradiso",
  "venue_id": "paradiso",
  "venue_name": "Paradiso",
  "city": "Amsterdam",
  "room": "Grote Zaal",
  "location": null,
  "title": "Nick Cave & the Bad Seeds",
  "subtitle": null,
  "date": "1996-11-14",
  "date_end": null,
  "event_url": "https://www.paradiso.nl/agenda/18423",
  "performers": ["Nick Cave & the Bad Seeds"],
  "performers_source": "scraper",
  "status": "scheduled",
  "occurred": true,
  "scraped_at": "2026-01-15T03:00:00Z",
  "first_seen_at": "2026-01-15T03:00:00Z"
}
```
