# Deploy Guide — L_ROS2CORE
**Tier:** TIER_9_ROBOTICS_IOT | **Stack:** Python 3.11, rclpy (ROS2 Humble), DDS (CycloneDDS), PAX 27B, AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
Python 3.11+, ROS2 Humble, CycloneDDS 0.10+. Ubuntu 22.04. Any robot hardware.

## Environment
Ubuntu 22.04 (required for ROS2 Humble). 8GB RAM. Robot hardware or simulation.

## AIOSS Integration
```bash
aioss init --module L_ROS2CORE --output ./l_ros2core.aioss
aioss append --chain ./l_ros2core.aioss --payload ./output.bin --module L_ROS2CORE
aioss verify --chain ./l_ros2core.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="L_ROS2CORE",
    aioss_chain="./L_ROS2CORE.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./L_ROS2CORE.aioss --verbose
python -m L_ROS2CORE.tests.smoke
```
