# Phuck Ads — Requirements

**Status:** Initial requirements baseline  
**Date:** 2026-11-19

## Purpose

An Android background service that detects advertisement-like tracks in eligible media apps, temporarily mutes media audio, announces the mute through Google TTS, and restores the previous media volume when normal content resumes.

## User control

- One Quick Settings tile enables/disables the service.
- No conventional app screens or dashboard.
- Tile state must reflect the actual enabled/disabled state.
- If Notification Access is missing, the app may direct the user to Android's system permission settings. No custom settings UI is authorized.

## Eligible media

- Consider active Android media sessions from dedicated media apps, including YouTube and audio-streaming services.
- A session is eligible only while playback state is PLAYING.
- Browser apps must be excluded, even if they publish media sessions.
- Do not scrape notifications or screen contents.
- Apps that do not publish a usable Android media session/metadata may not be detectable.

## Advertisement classification

Mute an eligible playing session if either condition is true:

1. Its known total media duration is 60,000 milliseconds or less; OR
2. Any string-valued field in its available media metadata has the exact value Advertisement.

For metadata comparison, trim surrounding whitespace and compare case-insensitively. This is an exact-value match, not substring matching. The title field is included but is not the only field inspected.

Duration means total media-item duration, not elapsed playback position. Missing, negative, unknown, or infinite duration does not satisfy the short-track condition. The metadata condition can still match when duration is unknown.

## Audio behavior

- Control Android media/music volume only.
- On the first transition into muted state, capture the current media volume index.
- Set media volume to zero while an eligible advertisement is playing.
- Do not replace the captured value during subsequent ad callbacks.
- Restore the captured value when no eligible playing session meets either ad condition.
- Restore the captured value when the user disables the tile, the service stops, or the monitored session disappears.
- Restoration must be safe to run more than once.
- Do not modify ring, notification, alarm, call, or system volume.

## Spoken announcement

Speak exactly: **Ads are fucking annoying, bullshit is muted until the music returns**

Use Android TextToSpeech with the Google text-to-speech engine and requested voice **VII**.

The exact installed Google TTS voice identifier corresponding to VII is not confirmed. Query available engine voices and select only the confirmed matching voice. Do not silently fall back to another voice. If unavailable, record the condition and preserve mute/restore behavior without claiming speech acceptance passed.

Speak once when a mute episode begins; do not repeat for every metadata callback.

## Permissions and privacy

- Notification Access is required to enumerate active media sessions through MediaSessionManager.
- Use it only for media-session state and metadata needed by this feature.
- Do not read, store, transmit, or log notification contents.
- No network access, analytics, telemetry, Accessibility Service, VPN, DNS interception, or root requirements.

## Compatibility

Android only. Minimum/target SDK will be selected during the foundation phase after reviewing current platform constraints. Android system-visible app label must be Phuck Ads from the first APK.

## Acceptance criteria

- [ ] Quick Settings tile can be added and toggles the service.
- [ ] Tile state survives service/process recreation.
- [ ] No custom app screens.
- [ ] Notification Access is required and used only for media sessions.
- [ ] Playing non-browser track <= 60 seconds triggers mute.
- [ ] Any metadata string exactly equal to Advertisement triggers mute, including when duration is longer/unknown.
- [ ] Substring values such as Advertisement break do not match.
- [ ] Browser sessions never trigger mute.
- [ ] Paused/stopped sessions do not trigger mute.
- [ ] Prior media volume is restored when ad classification ends.
- [ ] Prior volume is restored on disable and service/session shutdown.
- [ ] Exact announcement uses confirmed Google TTS voice VII.
- [ ] No other audio streams are modified.
- [ ] Automated tests pass and physical-device behavior is verified.
