# Support and diagnostic privacy

Community support is through the issue tracker associated with this release. There is no SLA or vendor support commitment. This package does not invent a maintainer email or a private upload endpoint.

## A useful public report

Include the driver version and VIB SHA-256, ESXi build, RouterOS/RouterBOOT versions, server model, one-card/multi-card and native/guest setup, MTU/L2MTU, a short reproduction, and the failure timestamp/timezone. Add a redacted NIC inventory and a small relevant driver-log excerpt. State whether management remained available, whether RouterOS uptime changed, and whether the same problem affects new connections.

Read-only examples:

```sh
vmware -vl
esxcli software vib get -n alc-community
esxcli system module get -m alc-community
esxcli network nic list
grep -F 'alc-community' /var/log/vmkernel.log
```

Inspect output locally before sharing. Limit it to the incident window and remove IP/MAC addresses, serials, hostnames, customer names and unrelated configuration. These commands do not authorize a reboot, a stress test or a change to RouterOS.

## Never attach sensitive diagnostics publicly

**Do not attach `supout.rif`, `autosupout.rif`, configuration backups, complete ESXi support bundles, private keys, tokens, passwords or unredacted logs to public issues.** RouterOS support files contain configuration and logs; even when passwords are not included, network details can be private. See the official Supout reference in REFERENCES.md.

For a RouterOS failure, request an agreed private diagnostic channel from the maintainer or MikroTik support first. Retain the original privately and share only the necessary material. Confirm that the supout date and hash identify the current incident, not a stale previous crash. Publicly state only that private evidence is available; do not upload it while waiting for a reply.

For a suspected security issue, request private contact without posting exploitation details or sensitive attachments. No support SLA is implied. Do not repeat a destructive reproduction on a production host merely to fill in a report.
