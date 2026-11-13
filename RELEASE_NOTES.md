# alc-community 0.7.43 — Community Preview

Native ESXi 8.0 U3 networking for the MikroTik CCR2004-1G-2XS-PCIe. Binary-only VIB and offline component bundle, for advanced users and lab evaluation. Not vendor-certified or affiliated with MikroTik, VMware or Broadcom.

## Driver highlights

Four host-facing interfaces, shared DMA handling for function 0's requester ID, MSI with polling fallback, MTU up to 9000, TX checksum support, IPv4/IPv6 segmentation and VLAN handling in the driver. Link recovery keeps/adopts ring indexes in the reported test cases to avoid unnecessary full resets. Full-reset fallback and known RouterOS failure modes remain.

The project reports a 12-hour run with 47 forced link drops and no full driver resets or RouterOS reboots, plus a separate 12-hour passthrough run with no errors. These are original project-reported results; see VALIDATION.md for evidence scope and missing per-run attribution. No new hardware test was performed in this publication revision.

## r1 publication changes

Clean ZIP without Git history; revised README; separate installation, recovery, validation and support guides; private-diagnostics guidance; whole-package manifest and checksums. **No executable, VIB descriptor, bundle metadata or LICENSE changes.** Internal version remains `0.7.43-1community`. Identical existing installations need no reinstall for r1.

## Before use

Read INSTALL.md and RECOVERY.md. Use independent management and console access, a backup and maintenance mode. Secure Boot is not supported. Do not split this card's PCI functions across owners. Verify RouterOS L2MTU before jumbo traffic. Known firmware panics can interrupt all four interfaces; recovery time is not guaranteed. vLCM image remediation is not claimed validated by this publication.

Assets: the original VIB, offline bundle and `dist/SHA256SUMS`. The outer archive is documentation plus assets, **not** itself an ESXi depot. Do not attach supout/configuration bundles to public issues; see SUPPORT.md.
