# VitalWatch

VitalWatch is a real-time patient monitoring prototype that uses computer vision to detect critical patient events from a webcam, RTSP stream, or local video file. It reads live frames, detects patient posture and movement, scores event severity, and displays alerts on a browser dashboard.

## My Contribution

VitalWatch was developed as a team project. I primarily contributed to the
AI/ML and computer vision module, including:

- Computer vision processing using OpenCV
- Integration of YOLOv8 for person detection
- Integration of MediaPipe for pose estimation
- Patient event detection and severity scoring
- Integration of the vision pipeline with the FastAPI backend and REST/WebSocket APIs

## System Overview

```mermaid
flowchart LR
    A[Video Source<br/>Webcam / RTSP / File] --> B[OpenCV Video Stream]
    B --> C[YOLOv8<br/>Person Detection]
    B --> D[MediaPipe<br/>Pose Estimation]
    C --> E[Event Engine]
    D --> E
    E --> F[Severity Scorer]
    F --> G[Alert Manager]
    G --> H[FastAPI Backend]
    H --> I[Dashboard<br/>Live Feed + Alerts]
```

## Workflow

```mermaid
sequenceDiagram
    participant Source as Video Source
    participant Stream as VideoStream
    participant AI as Detection + Pose
    participant Engine as Event Engine
    participant Score as Severity Scorer
    participant API as FastAPI / WebSocket
    participant UI as Dashboard

    Source->>Stream: Send video frames
    Stream->>AI: Provide current frame
    AI->>Engine: Person boxes + pose metrics
    Engine->>Score: Detected event
    Score->>API: Severity alert
    API->>UI: Live stream and real-time alert
```

## Core Modules

```mermaid
flowchart TD
    SRC[src/] --> VIDEO[video/<br/>Frame capture and buffering]
    SRC --> MODELS[models/<br/>YOLOv8 and MediaPipe wrappers]
    SRC --> EVENTS[events/<br/>Fall, bed-exit, immobility rules]
    SRC --> SEVERITY[severity/<br/>Risk scoring]
    SRC --> ALERTS[alerts/<br/>Alert logging and broadcast]
    SRC --> API[api/<br/>FastAPI routes and WebSocket]
    DASH[dashboard/] --> WEB[index.html<br/>Live monitoring UI]
```

## Features

| Area | Capability |
| --- | --- |
| Input | Webcam, RTSP stream, or local video file |
| Detection | Person detection with YOLOv8 |
| Pose | Patient posture and movement tracking with MediaPipe |
| Events | Fall, bed exit, immobility, abnormal movement |
| Severity | Normal, Warning, and Critical scoring |
| Backend | FastAPI, MJPEG stream, WebSocket alerts |
| Dashboard | Live feed, severity status, and event history |

## Tech Stack

```mermaid
mindmap
  root((VitalWatch))
    Computer Vision
      OpenCV
      YOLOv8
      MediaPipe
    Backend
      Python
      FastAPI
      Uvicorn
      WebSockets
    Frontend
      HTML
      CSS
      JavaScript
```

## Project Structure

```text
src/
  alerts/       Alert handling and WebSocket broadcast hooks
  api/          FastAPI server and dashboard routes
  events/       Rule-based patient event detection
  models/       Object detection and pose estimation
  severity/     Severity scoring logic
  video/        Video capture and frame buffering
dashboard/      Browser dashboard
requirements.txt
```

## Data Flow

```text
Camera / RTSP / Video
        |
        v
OpenCV frame reader
        |
        +--> YOLOv8 person detection
        |
        +--> MediaPipe pose estimation
                  |
                  v
          Rule-based event engine
                  |
                  v
          Severity scoring
                  |
                  v
        Console logs + WebSocket alerts
                  |
                  v
             Web dashboard
```

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

## Run

```bash
python -m src.main 0
```

Open `http://localhost:8000` to view the dashboard.

Use an RTSP stream or local video file instead of `0` when needed:

```bash
python -m src.main "rtsp://user:pass@host/path"
python -m src.main path/to/video.mp4
```

## License

This project is licensed under the terms in `LICENSE`.
