# All-In-One — Multi-Role Management Platform

A **Flutter** platform that consolidates several operational roles into a single application.
Each user role gets its own dashboard and permissions, while shared modules — document viewing,
file sharing, PDF rendering — are available across the app.

---

## Highlights

- **Multi-role architecture** — one codebase, role-specific dashboards and navigation paths.
- **GetX navigation and state** — `GetMaterialApp` with named routes and `Get` for reactive
  state, keeping screens free of manual wiring.
- **Service layer abstraction** — `core/service/cache/cache_service.dart` centralises persistence
  so screens never talk to storage directly.
- **Document handling** — in-app PDF rendering via `flutter_pdfview`, native file picking via
  `file_picker`, and system sharing through `share_plus`.
- **Media embedding** — `youtube_player_iframe` for video content and `dynamic_height_grid_view`
  for adaptive image grids.
- **Performance details** — `visibility_detector` to defer offscreen work and a `logger`-based
  debug layer.

---

## Architecture

```
lib/src/
├── core/
│   ├── routes/
│   │   └── app_pages.dart          # Named route table
│   ├── service/
│   │   └── cache/cache_service.dart # Caching + storage abstraction
│   ├── theme/
│   │   └── theme.dart
│   └── utils/
│       ├── strings.dart            # Centralised copy
│       └── util.dart
└── ...                             # Feature modules per role
```

`main.dart` initialises the cache service once, before the app starts, so every screen can rely
on it being ready:

```dart
Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await CacheService().initialize();
  runApp(const MyApp());
}
```

---

## Tech Stack

| Concern | Choice |
|---|---|
| UI | Flutter, Material |
| State & routing | `get` (GetX), `get_storage` |
| Networking | `dio` |
| Files | `file_picker`, `share_plus`, `flutter_pdfview`, `path_provider` |
| Media | `youtube_player_iframe`, `dynamic_height_grid_view` |
| Backend | Firebase (configuration included in `firebase.json`) |
| Utilities | `logger`, `visibility_detector`, `url_launcher`, `intl` |

---

## Getting Started

### Prerequisites

- Flutter `>= 3.16`
- Firebase project (optional but recommended for auth and storage)

### Setup

```bash
git clone https://github.com/developer-rahim/All-In-One--Multi-Role-Management.git
cd All-In-One--Multi-Role-Management
flutter pub get
flutter run
```

### Firebase

The repository includes `firebase.json`. Add your own `google-services.json` (Android) and
`GoogleService-Info.plist` (iOS), then uncomment the Firebase initialization in `main.dart` if
you want the hosted backend enabled.

---

## Notes

- `CacheService().initialize()` must complete before `runApp` — screens assume it is ready.
- Use `stage_navigation_bar` for the staged role-based flow between onboarding and the
  role dashboard.

---

## License

Released under the MIT License.
