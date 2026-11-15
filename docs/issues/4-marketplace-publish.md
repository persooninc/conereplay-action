# ACT-02 — Publish the action on the GitHub Marketplace (owner runbook)

Issue: #4 · Epic: #2 · Owner: Rajvir Dhillon · Size: S

This is an **owner-gated** task. Publishing a GitHub Marketplace listing requires
accepting the Marketplace Developer Agreement under the owning account and can
only be completed by the repository owner. No agent session may create the
listing or add an adoption claim. This document gives the owner the exact steps
to publish and the exact one-line code change to make afterward.

## Preconditions (verify before publishing)

- [ ] The action is public and consumable as `gitdhillonai/conereplay-action@v1`
      (tag `v1` currently points at `41cdb55`).
- [ ] `action.yml` has `name`, `description`, and `branding` (icon + color) set —
      Marketplace requires all three. Verified current values on `main`:
  - `name: ConeReplay Regression Report` (must be unique across Marketplace; it
    is a specific name and unlikely to collide — if GitHub reports it taken, pick
    another unique display name such as `ConeReplay Trace Replay`).
  - `description: Replay recorded agent traces against a proposed change and post
    the divergence report as a PR comment.`
  - `branding.icon: git-branch`, `branding.color: purple` (both present).
- [ ] CI is green on `main` (lint job runs actionlint + shellcheck + gitleaks;
      tests, ubuntu, threshold, replay, and fork-fallback jobs all pass).
- [ ] A release/tag exists to publish from — annotated tag `v1` dereferences to
      commit `41cdb55`.

All three required `action.yml` fields are already present on `main`, so no
pre-publish edit is needed. (For reference, if `branding` were ever missing it
would be added as e.g. `branding:\n  icon: git-branch\n  color: purple`.)

## Owner publishing steps (GitHub web UI — owner account only)

1. Sign in as the owner of `gitdhillonai/conereplay-action`.
2. Go to the repository → **Releases** → open the release tagged `v1`
   (or create a release from the `v1` tag).
3. On the release page, check **"Publish this Action to the GitHub Marketplace"**.
4. Accept the **GitHub Marketplace Developer Agreement** when prompted
   (one-time, per account).
5. Choose **one** primary category (suggested: *Continuous Integration*) and an
   optional secondary category (*Testing*).
6. GitHub validates `action.yml` (name uniqueness, description, branding). Fix
   any reported error and re-check the publish box.
7. Confirm the listing copy is taken **verbatim from the existing README**.
   Do **not** add customer counts, certifications, "trusted by", or any adoption
   claim. Allowed: the README's feature description only.
8. Click **Publish release**. Copy the resulting Marketplace listing URL
   (form: `https://github.com/marketplace/actions/<slug>`).

## After publishing — the one code change

Edit `README.md` line 81, replacing the "not listed" sentence with the URL:

```diff
-This repository is not listed on the GitHub Marketplace.
+This action is published on the GitHub Marketplace: https://github.com/marketplace/actions/<slug>
```

Commit on a branch and open a PR (do not push to `main` directly).

## Verification checklist

- [ ] Visiting the Marketplace URL shows the ConeReplay listing, public.
- [ ] Listing description matches README; no adoption/customer claims present.
- [ ] `README.md` line 81 shows the Marketplace URL, not "not listed".
- [ ] Site issue #33's "Action published to the marketplace" line can be checked
      with this URL as evidence.

## Closing

Once the URL is live and the README is updated, this issue is satisfied. The
listing URL is the only acceptance evidence an agent may not fabricate.
