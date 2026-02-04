# Cloudflare WARP MDM Configurator

A web-based tool for generating `mdm.xml` configuration files for deploying the Cloudflare WARP client via MDM (Mobile Device Management).

## Overview

This tool provides a user-friendly interface to create MDM configuration files for Cloudflare WARP deployments on **Windows** and **macOS**. It supports all available MDM parameters as documented in the [official Cloudflare documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/deployment/mdm-deployment/).

## Features

- **Multi-Platform Support** - Generate configurations for Windows and macOS
- **Single Organization** - Configure WARP for a single Zero Trust organization
- **Multi-Organization** - Allow users to switch between multiple Zero Trust organizations
- **Windows Multi-User Support** - Enable per-user WARP registrations on shared Windows devices (Windows only)
- **Windows Pre-Login** - Connect WARP before Windows login using service tokens (Windows only)
- **External Emergency Disconnect** - Configure emergency disconnect polling endpoints

## Platform Support

| Feature | Windows | macOS |
|---------|---------|-------|
| Single/Multi Organization | ✅ | ✅ |
| Multi-User Mode | ✅ | ❌ |
| Pre-Login | ✅ | ❌ |
| NetBIOS over TCP/IP | ✅ | ❌ |
| All other parameters | ✅ | ✅ |

## Supported Parameters

### Top-Level Parameters (Windows Only)
| Parameter | Description |
|-----------|-------------|
| `multi_user` | Enable multiple user registrations per Windows device |
| `pre_login` | Connect WARP before Windows login with service token |

### Organization Parameters (All Platforms)
| Parameter | Description |
|-----------|-------------|
| `organization` | Your Zero Trust team name |
| `display_name` | User-friendly name shown in WARP GUI |
| `gateway_unique_id` | DoH subdomain for DNS-only policy enforcement |
| `auth_client_id` | Service token Client ID |
| `auth_client_secret` | Service token Client Secret |
| `service_mode` | Operational mode (warp, 1dot1, proxy, tunnelonly, postureonly) |
| `proxy_port` | SOCKS proxy port for proxy mode |
| `support_url` | URL for user support (https:// or mailto:) |
| `switch_locked` | Prevent users from disabling WARP |
| `auto_connect` | Auto-reconnect interval in minutes |
| `onboarding` | Show/hide privacy policy onboarding screens |
| `enable_post_quantum` | Enable post-quantum cryptography |
| `warp_tunnel_protocol` | Tunnel protocol (masque or wireguard) |
| `enable_pmtud` | Enable Path MTU Discovery |

### Windows-Only Parameters
| Parameter | Description |
|-----------|-------------|
| `enable_netbt` | Enable NetBIOS over TCP/IP on WARP tunnel interface |

### External Emergency Disconnect
| Parameter | Description |
|-----------|-------------|
| `external_emergency_signal_url` | HTTPS endpoint for disconnect signal |
| `external_emergency_signal_fingerprint` | SHA-256 fingerprint for endpoint validation |
| `external_emergency_signal_interval` | Polling frequency in seconds (min 30) |

### Endpoint Overrides
| Parameter | Description |
|-----------|-------------|
| `override_api_endpoint` | Override API endpoint IP address |
| `override_doh_endpoint` | Override DoH endpoint IP address |
| `override_warp_endpoint` | Override WARP endpoint IP:port |

## Service Modes

| Mode | Description |
|------|-------------|
| `warp` | Gateway with WARP (default) |
| `1dot1` | Gateway with DoH (DNS only) |
| `proxy` | SOCKS proxy mode |
| `tunnelonly` | Secure Web Gateway without DNS filtering |
| `postureonly` | Device Information Only |

## Usage

1. Open `index.html` in a web browser
2. **Select your target operating system** (Windows or macOS)
3. Select deployment type (Single or Multi-Organization)
4. Configure platform-specific features if needed (Multi-User, Pre-Login for Windows)
5. Fill in organization settings
6. Configure optional parameters as needed
7. Download the generated `mdm.xml` file
8. Deploy via your MDM solution

### macOS Deployment

For macOS, place the `mdm.xml` file in `/Library/Application Support/Cloudflare/` or convert it to a `.plist` file for deployment via Jamf, Intune, Kandji, or other MDM tools.

## Disclaimer

This is an unofficial tool. Always verify generated profiles against the [official Cloudflare documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/deployment/mdm-deployment/) before deploying to production environments.

## References

- [MDM Deployment Parameters](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/warp/deployment/mdm-deployment/parameters/)
- [Windows Multi-User Support](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/warp/deployment/mdm-deployment/windows-multiuser/)
- [Windows Pre-Login](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/warp/deployment/mdm-deployment/windows-prelogin/)

## License

MIT
