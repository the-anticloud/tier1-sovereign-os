# SOVEREIGN_OS — Educator's Teaching Guide

## Course Fit: operating systems, AI system integration, security engineering, infrastructure

## 3-Week Module: Designing a Sovereign AI Operating System

### Week 1: What Makes an OS "Sovereign"?
**Lecture Topics:**
- Sovereignty in computing: data ownership, audit control, no forced updates
- SOVEREIGN_OS kernel design: microkernel vs. monolithic for AI workloads
- Resource isolation: GPU partitioning, memory namespacing, process sandboxing
- The Anticloud philosophy: why local-first matters at the OS layer

**Lab Exercise:**
```python
import sovereign_os as sos
# Inspect resource partitions
partition_manager = sos.PartitionManager()
print(partition_manager.list_gpu_partitions())
# Allocate a partition for an inference workload
partition = partition_manager.allocate(
    name="pax_inference_01",
    gpu_fraction=0.5,
    memory_gb=8
)
print(f"Allocated partition: {partition.id}, GPU: {partition.gpu_fraction}")
```

### Week 2: AI Service Management
**Lecture Topics:**
- SOVEREIGN_OS service registry: how AI components register and discover each other
- Health monitoring and automatic restart policies
- Capability-based access control for AI services
- Update management: immutable service images, rollback

**Lab Exercise:**
```python
import sovereign_os as sos
registry = sos.ServiceRegistry()
service = sos.Service(
    name="miirai_chat",
    image="./miirai_chat.simg",
    capabilities=["gpu:read", "network:local", "storage:/data"],
    health_check={"endpoint": "/health", "interval_sec": 30}
)
registry.register(service)
registry.start("miirai_chat")
status = registry.status("miirai_chat")
print(f"Service status: {status.state}, uptime: {status.uptime_sec}s")
```

### Week 3: SOVEREIGN_OS Integrating the Full Anticloud Stack
**Lecture Topics:**
- Bootstrapping the Anticloud stack on SOVEREIGN_OS
- AIOSS as the system-level audit bus
- Network isolation: air-gapped vs. local-network-only deployments
- Disaster recovery: snapshot, restore, and key escrow

**Lab Exercise:**
```python
import sovereign_os as sos
stack = sos.AnticloudStack.from_manifest("anticloud_manifest.yaml")
stack.boot()
print(stack.health_report())
stack.snapshot("snapshot_001")
```

## Exam Questions
1. Explain capability-based access control. How does it differ from discretionary access control, and why is it better suited for AI service isolation?
2. Describe the bootstrapping sequence for SOVEREIGN_OS. What services must start before PAX_INFERENCE_CORE can accept requests?
3. Design a disaster recovery procedure for a SOVEREIGN_OS deployment that hosts 50 active user sessions. What state must be preserved, and what can be re-derived?
