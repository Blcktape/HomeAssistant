# Alarmo UniFi Protect G4 Fingerprint Disarm Blueprint

## Version

**2026-01-07**

## Overview

This blueprint allows you to **disarm your Alarmo alarm panel** using **fingerprint scans from a UniFi Protect G4 Doorbell**. It integrates with a sensor that contains authorized user data (including names and disarm codes) and provides secure, scalable fingerprint-based disarming.

## Features

- **Per-user authorization**: Uses a sensor attribute (`users`) with JSON mapping ULP IDs to user details.
- **Per-user disarm codes**: Each authorized user can have their own Alarmo code.
- **Defensive checks**: Ensures valid fingerprint events and prevents false triggers.
- **Notifications**: Sends success or failure alerts to your chosen notify service.
- **Audit logging**: Records all disarm attempts in the Home Assistant logbook.

## Inputs

- **Authorized Users Sensor**: A sensor with a `users` attribute containing JSON like:

```json
{
  "abc123xx-xxxx-xxxx-xxxx-xxxxxxxxxxxx": {
    "name": "John Doe",
    "code": "1234"
  },
  "abc456xx-xxxx-xxxx-xxxx-xxxxxxxxxxxx": {
    "name": "Jane Doe",
    "code": "5678"
  }
}
```

- **Fingerprint Event Entity**: The UniFi Protect G4 Doorbell fingerprint event entity (e.g., `event.camera_doorbell_fingerprint`).
- **Alarmo Panel**: Your `alarm_control_panel` entity for Alarmo.
- **Notify Service**: Service for sending notifications (e.g., `notify.mobile_app_your_phone`).

## Example Configuration

### secrets.yaml

```yaml
alarmo_authorized_users_json: >-
  {
    "abc123xx-xxxx-xxxx-xxxx-xxxxxxxxxxxx": {
      "name": "John Doe",
      "code": "1234"
    },
    "abc456xx-xxxx-xxxx-xxxx-xxxxxxxxxxxx": {
      "name": "Jane Doe",
      "code": "5678"
    }
  }
```

### configuration.yaml

```yaml
template:
  - sensor:
      - name: alarmo_authorized_users
        state: "loaded"
        attributes:
          users: !secret alarmo_authorized_users_json
```

## How It Works

1. **Trigger**: Listens for a fingerprint scan event from the UniFi Protect G4 Doorbell.
2. **Validation**: Confirms the event is recent, identified, and includes a ULP ID.
3. **Authorization**: Checks if the scanned ULP ID exists in the authorized users JSON.
4. **Action**:
    - If authorized: Disarms Alarmo using the user’s code, sends a success notification, and logs the event.
    - If unauthorized: Sends a warning notification and logs the attempt.

## Notes

- Ensure the authorized users sensor is correctly configured and contains valid JSON.
- Verify your notify service name under **Developer Tools → Services**.
- Extendable: You can add multiple notify services or additional actions if needed.

---

## Import Blueprint

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/Blcktape/HomeAssistant/blob/main/Blueprints/alarmo_unifi_protect_g4_fingerprint_disarm_blueprint.yaml)

**Next Steps**: Click the button above to import the blueprint into Home Assistant, select your entities, and enjoy secure fingerprint-based disarming!
