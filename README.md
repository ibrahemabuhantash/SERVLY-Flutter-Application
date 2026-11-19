# SERVLY — Home Services App

Figma-to-Flutter portfolio concept • Android and iOS • English and Arabic (RTL).

Figma: https://www.figma.com/design/v64wySc08fDxSDfUEUiPlH

## Run on Windows / VS Code

Requirements: Flutter SDK 3.29+ (Dart 3.7+), Git, Android Studio Android SDK or connected Android device. Use an SDK compatible with the pinned packages. Open this folder in VS Code. In PowerShell:

```powershell
flutter doctor
powershell -ExecutionPolicy Bypass -File .\tool\setup.ps1
flutter analyze
flutter test
flutter devices
flutter run
```

The setup script creates **missing** Android/iOS platform folders from your local Flutter SDK in a temporary folder; it does not overwrite application source files. If PowerShell is unavailable, you can run `flutter create --no-pub --platforms=android,ios --org com.servly --project-name servly .` after backing up your project, followed by `flutter pub get`. The script is preferred to avoid changing source.

On macOS/Linux use `bash tool/setup.sh`. For iOS simulator/build you need **macOS + Xcode**, and may need CocoaPods / other Flutter doctor steps. On Windows you can develop the shared code and run Android, but cannot build iOS locally.

```bash
flutter build apk --release
# macOS only; select a simulator or configure signing for a physical device
flutter build ios --simulator
```

## What is implemented

- Welcome, guest access, demo sign-in, register, OTP (123456), recovery and validation.
- Home, catalogue and live bilingual search; six service details from one reusable screen.
- Date picker, four time slots, local collision checking, address form, review, pay-after-service demo and confirmation.
- Persistent booking list, booking details, reschedule preserving reference, cancel and free the slot.
- In-app notification history and read/unread state; profile, saved addresses, locale persistence, help, empty/error/loading states.
- Material 3 theme, Figma color tokens, original design vector icons, responsive scrolling, direction-aware layout and localized Material dates.

## Architecture

`domain/` pure Dart models, rules, repository contract; `data/` SharedPreferences adapter; `application/` Riverpod providers/controller; `presentation/` feature screens and reusable widgets; `core/` theme, bilingual text helpers and GoRouter. Figma repeated service/language/validation frames are represented by parameterized screens, not 142 separate Dart pages. String localization currently uses a two-language helper rather than generated ARB files.

## Important boundaries

- This is a **local portfolio demo**, not a live service marketplace. No API, real authentication, SMS, payment, push notifications, server availability, dispatch or multi-user sync. OTP 123456 and login are explicit simulations. Use fictional phone/address details and credentials. Passwords and OTPs are not persisted. Demo profile/booking/address data are stored in SharedPreferences, **not encrypted**; clear local data in Profile or sign out (which clears demo data). Do not use real customer data.
- Pricing uses exact catalogue values from Figma (JOD); time slots and dates are dynamically selected using device local time, with demo availability per service. Historical Figma dates are not hard-coded. No actual payment takes place. In-app notifications are created on confirmation, cancellation and reschedule; reminders and push are not implemented. Pending/completed labels exist for display but there is no provider workflow to update those statuses.
- Figma's temporary asset URL was unreachable in the authoring environment. Local SVGs use the original vector definitions from the design; their pixel geometry and rendering are not independently verified. Inter and Cairo family names are defined, but font files are not bundled; native fonts are fallbacks until licensed font files are added and declared in `pubspec.yaml` (under `flutter/fonts`).
- Flutter/Dart SDKs are not installed in the authoring environment and dependency downloads were unavailable. The source is provided with tests and platform setup scripts, **not a verified Android/iOS build or APK**. Run `flutter analyze`, `flutter test`, then device smoke tests. No claim of pixel-perfect parity or passing runtime tests.

## Manual QA

1. Start from Welcome, switch Arabic/English; enter guest mode. Confirm RTL navigation and back arrow.
2. Browse all six services, search by English and Arabic, search nonexistent text.
3. Select a service, pick future date and available time. Try invalid phone or missing address. Use fictional data, e.g. Demo User / 0790000000 / Amman / Sample Street.
4. Review the correct price and appointment; confirm. View bookings; relaunch app and verify persistence.
5. Try same service/slot; it should be disabled. Reschedule and confirm same reference. Cancel and confirm it is retained as cancelled and the slot is freed.
6. Check notifications, mark read; save/remove a fictional address, update profile, switch locale and relaunch.
7. Test on narrow 320dp, 390dp, 430dp and tablet, increasing text size, keyboard opened, no overflow or hidden action. Test Android and iOS.
8. Test registration with demo OTP 123456; invalid input should show errors. Test loading/error previews under Help & support. Sign out/clear data only after confirmation.

## License and ownership

Concept branded SERVLY; replace identity, legal/policy content, authentication and backend before commercial deployment. Review third-party dependency licenses before distribution.
