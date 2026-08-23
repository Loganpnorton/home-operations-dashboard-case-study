# Mobile delivery

## Shared application shell

Capacitor packages the deployed web application for iOS and Android while preserving a shared React codebase. Native plugins are limited to capabilities that materially improve the experience, such as push delivery, widgets, and platform lifecycle handling.

The mobile shell presents a controlled application origin and uses authenticated deep links. Native and web release compatibility is tracked because a remotely deployed web bundle can change independently from an installed shell.

## Push notifications

The server owns notification intent and recipient selection. It sends platform-specific payloads through APNs and FCM after checking user preferences and device registration state.

Device tokens are treated as sensitive identifiers. They are stored server-side, rotated when a platform replaces them, and removed after repeated invalid-token responses.

## iOS features

Live Activities expose time-sensitive household state without requiring the full app to remain open. Widgets provide a small set of intentionally safe status summaries and entry points. Neither surface displays private content on a locked device unless the user has explicitly enabled it.

## Android delivery

The Android package uses the same authenticated application shell and FCM delivery path. Build artifacts are produced in a controlled workflow and signed outside the source repository.

## Tradeoffs

The shared shell reduces duplicated UI work and keeps web and mobile behavior aligned. It also requires careful handling of offline startup, remote bundle compatibility, platform permissions, and native bridge failures. Platform-specific code is kept behind narrow interfaces so those concerns do not spread through the main application.

