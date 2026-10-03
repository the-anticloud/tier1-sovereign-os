# Developer Cookbook — SOVEREIGN_OS
**Stack:** Linux 5.15+, Python 3.11, systemd, cgroups v2, dm-verity, AppArmor

## Apply hardening profile
```bash
sudo python -m sovereign_os apply_hardening --profile anticloud_medical
# Profiles: anticloud_medical, anticloud_defense, anticloud_robotics, anticloud_minimal
```

## Verify boot integrity
```bash
python -m sovereign_os verify_boot --aioss ./boot_integrity.aioss
# BOOT_HASH = SHA3-256(kernel + initrd + cmdline) — must match AIOSS entry #1
```

## System health report
```python
from sovereign_os import SovereignOSMonitor
monitor = SovereignOSMonitor()
health = monitor.full_report()
print(f"Boot: {'VALID' if health.boot_valid else 'COMPROMISED'}")
print(f"AIOSS: {'INTACT' if health.aioss_valid else 'TAMPERED'}")
print(f"PAX service: {health.pax_status}")
```

## Set GPU cgroup for PAX
```bash
sudo python -m sovereign_os set_gpu_cgroup --service anticloud-pax --limit-mb 14336
```

## Performance
dm-verity: <5% I/O overhead. Huge pages for PAX weights:
`echo 'vm.nr_hugepages=8192' >> /etc/sysctl.conf && sysctl -p`

## Integration
Foundation for all other TIER_1 projects. Directly manages: MF_SO_PASSWORD_MANAGER (boot secret
unlock), AIOSS_FORMAT (chain partition), KAZCADE_RUNTIME (primary service), PAX_INFERENCE_CORE.
