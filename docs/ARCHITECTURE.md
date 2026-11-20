# Phuck Ads — Architecture

**Status:** Proposed; implementation not started  
**Date:** 2026-11-19

## Components

### Quick Settings tile

Use Android TileService as the only app control surface. It toggles a persisted enabled flag and starts/stops monitoring. Tile state must be reconstructed from persisted state, not from a long-lived tile-service instance. Android documents that TileService can be unbound between lifecycle callbacks.

References: [TileService](https://developer.android.com/reference/android/service/quicksettings/TileService), [Quick Settings tiles guide](https://developer.android.com/develop/ui/views/quicksettings-tiles).

### Media-session access

Use a narrowly scoped NotificationListenerService as the authorization bridge for MediaSessionManager.getActiveSessions() and active-session change callbacks. The user must grant Notification Access in Android Settings.

References: [MediaSessionManager](https://developer.android.com/reference/android/media/session/MediaSessionManager), [NotificationListenerService](https://developer.android.com/reference/android/service/notification/NotificationListenerService).

### Classification

For each active controller:
1. Identify its owning package.
2. Exclude browser packages before inspecting metadata.
3. Require playback state PLAYING.
4. Read MediaMetadata.
5. Evaluate known duration against 60,000 ms.
6. Inspect available string-valued metadata fields for exact Advertisement value.

References: [MediaMetadata](https://developer.android.com/reference/android/media/MediaMetadata).

### Volume

Use AudioManager to read, mute, and restore STREAM_MUSIC only. Keep one captured pre-mute volume value per mute episode. A state machine must prevent repeated callbacks from overwriting it.

Reference: [AudioManager](https://developer.android.com/reference/android/media/AudioManager).

### Speech

Use Android TextToSpeech configured for the Google TTS engine and confirmed voice identifier VII. Query available Voice entries at runtime; never assume an identifier is installed or silently substitute another voice.

Reference: [TextToSpeech](https://developer.android.com/reference/android/speech/tts/TextToSpeech).

## State model

- DISABLED: tile off; no monitoring; no retained mute episode.
- ENABLED_IDLE: monitoring active; no eligible ad detected.
- MUTED_AD: at least one eligible playing session is classified as an ad; media volume is zero and original volume is retained.
- PERMISSION_REQUIRED: monitoring cannot proceed until Notification Access is granted.
- TTS_UNAVAILABLE: monitoring/muting continues, but requested voice is unavailable; no substitute voice is selected.

## Classification precedence

Browser exclusion takes precedence over all other rules. For non-browser sessions: not PLAYING means ignore; known duration <= 60,000 ms means ad; otherwise any string metadata value exactly equal to Advertisement (case-insensitive after trim) means ad; otherwise not an ad.

If multiple sessions are active, mute while at least one eligible playing non-browser session is classified as an ad. Restore only when none remain classified as an ad.

## Volume state machine

On transition into MUTED_AD: read current STREAM_MUSIC volume, save it once, set media volume to zero, and speak once.

While already muted: keep the saved volume unchanged. Do not capture zero on subsequent callbacks.

On transition out of MUTED_AD: restore the saved volume and clear it only after restoration is attempted.

On tile disable or service teardown: restore any captured volume before stopping.

Test user volume-button interaction while muted. Current requirement is to restore the pre-mute value; no separate override behavior has been approved.

## Browser exclusion

Identify browser packages by package identity and Android package visibility, potentially combining intent-resolution checks for HTTP/HTTPS handlers with a maintained denylist. Do not rely only on app display names. Validate on current Android versions and installed browsers.

## Lifecycle and recovery

Reconcile active sessions when monitoring starts and whenever session/metadata changes. Consider tile disable during an ad, listener disconnection, session removal, app process death, service recreation, TTS failure, and missing metadata. Device reboot auto-start is not approved.

Abrupt process death can lose an in-memory saved volume before restoration. Do not claim guaranteed recovery until an approved persistence strategy is implemented and tested.

## Platform references

- [Quick Settings tiles](https://developer.android.com/develop/ui/views/quicksettings-tiles)
- [TileService](https://developer.android.com/reference/android/service/quicksettings/TileService)
- [MediaSessionManager](https://developer.android.com/reference/android/media/session/MediaSessionManager)
- [MediaMetadata](https://developer.android.com/reference/android/media/MediaMetadata)
- [NotificationListenerService](https://developer.android.com/reference/android/service/notification/NotificationListenerService)
- [TextToSpeech](https://developer.android.com/reference/android/speech/tts/TextToSpeech)
- [AudioManager](https://developer.android.com/reference/android/media/AudioManager)
