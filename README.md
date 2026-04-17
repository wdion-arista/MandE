# Media & Entertainment (MandE)

AVD lab deployment for the MandE customer fabric — L3LS-EVPN with ISIS underlay, eBGP EVPN overlay, and EVPN multicast across SITE1–SITE5.

> **This repo is a nested lab inside [ceos-act-projects](https://github.com/wdion-arista/ceos-act-projects).** All `make` commands, playbooks, and scripts are sourced from that parent repo. Clone and open from there.

---

## Prerequisites

- [OrbStack](https://orbstack.dev/) (macOS) with an ARM64 Debian VM set up per the [ceos-act-projects README](https://github.com/wdion-arista/ceos-act-projects)
- Docker available inside the VM (DooD)
- VSCode with the **Remote - SSH** and **Dev Containers** extensions

---

## Installation

### 1. Clone the parent repo

```sh
git clone https://github.com/wdion-arista/ceos-act-projects.git
cd ceos-act-projects
```

### 2. Clone this lab into `labs/`

```sh
git clone https://github.com/wdion-arista/MandE.git labs/MandE
```

### 3. Open in the devcontainer

In VSCode:

1. Open the `ceos-act-projects` folder (via Remote SSH → `orb` if using OrbStack)
2. **Reopen in Container** → select **CAP - MandE - AVD GH Containerlab-Docker-Outside-Docker**

This uses `.devcontainer/ceos-act-projects-base-MandE/devcontainer.json` and mounts `labs/MandE` as the workspace.

---

## Environment Setup

Inside the container, set up your `.env` file:

```sh
cd labs/MandE
cp .env-example .env
```

Edit `.env` and fill in your tokens:

| Variable | Description |
| --- | --- |
| `ARISTA_TOKEN` | (MANDATORY) arista.com profile API key (for cEOS image downloads) |
| `CE_ACT_APKEY` | (OPTIONAL) Arista CE ACT API key |
| `CVAAS_TOKEN_LAB` | (OPTIONAL) CVaaS token for lab tenant |
| `CVAAS_TOKEN_PROD` | (OPTIONAL) CVaaS token for prod tenant |
| `CVP_TOKEN_LAB` | (OPTIONAL) On-prem CVP token |
| `CEOS_ARM_IMAGE` | (OPTIONAL) cEOS image version (e.g. `4.36.0F`) |
| `LABPASSPHRASE` | (MANDATORY) Password for lab user |

Source it before running any make targets:

```sh
source .env
```

Ensure an SSH key exists — `make prod-build` (and other playbooks) will fail without one:

```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

> If you already have a key at `~/.ssh/id_ed25519` (or `~/.ssh/id_rsa`), you can skip this step.

---

## Common Workflows

Run `make help` to see all targets. Key workflows:

### Build AVD configs

```sh
make prod-build               # Build PROD configs for all sites
make clab-build               # Build ContainerLab configs
make act-build                # Build ACT configs
```

### ContainerLab (local cEOS)

```sh
make containerlab-get-image-arm                  # Download & import cEOS image (ARM64 — Apple Silicon)
make containerlab-get-image-amd64               # Download & import cEOS image (AMD64 — Intel/AMD)
make clab-containerlab-build-deploy-default      # Full workflow: build → deploy → default configs
make clab-containerlab-build-deploy-avd          # Full workflow: build → deploy → AVD configs as startup
make containerlab-destroy                        # Tear down the lab
```

> **cEOS image not downloaded?** Make sure `ARISTA_TOKEN` is set in your `.env` and sourced, then run the target that matches your host architecture:
>
> | Host | Command |
> | --- | --- |
> | Apple Silicon (ARM64) | `make containerlab-get-image-arm` |
> | Intel / AMD (x86-64) | `make containerlab-get-image-amd64` |

### ACT (Arista Cloud Test)

```sh
make act-config-build-deploy-to-act   # Full ACT workflow (all steps)
```

> Wait 5–20 minutes after `make ce_act_labs_deploy` for devices to come up.

### Deploy to CVaaS

```sh
make prod-deploy-cvaas        # Deploy PROD configs to CVaaS
make act-deploy-cvaas         # Deploy ACT configs to CVaaS
```

### Validate

```sh
make prod-validate            # Validate PROD network state
make clab-validate            # Validate ContainerLab network state
```

---

## Fabric Overview

| Group | Role | Platform |
| --- | --- | --- |
| `BLUE_SPINES` / `BLUE_LEAFS` | Blue plane | 7280SR3 |
| `RED_SPINES` / `RED_LEAFS` | Red plane | 7280SR3 |
| `PURPLE_LEAFS` | Access/campus | 720XP |

Sites: **SITE1 – SITE5** across the `EASTCOAST_FABRIC`.
