# Sources and reference scope

## Current project material

This publication uses the supplied 0.7.51 public-package README, CHANGELOG, VALIDATION, install/recovery/support documents, LICENSE, two VIB variants, offline bundle, checksum file and static package-check report. The exact distributed assets and their hashes are listed in [VALIDATION.md](VALIDATION.md).

The current project validation summary is the source for hardware-test claims. Runtime logs and source/build files are not included. Earlier 0.7.43 and development results are marked as historical; they are not silently converted into 0.7.51 tests. Private Git history, old installers and internal reports are excluded from this handoff.

Local publication checks independently verified the three installer hashes, VIB descriptors and payload checksums, embedded namespace/version identities, LICENSE equality, bundle ZIP integrity and identical inner/standalone 7.0/8.0 VIBs. These checks are not runtime validation or signature verification.

## Official command and recovery references

The ESXCLI software/system references were consulted again on 2026-11-16 for command semantics only. A latest-version reference does not guarantee the same options exist on every older host: check that host's local `--help`, especially on 6.7. No support or endorsement of this driver is implied.

- [Broadcom ESXCLI software reference](https://developer.broadcom.com/xapis/esxcli-command-reference/latest/namespace/esxcli_software.html): VIB/component operations, dry-run, staged VIB changes and signature-bypass behavior. Component commands in this release are only for 7.0/8.0.
- [Broadcom ESXCLI system reference](https://developer.broadcom.com/xapis/esxcli-command-reference/latest/namespace/esxcli_system.html): maintenance, module state and controlled reboot.

The recovery/privacy references below are retained from the supplied guides; their presence does not establish a new recovery rehearsal:

- [Broadcom KB 316592 — Reverting to a previous version of ESXi](https://knowledge.broadcom.com/external/article/316592/reverting-to-a-previous-version-of-esxi.html): conditional previous-bootbank recovery and its limits.
- [Broadcom KB 320884 — Unsigned VMkernel modules](https://knowledge.broadcom.com/external/article/320884): VIB-level signing context.
- [MikroTik Supout.rif documentation](https://help.mikrotik.com/docs/spaces/ROS/pages/328106/Supout.rif): configuration/log inclusion in support files; share diagnostics privately as described in SUPPORT.md.

External references do not extend the tested compatibility matrix or supersede independent recovery preparation. Proposed runbooks remain distinct from the project's reported runtime results.
