# Industrial Fire & Smoke Detection

A video-based computer-vision prototype that combines classical image-processing techniques with YOLOv8n scene-object detection. The application analyses frames, combines detection signals, assigns a heuristic risk level, and renders a monitoring HUD.

> **Scope:** This is an educational prototype, not a certified fire-alarm or life-safety system. The repository does not currently publish a validated benchmark, precision/recall, or mAP result. Do not rely on it for real-world emergency decisions.

## Detection pipeline

```text
Video / Webcam / Stream
          |
     Frame capture
          |
     Preprocessing
       /   |    \
      /    |     \
   Fire  Smoke  Scene context
   HSV   MOG2   YOLOv8n
   masks frame  object
   +     diff   detection
   morphology   (optional)
      \    |     /
       \   |    /
      Signal fusion
            |
      Risk classification
            |
      HUD + live / saved video
```

### Detection stages

- **Fire cues:** HSV colour masks, morphological cleanup, contour-area filtering, and brightness checks.
- **Smoke cues:** MOG2 background subtraction, frame differencing, low-saturation/brightness filtering, and temporal smoothing.
- **Scene context:** YOLOv8n detects selected COCO classes such as people and vehicles. This is contextual object detection, not a custom fire/smoke YOLO model.
- **Risk output:** heuristic thresholds combine detection confidence into `CLEAR`, `CAUTION`, `WARNING`, or `CRITICAL`.

See [ARCHITECTURE.md](./ARCHITECTURE.md) for the detailed component diagram, file-responsibility map, and interview revision notes.

## Technology stack

| Area | Technology |
|---|---|
| Language | Python |
| Image and video processing | OpenCV, NumPy |
| Learned object detection | Ultralytics YOLOv8n |
| Inference framework | PyTorch |
| Video input/output | OpenCV VideoCapture / VideoWriter |

## Project structure

```text
Smoke and fire detection using classical cv/
├── app.py              # Detection pipeline, risk logic, HUD, and CLI
├── yolov8n.pt          # Pretrained YOLOv8n weights
├── requirements.txt    # Explicit Python dependencies
├── README.md           # Project guide
└── ARCHITECTURE.md     # Architecture and interview notes
```

## Requirements

- Python 3.10 or newer
- pip
- A webcam, video file, or supported stream URL
- Optional: NVIDIA GPU with a compatible PyTorch/CUDA installation

## Setup

Run these commands from this project directory.

### Windows PowerShell

```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Dependencies are installed explicitly from `requirements.txt`. The application does not install packages at runtime. For GPU acceleration, install the PyTorch build that matches your CUDA environment by following the [official PyTorch installation selector](https://pytorch.org/get-started/locally/).

## Run

Start with the default video source:

```bash
python app.py
```

Process a video file:

```bash
python app.py path/to/video.mp4
```

Disable YOLO scene-object detection:

```bash
python app.py --no-yolo
```

The application also accepts a webcam index or stream URL. Use `python app.py --help` to inspect the available command-line options.

### Keyboard controls

| Key | Action |
|---|---|
| `Q` | Quit |
| `P` | Pause / resume |
| `S` | Save a screenshot |

## Evaluation and limitations

- Fire detection relies on colour, brightness, morphology, and contour heuristics. Similar-looking lights, reflections, and warm-coloured objects may trigger false positives.
- Smoke detection uses motion and appearance cues that can be affected by fog, dust, steam, camera movement, and lighting changes.
- YOLOv8n contributes general scene context; it is not trained here as a dedicated fire/smoke detector.
- Risk levels are heuristic outputs, not calibrated probabilities or safety certifications.
- No validated test set or quantitative performance report is currently included.

A stronger evaluation should use a labelled dataset with separate train/validation/test sources, report precision, recall, F1 and false-alarm rates, and test across lighting, camera, weather, and scene conditions.

## Roadmap

- [ ] Build a labelled fire/smoke dataset with documented provenance.
- [ ] Add quantitative evaluation and reproducible benchmarks.
- [ ] Improve temporal tracking and false-positive handling.
- [ ] Add structured event logging and configurable alert thresholds.
- [ ] Add automated tests for detection and risk-classification components.
- [ ] Package the application for reproducible deployment.

## License

No license file is currently provided. Until a license is added, do not assume the code or model weights are available for unrestricted reuse.
