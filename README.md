# Hey Lucy — ESPHome micro Wake Word Model

A small, English-language **“Hey Lucy”** wake-word model and its ESPHome-compatible manifest files, shared by [Purpose in Practice](https://pipbuilds.com/). A wake word is the phrase a voice assistant listens for before it starts processing a command. This repository provides **wake-word detection assets**, not a complete voice assistant.

> **Project status:** Model and manifest files are published. This repository does **not** currently include training notebooks, training data, hardware-specific ESPHome configurations, benchmark results, or a documented end-to-end installation test. Do not interpret the included configuration values as independently measured accuracy.

## Repository files

| File | Purpose |
| --- | --- |
| [`hey_lucy.json`](hey_lucy.json) | Main ESPHome micro wake-word model manifest; references `hey_lucy.tflite`. |
| [`hey_lucy.tflite`](hey_lucy.tflite) | TensorFlow Lite wake-word model binary. |
| [`old_lucy.json`](old_lucy.json) | Additional manifest, currently **identical** to `hey_lucy.json` and still referencing `hey_lucy.tflite`. |
| [`old_lucy.tflite`](old_lucy.tflite) | Separately named model binary. Its behavior and relationship to the main model have not been benchmarked here. |
| [`LICENSE`](LICENSE) | Repository MIT license. Check the original trainer/model attribution and any applicable upstream terms before redistribution. |

**Important:** `old_lucy.json` does *not* point to `old_lucy.tflite`. If you intend to use the older binary, make a separate manifest that references that filename, and test the resulting configuration. No file has been renamed or altered for this documentation update.

## What the manifest says

The published `hey_lucy.json` declares:

| Field | Value |
| --- | --- |
| Wake word | `hey_lucy` (“Hey Lucy”) |
| Model type | `micro` |
| Trained language | `en` (English) |
| Manifest version | `2` |
| Probability cutoff | `0.97` |
| Sliding window size | `5` |
| Feature step size | `10` |
| Tensor arena size | `30000` |
| Minimum ESPHome version declared | `2024.7.0` |
| Manifest author | `Tater Totterson` |
| Referenced training project | [microWakeWord Trainer — Apple Silicon](https://github.com/TaterTotterson/microWakeWord-Trainer-AppleSilicon) |

These values are **manifest settings**, not evidence of detection accuracy, latency, memory usage on a particular board, or compatibility with every ESP32 device.

## How it fits into a voice assistant

1. A supported device with a microphone captures audio.
2. A compatible ESPHome `micro_wake_word` component evaluates audio for the phrase **“Hey Lucy.”**
3. After a recognized wake word, a separately configured voice-assistant pipeline can capture and process a spoken command.
4. Home Assistant can then handle supported actions, depending on your voice-assistant configuration.

Wake-word detection, speech recognition, intent processing, and text-to-speech are **different functions**. This model provides only the first one. Local wake-word detection also does not guarantee that the rest of the pipeline is local.

## Using the model with ESPHome

ESPHome supports custom micro wake-word models through a manifest URL in compatible configurations. Consult the [official ESPHome micro_wake_word documentation](https://esphome.io/components/micro_wake_word/) for supported hardware, microphone configuration, and the current YAML syntax.

The raw manifest URL for this repository is:

```text
https://raw.githubusercontent.com/loperkin/micro-wake-word-model/main/hey_lucy.json
```

A **minimal illustrative fragment** of the model selection is:

```yaml
micro_wake_word:
  models:
    - model: https://raw.githubusercontent.com/loperkin/micro-wake-word-model/main/hey_lucy.json
```

This is **not** a complete flashable configuration. You still need a supported board, audio input, and the other required ESPHome components; use the official documentation for your specific hardware and ESPHome release. The model has not been independently validated on a named device in this repository.

## Testing checklist

Before describing the model as production-ready, test and record:

- **Successful compilation and flashing:** ESPHome version, board, microphone, and configuration.
- **True activations:** Speak “Hey Lucy” at multiple distances and in different rooms.
- **Missed activations:** Note when the phrase is spoken but not detected.
- **False activations:** Run normal conversations, television, and music without saying the phrase; record unexpected triggers over time.
- **Real-world conditions:** Background noise, microphone placement, speakers, and different voices.
- **Resource use:** Memory, stability, and response latency on the target device.

The probability cutoff of `0.97` is a starting configuration value in the supplied manifest, **not** a documented success rate.

## Attribution and licensing

The manifest lists **Tater Totterson** as author and links to the [Apple Silicon microWakeWord training project](https://github.com/TaterTotterson/microWakeWord-Trainer-AppleSilicon). This repository includes an [MIT license](LICENSE) credited to Purpose in Practice. Preserve attribution and review the provenance and upstream licensing of model artifacts and training materials before distributing derivatives.

## Learn more

- [Micro Wake Word project overview on PIP Builds](https://pipbuilds.com/projects/wake-word/)
- [What Is Home Assistant?](https://pipbuilds.com/guides/what-is-home-assistant/)
- [ESPHome micro wake word documentation](https://esphome.io/components/micro_wake_word/)
- [Purpose in Practice on YouTube](https://www.youtube.com/@PurposeinPracticeCh)

**Purpose in Practice:** Learn it. Build it. Put it into practice.
