
# Delta-GameGPT 🎮

[![Rank](https://img.shields.io/badge/Rank-5th_Preliminary-blue.svg)]() [![Rank](https://img.shields.io/badge/Rank-Top_40_Finals-orange.svg)]() 

> [cite_start]**Delta-GameGPT** is the award-winning open-source solution for the 2026 Tencent Game Security Technical Competition (Delta GameGPT Track)[cite: 1, 106]. [cite_start]This project achieves high-precision micro-action prediction and hallucination-free tactical narrative generation based on complex FPS game logs[cite: 111].

## 📖 Overview

[cite_start]This repository tackles the challenge of analyzing 20 seconds of highly dynamic, multi-agent multimodal game logs (including 3D coordinates, field of view, damage events, and skill usage) to predict a player's micro-actions in the future 5 seconds [cite: 9, 111, 126-131, 190]. [cite_start]Furthermore, it generates professional, highly compliant tactical commentary texts[cite: 190, 206].

[cite_start]To overcome the "physical blindness" and "spatiotemporal hallucinations" common in End-to-End LLMs, this project introduces a **Strongly Decoupled Paradigm**[cite: 111, 204]. [cite_start]By splitting the pipeline into a rigorous **Logic Layer** and a constrained **Narrative Layer**, the system strictly binds language generation to immutable mathematical facts [cite: 204-206].

## ✨ Core Architecture & Innovations

### 🧠 1. Logic Layer (Micro-Action Prediction)
[cite_start]Dedicated strictly to high-dimensional tensor operations and time-series regression[cite: 205].
* [cite_start]**High-Density Feature Engineering:** Constructs a 320-dimensional feature space covering 150 time steps at a 0.133s resolution[cite: 112]. [cite_start]This limit captures the Nyquist frequency of human reaction times, preventing the aliasing of high-speed gunfight micro-operations[cite: 212, 213].
* [cite_start]**Feature Domains:** Extracted features include orthogonal pose encoding (smooths Yaw boundaries), kinematics derivatives (Jerk/Acceleration), Combat Geometry (Angle Consistency for backstab detection), and sliding window macro-states [cite: 221-246].
* [cite_start]**DualStream V3 Network:** A parallel sequence encoder architecture[cite: 250, 381].
  * [cite_start]*Slow-Thought Stream (Bi-LSTM):* Captures continuous physical inertia and movement trajectories[cite: 321, 322].
  * [cite_start]*Fast-Thought Stream (Transformer):* Anchors discrete, sudden burst events (e.g., sudden damage spikes) across long sequences[cite: 328, 329].
* [cite_start]**Dynamic Multi-Pooling Gate:** Extracts transient (Last), macro (Mean), and extreme (Max) events via learnable gating weights, preventing the dilution of early critical actions [cite: 338-353].
* [cite_start]**Context-Initialized Decoder:** Cures "cold start amnesia" during autoregressive decoding by pre-loading historical context and macro-intent directly into the initial LSTM hidden states[cite: 354, 355].

### 📝 2. Narrative Layer (Tactical Generation)
[cite_start]Dedicated to language rendering under strict factual boundaries, powered by DeepSeek V3[cite: 113, 206].
* [cite_start]**Multi-Agent Blackboard System:** Utilizes three collaborative agents—Tactical Analyst (A), Logic Interpreter (B), and Narrative Executive (C)—synchronized via a central data blackboard [cite: 113, 467-475].
* [cite_start]**Grounding Gate:** Forces exact substring matching to ensure LLM-extracted events strictly trace back to the raw JSON facts, killing hallucinations at the source[cite: 471].
* [cite_start]**Constraint Compiler:** Scans the predicted logic card to inject hard rules and blackout banned gaming slangs (e.g., "peek", "wallhack", "aimbot")[cite: 199, 480, 492].
* **Dual-Loop Critic & Refiner:**
  * [cite_start]*Inner Loop:* Uses Regex to check structural formatting and semantic conflicts (e.g., automatically downgrading generated text like "Double Kill" to "Fire suppression" if the logic layer predicted zero kills) [cite: 494-498].
  * [cite_start]*Outer Loop:* Triggers macro replanning or a fallback rule-based generation mechanism if max retries are exceeded, ensuring 100% compliance [cite: 500-502].

### ⚡ 3. Industrial-Grade Data Pipeline
* [cite_start]Implements highly concurrent multiprocessing (`n_workers=64`) combined with Parquet columnar caching[cite: 31, 32]. 
* [cite_start]Compresses data parsing and feature extraction time for hundreds of thousands of logs (tens of gigabytes) from hours to under 8 minutes[cite: 31, 32].

## 📊 Performance & Evaluations
[cite_start]All architectures were rigorously evaluated using Stratified 5-Fold Cross Validation[cite: 368]:
* [cite_start]**Preliminary Round:** The baseline Dual-CatBoost model achieved **99.54% accuracy** in Intent Decision (Engage vs. Retreat) and **88.80% Macro F1** in Action Prediction[cite: 83]. [cite_start]The overall simulation score reached **0.9732**[cite: 86].
* [cite_start]**Final Round:** The advanced DualStream V3 architecture pushed Micro-Action sequence accuracy to **91.7%** under the 150-timestep resolution[cite: 113, 388]. 

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
* [cite_start]**Tencent Game Security:** For hosting the competition and providing the dataset[cite: 1, 190].
* [cite_start]**DeepSeek:** For the V3 LLM powering the Narrative Layer[cite: 113, 468].
