# Bosch Home Connect Dryer — Finish Notifications, Reminders & Maintenance Counter

## Import Blueprint

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import?blueprint_url=https://raw.githubusercontent.com/Blcktape/HomeAssistant/main/Blueprints/bosch_home_connect_dryer_reminders_and_service/bosch_home_connect_dryer_reminders_and_service.yaml)

Blueprint URL:
https://raw.githubusercontent.com/Blcktape/HomeAssistant/main/Blueprints/bosch_home_connect_dryer_reminders_and_service/bosch_home_connect_dryer_reminders_and_service.yaml

---

## Overview

This Home Assistant blueprint enhances Bosch/Siemens dryers connected through the Home Connect integration.  
It provides finished notifications, repeated reminders if laundry is not removed, and a built-in maintenance counter.

---

## Key Features

- Notification when the dryer program finishes
- Configurable hysteresis to avoid false triggers
- Repeating reminders until the dryer door is opened
- Snooze and stop reminder actions via mobile notifications
- Service / maintenance cycle counter
- Optional tracking of last maintenance date

---

## Required Integrations

- Home Connect integration
- Home Assistant notification services (supporting actions)
- Input helpers:
  - input_number for service counter
  - optional input_datetime for maintenance date

---

## How It Works

1. Detects when the dryer reaches the *finished* state
2. Waits for a configurable hysteresis time
3. Sends a finish notification
4. Repeats reminders while the door remains closed
5. Tracks completed cycles
6. Sends maintenance reminders when threshold is reached
7. Allows reset via notification action

---

## Inputs Explained

| Input | Description |
|------|-------------|
| Dryer name | Friendly name used in notifications |
| Operation state | Home Connect dryer state sensor |
| Door state | Sensor indicating door open/closed |
| Notify services | One or more notify targets |
| Finish hysteresis | Minutes before notification |
| Reminder interval | Minutes between reminders |
| Snooze duration | Snooze time in minutes |
| Service counter | input_number helper |
| Maintenance threshold | Cycles before reminder |
| Maintenance date | Optional input_datetime |

---

## Installation

1. Import the blueprint using the button above
2. Create a new automation from the blueprint
3. Select the required sensors and helpers
4. Save and enable the automation

---

## Source

GitHub Repository:
https://github.com/Blcktape/HomeAssistant
