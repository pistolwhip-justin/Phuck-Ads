# Phuck Ads — Project Plan

**Protocol baseline:** development-protocols-n-assets v1.1.7  
**Date:** 2026-11-19  
**Project board:** Not required; intentionally omitted.

## Scope

Android background service controlled exclusively by a Quick Settings tile. Detect qualifying advertisements in media apps, mute media volume, announce through Google TTS voice VII, and restore pre-mute media volume when ad classification ends.

## Exclusions

No conventional app screens/dashboard; no browser apps; no network inspection, VPN, DNS filtering, accessibility scraping, root, telemetry, or boot auto-start; no non-media stream changes; no project board; no license selection until authorized.

## Selected infrastructure

| Infrastructure | Selection |
|---|---|
| GitHub ProjectV2 board | NO |
| Resource planning | NO |
| Knowledge management / formal decision archive | NO |
| Formal phase-based development plan | YES |
| Research / data provenance subsystem | NO |
| Formal testing / validation plan | NO; global testing protocol applies |
| Deployment / release management | YES |
| Public documentation / knowledge vault | NO |
| Security / privacy document | NO; normal security practices apply |

## Phase 0 — Repository and requirements baseline

**Status:** Documentation initialized; implementation not started.

- [x] Repository exists and is accessible.
- [x] README adapted to profile style reference.
- [x] Requirements and architecture documented.
- [x] Project board intentionally omitted.
- [ ] Confirm exact Google TTS voice identifier meant by VII.
- [ ] User reviews initial behavior baseline.

Readiness gate: requirements are understood, open decisions are resolved, and the user approves moving into implementation.

## Phase 1 — Android foundation and Quick Settings control

Deliverables: minimal Android project and Gradle wrapper; app label Phuck Ads; Quick Settings tile with persistent state; Notification Access declaration/system permission handoff; no conventional app screens; GitHub Actions CI for build and unit tests.

Tests: tile appears; toggle persists across rebinding; permission handoff works; no dashboard or unnecessary screens.

Readiness gate:
- [ ] Clean CI build succeeds.
- [ ] Automated tests pass.
- [ ] Test APK is published via versioned GitHub Release when requested/produced.
- [ ] Tile and permission flow verified on physical device.
- [ ] README updated for delivered foundation.

## Phase 2 — Media session discovery and classification

Deliverables: active media-session enumeration/change monitoring; browser exclusion; playback-state and metadata inspection; classifier for <= 60-second media and exact Advertisement metadata.

Tests: 60,000 ms matches and 60,001 ms does not; unknown duration does not match duration rule; exact metadata matches across fields; trimmed/case-normalized exact match works; substring does not; browser sessions are excluded; paused/stopped sessions ignored; multiple sessions handled.

Readiness gate: automated tests pass, YouTube and at least one audio-streaming app are tested, browser exclusion is physically verified, and README is updated.

## Phase 3 — Mute, restore, and speech

Deliverables: capture pre-mute volume once; mute media stream only; restore when no eligible ad remains or service is disabled/stopped; Google TTS exact voice VII; one announcement per mute episode.

Tests: restore prior volume; repeated callbacks do not overwrite saved volume; disable/shutdown restores; multiple ad sessions do not restore prematurely; exact speech and voice verified; no silent voice fallback.

Readiness gate: state-machine tests pass, physical volume behavior and TTS voice are verified, README is updated.

## Phase 4 — Hardening and test release

Deliverables: lifecycle/recovery hardening, session reconciliation after reconnect, battery/performance observation, dedicated versioned release workflow, release notes, and protocol-compliant test APK.

Tests: repeated toggles; Notification Access revoke/regrant; session removed while muted; service/process recreation; reboot behavior documented; no media/notification telemetry; physical installation and update verification.

Readiness gate: Actions completed; build succeeded; tests passed; immutable release asset published; APK checksum and in-chat attachment verified; physical installation and device behavior verified or explicitly marked unverified; README/release notes complete.

## Initial risks and unknowns

1. Voice VII is ambiguous; exact Google TTS voice identifier must be confirmed.
2. Media apps may omit duration, fail to publish sessions, or expose different metadata.
3. Browser package identification and package visibility require validation.
4. Abrupt process death can interrupt restoration if captured volume is only in memory.
5. Android background execution and service lifecycle vary by OS version and OEM.
6. Proposed multiple-session rule: mute if any eligible playing non-browser session meets an ad condition; restore when none do.

## Next action

Confirm the exact Google TTS voice identifier meant by VII and review this behavior baseline before Phase 1 implementation.
