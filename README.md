# spank

Slap your MacBook, it yells back.

Uses the Apple Silicon accelerometer (Bosch BMI286 IMU via IOKit HID) to detect physical hits on your laptop and plays audio responses.

## Requirements

- macOS on Apple Silicon

## Install

Download both `spank` and `sensord` from the [latest release](https://github.com/taigrr/spank/releases/latest).

Or build from source:

```bash
GOPRIVATE=github.com/taigrr/apple-silicon-accelerometer go install github.com/taigrr/spank@latest
GOPRIVATE=github.com/taigrr/apple-silicon-accelerometer go install github.com/taigrr/spank/cmd/sensord@latest
```

## Usage

```bash
# Start sensor daemon first (in another terminal)
sudo sensord

# Normal mode — says "ow!" when slapped
spank

# Sexy mode — escalating responses based on slap frequency
spank --sexy
```

### Modes

**Pain mode** (default): Randomly plays from 10 pain/protest audio clips when a slap is detected.

**Sexy mode** (`--sexy`): Tracks slaps within a rolling 1-minute window. The more you slap, the more intense the audio response. 60 levels of escalation.

## How it works

1. `sensord` reads raw accelerometer data via IOKit HID and writes to POSIX shared memory
2. `spank` reads from shared memory and runs vibration detection (STA/LTA, CUSUM, kurtosis, peak/MAD)
3. When a significant impact is detected, plays an embedded MP3 response
4. 500ms cooldown between responses to prevent rapid-fire

## License

0BSD
