# alc-community 0.7.51 — Community Preview

Native ESXi networking for the **MikroTik CCR2004-1G-2XS-PCIe**, now targeting **ESXi 6.7 U3, 7.0 U3 and 8.0 U3** through two package variants. Binary-only, unsigned community packages for advanced users and lab evaluation; not affiliated with or certified by MikroTik, VMware or Broadcom.

This is a new driver build after the published **0.7.43 preview**, not a documentation-only repack. Versions 0.7.44–0.7.50 were internal builds.

## Choose the matching package

| Host ESXi | Release asset | Package version |
|---|---|---|
| **7.0 U3 or 8.0 U3** | `alc-community-0.7.51-1.vib` | `0.7.51-1community`, VMKAPI 2.9 |
| **7.0 U3 or 8.0 U3** | `alc-community_0.7.51-1-offline_bundle.zip` | Alternative component bundle containing that same VIB |
| **6.7 U3 only** | `alc-community-0.7.51-0.67-esxi67.vib` | `0.7.51-0.67community`, VMKAPI 2.5 |

`SHA256SUMS` verifies all three installer files. **Do not install both VIB variants, merge them into one depot, or force the 7.0/8.0 package onto 6.7.** There is no 6.7 component bundle.

## Changes since 0.7.43

- ESXi 7.0 U3 support through VMKAPI 2.9, plus a dedicated ESXi 6.7 U3 build through VMKAPI 2.5.
- PCIe negotiated-link diagnostics and warnings for slot limitations; PCIe device/AER status logging at load/stop.
- Log lines kept within the ESXi length limit and a common VMkernel logging path for both builds.
- The supplied changelog reports **no change to the data path, offloads or link-recovery logic**. This is not a fix for RouterOS firmware panics.

## Validation scope

The project reports tests of the exact 7.0/8.0 VIB on native ESXi 8.0 U3j and a 7.0 U3n guest with all four functions assigned, and of the exact 6.7 VIB on native ESXi 6.7 U3. These include load/unload, traffic on functions 0 and 3, 20 load/unload cycles and 120-second traffic on all four functions. Persistence tests are recorded in the validation table. The 6.7 update test started from an internal 0.7.50 package.

The offline bundle received a content check and component **dry-run**, not application or vLCM remediation. The longer soak results belong to earlier builds. An in-place update from public 0.7.43 and long soaks of the exact 0.7.51 assets are **not established** by the supplied record. Publication preparation checked package integrity but did not run hardware tests.

See [VALIDATION.md](https://github.com/mortiz67/alc-community/blob/v0.7.51-preview.1/VALIDATION.md) and [CHANGELOG.md](https://github.com/mortiz67/alc-community/blob/v0.7.51-preview.1/CHANGELOG.md).

## Before installing or updating

Read [INSTALL.md](https://github.com/mortiz67/alc-community/blob/v0.7.51-preview.1/INSTALL.md), [RECOVERY.md](https://github.com/mortiz67/alc-community/blob/v0.7.51-preview.1/RECOVERY.md) and [KNOWN_ISSUES.md](https://github.com/mortiz67/alc-community/blob/v0.7.51-preview.1/KNOWN_ISSUES.md). Use independent management and console access, a backup and maintenance mode. Secure Boot is not supported. Keep all four PCI functions under one owner and verify RouterOS L2MTU before jumbo traffic.

Both VIB variants sort newer than 0.7.43, but that version-order check is not a completed upgrade test. Choose the matching same-name update route and review a dry-run first. Do not replace assets in the previous published release.

Known RouterOS panics can interrupt all four interfaces; recovery is not guaranteed. Share only redacted excerpts in public issues; never attach supout or complete configuration/support bundles publicly. See [SUPPORT.md](https://github.com/mortiz67/alc-community/blob/v0.7.51-preview.1/SUPPORT.md).
