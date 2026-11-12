<div align="center">

<img src="snap/gui/floci-gcp.png" alt="Floci GCP logo" width="128" />

# Floci GCP

**Run GCP-compatible services locally for development, testing and CI.**

[![floci-gcp](https://snapcraft.io/floci-gcp/badge.svg)](https://snapcraft.io/floci-gcp)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [Floci GCP](https://github.com/floci-io/floci-gcp), a GCP emulator with local storage and optional Docker-backed services.

</div>

---

## Getting Started

<a href="https://snapcraft.io/floci-gcp"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install floci-gcp
```

The emulator starts automatically on `http://127.0.0.1:4588`. Point your GCP SDK or tools at it as described in the [upstream documentation](https://floci.io/floci-gcp/).

Docker-backed services and the web console need Docker Engine, installed separately:

```bash
sudo snap connect floci-gcp:docker
sudo snap restart floci-gcp.daemon
```

## Configure

| Option | Default | Purpose |
|---|---|---|
| `addr` | `127.0.0.1` | Listen address |
| `port` | `4588` | GCP API port |
| `storage-mode` | `persistent` | `memory`, `persistent`, `hybrid` or `wal` |
| `project` | `floci-local` | Project used when a request names none |
| `base-url` | `http://localhost:<port>` | Address advertised in API responses |
| `tls` | `false` | Also serve HTTPS on `port` and 443 |
| `allow-unsafe-network-exposure` | `false` | Required to listen outside loopback or with `tls` |

```bash
sudo snap set floci-gcp port=4589
```

State lives in `/var/snap/floci-gcp/common/data`. Other [upstream settings](https://floci.io/floci-gcp/configuration/advanced/application-yml/) go in `/var/snap/floci-gcp/common/config/application.properties`; restart the daemon after editing.

## Limitations

- The emulator has no authentication: keep it on loopback or a trusted network.
- The `docker` interface is not connected automatically, and Docker publishes container ports on all interfaces.
- Init hooks run only in the daemon and cannot use Python or the gcloud CLI, which the snap does not include.
- `snap revert` does not roll back the emulator's data.

## Build

```bash
git clone https://github.com/sidalit/floci-gcp-snap.git
cd floci-gcp-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/floci-gcp-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/floci-io/floci-gcp/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/floci-gcp-snap/blob/main/LICENSE).
