# Sources and reference scope

## Project sources

The public material is based on the supplied `alc-community-public.zip`: README, CHANGELOG, LICENSE, VIB, bundle and published checksums. Binary identities are in VALIDATION.md and `RELEASE_MANIFEST.json`.

The accompanying project bitacora/continuity provided historical context for 0.7.35 and the limits of vLCM validation. They are not included because the complete files contain internal laboratory details. Historical findings are not relabeled as tests of the exact 0.7.43 artifact. The publication review supplied with the project informed the packaging changes; it is not vendor certification.

## Official command and recovery references

Consulted on 2026-11-11 only for command/operating semantics, not proof that this driver is supported:

- [Broadcom ESXCLI software reference](https://developer.broadcom.com/xapis/esxcli-command-reference/latest/namespace/esxcli_software.html): VIB/component operations, dry-run, staged VIB changes and signature-bypass behavior.
- [Broadcom ESXCLI system reference](https://developer.broadcom.com/xapis/esxcli-command-reference/latest/namespace/esxcli_system.html): maintenance, module state and controlled reboot. Check the target host's local `--help` as well.
- [Broadcom KB 316592 — Reverting to a previous version of ESXi](https://knowledge.broadcom.com/external/article/316592/reverting-to-a-previous-version-of-esxi.html): conditional previous-bootbank recovery and its limits.
- [Broadcom KB 320884 — Unsigned VMkernel modules](https://knowledge.broadcom.com/external/article/320884): VIB-level signing context.
- [MikroTik Supout.rif documentation](https://help.mikrotik.com/docs/spaces/ROS/pages/328106/Supout.rif): support-file contents. This older documentation site is marked frozen; it still describes configuration/log inclusion. Private-sharing guidance in SUPPORT.md is this release's policy.

External references do not add an endorsement, a tested compatibility claim or permission to skip independent recovery preparation. Proposed runbooks remain distinct from the project's reported runtime results.
