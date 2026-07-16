# Failure Modes

Catalog of how the platform can fail, how each failure is detected, and how it is reported to the user.

## Failure Mode Catalog

| ID | Failure Mode | Detection | User Signal | Recovery |
| :--- | :--- | :--- | :--- | :--- |
| **F-001** | **Pellet Feeder Jam** | PG1 (pellet sensor) remains blocked for >3s (`kPg1JamMs`) or fails to detect a pellet within 30s (`kDefaultFeedTimeoutMs`) / 4096 steps (`kDefaultFeedMaxSteps`) during feeding. | Status LED blinks red; reports `ServiceStatus::Jam` or `ServiceStatus::Timeout` in CAN heartbeat/event; base station dashboard alert. | Clear hopper/dispenser obstruction. Send CAN `Abort` command to clear the sticky fault state and return to Idle. |
| **F-002** | **Actuator Lowering Failure** | Actuator motor (M2) lowers to seek home, but PG2 (home sensor) fails to trigger within 8s (`kDefaultLowerTimeoutMs`) or 2048 steps (`kDefaultLowerSteps`). | Status LED blinks red; reports `ServiceStatus::Timeout` in CAN heartbeat/event; base station dashboard alert. | Inspect actuator path for mechanical blocks, verify PG2 alignment, and send CAN `Abort` command. |
| **F-003** | **Actuator Raising / Home Clearance Failure** | Actuator motor (M2) raises pellet, but PG2 fails to clear within 5s (`kPg2ClearOnRaiseMs`) of starting or the raise motion fails to complete within 8s (`kDefaultRaiseTimeoutMs`). | Status LED blinks red; reports `ServiceStatus::Jam` or `ServiceStatus::Timeout` in CAN heartbeat/event; base station alert. | Clear obstruction, check sensor wiring, and send CAN `Abort` command. |
| **F-004** | **Dome Open Warning (Extended Access)** | PG3 (dome sensor) remains open continuously for >30s (`kDomeOpenWarnMs`). | Module status LED flashes yellow (warning); reports open PG3 state in CAN heartbeat; base station UI alert. | Close the dome or remove any physical obstructions. Clears automatically when PG3 returns to closed (non-sticky). |
| **F-005** | **Node Offline (CAN Communication Failure)** | Base station fails to receive CAN heartbeats (`0x200 + nodeId`) or general CAN messages from an active node for longer than the timeout period (typically 2-3x the heartbeat interval, e.g., 10-15s). | Base station UI marks node as "Offline" (red indicator); logs network event. | Verify RJ45 bus cabling, check the 12 V power supply distribution, and confirm that the 120 Ω CAN termination resistor is active on the last node. |
| **F-006** | **Node Initialization Failure** | Identity/NVS loading fails (`identity.begin() != Ok`) or CAN transceiver hardware/TWAI driver fails to start (`can.begin() != Ok`). | Status LED blinks fast during boot; node fails to participate in CAN discovery; remains unassigned. | Check MCU board, re-flash firmware, or format NVS storage to force fresh ID discovery. |

## Detection Categories

- **Automatically detected by firmware**: The VFM node checks photogates (PG1-3) and stepper motor position step-counters and timeouts locally, updating its internal FSM and immediately broadcasting events.
- **Detectable from base station / host software**: Base station registers node presence, tracks response times, and monitors missing heartbeats on the CAN bus.
- **Requires manual inspection**: Physical damage, loose RJ45 connectors, or stepper driver heating issues.

## Reporting Paths
1. **Local Indication**: When a fault is declared, the VFM node switches the local status LED.
2. **Network Event Broadcast**: The module sends a `CanEvent::Fault` message (ID `0x300 + nodeId`) over the CAN bus containing the exact `ServiceStatus` code.
3. **Heartbeat Updates**: The module packs its current FSM state (byte 0) and the sticky fault/warning code (byte 5) into its periodic `0x200 + nodeId` heartbeat packet.
4. **Base Station Host UI**: The Raspberry Pi base station monitors the CAN bus, logs incoming event frames, updates the node state registry, and notifies the supervisor software/user interface.

## Base Station Presence & Offline Detection

The base station uses a watchdog mechanism based on CAN communication to determine if a node is online:

- **Periodic Heartbeats**: Active nodes transmit a status heartbeat frame (`0x200 + nodeId`) periodically. The default interval is **5 seconds** (`kDefaultHeartbeatIntervalMs`), which can be configured at runtime via `CanCmd::SetConfig`.
- **Offline Classification**: The base station tracks the timestamp of the last message received from each registered Node ID. If no packet is received for **15 seconds** (3x the default heartbeat interval), the base station classifies the node as **offline**.
- **Rejoin Processing**: If a node goes offline but then broadcasts a message (e.g., standard heartbeat or rejoin discovery frame), the base station processes the message, checks if the MAC address matches the registered configuration, and marks the node back as **online**.

## Cross-references

- [`function-checks.md`](function-checks.md) — procedures to confirm a module is healthy.
- [`maintenance.md`](maintenance.md) — preventive measures and scheduled inspections.
- [`architecture.md`](architecture.md) — network topology and CAN message class details.
