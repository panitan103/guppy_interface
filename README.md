# guppy_interface

ROS 2 interface package for the Guppy ROV.

## Role In The System

`guppy_interface` defines the message contract shared by:

- `guppy`, the onboard SBC to MCU serial driver.
- `guppy_web`, the browser-based ground-control interface.
- Any future ROS 2 nodes that publish commands or consume telemetry.

## Messages

### `GuppyDriverTransmitted.msg`

Command path:

```text
ground control / web UI -> ROS 2 -> guppy driver -> MCU
```

Fields:

- `surge`
- `sway`
- `yaw`
- `heave`
- `s1`
- `s2`
- `o1`
- `o2`
- `o3`
- `o4`

The names are historical, but the meaning is: command transmitted from
ground-control intent toward the MCU.

Current MCU command range:

- `surge`, `sway`, `yaw`, and `heave`: `-400..400`, matching the RC receiver
  style command values expected by the Teensy firmware.
- `s1`, `s2`, `o1`, `o2`, `o3`, and `o4`: encoded as unsigned byte fields.

### `GuppyDriverReceived.msg`

Telemetry path:

```text
MCU -> guppy driver -> ROS 2 -> ground control / web UI
```

Fields:

- `roll`
- `pitch`
- `yaw`
- `depth`
- `water`
- `volt`
- `amp`

The names are historical, but the meaning is: telemetry received from the MCU
and made available to ROS/ground control.

## Hardware Context

The matching MCU firmware is maintained here:

<https://github.com/panitan103/TEENSY_40_YOUYEETOO.git>

Current firmware hardware notes:

- PCA9685 is connected.
- BNO055 is connected.
- MS5837 is connected.
- No servo is currently attached.

## Development

Build and test:

```bash
colcon build --packages-select guppy_interface
colcon test --packages-select guppy_interface
colcon test-result --verbose
```

This package has its own git repository. Commit every coherent change locally;
push is manual.
