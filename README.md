# Home Assistant Grafana Dashboards

A collection of Grafana dashboards for monitoring a smart home energy setup in the Netherlands. Built on top of Home Assistant with InfluxDB 2.7 as the time series backend, running on a Synology NAS.

## Stack

| Component | Version | Location |
|-----------|---------|----------|
| Home Assistant | latest | Dedicated host |
| InfluxDB | 2.7 | Synology NAS (Docker) |
| Grafana | 12.4+ | Synology NAS (Docker) |

### Docker Compose

```yaml
services:
  influxdb:
    image: influxdb:2.7
    ports:
      - "8086:8086"
    volumes:
      - /volume2/docker/monitor/influxdb/data:/var/lib/influxdb2
      - /volume2/docker/monitor/influxdb/config:/etc/influxdb2
    environment:
      - DOCKER_INFLUXDB_INIT_MODE=setup
      - DOCKER_INFLUXDB_INIT_USERNAME=admin
      - DOCKER_INFLUXDB_INIT_PASSWORD=yourpassword
      - DOCKER_INFLUXDB_INIT_ORG=homelab
      - DOCKER_INFLUXDB_INIT_BUCKET=homeassistant

  grafana:
    image: grafana/grafana:latest
    user: "472:472"
    ports:
      - "3060:3000"
    volumes:
      - /volume2/docker/monitor/grafana:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=yourpassword
    depends_on:
      - influxdb
```

### Home Assistant — InfluxDB integration

```yaml
# configuration.yaml
influxdb:
  api_version: 2
  host: 192.168.x.x
  port: 8086
  token: !secret influxdb_token
  organization: homelab
  bucket: homeassistant
```

### Grafana datasource

Add a datasource in Grafana → Connections → Data Sources:

- **Name:** `influxdb-flux`
- **Query language:** Flux
- **URL:** `http://192.168.x.x:8086`
- **Organization:** `homelab`
- **Token:** your InfluxDB API token
- **Default bucket:** `homeassistant`

All dashboards in this repo use the **Flux** query language.

---

## Dashboards

| Dashboard | Description |
|-----------|-------------|
| [Battery HA-Velp](battery/) | Battery storage monitoring — Marstek Venus + 3× Sessy + solar + P1 meter + dynamic pricing + revenue |
| [Quatt Energy & Costs](quatt-energy/) | Heat pump energy consumption, self-sufficiency vs grid import, boiler vs heat pump, gas usage |
| [Quatt Performance](quatt-performance/) | COP analysis, temperature curves, delta T, flowrate, COP vs outdoor temperature scatter plot |

---

## Hardware

| Device | Type | Capacity / Spec |
|--------|------|-----------------|
| Marstek Venus E | Home battery | 5.12 kWh usable |
| Sessy DN4T | Home battery | 5.2 kWh usable |
| Sessy D9AF | Home battery | 5.2 kWh usable |
| Sessy DX3J | Home battery | 5.2 kWh usable |
| Enphase Envoy | Solar inverter | ~6 kWp |
| Quatt Hybrid Duo | Heat pump | 2× unit, hybrid with gas boiler |
| P1 meter | Smart meter | DSMR |
| Frank Energie | Energy contract | EPEX spot dynamic pricing |

**Total battery capacity: 20.72 kWh**

---

## General Notes

- All queries use `v.timeRangeStart`, `v.timeRangeStop` and `v.windowPeriod` — they scale automatically with the dashboard time range selector
- InfluxDB writes data from Home Assistant continuously; only state changes are written by default
- Sessy batteries operate in **eco mode** (solar surplus charging) — revenue calculations use Frank Energie EPEX spot prices
- Marstek Venus charges exclusively on solar surplus, no grid charging
- Revenue calculations: `kWh discharged × spot price per 5-minute interval`, cumulative sum over selected time range

---

## Author

**Erik Westrum** — [@ewestrum](https://github.com/ewestrum)
