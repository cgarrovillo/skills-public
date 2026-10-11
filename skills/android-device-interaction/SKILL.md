---
name: android-device-interaction
description: Use when observing, testing, diagnosing, or controlling an Android phone or emulator through ADB, Android CLI, UI Automator, device logs, or screen evidence.
---

# Android Device Interaction

Use this skill for Android-device work. Prefer structured, semantic evidence
over a screenshot: it is usually faster, more robust to visual changes, and
less likely to expose unnecessary screen content.

## Select and protect the target

- Identify the intended emulator or device before acting. If more than one is
  connected, require an explicit serial and pass it to ADB; never silently
  choose a physical device.
- Treat physical devices, UI-hierarchy exports, logs, and screenshots as
  potentially sensitive. Collect only what the task needs.
- Do not enter payment details, PINs, credentials, or security-protection
  inputs. Do not install tooling, change device settings, reset, or clear a
  device unless the task specifically authorizes it.

## Observe in this order

Use the first source that can reliably answer the question.

1. **Application or test state.** Prefer an app callback, SDK result,
   instrumentation assertion, log/event, API response, or test fixture for a
   business outcome. Pixels alone do not prove application state.
2. **Android CLI layout.** When the official Android CLI is installed and its
   `layout` command supports the selected target, first verify it with
   `android layout --help`. Capture a baseline with `android layout`, then use
   `android layout --diff` for later checks. It provides structured JSON rather
   than a bitmap. If the command is unavailable or cannot target the device
   unambiguously, continue to the next option; do not install it automatically.
3. **Semantic UI hierarchy.** For app-owned tests, prefer Compose semantics or
   Espresso. For system, cross-app, or third-party flows, use UI Automator or a
   carefully managed `uiautomator dump`. Prefer resource IDs, test tags,
   content descriptions, text, roles, enabled state, and bounds over coordinate
   guesses. Use a unique temporary device path for a hierarchy dump, retrieve
   only what is needed, and remove it when safe.
4. **Screenshot fallback.** Take a screenshot only when structured state is
   unavailable or insufficient—for example, a custom-rendered or secure
   surface, a third-party screen without semantics, or a visual-regression
   assertion. State why the fallback was needed and retain it only as long as
   necessary.

## Interact and wait reliably

- Select controls through stable semantic identifiers whenever possible. Do
  not make screenshot-estimated coordinate taps the normal interaction method.
- If a coordinate action is unavoidable, derive it from the current hierarchy
  or a documented screenshot fallback, then confirm the expected semantic or
  application-state transition.
- Wait for a named UI condition, callback, or event. Do not use arbitrary
  sleeps to compensate for asynchronous UI.
- A changed screenshot is not sufficient success evidence when application or
  server state is available.

## Record useful evidence

For each Android-device check, record the selected target and relevant
environment details, the asserted condition and observation source, any action
and post-action confirmation, temporary-artifact cleanup, and the reason and
privacy minimization for any screenshot fallback.

## References

- [Android UI test behavior](https://developer.android.com/training/testing/ui-tests/behavior)
- [Android CLI](https://developer.android.com/tools/agents/android-cli)
- [Instrumented-test stability](https://developer.android.com/training/testing/instrumented-tests/stability)
- [Compose semantics with UI Automator](https://developer.android.com/develop/ui/compose/testing/interoperability)
