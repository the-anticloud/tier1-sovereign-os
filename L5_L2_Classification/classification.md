# L5 Narrow / L2 General Classification — SOVEREIGN_OS
**Platform:** Anticloud | **PAX:** 27B | **IP:** USPTO pending 2026, Anticloud FZ LLE

## L5 Narrow
SOVEREIGN_OS specializes in hardened Linux configuration for AI-first sovereign deployments:
kernel module restrictions, GPU memory isolation, AIOSS chain partition, PAX model protection
via dm-verity and Secure Boot. Not a general OS distribution — targeted hardening of Ubuntu LTS
or Debian for the Anticloud security surface.

## L2 General
Hardware-agnostic: same hardening profile for x86_64 servers, ARM embedded boards, and
defense workstations. Anticloud platform runs identically across all hardware targets.

## PAX Integration
PAX 27B runs as a systemd service under SOVEREIGN_OS cgroup isolation. Boot integrity
verified via dm-verity before PAX is allowed to start. GPU memory allocated exclusively
to the PAX cgroup.

## AIOSS Audit Relevance
Boot integrity hash, kernel module loads, service start/stop events all chained.
Any tampering with the OS layer is detectable before a single PAX inference runs.

## Regulatory
NIST SP 800-53 (security controls), FedRAMP Moderate, IEC 62443 (industrial cybersecurity)
