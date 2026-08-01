# Sample n8n Memory Portability Audit

This is a redacted example of the written handoff delivered by the service. It
uses fictional values and is not a customer report.

## Executive summary

The workflow stores useful routing decisions in execution history, but the
current payload mixes durable decisions with transient headers and contact
data. A small allow-list can preserve the decisions while removing secrets and
unrelated fields before they become portable memory.

**Recommendation:** export only the selected decision fields, attach a stable
source reference, and validate the resulting OKF bundle with three golden
questions before publishing it.

## Findings

| ID | Finding | Impact | Recommendation |
| --- | --- | --- | --- |
| F-01 | Execution payload contains transient headers | High | Exclude headers and authorization-shaped keys from the mapping. |
| F-02 | Memory fields have no explicit retention boundary | Medium | Add a retention policy and review date to the handoff. |
| F-03 | Mapping depends on display labels | Medium | Use stable node IDs and deterministic source-derived record IDs. |

## Proposed allow-list

```yaml
source: n8n
include:
  - node: "lead-score"
    fields: [score, route, reason]
  - node: "follow-up"
    fields: [next_action, due_at]
exclude:
  - headers
  - cookies
  - authorization
  - raw_email_body
```

## Acceptance checklist

- [ ] No API keys, cookies, bearer tokens, or private customer data in the
  redacted fixture.
- [ ] Every exported record has a deterministic source reference.
- [ ] The OKF bundle imports without skipped rows.
- [ ] Three golden questions return the same decision facts before and after
  the round trip.
- [ ] The written handoff includes commands, assumptions, and rollback notes.

The paid audit applies this method to one redacted workflow and delivers the
decision map, privacy review, validation evidence, and follow-up notes in three
business days.
