- Elk is an **embedded Linux distribution**, _highly_ _optimized for low-latency audio_, that comes with _several components to streamline the development of professional audio and music devices.
- The supported hardware are either one of our **Elk development boards**, or a simple Raspberry Pi with a audio hat, or custom hardware designed for products powered by Elk
- The last ingredients needed for the product recipe are **audio processing plugins**. Luckily, Elk supports standard formats such as **VST** (both 2.x and 3.x), **LV2** and an internal plugin format. If you have access to the plugins’ source code it is usually trivial to rebuild them for Elk.

![[Pasted image 20230701231405.png]]

- **Hardware components of the above box may include any of the following**:
	- Analog audio connections, e.g. TSR or XLR connectors
	- Digital audio I/O: SPDIF, ADAT, USB UAC 2.0, Audio over Ethernet (AES67, AVB, etc.)
	 - MIDI, Control Voltage
	 - MIDI, Control Voltage
	 - Some physical controls e.g. knobs, faders, buttons
	 - A bunch of LEDs and/or smaller secondary displays
	 - Bluetooth Low-Energy (BLE) and WiFi components

- **Usages include the following**:
	- Use it as a standalone FX processor for applications like guitar or vocals
	- Use it as a sound module driven by a MIDI controller
	- Use it as an USB class-compliant audio interface to connect it with a computer running a DAW
	- Share data and performances with an app running on a mobile device
	- Connect it to the cloud over WiFi to get updates, sharing presets, etc.


### Software Components Inside an ELK Product

![[Pasted image 20230701232006.png]]

- **SUSHI**:
	- Sushi simplifies the integration of audio and MIDI plugins into the Elk Audio OS ecosystem, enabling real-time audio processing on embedded platforms. It provides a flexible and extensible framework for building audio applications and systems.