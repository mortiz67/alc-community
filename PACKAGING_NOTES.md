# Publication revision r1

Prepared 2026-11-11 from the supplied `alc-community-public.zip` (SHA-256 `c3e2c7b197a5043a4aa13e7877bf1da5b7f60c376ad48867be372e728447efb2`). This file describes editorial/packaging work, not new driver development or authorization to install on a host.

## Preserved

The VIB, offline bundle, `dist/SHA256SUMS`, LICENSE and existing `.gitignore` are copied without changes. Consequently the embedded driver version, empty VIB signature, live-install/live-remove flags, component identity and empty replacement/conflict relationships are also unchanged. No new signature, code fix or metadata enforcement is implied.

The original driver changelog entry is retained. The author's two reported 12-hour validation claims are retained with their evidence limits stated, rather than silently replaced with older project runs.

## Changed

Removed all `.git` history, objects, hooks and configuration from the distributable. No private development archive, logs, firmware, reverse-engineering notes, source headers, credentials or supout files are included. Revised the public README and added focused manuals, issue-report guidance, a validation scope document and release text.

Operational proposals in INSTALL/RECOVERY are labeled separately from actual tests. In particular, the staged VIB route is documented from ESXCLI semantics, not newly exercised on a host. There is no automatic install/uninstall script. Safety instructions appear before modifying commands.

## Publishing

Use `RELEASE_NOTES.md` as the release description and label it a community preview/pre-release. Use a fresh public repository or copy only the intended documentation; do not upload the private project's Git history. The preserved `.gitignore` excludes `dist/`, so upload the VIB, bundle and `dist/SHA256SUMS` explicitly as release assets. The full clean ZIP may also be offered as a convenience download.

Keep version and hash identity stable. `r1` is the archive/documentation revision, not a changed VIB. If code or VIB metadata changes, create a new driver/package build and refresh validation, not a silent replacement of these assets. No repository URL, contact mailbox or publication action is created by this archive.

## Open items, not silently solved by repackaging

Exact-hash attribution for the newly reported long runs; rehearsal of recovery; exact-component vLCM remediation; error-path source review for DMA teardown; general hardware compatibility and legal/provenance review. LICENSE is retained as supplied; this packaging work is not a legal clearance or production certification.
