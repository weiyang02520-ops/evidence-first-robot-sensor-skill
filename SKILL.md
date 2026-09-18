---
name: evidence-first-robot-sensor
description: Use when inspecting, integrating, debugging, or handing off ROS, Jetson, RDK, UWB, camera, radar, or mobile-robot hardware. Enforce read-only probes, provenance labels, and live-evidence gates before claiming deployment or hardware success.
---

# Evidence-First Robot and Sensor Work

## Purpose

Turn robot and auxiliary-sensor work into an auditable sequence of observations, authorized changes, and verification. Keep software startup, target writes, device detection, and real-scene behavior as separate claims.

This skill is for engineering assistance and handoff. It does not authorize motion, system changes, driver installation, reboots, or destructive synchronization.

## Activate when

- The task involves ROS/ROS 2, Jetson, RDK, UWB, event cameras, RGB/APS cameras, mmWave/radar, CAN, serial devices, Nav2, mobile bridges, or robot networking.
- Someone asks whether a sensor, node, service, topic, port, or deployment is really working.
- A repository or device must be handed off, migrated, patched, or recovered.
- Logs, screenshots, simulated messages, or a launch command might be mistaken for hardware evidence.

Do not activate for ordinary programming tasks that do not touch a robot, device, deployment target, or hardware claim.

## Non-negotiable evidence states

Use these labels exactly and do not silently upgrade one state into another:

- `LOCAL_ONLY`: files or tests exist on the current machine only.
- `TARGET_WRITTEN`: a change was transferred to the target, but its effect is not verified.
- `TARGET_VERIFIED`: the target reports the expected files, process, service, topic, or device state after the change.
- `REMOTE_VERIFIED`: an independent remote observer, real sensor stream, real-scene trial, or user-visible behavior confirms the claim.
- `NOT_VERIFIED`: the available evidence is insufficient or contradictory.

For every important claim, record the target, timestamp, command or observation, expected result, actual result, and evidence state.

## Operating procedure

### 1. Establish scope and safety

State the exact target, current connection, requested outcome, and what is out of scope. Treat physical motion, power, `/boot`, kernel modules, package installation, reboots, and broad file replacement as high-risk actions. If the user has bounded the task as non-invasive, remain read-only.

Ask for authorization before a high-risk change. Before any authorized change, identify a recoverable backup, rollback path, and the smallest file or service scope.

### 2. Capture a baseline before changing anything

Collect only relevant read-only facts:

- host and target identity, network path, time, and working directory;
- battery/power and service status when relevant;
- running processes, ROS nodes, topic names, topic rates, and message timestamps;
- USB, serial, I2C, camera, CAN, and kernel enumeration;
- current git branch, commit, modified files, and checksums for files in scope.

Do not infer a node, device, or sensor from a package name, kernel module, configured topic, or stale screenshot alone.

### 3. Separate the observation chain

Report these layers independently:

1. software or launch command starts;
2. process/service remains alive;
3. expected device or transport enumerates;
4. expected topic or port carries fresh data;
5. calibration, timing, and coordinate conventions are plausible;
6. real-scene behavior is correct and repeatable.

A pass at an earlier layer is not a pass at a later layer. For example, an empty video-device list, a camera-driver error, or a synthetic replay cannot be described as real-camera detection.

### 4. Investigate from evidence, not from the desired story

For each symptom, write:

- confirmed facts;
- observations that are absent or contradictory;
- the smallest set of hypotheses;
- the next discriminating read-only probe;
- the condition that would justify a change.

Prefer short, batched probes on unstable links. Preserve raw outputs and timestamps. Do not call a virtual machine copy, local staging directory, or generated test stream a target or physical-device result.

### 5. Make authorized changes minimally

Use compare-and-merge or a focused patch. Avoid blind recursive copies, delete-synchronization, whole-repository replacement, and unreviewed configuration overwrites. Keep robot-local drivers, odometry, navigation, calibration, and launch behavior unless the change explicitly covers them.

After a write, re-run the baseline checks and label the result. If the target is not reachable or the probe is inconclusive, stop at `TARGET_WRITTEN` or `NOT_VERIFIED`.

### 6. Handoff with a verification ledger

End with a compact ledger:

| Claim | Evidence | State | Remaining gap |
|---|---|---|---|
| launch starts | timestamped command output | `TARGET_VERIFIED` | real sensor still untested |
| device enumerates | target enumeration | `TARGET_VERIFIED` | calibration pending |
| real scene works | repeatable observation | `REMOTE_VERIFIED` | none / list limitation |

Include exact next commands only when they are safe and relevant. Motion remains under onsite human control.

## Response contract

Use this order:

1. one-sentence human-language conclusion;
2. evidence state and what it proves;
3. what it does not prove;
4. likely causes ranked by discriminating evidence;
5. safe next probe or authorized change;
6. handoff ledger.

If evidence is missing, say `NOT_VERIFIED` instead of filling the gap with confidence.

## Stop conditions

Stop and report the boundary when:

- the target identity or connection is uncertain;
- a command could move the robot or alter a protected system area without authorization;
- the only support is a screenshot, package name, stale PID, configured topic, or synthetic replay;
- a checksum, backup, rollback path, or target read-back is missing for a requested write;
- live hardware evidence contradicts the local repository or launch logs.

