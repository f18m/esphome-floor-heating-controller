# Copilot Instructions for floor-heating-controller

## Project Overview
This is an **ESPHome package** for controlling under-floor heating systems via Home Assistant. It runs on an ESP32 relay board and presents multiple relays (heating zones) and temperature sensors to HA. The project is designed as a reusable package using substitutions for easy deployment across multiple instances.

## Architecture & Key Concepts

### Package-Based Design
- **main.yaml** is an ESPHome package that must be referenced from other YAML configs (see `example-instance.yaml`)
- Uses [substitutions](https://esphome.io/components/substitutions/) (e.g., `${relay1_name}`, `${encryption_key}`) to make the package reusable
- Consumers override substitutions and extend sensor definitions (see `example-instance-with-different-sensors.yaml` using YAML anchors `!extend`)
- Never modify substitution defaults for production—consumers should always provide their own values

### Hardware Mapping
- **ESP32 Module**: ESP32-WROOM-32E (GPIO pins 12-14, 23, 25-27, 32-33 for relays; 5, 15-17 for sensors)
- **GPIO12 is a strapping pin**—handle carefully; see [ESPHome FAQ](https://esphome.io/guides/faq.html#why-am-i-getting-a-warning-about-strapping-pins)
- **Sensors**: MAX6675 (SPI-based pipe temperature) and DHT11 (GPIO5 ambient) with 60s update intervals
- **Connectivity**: WiFi fallback AP + OTA updates for wireless flashing; reboot timeout set to 1min to recover from WiFi glitches

## Critical Patterns & Conventions

1. **Relay Configuration**: All relay switches use `restore_mode: RESTORE_DEFAULT_OFF` to safely default to off on power-up
2. **Sensor Substitution**: Consumers extend sensor configurations rather than duplicate them (e.g., switching DHT11→DHT22)
3. **Secrets Management**: All credentials (encryption_key, passwords, wifi) are substitutions—never hardcode in main.yaml
4. **SPI Bus Shared**: MISO (GPIO16) and CLK (GPIO17) are shared for temperature sensor; MOSI not used (sensor read-only)

## Common Tasks

- **Extending for new sensors**: Use YAML `!extend` anchor on sensor ID and override specific fields (see `example-instance-with-different-sensors.yaml`)
- **Adding relays**: Append GPIO-mapped switches with sequential numbering; update README.md pinout table
- **CI/Build**: GitHub Actions runs ESPHome build on `example-instance.yaml` and publishes `.ota.bin` and `.factory.bin` artifacts on tagged releases

## Critical Files
- [main.yaml](../main.yaml) — The reusable package (substitutions, relays, sensors, WiFi/OTA config)
- [example-instance.yaml](../example-instance.yaml) — Minimal consumer config (override secrets and name)
- [.github/workflows/build.yaml](../.github/workflows/build.yaml) — CI that validates example config on every push/tag
- [README.md](../README.md) — Bill of materials, pinout, first-install guide

## Known Constraints & TODOs
- No local fallback control when HA connection drops (see TODO in README)
- LED control for WiFi status not implemented
- GPIO12 strapping pin warnings may appear—document reason in comments
