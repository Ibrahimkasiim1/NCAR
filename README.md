# NCAR: Nighttime Collision Avoidance Response

A low-cost wearable that helps drivers see pedestrians at night. An infrared camera and an on-device ML model detect people on the road in the dark, and an OLED display projects an alert onto the windshield. **Winner of Best Travel and Transportation Hack at MakeUofT 2022.**

<!-- Add a photo of the hardware or a demo GIF here, e.g. ![demo](docs/demo.gif) -->

[Devpost](https://devpost.com/software/ncar-nighttime-collision-avoidance-system)

## Features

- Detects people on the road in near-total darkness using an infrared camera
- Runs object detection entirely on a Raspberry Pi, with no cloud connection
- Flashes an "OBJECT DETECTED" alert on an OLED display that reflects onto the windshield
- Works as a backup set of eyes if the car's headlights fail
- Portable design that can be mounted in any car

## How it works

```
IR camera ──frames──▶ Raspberry Pi 4B ──▶ EfficientDet-Lite0 (TFLite) ──"person"?──▶ SSD1306 OLED ──▶ windshield
```

`NCAR.py` captures frames with OpenCV and runs them through an EfficientDet-Lite0 TensorFlow Lite detector (optionally on a Coral Edge TPU). When a person is detected, the Pi drives a 128×64 SSD1306 OLED over I2C to show an alert, which is reflected onto the windshield in the driver's line of sight.

## Tech stack

- **ML:** TensorFlow Lite (EfficientDet-Lite0), optional Coral Edge TPU
- **Software:** Python, OpenCV, Pillow, Adafruit CircuitPython
- **Hardware:** Raspberry Pi 4B, infrared camera, 1" SSD1306 OLED display

## Results

- Won **Best Travel and Transportation Hack** at MakeUofT 2022
- Detects people at **5–7 m** from the camera in the dark

## Running it locally

Requires a Raspberry Pi with a camera and an SSD1306 OLED wired over I2C (address `0x3C`, reset on GPIO 4).

1. Clone the repo and install the dependencies and models:
   ```bash
   git clone https://github.com/Ibrahimkasiim1/NCAR.git
   cd NCAR
   sh setup.sh
   ```
2. Run the detector:
   ```bash
   python3 NCAR.py --model efficientdet_lite0.tflite
   # with a Coral Edge TPU:
   python3 NCAR.py --model efficientdet_lite0_edgetpu.tflite --enableEdgeTPU
   ```

## Team

Built at MakeUofT 2022 by:

- [Mustafa](https://github.com/Mustaballer)
- [Ibrahim](https://github.com/Ibrahimkasiim1)
<!-- Add the other teammates and who built what -->

The object detection pipeline is adapted from the [TensorFlow Lite examples](https://github.com/tensorflow/examples) (Apache 2.0).
