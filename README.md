# Evidence-First Robot and Sensor Skill

An Agent Skills-compatible workflow for ROS, Jetson/RDK, UWB, cameras, radar, CAN, serial devices, and mobile robots.

## What it does

This skill helps an agent distinguish:

- a local file from a target deployment;
- a launch command from a healthy process;
- an enumerated device from a live data stream;
- a replay or simulation from a real sensor;
- a target write from a verified result;
- software startup from real-scene robot behavior.

It uses the evidence states `LOCAL_ONLY`, `TARGET_WRITTEN`, `TARGET_VERIFIED`, `REMOTE_VERIFIED`, and `NOT_VERIFIED`.

## Installation

Copy this directory into an Agent Skills-compatible skills directory, or install the repository with the client-specific skill manager.

## Intended use

Use it for diagnosis, integration, deployment handoff, and recovery planning. It is deliberately observation-first and does not grant permission to move a robot, reboot a target, modify `/boot`, install drivers, or overwrite a workspace.

## License

MIT. See [LICENSE](LICENSE).

