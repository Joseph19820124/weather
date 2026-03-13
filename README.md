# weather

A Rust CLI that fetches current weather from [Open-Meteo](https://open-meteo.com/) and optionally generates an AI image via [banana](https://github.com/thepagent/banana).

## Usage

```bash
weather --timezone Asia/Taipei
# weather=Partly cloudy current=13.9°C max=20.9°C min=13.8°C localtime=2026-03-14T02:15
```

### Generate image

```bash
GEMINI_API_KEY=<key> weather --timezone Asia/Shanghai \
  --image \
  --prompt "夢幻插畫風格，手機桌布" \
  --output /tmp/weather.png \
  --resolution 1K
```

### Full example with detailed prompt

```bash
GEMINI_API_KEY=<key> weather --timezone Asia/Taipei \
  --image \
  --prompt "根據所給的時間、城市與天氣進行創作，夢幻插畫風格，手機桌布，展現城市、建築、交通、地標、美食、人文、風景、文化等隨機其中一個特色，畫面元素不要太多，所有文字都不要" \
  --output /tmp/out.png \
  --resolution "1K"
```

## Options

| Flag | Description | Default |
|------|-------------|---------|
| `--timezone <tz>` | City timezone | `America/New_York` |
| `--image` | Generate weather image via banana | off |
| `--prompt <text>` | Extra prompt appended to image generation | — |
| `--output <path>` | Image output path | `output.png` |
| `--resolution <res>` | Image resolution: `1K`, `2K`, `4K` | `1K` |

## Supported Timezones

`America/New_York` · `America/Los_Angeles` · `Europe/London` · `Asia/Tokyo` · `Asia/Taipei` · `Asia/Shanghai`

## Install

```bash
cargo build --release
ln -sf $PWD/target/release/weather ~/.local/bin/weather
```

## Image generation

Requires `banana` in `$PATH` and `GEMINI_API_KEY` set in the environment.
The prompt sent to banana is auto-constructed as:

```
{city}, {localtime}, {weather}, current {temp}°C, max {max}°C, min {min}°C. {--prompt}
```
