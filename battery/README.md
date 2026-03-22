# Battery - HA-Velp

Overview dashboard for the complete home battery setup, combining solar production, battery status, energy prices and revenue tracking.

## Panels

### Total Power (Stat)
Live status of all batteries:
- Marstek Venus SOC (%)
- Marstek Venus stored energy (kWh)
- Sessy DN4T SOC (%)
- Sessy D9AF SOC (%)
- Sessy DX3J SOC (%)
- Total stored energy across all batteries (kWh)

Color coded: red = low (<20%), orange = medium, green = good (>50%).

### Revenue Generated Marstek
Cumulative daily revenue from the Marstek battery in euros. Calculated as:
`discharged kWh × Frank Energie spot price`

Only discharge moments are counted (the battery charges exclusively on solar, so no charging costs).

### Sessy Opbrengst
Cumulative daily revenue from the three Sessy batteries combined, calculated the same way as Marstek. Note: only valid when Sessy is in Eco mode (solar charging). Revenue from imbalance market trading via Frank Energie is not reflected here.

### Frank Stroom Prijzen
Bar chart of electricity prices (€/kWh) color coded by price level:
- 🔵 Blue: negative price
- 🟢 Green: < €0.28
- 🟡 Yellow: €0.28 - €0.32
- 🟠 Orange: €0.32 - €0.37
- 🔴 Red: > €0.37

### Marstek / Solar Power
Time series showing:
- Solar production (W) — from Enphase Envoy
- Marstek battery power (W) — positive = charging, negative = discharging
- Marstek SOC (%) — on right Y-axis (0-100%)

### Total Power (P1 Meter)
Net grid consumption per phase:
- Fase 1, 2, 3 (W)
- Total (W)

Negative values = feeding back to grid.

### Sessy Batteries
Power flow for all three Sessy units combined with solar production overlay.

### Sessy State of Charge
SOC (%) for all three Sessy units over time.

## Required Sensors

| Sensor | Integration |
|--------|-------------|
| `sensor.marstek_venus_modbus_stored_energy` | Marstek Venus Modbus |
| `sensor.marstek_venus_modbus_battery_power` | Marstek Venus Modbus |
| `sensor.sessy_dn4t_power` | Sessy |
| `sensor.sessy_d9af_power` | Sessy |
| `sensor.sessy_dx3j_power` | Sessy |
| `sensor.sessy_dn4t_state_of_charge` | Sessy |
| `sensor.sessy_d9af_state_of_charge` | Sessy |
| `sensor.sessy_dx3j_state_of_charge` | Sessy |
| `sensor.envoy_122149071873_current_power_production` | Enphase Envoy |
| `sensor.p1_meter_3c39e72bdf42_active_power` | DSMR/P1 |
| `sensor.p1_meter_3c39e72bdf42_active_power_l1` | DSMR/P1 |
| `sensor.p1_meter_3c39e72bdf42_active_power_l2` | DSMR/P1 |
| `sensor.p1_meter_3c39e72bdf42_active_power_l3` | DSMR/P1 |
| `sensor.frank_energie_prices_current_electricity_price_all_in` | Frank Energie |
| `sensor.totaal_opgeslagen_energie` | Template sensor (see below) |

## Template Sensor

Add to `configuration.yaml` for the total stored energy calculation:

```yaml
template:
  - sensor:
      - name: "Totaal Opgeslagen Energie"
        unit_of_measurement: "kWh"
        state: >
          {% set marstek = states('sensor.marstek_venus_modbus_stored_energy') | float(0) %}
          {% set dn4t = states('sensor.sessy_dn4t_state_of_charge') | float(0) / 100 * 5.2 %}
          {% set d9af = states('sensor.sessy_d9af_state_of_charge') | float(0) / 100 * 5.2 %}
          {% set dx3j = states('sensor.sessy_dx3j_state_of_charge') | float(0) / 100 * 5.2 %}
          {{ (marstek + dn4t + d9af + dx3j) | round(2) }}
```

## Battery Specifications

| Battery | Capacity | Notes |
|---------|----------|-------|
| Marstek Venus | 5.12 kWh | Modbus RS485, charges on solar only |
| Sessy DN4T | 5.2 kWh | AC-coupled, Frank Energie integration |
| Sessy D9AF | 5.2 kWh | AC-coupled, Frank Energie integration |
| Sessy DX3J | 5.2 kWh | AC-coupled, Frank Energie integration |
| **Total** | **20.72 kWh** | |
