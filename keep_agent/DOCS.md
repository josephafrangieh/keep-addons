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
The one exception, while **Let keep install updates** is on: Keep staff may install an update Home Assistant itself offers (Core, OS, add-ons, integrations, firmware), after a backup where it can make one. Never a chosen version, never anything else.

## Remote support

Keep staff can't reach this Home Assistant from outside on their own: no port is open and no VPN is needed. When you need keep's help (a new sensor, a change), a session works like this:

1. A Keep staff member asks from keep's admin. You get a notification in the keep app with who is asking, why, and for how long.
2. You approve it in the app (Face ID), choosing how long and whether cameras may be seen.
3. The staff member gives you a 6-digit code (by phone, for example); you type it in the app. Only then does the session open.

During the session:

- The box makes a Home Assistant user for that session only (named "keep support · <name>") and deletes it when the session ends, which ends every login made with it. Nobody ever knows its password.
- Home Assistant's screen reaches the staff member through the box's own connection to keep.
- **Cameras stay private** unless you allowed them: camera pictures, live video, recordings and camera add-ons (Frigate, go2rtc…) are refused by the box.
- Some things are never possible: downloading or restoring backups, making users or access tokens, and shell add-ons (Terminal & SSH, Studio Code Server…).
- The keep app and your wall panels show that keep is working in your Home Assistant, until when.
- You can give more time or end it at any moment from the app. When its time is up, the box ends it by itself, even without internet.

What was done (settings changed, devices added, what was refused) is recorded **on this box only**; you can read it in the app. Turn off **Allow remote support** to refuse every session.

## The keep app at home

On the home network the keep app talks to this box directly on port **8125**, without going through the internet. It still needs the internet once to sign in and receive its pass; after that, lights, locks, climate and cameras keep working at home even when the internet is down.

- A pass belongs to one person and one home, lists exactly what they may see and control, and expires after three days (the app renews it whenever it's online). Changing someone's role, removing them, or republishing the home's dashboard in keep cuts off older passes.
- Every request is signed with a key that never travels on the home network, and every answer is signed back.
- What people do locally is kept on the box and handed to keep, so it appears in the account's activity.
- To turn it off, set the port to empty under the add-on's **Network** settings. The app then always goes through keep's cloud.

## Wall panels

Wall panels (a Shelly Wall Display, a tablet) open the panel page this box serves on the same port: **http://<box address>:8125**. They need no keep account and keep working when the internet is down.

- A new panel shows a 6-character code. Keep staff type it in keep's admin (the home, then **Panels**) to pair it with this home.
- Pairing is the only time the panel's secret crosses the home network. After that the box gives the panel its passes itself, for exactly what keep allows that panel.
- Removing a panel in keep's admin makes the box forget it as soon as the box is online.
- Turning off port 8125 turns off wall panels too.

## Who is home

People who turn on **Share when I'm home** in the keep app are published here, for automations:

| Entity | Meaning |
| --- | --- |
| `binary_sensor.keep_<name>_home` | On while that person is home. Keeps its id if they're renamed. |
| `binary_sensor.keep_anyone_home` | On while at least one of them is home. Attributes: `people` (who), `not_sharing` (members who haven't turned it on, so aren't counted). |
| `sensor.keep_people_home` | How many are home. |

The phone notices arriving and leaving the circle around the home's location (from Home Assistant: Settings → System → General → Location), usually within a few minutes, even with the app closed. keep receives only "arrived" or "left", never where the person is. A person who stops sharing or leaves the home disappears from these entities.

Example: arm the alarm when everyone has been gone for 5 minutes.

```yaml
triggers:
  - trigger: state
    entity_id: binary_sensor.keep_anyone_home
    to: "off"
    for: "00:05:00"
actions:
  - action: alarm_control_panel.alarm_arm_away
    target:
      entity_id: alarm_control_panel.home
```

## Options

| Option | Default | Meaning |
| --- | --- | --- |
| `gateway_url` | `wss://keep-gateway.fly.dev/agent` | keep's gateway. Change only if keep asks you to. |
| `log_level` | `info` | `debug` shows each request. |
| `allow_updates` | `true` | **Let keep install updates**: Keep staff may install Home Assistant's updates from keep's admin. Turn off to manage updates yourself. |
| `allow_remote_support` | `true` | **Allow remote support**: sessions you approve in the keep app (see above). Turn off to refuse them all. |
| `ha_url` | (empty) | Home Assistant's own address for remote support, only if it serves https itself. Empty means `http://homeassistant:8123`. |

## Privacy

The box's identity is a key pair kept in the add-on's data folder (and in Home Assistant backups). keep's cloud stores only the public half, so it can recognise the box but can't impersonate it.
