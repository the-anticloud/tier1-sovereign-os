# Deploy Guide — SOVEREIGN_OS
## Prerequisites
- Linux 5.15+ LTS (Ubuntu 22.04 or Debian 12). Root access for hardening.
- dm-verity capable storage. Optional: Secure Boot with custom key enrollment.

## Environment
- Bare metal or VM (no nested virt for GPU). 32GB RAM for full stack. NVMe SSD for AIOSS.

## Apply hardening
```bash
sudo python -m sovereign_os apply_hardening --profile anticloud_medical
# AppArmor profiles for PAX, encrypted AIOSS partition, kernel module restrictions
```

## Air-Gap
All packages pre-installed. No internet required post-hardening.

## AIOSS Integration
Boot integrity hash appended at every boot:
```bash
python -m sovereign_os log_boot --aioss /var/lib/anticloud/boot_integrity.aioss
```

## Register PAX as service
```bash
sudo python -m sovereign_os register_service   --name anticloud-pax --exec "python -m pax_inference_core"   --gpu-memory-limit 14336 --aioss-chain /var/lib/anticloud/pax.aioss
```

## Verification
```bash
python -m sovereign_os verify_boot --aioss /var/lib/anticloud/boot_integrity.aioss
python -m sovereign_os health_report
```
