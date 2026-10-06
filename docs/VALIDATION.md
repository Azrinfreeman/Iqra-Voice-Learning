# Documentation validation

Reviewed on **2026-10-06**, against source commit `f4d6d7440af301216f5e43b8689503cf77e38558`.

## Verified from the repository

- Unity `2022.3.60f1`, embedded Whisper `1.3.2`, TextMeshPro `3.0.9`, and RTLTMPro `3.4.5` were checked against project/package metadata.
- All four enabled build scenes are tracked. The start scene is `Assets/_Flippy_Journey/Scenes/Home.unity`.
- The Ingame scene references the microphone transcription and question controllers. Its Whisper component selects `Whisper/ggml-tiny-q8_0.bin`, StreamingAssets, language `ms`, and no English translation.
- No Whisper `.bin` model is tracked. The separate recognizer defaults to `SpeechRecognitionSystem/model/english_small`, while the tracked model folder is `Assets/StreamingAssets/model/english_small`.
- Question feedback, text similarity, level selection, and local progress descriptions were checked against their source files and question prefabs.
- The game framework can submit leaderboard data after round completion when a saved username exists. A write credential is embedded in the leaderboard source; its ownership and validity were not tested.
- Documentation links were checked against tracked paths, and the change was checked for whitespace errors.
- The change is limited to a README, these notes, and ignore rules for future macOS metadata and Mono crash files. Existing tracked files are retained.

## Not validated in this review

Unity import, compilation, Play Mode, speech-model loading, microphone permissions, game completion, device performance, Android packaging, and recognition accuracy were not tested. Native libraries, crash blobs, and repository history were not security-audited. Third-party speech tests are present, but no passing test result is claimed here.

## Follow-up before a live demonstration

1. Rotate the exposed leaderboard write credential with the service owner and isolate any test leaderboard; removing a value from the latest source alone does not revoke it.
2. Provide the expected Whisper model, confirm the intended recognition language, and verify native-plugin compatibility on the target device.
3. Review all configured prompts and answer variants with a qualified educator. Test empty answer lists, recognition failures, similarity boundaries, retries, and final-question progression.
4. Exercise all enabled scenes and local progress using test data. Check which speech plugin is active, rather than relying on toggle labels.
5. Review tracked Mono crash files and macOS metadata separately before deciding whether to remove them. Ignore rules do not remove files already committed or erase their history.
6. Confirm asset redistribution permissions, then capture a verified demonstration and record the actual device/build results.
