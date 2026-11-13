# Installation and removal

**Scope:** manual evaluation on a locally installed, stateful ESXi 8.0 U3 host. These reorganized runbooks have not been replayed on hardware during publication preparation. Rehearse them on a disposable lab host. They do not cover Auto Deploy/PXE hosts or unattended cluster remediation.

Commands are individual steps, not an installation script. Inspect every result and stop on any error, skipped operation or unexpected package change. A dry-run is not a runtime test. External command semantics are referenced in [REFERENCES.md](REFERENCES.md).

## 1. Safety gate — before any package change

Arrange a maintenance window and a verified configuration backup. Keep compatible installation media and an independent recovery console available. Move management, active storage and cluster dependencies off this card. Confirm the console's network does not traverse the same CCR2004. Evacuate or shut down workloads and coordinate HA/vSAN maintenance under the site's procedures.

Do not proceed when this would leave the host without management or storage access. Review [RECOVERY.md](RECOVERY.md) first. Use a dedicated test host for initial evaluation.

Record the current state, privately:

```sh
vmware -vl
esxcli software acceptance get
esxcli software vib list
esxcli software component list
esxcli system module list
esxcli network nic list
```

After workloads and dependencies are handled, enter maintenance mode:

```sh
esxcli system maintenanceMode set --enable true --timeout 300
esxcli system maintenanceMode get
```

Continue only after successful entry and `Enabled`. Do not substitute the software install flag `--maintenance-mode`; that flag is not entry into real maintenance mode.

## 2. Integrity and security decision

Verify the release hashes on your workstation as described in README. Copy the chosen original asset to `/tmp` on the test host and compare its SHA-256 again. The two expected hashes are in `dist/SHA256SUMS` and [VALIDATION.md](VALIDATION.md).

The VIB is unsigned. `--no-sig-check` bypasses signature/acceptance checks and can produce a vSphere security alert. Use it only after accepting that risk for this lab. Do not add `--force`, disable the firewall, change IOMMU settings or disable `ExecInstalledOnly` to get past a failure. Secure Boot compatibility is not added by repackaging.

Record the original acceptance level before changing it:

```sh
esxcli software acceptance set --level=CommunitySupported
```

If the procedure is abandoned, restore the recorded policy only after verifying that all installed packages satisfy it. Do not blindly raise the level while community packages remain.

## 3. Choose one installation route

### A. Standalone VIB — staged activation on reboot

This publication proposes `--no-live-install` for initial evaluation so the change is staged for the next boot. Confirm it exists in the host's `esxcli software vib install --help`. This is a documented CLI option, **not a newly tested driver installation result**. It is not suitable for PXE-booted hosts.

First preview the transaction:

```sh
esxcli software vib install -v /tmp/alc-community-0.7.43-1.vib --no-sig-check --no-live-install --dry-run
```

Review the affected packages. If successful and exactly the intended transaction, perform it:

```sh
esxcli software vib install -v /tmp/alc-community-0.7.43-1.vib --no-sig-check --no-live-install
```

Check success and the staged image before proceeding:

```sh
esxcli software vib get -n alc-community --rebooting-image
```

The unchanged descriptor permits live installation/removal and does not require maintenance. This runbook deliberately requires maintenance and staged activation; **the metadata does not enforce this policy for you**.

### B. Component from the offline bundle — alternative lab route

Use the inner `dist/alc-community_0.7.43-1-offline_bundle.zip`, not the outer release ZIP. Do not combine this with route A for the same installation.

```sh
esxcli software sources component list -d /tmp/alc-community_0.7.43-1-offline_bundle.zip
esxcli software component apply -d /tmp/alc-community_0.7.43-1-offline_bundle.zip --no-sig-check --dry-run
```

Review the component and complete proposed operation. Only after approval:

```sh
esxcli software component apply -d /tmp/alc-community_0.7.43-1-offline_bundle.zip --no-sig-check
```

This path can apply live according to the unchanged component/VIB metadata. The documented `component apply` options are not identical to `vib install`; do not assume it accepts `--no-live-install`. Keep the host isolated and in maintenance, review the result and plan a controlled reboot even when metadata reports otherwise.

### C. vSphere Lifecycle Manager — evaluation only

The component can be inspected/imported into a test depot. **Import, CLI dry-run and actual cluster-image remediation are separate validation stages.** This archive does not establish end-to-end remediation of this exact release. Do not use it as a ready-to-run cluster-remediation recipe or convert a production cluster's image management solely to try it. See [VALIDATION.md](VALIDATION.md).

## 4. Controlled reboot — a separate decision

Do not reboot automatically after an error or a skipped operation. With the successful transaction reviewed, management/recovery ready, workloads stopped/evacuated and maintenance still enabled, authorize the planned reboot:

```sh
esxcli system shutdown reboot --delay 10 --reason "alc-community lab installation"
```

The project has observed RouterOS restarting with the host. All services traversing the card may be interrupted; arrange recovery for VMs and vCenter separately.

## 5. Verify before returning workloads

From independent management or the console:

```sh
esxcli software vib get -n alc-community
esxcli system module get -m alc-community
esxcli network nic list
grep -F 'alc-community: init' /var/log/vmkernel.log
cat /opt/alc-community/LICENSE
```

Expect version `0.7.43-1community` and four host-facing functions for one card. Link state depends on RouterOS and configuration; `Up` is not a substitute for traffic/integrity tests. Use the current boot's timestamps, not older log entries. Check that the module was not intentionally disabled by a previous development recovery procedure; do not force it to load.

Do not assume vmnic numbers or port-group bindings survived a driver-name change. Verify management and storage mappings and keep MTU 1500 until RouterOS L2MTU has been checked. Test only a dedicated network before restoring dependencies. Exit maintenance only after those checks and the site's approval.

## 6. Future upgrades with the same package name

A documentation-only r1 repack does not require reinstalling an identical VIB. For a future **newer** `alc-community` VIB, repeat the safety gate and use `software vib update` with a dry-run first; select the exact new filename. Different package names require migration, not `vib update`. Never overwrite a published version with changed bytes under the same filename.

## 7. Planned removal

First repeat the safety gate and confirm no active dependency uses the CCR interfaces. Use the package ownership route that installed it.

For a standalone VIB, preview a staged removal:

```sh
esxcli software vib remove -n alc-community --no-live-install --dry-run
```

Only after reviewing success and the intended package list:

```sh
esxcli software vib remove -n alc-community --no-live-install
```

`--no-live-install` is also the documented option name for staging a VIB removal; it is not a typo. Check local help. Review the result, perform a separate controlled reboot, and verify that the driver no longer owns devices.

For a component-managed installation, inspect the installed component first and use `esxcli software component remove -n alc-community --dry-run`; after reviewing the result, use the same command without `--dry-run`. It may remove live. Do not strip an individual VIB out of a managed image without accounting for its desired component/image state.

Stop on failure; never force unload/remove or delete the module/map files manually. After completed removal, restore the recorded acceptance level only if all remaining packages permit it. [RECOVERY.md](RECOVERY.md) covers a failed boot rather than a normal uninstall.
