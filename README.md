# HJDC (H)ome Assistant - (J)K - (D)eye - (C)ontroller

![GitHub stars](https://img.shields.io/github/stars/Ackmaniac/esphome-jk-bms-can)
![GitHub forks](https://img.shields.io/github/forks/Ackmaniac/esphome-jk-bms-can)
![GitHub watchers](https://img.shields.io/github/watchers/Ackmaniac/esphome-jk-bms-can)

## This fork supports CAN bus communication with inverters supporting the CANBUS Protocol compatible with Pylontech V1.3 and Goodwe V1.5. Additionaly values of Deye inverters are read via RS485 via Modbus. All values are send to Home Assistant.

Note Pylontech uses 15s/48v Goodwe uses 16s/51.2v @3.2v/cell nominal.
Other battery profiles that utilise the pylonton/goodwe protocol with differnt cell counts may also work, eg Alpha Ess Smile, BYD Battery-Box LV Flex Lite 
Select the correct battery profile in the inverter to match your battery pack!

The ESP32 communicates with the JK-BMS using the RS485 port(GPS) which is in fact not RS485, it is 3.3V TTL so it can be directly connected to the ESP32.
The ESP32 then sends the required CAN bus data to the inverter via a CAN bus transceiver.

To read the values from the Inverter the ESP32 communicates via RS485 transreceiver.

### Sends over CAN bus to inverter:
  - Battery Voltage
  - Battery Current (+charge, -discharge)
  - State of Charge (SOC)
  - State of health (SOH)
  - BMS Temperature
  - Charging Voltage
  - Charging Amps
  - Discharge min Voltage
  - Battery name
  - Alarms: Cell over/under voltage, Charge/discharge over current, High/low Temp, BMS fault
  - Charging logic: Complete rework of the charging logic, now charges with constant current(CC) to the absorption voltage, then has an absorption timer (Constant Voltage, user configurable time), with rebulk feature (user configurable offset from absorption voltage).
  
## Home Assistant native integration via ESPHome:

<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/HJDC-settings.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/HJDC-settings.jpg?raw=true" width="33%"></a>
- Controls to manage inverter remotely
  - Set "Balance Voltage" to tell inverter to only charge till this voltage
  - Set "Min Discharge Voltage" to tell inverter stop discharging below this voltage
  - Set "Charging Current"
  - Set "Discharging Current"
  - Disable/Enable inverter charging
  - Disable/Enable inverter discharging
  - Disable/Enable top balance charging
  - Set "Days Bewteen Balancing" to only balance every x days, in between it will stop charging when "Max Charge %" is reached
  - Set "Max Charge %" for days in between top balance cycles (on top balance days it will charge till "Balance Voltage")
  - Set "Min Discharge %" to stop discharging

## Supported devices

Inverters supporting CAN Pylon/Goodwe Low Voltage protocol should work, check your inverter manual to confirm.

The following are confirmed and known to work:
* Deye SUN-12K-SG04LP3-EU

<br>All JK-BMS models with software version `>=6.0` are using the implemented protocol and should be supported.

* JK-BD6A17S6P, hw 7.2, sw 7.1.0H
* JK-BD6A17S8P, hw 9.x, sw 9.01G (reported by [@jonadis](https://github.com/syssi/esphome-jk-bms/issues/35#issuecomment-1035312712))
* JK-BD6A20S10P, hw 10.XW, sw 10.07 (reported by [@adadrag](https://github.com/syssi/esphome-jk-bms/issues/123))
* JK-BD6A24S6P, hw 6.x, sw 6.10S (reported by [@ziporah](https://github.com/syssi/esphome-jk-bms/issues/41))
* JK-BD6A24S10P, hw 8.x, sw 8.0.6G (reported by [@spoonwzd](https://github.com/syssi/esphome-jk-bms/issues/67#issuecomment-1093844076))
* JK-BD4A17S4P, hw 11.xw, sw 11.01 (reported by [@Condor-XYZ](https://github.com/syssi/esphome-jk-bms/issues/221))
* JK-B1A24S15P, hw 8.x, sw 8.1.0H (reported by [@killee](https://github.com/syssi/esphome-jk-bms/discussions/4))
* JK-B1A20S15P, hw 8.x, sw 8.14U (reported by  [@trippfam07](https://github.com/syssi/esphome-jk-bms/issues/31))
* JK-B2A24S15P, hw 6.x, sw 6.1.3S (reported by [@miguel300477](https://github.com/syssi/esphome-jk-bms/issues/57))
* JK-B2A24S15P, hw 8.x, sw 8.21W (reported by [@mariusvaida](https://github.com/syssi/esphome-jk-bms/issues/120))
* JK-B2A24S15P, hw 10.xw, sw 10.07
* JK-B2A24S15P, hw 10.xw, sw 10.08 (reported by [@meccip](https://github.com/syssi/esphome-jk-bms/discussions/175#discussioncomment-3687287))
* JK-B2A24S20P, hw 8.x, sw 8.1.2H (reported by [@KlausLi](https://github.com/syssi/esphome-jk-bms/issues/15#issuecomment-961447064))
* JK-B2A24S20P, hw 8.x, sw 8.20G (reported by [@rob-oravec](https://github.com/syssi/esphome-jk-bms/discussions/46))
* JK-B2A24S20P, hw 10.X-W, sw 10.02 (reported by [@SeByDocKy](https://github.com/syssi/esphome-jk-bms/issues/37#issuecomment-1040569576))
* JK-B2A24S20P, hw 10.XG, sw 10.07D30 (reported by [@TheSmartGerman](https://github.com/syssi/esphome-jk-bms/discussions/122))
* JK-B2A24S20P, hw 10.XW, sw 10.07 (reported by [@amagr0](https://github.com/syssi/esphome-jk-bms/issues/124#issuecomment-1166366196))
* JK-B2A8S20P,  hw 9.x, sw 9.01M3 (reported by [@EasilyBoredEngineer](https://github.com/syssi/esphome-jk-bms/discussions/110))
* JK-B2A8S20P, hw 9.x, sw 9.08W (reported by [@vrabi-cv](https://github.com/syssi/esphome-jk-bms/discussions/144#discussioncomment-3285901))
* JK-B2A8S20P, hw 11.XW, sw 11.17 (reported by [@senfkorn](https://github.com/syssi/esphome-jk-bms/issues/147))
* JK-B2A20S20P, hw 10.XW, sw 10.09 (reported by [@markusgg84](https://github.com/syssi/esphome-jk-bms/discussions/173))
* JK-B5A24S, hw 8.x, sw 8.0.3M, using `JK04` (reported by [@JSladen](https://github.com/syssi/esphome-jk-bms/issues/213))
* GW-24S4EB (NEEY/Heltec 4A Smart Active Balancer), hw HW-2.8.0, sw ZH-1.2.3 (reported by [@cristi2005](https://github.com/syssi/esphome-jk-bms/issues/109))

## Requirements

* [ESPHome 2022.11.0 or higher](https://github.com/esphome/esphome/releases).
* Optimized for ESP32-S2 because of lower power consumption but works with Generic ESP32 as well
* TJA1050 CAN controller interface module and 4.7K resistor for 5v to 3.3v level shifing.
* Optional: JK RS485 Adaptor and RS484 to TTL3.3v Adaptor (see optional schematic below)

## Schematics
I have designed and build a PCB pictured below.
This has all the hardware needed to communicate with BMS and Inverter via CAN bus and Modbus.
It is a plug and play device, just connect the 4 pin headder in the BMS and a LAN cable to the Inverter.

HJDC-PCB<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/HJDC-PCB.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/HJDC-PCB.jpg?raw=true" width="33%"></a>

Deye BMS LAN Port (RJ45)<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/Deye_BMS_Port.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/Deye_BMS_Port.jpg?raw=true" width="33%"></a>


```
ESP32-S2 Wemos Mini

Optional below, as seen in pic above: RS485 between JK-BMS GPS port and ESP32-S2 Wemos Mini, uses JK RS485 adaptor and RS485 to TTL3.3v adaptor.

┌──────────┐                               ┌───────────┐           ┌─────────┐           ┌───────────┐
│          │                               │          7│<TX-----TX>|         |           |           |
│  JK-BMS  │<TX ----------------------- RX>│16        9│<RX-----RX>| TJA1050 |<- CAN H ->|4          |
│ GPS Port │<RX ----------------------- TX>│18         │<-- GND  ->|   CAN   |<- CAN L ->|5          |
│          │          ┌──────────┐         │           │<-- 3.3V ->|         |           |  Inverter |
│       60V│<- VBus ->│  DC/DC   │<- 5V  ->│  ESP32-S2 │           └─────────┘           │    LAN    │
│          │<- GND  ->│ Converter│<- GND ->│   Wemos   │                                 │    BMS    │
└──────────┘          |  60V/5V  |         │   Mini    │           ┌─────────┐           │           │
                      └──────────┘         │         11│<TX-----TX>│         │<-- GND -->│6          │
                                           │         12│<RX-----RX>│  RS485  │<--  A  -->│7          │
                                           │           │<-- GND  ->│  to TTL │<--  B  -->│8          │
                                           │           │<-- 3.3V ->│         │           │           │
                                           └───────────┘           └─────────┘           └───────────┘

Esp32 Dev Kit Board

┌──────────┐                               ┌───────────┐           ┌─────────┐           ┌───────────┐
│          │                               │         23│<TX-----TX>|         |           |           |
│  JK-BMS  │<TX ----------------------- RX>│16       22│<RX-----RX>| TJA1050 |<- CAN H ->|4          |
│          │<RX ----------------------- TX>│17         │<---GND--->|   CAN   |<- CAN L ->|5          |
│          │          ┌──────────┐         │           │<---3.3V-->|         |           |  Inverter |
│       60V│<- VBus ->│  DC/DC   │<- 5V  ->│  ESP32    │           └─────────┘           │    LAN    │
│          │<- GND  ->│ Converter│<- GND ->│  Dev Kit  │                                 │    BMS    │
└──────────┘          |  60V/5V  |         │           │           ┌─────────┐           │           │
                      └──────────┘         │         19│<TX-----TX>│         │<-- GND -->│6          │
                                           │         18│<RX-----RX>│  RS485  │<--  A  -->│7          │
                                           │           │<-- GND  ->│  to TTL │<--  B  -->│8          │
                                           │           │<-- 3.3V ->│         │           │           │
                                           └───────────┘           └─────────┘           └───────────┘


# RS485-TTL jack on JK-BMS (4 Pin, JST 1.25mm pitch)
┌─── ─────── ────┐
│                │
│ O   O   O   O  │
│GND  RX  TX VBAT│ 
└────────────────┘
  │   │   │   | VBAT is full battery volatge eg 55.3V (Used as power supply for 60V/5V DC/DC converter)
  │   │   └──── ESP32-S2 GPIO18 (`rx_pin`)
  │   └──────── ESP32-S2 GPIO16 (`tx_pin`)
  └──────────── GND
```


The RS485-TTL jack of the BMS can be attached to any UART pins of the ESP. A hardware UART should be preferred because of the high baudrate (115200 baud). The connector is called 4 Pin JST with 1.25mm pitch.

## Installation

You can install this component with [ESPHome external components feature](https://esphome.io/components/external_components.html) like this:
```yaml
external_components:
  - source: github://Ackmaniac/esphome-jk-bms-can@main
```

or just use the `esp32-example-can-deye-modbus_espidf.yaml` as proof of concept:

```bash
# Install esphome
pip3 install esphome

# Clone this external component
git clone https://github://Ackmaniac/esphome-jk-bms-can.git
cd esphome-jk-bms-can

# Create a secrets.yaml containing some setup specific secrets
cat > secrets.yaml <<EOF
wifi_ssid: MY_WIFI_SSID
wifi_password: MY_WIFI_PASSWORD

# Validate the configuration, create a binary, upload it, and start logs
esphome run esp32-example-can-deye-modbus_espidf.yaml

# Optional add to Home Assistant
In Home Assistant under settings->Intergration "Add Intergration" select ESPHome add device jk-bms-can if found or supply ip address of ESP32

```
## Example CAN messages

```
[main:141]: send can id: 0x359 hex: 0 0 0 0 3 0 0 0
[canbus:033]: send extended id=0x359 rtr=FALSE size=8
[main:187]: send can id: 0x351 hex: 28 2 e8 3 e8 3 a0 1
[canbus:033]: send extended id=0x351 rtr=FALSE size=8
[main:199]: send can id: 0x355 hex: 4f 0 64 0
[canbus:033]: send extended id=0x355 rtr=FALSE size=4
[main:213]: send can id: 0x356 hex: 63 14 62 fc 86 1
[canbus:033]: send extended id=0x356 rtr=FALSE size=6
[canbus:070]: received can message (#1) std can_id=0x305 size=8
[light:035]: 'Builtin LED' Setting:
[light:046]:   State: OFF
[main:238]: send can id: 0x35C hex: c0 0
[canbus:033]: send extended id=0x35c rtr=FALSE size=2
[canbus:033]: send extended id=0x35e rtr=FALSE size=8
```

## Known issues

* The battery type sensor is pretty useless because the BMS reports always the same value (`Ternary Lithium`). Regardless of which battery type was set / parameter set was loaded via the android app. ([#9][i9])
* ESP32: Adding all supported sensors can lead to a stack overflow / boot loop. This can be solved by increasing the stack size. ([#63][i63])

[i9]: https://github.com/syssi/esphome-jk-bms/issues/9
[i63]: https://github.com/syssi/esphome-jk-bms/issues/63

## Debugging

If this component doesn't work out of the box for your device please update your configuration to enable the debug output of the UART component and increase the log level to the see outgoing and incoming serial traffic:

```
logger:
  level: DEBUG

uart:
  id: uart0
  baud_rate: 115200
  rx_buffer_size: 384
  tx_pin: GPIO14
  rx_pin: GPIO4
  debug:
    direction: BOTH
```

## References
* https://github.com/uksa007/esphome-jk-bms-can.git Thanks go to uksa007 for help and making the original CAN communication code!
* https://github.com/syssi/esphome-jk-bms Thanks go to syssi for help and making the original RS485 code!
* https://secondlifestorage.com/index.php?threads/jk-b1a24s-jk-b2a24s-active-balancer.9591/
* https://github.com/jblance/jkbms
* https://github.com/jblance/mpp-solar/issues/112
* https://github.com/jblance/mpp-solar/blob/master/mppsolar/protocols/jk232.py
* https://github.com/jblance/mpp-solar/blob/master/mppsolar/protocols/jk485.py
* https://github.com/sshoecraft/jktool
* https://github.com/Louisvdw/dbus-serialbattery/blob/master/etc/dbus-serialbattery/jkbms.py
* https://blog.ja-ke.tech/2020/02/07/ltt-power-bms-chinese-protocol.html
* https://blog.ja-ke.tech/2020/02/07/ltt-power-bms-chinese-protocol.html


# Home Assistant intergration

## HJDC Settings<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/HJDC-settings.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/HJDC-settings.jpg?raw=true" width="33%"></a>
```
type: entities
title: HJDC Settings
entities:
  - entity: sensor.hjdc_charging_status
    name: Charging Status
  - entity: number.hjdc_balance_voltage
    name: Balance Voltage
  - entity: number.hjdc_min_discharge_voltage
    name: Min Discharge Voltage
  - entity: number.hjdc_charging_current_max
    name: Charging Current Max
  - entity: number.hjdc_discharging_current_max
    name: Discharging Current Max
  - entity: switch.hjdc_charging_enabled
    name: Charging Enabled
  - entity: switch.hjdc_discharge_enabled
    name: Discharge Enabled
  - entity: switch.hjdc_charging_top_balance
    name: Charging top balance
  - entity: number.hjdc_days_between_balancing
    name: Days Between Balancing
  - entity: sensor.hjdc_next_top_balance
    name: Next Top Balance
  - entity: number.hjdc_max_charge
    name: Max Charge %
  - entity: number.hjdc_min_discharge
    name: Min Discharge %
```

## Send values to Deye<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/send-values.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/send-values.jpg?raw=true" width="33%"></a>
```
type: entities
entities:
  - entity: sensor.hjdc_send_inverter_charge_current
    name:
      type: entity
  - entity: sensor.hjdc_send_inverter_discharge_current
    name:
      type: entity
  - entity: sensor.hjdc_send_inverter_charge_voltage
    name:
      type: entity
  - entity: sensor.hjdc_send_inverter_discharge_voltage
    name:
      type: entity
```

## Phase Power<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/phase-power.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/phase-power.jpg?raw=true" width="33%"></a>
```
show_name: true
show_icon: true
show_state: true
type: glance
columns: 4
entities:
  - entity: sensor.hjdc_deye_internal_grid_l1_power
    name: Internal L1
  - entity: sensor.hjdc_deye_internal_grid_l2_power
    name: Internal L2
  - entity: sensor.hjdc_deye_internal_grid_l2_power
    name: Internal L3
  - entity: sensor.hjdc_deye_internal_grid_total_power
    name: Internal Total
  - entity: sensor.hjdc_deye_external_grid_l1_power
    name: External L1
  - entity: sensor.hjdc_deye_external_grid_l2_power
    name: External L2
  - entity: sensor.hjdc_deye_external_grid_l3_power
    name: External  L3
  - entity: sensor.hjdc_deye_external_grid_total_power
    name: External Total
  - entity: sensor.hjdc_deye_load_l1_power
    name: Load L1
  - entity: sensor.hjdc_deye_load_l2_power
    name: Load L2
  - entity: sensor.hjdc_deye_load_l3_power
    name: Load L3
  - entity: sensor.hjdc_deye_load_total_power
    name: Load Total
```

## Temp<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/temperature.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/temperature.jpg?raw=true" width="33%"></a>
```
show_name: true
show_icon: true
show_state: true
type: glance
entities:
  - entity: sensor.hjdc_jk_temperature_sensor_1
    name: Battery 1
  - entity: sensor.hjdc_jk_temperature_sensor_2
    name: Battery 1
  - entity: sensor.hjdc_jk_power_tube_temperature
    name: BMS
  - entity: sensor.hjdc_jk_temperature_wire
    name: Room
state_color: false
```

## Cell Overview<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/cell-overview.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/cell-overview.jpg?raw=true" width="33%"></a>
```
show_name: true
show_icon: true
show_state: true
type: glance
columns: 6
entities:
  - entity: sensor.hjdc_jk_min_cell_voltage
    name: min
  - entity: sensor.hjdc_jk_max_cell_voltage
    name: max
  - entity: sensor.hjdc_jk_delta_cell_voltage
    name: delta
  - entity: binary_sensor.hjdc_jk_balancing
    name: balance
  - entity: sensor.hjdc_jk_min_voltage_cell
    name: min C
  - entity: sensor.hjdc_jk_max_voltage_cell
    name: max C
```

## Battery Overview<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/battery-overview.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/battery-overview.jpg?raw=true" width="33%"></a>
```
type: glance
columns: 5
entities:
  - entity: sensor.hjdc_jk_power
    name: power
  - entity: sensor.hjdc_jk_current
    name: A
  - entity: sensor.hjdc_jk_total_voltage
    name: V
  - entity: sensor.hjdc_jk_capacity_remaining
    name: SOC
  - entity: sensor.hjdc_jk_capacity_remaining_derived
    name: Ah
```    

## Cells<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/Cells.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/Cells.jpg?raw=true" width="33%"></a>
```
show_name: true
show_icon: false
show_state: true
type: glance
title: Cells
columns: 4
entities:
  - entity: sensor.hjdc_jk_cell_voltage_1
    name: '1'
  - entity: sensor.hjdc_jk_cell_voltage_2
    name: '2'
  - entity: sensor.hjdc_jk_cell_voltage_3
    name: '3'
  - entity: sensor.hjdc_jk_cell_voltage_4
    name: '4'
  - entity: sensor.hjdc_jk_cell_voltage_5
    name: '5'
  - entity: sensor.hjdc_jk_cell_voltage_6
    name: '6'
  - entity: sensor.hjdc_jk_cell_voltage_7
    name: '7'
  - entity: sensor.hjdc_jk_cell_voltage_8
    name: '8'
  - entity: sensor.hjdc_jk_cell_voltage_9
    name: '9'
  - entity: sensor.hjdc_jk_cell_voltage_10
    name: '10'
  - entity: sensor.hjdc_jk_cell_voltage_11
    name: '11'
  - entity: sensor.hjdc_jk_cell_voltage_12
    name: '12'
  - entity: sensor.hjdc_jk_cell_voltage_13
    name: '13'
  - entity: sensor.hjdc_jk_cell_voltage_14
    name: '14'
  - entity: sensor.hjdc_jk_cell_voltage_15
    name: '15'
  - entity: sensor.hjdc_jk_cell_voltage_16
    name: '16'
```

## All Deye Values<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/deye-values.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/deye-values.jpg?raw=true" width="33%"></a>
```
type: entities
title: Deye
entities:
  - entity: sensor.hjdc_deye_heat_sink_temperature
    name: Heat Sink Temperature
  - entity: sensor.hjdc_deye_battery_output_power
    name: Battery Output Power
  - entity: sensor.hjdc_deye_battery_capacity
    name: Battery Capacity
  - entity: sensor.hjdc_deye_battery_output_current
    name: Battery Output Current
  - entity: sensor.hjdc_deye_battery_temperature
    name: Battery Temperature
  - entity: sensor.hjdc_deye_battery_total_charge
    name: Battery Total Charge
  - entity: sensor.hjdc_deye_battery_total_discharge
    name: Battery Total Discharge
  - entity: sensor.hjdc_deye_battery_voltage
    name: Battery Voltage
  - entity: sensor.hjdc_deye_daily_battery_charge
    name: Daily Battery Charge
  - entity: sensor.hjdc_deye_daily_battery_discharge
    name: Daily Battery Discharge
  - entity: sensor.hjdc_deye_daily_energy_bought
    name: Daily Energy Bought
  - entity: sensor.hjdc_deye_daily_energy_sold
    name: Daily Energy Sold
  - entity: sensor.hjdc_deye_daily_load_consumption
    name: Daily Load Consumption
  - entity: sensor.hjdc_deye_daily_production
    name: Daily Production
  - entity: sensor.hjdc_deye_daily_pv1_production
    name: Daily PV1 Production
  - entity: sensor.hjdc_deye_daily_pv2_production
    name: Daily PV2 Production
  - entity: sensor.hjdc_deye_failure_status_of_communication_board
    name: Failure status of communication board
  - entity: sensor.hjdc_deye_grid_current_l1
    name: Grid Current L1
  - entity: sensor.hjdc_deye_grid_current_l2
    name: Grid Current L2
  - entity: sensor.hjdc_deye_grid_current_l3
    name: Grid Current L3
  - entity: sensor.hjdc_deye_grid_voltage_l1
    name: Grid Voltage L1
  - entity: sensor.hjdc_deye_grid_voltage_l2
    name: Grid Voltage L2
  - entity: sensor.hjdc_deye_grid_voltage_l3
    name: Grid Voltage L3
  - entity: sensor.hjdc_deye_external_grid_l1_power
    name: External Grid L1 Power
  - entity: sensor.hjdc_deye_external_grid_l2_power
    name: External Grid L2 Power
  - entity: sensor.hjdc_deye_external_grid_l3_power
    name: External Grid L3 Power
  - entity: sensor.hjdc_deye_external_grid_total_power
    name: External Grid Total Power
  - entity: sensor.hjdc_deye_internal_grid_l1_power
    name: Internal Grid L1 Power
  - entity: sensor.hjdc_deye_internal_grid_l2_power
    name: Internal Grid L2 Power
  - entity: sensor.hjdc_deye_internal_grid_l3_power
    name: Internal Grid L3 Power
  - entity: sensor.hjdc_deye_internal_grid_total_power
    name: Internal Grid Total Power
  - entity: sensor.hjdc_deye_inverter_frequency
    name: Inverter Frequency
  - entity: sensor.hjdc_deye_load_frequency
    name: Load Frequency
  - entity: sensor.hjdc_deye_load_total_power
    name: Load Total Power
  - entity: sensor.hjdc_deye_power_generation_today
    name: Power Generation Today
  - entity: sensor.hjdc_deye_pv1_current
    name: PV1 Current
  - entity: sensor.hjdc_deye_pv1_power
    name: PV1 Power
  - entity: sensor.hjdc_deye_pv1_voltage
    name: PV1 Voltage
  - entity: sensor.hjdc_deye_pv2_current
    name: PV2 Current
  - entity: sensor.hjdc_deye_pv2_power
    name: PV2 Power
  - entity: sensor.hjdc_deye_pv2_voltage
    name: PV2 Voltage
  - entity: sensor.hjdc_deye_pv_total_power
    name: PV Total Power
  - entity: sensor.hjdc_deye_running_status
    name: Running Status
  - entity: sensor.hjdc_deye_total_consumption
    name: Total Consumption
  - entity: sensor.hjdc_deye_total_energy_bought
    name: Total Energy Bought
  - entity: sensor.hjdc_deye_total_energy_sold
    name: Total Energy Sold
  - entity: sensor.hjdc_deye_total_grid_power
    name: Total Grid Power
  - entity: sensor.hjdc_deye_total_pv_production
    name: Total PV Production
  - entity: sensor.hjdc_deye_warning_1
    name: Warning 1
  - entity: sensor.hjdc_deye_warning_2
    name: Warning 2
  - entity: sensor.hjdc_deye_error_1
    name: Error 1
  - entity: sensor.hjdc_deye_error_2
    name: Error 2
  - entity: sensor.hjdc_deye_error_3
    name: Error 3
  - entity: sensor.hjdc_deye_error_4
    name: Error 4
```

## All JK Values<br>
<a href="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/jk-values.jpg?raw=true" target="_blank"><img src="https://github.com/Ackmaniac/esphome-jk-bms-can/blob/main/images/jk-values.jpg?raw=true" width="33%"></a>
```
type: entities
title: JK
entities:
  - entity: sensor.hjdc_jk_actual_battery_capacity
    name: Actual Battery Capacity
  - entity: sensor.hjdc_jk_alarm_low_volume
    name: Alarm Low Volume
  - entity: sensor.hjdc_jk_average_cell_voltage
    name: Average Cell Voltage
  - entity: sensor.hjdc_jk_balance_opening_pressure_difference
    name: Balance Opening Pressure Difference
  - entity: sensor.hjdc_jk_balance_starting_voltage
    name: Balance Starting Voltage
  - entity: binary_sensor.hjdc_jk_balancing
    name: Balancing
  - entity: binary_sensor.hjdc_jk_balancing_switch
    name: Balancing Switch
  - entity: sensor.hjdc_jk_battery_strings
    name: Battery Strings
  - entity: sensor.hjdc_jk_battery_type
    name: Battery Type
  - entity: sensor.hjdc_jk_capacity_remaining
    name: Capacity Remaining
  - entity: sensor.hjdc_jk_capacity_remaining_derived
    name: Capacity Remaining Derived
  - entity: sensor.hjdc_jk_cell_pressure_difference_protection
    name: Cell Pressure Difference Protection
  - entity: sensor.hjdc_jk_cell_voltage_overvoltage_delay
    name: Cell Voltage Overvoltage Delay
  - entity: sensor.hjdc_jk_cell_voltage_overvoltage_protection
    name: Cell Voltage Overvoltage Protection
  - entity: sensor.hjdc_jk_cell_voltage_overvoltage_recovery
    name: Cell Voltage Overvoltage Recovery
  - entity: sensor.hjdc_jk_cell_voltage_undervoltage_delay
    name: Cell Voltage Undervoltage Delay
  - entity: sensor.hjdc_jk_cell_voltage_undervoltage_protection
    name: Cell Voltage Undervoltage Protection
  - entity: sensor.hjdc_jk_cell_voltage_undervoltage_recovery
    name: Cell Voltage Undervoltage Recovery
  - entity: sensor.hjdc_jk_cells
    name: Cells
  - entity: binary_sensor.hjdc_jk_charging
    name: Charging
  - entity: sensor.hjdc_jk_charging_cycles
    name: Charging Cycles
  - entity: sensor.hjdc_jk_charging_high_temperature_protection
    name: Charging High Temperature Protection
  - entity: sensor.hjdc_jk_charging_low_temperature_protection
    name: Charging Low Temperature Protection
  - entity: sensor.hjdc_jk_charging_low_temperature_recovery
    name: Charging Low Temperature Recovery
  - entity: sensor.hjdc_jk_charging_overcurrent_delay
    name: Charging Overcurrent Delay
  - entity: sensor.hjdc_jk_charging_overcurrent_protection
    name: Charging Overcurrent Protection
  - entity: sensor.hjdc_jk_charging_power
    name: Charging Power
  - entity: binary_sensor.hjdc_jk_charging_switch
    name: Charging Switch
  - entity: sensor.hjdc_jk_current
    name: Current
  - entity: sensor.hjdc_jk_current_calibration
    name: Current Calibration
  - entity: binary_sensor.hjdc_jk_dedicated_charger_switch
    name: Dedicated Charger Switch
  - entity: sensor.hjdc_jk_delta_cell_voltage
    name: Delta Cell Voltage
  - entity: sensor.hjdc_jk_device_address
    name: Device Address
  - entity: sensor.hjdc_jk_device_type
    name: Device Type
  - entity: binary_sensor.hjdc_jk_discharging
    name: Discharging
  - entity: sensor.hjdc_jk_discharging_high_temperature_protection
    name: Discharging High Temperature Protection
  - entity: sensor.hjdc_jk_discharging_low_temperature_protection
    name: Discharging Low Temperature Protection
  - entity: sensor.hjdc_jk_discharging_low_temperature_recovery
    name: Discharging Low Temperature Recovery
  - entity: sensor.hjdc_jk_discharging_overcurrent_delay
    name: Discharging Overcurrent Delay
  - entity: sensor.hjdc_jk_discharging_overcurrent_protection
    name: Discharging Overcurrent Protection
  - entity: sensor.hjdc_jk_discharging_power
    name: Discharging Power
  - entity: binary_sensor.hjdc_jk_discharging_switch
    name: Discharging Switch
  - entity: sensor.hjdc_jk_errors
    name: Errors
  - entity: sensor.hjdc_jk_errors_bitmask
    name: Errors Bitmask
  - entity: sensor.hjdc_jk_manufacturer
    name: Manufacturer
  - entity: sensor.hjdc_jk_manufacturing_date
    name: Manufacturing Date
  - entity: sensor.hjdc_jk_max_cell_voltage
    name: Max Cell Voltage
  - entity: sensor.hjdc_jk_max_voltage_cell
    name: Max Voltage Cell
  - entity: sensor.hjdc_jk_min_cell_voltage
    name: Min Cell Voltage
  - entity: sensor.hjdc_jk_min_voltage_cell
    name: Min Voltage Cell
  - entity: sensor.hjdc_jk_operation_mode
    name: Operation Mode
  - entity: sensor.hjdc_jk_operation_mode_bitmask
    name: Operation Mode Bitmask
  - entity: sensor.hjdc_jk_password
    name: Password
  - entity: sensor.hjdc_jk_power
    name: Power
  - entity: sensor.hjdc_jk_power_tube_temperature
    name: Power Tube Temperature
  - entity: sensor.hjdc_jk_power_tube_temperature_protection
    name: Power Tube Temperature Protection
  - entity: sensor.hjdc_jk_power_tube_temperature_recovery
    name: Power Tube Temperature Recovery
  - entity: sensor.hjdc_jk_sleep_wait_time
    name: Sleep Wait Time
  - entity: sensor.hjdc_jk_software_version
    name: Software Version
  - entity: sensor.hjdc_jk_temperature_sensor_1
    name: Temperature Sensor 1
  - entity: sensor.hjdc_jk_temperature_sensor_2
    name: Temperature Sensor 2
  - entity: sensor.hjdc_jk_temperature_sensor_temperature_difference_protection
    name: Temperature Sensor Temperature Difference Protection
  - entity: sensor.hjdc_jk_temperature_sensor_temperature_protection
    name: Temperature Sensor Temperature Protection
  - entity: sensor.hjdc_jk_temperature_sensor_temperature_recovery
    name: Temperature Sensor Temperature Recovery
  - entity: sensor.hjdc_jk_temperature_sensors
    name: Temperature Sensors
  - entity: sensor.hjdc_jk_temperature_wire
    name: Temperature Wire
  - entity: sensor.hjdc_jk_total_battery_capacity_setting
    name: Total Battery Capacity Setting
  - entity: sensor.hjdc_jk_total_charging_cycle_capacity
    name: Total Charging Cycle Capacity
  - entity: sensor.hjdc_jk_total_runtime
    name: Total Runtime
  - entity: sensor.hjdc_jk_total_runtime_formatted
    name: Total Runtime Formatted
  - entity: sensor.hjdc_jk_total_voltage
    name: Total Voltage
  - entity: sensor.hjdc_jk_total_voltage_overvoltage_protection
    name: Total Voltage Overvoltage Protection
  - entity: sensor.hjdc_jk_total_voltage_undervoltage_protection
    name: Total Voltage Undervoltage Protection
```
