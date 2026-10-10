# Changelog

## 0.13.0

- **Camera pictures in notifications.** A notification can carry a camera's picture: add `camera: camera.front_door` to a `keep_notify` event (or choose a camera for a standard notification, such as the doorbell, in keep). The box takes the picture at that moment and sends it with the notification; tapping it opens the camera in the app. Only cameras shared with keep can be used. If the camera doesn't answer within 8 seconds, the notification goes without its picture.

## 0.12.0

- **Notifications.** The box passes notifications from the home to keep, which sends them to the right people's phones. Any Home Assistant automation can send one by firing the event `keep_notify` (with a `channel`, a `title` and a `message`). keep's standard notifications (alarm, water leak, smoke, gas, opened while away, doorbell, unlocked at night) are written by the box as Home Assistant automations named "keep – Notify: …", watching only devices shared with keep. A notification fired while the internet is down is sent once it's back, if it's less than 30 minutes old.

## 0.11.0

- **Device schedules.** In the keep app, an owner can give an on/off device (a water heater's switch, a pump, a light, a fan) a weekly schedule in 30-minute slots: on where painted, off elsewhere. The box runs it, so it keeps working without internet. A device switched by hand keeps that until the schedule's next change (or until "Resume schedule"); a paused schedule switches nothing.

## 0.10.0

- **Heating and cooling presets.** A thermostat's program now has a heating plan and a cooling plan, each with its own presets, default and weekly schedule. The box follows the plan of the thermostat's mode: switching between heat and cool starts the other plan at once, and auto, dry, fan or off run no plan. On a Versatile Thermostat, cooling presets edit its cooling (AC) temperatures. Programs saved with 0.9.0 become the heating plan.

## 0.9.0

- **Thermostat programs.** In the keep app, a thermostat can have presets with a temperature for when someone is home and one for when everyone is away, and a weekly schedule in 30-minute slots. The box runs it, so it keeps working without internet: it applies each slot's preset, holds a temperature set by hand until the next slot (or until "Resume schedule"), and switches keep's presets between home and away. On a Versatile Thermostat, Frost/Eco/Comfort/Boost stay its own presets: keep writes their temperatures to it and selects them. Switched-off heating is left alone.

## 0.8.0

- **Automations from the keep app.** A home's owner can make simple automations in the app (when everyone has left, when someone arrives, at a time, at sunset or sunrise). keep sends the box a description and the box writes the Home Assistant automation itself, named "keep – …" and marked as managed by keep. Only devices shared with keep can be used, and only safe actions: lights, switches and fans on or off, shutters open or closed, doors locked, the alarm armed, scenes. Automations can't unlock a door, disarm the alarm or open a garage door.

## 0.7.0

- **Who is home, for automations.** People who turn on "Share when I'm home" in the keep app now appear in Home Assistant: `binary_sensor.keep_<name>_home` for each of them, `binary_sensor.keep_anyone_home` and `sensor.keep_people_home`. Use them like any sensor, for example "when no one is home for 5 minutes, arm the alarm". The app tells keep only when someone arrives or leaves, never where they are. The home's location comes from Home Assistant (Settings → System → General).

## 0.6.0

- **Local access for the keep app.** On the home Wi-Fi the app now talks to this box directly: faster, and the home keeps answering when the internet is slow or down. The app shows a pass keep issued while online; the pass lists exactly which devices that person may see and control, and every request and answer is signed, so nobody else on the network can use or imitate it. Sharing choices on the keep page and the blocked services still apply. Uses port 8125 on the home network; switch it off in the add-on's Network settings to use the app through the cloud only.

## 0.5.3

- Clearer recordings: when Frigate has already deleted a detection's video (it keeps video for fewer days than snapshots), keep now says "This clip is no longer on the box" instead of Home Assistant's "500" error.

## 0.5.2

- Fix: recordings' snapshots and clips failed ("Home Assistant answered 500") where Home Assistant's Frigate media proxy doesn't work. The agent now reads them from Frigate directly on the box (found by itself, or set **frigate_url** in the options), and uses Home Assistant's proxy only as a fallback.

## 0.5.1

- Fix: recordings didn't open with some versions of the Frigate integration ("not a valid option at decode_json").

## 0.5.0

- **Camera recordings**: keep shows Frigate's detections (what was seen and when) for shared Frigate cameras, with their snapshots, and plays or downloads their clips. Read through Home Assistant's Frigate integration; clips go from this box straight to the viewer through a one-time link, and keep never stores them. A recording can only be reached through the camera it belongs to.
- go2rtc running on Home Assistant's own host (like the go2rtc add-on) is now found by itself: no go2rtc_url needed in most cases.

## 0.4.5

- Fix: the keep page stayed empty ("Loading…", no status) since 0.4.3, because of a broken quote in its script. Its script is now checked by the tests before every release.

## 0.4.4

- Fix: the keep page could stay on "Loading…" after 0.4.3. The device list no longer waits for the box to look for go2rtc; camera stream choices load on their own, in 4 seconds at most, and a go2rtc that isn't there is remembered for a minute.

## 0.4.3

- **Choose each camera's live video stream** on the keep page (under each shared camera), for cameras whose go2rtc stream has a different name (for example a "Living Room" camera whose stream is "ezviz_main"). Automatic matching stays the default.

## 0.4.2

- Live video finds a camera's go2rtc stream even when the names differ: Frigate's "outdoor_cam" plays go2rtc's "outdoor_main" (full quality; a "_sub" stream only when there's nothing better).
- Keep staff can see what the box found for each camera (go2rtc address, matching stream), to set things up faster.

## 0.4.1

- **Live video straight from go2rtc**: Frigate cameras (which Home Assistant itself can't stream) now play live. The agent finds go2rtc on the box by itself (Frigate's built-in go2rtc or the go2rtc add-on) and matches each camera to its stream by its Frigate name. If yours lives elsewhere, set its address in the add-on's **go2rtc_url** option.

## 0.4.0

- **Cameras in keep**: snapshots and live video for cameras you share on the keep page (cameras stay private until you switch them on).
- Live video uses Home Assistant's own engine (WebRTC): keep only introduces the viewer's device to this box; the video goes directly between them and is never stored by keep.
- Unsharing a camera ends anyone's live view of it at once; a live view lasts 15 minutes at most, and 4 can run at the same time.

## 0.3.1

- Fix: backups made from keep stayed "running" with "the keep agent add-on needs its backup permission". The add-on's backup permission covers backups but not the Supervisor's job tracker, so keep now follows each backup by its name in the backup list. No new permission needed.

## 0.3.0

- **Backups for keep**: Keep staff can make a backup of this Home Assistant from the Keep Console and download it to their computer. Backups hold everything except the media folder, are always password-protected, and go straight from this box to the staff member's browser through a link that works once, for 10 minutes.
- New permission: the Supervisor's **backup** role (make, list and download backups). The add-on still can't restore backups, manage add-ons or touch the host, and backup services stay blocked for everything else keep sends.

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
