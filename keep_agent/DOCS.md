# keep agent

## Pairing a new box

1. Start the add-on and open **keep** in the sidebar.
2. The page shows a pairing code and a QR code. They change every 15 minutes.
3. In the Keep Console, open the client's site, choose **Pair a box**, and scan the QR code (or type the code).
4. The page switches to **Connected**. The box is now linked to that site.

To move a site to a new box, pair the new box with the same site: the old one is disconnected.
Unpairing a site in the Keep Console resets this box; it shows a new code.

## Shared with keep

The keep page lists every device, grouped by type and room, each with a switch. Only what is switched on is ever sent to keep, and keep can't control anything else, whatever it asks. Switching a device off takes effect immediately, even for a portal that is open.

- A type's switch (for example **Cameras**) applies to all its devices; a device can then be switched differently on its own.
- New devices follow their type's switch.
- **Off by default:** cameras, people, device locations, software updates.
- Only people who can log in to this Home Assistant can change these switches.

## What the agent can and can't do

keep can read states and history, control devices, manage lock codes and NFC cards, and read automations.
The agent refuses services that could damage the box whatever the cloud asks: Supervisor and backups, updates, restarts, history purges, shell and script commands, raw MQTT, and Z-Wave/Zigbee network management.

## Options

| Option | Default | Meaning |
| --- | --- | --- |
| `gateway_url` | `wss://keep-gateway.fly.dev/agent` | keep's gateway. Change only if keep asks you to. |
| `log_level` | `info` | `debug` shows each request. |

## Privacy

The box's identity is a key pair kept in the add-on's data folder (and in Home Assistant backups). keep's cloud stores only the public half, so it can recognise the box but can't impersonate it.
