# VibeNest assessment: not ready for deployment

As of **27 October 2026**, the assessed configuration-only variant is closed
without deployment acceptance. This is a decision about a specific runtime and
hosting path, not a permanent rejection of rustypaste. This fork contains a
documentation assessment; it does not contain an accepted VibeNest runtime patch.

## Assessed source and image

- Fork: https://github.com/NikitaBabenko/rustypaste
- Preserved upstream master: `35d1c5dbbf3bb751d65ee1a5a382a6d2200dba91`.
- Official runtime: `orhunp/rustypaste:0.18.1`, release source
  `71d0b5d001fb2ec041d95ca75b4eff18293e9f10`.
- Immutable image index:
  `sha256:5c1692cc1fa2c9722f39d46e54455ab8f71ec86f5da9639769f8bd92fc6b882a`.
- Linux/amd64 manifest:
  `sha256:2c13b125034f557ca86398d40d5f381e9400ff33bdcec04bb8b788aa7fffd65c`.

The release image is not built from current master. Its OCI labels and published
BuildKit provenance identify the release commit; this assessment did not perform
an independent rebuild or cryptographic signature verification. The image was
inspected as data and was not executed.

## Why this variant was not accepted

The intended service is authenticated sharing of small files and logs. The
release configuration has no supported switch to disable remote-URL retrieval
or URL-shortening multipart variants while keeping ordinary uploads. Its body
limit and timeout settings also do not establish all the incoming-request bounds
required by this pilot. A configuration file cannot change these compiled paths.

The candidate packaging would copy a config and a writable-directory seed onto
the pinned official image, with no compilation or `RUN` instruction. That path
cannot apply application fixes. A reviewed replacement runtime would need a
separately authorized build and acceptance process. No new proxy/parser,
platform change, security-policy change or build workaround is included here.

This is a static compatibility decision. No successful exploit, production
safety hold or rejection by the VibeNest deployment gate is claimed.

## Configuration work identified

A future accepted recipe must bind `0.0.0.0:8000`, use one worker, and preserve
`/app/upload` with UID/GID `1000:1000`. Fresh volume ownership and existing-volume
reuse must both be tested. Do not solve permissions with `chmod 777`.

Use `server.hardening = true` in the **server** table and serve uploaded content
as plain text with security headers. Restrict incoming bytes, duration, storage
and expiry with controls that are actually enforced. The release's default
expiry is not a maximum lifetime; its directory-size check is not a hard disk
quota. These distinctions must remain visible in any future hosting instructions.

Uploads must require a per-installation secret. Upstream treats absent or blank
auth tokens as disabled authentication. A future Compose adapter can map a
VibeNest-generated internal `AUTH_SECRET` to `AUTH_TOKEN`, require a nonempty
value before launch, and configure deletion authentication explicitly. No shared
sample credential is supplied here. Secret persistence, blank/whitespace inputs,
list/delete behavior and absence from image history still require verification.

## Remaining acceptance

No account, project or hardware was created for this assessment. No build,
deployment, restart, upload, load test or persistence test ran. No Free-tier
memory, CPU, disk or uptime measurement is available. GitHub Actions is disabled
on this fork; upstream workflow files and tests remain intact.

Before enabling a deployment badge, a safe runtime must pass the normal path:
badge → anonymous preview → registration/sign-in preserving the repository →
configuration → gated deployment → public HTTPS. Test clean configuration and
reuse separately, then authenticated upload, read, list/delete authorization,
expiry and persistence after ordinary container replacement. Verify applied
resource limits, actual disk-quota behavior and secret handling. Any manual
setup steps must be documented as guided deployment, not unattended one-click.

The intended badge markup is retained here as **inactive documentation only**:

```markdown
[![Deploy to VibeNest](https://vibenest.net/deploy-to-vibenest.svg)](https://vibenest.net/deploy?repo=https://github.com/NikitaBabenko/rustypaste)
```

There is no verified demo URL or deployment success claim. Upstream MIT license,
source, fixtures and examples are preserved. Maintainer outreach has not been sent.
