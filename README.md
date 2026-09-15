# Qwen Voice Cost — Omarchy bar widget

A cost and quota tracker for a [Qwen](https://qwenlm.github.io/) voice
assistant, as an Omarchy bar widget. One icon in the status bar; click for a
dropdown with usage, spend, free quota, and Alibaba Cloud billing.



## Features

- Token usage + estimated cost for **today**, **this month**, **all-time**
- **Free quota** progress bar (turns red at ≥90% consumption)
- Live **Alibaba Cloud billing** (balance + month bill) when an AccessKey is
  configured; falls back to a local estimate otherwise
- Data updates live (no polling) and on click / via Refresh button

## Requirements

- Omarchy (Quickshell-based shell with QML plugin support)
- The `qwen-cost-update` companion binary (produces the overview record the
  widget reads)
- *Optional:* Alibaba Cloud AccessKey for live billing

## Installation

```bash
# 1. Clone the plugin into your Omarchy plugins directory
git clone https://github.com/neilmc81/omarchy-qwen-cost \
    ~/.config/omarchy/plugins/qwen.cost

# 2. Make sure the qwen-cost-update binary is installed and points to the
#    path below (the widget invokes it to refresh).

# 3. Register the widget in ~/.config/omarchy/shell.json (bar section),
#    then reload the shell:
#    omarchy restart shell
```

## How it works

The widget performs no fetching itself. It watches the overview file that
`qwen-cost-update` writes:

```
$XDG_STATE_HOME/qwen-voice/cost/overview.json   # usage, quota, billing
```

Records are re-read live via `FileView` (watch changes), refreshed on a
5-minute timer, and on click.

## Optional configuration

- **Live billing** — add an Alibaba Cloud AccessKey to
  `~/.config/qwaudio/cost.json`
- **Free quota** — define your quota limit in `~/.config/qwaudio/cost.json` to
  render the progress bar

Per-request records come from `~/.config/qwaudio/state/usage.jsonl`.

## Files

```
manifest.json   plugin metadata
BarWidget.qml   bar icon (opens dropdown, refreshes on click)
Panel.qml       dropdown: usage, free quota, billing, refresh
```

## License

[MIT](LICENSE)