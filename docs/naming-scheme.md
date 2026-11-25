# Naming scheme

Pick names once and keep them. Renaming entities later breaks dashboards and automations.

## Rules

- Lowercase, words separated by underscores: `living_room_temp_01`.
- Pattern: `<room>_<thing>_<number>`.
- The number starts at 01, even if there is only one, so a second one never forces a rename.
- No spaces, no capital letters, no dates in names.

## Rooms (edit this list)

`living_room`, `kitchen`, `bedroom`, `office`, `bathroom`, `garage`, `hallway`, `basement`, `outside`

## Things

| Thing | Name part | Example |
|---|---|---|
| Temperature and humidity sensor | `temp` | `office_temp_01` |
| Smart plug | `plug` | `kitchen_plug_01` |
| Door or window sensor | `door` or `window` | `garage_door_01` |
| Motion sensor | `motion` | `hallway_motion_01` |
| Light | `light` | `living_room_light_01` |
| Tablet or display | `tablet`, `display` | `kitchen_tablet_01` |

## Home Assistant

- **Device name** uses the pattern above, such as `office_temp_01`. Home Assistant builds entity IDs from it, for example `sensor.office_temp_01_temperature`.
- **Areas** use the room list, with the same spelling in friendly form: "Living room".
- **Friendly names** can be plain English for dashboards: "Office temperature".

## Servers and VMs

| Item | Name |
|---|---|
| Proxmox host | `pve01` |
| Pi-hole container | `pihole` |
| Home Assistant VM | `homeassistant` |
| Immich VM | `immich` |
| Forgejo container | `forgejo` |
| Jellyfin container or VM | `jellyfin` |
| Test VM | `test-` plus a short label |

Proxmox IDs: 100s for infrastructure, 200s for apps, 900s for tests.

## Git

- Commit messages start with a verb: "Add office temp sensor", "Fix kitchen automation".
- One change per commit.
