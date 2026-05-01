# BLE Analysis using nRF52840 Dongle and Wireshark in emay oxymeter and beurer Pressure Monitor

## Overview
This project focuses on analyzing Bluetooth Low Energy (BLE) communication between two medical IoT devices:
- EMAY EMO-80 Pulse Oximeter
- Beurer Blood Pressure Monitor

The analysis is performed using:
- nRF52840 Dongle (BLE Sniffer)
- Wireshark
- nRF Connect Mobile Application

The objective is to understand:
- BLE communication layers
- Device connectivity behavior
- GATT structure and data exposure
- Security posture of both devices

---

## BLE Architecture Overview

Bluetooth Low Energy operates in the following layers:

1. **Physical Layer (PHY)**
   - Radio transmission over 2.4 GHz

2. **Link Layer (LL)**
   - Advertising (ADV_IND)
   - Connection establishment (CONNECT_IND)
   - Channel hopping

3. **L2CAP**
   - Packet multiplexing

4. **ATT (Attribute Protocol)**
   - Communication between client and server

5. **GATT (Generic Attribute Profile)**
   - Defines services and characteristics

---

## Experimental Setup

### Hardware
- nRF52840 Dongle
- Android smartphone (OnePlus 13R)
- EMAY Oximeter
- Beurer BP Monitor

### Software
- Wireshark with nRF Sniffer plugin
- nRF Connect App
- Android Developer Options (HCI snoop enabled)

---

## Setting up nRF52840 Dongle

1. Install nRF Sniffer firmware:

nrfutil device program --firmware <firmware.hex> --serial-number <id>


2. Verify device:

nrfutil device list


3. Open Wireshark and select:

nRF Sniffer for Bluetooth LE


---

## BLE Advertising Analysis

Advertising packets are broadcast by BLE devices before connection.

**Observations:**
- Devices continuously broadcast ADV_IND packets
- Device names and metadata are visible in plaintext
- No encryption at advertising stage

---

## Connection Establishment

BLE connection is established using CONNECT_IND.

**Captured Parameters:**
- Initiator Address (Phone)
- Advertising Address (Device)
- Access Address
- Connection Interval
- Channel hopping enabled

---

## EMAY Device Analysis

### Identification
- Device advertises identifiable name (EMO-80)
- Uses random/static address

### GATT Structure
- Multiple services exposed
- Includes standard services:
- Generic Access
- Device Information

- Several **Unknown Services and Characteristics**
- Characteristics support:
- READ
- WRITE
- NOTIFY

### Key Finding
- Large number of readable characteristics
- Possible exposure of health data in plaintext

---

## Beurer Device Analysis

### Identification
- Device visible via BLE advertising
- Recognizable manufacturer profile

### GATT Structure
- Fewer services compared to EMAY
- Includes:
- Generic Access
- Generic Attribute
- Blood Pressure Service

### Key Finding
- More structured and minimal GATT design
- Less exposed surface compared to EMAY

---

## Comparison: EMAY vs Beurer

| Feature | EMAY | Beurer |
|--------|------|--------|
| GATT Complexity | High | Moderate |
| Unknown Services | Many | Few |
| Data Exposure | High | Lower |
| Characteristics | Read/Write/Notify | Mostly structured |
| Security Surface | Larger | Smaller |

---

## Wireshark Analysis

### Observations
- Continuous BLE advertising packets
- SCAN_REQ and SCAN_RSP visible
- Device names visible in payload
- Channel hopping observed

### Limitations
- Full data capture not possible without following channel hopping
- No complete ATT/GATT traffic captured

---

## Security Observations

- BLE advertising is unencrypted
- Device identity visible
- EMAY exposes more accessible characteristics
- No clear evidence of strong authentication mechanisms

---

## Conclusion

- BLE communication exposes device metadata openly
- EMAY device has higher exposure risk due to richer GATT surface
- Beurer device shows relatively better structure and reduced exposure
- BLE security depends heavily on pairing and encryption implementation

---

### Required Screenshots

#### Setup
- screenshots/setup_devices.png
- screenshots/nrf_setup.png
- screenshots/wireshark_interface.png

#### BLE Configuration
- screenshots/hci_enabled.png

#### Advertising
- screenshots/ble_advertising.png

#### Connection
- screenshots/connect_ind.png

#### EMAY Analysis
- screenshots/emay_identification.png
- screenshots/emay_gatt.png

#### Beurer Analysis
- screenshots/beurer_identification.png
- screenshots/beurer_gatt.png
