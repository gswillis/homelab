# Device inventory

One row per device. Keep this up to date as you add, move or retire things.

Do not put Z-Wave PINs, QR codes or DSK strings here. Store those in the password manager.

## Inventory

| Planned name | Product | ID | Protocol | Power | Room | Status | Notes |
|---|---|---|---|---|---|---|---|
| `room_temp_01` | Shelly BLU H&T ZB (ivory) | EAN 3800238072414 | Zigbee and Bluetooth LE | Battery, 3 V | TBD | Not paired | Needs a Zigbee stick |
| `room_plug_01` | Shelly Wave Plug US | FCC ID 28DC6-WAVEPLUGUS | Z-Wave 800 series, 908.4 MHz | Mains, 120 V | TBD | Not paired | Needs a US Z-Wave stick. Also repeats the Z-Wave signal |

Status values: Not paired, Paired, Retired.

## Sensor 1: Shelly BLU H&T ZB

- Identified from barcode 3800238072414. Retail listings give the model code `SHELLY-BLU-HT-GEN3-ZB-IVORY` and describe it as a Bluetooth and Zigbee temperature and humidity sensor.
- Plan to use it over Zigbee through Home Assistant's built-in Zigbee Home Automation (ZHA) integration once the Zigbee stick is installed.
- Check Shelly's manual for the exact pairing button sequence when you pair it.
- Battery powered, so add the battery type and replacement date here once you check the back.

## Sensor 2: Shelly Wave Plug US

- Identified from FCC ID 28DC6-WAVEPLUGUS. Shelly's documentation lists the US part number `QNPL-001X16US`.
- Z-Wave 800 series chip, frequency 908.4 MHz. This matches a US Z-Wave stick. EU (868.4 MHz) and Australia/New Zealand (919.8 MHz) versions would not.
- Plug and socket are NEMA 5-15 (standard US 3-prong). Retail listings give a rating of up to 15 A and 1800 W. Check the label on your unit.
- Power metering: it measures power (watts) and energy (kWh).
- It has overheat and over-current protection, and it acts as a Z-Wave repeater, so place it somewhere central to strengthen the Z-Wave network.
- Security: supports S2. For S2 pairing, Home Assistant asks for the **5-digit PIN** printed on the Z-Wave DSK label on the side of the device and on a label inside the packaging. Shelly warns the PIN must not be lost. Photograph it into the password manager.
- SmartStart pairing is possible by scanning the Z-Wave QR code. Do not post the QR code or PIN anywhere.
- There is a separate "Wave Plug US LR" model. Your FCC ID points to the standard one.

### Safety: what to plug into it

- Shelly lists supported load types as resistive (heating devices, incandescent bulbs) and capacitive (electronic equipment).
- **Do not use it with an EV charger.** GM's instructions say not to use wall adapters or extension cords of any kind with its dual level charge cord, and for both charging levels not to use power strips, splitters, grounding adapters, surge protectors or similar devices. A smart plug is an adapter between the outlet and the cord. EV charging is also a long, continuous high-current load.
- To schedule EV charging, use the vehicle's own charging schedule or its app instead, or ask an electrician about a charger that supports scheduling.
- Good uses: lamps, a tablet charger (see the tablet battery idea in the study guide), a fan, a small appliance within its rating.

## Pairing notes

- Pair each device once, directly to Home Assistant, after the radio stick is installed.
- If a device was paired to another controller before, factory reset it first.

## Other devices

| Device | Model and year | Connection | Notes |
|---|---|---|---|
| Samsung Smart TV | | Prefer Ethernet | The only Jellyfin client. Check the model year for the Jellyfin app. Also runs the streaming apps (Netflix, Hulu, HBO Max, Prime, Disney+, YouTube, YouTube TV, Paramount) |
