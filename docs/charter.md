# Groupies Commons — Charter

*Version 1.0 — May 2026*

---

## What Groupies Commons is

Groupies Commons is the public data layer of the Groupies project. It is a canonical, provenance-sourced archive of live music events in the Netherlands — openly licensed, permanently accessible, and maintained as a cultural public good.

The archive covers concerts, festivals, and live performances that took place in the Netherlands. Coverage extends from 1953 to the present. New events are published after they occur, once the record is settled.

Groupies Commons is not the Groupies operational system. The scraping infrastructure, moderation tooling, matching pipelines, and internal dashboards remain proprietary. The commons is the output of that system — the data, the schema, and the documentation — made public.

---

## License

Groupies Commons data is published under the **Open Database License (ODbL) 1.0**.

This means:

- You are free to use, share, and adapt the data for any purpose, including commercial use
- You must attribute Groupies Commons when you use or republish the data
- If you produce a derived database, you must publish it under the same ODbL license
- The underlying facts (event dates, artist names, venue names) are not copyrightable and are not restricted by this license; the license covers the compiled database as a whole

Full license text: [https://opendatacommons.org/licenses/odbl/1-0/](https://opendatacommons.org/licenses/odbl/1-0/)

---

## Identifier contract

Every published record carries a stable public identifier: `commons_id`, formatted as `GC-{integer}` (e.g. `GC-00042381`).

The contract:

- A `commons_id` is assigned once, at the time of first publication
- It is permanent and never reassigned
- If a record is corrected, the correction is logged with a timestamp — the record is not silently overwritten
- The `commons_id` is independent of any internal system identifier

Records are never deleted except under a formal takedown request (see below).

---

## Publication model

Events are published after they occur. The minimum window between an event's date and its first appearance in the public archive is 24 hours. This ensures that cancellations, corrections, and post-event data quality checks are resolved before publication.

Groupies Commons does not publish future events. The collection of future event data is done internally for preservation purposes only.

---

## Geographic scope

Groupies Commons covers **live music events in the Netherlands**. Scope is defined by where the event took place, not the nationality of the artists performing.

The current archive is the most comprehensive public record of live music in the Netherlands. Coverage outside the Netherlands is out of scope for v1.

---

## Takedown policy

Groupies Commons publishes factual records of public events. In most cases these records cannot be removed.

Exceptions:

- **Individual artists** (natural persons) may request removal of their name from the archive. Requests are reviewed case by case under GDPR legitimate interest grounds. Reasonable requests from living individuals will be honoured.
- **Venues and festivals** (commercial entities) do not have a general right to removal.
- **Factual errors** can be corrected via the corrections process (see Contributing). Corrections are versioned, not silent overwrites.

To submit a request: [contact TBD]

---

## What Groupies Commons is not

- It is not the Groupies platform or its operational tooling
- It is not a live event feed or discovery service
- It is not a commercial product
- It is not a Wikipedia-style open editing platform (corrections are accepted but curated)

---

## Governance

Groupies Commons is published and maintained by **Tatemae**. Editorial decisions about schema, scope, and data quality are made by Tatemae.

The project is open to:

- Corrections and data quality reports from the public
- Schema feedback from researchers and developers
- Institutional partnerships for archive access and enrichment

Community contributions do not grant editorial control. The archive is curated, not crowd-sourced.

---

## Versioning

The schema and export format are versioned. Breaking changes require a new major version. Additive changes (new fields) are minor versions. The current version is documented in the schema reference.
