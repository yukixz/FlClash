# Local Fork Goals

This fork (branch `local`) builds a personal Candy release from upstream FlClash. The customization lives in a
single commit on top of the `main` branch (upstream release tags). Every change made on `local` must serve the
goal below; if a proposed change does not, keep upstream code as-is.

## Goal

Maintain a personal Candy build that hides the FlClash branding visible to the operating system, keeps an
independent app identity, and tracks upstream releases.

## Principles

1. Only change what the OS can see, never app-internal surfaces.
   - Change: launcher/tile labels, debug build label, VPN foreground notification title (Candy).
   - Keep upstream: in-app texts, internal identifiers (notification channel id, logcat tag, `FlClashHelperService`,
     isolate names).

2. Independent app identity.
   - `applicationId: com.airis.flcandy`.
   - Dart and Kotlin MethodChannel name prefixes stay in sync via `Components.APP_ID`.

3. Use upstream code and configuration as-is unless necessary, to minimize maintenance cost.
   - CI steps in `.github/workflows/local-build.yaml` stay verbatim identical to the upstream `build.yaml` build
     job; only the trigger (push to `local`) and the matrix (android/ubuntu only) differ.
   - `main` mirrors upstream release tags with zero modifications; `local` is `main` plus the single custom commit.
   - Sync flow: fast-forward `main` to the new tag, rebase `local`, force-push to trigger CI. The customization
     stays one commit and never diverges.

## Known Gaps

- Launcher icon is still the FlClash logo (asset work, not done).
