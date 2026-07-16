# Function Checks

Per-module and system-level procedures to confirm the platform is healthy
before an experiment.

---

## Module function check

Step-by-step procedure to verify a single foraging module after power-on.

### 1. Status LED indication

Flash the `CanNode` firmware and power the module. Observe the **Status LED**:

| Phase | Status LED | Meaning |
| :---- | :--------- | :------ |
| Booting | Slow blink (1 s) | `begin()` executing |
| Waiting for discovery | Slow blink (1 s) | CAN up, awaiting AEI / base station |
| **Unassigned — no ID** | **Continuous slow blink** | Node has no saved NVS ID; base station has not yet driven AEI HIGH |
| ID assigned & online | **Off** | Normal operation |
| Fault | Red blink pattern | See `failure-modes.md` |

> [!IMPORTANT]
> If the status LED **keeps blinking indefinitely**, the node has not been
> assigned a Node ID. This is the expected state until the base station
> drives the AEI line HIGH and completes ID assignment. It is **not** a
> hardware fault on its own.

### 2. Node discovery and ID assignment

Connect the module to the base station via RJ45. The base station drives AEI
HIGH on the first module's AEI pin, initiating discovery:

1. Base station drives **AEI HIGH** on module 1.
2. Module transitions: `WaitAEI → CheckNVS`.
   - **First boot (NVS empty)**: transitions to `Announce`, broadcasts its
     MAC address, waits for `AssignId` CAN command.
   - **Subsequent boot (NVS has saved ID)**: transitions to `Rejoin` and
     immediately propagates AEO HIGH to the next node.
3. Once the base station sends `ASSIGN(MAC, nodeId)` and the module confirms,
   the **status LED turns off** and the node becomes fully operational.
4. The module begins transmitting periodic heartbeat frames (`0x200 + nodeId`)
   every ~5 s.

**Expected result**: Status LED off; node visible in base station GUI.

### 3. GUI node status display

Once a node's MAC address is registered and an ID is assigned, the base
station GUI (`tools/dev_gui`) shows the live node status. Available states
displayed in the GUI:

| GUI State | Description |
| :-------- | :---------- |
| **Idle** | Node online, no active dispense |
| **Loading** | Actuator (M2) lowering to home / pellet loading in progress |
| **Presenting** | Actuator raised; pellet is presented to the animal |
| **Fault** | Sticky fault active (Jam, Timeout) — requires Abort |
| **Offline** | No heartbeat received within watchdog window (~15 s) |

The GUI continuously monitors the `0x200 + nodeId` heartbeat. If a node
goes offline the card turns red and the state shows **Offline**.

### 4. Ping / locate node

The base station GUI sends a **Ping** (`CanCmd::Ping`) command to a specific
node or broadcast. The target node:

1. Responds with a `Pong` event (`CanEvent::Pong`) containing its **MAC
   address** in the payload — logged in the GUI event log.
2. Executes a **fast LED blink** pattern on its status LED so you can
   visually locate the physical module in the arena.

> [!TIP]
> Use Ping to confirm which physical box corresponds to a given Node ID
> before an experiment, or to verify bus connectivity to an individual node
> without sending a dispense command.

### 5. Motor and dispenser verification

Send a **Dispense** command from the base station GUI to cycle through a full dispense:

- **PG1**: beam-break at pellet exit — should trip during feed.
- **PG2**: home sensor for actuator (M2) — should clear when raised, block when home.
- **PG3**: dome sensor — should open when animal accesses port.

Expected dispense state sequence: `Idle → Loading → Presenting → Idle`.

### 6. Individual hardware tests (bench / troubleshooting)

The `examples/Troubleshooting/HardwareExamples/` directory contains
standalone Arduino sketches for isolated hardware verification:

| Sketch | What it checks |
| :----- | :------------- |
| `CanBusTest.ino` | CAN TWAI driver, frame TX/RX, bus termination |
| `LEDTest.ino` | All three LEDs (red/green/yellow) cycle through states |
| `PhotogateTest.ino` | PG1, PG2, PG3 beam-break outputs with Serial Monitor |
| `StepperMotorTest.ino` | M1 and M2 stepper motion with configurable step count |

---

## Base station function check

Step-by-step procedure to verify the Raspberry Pi base station HAT.

### Interactive hardware validation — `test_hat.py`

The primary automated bring-up tool is located at:

```
tests/test_hat.py
```

Run it **directly on the Raspberry Pi** with the HAT connected. The script is an **interactive checklist** — each section prints instructions, waits for your confirmation, then prints `[PASS]` or `[FAIL]` with a summary at the end.

#### Sections covered

| Section flag | What it validates |
| :----------- | :---------------- |
| `can` | SPI/MCP2515 interface up; loopback self-test; live node discovery |
| `aeo` | AEO (GPIO27) daisy-chain enable output |
| `bnc_out` | BNC OUT (GPIO6) idle/drive/pulse timing |
| `bnc_in` | BNC IN 1 / BNC IN 2 (GPIO12/13) idle level + edge detection |
| `button` | User button (GPIO3) |
| `full_loop` | End-to-end: BNC IN 1 → CAN broadcast dispense → BNC OUT pulse |

### Automated pytest suite

Run the full automated test suite (no hardware required — uses mock objects):

```bash
cd tools/dev_gui
pip install pytest
python -m pytest tests/ -v
```

Test files and their scope:

| File | Scope |
| :--- | :---- |
| `test_app.py` | GUI application lifecycle |
| `test_discovery_manager.py` | Node discovery FSM logic |
| `test_hat.py` | Interactive HAT hardware validation (manual) |
| `test_log_manager.py` | CSV event log writing |
| `test_mac_id_registry.py` | MAC ↔ Node ID persistent registry |
| `test_node_registry.py` | Node state machine and heartbeat watchdog |
| `test_protocol.py` | CAN frame encode/decode |
| `test_schedule.py` | Session scheduling logic |

### CAN interface bring-up (first-time)

Verify the CAN driver and interface are up and running:

```bash
ip link show can0          # should show "UP" and "mtu 16"
```

If `can0` is absent, follow the one-time setup in
`deploy/README.md` to add the device tree overlay to
`/boot/firmware/config.txt`.

---

## System function check (end-to-end)

Exercises the full platform with all modules attached.

1. **Power on** all modules and the base station.
2. **Start the GUI** on the Raspberry Pi:
   ```bash
   python run.py
   ```
3. **Verify discovery** completes — all expected nodes appear in the GUI grid
   with their correct Node IDs, and status LEDs turn off.
4. **Ping all nodes** from the GUI to confirm bus reachability (`ping_all`).
5. **Dispense check** — trigger one dispense per module and confirm the GUI
   state sequences through `Idle → Loading → Presenting → Idle`.
6. **BNC sync test** — assert BNC IN 1 with a test pulse; verify the GUI
   event log records the edge and (if configured) a CAN event is broadcast.
7. **Offline detection** — disconnect one module's RJ45; within ~15 s the GUI
   should show that node as **Offline**.
8. **Reconnect** — reattach the cable; the node should rejoin and return to
   **Idle**.

### Simulated system test (no hardware required)

Bring up a virtual CAN interface and run the node simulator for a software-
only end-to-end check:

```bash
# One-time per boot
sudo modprobe vcan
sudo ip link add dev vcan0 type vcan
sudo ip link set up vcan0

# Terminal 1 — simulate N nodes
python node_simulator.py --interface vcan0 --nodes 9

# Terminal 2 — start GUI connected to vcan0
python run.py --interface vcan0 --nodes 9
```

---

## Pass / fail criteria

| Check | Pass | Fail |
| :---- | :--- | :--- |
| Node discovery | Status LED turns **off** after ID assignment | Blink never stops |
| GUI node state | GUI shows live state (Idle/Loading/Presenting) | Node shows Offline or blank |
| Ping response | LED fast-blinks; MAC logged in event pane | No response within 2 s |
| Dispense cycle | State machine returns to Idle; pellet detected by PG1 | Fault or Timeout reported |
| CAN loopback (`test_hat.py`) | `[PASS] Loopback TX/RX matches` | Frame mismatch or interface not UP |
| BNC OUT pulse | Oscilloscope confirms pulse width within ±10 % | No pulse or incorrect width |
| Offline detection | Node goes **Offline** within heatbeat x 3 s of cable removal | State never changes |

---

## Cross-references

- [`failure-modes.md`](failure-modes.md) — fault catalog and LED patterns.
- [`maintenance.md`](maintenance.md) — scheduled preventive checks.
- [`architecture.md`](architecture.md) — CAN ID layout and discovery FSM.
