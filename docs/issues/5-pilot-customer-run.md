# ACT-03 — Record a pilot customer workflow run (owner runbook)

Issue: #5 · Epic: #2 · Owner: Rajvir Dhillon · Size: S

This is an **owner-gated** task. "Used in a pilot customer's CI" can only be
satisfied by a real workflow run in a repository that is **not**
`conereplay-action`. The action repo's own self-test is not a customer. No agent
may invent a customer name or copy a customer's traces into this repo. This
document gives the owner everything needed to stand up a pilot run and attach the
evidence.

## What counts as acceptance

- [ ] A URL to a **successful** Actions run in a repository other than
      `gitdhillonai/conereplay-action`, where that repo consumes
      `gitdhillonai/conereplay-action@v1`.

## Pilot workflow the customer adds

Give the pilot customer this workflow to drop into their repo at
`.github/workflows/conereplay.yml`. They supply their own traces and modify file;
nothing from their repo is copied back here.

```yaml
name: conereplay
on:
  pull_request:
permissions:
  contents: read
  pull-requests: write
jobs:
  replay:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Replay traces
        uses: gitdhillonai/conereplay-action@v1
        with:
          traces: path/to/their/traces       # customer-owned
          modify: path/to/their/modify.json   # customer-owned
          fail-on: outcome-change:5%          # customer-chosen threshold
```

Notes for the owner to pass along:
- Pin to `@v1` (or a full SHA) so the pilot is reproducible.
- `pull-requests: write` is only needed if the customer wants the PR comment;
  without it the action still runs and the `comment-url` output stays empty
  (fork-safe behavior, already covered by the `fork-fallback` self-test job).
- The customer keeps their traces in their repo; do not send them here.

## Owner steps

1. Identify a real pilot repository (customer-owned or an owner-controlled
   non-`conereplay-action` repo used for the pilot). Do not name a customer who
   has not agreed.
2. Add the workflow above with the customer's real traces/modify paths.
3. Open a PR in that repo to trigger the `pull_request` run.
4. Confirm the run **succeeds** (or fails intentionally on the chosen threshold —
   acceptance here is "a successful run exists"; use `fail-on: never` for the
   first proof-of-adoption run if needed).
5. Copy the run URL, form:
   `https://github.com/<owner>/<repo>/actions/runs/<run-id>`.

## Attach evidence

Paste the run URL into this issue (#5) as a comment. That URL is the acceptance
artifact. Site issue #33's "used in a pilot customer's CI" line can then cite it.

## Verification checklist

- [ ] Run URL is in a repo whose name is not `conereplay-action`.
- [ ] The run used `gitdhillonai/conereplay-action@v1` (or a pinned SHA).
- [ ] The run concluded (success, or an intended threshold failure).
- [ ] No customer traces were committed into this repo.
