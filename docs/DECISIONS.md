# Phuck Ads — Decisions and Open Questions

**Date:** 2026-11-19

This is the current decision record, not a full historical chat archive.

## D-001 — Quick Settings tile is the only app control

**Status:** Accepted  
**Decision:** Enable/disable the background service from a Quick Settings tile. No conventional app screens or dashboard. Android Settings may be required for Notification Access.

## D-002 — Browser apps are excluded

**Status:** Accepted  
**Decision:** Browser-owned media sessions never trigger muting. Dedicated media apps such as YouTube and audio-streaming services are in scope.

## D-003 — Advertisement classification is duration OR metadata

**Status:** Accepted  
**Decision:** A playing eligible track is an ad if its known total duration is <= 60,000 ms, or any string-valued metadata field has the exact value Advertisement. Match case-insensitively after trimming whitespace; no substring matching.

## D-004 — Restore captured media volume

**Status:** Accepted  
**Decision:** Capture STREAM_MUSIC volume once before muting, then restore that exact value when no eligible ad session remains or monitoring is disabled/stopped. Never recapture zero during repeated callbacks.

## D-005 — Google TTS voice VII

**Status:** Open / blocking clarification  
**Decision requested:** Use the Google TTS engine and voice VII for the exact spoken sentence.  
**Unknown:** Is VII the voice's displayed name, an engine voice identifier, a voice number in a particular UI, or another identifier?  
**Constraint:** Do not silently select a different voice. Query installed Google TTS voices and bind to the confirmed ID/name.

## D-006 — Multiple simultaneous media sessions

**Status:** Proposed default, awaiting review  
**Decision:** If any eligible playing non-browser session meets either ad condition, mute the shared media stream. Restore only when no eligible session remains classified as an ad.  
**Rationale:** Media volume is shared across sessions; this avoids restoring music while another eligible session remains classified as an ad.

## D-007 — No project board

**Status:** Accepted  
**Decision:** Do not create a GitHub ProjectV2 board.

## D-008 — No license yet

**Status:** Accepted  
**Decision:** Do not add a license file or claim a license until authorized.
