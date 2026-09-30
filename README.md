# keep add-ons for Home Assistant

Add-ons that connect a Home Assistant to [keep](https://keep-website-ashen.vercel.app).

## Install

1. In Home Assistant: **Settings → Add-ons → Add-on store → ⋮ → Repositories**.
2. Add `https://github.com/josephafrangieh/keep-addons` and close the dialog.
3. Install **keep agent**, start it, and open **keep** in the sidebar to see the pairing code.

## Add-ons

| Add-on | What it does |
| --- | --- |
| [keep agent](keep_agent/) | Connects this Home Assistant to keep from the inside out: one encrypted outbound connection, nothing opened to the internet. |

This repository only describes the add-ons; Home Assistant downloads the ready-built images from `ghcr.io/josephafrangieh/keep-agent`. It is updated automatically on each release.
