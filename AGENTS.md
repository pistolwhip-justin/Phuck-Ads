# AI Agent Instructions — Phuck Ads

## Required reading

At the start of every development session:
1. Read the current global protocols: https://github.com/pistolwhip-justin/development-protocols-n-assets/blob/main/DEVELOPMENT_PROTOCOLS.md
2. Read this repository's README and relevant documents under docs/.
3. Check repository state, changes, Actions runs, releases, and active phase.
4. Follow global protocols; do not rely on stale copied instructions.

## Project identity and scope

- Repository: pistolwhip-justin/Phuck-Ads.
- Android app label: Phuck Ads.
- Package ID is not yet selected; do not invent it without documenting the decision.
- No conventional app screens or dashboard. Android Settings may be used to grant Notification Access.
- Quick Settings tile is the sole app control.
- Browser applications are excluded.
- Do not add features, permissions, network access, root requirements, or boot behavior outside the documented requirements.

## Authoritative documents

- docs/REQUIREMENTS.md — functional requirements.
- docs/ARCHITECTURE.md — technical direction and platform constraints.
- docs/PROJECT_PLAN.md — phases and readiness gates.
- docs/DECISIONS.md — decisions and unresolved questions.

Update README and affected documents in the same phase as implementation.

## Functional invariants

1. The service operates only while its Quick Settings tile is enabled.
2. Only actively playing, eligible non-browser media sessions are considered.
3. Mute when duration is known and <= 60,000 ms, OR any string metadata field has the exact value Advertisement.
4. Metadata matching is case-insensitive after trimming whitespace; do not use substring matching.
5. Browser apps are always excluded.
6. Capture media volume before the first mute and restore that captured value when no eligible ad remains.
7. Do not overwrite the captured volume during repeated metadata updates.
8. Do not silently substitute a different TTS voice for VII.
9. Spoken sentence must remain exactly: Ads are fucking annoying, bullshit is muted until the music returns.

## Reliability and privacy

- Make volume restoration idempotent; cover disable, shutdown, session loss, and metadata transitions.
- If disabled while muted, restore captured volume before stopping.
- Preserve the original volume only once per mute episode.
- Handle missing metadata, unknown duration, paused/stopped sessions, multiple sessions, and browser identification explicitly.
- Never log or upload track titles, metadata, notification contents, or personal media details.
- Use Notification Access only for media-session access.
- No Accessibility Service, VPN, DNS interception, root, network inspection, analytics, or telemetry.

## Development and release

- Use small, testable phases and explicit readiness gates.
- Verify platform API behavior against current Android documentation.
- Test classification, browser exclusion, volume transitions, and lifecycle recovery.
- Update README and project docs with every phase.
- Use GitHub Actions for reproducible Android builds.
- Each versioned APK release gets its own dedicated workflow and immutable GitHub Release, following Section 7.1 of the global protocols.
- Never commit APK binaries to source control.
- Report workflow, build, tests, release, installation, and device behavior as separate verification states.
- Do not claim APK delivery unless the protocol's release, checksum, and in-chat attachment requirements are met.

## Explicit exclusions

No project board is required or authorized. Do not create one.
No license has been selected. Do not add a license file.
No auto-start-on-boot requirement is approved.
No dashboard, configuration screen, analytics, telemetry, or remote service is authorized.
