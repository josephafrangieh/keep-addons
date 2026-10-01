# Changelog

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
