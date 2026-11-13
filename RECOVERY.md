# Recovery plan

**Prepare before installing.** These are proposed operating procedures based on the project history and the official references below, not an assurance that every host can be recovered the same way. This r1 preparation did not perform a boot, rollback, removal or recovery drill with 0.7.43.

An independent management NIC, console path, configuration backup and known-good boot/install media are prerequisites. A console routed through this CCR is not independent. A backup of configuration is not a backup of VM disks. Do not repeatedly reset an unstable card or force driver unload.

## Host still reachable through independent management

1. Stop new testing, move or shut down dependent workloads and storage safely, and enter real maintenance mode using INSTALL.md.
2. Preserve a short timestamped error excerpt and package/version inventory privately. Avoid repeated full diagnostic collections on a hung system.
3. To prevent subsequent module loading, use the explicit module name:

```sh
esxcli system module set -m alc-community -e false
esxcli system module get -m alc-community
```

Check the result. This does not prove the current module is unloaded or that DMA has stopped. Do not follow it with a forced unload. Preserve the configuration after a successful change:

```sh
auto-backup.sh
```

4. Arrange a controlled reboot only after dependencies are safe. Confirm the module is disabled/not loaded on the next boot. Then remove the package through its VIB/component owner as described in INSTALL.md. If a removal fails, preserve the error instead of deleting files.
5. A disabled-module setting may survive package changes. Re-enabling it is a separate deliberate action after the cause is understood, not a routine step to bypass recovery.

## Host fails to boot or enters a PSOD loop

Do not rely on SSH becoming available. Use the independent console and the site's recovery plan.

**Previous bootbank, where available:** Broadcom documents pressing **Shift+R while the hypervisor is starting**, inspecting the proposed recovery image and confirming only when it is the intended known-good state. This is conditional recovery, not a universal undo. Additional package changes can replace the retained previous image; a matching base-build number alone does not tell you which driver payload it contains. Review Broadcom KB 316592 before relying on it. Do not assert that a rollback restored all configuration or every VIB; verify afterward.

**No usable rollback:** on a disposable/recoverable lab host, a qualified operator may follow the server's powered-off hardware procedure to remove the test card, boot using an independent NIC, and disable/remove the driver through the package manager. This is not hot removal. Account for downtime, boot dependencies and backups first. If the host still cannot boot, use the tested host-recovery or reinstall/restore plan; this package provides no guaranteed universal boot flag.

Do not edit bootbank contents, disable IOMMU or bypass additional security policies as a generic recovery recipe for this release. Do not reset the CCR repeatedly in an attempt to repair the ESXi image.

## Host boots but networking changed

Use independent management or the console to compare the new NIC inventory and vSwitch/vDS bindings with the recorded inventory. Do not guess by vmnic number. Restore the known-good management path before changing workloads. Never remove the remaining working path during remote troubleshooting.

## RouterOS panics while ESXi remains available

Stop the test that triggered the event. Observe whether RouterOS and the interfaces recover; no recovery time is guaranteed. Do not repeatedly call reset/disable or unload the driver against a nonresponsive card. Keep the alternative path active and capture the relevant time window. If a supout is collected, confirm that its timestamp/hash belongs to the new incident and share it privately as described in SUPPORT.md.

## Close the recovery

Verify package/image state, module enabled/loaded state, NIC ownership and management/storage paths. Recheck the acceptance/security settings against the recorded baseline rather than blindly assigning a default. Keep the incident and the exact VIB hash associated. Recovery success is not approval to resume a stress loop.

References: [ESXCLI system](REFERENCES.md#official-command-and-recovery-references), [ESXCLI software](REFERENCES.md#official-command-and-recovery-references), and [Broadcom rollback KB](REFERENCES.md#official-command-and-recovery-references).
