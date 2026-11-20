# 📵 Phuck Ads

&nbsp;&nbsp;![Status](https://img.shields.io/badge/status-in%20development-orange)&nbsp;&nbsp;&nbsp;&nbsp;![Platform](https://img.shields.io/badge/platform-Android-green)

A tiny Android background service for muting the bullshit between the music. Enable or disable it from a Quick Settings tile—no app dashboard, no settings maze, and no browser interference.

When enabled, Phuck Ads watches active media sessions. If a non-browser media app is playing a track that is 60 seconds or shorter, or any media metadata field has the exact value **Advertisement**, it temporarily mutes media audio and announces:

> Ads are fucking annoying, bullshit is muted until the music returns

When the ad metadata disappears or the track changes to something longer than 60 seconds, the service restores the media volume level captured before muting.

**Current state:** Repository and requirements initialized. Android implementation and test APK are not yet available. The exact Google TTS voice identifier meant by **VII** needs confirmation before voice selection is finalized.

## 🎯 Project scope

- Background Android service controlled by one Quick Settings tile.
- Media-session based detection for dedicated media apps such as YouTube and audio-streaming services.
- Short-track detection (60 seconds or less) and Advertisement metadata detection.
- Temporary media-stream muting and restoration of the prior volume.
- Google TTS announcement using the specifically requested voice, once its identifier is confirmed.
- Browser apps excluded.

## 🚫 Explicit exclusions

- No conventional app screens or dashboard.
- No browser ad blocking or browser media-session muting.
- No network inspection, DNS filtering, VPN, Accessibility scraping, root, or telemetry.
- No permanent volume reduction.
- No automatic reboot-start behavior unless separately approved.

Android's own Settings screens may be required once to grant Notification Access. That is a platform permission flow, not an app-created settings screen.

## 🧭 Project status

| Area | Status |
|---|---|
| Repository and requirements | Documented |
| Architecture and phase plan | Documented |
| Quick Settings tile | Planned |
| Media-session monitoring | Planned |
| Advertisement detection | Planned |
| Volume mute/restore | Planned |
| Google TTS voice VII | Awaiting identifier confirmation |
| Automated tests / APK | Not yet available |
| Physical-device verification | Not started |

See [project plan](docs/PROJECT_PLAN.md), [requirements](docs/REQUIREMENTS.md), [architecture](docs/ARCHITECTURE.md), and [decisions](docs/DECISIONS.md).

## ⚙️ Technical direction

The proposed design uses Android TileService for the Quick Settings switch and NotificationListenerService authorization to access active media sessions through MediaSessionManager. It reads MediaMetadata and PlaybackState, controls the media stream through AudioManager, and uses Android TextToSpeech with the Google engine.

- [Quick Settings tiles](https://developer.android.com/develop/ui/views/quicksettings-tiles)
- [TileService API](https://developer.android.com/reference/android/service/quicksettings/TileService)
- [MediaSessionManager API](https://developer.android.com/reference/android/media/session/MediaSessionManager)
- [MediaMetadata API](https://developer.android.com/reference/android/media/MediaMetadata)
- [NotificationListenerService API](https://developer.android.com/reference/android/service/notification/NotificationListenerService)
- [TextToSpeech API](https://developer.android.com/reference/android/speech/tts/TextToSpeech)
- [AudioManager API](https://developer.android.com/reference/android/media/AudioManager)

## 🛠️ Development

Development follows the shared [Pistol Whip development protocols](https://github.com/pistolwhip-justin/development-protocols-n-assets), currently version 1.1.7. Read [AGENTS.md](AGENTS.md) before making changes.

Development is organized into readiness-gated phases. This README must be reviewed and updated alongside every phase. APKs are published through dedicated GitHub Actions workflows to immutable, versioned GitHub Releases, following the global protocol's in-chat attachment and checksum requirements.

A GitHub Project board is intentionally **not** part of this project.

## 🧪 Testing

Validation includes unit tests for ad classification and volume restoration, plus physical-device checks for media-session behavior, browser exclusion, tile state, TTS voice selection, and Android background/service lifecycle behavior. No test results are claimed until they have actually run.

## ⚠️ Known limitations and open questions

- Media apps must expose an Android media session and usable metadata.
- Missing/unknown duration cannot satisfy the short-track rule.
- Notification Access is required to enumerate active media sessions.
- The exact Google TTS voice identifier corresponding to **VII** is not yet confirmed; no substitute voice will be selected silently.
- Audio volume and background lifecycle behavior must be verified on physical devices.

## 📜 License

No license has been selected. Do not add a license file or claim open-source licensing until explicitly authorized.

---

Last updated: 2026-11-19
