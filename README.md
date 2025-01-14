# Everything.AI
 Everything.AI is a comprehensive real-time object detection and tracking system. It leverages YOLO v4 for object detection and DeepSORT for multi-object tracking, combined with a knowledge base to provide context-rich information about every recognized entity in the scene.

---

## Table of Contents
1. [Features](#features)  
2. [Project Architecture](#project-architecture)  
3. [Getting Started](#getting-started)  
4. [Installation](#installation)  
5. [Usage](#usage)  
6. [System Overview](#system-overview)  
7. [Roadmap](#roadmap)  

---

## Features
- **Real-Time Detection & Tracking**  
  Utilizes **YOLO v4** (PyTorch) and **DeepSORT** to detect and track multiple objects in a live video feed with minimal latency.

- **Contextual Information Retrieval**  
  Queries a **FastAPI** microservice with a **GraphQL** knowledge base, providing immediate, detailed insights about each detected object.

- **GPU-Accelerated Performance**  
  Leverages **CUDA** optimizations for efficient inference and fast object tracking, even with multiple camera streams.

- **Scalable Deployment**  
  Containerized with **Docker** and orchestrated using **Kubernetes**, supporting seamless horizontal scaling for multi-camera setups.

---

## Project Architecture
```
 ┌───────────────┐             ┌─────────┐
 │ Live Video    │             │ GraphQL │
 │ (OpenCV Feed) │             │Knowledge│
 └──────┬────────┘             │  Base   │
        │                      └─────┬────┘
        ▼                            │
 ┌───────────────┐             ┌────▼─────┐
 │   YOLO v4     │  Detection  │ FastAPI  │
 │ (PyTorch)     │────────────▶│ Service  │
 └──────┬────────┘             └─────┬────┘
        │ Tracking                   │
        ▼                            │
   ┌───────────────┐                 │
   │   DeepSORT     │◀───────────────┘
   └───────────────┘
```
1. **OpenCV** captures live video from one or more camera feeds.  
2. **YOLO v4** (running on PyTorch with CUDA support) performs real-time object detection.  
3. **DeepSORT** tracks objects across frames, maintaining consistent IDs.  
4. A **FastAPI** microservice connects to a **GraphQL** knowledge base for retrieving contextual data about each detected object.  
5. The entire pipeline is containerized with **Docker**, and can be orchestrated in production using **Kubernetes**.

---

## Getting Started
These instructions will help you get a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites
- **Python 3.8+**
- **Docker** (for containerization)
- **CUDA-enabled GPU** (recommended for real-time performance)
- **Kubernetes** (optional, for scaling in a production environment)

---

## Installation
1. **Clone the repository**  
   ```bash
   git clone https://github.com/your-username/everything-ai.git
   cd everything-ai
   ```

2. **Install Python dependencies**  
   If you want to run everything natively (without Docker), install the requirements:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up Docker (Optional)**  
   ```bash
   docker build -t everything-ai:latest .
   docker run -it --gpus all -p 5000:5000 everything-ai:latest
   ```
   > **Note**: Make sure you have [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) installed to enable GPU access in Docker.

---

## Usage
1. **Configure your video sources**  
   - Update the `config.yaml` or `app_settings.json` (depending on how you organize configs) with your camera feed URLs or file paths.

2. **Start the application**  
   ```bash
   python main.py
   ```
   Once the app is running, it will open or connect to your specified video sources, perform object detection and tracking, and query the knowledge base for contextual information.

3. **Access the output**  
   - By default, a streaming window or a local dashboard will display the detection results.  
   - The **FastAPI** service endpoint (e.g., `http://localhost:5000`) can be used for retrieving or updating object context data.

---

## System Overview
- **YOLO v4 (PyTorch)**  
  - High-speed object detection, optimized with CUDA for real-time inference.

- **DeepSORT**  
  - Maintains track IDs for multiple objects, even under partial occlusions or overlapping movement.

- **FastAPI**  
  - Acts as a backend microservice, handling requests to and from the **GraphQL** knowledge base.  
  - Can be extended for user-defined queries, additional context retrieval, or integration with external APIs.

- **OpenCV**  
  - Interface for capturing video feeds and performing image transformations.

- **Docker & Kubernetes**  
  - Ensures consistent deployment across environments and the ability to scale horizontally for multiple camera streams.

---

## Roadmap
- **Deploy on Edge Devices (e.g., NVIDIA Jetson)**  
  - Integrate optimized builds for resource-constrained environments.

- **Extended Knowledge Sources**  
  - Support additional knowledge bases (e.g., Wikipedia APIs, domain-specific databases) for richer context.

- **Advanced UI/UX**  
  - Web-based or mobile-friendly real-time dashboards with interactive object details.

- **Customizable Detection**  
  - Allow users to define objects of interest on the fly and retrain/fine-tune models with minimal downtime.

---

**Happy Coding!**  
If you have any questions or suggestions, please [open an issue](https://github.com/your-username/everything-ai/issues) or submit a pull request.
