# python-orvibo

A Python 3 library to control Orvibo devices, specifically the S20 WiFi Smart Switch. This library allows for device discovery, status checking, and remote control (on/off) of the smart switch over a local network.

## Features

- **Discover Orvibo S20 devices** on your local network.
- **Query the current state** (on/off) of a device.
- **Turn devices on or off** remotely.
- **Lightweight and easy-to-use** with minimal dependencies.

## Installation

Install the library using pip:

```bash
pip install orvibo
```

## Usage

```python
from orvibo.s20 import S20, discover

# Discover devices on your local network
hosts = discover()

# Initialize the S20 switch using a discovered host or a known IP address
s20 = S20("192.168.1.100")

# Check the current state (True = ON, False = OFF)
print(s20.on)

# Turn the switch on
s20.on = True

# Turn the switch off
s20.on = False
```

## Detailed Example

Here's a more detailed example demonstrating the usage of the library:

```python
import time
from orvibo.s20 import S20, discover

# Discover available S20 devices on the network
hosts = discover()
print("Discovered devices:", hosts)

# Initialize an S20 device using the first discovered host
if hosts:
    host_ip = list(hosts.keys())[0]
    s20 = S20(host_ip)
    
    # Print the current state
    print("Current state:", "ON" if s20.on else "OFF")
    
    # Toggle the state of the switch
    s20.on = not s20.on
    print("New state:", "ON" if s20.on else "OFF")
    
    # Wait for 5 seconds
    time.sleep(5)
    
    # Turn off the switch
    s20.on = False
    print("Switch turned off")
else:
    print("No devices found")
```

## Contributions

Pull requests are welcome! Here are some possible areas for improvement:

- Add support for additional Orvibo devices.
- Expand the functionality of the S20 class to include timers, configuration options, and more.

To contribute, fork the repository, make your changes, and submit a pull request.

## Technical Details

The library communicates with the Orvibo S20 using UDP packets. It handles device discovery, subscription, and control using predefined packet formats.

### Packet Structure

- **MAGIC:** The magic bytes used for packet identification.
- **DISCOVERY:** Payload for discovering devices.
- **SUBSCRIBE:** Payload for subscribing to a device.
- **CONTROL:** Payload for controlling a device's state (on/off).

### Constants

- **PORT:** The UDP port used for communication (default: 10000).
- **RETRIES:** Number of retries for sending packets.
- **TIMEOUT:** Timeout for receiving responses.
- **DISCOVERY_TIMEOUT:** Timeout for the discovery process.
- **SUBSCRIPTION_TIMEOUT:** Timeout for renewing device subscriptions.

### Internal Methods

- **_setup():** Initializes the UDP socket and starts the listener thread.
- **_listen():** Listens for incoming UDP packets.
- **_device_time(tab):** Converts device time to a human-readable format.
- **_udp_transact(payload, handler, \*args, broadcast=False, timeout=TIMEOUT):** Handles UDP transactions with retries and timeouts.

### Class: `S20`

- **__init__(self, host, mac=None):** Initializes the S20 object with the device's IP and MAC address.
- **on:** Property to get or set the device's state.
- **_discover_mac(self):** Discovers the MAC address of the device.
- **_subscribe(self):** Subscribes to the device to get its state and enable control.
- **_subscription_is_recent(self):** Checks if the subscription is recent.
- **_control(self, state):** Sends a control command to the device to change its state.
- **_turn_on(self):** Turns the device on.
- **_turn_off(self):** Turns the device off.

## Disclaimer

This project is not affiliated with Shenzhen Orvibo Electronics Co., Ltd. Use this library at your own risk.
