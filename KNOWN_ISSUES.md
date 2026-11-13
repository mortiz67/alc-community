# Known issues and limits

These are the limits reported in the supplied project release documentation, with the scope of the claims clarified. They are not a claim that every future error has the same cause. No defect was fixed by this documentation-only revision.

## RouterOS hung-task panic

Project testing has observed RouterOS panics involving `al_ep_enable_work` after some endpoint/interface transitions, including certain RouterOS administrative operations. The driver tries to avoid the triggering reset sequence where possible; full-reset recovery paths still exist. Avoidance in the reported link-drop tests is not a firmware fix.

Outages around two to two-and-a-half minutes were observed in earlier tests. They are not a timeout contract or guaranteed recovery duration. A panic or reboot can interrupt all four interfaces and services traversing the card. Stop the triggering test and use the independent management/recovery path.

## Oversized frames and MTU

Frames larger than the configured `ether-pcie` L2MTU have caused RouterOS kernel failures. Apply the project's conservative **L2MTU >= ESXi MTU + 18** rule before traffic, including VLAN and jumbo tests. Start at MTU 1500. A vDS MTU change can affect uplinks; check both sides before sending large frames. The driver does not discover remote L2MTU automatically.

## Shared DMA requester ID and guest assignment

The project reports that the card uses function 0's DMA requester ID for its four functions. The driver uses shared DMA handling. Do not split functions between owners or use an IOMMU/ACS workaround as proof of physical isolation. PCI DirectPath of all four functions to one guest is distinct from RouterOS SFP passthrough and still needs guest-driver validation. Multi-card operation is not validated here.

## RouterOS reboot and PCI state

A card reboot can lose PCI configuration. With the driver loaded, it attempts restoration. Project tests with the driver absent required a host reboot for re-enumeration. Plan a maintenance window rather than relying on repeated card reboots to recover it.

## TCP stalls after link loss

The project traced a particular VMkernel TCP-stall pattern to the ESXi stack: ACK delivery continued and new connections worked. This is a laboratory diagnosis, not a blanket vendor confirmation or an explanation for every iperf error. Do not reset the card solely because one connection stalls while other traffic is healthy.

## Offload and performance limits

TCP segmentation and VLAN handling occur in the driver. The original release reports single-core RouterOS TSO behavior, no usable RX checksum-offload indication, and hardware statistics that read zero; driver counters supply statistics. Advertised link speed is not measured capacity. In RouterOS SFP passthrough mode, administratively disabling an SFP port was reported not to signal link-down to the host.

## Package and validation limits

The VIB is unsigned and CommunitySupported. Secure Boot compatibility and vendor certification are not provided. Existing live-install/live-remove metadata has been preserved; the manual maintenance policy must be followed. vLCM depot import is not proof of cluster-image remediation.

The supplied binary has not received a complete independent source/error-path audit. Handling of DMA unmap/destruction failures remains an engineering review item; a successful normal-path soak does not exercise those failures. Do not force unload/remove after failed teardown. Source changes or such failures need a separate tested driver build, not this repack.

See VALIDATION.md for the distinction between project-reported runs and checks performed on these archive bytes.
