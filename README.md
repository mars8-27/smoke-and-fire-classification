# Industrial Fire & Smoke Detection

This repository contains a video-based computer-vision prototype that combines classical fire/smoke cues with YOLOv8n scene-object detection.

The main implementation and project documentation are located in:

[`Smoke and fire detection using classical cv/`](./Smoke%20and%20fire%20detection%20using%20classical%20cv/)

## Project overview

- Fire-region cues using HSV colour segmentation, morphology, contour filtering, and brightness checks.
- Smoke cues using MOG2 background subtraction, frame differencing, and temporal smoothing.
- Optional YOLOv8n object detection for scene context.
- Heuristic risk levels and an OpenCV monitoring HUD.

## Documentation

- [Project README and setup instructions](./Smoke%20and%20fire%20detection%20using%20classical%20cv/README.md)
- [Architecture and interview revision guide](./Smoke%20and%20fire%20detection%20using%20classical%20cv/ARCHITECTURE.md)

> **Safety note:** This is an educational prototype, not a certified fire-safety or emergency-response system. No validated detection benchmark is currently published.

## Quick start

Open the project directory linked above, create a Python virtual environment, install its `requirements.txt`, and run:

```bash
python app.py
```

See the project README for platform-specific setup and command-line options.
