# Cloudflare One Client MDM Configurator

A web-based tool for generating `mdm.xml` configuration files for the [Cloudflare One Client](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/) (formerly WARP) on Windows, macOS, and Linux.

## Overview

This tool provides a clean, professional interface to build MDM deployment files for Cloudflare One Client. It supports the full set of MDM parameters as documented in the [official Cloudflare documentation](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/parameters/), including the latest `environment` parameter for FedRAMP High deployments.

## Features

- **Multi-platform** — Generate configurations for Windows, macOS, and Linux
- **Single or multi-organization** — Support for the `configs` array used to switch between Zero Trust orgs
- **Windows multi-user** — Per-user WARP registrations on shared Windows devices
- **Windows pre-login** — Connect with a service token before user login
- **FedRAMP High** — New `environment` parameter for FedRAMP-authorized deployments
- **External Emergency Disconnect** — Configure HTTPS polling, fingerprint, and interval
- **Endpoint overrides** — For Cloudflare China and partner-network deployments
- **Live XML preview** — Syntax-highlighted preview that updates as you type
- **Inline validation** — URL, IP, port, and SHA-256 fingerprint format checks
- **One-click copy / download** — Copy XML to clipboard or download `mdm.xml`

## Platform support

| Feature                          | Windows | macOS | Linux |
| -------------------------------- | :-----: | :---: | :---: |
| Single / multi-organization      |   Yes   |  Yes  |  Yes  |
| FedRAMP High (`environment`)     |   Yes   |  Yes  |  Yes  |
| Multi-user mode                  |   Yes   |   —   |   —   |
| Pre-login                        |   Yes   |   —   |   —   |
| NetBIOS over TCP/IP              |   Yes   |   —   |   —   |
| All other parameters             |   Yes   |  Yes  |  Yes  |

## Deployment paths

| Platform | Default location                                      |
| -------- | ----------------------------------------------------- |
| Windows  | `C:\ProgramData\Cloudflare\mdm.xml`                   |
| macOS    | `/Library/Application Support/Cloudflare/mdm.xml`     |
| Linux    | `/var/lib/cloudflare-warp/mdm.xml`                    |

On macOS, the file can also be wrapped as a `.plist` or `.mobileconfig` for deployment via Jamf, Intune, Kandji, JumpCloud, or other MDM tools.

## Supported parameters

### Top-level (Windows only)

| Parameter   | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| `multi_user`  | Enable multiple user registrations per Windows device       |
| `pre_login`   | Connect WARP before Windows login using a service token     |

### Organization

| Parameter                              | Description                                                                            |
| -------------------------------------- | -------------------------------------------------------------------------------------- |
| `organization`                         | Your Zero Trust team name                                                              |
| `display_name`                         | User-friendly name shown in the client GUI (required when using `configs`)             |
| `environment`                          | **NEW** — `normal` or `fedramp_high` (client v2025.9.558.0+)                           |
| `gateway_unique_id`                    | DoH subdomain for DNS-only policy enforcement                                          |
| `auth_client_id`                       | Service token Client ID                                                                |
| `auth_client_secret`                   | Service token Client Secret                                                            |
| `service_mode`                         | `warp`, `1dot1`, `proxy`, `tunnelonly`, or `postureonly`                               |
| `proxy_port`                           | SOCKS proxy port (used only with `proxy` service mode)                                 |
| `support_url`                          | URL for user support (`https://` or `mailto:`)                                         |
| `switch_locked`                        | Prevent users from disabling the client                                                |
| `auto_connect`                         | Auto-reconnect interval in minutes (0–1440)                                            |
| `onboarding`                           | Show or hide the privacy-policy onboarding screens                                     |
| `enable_post_quantum`                  | Enable post-quantum key agreement (requires MASQUE)                                    |
| `warp_tunnel_protocol`                 | `masque` (default) or `wireguard`                                                      |
| `enable_pmtud`                         | Enable Path MTU Discovery                                                              |
| `enable_netbt`                         | Enable NetBIOS over TCP/IP (Windows only)                                              |
| `external_emergency_signal_url`        | HTTPS endpoint for the External Emergency Disconnect signal                            |
| `external_emergency_signal_fingerprint`| SHA-256 fingerprint validating the emergency signal endpoint                           |
| `external_emergency_signal_interval`   | Polling frequency in seconds (minimum 30, default 300)                                 |
| `override_api_endpoint`                | Override the client orchestration API IP                                               |
| `override_doh_endpoint`                | Override the DoH IP (DNS-only mode)                                                    |
| `override_warp_endpoint`               | Override the WARP ingress IP and UDP port (e.g., `203.0.113.0:500`)                    |

### Service modes

| Mode          | Description                                              |
| ------------- | -------------------------------------------------------- |
| `warp`        | Gateway with WARP — Traffic and DNS (default)            |
| `1dot1`       | Gateway with DoH — DNS only                              |
| `proxy`       | Local SOCKS proxy mode                                   |
| `tunnelonly`  | Traffic only (Secure Web Gateway, no DNS filtering)      |
| `postureonly` | Posture only — Device information only                   |

## Notes on deprecations

The legacy `enabled` property has been removed. Use `switch_locked` together with `auto_connect` instead. This tool always emits these parameters together (as Cloudflare requires) so output is compatible with current client versions.

## Usage

1. Open `index.html` in any modern browser (no build step required).
2. Choose your **target operating system** and **deployment type**.
3. Fill in organization settings — the live preview updates as you type.
4. Configure optional features (emergency disconnect, endpoint overrides, multi-user, pre-login).
5. **Copy** the XML to clipboard or **Download** `mdm.xml`.
6. Deploy via your MDM tool to the appropriate path on the target device.

## Disclaimer

This is an unofficial tool. Always verify generated profiles against the [official Cloudflare documentation](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/parameters/) before deploying to production.

## References

- [MDM deployment parameters](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/parameters/)
- [Managed deployment overview](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/)
- [Switch between Zero Trust organizations](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/switch-organizations/)
- [Windows multi-user support](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/windows-multiuser/)
- [Windows pre-login](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/deployment/mdm-deployment/windows-prelogin/)

## License

MIT
