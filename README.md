# Qwen Voice Cost — Omarchy bar plugin

A cost tracker for the Qwen voice assistant (DashScope), as an Omarchy bar
widget: one icon in the top bar, a dropdown on click.

## What it shows

- Real token usage and estimated cost for **today**, **this month**, and
  **all-time** (from per-request records).
- **Free quota** remaining, with a progress bar that turns red at ≥90%
  consumption.
- Live **Alibaba Cloud billing** (account balance + month bill) when an
  AccessKey is configured; otherwise a local estimate.

## How data gets in

The widget does no fetching itself. It watches the overview file that
`qwen-cost-update` writes:

    $XDG_STATE_HOME/qwen-voice/cost/overview.json   (usage, quota, billing)

It re-reads the file live (`FileView` + `watchChanges`), refreshes every 5
minutes, and on click. A "Refresh" button in the panel re-runs
`qwen-cost-update --refresh`.

## Files

- `BarWidget.qml`  — the bar icon (opens the dropdown, refreshes on click)
- `Panel.qml`      — the dropdown: usage, free quota, billing, refresh
- `manifest.json`  — plugin metadata

## Optional config

Add an Alibaba Cloud AccessKey in `~/.config/qwaudio/cost.json` for live
billing; define your free-quota limit there to render the progress bar.

## Install

Registered in `~/.config/omarchy/shell.json` (bar). Requires the companion
`qwen-cost-update` binary to be present at
`~/.local/share/qwen-omarchy-control/bin/qwen-cost-update`.