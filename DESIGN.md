# High-Level Design: Llama Box

## 1. Introduction

The Llama Box is an open-source, offline-first personal AI device designed for privacy and local inference of large language models (LLMs). It leverages a user's smartphone for its interface, allowing for a compact, powerful, and private AI experience without relying on cloud services. The project provides a complete blueprint for both hardware and software, fostering a community-driven ecosystem.

## 2. Core Principles

The design of the Llama Box is guided by the following core principles:

*   **Offline First:** All LLM processing, data storage, and inference happen directly on the device. No data is sent to the cloud.
*   **Privacy-Centric:** User data, including microphone input and conversation history, never leaves the Llama Box. The only cloud connectivity is for one-way updates of models and the operating system.
*   **Open Source:** The entire project—from the hardware schematics and bill of materials to the operating system and companion app code—will be open-source. This allows for transparency and community contributions.
*   **Dedicated Hardware:** The device is built around a dedicated GPU and a solid-state drive (SSD) to provide high-speed inference and ample storage for multiple open-source models.
*   **Smartphone as Interface:** The Llama Box connects to a smartphone via USB-C or a wireless standard like Wi-Fi Direct. The phone's screen and keyboard serve as the user interface, eliminating the need for a separate display and input device.

## 3. Hardware Architecture

The hardware design prioritizes a balance of performance, power efficiency, and affordability.

| Component | Specification | Rationale |
|---|---|---|
| SoC | A low-power CPU with an integrated GPU for system tasks, e.g., an ARM-based chip. | Handles OS and app management without consuming excessive power. |
| Dedicated GPU | NVIDIA Jetson Orin Nano, or similar. At least 16GB of VRAM is recommended. | VRAM capacity is the single most important factor for LLM performance. A minimum of 16GB is necessary to run larger quantized models like Llama 3 8B. |
| SSD | M.2 NVMe SSD, 1TB minimum. | Provides fast read/write speeds for loading large models and storing user data and conversation history. |
| Microphone | An integrated array microphone with a low-power, always-on voice activity detector. | Enables the use of an offline speech-to-text model like Whisper, making the device function as an always-on assistant without cloud connectivity. |
| Connectivity | Wi-Fi 6E, Bluetooth 5.0, USB-C 3.2. | Wi-Fi for one-way updates; Bluetooth for low-power connections; USB-C for high-speed data transfer and power. |
| Power | USB-C Power Delivery (PD) enabled. The device will be powered by a wall adapter, not the host phone. | Ensures a stable power supply for the GPU and prevents a significant drain on the phone's battery. |

## 4. Software Architecture

The software is designed to be modular and easy to contribute to. The main components are:

*   **Llama OS:** A custom, Linux-based operating system that is highly optimized for LLM inference. It will run an inference engine like Llama.cpp or Ollama and a local instance of the Whisper model for speech-to-text.
*   **Companion App (Android):** A native Android app that connects to the Llama Box. It provides the conversational interface, displays responses, and handles model management. The app will communicate with the Llama OS via a local API over USB or Wi-Fi.
*   **Comparability Test Suite:** A suite of scripts and benchmarks to ensure that the hardware and software work together seamlessly. This will include performance metrics like tokens per second and latency tests for different models.
*   **Model Management:** Users can download new open-source models directly to the Llama Box via a built-in repository browser in the companion app.

## 5. System Operation

This section describes the typical user workflows.

### 5.1. Initial Setup
1.  A user connects their smartphone to the Llama Box via USB-C.
2.  The user opens the companion app on their phone, which establishes a secure, local connection to the Llama Box.

### 5.2. Inference Flow

#### Text Input
1.  The user types a text prompt into the companion app on their phone.
2.  The prompt is sent to the Llama Box.
3.  The Llama OS performs inference using the local LLM.
4.  The response is sent back to the phone for display in the companion app.

#### Voice Input
1.  The on-device microphone, powered by the always-on Whisper model, detects speech.
2.  The audio is processed locally and converted to text.
3.  The transcribed text is used as a prompt for the LLM.
4.  The LLM's response is sent back to the phone.

### 5.3. System and Model Updates
1.  When the Llama Box is connected to Wi-Fi, it periodically checks for updates to the Llama OS and new models.
2.  The user can initiate a download of these updates via the companion app.
3.  All data stays local and is never uploaded.
