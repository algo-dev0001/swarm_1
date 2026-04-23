# Swarm SN124 Drone Agent

Autonomous drone navigation agent for Bittensor Swarm Subnet 124 (SN124).

## Overview

FSM-based drone controller with environment-adaptive flight parameters. Uses an ONNX goal detector to identify landing platform positions from a 128×128 depth camera.

## Architecture

- **4-mode FSM**: takeoff → search → navigation → landing
- **ONNX goal detector** (`goal_detector.onnx`): depth + state → platform visibility probability + 3D position
- **Depth-based obstacle avoidance**: `_safe_ctx` / `_pick_ctx` with SLERP direction smoothing
- **Environment-adaptive parameters** (Track D):
  - Mountain/village environments (distant search area > 45m): drone rises to 12m altitude before transit, clearing terrain peaks
  - Environments with search area > 35m: landing patience (30 steps) before returning to search, improving goal re-acquisition near platform

## Performance (120 seeds)

| Environment | Success | Collision | Score |
|-------------|---------|-----------|-------|
| City (T1) | 92.3% | 0.0% | 0.807 |
| Open (T2) | 86.7% | 6.7% | 0.704 |
| Mountain (T3) | 84.0% | 4.0% | 0.626 |
| Village (T4) | 54.2% | 8.3% | 0.430 |
| Warehouse (T5) | 100.0% | 0.0% | 0.934 |
| Forest (T6) | 85.7% | 4.8% | 0.793 |
| **Overall** | **83.3%** | **4.2%** | **0.702** |

## Files

| File | Description |
|------|-------------|
| `drone_agent.py` | Flight controller (`DroneFlightController` class) |
| `goal_detector.onnx` | ONNX goal detection model (~14 MB) |
| `requirements.txt` | Python dependencies |

## Submission

The validator expects `submission.zip` containing `drone_agent.py`, `goal_detector.onnx`, and `requirements.txt` at the zip root.
