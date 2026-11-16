# Publishing alc-community 0.7.51

**Suggested tag:** `v0.7.51-preview.1`  
**Title:** `alc-community 0.7.51 — Community Preview`  
**Classification:** pre-release

## Files to publish

The handoff ZIP separates two destinations:

- `repositorio/`: copy its contents into the existing repository root, preserving `.github/ISSUE_TEMPLATE/bug_report.md`. These are public documents, LICENSE and the GitHub support template. Do not upload the outer `repositorio` folder as an extra directory level.
- `release/`: attach its four files to the new GitHub release: the two 0.7.51 VIBs, the 7.0/8.0 offline bundle and `SHA256SUMS`. This folder is a staging directory, not a combined ESXi depot.

Use `RELEASE_NOTES.md` as the release description. Keep the old 0.7.43 release and its assets intact. The repository's `.gitignore` continues to exclude `dist/`; installers are distributed as release assets, not through GitHub's automatically generated source archives.

There is one release `SHA256SUMS`, covering exactly the three new installer files. It does not cover repository documents or the outer handoff ZIP. Downloaded installers must be checked against their matching filenames. No stale whole-package manifest or historical audit JSON is included in the handoff.

## Preserved and changed

The supplied 0.7.51 VIBs, offline bundle and LICENSE are unchanged byte for byte. No recompilation, code patch, metadata edit, signature or runtime test was performed during publication preparation. The bundle's VIB is identical to the standalone 7.0/8.0 VIB. The dedicated 6.7 variant has its own namespace, package version and hash.

Public documentation was brought forward to the new version, obsolete links were removed, package selection was separated by host version, and the reported runtime tests were distinguished from static checks. Full-remediation and public-0.7.43-upgrade claims are not inferred from version order or a dry-run.

The distribution contains no private Git history, prior installers, diagnostic logs, firmware images, configuration backups or internal development material. Only public release documents and the three new supplied installers are included.

The LICENSE text is retained verbatim, including its historical ESXi 8 title and FreeBSD attribution. The current compatibility scope is in README/VALIDATION; the license header was not rewritten inside the binary packages.

## Boundaries

The descriptors still permit live install/removal and do not require maintenance. The guides deliberately require a maintenance window and independent recovery access; the unchanged metadata does not enforce that policy. These procedural changes are not a hardware rehearsal.

The supplied project reports no data-path/offload/recovery logic change in 0.7.51. Existing RouterOS failure modes, limited runtime evidence for exact-artifact long runs, unverified vLCM remediation and the pending error-path audit remain documented. Packaging is not vendor certification, legal/provenance clearance or production support.
