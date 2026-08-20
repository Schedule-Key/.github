# CLAUDE.md

## CRITICAL: Logging Hours On An Issue

Hours logged on an issue are what the customer is invoiced. Treat them as customer-facing.

**Precedence — read before touching any hours figure:**

1. **A human-stated figure wins.** If Brennan has stated the actual hours, that number is
   authoritative. **Never lower it** — not to match a measurement, not to match a reconcile
   report, not ever.
2. **No figure stated → measure it.** Measurement fills a gap; it never overwrites.
3. **Neither → `UNMEASURED`.** Say so on the issue and ask. Never guess. Never write zero.

```bash
python3 ../google-workspace-automation/internal/github-issue-billing/src/measure_hours.py \
  --repo .github --issue <number>
```

**Measurement is a floor, not a ceiling.** The script reads Claude Code session transcripts and
cannot see customer calls, email threads, walkthroughs or manual verification. On an issue with
customer contact the real number is routinely double what it reports. So `logged > measured` is
normal and is **not** an error. Never reconcile a figure downward. Record the invisible half:

```
- 2026-08-06 - 1.75 hrs - what was done that day
- 2026-08-06 - 1.00 hrs [non-billable] - our own defect
- 2026-08-06 - 2.00 hrs [comms] - customer call + email thread
```

`[comms]` is provenance only and does **not** change billability — it is unrelated to
`[non-billable]`, and both tags may sit on one line. One line per **day worked**.

- **Never collapse a whole issue onto one date.** A whole-issue total stamped on the close date
  is what made one client's month read 158.5 h against ~91 h quoted to them — and it looks
  perfectly healthy, because the hours get rescued from `Total Actual` and export cleanly.
- **Never rewrite a Time Log for a period already invoiced or quoted to the customer.** Correct
  it forward and say what changed.
- **`Billable:` takes `Yes` or `No` and nothing else.** Put reasoning, judgement calls and
  "confirm with Brennan" in a `### Billable rationale` section below it. A sentence in the field
  reads as unresolved and **silently blocks the issue from being invoiced**.

Full standard: `internal/github-issue-billing/ISSUE-HOURS-STANDARD.md` in
`google-workspace-automation`.
