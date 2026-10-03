# SOVEREIGN_OS — Student Getting Started

## What You'll Build
Hands-on experience with a sovereign AI operating system: register AI services, allocate GPU partitions, monitor health, and snapshot/restore a running deployment.

## Prerequisites
- Python 3.10+
- Linux preferred (or WSL2 on Windows)
- Basic understanding of processes and system calls

## Install
```bash
pip install sovereign-os
```

## First Working Example
```python
import sovereign_os as sos

# Start the SOVEREIGN_OS daemon (runs in background)
daemon = sos.Daemon.start(config="default")
print(f"Daemon running at: {daemon.socket_path}")

# Check system resources
resources = daemon.get_resources()
print(f"GPU: {resources.gpu_free_gb:.1f} GB free of {resources.gpu_total_gb:.1f} GB")
print(f"RAM: {resources.ram_free_gb:.1f} GB free")

# List registered services
for svc in daemon.list_services():
    print(f"  {svc.name}: {svc.state}")
```

## Register and Start an AI Service
```python
import sovereign_os as sos

daemon = sos.Daemon.connect()
service = sos.Service(
    name="my_chat_service",
    image="./miirai_chat.simg",
    capabilities=["gpu:read", "network:local", "storage:/data/chat"],
    health_check={"endpoint": "/health", "interval_sec": 30}
)
daemon.register(service)
daemon.start("my_chat_service")
print(daemon.status("my_chat_service"))
```

## Snapshot and Restore
```python
import sovereign_os as sos

daemon = sos.Daemon.connect()
snap_id = daemon.snapshot("snap_before_update")
print(f"Snapshot saved: {snap_id}")

# Simulate a bad update
daemon.stop("my_chat_service")
daemon.restore(snap_id)
print("Restored successfully")
```

## On Kaggle (loiskleinner account, T4 GPU)
```python
!pip install sovereign-os
# Use simulation mode on Kaggle (no root required)
import sovereign_os as sos
daemon = sos.Daemon.start(config="simulation")
```

## What's Next
- Explore GPU partition allocation for multiple services
- Try the capability-based access control system
- Read the EDUCATORS guide for microkernel architecture details
