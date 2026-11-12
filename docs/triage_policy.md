# Sprint Triage Policy

Every planning cycle the customer-success intake queue is reviewed against the
team registry and the current issue tracker.

## Routing

1. Look up the request `component` in `config/teams.json`. A component may be
   owned by exactly one team.
2. Requests whose component has no owning team are deferred with reason
   `no_owning_team` and must not create an issue.

## Duplicate handling

A request is already tracked when an **open** issue body contains the line
`Request-ID: <request_id>`. Closed issues do not count as tracked: if the
request qualifies again, raise a fresh issue.

Requests that are already tracked are skipped and reported under
`already_tracked`; do not create a second issue.

## Acceptance

A request that has an owning team and is not already tracked becomes sprint
work when **any** of the following holds:

- `severity` is `critical`
- `votes` is greater than or equal to 20
- `customer_tier` is `enterprise` and `votes` is greater than or equal to 10

All other requests are deferred with reason `below_threshold`.

## Issue format

Accepted requests become GitHub issues on the default branch with:

- Title: `[<request_id>] <title>`
- Body lines: `Request-ID:`, `Customer:`, `Component:`, `Severity:`,
  `Customer-Tier:`, `Votes:`, `Region:`, `Submitted:`, followed by a blank line
  and the request description.
- Labels: `triage/accepted`, `severity/<severity>`, and the owning team label
  listed in `config/teams.json`.

## Sprint log

The triage run records the outcome in `triage/last_run.json`:

```json
{
  "run_at": "<timestamp>",
  "accepted": ["<request_id>", ...],
  "already_tracked": ["<request_id>", ...],
  "deferred": ["<request_id>", ...]
}
```
