# Changelog

## 0.7.51-1community / 0.7.51-0.67community (2026-11-16)

New driver build. Next version after the published `0.7.43-1community` preview; versions 0.7.44 to 0.7.50 were internal
builds and were not published.

- Two packages built from the same source:
  - `0.7.51-1community` for ESXi 7.0 Update 3 and 8.0 Update 3 (VIB and offline bundle). The module now requires VMKAPI
    `v2_9_0_0` (ESXi 7.0 U3) instead of `v2_10_0_0`; ESXi 8.0 U3 provides it.
  - `0.7.51-0.67community` for ESXi 6.7 Update 3 (VIB only; ESXi 6.7 has no components or vLCM). The module requires
    VMKAPI `v2_5_0_0`. Its package release (`0.67`) is lower than the 7.0/8.0 package, so after a host upgrade from 6.7
    the 7.0/8.0 package has the higher update order; that combined OS/driver upgrade is not a newly validated procedure here.
- The project reports version ordering checked as `0.7.43-1community < 0.7.51-0.67community < 0.7.51-1community`. An in-place update from the published 0.7.43 package has not yet been established by the supplied test record; use the appropriate same-name update path with a dry-run first.
- PCIe diagnostics: the negotiated PCIe link is logged at load against the card maximum, with a warning when the slot
  limits it (for example a chipset x4 slot). PCIe error status (device status and AER) is logged and cleared at load,
  and logged again when the interface stops.
- Log lines are kept under the ESXi line-length limit, so counters are no longer cut from the log.
- Both builds log through the same VMkernel call (`vmk_LogNoLevel`); warnings still appear in `vmkwarning.log`.
- No change to the data path, offloads or link-recovery logic since 0.7.43. The 6.7 build differs only in the
  kernel interfaces it registers (VMKAPI 2.5 uplink layout and revision).

### 0.7.51 publication preparation

- Separated current release assets from repository documentation and excluded old installers and private Git history.
- Updated installation/package selection for ESXi 6.7 versus 7.0/8.0 and removed obsolete document links.
- Distinguished reported exact-asset tests from earlier-build soaks and static package checks.
- Preserved the supplied 0.7.51 VIBs, offline bundle and LICENSE byte for byte; no driver rebuild, metadata change or signature was added.

## Publication revision r1 (2026-11-11)

- Documentation-only Community Preview packaging; no new driver build.
- Removed Git history from the release ZIP and added installation, recovery, validation and private-support guidance.
- VIB, offline bundle, their existing checksums and LICENSE remain byte-for-byte unchanged.
- Driver history below is retained from the supplied changelog; it is not a new runtime validation.


## 0.7.43-1community (2026-11-04)

- First public release.
- Link-up handling: the driver adopts the ring indexes after a RouterOS `disable`/`enable` of an `ether-pcie`, and keeps
  them after an SFP link loss in passthrough mode. In both cases it avoids the reset sequence that can trigger the
  RouterOS `hung_task` panic.
- Supports the four PCIe functions and works with the ESXi IOMMU.
- Offloads: MTU up to 9000, TX checksum and TSO for IPv4 and IPv6 (segmentation in the driver), VLAN in the driver.
- The VIB installs the license at `/opt/alc-community/LICENSE`.
