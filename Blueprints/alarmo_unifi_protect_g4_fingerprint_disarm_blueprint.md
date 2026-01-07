# Alarmo UniFi Protect G4 Fingerprint Disarm Blueprint

## Version
Alpha
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


# How to Capture the ULP ID for Each Fingerprint

When a fingerprint scan occurs on your UniFi Protect G4 Doorbell, the integration sends an **event** to Home Assistant. This event contains attributes such as:

- `event_type`: `identified`
- `ulp_id`: The unique fingerprint ID
- `full_name`: The name associated with the fingerprint (if configured in UniFi Protect)

## Steps to Capture ULP ID

1. **Enable Developer Tools in Home Assistant**
   - Go to **Developer Tools → Events**.

2. **Listen for the Fingerprint Event**
   - In the **Listen to events** field, enter:
     ```
     state_changed
     ```
   - Click **Start Listening**.

3. **Trigger a Fingerprint Scan**
   - Scan a fingerprint on your UniFi Protect G4 Doorbell.

4. **Inspect the Event Data**
   - Look for the entity ID of your doorbell fingerprint sensor (e.g., `event.camera_doorbell_fingerprint`).
   - In the `new_state.attributes`, find:
     ```
     ulp_id: abc123xx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
     full_name: John Doe
     event_type: identified
     ```

5. **Copy the ULP ID**
   - Use this `ulp_id` in your authorized users JSON:
     ```json
     {
       "abc123xx-xxxx-xxxx-xxxx-xxxxxxxxxxxx": {
         "name": "John Doe",
         "code": "1234"
       }
     }
     ```

## Tip
- Repeat this process for each fingerprint you want to authorize.
- Store all ULP IDs in your `secrets.yaml` or template sensor as shown in the blueprint guide.

---

## Notes

- Ensure the authorized users sensor is correctly configured and contains valid JSON.
- Verify your notify service name under **Developer Tools → Services**.
- Extendable: You can add multiple notify services or additional actions if needed.

---

## Import Blueprint

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/Blcktape/HomeAssistant/blob/main/Blueprints/alarmo_unifi_protect_g4_fingerprint_disarm_blueprint.yaml)

**Next Steps**: Click the button above to import the blueprint into Home Assistant, select your entities, and enjoy secure fingerprint-based disarming!
