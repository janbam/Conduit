# Conduit fork changes

This file records the intentional differences between [janbam/Conduit](https://github.com/janbam/Conduit) and [gwitko/Conduit](https://github.com/gwitko/Conduit). It is the behavior checklist for upstream integration, not a copy of upstream's changelog.

Last integrated upstream `master`: [`9cd2ff9`](https://github.com/gwitko/Conduit/commit/9cd2ff9).

## Integrating upstream

1. Read the active changes below before merging. Review upstream's changes to their code paths even when Git reports no conflict.
2. Preserve upstream ancestry with a merge commit rather than a squash or rebase of upstream history.
3. Check that the documented fork behavior still works. If upstream provides an equivalent implementation, use it and record that the local implementation is retired. Get an explicit decision before changing or dropping the behavior.
4. Update the integration baseline and affected entries in this file in the same PR. Keep a short historical reference for changes retired or superseded by upstream.

## Active changes

### 2026-11-01: Custom Ctrl+backslash key

Commit subject: `feat: add custom Ctrl+backslash terminal key`.

- The custom-key dialog's Ctrl dropdown includes **Ctrl+\**. Its saved control-key value is `\`. It sends FS (`0x1C`) directly through the existing text-input path because the current terminal library does not encode Ctrl+backslash key events. Like other row keys, it clears latched modifiers after sending.
- This adds a custom-key option, not a new built-in key or a change to existing layouts.
- Implementation: [control-key choices](lib/core/theme/terminal_appearance.dart) and [keyboard bar](lib/features/terminal/presentation/terminal_keyboard_bar.dart).
- Regression checks: [editor selection and saved key](test/core/theme_sheet_test.dart) and [terminal wire output](test/features/terminal/terminal_test.dart).

### 2026-11-01: Return and custom Ctrl+Esc keys

[PR #1](https://github.com/janbam/Conduit/pull/1), landed as [`073c2cd`](https://github.com/janbam/Conduit/commit/073c2cd).

- The built-in **Return** row key sends exactly CR (`0x0D`), independently of the configured Enter sequence and latched Ctrl or Alt modifiers. Ordinary Enter continues to honor its configured sequence.
- Return follows Esc in the default layout. Existing saved layouts receive it through the unseen-built-in mechanism, which still allows users to move or remove it.
- The custom-key dialog's Ctrl dropdown includes **Ctrl+Esc**. Its saved label is `Esc`, and it uses the existing control-key handler with `TerminalKey.escape`. The current terminal encoding emits ESC (`0x1B`), not a distinct extended keyboard-protocol sequence.
- Implementation: [keyboard definitions](lib/core/theme/terminal_appearance.dart), [row editor](lib/core/presentation/theme_sheet.dart), [keyboard bar](lib/features/terminal/presentation/terminal_keyboard_bar.dart), and [session input](lib/features/terminal/presentation/terminal_session_controller.dart).
- Regression checks: [wire output and modifier behavior](test/features/terminal/terminal_test.dart), [custom-key editor](test/core/theme_sheet_test.dart), and [saved layouts](test/core/theme_preferences_repository_test.dart).

### 2026-11-01: Flutter 3.47 development setup

Commit [`ea451b4`](https://github.com/janbam/Conduit/commit/ea451b4).

- [analysis_options.yaml](analysis_options.yaml) excludes `build/**`, `android/**`, and `ios/**` from Dart analysis while retaining the existing strict language and lint rules.
- [pubspec.lock](pubspec.lock) reflects the local Flutter SDK's dependency resolution: `matcher` 0.12.20, `meta` 1.19.0, `test_api` 0.7.12, and `vector_math` 2.4.4. These are tooling adjustments, not pinned product behavior. Reconcile them with the selected SDK when integrating upstream.

### 2026-11-01: Fork development policy

Introduced in [`666dd00`](https://github.com/janbam/Conduit/commit/666dd00), extended by this document.

- [AGENTS.md](AGENTS.md) defines the fork's Android-only development-build scope, local Flutter and Android SDK paths, and prohibition on release signing and publishing from this checkout.
- Keep this record current alongside fork changes and upstream integrations, as required by `AGENTS.md`.
