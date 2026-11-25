# Home Assistant study guide

You can learn the main ideas before the server exists. Each idea below uses your own two devices as the example.

## The five ideas

### 1. Integration

An integration is a connection between Home Assistant and a kind of device or service. Zigbee devices connect through the Zigbee Home Automation (ZHA) integration. Z-Wave devices connect through the Z-Wave integration. You add integrations under Settings, then Devices and services.

### 2. Device

A device is the physical thing. Your Shelly H&T sensor will show up as one device after you pair it.

### 3. Entity

An entity is one value or control that a device exposes. One device usually has several entities. The H&T sensor should expose temperature, humidity and battery level as separate entities. Your Shelly Wave Plug US should expose an on/off switch plus power (watts) and energy (kWh) readings. Automations and dashboards use entities, not devices.

### 4. Area

An area is a room. Assign each device to an area, and you can then say "turn off everything in the kitchen."

### 5. Automation

An automation has three parts: a trigger (what starts it), conditions (optional checks), and actions (what to do).

Example for your plug and a future tablet: when the tablet's battery drops below 40%, switch the plug on. When it reaches 80%, switch the plug off. This protects the tablet's battery.

```yaml
alias: Tablet charge on
triggers:
  - trigger: numeric_state
    entity_id: sensor.kitchen_tablet_01_battery_level
    below: 40
actions:
  - action: switch.turn_on
    target:
      entity_id: switch.kitchen_plug_01
```

Only use the plug for loads it supports, such as a tablet charger or a lamp. Never use it with an EV charger (see `docs/devices.md`).

Home Assistant's YAML syntax changes between versions, so check the documentation for your version before you copy an example. The entity names above are placeholders that follow `docs/naming-scheme.md`.

## Other things you will meet

- **Dashboards:** screens made of cards that show entities and buttons. Your control tablet will show one.
- **Helpers:** extra switches, numbers and timers you create yourself, such as a "guest mode" toggle.
- **Notifications:** messages sent to your phone or a dashboard. Keep them local, to match your no-outside-access goal.
- **Backups and updates:** Home Assistant makes its own backups and releases updates often. Read release notes before updating.

## Suggested reading order

Read the official Home Assistant documentation in this order: getting started, integrations, devices and entities, areas, automation basics, then dashboards. Use the search box on the documentation site and look for the page for each of your two devices.

## Self-quiz

1. What is the difference between a device and an entity?
2. Which integration will talk to the Shelly H&T sensor?
3. What are the three parts of an automation?
4. Why do you use underscores and lowercase in names?
5. What goes in `secrets.yaml`, and why is it not committed?
6. What would you do before changing a working automation?

### Answers

1. A device is the physical thing. An entity is one value or control it exposes, like temperature or battery level.
2. Zigbee Home Automation (ZHA), through a Zigbee stick.
3. Trigger, conditions (optional) and actions.
4. So entity IDs stay consistent and nothing breaks when you refer to them in YAML.
5. Passwords, keys and other secrets. It is not committed so they never reach GitHub.
6. Commit the current version to git, or take a Home Assistant backup, so you can go back.
