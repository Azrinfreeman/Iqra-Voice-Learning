# Iqra Voice Learning

A Unity learning game prototype that combines Iqra reading practice, microphone transcription, and game progression. Learners answer displayed prompts aloud; the application compares recognized text with configured answer variants and provides visual and audio feedback.

The project demonstrates speech recognition integration, Arabic text presentation, question progression, and adaptation of an existing game framework. Its feedback is based on **transcribed text similarity**. It has not been validated as a pronunciation or tajwid assessment tool.

## Learning flow

1. Enter through the home screen and choose a learning level.
2. Read the displayed prompt and record a response using the microphone control.
3. Compare the transcription with the question's configured answer variants.
4. Receive traffic-light feedback, a similarity percentage, and a correct-answer sound when accepted.
5. Continue through questions and complete a round within the game progression system.

This flow is described from the source and scene configuration; a fresh playable build has not been verified during this documentation review.

## Technology

| Component | Repository configuration |
| --- | --- |
| Unity Editor | **2022.3.60f1** |
| Language | C# |
| Speech recognition | Embedded `com.whisper.unity` **1.3.2**, with native whisper libraries |
| Text presentation | TextMeshPro **3.0.9**, RTLTMPro **3.4.5**, ArabicSupport |
| Progress | Local `PlayerPrefs` values for selected level, question position, and game progress |
| Game framework | Existing `_Flippy_Journey` scenes, controllers, and services |

The enabled build scenes are **Home**, **Loading**, **Ingame**, and **Character** under [Assets/_Flippy_Journey/Scenes](Assets/_Flippy_Journey/Scenes). The Unity product name is currently `GISuara`, with configured version `1.0.9`; these settings are preserved.

## Open the project

1. Clone the repository:

   ```sh
   git clone https://github.com/Azrinfreeman/Iqra-Voice-Learning.git
   ```

2. Install Unity **2022.3.60f1** through Unity Hub and open the repository root. Package restoration needs network access, including the configured OpenUPM registry for RTLTMPro.
3. Resolve any package or native-plugin import errors before running. For Android work, install the matching Android build support and SDK/NDK tooling.
4. Supply a compatible Whisper model. The **Ingame scene** expects `Assets/StreamingAssets/Whisper/ggml-tiny-q8_0.bin`, uses language `ms`, and disables translation to English. That model file is **not tracked in this repository**. The manager's code default is a different path, `Whisper/ggml-tiny.bin`; inspect the component settings for the scene you are using.
5. Inspect microphone permissions, scene references, and service configuration, then open `Assets/_Flippy_Journey/Scenes/Home.unity` to review the full flow.

The project also retains a separate SpeechRecognitionSystem integration and an `english_small` model folder. Its recognizer's default directory differs from that tracked folder; it does not replace the missing Whisper model. Check which recognizer is enabled before testing.

The inherited framework includes a Dreamlo leaderboard client. Review its configuration before Play Mode: completing a round can submit data when a saved username exists. Use an isolated test service for validation. No service request was made during this documentation work.

## Source guide

| Area | Starting point |
| --- | --- |
| Prompts, answer variants, feedback, and round completion | [QuestionController.cs](Assets/QuestionController.cs) |
| Three learning-level selections | [LevelController.cs](Assets/LevelController.cs) |
| Levenshtein text comparison and percentage display | [SimilarityCalculator.cs](Assets/SimilarityCalculator.cs) |
| Recording, transcription, and answer comparison | [MicrophoneDemo.cs](Assets/Samples/2%20-%20Microphone/MicrophoneDemo.cs) |
| Question data prefabs | [Assets/PrefabsQuestion](Assets/PrefabsQuestion) |
| Embedded Whisper integration | [Packages/com.whisper.unity](Packages/com.whisper.unity) |
| Additional speech recognizer | [SpeechRecognizer.cs](Assets/SpeechRecognitionSystem/Scripts/SpeechRecognizer.cs) |
| Framework progression and services | [Assets/_Flippy_Journey/Scripts](Assets/_Flippy_Journey/Scripts) |

## Current status

- Recognition quality, Arabic reading suitability, microphone behavior, and Android performance require manual validation with the intended learners and devices.
- Answer acceptance uses hard-coded text similarity conditions; boundary values and incomplete answer lists need review before a classroom demonstration.
- Multiple speech integrations and sample scripts coexist. The streaming sample's answer-comparison call is commented out, and `PluginController` currently selects the same plugin in every branch.
- A leaderboard write credential is embedded in the source. Treat it as exposed and arrange rotation with the service owner before connecting a deployed application. Do not copy it into a new service or documentation.
- No GitHub release or verified demo capture was available at the time of this review. See [validation notes](docs/VALIDATION.md) for what was checked and the remaining verification work.

## Third-party assets

This repository contains third-party game framework, speech, native-plugin, font, and text-rendering assets. Preserve their notices and check the applicable licenses before redistribution. A repository-wide license has not been added; the embedded Whisper package declares MIT in its own package metadata.
