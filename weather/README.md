# Weather Station

Dashboard for the Ecowitt WS2900 weather station, showing temperature, wind, precipitation, solar radiation and air pressure.

## Panels

### Temperature & Comfort
- Outdoor temperature (°C) — orange
- Feels like temperature (°C) — yellow
- Dew point (°C) — blue
- Windchill (°C) — light blue
- Indoor temperature (°C) — green, on right Y-axis

### Wind
- Wind speed (km/h) — green with fill
- Wind gusts (km/h) — orange with fill
- Max daily gust (km/h) — red dashed line

Thresholds: 50 km/h (strong), 75 km/h (hard), 100 km/h (storm).

### Rain
- Hourly rain rate (mm)
- Daily cumulative rain (mm) — on right Y-axis
- Rain intensity (mm/h)

### Solar & UV
- Solar radiation (W/m²) — yellow/orange with fill
- Solar lux (lx) — on right Y-axis
- UV index — on right Y-axis with thresholds (3=moderate, 6=high, 8=very high, 11=extreme)

### Air Pressure
- Absolute pressure (hPa)
- Relative pressure (hPa)

Color coded by weather tendency:
- 🔴 Red: < 1000 hPa (storm system)
- 🟠 Orange: 1000-1013 hPa (unsettled)
- 🟡 Yellow: 1013-1020 hPa (variable)
- 🟢 Green: > 1020 hPa (high pressure, good weather)

## Required Sensors

| Sensor | Integration |
|--------|-------------|
| `sensor.ws2900_v2_01_18_outdoor_temperature` | Ecowitt |
| `sensor.ws2900_v2_01_18_feels_like_temperature` | Ecowitt |
| `sensor.ws2900_v2_01_18_dewpoint` | Ecowitt |
| `sensor.ws2900_v2_01_18_windchill` | Ecowitt |
| `sensor.ws2900_v2_01_18_indoor_temperature` | Ecowitt |
| `sensor.ws2900_v2_01_18_humidity` | Ecowitt |
| `sensor.ws2900_v2_01_18_indoor_humidity` | Ecowitt |
| `sensor.ws2900_v2_01_18_wind_speed` | Ecowitt |
| `sensor.ws2900_v2_01_18_wind_gust` | Ecowitt |
| `sensor.ws2900_v2_01_18_max_daily_gust` | Ecowitt |
| `sensor.ws2900_v2_01_18_hourly_rain_rate` | Ecowitt |
| `sensor.ws2900_v2_01_18_daily_rain_rate` | Ecowitt |
| `sensor.ws2900_v2_01_18_rain_rate` | Ecowitt |
| `sensor.ws2900_v2_01_18_solar_radiation` | Ecowitt |
| `sensor.ws2900_v2_01_18_solar_lux` | Ecowitt |
| `sensor.ws2900_v2_01_18_uv_index` | Ecowitt |
| `sensor.ws2900_v2_01_18_absolute_pressure` | Ecowitt |
| `sensor.ws2900_v2_01_18_relative_pressure` | Ecowitt |

## Setup Notes

The WS2900 sensor IDs contain the device MAC address (`v2_01_18` in this case). Replace this with your own device ID if it differs.

Solar radiation data from this station can be correlated with Enphase solar production to calculate panel efficiency per W/m² of irradiance — useful for detecting panel degradation or soiling over time.
