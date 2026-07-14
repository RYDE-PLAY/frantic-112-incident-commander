# Frantic #112 incident-commander delivery

## Summary

Published `ryde-play/incident-commander@sha-5623e2b1f9f3` as a runx skill for monitor-backed incident agency turns. The skill records a sealed review decision, enforces roster-matched approval, names `governed-outbound` for stakeholder communications, and refuses to mark delivery without a linked downstream send receipt.

## Artifacts

- Registry: https://runx.ai/x/ryde-play/incident-commander@sha-5623e2b1f9f3
- Source: https://github.com/RYDE-PLAY/runx/tree/f10a41dc04fe4a60462be48fcddee6268c01e8c0/skills/incident-commander
- PR: https://github.com/runxhq/runx/pull/319
- X.yaml: https://raw.githubusercontent.com/RYDE-PLAY/runx/f10a41dc04fe4a60462be48fcddee6268c01e8c0/skills/incident-commander/X.yaml
- SKILL.md: https://raw.githubusercontent.com/RYDE-PLAY/runx/f10a41dc04fe4a60462be48fcddee6268c01e8c0/skills/incident-commander/SKILL.md
- Dogfood receipt: `runx:receipt:sha256:0962f5586b010a4166174e37da6138076b43a50871fa0dd8b1d381dd15ae2251`

## Verification

- runx CLI: `runx-cli 0.6.14`
- Local harness: passed `status-update-approval-then-delivered` and `assign-missing-roster-owner-needs-agent`.
- Hosted harness: passed during registry publish.
- Registry read: captured in `registry-read.json`.
- Clean install: captured in `install.json`.
- Dogfood: ran the published registry package, not a local path.
- Plain verify: `runx verify --receipt runx-receipt.json --json` returned `valid=true` with production signature status valid.

## Rejection Lessons Applied

- The dogfood receipt is a post-publish skill run receipt, not a harness fixture receipt.
- The stop harness is a real `needs_agent` case, not a sealed success with a business status string.
- The incident state is grounded by `case_state.source_alert.ref=monitor:checkout:error-rate:2026-07-14T10:40Z`.
- The comms path names `governed-outbound` with channel, audience, principal, content digest, and source alert ref.
- Delivered status requires both roster-matched approval and `member_result.receipt_ref`.
- The receipt act is `form=review` and records the delivered decision and reason in the sealed summary.

## Install And Run

- Install: `runx add ryde-play/incident-commander@sha-5623e2b1f9f3 --registry https://api.runx.ai`
- Run: `runx skill ryde-play/incident-commander@sha-5623e2b1f9f3 --registry https://api.runx.ai ... --json`
- Verify: set the public key from `verification-key.json`, then run `runx verify --receipt runx-receipt.json --json`.
