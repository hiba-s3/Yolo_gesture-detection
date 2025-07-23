# 🖐️ YOLO-based Hand Gesture Detection – Sign Language Commands

## 📌 Overview

This project implements a **YOLO-based object detection model** to recognize **hand gestures** used in **sign language command interfaces**.  
Instead of recognizing letters, it focuses on detecting symbolic gestures like "fist", "open palm", "next", and "prev" to enable gesture-based control and communication support.

The goal is to provide an accessible and real-time recognition system that could be integrated into smart systems or assistive tools.

---

## 🎯 Recognized Gestures

The model detects the following six gesture classes:

| Class ID | Gesture Name     | Intended Meaning     |
|----------|------------------|----------------------|
| 0        | `open_palm`      | Hello / Attention    |
| 1        | `close_palm`     | Stop / Pause         |
| 2        | `fist`           | Confirm / Click      |
| 3        | `four_fingers`   | Signal / Ready       |
| 4        | `next`           | Move to Next Slide   |
| 5        | `prev`           | Move to Previous     |

---

## 🧠 Model & Dataset

- **Model Architecture**: YOLOv5 / YOLOv8
- **Framework**: Ultralytics YOLO (PyTorch)
- **Dataset**: Custom, collected locally using webcam
- **Classes**: 6 gesture classes (see table above)
- **Label Format**: YOLO (`.txt` files with class + bounding box)

> ⚠️ The dataset is **private** and cannot be shared due to privacy restrictions.

---

## 🧾 Label Correction Script

A helper script is used to update the class IDs in YOLO label files to match the defined gesture classes.

```python
import os
from glob import glob

class_map = {
    'open_palm': 0,
    'close_palm': 1,
    'fist': 2,
    'four_fingers': 3,
    'next': 4,
    'prev': 5
}

base_label_dir = r"PATH_TO_PRIVATE_DATASET"

for class_name, correct_index in class_map.items():
    label_folder = os.path.join(base_label_dir, class_name)
    if not os.path.exists(label_folder):
        print(f" Folder {label_folder} not found, skipping.")
        continue

    label_files = glob(os.path.join(label_folder, '*.txt'))

    for file_path in label_files:
        with open(file_path, 'r') as f:
            lines = f.readlines()

        new_lines = []
        for line in lines:
            parts = line.strip().split()
            if len(parts) == 5:
                parts[0] = str(correct_index)
                new_lines.append(' '.join(parts))

        with open(file_path, 'w') as f:
            f.write('\n'.join(new_lines))

        print(f"✅ Updated: {file_path}")
