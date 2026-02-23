# spank

Slap your MacBook, it yells back.

Uses the Apple Silicon accelerometer (Bosch BMI286 IMU via IOKit HID) to detect physical hits on your laptop and plays audio responses.

## Requirements

- macOS on Apple Silicon
- [sensord](https://github.com/taigrr/apple-silicon-accelerometer) running (with `sudo`) to provide sensor data

## Install

```bash
go install github.com/taigrr/spank@latest
```

Or download from [Releases](https://github.com/taigrr/spank/releases).

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

1. Reads accelerometer data from POSIX shared memory (written by `sensord`)
2. Runs vibration detection (STA/LTA, CUSUM, kurtosis, peak/MAD)
3. When a significant impact is detected (CHOC_MAJEUR, CHOC_MOYEN, or MICRO_CHOC severity), plays an embedded MP3 response
4. 500ms cooldown between responses to prevent rapid-fire

## License

0BSD
