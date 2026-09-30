# Changelog

## 0.2.0

- **Shared with keep**: choose on the keep page which devices keep may see and control, by type or one by one, grouped by room. Anything not shared never leaves the house, and keep can't control it.
- Off by default: cameras, people, device locations and software updates.
- Controls must name their devices; controlling a whole area or device at once is refused, so nothing unshared can be reached indirectly.
- Access tokens in camera and media attributes are removed before anything leaves the box.

## 0.1.1

- keep's real logo (three squares, word and dot) as the add-on icon and logo, and on the keep page.
- Sidebar icon: a boxed K (Home Assistant only allows built-in icons in the sidebar).

## 0.1.0

- First version: outbound connection to the keep gateway, pairing with a code or QR, states, services, history, statistics, registry, live updates, lock codes, NFC cards and tag scans.
- Refuses Supervisor, backup, update, restart, shell, raw MQTT and Z-Wave/Zigbee network services.
