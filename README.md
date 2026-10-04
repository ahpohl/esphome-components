# esphome-components

External components for [ESPHome](https://esphome.io).

## xiaomi_mccgq02hl

Xiaomi Mijia Door/Window Sensor 2 (MCCGQ02HL, MiBeacon product id `0x098b`).
Passive BLE, encrypted with a bindkey; no pairing, no effect on the sensor's battery.

Self-contained: it does its own MiBeacon header and object parsing and decrypts
with `ble_device_base::aes_ccm_auth_decrypt()`, so it does not use or shadow core
`xiaomi_ble`. Requires ESPHome 2026.8 or newer (`ble_device_base`).

```yaml
external_components:
  - source: github://ahpohl/esphome-components
    components: [xiaomi_mccgq02hl]

esp32_ble_tracker:

binary_sensor:
  - platform: xiaomi_mccgq02hl
    name: "Front-Door"             # opening state (device_class: opening)
    mac_address: "E4:AA:EC:00:00:00"
    bindkey: "00112233445566778899aabbccddeeff"
    light:                         # optional, light above threshold
      name: "Front-Door Light"
    battery_level:                 # optional, %
      name: "Front-Door Battery-Level"
```

Door object values: `0` open, `1` closed, `2` left open past the device's
timeout (reported as open), `3` device reset (ignored).
Unencrypted frames are ignored, since the bindkey is required.

Submitted upstream as esphome/esphome#20104 (superseding #4605), docs in
esphome/esphome.io#7510. Once that is released, drop the `external_components:` entry.
