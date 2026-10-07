# Telemetry

Three modes, one switch. The full statement is the privacy policy at [m0saic.io/privacy](https://m0saic.io/privacy); this page is the short version.

| Mode | Recorded on your machine | Sent to m0saic.io |
|---|---|---|
| `standard` (the default) | yes, under `~/m0saic/telemetry/` | anonymous daily counts |
| `local` | yes | nothing |
| `ghost` | nothing, no telemetry file is touched | nothing |

## Switching

```sh
m0saic telemetry                 # what is recorded locally, what leaves, the current mode
m0saic telemetry preview         # the exact payloads that would be sent
m0saic telemetry set-mode local  # keep the history, send nothing
m0saic telemetry set-mode ghost  # record nothing at all
```

Mosaic Desktop has the same switch on its Telemetry page. `M0SAIC_TELEMETRY=ghost` sets the mode for one process, and `DO_NOT_TRACK=1` turns sending off. A dev checkout, `npm link` and the test suites never transmit; only published builds carry an endpoint.

## What leaves in `standard`

An anonymous install id, closed enums, counts and histograms: renders by outcome, a duration histogram, which features and CLI commands were used and how often, which built-in templates were opened by catalog id (community and third-party templates are counted, never named), coarse OS and architecture, major.minor versions, and install and update events. One day summary per UTC day plus a today-so-far update. Error reports are off unless you turn them on.

Never: file paths or names, template code, props, media, ffmpeg arguments, stderr, error messages, your layouts, or anything personal. Creating a Layout share link adds one to a share count; the layout itself stays with you.

## Outside the modes

Activating a license key tells m0saic.io the key's id, the outcome and the app version, in every mode. Never the key, your email or your install id, and never combined with it. It is one request, never retried.

Mosaic Desktop also makes two plain GETs that carry no identifier: a once-a-day version check against the releases page (it never downloads or installs anything by itself) and, only while the official community repo is enabled, a signed community template sync every few hours. `M0SAIC_NO_UPDATE_CHECK=1` turns the first off; the Templates page turns the second off.

## The browser app

app.m0saic.io sends the same kind of usage counts and stores no id in your browser. The server keeps a per-day hash of your address and browser string with a key that rotates daily, and never the address itself. Global Privacy Control and Do Not Track turn it off.
