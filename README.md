# Snakie — Flutter Snake Game

<p align="center">
  <img src="images/Logo2.png" alt="Snakie logo" width="300">
</p>

A Flutter learning project that extends a 2D snake game with account screens, customizable snake styles, and weather-dependent difficulty.

Built with **Dart**, **Flutter**, **Firebase Authentication**, and the **OpenWeather API**. The core game is based on a Kodeco tutorial, credited below.

## Features

- **Grid-based gameplay:** steer with four on-screen directional buttons, eat food, and grow the snake.
- **Score and restart:** each food item adds 5 points; hitting a boundary opens a game-over dialog with restart and home options.
- **Snake customization:** choose a body color and one of three modes.
- **Weather integration:** request device location and display local weather, temperature, and time.
- **Temperature-based acceleration:** the snake speeds up after eating, with larger increments at higher temperatures.
- **Account flow:** Firebase email/password sign-up and login, with form validation and toast feedback.
- **Local preferences:** store the player email, snake name, and first-launch state using SharedPreferences.
- **In-app instructions:** a How to Play screen explains the controls and objective.

### Snake modes

| Mode | Behavior |
| --- | --- |
| Normal | Standard movement with the selected body color |
| Cheese | Alternating body segments use a translucent color |
| Direction | Reverses movement relative to the directional controls |

### Temperature and speed

The movement timer uses an interval of `200 ~/ speed` milliseconds. Eating food increases the speed multiplier according to the current weather temperature:

| Temperature | Speed increment per food |
| --- | --- |
| 30°C or higher | +0.10 |
| 15°C to below 30°C | +0.08 |
| Below 15°C | +0.05 |

These rules describe the current source implementation. Gameplay has not been run as part of this documentation update.

## How to play

1. Create an account or log in.
2. Set your snake name and open Settings to choose a color or mode.
3. Use the location/weather screen to load local weather.
4. Start a game and tap the on-screen arrows to steer.
5. Collect food to grow and increase your score; avoid the play-area boundaries.
6. Choose **Restart** after game over to play again.

The current collision logic checks boundaries; self-collision is not implemented. Keyboard and swipe controls are not implemented in the inspected control panel.

## Getting started

### Prerequisites and compatibility

- Flutter SDK and its bundled Dart SDK.
- A configured target device or emulator.
- A Firebase project with Email/Password Authentication enabled.
- An OpenWeather API key for weather functionality.

**This is a legacy project.** `pubspec.yaml` declares Dart `>=2.7.0 <3.0.0`, and parts of the source use pre-null-safety Dart. The dependency set has not been verified against that range. A current Flutter installation using Dart 3 cannot resolve the project unchanged; either establish a compatible historical toolchain and dependency set or migrate the code and constraints together.

The repository contains Android, iOS, web, Windows, macOS, and Linux platform folders. Their presence does not establish that every target builds or supports all integrations.

### 1. Clone the repository

```sh
git clone https://github.com/Codooooooo/Snake-Game-_Flutter.git
cd Snake-Game-_Flutter
flutter doctor
```

### 2. Configure weather access

Create a `.env` file in the project root:

```dotenv
API_KEY=your_openweather_api_key
```

The application loads this file at startup, and it is declared in the Flutter assets list. Keep local configuration out of Git. Values bundled into a client application can be extracted, so do not place privileged server credentials in this file.

Weather requests use device coordinates and metric units. Location permission and network access are required.

### 3. Configure Firebase

Register your intended app platform in your own Firebase project, enable the Email/Password sign-in provider, and supply the platform configuration required by Firebase. The login and sign-up screens call `Firebase.initializeApp()`; they need working configuration for the chosen target.

### 4. Resolve dependencies and run

After resolving the SDK/dependency compatibility issues:

```sh
flutter pub get
flutter devices
flutter run
```

If dependency resolution or compilation fails, review the compatibility notes below before treating these commands as a verified build procedure.

## Project structure

```text
lib/
├── main.dart                 # Environment loading and initial navigation
├── game.dart                 # Movement, food, score, timer, game-over flow
├── piece.dart                # Snake and food rendering
├── control_panel.dart        # On-screen directional controls
├── control_button.dart       # Reusable control button
├── direction.dart            # Direction definitions
├── direction_type.dart       # Horizontal / vertical direction categories
├── Screens/                  # Home, login, sign-up, settings, name, instructions
├── models/                   # Local profile preferences and weather data
└── widgets/                  # Weather display components
assets/                       # Runtime images, font, and app icon resources
images/                       # Project artwork
android/, ios/, web/, ...      # Platform-specific Flutter scaffolding
pubspec.yaml                  # Dependencies, assets, and SDK constraints
```

## Implementation notes

- Gameplay uses Flutter widgets, a list of grid positions, and a periodic timer rather than a separate game engine.
- Settings supply the selected body color and snake mode to the game.
- Dio retrieves current weather from OpenWeather; the location plugin supplies device coordinates.
- Firebase Authentication handles account creation and sign-in. SharedPreferences provides local profile preferences, not a server-side player database.
- There is no implemented leaderboard or persistent high-score system in the inspected game code.

## Current limitations

- The project needs SDK and dependency compatibility verification before a reproducible build can be claimed.
- `lib/widgets/Weather.dart` imports `package:intl/intl.dart`, but `intl` is not declared as a direct dependency in the current manifest.
- Firebase, weather configuration, location permissions, and target-specific behavior require local setup.
- The game does not implement self-collision, pause/resume, or keyboard controls.
- No runtime, device, or automated gameplay tests were performed for this README update.

## Credits

The original README credits the core game source and tutorial to:

**Kodeco — [How to Create a 2D Snake Game in Flutter](https://www.kodeco.com/19430602-how-to-create-a-2d-snake-game-in-flutter)**

This repository builds on that tutorial with account screens, customization, local preferences, and weather-related gameplay. Retain the original attribution when discussing or reusing the project. No repository license is assigned in the inspected root; check the original material's terms and contributor permissions before redistribution.
