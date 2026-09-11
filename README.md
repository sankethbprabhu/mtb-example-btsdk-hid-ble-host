# BLE HID Host Sample Application

## Overview

This application demonstrates a Bluetooth Low Energy HID Host running on an AIROC Bluetooth device. It uses the BTSDK HID Host library to discover BLE peripherals that implement HID over GATT (HOGP), connect to a selected HID device, and exchange HID reports.

The embedded application is controlled by a host processor or PC through the WICED HCI UART interface. It provides LE scanning and HID Host commands for connection management, trusted-device management, HID descriptor access, report exchange, protocol selection, and wake-up pattern configuration.

## Features demonstrated

- BLE scanning and advertisement-report forwarding over WICED HCI
- BLE HID Host initialization and event handling
- Connecting to and disconnecting from BLE HID devices
- Adding and removing HID devices from the trusted-device list
- Persistent BLE HID GATT-cache and bonding-data handling
- Reading HID descriptors
- Getting and setting HID reports
- Selecting the HID boot or report protocol
- Configuring HID wake-up patterns
- BTSDK HCI trace output

## Hardware and software

- ModusToolbox 3.7 or later
- An AIROC Bluetooth development kit supported by the application makefile
- A BLE HID peripheral, such as a BLE keyboard or mouse
- A PC or host MCU connected to the board's WICED HCI UART
- Client Control or another application that sends the WICED HCI HID Host commands

The default target is `CYW920820M2EVB-01`. Other supported targets are listed in `makefile`, including `Vela-IF820-INT-ANT-DVK` and `Vela-IF820-EXT-ANT-DVK`.

## Building and programming

On Windows, launch the ModusToolbox Cygwin shell:

```text
<ModusToolbox installation>\tools_3.7\modus-shell\Cygwin.bat
```

Then run the following commands from the application directory:

```bash
make getlibs
make build
make qprogram
```

To build and program in one step:

```bash
make program
```

To select another supported target:

```bash
make TARGET=Vela-IF820-INT-ANT-DVK program
```

The shared BTSDK assets are stored in the workspace-level `mtb_shared/wiced_btsdk` directory and are populated from the `.mtb` files in `deps`.

## Demonstration workflow

1. Connect the AIROC development kit to the PC and identify the WICED HCI UART port.
2. Build and program the application.
3. Connect Client Control or the host application to the WICED HCI UART at the configured baud rate.
4. Start BLE scanning and inspect the advertisement reports returned over HCI.
5. Select a BLE HID device and send a connect command using its address.
6. Add the device after connection if it should reconnect as a trusted HID device.
7. Read the HID descriptor or request HID reports as needed.
8. Send HID reports or change the HID protocol through the HID Host commands.
9. Disconnect or remove the device when it is no longer trusted.

The exact HCI command format is defined by the BTSDK HCI Control API headers included by the application. The implementation handles commands including connect, disconnect, add, remove, get descriptor, get report, set report, set protocol, wake-up pattern set, and wake-up control.

## Application settings

The following make variables can be passed on the command line or adjusted in `makefile`:

- `TARGET`: board target. The default is `CYW920820M2EVB-01`.
- `APP_NAME`: Bluetooth device name when applicable.
- `BT_DEVICE_ADDRESS`: Bluetooth device address, or `default`.
- `UART`: programming UART, or `AUTO`.
- `ENABLE_DEBUG`: set to `1` to enable hardware-debug support.
- `TRANSPORT`: transport selection. The default is `UART`.
- `NVRAM_EMULATION`: set to `1` when using the optional NVRAM emulation component.
- `NVRAM_EMULATION_HCI`: set to `1` to use HCI communication for NVRAM backup storage when NVRAM emulation is enabled.

For example:

```bash
make TARGET=Vela-IF820-INT-ANT-DVK ENABLE_DEBUG=1 build
```

## BTSTACK version

BTSDK devices use different embedded BTSTACK versions. The application contains variant configuration under `COMPONENT_btstack_v1` and `COMPONENT_btstack_v3`; the active BSP selects the appropriate component through the BTSDK component mechanism.

## Repository contents

- `hci_control.c`, `hci_control.h`: common WICED HCI command and event handling
- `hci_control_le.c`, `hci_control_le.h`: BLE scan control and advertisement reporting
- `hci_control_ble_hidh.c`, `hci_control_ble_hidh.h`: BLE HID Host command handling
- `wiced_le_gatt.c`: GATT support used by the host application
- `makefile`: targets, components, build options, and supported boards
- `deps`: BTSDK libraries, BSPs, and host tools used by the application

## Notes

Build output is generated under `build/` and should not be committed. The `COMPONENT_btstack_v1` and `COMPONENT_btstack_v3` directories contain application configuration for the corresponding BTSTACK versions.

BTSDK documentation is available at https://infineon.github.io/btsdk-docs/BT-SDK/index.html.
