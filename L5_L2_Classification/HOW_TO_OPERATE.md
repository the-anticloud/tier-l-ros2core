# How to Operate — L_ROS2CORE
**Platform:** Anticloud | **IP:** USPTO pending 2026, Anticloud FZ LLE, 0-1.gg

## Module Overview
L_ROS2CORE — ROS2 Core: ROS2 Humble infrastructure for all Anticloud robot platforms
Stack: Python 3.11, rclpy (ROS2 Humble), DDS (CycloneDDS), PAX 27B, AIOSS_FORMAT

## Daily Operations
1. `aioss verify --chain ./l_ros2core.aioss`
2. Check service health via api-oss-monitor
3. Review api-oss-logging for error-level events
4. Confirm PAX 27B is loaded and responding

## Incident Response
- Chain tamper: halt, notify compliance, restore from backup
- GPU OOM: reduce batch size, check memory leak
- High latency >2s P99: check queue depth, scale workers
- Compliance gap: run api-oss-compliance report

## Backup (nightly)
```bash
python -m api_oss_backup backup --sources ./l_ros2core.aioss --output ./backups/
```
