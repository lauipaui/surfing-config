# Surfing Remote Mihomo Configuration Template

[中文](README.md) | **English**

A sanitized configuration template for the Surfing Android module. This repository publishes [`config.yaml`](config.yaml); real proxy subscriptions and effective runtime configuration remain local to each device.

## How updates work

1. Download the template from `main`.
2. The device's existing updater merges its locally retained subscription block.
3. Validate the candidate using the actual Mihomo binary installed on that device.
4. Back up the previous configuration and restart Surfing only after validation succeeds.

The documented device workflow checks for template changes every six hours. **No updater or installer is included here**: cloning this repository alone does not enable automatic updates. A provider's `interval: 86400` controls subscription refresh, not remote-template refresh.

## Use and publish

```sh
git clone https://github.com/lauipaui/surfing-config.git
cd surfing-config
```

Template URL:

```text
https://raw.githubusercontent.com/lauipaui/surfing-config/main/config.yaml
```

- Review changes locally and preserve the subscription-block start/end markers.
- **Never commit a real subscription in place of `__LOCAL_SUBSCRIPTION_URL__`.**
- Merge local subscription data before treating the template as a runtime configuration.
- Only changes merged into `main` update the public URL above. An unmerged PR does not publish a device update.

Example device-side validation; adapt the binary and path to your installation:

```sh
mihomo -t -f /path/to/merged-config.yaml
```

There is no generic CI or core binary in this repository. Valid YAML alone does not prove core compatibility, successful rule downloads or subscription connectivity.

## Network and security

The current template enables `allow-lan: true`, exposes the controller on `0.0.0.0:9090`, and has an empty `secret`. Set authentication, restrict the bind address, or restrict sources through the device firewall in the **local configuration** before using untrusted Wi-Fi or shared networks. Do not publish real controller credentials.

IPv6 and keep-alive are currently disabled; rule sets and UI assets are downloaded externally. These are template choices, not universally optimal defaults. Verify against the actual core, proxy mode, power use and stability requirements. This is not a BoxProxy eBPF-specific configuration.

## Rollback and troubleshooting

- Validation failure: keep the working configuration; do not force a restart.
- Regression after updating: restore the local backup made by the device updater. This repository has no one-click rollback tool.
- Download failure: check DNS and HTTPS for GitHub raw, subscriptions and rule-set sources separately.
- Empty nodes: confirm the local subscription was merged; a placeholder URL cannot supply nodes.

## Attribution and licensing

Mihomo, Surfing, rule sets and Zashboard belong to their respective projects. No standalone `LICENSE` is included here; verify each component's and data source's terms. Public visibility is not a grant of unrestricted redistribution rights.
