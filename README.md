<div align="center">

# 🫥 Invisible V2

### Real-time gesture-controlled disintegration effect using Python, OpenCV & MediaPipe

**Make a fist → you dissolve into particles. Open your hand → you reassemble.**

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-5C3EE8?logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Hand%20%26%20Segmentation-00A98F)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20macOS-lightgrey)
![License](https://img.shields.io/badge/License-MIT-green)
![Made by](https://img.shields.io/badge/Made%20by-TechTadka360-orange)

[▶ Watch the Demo](#-demo) · [⚙ Installation](#-installation) · [🎮 Controls](#-controls) · [🛠 Troubleshooting](#-troubleshooting)

</div>

---

## 📺 Demo

<!-- Replace the paths/links below with your real files after uploading -->

<div align="center">

![Invisible V2 Demo](assets/demo.gif)

*Fist gesture → dissolve · Open hand → reassemble*

</div>

### 🎬 Watch the full video

| Platform | Link |
|---|---|
| ▶ YouTube | [Watch on YouTube](UCNxiYT4F9ASH2bbbhL-zpKQ) |
| 📸 Instagram | [Watch on Instagram]([https://instagram.com/YOUR_HANDLE](https://www.instagram.com/techtadka360official?rpxt=cWR4bnhjdWw1MHdh)) |
| 📘 Facebook | [Watch on Facebook](https://www.facebook.com/share/1JTPQ3GWBm/) |

---

## ✨ Features

- 🖐 **Gesture control**: a fist triggers the dissolve, an open hand brings you back, with no keyboard needed during the effect
- 🧍 **Real-time person segmentation** with MediaPipe Selfie Segmentation
- 💥 **Particle disintegration**: glowing edge and flying particles as the body dissolves
- 🔁 **Smooth state machine**: `NORMAL → DISINTEGRATING → INVISIBLE → REASSEMBLING`
- 🧼 **Clean masks**: temporal smoothing and hole filling to avoid flicker and black spots
- 📱 **Phone as webcam**: works with DroidCam, IP Webcam, Iriun, or any OpenCV-readable source
- 🧪 **Built-in debug view**: live FPS, hand state, and mask preview
- ⚡ **CPU only**: no GPU required

---

## 🧠 How It Works

```
Camera frame
     │
     ├──► MediaPipe Hands ──► Fist / Open-hand detection ──► State machine
     │
     ├──► MediaPipe Selfie Segmentation ──► Person mask (smoothed + hole-filled)
     │
     └──► Saved clean background
                    │
                    ▼
   Dissolve map (noise + top-to-bottom sweep) decides which pixels
   of the person are replaced by the background, while edge glow
   and particles are drawn on top.
```

1. **Background capture**: press `SPACE`, step out of the frame, and the script saves a clean background (averaged over several frames).
2. **Segmentation**: MediaPipe separates you from the background.
3. **Gesture detection**: finger-tip vs. finger-joint distances from the wrist classify the hand as a fist or open.
4. **Dissolve**: pixels of your body are progressively replaced by the saved background, with glowing edges and particles.
5. **Reassemble**: an open hand reverses the process.

---

## 🧰 Tech Stack

| Component | Purpose |
|---|---|
| **Python 3.9+** | Core language |
| **OpenCV** | Camera capture, image processing, rendering |
| **MediaPipe** | Hand landmarks and selfie segmentation |
| **NumPy** | Fast array and mask operations |

---

## 📦 Installation

```bash
# 1. Clone the repository
git clone https://github.com/Mr-soumik/Invisible-V2.git
cd Invisible-V2

# 2. (Recommended) create a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Linux / macOS

# 3. Install dependencies
pip install -r requirements.txt
```

> If you see an `mp.solutions` error, install a compatible version:
> `pip install mediapipe==0.10.14`

---

## 🚀 Usage

```bash
python invisible_v2.py
```

1. Place your camera on a **tripod or stable surface** (it must not move).
2. Press **`SPACE`** and **step completely out of the frame** for the 3-second countdown.
3. Step back in.
4. Show a **fist** ✊ to dissolve.
5. Show an **open hand** 🖐 to reassemble.

---

## 🎮 Controls

| Key / Gesture | Action |
|---|---|
| ✊ **Fist** | Start dissolving |
| 🖐 **Open hand** | Reassemble |
| `SPACE` | Capture a new background |
| `D` | Toggle mask debug window |
| `R` | Reset state |
| `Q` | Quit |

---

## ⚙️ Configuration

Edit the config block at the top of `invisible_v2.py`:

| Setting | Default | Description |
|---|---|---|
| `CAMERA_SOURCE` | `1` | Camera index (`0`, `1`, `2`…) or a stream URL |
| `CAMERA_BACKEND` | `"MSMF"` | `"MSMF"`, `"DSHOW"` or `"ANY"` (Windows) |
| `USE_MJPG` | `False` | Set `True` if the picture is green or corrupted |
| `FRAME_W, FRAME_H` | `640, 480` | Lower for higher FPS |
| `FLIP` | `True` | Mirror view |
| `DISSOLVE_SECONDS` | `2.5` | Duration of dissolve / reassemble |
| `GESTURE_FRAMES` | `3` | Frames a gesture must be held (raise to avoid false triggers) |
| `MAX_PARTICLES` | `500` | Particle cap |

---

## 📱 Using Your Phone as a Webcam

| Method | `CAMERA_SOURCE` |
|---|---|
| Laptop webcam | `0` |
| DroidCam / Iriun (virtual camera) | `1` or `2` (find it with `camera_test.py`) |
| DroidCam over Wi-Fi | `"http://PHONE_IP:4747/video"` |
| IP Webcam app | `"http://PHONE_IP:8080/video"` |
| DroidCam over USB (adb) | run `adb forward tcp:4747 tcp:4747`, then `"http://127.0.0.1:4747/video"` |

**Find the right camera index:**

```bash
python camera_test.py
```

It scans camera indexes and tells you which one gives a real picture.

---

## 🛠 Troubleshooting

| Problem | Fix |
|---|---|
| **Green screen** | The virtual camera isn't receiving the phone's video. Start the DroidCam Client, or try another `CAMERA_SOURCE`, `CAMERA_BACKEND = "DSHOW"`, or `USE_MJPG = True` |
| **Low FPS / lag** | Lower `FRAME_W, FRAME_H` to `480, 360` |
| **Gestures trigger by mistake** | Raise `GESTURE_FRAMES` to `5` |
| **Ghosting / patches** | The camera moved or lighting changed. Press `SPACE` and recapture the background |
| **Rough edges** | Improve lighting and use a plain background. Press `D` to inspect the mask |
| **Black spots** | Make sure you fully left the frame while capturing the background |

---

## 📁 Project Structure

```
Invisible-V2/
├── invisible_v2.py     # Main application
├── camera_test.py      # Camera index / backend finder
├── requirements.txt    # Dependencies
├── assets/
│   └── demo.gif        # Demo preview
└── README.md
```

---

## 🗺 Roadmap

- [ ] Built-in video recording (`R` to record)
- [ ] More effects (fire, glitch, smoke)
- [ ] Selectable particle colors
- [ ] GUI settings panel
- [ ] GPU-accelerated segmentation

---

## 🤝 Contributing

Contributions, issues and feature requests are welcome.

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push: `git push origin feature/my-feature`
5. Open a Pull Request

---

## 📄 License

Distributed under the **MIT License**. See `LICENSE` for details.

---

## 👨‍💻 Author

**Soumik** is a maker and content creator behind **TechTadka360**, building ESP32, Arduino and computer-vision DIY projects.

<div align="center">

| Platform | Link |
|---|---|
| 🐙 GitHub | [@Mr-soumik](https://github.com/Mr-soumik) |
| ▶ YouTube | [TechTadka360](https://youtube.com/@YOUR_CHANNEL) |
| 📸 Instagram | [@YOUR_HANDLE](https://instagram.com/YOUR_HANDLE) |
| 📘 Facebook | [TechTadka360](https://facebook.com/YOUR_PAGE) |

</div>

---

<div align="center">

⭐ **If this project helped you, please star the repo and subscribe for more projects!** ⭐

*Made with ❤️ by TechTadka360*

</div>
