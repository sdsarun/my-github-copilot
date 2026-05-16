---
agent: build-error-resolver
description: Fix Dart and Flutter build errors — null safety, pub dependency issues, missing implementations, and analyzer failures
---

# Dart / Flutter Build Fix

Analyze and fix the Dart or Flutter build failure shown below.

## Step 1 — Collect Error Output

```bash
flutter pub get
flutter analyze 2>&1 | head -80
flutter build apk --debug 2>&1 | tail -60   # Android
flutter build ios --debug 2>&1 | tail -60   # iOS (macOS only)
dart analyze 2>&1 | head -60                 # Dart-only project
dart pub get
```

Paste the full analyzer and build output.

## Step 2 — Categorize Errors

| Error Pattern                                                   | Meaning                                                             |
| --------------------------------------------------------------- | ------------------------------------------------------------------- |
| `The non-nullable variable X must be assigned`                  | Null safety — initialize the variable                               |
| `The method X isn't defined for the type Y`                     | Method does not exist on that type — check import or type           |
| `A value of type X can't be assigned to a variable of type Y`   | Type mismatch — use explicit cast or fix type                       |
| `The argument type X can't be assigned to the parameter type Y` | Wrong type passed to function                                       |
| `Undefined name X`                                              | Missing import, typo, or out of scope                               |
| `Target of URI hasn't been generated`                           | Generated code (json_serializable, freezed) not yet built           |
| `Because X depends on Y which requires SDK >=Z`                 | Flutter/Dart SDK constraint conflict — upgrade or constrain version |
| `Because X X.Y.Z is required by Z, version solving failed`      | Version conflict in `pubspec.yaml`                                  |

## Step 3 — Fix Strategy

**Null safety errors**

- Initialize with a default: `String name = '';`
- Make nullable: `String? name;`
- Use late: `late String name;` only when guaranteed initialized before first access
- Use null-aware operator: `value?.method() ?? fallback`

**Generated code missing (json_serializable, freezed, etc.)**

```bash
dart run build_runner build --delete-conflicting-outputs
# or
flutter pub run build_runner build --delete-conflicting-outputs
```

**Pub version conflict**

```bash
dart pub upgrade --major-versions     # Upgrade to latest compatible
dart pub deps                          # View dependency tree
```

Edit `pubspec.yaml` to relax conflicting constraints.

**Missing import**

```dart
import 'package:your_package/your_file.dart';
import 'path/to/local/file.dart';
```

**Abstract class not fully implemented**

- Add `@override` for all unimplemented abstract methods

## Step 4 — Verify

```bash
flutter analyze
flutter test
flutter build apk --debug   # or appropriate platform
```

## Output

List each fix applied:

```
File: lib/path/to/file.dart
Change: [What was changed and why]
```

Confirm with final `flutter analyze` output showing no issues.
