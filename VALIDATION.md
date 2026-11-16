# Validation and artifact identity

## 0.7.51 Community Preview — 2026-11-16

This document separates **runtime results reported by the project** from **local checks of the supplied files**. The supplied input included a current README, changelog, validation summary and static-check report, but not the underlying private runtime logs or source/build tree. The handoff retains public documentation and current installers only; private audit material is not redistributed. Publication preparation did not rerun those host tests or independently certify their conclusions.

Both installers are new 0.7.51 driver builds. They are preserved byte for byte, together with the supplied 7.0/8.0 offline bundle. Hashes identify artifacts; they are not a manufacturer signature or proof of production readiness.

### Exact assets

| Asset | Bytes | SHA-256 |
|---|---:|---|
| `alc-community-0.7.51-1.vib` | 45864 | `dc6c74d9494cc6e266e6aa158f159bd06e60a34bf31c89e94e9d8fa9fda36ae5` |
| Module inside it: `usr/lib/vmware/vmkmod/alc-community` | 114440 | `af8628189b3cc46e489ccd0e3e8c458a6cf057a1a79cedf851ca8538a871d819` |
| `alc-community_0.7.51-1-offline_bundle.zip` | 48607 | `eb6d5fed59a702cd639c4848957fc1877f12b171b9251da3514e571ea0553819` |
| `alc-community-0.7.51-0.67-esxi67.vib` | 45830 | `5791b3b39fb2bb121d71d181eae67eb7ad5a044777ca12ff660107d78e39a382` |
| Module inside it: `usr/lib/vmware/vmkmod/alc-community` | 114408 | `ad28327105580ac80afcc3dddff414cc2c302a78ba429c36013e65b33d4cab15` |

7.0/8.0 package: VIB `CMTY_bootbank_alc-community_0.7.51-1community`, depends `vmkapi_2_9_0_0`, platforms 7.0/8.0; component
`alc-community` `0.7.51-1`; the VIB inside the bundle is byte-identical to the standalone VIB. 6.7 package: VIB
`CMTY_bootbank_alc-community_0.7.51-0.67community`, depends `vmkapi_2_5_0_0`, platform 6.7.0. The project reports checking version order with the
ESXi 8 image library: `0.7.43-1community < 0.7.51-0.67community < 0.7.51-1community`.

Publication preparation independently checked the three asset hashes, VIB archive/descriptor structure, all declared compressed/uncompressed payload checksums, payload file lists, ELF namespaces and version strings, driver map, and LICENSE equality. It also checked bundle and metadata ZIP CRCs and byte identity of the bundled and standalone 7.0/8.0 VIB. No executable was rebuilt, patched, installed or run.

The 6.7 VIB descriptor is `0.7.51-0.67community`, while its module retains the internal string `0.7.51-1community`. The namespace and hash distinguish the 6.7 binary. This packaging preserves those supplied bytes; it does not rename an embedded version to make the strings match.

The project reports these `vmkapi` ranges provided by `esx-base` (from the installation media / host): 6.7 U3 build 19195723: 2.2 to 2.5; 7.0 U3n build
21930508: 2.4 to 2.9; 8.0 U3j build 25429389: 2.5 to 2.13. So the 7.0/8.0 package cannot satisfy its dependency on 6.7 (confirmed on a 6.7 U3 host, below).

### Project-reported tests of these exact assets

| Asset | Environment | Result |
|---|---|---|
| 7.0/8.0 VIB | ESXi 8.0 U3j build 25429389, host install, Dell OptiPlex 9010, PCIe 8 GT/s x8, RouterOS 7.25 | Load/unload, ping tests on function 0 and 3, persistent install, 20 load/unload cycles with 0 warnings, 120 s iperf3 on all four functions (two pairs, 5031 and 4965 Mb/s, 0 retransmissions), recovery counters 0 |
| 7.0/8.0 VIB | ESXi 7.0 U3n build 21930508, guest on the same host with all four functions assigned | Same tests (4411 and 4383 Mb/s), and persistence after a guest reboot |
| Offline bundle | ESXi 8.0 U3j | Content check, `component apply --dry-run` only; not applied, no vLCM remediation |
| 6.7 VIB | ESXi 6.7 U3 build 19195723, host install, OptiPlex 9010, PCIe 8 GT/s x8, RouterOS 7.25 | `vib update` from 0.7.50-1community, load/unload, ping tests on function 0 and 3, persistent install, 20 load/unload cycles with 0 warnings, 120 s iperf3 on all four functions (5510 and 5489 Mb/s), persistence after a host reboot. Its module is also code-identical to the 0.7.50 6.7 build (same disassembly; only the two version-string bytes differ) |
| 7.0/8.0 VIB and bundle | ESXi 6.7 U3 build 19195723 | Refused as expected: `vib install`, `vib update` and `vib install -d <bundle>` all return `DependencyError` (requires `vmkapi_2_9_0_0`), host unchanged; `software component` does not exist on 6.7. With `--force` the VIB installs but the module does not load (`Invalid or missing namespace`) |

The project reports that a test build with one extra log line confirmed that warnings appear in `vmkwarning.log` on 7.0 U3n and 8.0 U3j.

### Earlier-build results reported by the project

| Build | Environment | Result |
|---|---|---|
| 0.7.50, 6.7 build | ESXi 6.7 U3 build 19195723, host install, OptiPlex 9010 | Load/unload and traffic tests, persistence after reboot, 1-hour soak with link drops: 0 errors |
| 0.7.50, 6.7 binary packaged for 6.7/7.0/8.0 | ESXi 8.0 U3j host | 6-hour soak, two TCP pairs: about 9.9 Gb/s in total, 0 seconds without traffic, recovery counters 0 |
| 0.7.47 (VMKAPI 2.9, earlier logging calls) | ESXi 7.0 U3n host install | Load/unload and traffic tests, 1-hour soaks with and without link drops: 0 errors |

Not established for 0.7.51: an in-place update from an installed 0.7.43-1community (version order only checked), long
soaks of the exact assets, vLCM remediation, Secure Boot, ESXi 9, multi-card hosts.


## Historical 0.7.43 and development results — not retests of 0.7.51

The earlier public preview reported a 12-hour run with 47 forced RouterOS link drops, no full driver resets and no RouterOS reboots, and a separate 12-hour RouterOS-passthrough run with no errors. Its summary did not supply all per-run dates, RouterBOOT versions, effective DMA mappers or exact tested binary hashes. These results remain historical and are not relabeled as long runs of 0.7.51.

Historical project records also cover U3j/U3k, the OptiPlex 7020 and PowerEdge R520, vDS/vCenter, VLAN, teaming/failover, NIOC, RSPAN and data-integrity testing. The earlier vLCM record established depot import and a component dry-run, not a completed cluster-image remediation. It does not automatically validate the new bundle.

Earlier public binaries remain in the earlier release; they are not included among this release's assets. The benchmark figures in README describe the earlier reported paths and are not guaranteed per-port capacity.

## Remaining evidence boundaries

The manual staged-install/update/removal and recovery guides are operating proposals, not additional rehearsals on hardware during publication preparation. Dependency/version ordering, component import or a successful dry-run do not prove a completed upgrade or remediation. Secure Boot, ESXi 9, multiple cards per host, exhaustive teardown-error handling and general hardware compatibility are not certified here.

For stronger validation, retain the exact VIB/module hashes, source/build identity, ESXi build, RouterOS and RouterBOOT versions, platform, native/guest ownership, MTU/L2MTU, effective mapper, traffic path, duration, recovery events, data-integrity results, incomplete intervals and cleanup outcome. Share a redacted summary publicly and keep raw diagnostics private. Zero failures in a finite run are not a guarantee of zero failures in other conditions.
