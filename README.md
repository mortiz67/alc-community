# alc-community
## Native ESXi 6.7, 7.0 and 8.0 driver for the MikroTik CCR2004-1G-2XS-PCIe

**0.7.51 Community Preview**

`alc-community` lets an ESXi host use the card's four host-facing RouterOS interfaces (`ether-pcie1` through `ether-pcie4`) as `vmnic` uplinks. These are not four physical SFP ports.

This is a **binary-only community release** for advanced users and lab evaluation. It is not affiliated with, endorsed by or supported by MikroTik, VMware or Broadcom. It is not vendor-certified, a VMware Fling, or a production support commitment.

> **Read before installing:** kernel drivers can interrupt the entire host. Known RouterOS firmware issues can reboot the card and interrupt all four interfaces. Use an independent management NIC and a recovery console whose network path does not depend on this card. Back up the host configuration, evacuate workloads and enter maintenance mode before changing packages. Do not remove or replace a driver that carries active management, storage or cluster traffic. Start with [INSTALL.md](INSTALL.md) and [RECOVERY.md](RECOVERY.md).

0.7.51 is a new driver build that follows the published `0.7.43-1community` preview (0.7.44 to 0.7.50 were internal builds). It adds ESXi 7.0 U3 and ESXi 6.7 U3 support, in two packages built from the same source. Both VIB variants sort newer than `0.7.43-1community`; an in-place update from that published version has not yet been established by the supplied test record. See [INSTALL.md](INSTALL.md), [CHANGELOG.md](CHANGELOG.md) and [VALIDATION.md](VALIDATION.md).

## Status and scope

| Item | Scope |
|---|---|
| Driver version | `0.7.51` |
| Package for ESXi 7.0 U3 / 8.0 U3 | VIB `0.7.51-1community`, component `alc-community` / `0.7.51-1` (VMKAPI `v2_9_0_0`) |
| Package for ESXi 6.7 U3 | VIB `0.7.51-0.67community` (VMKAPI `v2_5_0_0`); VIB only |
| ESXi test scope reported by the project | 8.0 Update 3 (U3j build 25429389; U3k build 25595708 with 0.7.43), 7.0 Update 3 (U3n build 21930508), 6.7 Update 3 (build 19195723) |
| RouterOS test scope reported by the project | 7.24.5 and 7.25 |
| Hardware test scope reported by the project | Dell OptiPlex 9010 (Ivy Bridge, PCIe 3.0) for 0.7.51; Dell OptiPlex 7020 (Haswell, PCIe 3.0 x16) and Dell PowerEdge R520 with earlier builds |
| Device | CCR2004-1G-2XS-PCIe; PCI `1969:1083`, four functions |
| Package | CommunitySupported, unsigned; Secure Boot is not supported by this release |
| Other combinations | ESXi 9, ESXi 6.7/7.0/8.0 updates other than U3, a genuine AR8151, other cards and multiple CCR cards in one host are not validated by this release |

Reported test environments are not a claim that every release was tested on every listed combination. [VALIDATION.md](VALIDATION.md) separates reported results, artifact checks and remaining evidence gaps.

## Features

- Four PCIe functions, each exposed as a `vmnic`. RouterOS supplies the advertised link speed: normally 25000 Mb/s, or the SFP speed in RouterOS passthrough mode.
- MTU up to 9000. TX checksum offload and TCP segmentation for IPv4 and IPv6; **segmentation runs in the driver**. VLAN insertion/removal also runs in the driver.
- MSI interrupts with a polling fallback.
- New in 0.7.51: negotiated PCIe link diagnostics, a warning for slot limits, PCIe device/AER status logging, and shorter log lines that preserve counters. The data path, offloads and link-recovery logic are reported unchanged from 0.7.43.
- Shared DMA-engine handling for the card's function-0 requester ID; the project reports operation with ESXi DMAIOMMU applied.
- Link recovery that keeps or adopts ring indexes without a full reset in the tested cases. Full-reset fallback paths remain; this does **not** eliminate RouterOS's `hung_task` issue. After a RouterOS reboot, the driver attempts to restore PCI configuration and reinitialize the card.
- Project-reported use with standard/distributed switches, NIC teaming, failover, NIOC and vCenter 8.0.3. An offline component bundle is included for ESXi 7.0 and 8.0. Full vLCM cluster-image remediation is **not established by the evidence supplied for this release**.

## Performance and reported validation

The project reports the following earlier-build iperf3 TCP observations. These are historical performance references, not a fresh benchmark of every 0.7.51 package:

| Test path | Reported throughput |
|---|---|
| Between ESXi netstacks on different functions, MTU 1500 | About 8.4 Gb/s (one flow), 9.7 Gb/s (four flows) |
| Same type of path, MTU 9000 | 12–13 Gb/s |
| VM to VM over a distributed switch | 7.4–7.9 Gb/s |
| RouterOS passthrough between SFP28 ports | About 8.9 Gb/s |

They also report a 12-hour soak with 47 forced RouterOS link drops, zero full driver resets and zero RouterOS reboots, plus a separate 12-hour passthrough soak with zero errors, with earlier builds. The supplied 0.7.51 test record covers native ESXi 6.7 U3 and 8.0 U3j, plus ESXi 7.0 U3n in a guest with all four functions assigned. [VALIDATION.md](VALIDATION.md) separates these exact-asset tests from earlier-build and long-run results. No new hardware tests were performed while preparing this publication.

Throughput depends on the path, RouterOS processing and host resources. An advertised 25000 Mb/s link is not a throughput guarantee.

## Requirements

Stay within the reported lab scope above and use independent management/recovery access. Secure Boot must already be disabled under your organization's policy; do not weaken an existing security baseline merely to try this preview.

For every `ether-pcieN`, follow the project's conservative rule **RouterOS L2MTU >= ESXi MTU + 18**. Examples: L2MTU 1600 for ESXi MTU 1500; at least 9018 for MTU 9000. Confirm the entire path before sending jumbo frames. The driver does not establish the remote L2MTU for you. Oversized frames have caused RouterOS failures in project testing.

**RouterOS passthrough mode** means forwarding between the card's SFP and PCIe interfaces. **PCI DirectPath/passthrough to a VM** transfers PCI functions to a guest. These are different tests. Do not split this card's four functions between host and guests; guest operation needs its own compatible driver and validation.

## Download and installation

Download from the **Assets** section of the [0.7.51 Community Preview release](https://github.com/mortiz67/alc-community/releases/tag/v0.7.51-preview.1). The repository contains documentation; installers are attached to the release, not committed to the repository.

| File | ESXi | Purpose |
|---|---|---|
| `alc-community-0.7.51-1.vib` | 7.0 U3, 8.0 U3 | Standalone VIB |
| `alc-community_0.7.51-1-offline_bundle.zip` | 7.0 U3, 8.0 U3 | Alternative component/depot bundle containing the same 7.0/8.0 VIB |
| `alc-community-0.7.51-0.67-esxi67.vib` | 6.7 U3 | Dedicated 6.7 VIB; no component bundle |
| `SHA256SUMS` | All | SHA-256 values for the three installer files above |

**Choose the package for the host's ESXi version.** Do not install both variants or merge them into one depot. The offline bundle is for 7.0/8.0 only. ESXi 6.7 cannot satisfy its VMKAPI dependency; do not bypass that error with `--force`.

Download the matching installer and `SHA256SUMS` together. Compute the installer's SHA-256 on your workstation and compare it with that exact filename's entry. If all three installer files are in the same directory, verify the entire list with:

```sh
sha256sum -c SHA256SUMS
```

If you downloaded only one installer, the full-list command will also report the other files as missing; this is not a hash mismatch for the file you downloaded. Hashes detect changed bytes; they are not a vendor signature or proof of trust. GitHub's automatic **Source code** archives contain the repository documentation, not these binary release assets.

**Follow [INSTALL.md](INSTALL.md) rather than pasting a one-line install-and-reboot command.** This release keeps the package name `alc-community`; the same-name update path and its validation limits are documented there.

## Known issues

Read [KNOWN_ISSUES.md](KNOWN_ISSUES.md) before deployment. In particular: RouterOS `hung_task` panics, oversized-frame failures, shared DMA identity, PCI configuration loss after a card reboot, and occasional TCP stalls observed in the ESXi stack remain operational considerations. Recovery duration is not guaranteed.

## Support and privacy

Community support is through the issue tracker associated with this release, with no SLA. See [SUPPORT.md](SUPPORT.md) and the issue template.

**Do not attach `supout.rif`, configuration backups, complete support bundles, credentials or unredacted logs to public issues.** Agree on a private diagnostic channel first. No private inbox or upload service is created by this package.

## Name and license

The name uses the community-driver naming convention; `alc` refers to the FreeBSD alc(4) ancestry. It is not maintained by VMware, Broadcom or the FreeBSD Project.

BSD-2-Clause; see the unchanged [LICENSE](LICENSE). Portions are derived from FreeBSD alc(4), Copyright (c) 2009 Pyun YongHyeon. The VIB installs the same license at `/opt/alc-community/LICENSE`.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND. MikroTik, CCR2004, VMware, vSphere and ESXi are trademarks of their respective owners.
