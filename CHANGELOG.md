# Changelog

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
