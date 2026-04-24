
# Delta-GameGPT 🎮

[![Rank](https://img.shields.io/badge/Rank-5th_Preliminary-blue.svg)]() [![Rank](https://img.shields.io/badge/Rank-Top_40_Finals-orange.svg)]() 

> **Delta-GameGPT 🎮** is the open-source solution for the 2026 Tencent Game Security Technical Competition (Delta GameGPT Track). Earning **5th place in the Preliminary Round** and advancing as a **Top 40 Finalist**, this project achieves high-precision micro-action prediction and hallucination-free tactical narrative generation based on complex FPS game logs.

## 📖 Overview

This repository tackles the challenge of analyzing 20 seconds of highly dynamic, multi-agent multimodal game logs (including 3D coordinates, field of view, damage events, and skill usage) to predict a player's micro-actions in the future 5 seconds. Furthermore, it generates professional, highly compliant tactical commentary texts.

To overcome the "physical blindness" and "spatiotemporal hallucinations" common in End-to-End LLMs, this project introduces a **Strongly Decoupled Paradigm**. By splitting the pipeline into a rigorous **Logic Layer** and a constrained **Narrative Layer**, the system strictly binds language generation to immutable mathematical facts .

## ✨ Core Architecture & Innovations

### 🧠 1. Logic Layer (Micro-Action Prediction)
Dedicated strictly to high-dimensional tensor operations and time-series regression.
* **High-Density Feature Engineering:** Constructs a 320-dimensional feature space covering 150 time steps at a 0.133s resolution. This limit captures the Nyquist frequency of human reaction times, preventing the aliasing of high-speed gunfight micro-operations.
* **Feature Domains:** Extracted features include orthogonal pose encoding (smooths Yaw boundaries), kinematics derivatives (Jerk/Acceleration), Combat Geometry (Angle Consistency for backstab detection), and sliding window macro-states.
* **DualStream V3 Network:** A parallel sequence encoder architecture.
  * *Slow-Thought Stream (Bi-LSTM):* Captures continuous physical inertia and movement trajectories.
  * *Fast-Thought Stream (Transformer):* Anchors discrete, sudden burst events (e.g., sudden damage spikes) across long sequences.
* **Dynamic Multi-Pooling Gate:** Extracts transient (Last), macro (Mean), and extreme (Max) events via learnable gating weights, preventing the dilution of early critical actions.
* **Context-Initialized Decoder:** Cures "cold start amnesia" during autoregressive decoding by pre-loading historical context and macro-intent directly into the initial LSTM hidden states.

### 📝 2. Narrative Layer (Tactical Generation)
Dedicated to language rendering under strict factual boundaries, powered by DeepSeek V3.
***Multi-Agent Blackboard System:** Utilizes three collaborative agents—Tactical Analyst (A), Logic Interpreter (B), and Narrative Executive (C)—synchronized via a central data blackboard.
* **Grounding Gate:** Forces exact substring matching to ensure LLM-extracted events strictly trace back to the raw JSON facts, killing hallucinations at the source.
* **Constraint Compiler:** Scans the predicted logic card to inject hard rules and blackout banned gaming slangs (e.g., "peek", "wallhack", "aimbot").
* **Dual-Loop Critic & Refiner:**
  * *Inner Loop:* Uses Regex to check structural formatting and semantic conflicts (e.g., automatically downgrading generated text like "Double Kill" to "Fire suppression" if the logic layer predicted zero kills).
  * *Outer Loop:* Triggers macro replanning or a fallback rule-based generation mechanism if max retries are exceeded, ensuring 100% compliance.

### ⚡ 3. Industrial-Grade Data Pipeline
* Implements highly concurrent multiprocessing (`n_workers=64`) combined with Parquet columnar caching. 
* Compresses data parsing and feature extraction time for hundreds of thousands of logs (tens of gigabytes) from hours to under 8 minutes.

## 📊 Performance & Evaluations
All architectures were rigorously evaluated using Stratified 5-Fold Cross Validation:
* **Preliminary Round:** The baseline Dual-CatBoost model achieved **99.54% accuracy** in Intent Decision (Engage vs. Retreat) and **88.80% Macro F1** in Action Prediction. [cite_start]The overall simulation score reached **0.9732**.
* **Final Round:** The advanced DualStream V3 architecture pushed Micro-Action sequence accuracy to **91.7%** under the 150-timestep resolution. 

## 📁 Repository Structure
```text
├── data/                  # Sample Parquet logs and JSON facts
├── preprocessing/         # Multiprocessing pipeline & Feature Engineering (150T x 320D)
├── models/                
│   ├── dualstream_v3.py   # PyTorch implementation of the Logic Layer
│   └── catboost_baseline/ # Preliminary round baseline models
├── narrative_engine/      # DeepSeek V3 ABC Agents, Blackboard, & Dual-Loop Critic
├── train.py               # 5-Fold CV training script
└── evaluate.py            # End-to-end inference & generation script
```

## 🚀 Getting Started
*(Add your specific installation commands, requirements.txt instructions, and usage scripts here.)*

## 📜 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. 

## 🙏 Acknowledgments
* **Tencent Game Security:** For hosting the competition and providing the dataset.
* **DeepSeek:** For the V3 LLM powering the Narrative Layer.
