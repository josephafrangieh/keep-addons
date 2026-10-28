# Changelog

## 0.25.3

- **A wrong `frigate_url` no longer blocks recordings.** If the address set in the options doesn't answer as Frigate's API (go2rtc's port 1984, say), the box tries its port 5000 and finds Frigate by itself as usual. The option's description now says which port is meant.

## 0.25.2

- **Why recordings' pictures and clips fail.** When Frigate isn't reached directly and Home Assistant's Frigate proxy fails too, the error now says what each address the box tried answered (name not found, refused, timed out, an HTTP status), so the right `frigate_url` is easy to set. Home Assistant's own host on port 5000 is tried too.

## 0.25.1

- **Recordings' pictures and clips on more setups.** The box now finds Frigate where Home Assistant's Frigate integration reaches it (the address its entry is named after), or as any installed Frigate add-on, whatever its repository, instead of only the usual add-on addresses. Before, a Frigate it didn't find was reached through Home Assistant's Frigate proxy, which fails on some setups: no thumbnails, snapshots or clips. If the proxy's notification path fails, its thumbnail and snapshot paths are tried too. The add-on's log says when Frigate wasn't reached directly, and what was tried.

## 0.25.0

- **Why the alarm went off.** The alarm's notification now says what set it off: the doors, windows and motion sensors (shared with keep) that tripped in the minutes before, with their times, and what the alarm still lists as open (Alarmo). For example: "Front door opened at 14:02:31, then Hall motion detected movement at 14:02:40."
- **Every camera when the alarm goes off.** The box takes a picture of each shared camera (six at most) at that moment. As before, the pictures stay on the box for 24 hours and keep fetches them when a phone shows the notification.

## 0.24.0

- **Timers.** The keep app can turn a light, a switch or a fan on for a while (the water heater for 15 minutes): the box turns it on, then off when the time is up, even without internet, and also after a restart. Turned off by someone else, the timer simply ends. The box keeps the timers people used, to start them again in one tap. While a timer runs, the box tells keep when its device goes off, so the timer on people's lock screens ends too.
- **TV power through an IR blaster.** A Remote card can turn its TV on (and off) with an IR blaster's learned command (device/command), for TVs their integration can't turn on. Also on the home network.

## 0.23.0

- **Check for updates without the wait.** keep's admin starts the look for new versions on the box and follows it, instead of waiting for the Supervisor to re-read every add-on repository in one go (which could take over a minute and time out, leaving the list behind). Home Assistant's update list is also refreshed a second time a few seconds later, so a new add-on version that shows up late isn't missed.

## 0.22.0

- **TV remote at home.** The keep app's Remote card works on the home network too: volume, mute, apps and inputs, next and previous on a TV or media player, and its remote's keys (arrows, OK, Back, Home…) by their own names. Only on devices shared with keep and placed on a Remote card.

## 0.21.0

- **keep's thermostat presets stand alone.** The box always sets the thermostat's temperature itself: a preset's home temperature, or its away temperature when keep says everyone is away (binary_sensor.keep_anyone_home). A Versatile Thermostat's own presets and presence sensor are no longer used or changed, so a presence sensor set up there can't keep the heating at home temperature while everyone is away. It still regulates, and still handles an open window or a quiet sensor on its own. Thermostats left on one of its presets move to keep's temperature at once.
- **Lighter widget requests.** keep can ask for only the devices a widget shows, instead of every state in the home.
- **Guests.** keep's app can say guests are home: binary_sensor.keep_guests_home turns on, and while it is, binary_sensor.keep_anyone_home stays on and sensor.keep_people_home counts them as one, so automations and keep's thermostats treat the home as occupied whoever else is away.
- **Recordings by day.** keep can ask Frigate for one day's detections (after/before), without their pictures (fetched one by one).

## 0.20.4

- **Remote support on any Home Assistant setup.** The box no longer assumes Home Assistant's web server is at http://homeassistant:8123: it asks the Supervisor for the port and whether Home Assistant serves https itself (its own certificate), and tries both. It also checks this ahead, so keep's admin shows the reason before anyone asks the owner, instead of "the box isn't connected". `ha_url` still overrides it.

## 0.20.3

- **Check for updates finds new add-on versions.** Home Assistant refuses to pass "look for updates" on for an add-on, so the agent now asks the Supervisor itself. That needs the Supervisor's **manager** permission (it was **backup**); the agent uses it only for that and for backups, nothing else (no restores, no installing add-ons, no host control). Home Assistant shows the add-on's security rating one point lower because of it.

## 0.20.2

- **Remote support: dashboards load.** A page that loads many files at once (dashboards with custom cards) no longer fails with "Error while loading page lovelace": its requests wait their turn on the box instead of being refused past 64. Home Assistant Cloud's status, which every page reads, is no longer refused (signing in to it and turning its remote access on still are). The page reporting its own errors is no longer listed in the session's log.

## 0.20.1

- **"Check for updates" from keep's admin really checks.** Home Assistant now looks for new versions first (the Supervisor reloads the add-on store and the Core, OS and Supervisor versions, then every update entity refreshes, like Home Assistant's own Check for updates), so a release published since its last automatic check shows up at once.

## 0.20.0

- **Remote support, only when you allow it.** Keep staff can work in this Home Assistant (to add a sensor, change a setting) only after the home's owner approves their request in the keep app and types the code the staff member gives them, until the time the owner sets or until they end it. The box makes a Home Assistant user for each session and deletes it at the end; the screen travels through the box's own connection to keep, with no port opened and no VPN. Cameras stay private unless the owner allows them for that session; backup downloads, new users or tokens, and shell add-ons are always refused. What was done is recorded on the box only. New settings: **Allow remote support** (on) and `ha_url` (only if Home Assistant serves https itself).

## 0.19.0

- **Updates from keep's admin.** Keep staff can see the updates Home Assistant offers on this box (Core, the OS, the Supervisor, add-ons, integrations, device firmware) and install them, one at a time, after a backup where Home Assistant can make one. Only what Home Assistant itself offers; nothing else of updates is reachable. A new setting, **Let keep install updates** (on), lets the home's owner keep updates to themselves.

## 0.18.0

- **Notification pictures stay on the box.** A camera's picture for a notification is kept here for 24 hours and fetched by keep only when a phone shows the notification; nothing of it is stored in keep's cloud any more.
- **The box is the record of who is home.** keep now sends only the arrivals and departures the box hasn't had yet (it ignores a report older than what it knows), and reads who is home from the box for the app's Home & away screen.

## 0.17.0

- **Wall panels.** The box now serves keep's panel page on the home network (port 8125), for shared screens such as a Shelly Wall Display or a tablet. A new panel shows a code; Keep staff pair it from keep's admin. Paired panels get their passes from the box itself, so they keep working without internet. Removing a panel in keep makes the box forget it.

## 0.16.1

- **Fewer "Resume schedule" for nothing.** A Versatile Thermostat that changes its own preset for a while (safety mode when a temperature sensor goes quiet, which battery TRVs do; a window open; power shedding) is no longer taken for a change by hand: the schedule carries on. A difference also has to last half a minute to count. The add-on's log now says what keep expected and what it saw when it does count one.

## 0.16.0

- **Weather forecast cards.** keep can show a weather entity's forecast (the next hours, or the next days) on the app's dashboard, from Home Assistant's own weather integration (Met.no by default). Only weather entities shared with keep.

## 0.15.0

- **Widgets that update within seconds.** When the alarm (armed, disarmed, going off), a lock, a garage door or gate, or a door or window sensor changes, the box tells keep, which asks the phones' keep widgets to refresh at once instead of when iOS gets round to it. Changes close together go as one; the alarm goes out almost immediately. Only devices shared with keep, only real state changes; nothing is queued while the box is offline.

## 0.14.1

- **Fix: live video through Home Assistant could close the keep app.** When live video goes through Home Assistant's own WebRTC (no go2rtc found on the box), its network candidates name their fields in snake_case; the box passed them on with empty fields, and the iPhone app closed. The box now reads both spellings and never sends an empty field.

## 0.14.0

- **Heating and cooling history.** keep can show when each thermostat was heating, cooling, idle or off (its `hvac_action`), not only its mode, for the History page's "Heating & cooling" card. It works with any thermostat that reports what it's doing, including Versatile Thermostat over a radiator valve, an oil heater's switch or an AC. Only thermostats shared with keep.

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
