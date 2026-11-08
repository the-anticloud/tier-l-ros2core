# Developer Cookbook — L_ROS2CORE
**Stack:** Python 3.11, rclpy (ROS2 Humble), DDS (CycloneDDS), PAX 27B, AIOSS_FORMAT
**Domain:** ROS2 Core: ROS2 Humble infrastructure for all Anticloud robot platforms

## ROS2 graph monitoring
```python
from l_ros2core import ROS2CoreMonitor

monitor = ROS2CoreMonitor(
    pax_model="./pax-27b-q4.gguf",
    aioss_chain="./ros2core.aioss"
)

monitor.start()

# PAX-driven graph health check
health = monitor.pax_health_check()
for node_name, status in health.nodes.items():
    print(f"{node_name}: {status.state}")
    if not status.healthy:
        print(f"  Issue: {status.issue}")
        monitor.auto_restart(node_name)
```

## Secure DDS (SROS2)
```python
monitor.configure_dds_security(
    policy="./anticloud_sros2_policy/",
    enforce=True
)
```

## AIOSS Chain Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```
