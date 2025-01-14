
# Home Assistant Energy Tariff Sensors

## Description

This repository contains the configuration for **Home Assistant** sensors that dynamically determine the current electricity tariff (day or night) and the time remaining until the next tariff change. These sensors can be seamlessly integrated into your dashboard, providing real-time updates.

---

## Contents

- **`configuration.yaml`**:
  - Contains a reference to an external folder with sensor files:
    ```yaml
    sensor: !include_dir_merge_list sensors/
    ```

- **`sensors/` folder**:
  - Contains `energy_tariff_sensors.yaml`, which defines two sensors:
    1. **`sensor.energy_tariff`** – Identifies the current tariff (`Night` or `Day`) based on the time, day of the week, and public holidays.
    2. **`sensor.time_to_tariff_change`** – Calculates the time remaining until the next tariff change (e.g., "4h 30m").

---

## Features

### 1. **`sensor.energy_tariff`**
- Displays the current tariff as `Night` or `Day`.
- Takes into account:
  - Night hours: 22:00–6:00.
  - Midday night tariff on weekdays: 13:00–15:00.
  - Weekends and public holidays (e.g., January 1, August 15).
- Automatically changes the icon based on the tariff:
  - Night tariff: `mdi:moon-waning-crescent`.
  - Day tariff: `mdi:weather-sunny`.

### 2. **`sensor.time_to_tariff_change`**
- Calculates the time remaining until the next tariff change:
  - From night to day: at 6:00 or 15:00.
  - From day to night: at 22:00 or 13:00.
- Displays the result in the format `Xh Ym` (e.g., "2h 15m").

---

## How to Use

1. **Copy the files** to your Home Assistant configuration folder:
   - Place `configuration.yaml` in the root directory.
   - Create a folder named `sensors` and add the `energy_tariff_sensors.yaml` file inside it.

2. **Validate the configuration**:
   - In Home Assistant, go to **Settings → System → Check Configuration**.

3. **Restart Home Assistant**:
   - Go to **Settings → System → Restart**.

4. **Add the sensors to your dashboard**:
   - `sensor.energy_tariff` – Displays the current tariff.
   - `sensor.time_to_tariff_change` – Displays the time until the next tariff change.

---

## Dashboard Examples

### Entity Card

```yaml
type: entities
entities:
  - entity: sensor.energy_tariff
    name: Current Energy Tariff
  - entity: sensor.time_to_tariff_change
    name: Time to Tariff Change
```

### Dynamic Markdown Card

```yaml
type: markdown
content: >
  {% if states('sensor.energy_tariff') == 'Night' %}
    **Night tariff is active** 🌙  
    Time until day tariff: {{ states('sensor.time_to_tariff_change') }}
  {% else %}
    **Day tariff is active** ☀️  
    Time until night tariff: {{ states('sensor.time_to_tariff_change') }}
  {% endif %}
```

---

## Requirements

- Installed **Home Assistant**.
- Basic knowledge of YAML configuration.

---

## Contribution

If you have ideas for improvements or encounter any issues, feel free to open an **Issue** or submit a **Pull Request**.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
