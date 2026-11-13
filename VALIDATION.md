# Validation and artifact identity

**Publication revision:** r1, prepared 2026-11-11. **Driver:** unchanged `0.7.43-1community`.

This document separates what was measured in the original project from what can be checked directly in this archive. Packaging checks are not hardware tests or a production certification.

## Exact assets in this release

| Asset | Bytes | SHA-256 |
|---|---:|---|
| `dist/alc-community-0.7.43-1.vib` | 42642 | `2442fdc243b2b14f314a177773159d2613874a15d1d9c8b0f0ca2dd84f21a2b3` |
| `dist/alc-community_0.7.43-1-offline_bundle.zip` | 45451 | `ed75e4c19e23367a64f5d1bb08570774f60dad142f6fb129b70f806cf7428453` |
| Module inside the VIB: `usr/lib/vmware/vmkmod/alc-community` | 103776 | `5e2b053f4bc5724dccb2c72b679086ec1be558be37c6fdb4cb899a20a7a26daa` |

The VIB's vendor field is `CMTY`; the component is `alc-community` version `0.7.43-1`. The module requires VMKAPI `v2_10_0_0`. These identifiers do not establish compatibility outside the reported test scope.

The three `dist/` files and root LICENSE match the original supplied public archive byte for byte. Hashes identify bytes, not signature trust or proof that those bytes ran in every historical test.

## Results reported by the supplied release README

| Reported result | Attribution/limit |
|---|---|
| 12-hour soak, 47 forced RouterOS link drops, zero full driver resets, zero RouterOS reboots | Preserved from the original public README. Per-run date, RouterBOOT version, mapper, test topology and exact tested binary hash were not supplied with this summary. |
| 12-hour soak in RouterOS passthrough mode, zero errors | Preserved from the original public README. Per-run date/hash and detailed error criteria were not supplied with the summary. |
| ESXi U3j/U3k; RouterOS 7.24.5/7.25; OptiPlex 7020 and R520 | Reported project test scope, not a complete matrix of this exact binary on every combination. |
| Throughput listed in README | Project observations for the stated paths, not guaranteed per-port speed. |

These two 12-hour claims have not been replaced with older 0.7.35 runs. They also have not been independently re-executed or promoted to stronger claims by this repack.

## Historical evidence and vLCM boundary

The supplied project bitacora documents 0.7.35 vDS/vCenter testing, including traffic, file hashes, failover, NIOC and persistence after a host reboot. It records successful depot import and component dry-run, **not a completed cluster-image remediation**. Those records support development history, not an automatic certification of this differently named 0.7.43 component. Private logs and laboratory identifiers are intentionally not redistributed.

| Stage for the exact public artifact | Status established during this packaging revision |
|---|---|
| ZIP CRC, asset hashes, VIB/bundle consistency | Checked locally; results in `PACKAGE_CHECKS.json` |
| Binary, license and original asset checksum preservation | Checked locally against the supplied archive |
| VIB signature | Empty signature member; no vendor signature added |
| Module load / attach / TX/RX / unload | No new hardware execution |
| Recovery and staged-install runbooks | Proposed/documented procedures; no new rehearsal on hardware |
| vLCM import, dry-run and full cluster-image remediation | No new execution with this repack; full remediation not established by the supplied evidence |
| Error injection, teardown failure handling, multi-card compatibility | Not certified; source/error-path review remains open |

## Record required for stronger validation

For each future claim, retain the tested VIB and module hashes, exact driver/build options, ESXi build, RouterOS and RouterBOOT versions, platform, native versus guest ownership, MTU/L2MTU, effective DMA mapper, traffic path, duration and number/type of recovery events. Include data-integrity checks, failed/incomplete intervals, reset/panic counters and cleanup outcome. Publish an anonymized summary while retaining raw evidence privately.

Do not describe zero failures in one finite run as a guaranteed absence of failures. Keep separate: ordinary link loss, administrative interface changes, RouterOS reboots, full driver resets, TCP recovery and IOMMU faults.
