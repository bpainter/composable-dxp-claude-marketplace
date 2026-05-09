---
description: Guided API-key rotation runbook — inventory, create new, deploy, verify, retire old. For staff changes, suspected leaks, scheduled quarterly rotation, or post-incident.

# Project context
type: command
project: skills-library
plugin: algolia
tags: [type/command, plugin/algolia]
status: active
---

Use the `algolia-agent` (delegating to `algolia-api-keys-security`) to rotate Algolia API keys with zero downtime.

Triggers:
- Staff offboarding — rotate any key the leaver had access to.
- Suspected leak (key in CI logs, public repo, screenshot, error tracker).
- Scheduled quarterly rotation for non-critical scoped keys.
- Post-incident rotation after a 403 spike or unusual usage pattern.
- Compliance / audit requirement.

Pre-flight: a working admin key, the engagement's key inventory doc, and the engagement's secret store (1Password vault, GitHub Actions secrets, Vercel env config).

Run the seven stages, per app and per environment.

1. **Inventory** — list all keys per app:
   ```
   algolia --profile {env} keys list
   ```
   Cross-reference against the engagement's key inventory doc. Surface:
   - Keys in use by which workload (indexer, settings deploy, browser, MCP)
   - Where each is stored (1Password vault path, GitHub secret name, Vercel env name)
   - Last rotation date
   - Description (warn if missing)

2. **Scope the rotation** — decide which keys rotate now:
   - **All keys** for: admin compromise, suspected leak of the search-only/browser key, regulatory ask.
   - **Targeted keys** for: a single workload's leak (rotate just that key), staff offboarding (rotate keys the person held).
   - Don't rotate keys you don't have a deployment plan for. Half-rotation is worse than no rotation.

3. **Create the new key** — for each key in scope:
   ```
   algolia --profile {env} keys add --acl {acls} --description "{purpose} | rotated {date}" [--indices {pattern}]
   ```
   Match the prior key's ACLs, index restrictions, IP allowlist, rate limits, and `validUntil`. Don't broaden scope during rotation; that's a separate decision.

4. **Deploy the new key** — to wherever it's used:
   - **Vercel runtime env** — update via Vercel Dashboard or `vercel env add`; redeploy.
   - **GitHub Actions secret** — update via repo Settings → Secrets.
   - **1Password / engagement secret store** — update the entry; communicate the rotation.
   - **Local developer envs** — communicate via the engagement's secure channel (don't paste into Slack DMs without ephemeral disposal).
   - **MCP configs** — update each engineer's `claude_desktop_config.json` or equivalent.

5. **Verify the new key works** — for each deployment, run a synthetic test:
   - Indexer key: run a `partialUpdateObject` against a canary record on staging, verify the update.
   - Settings deploy key: re-run the settings deploy on staging.
   - Browser/search-only key: hit the SERP and confirm hits return.
   - MCP key: open an MCP session and run a read query.

6. **Watch for 403s** — for the next hour, watch Vercel logs / observability for 403 spikes. Means a deployment was missed and is still using the old key.

7. **Retire the old key** — only after stages 4–6 are clean:
   ```
   algolia --profile {env} keys delete {old-keyID} --confirm
   ```
   Or, for safer phased retirement, set `validUntil` to ~24h out and watch for stragglers; delete after the window.

8. **Update the rotation log** — append to `Runbooks/algolia-keys-rotation-{date}.md`:
   - Trigger
   - Keys rotated (purpose + index restriction + ACL set)
   - Deployments updated
   - Verification results
   - Old key deletion timestamp
   - Inventory doc updated (next rotation date for each)

Don't rotate by overwriting the same key. Always create-deploy-verify-retire — overwriting causes downtime windows and races.

Don't rotate in parallel across environments without coordination. Stagger: dev → staging → prod, with verification at each step.

Don't delete a key in the same minute you deployed its replacement. The propagation window can be a few seconds; give it a coffee.

Don't rotate during high-traffic windows for browser-facing keys. The verification window is hours; pick a quiet time.

Don't skip the rotation log. The whole point of rotation is auditable provenance — without the log, you can't prove you rotated.

If the rotation is for a suspected leak, also:
- File an incident note (engagement's incident log).
- Audit Algolia's Logs API for unusual activity from the leaked key during its valid window.
- Notify the engagement's security stakeholder if applicable.
