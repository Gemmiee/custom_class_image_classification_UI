# Custom Image Classifier UI

A desktop app for training your own image classifier — no coding required after setup. Define classes, feed it example images, hit train, and start classifying new images through a clean dark-themed UI.

Built with a frozen **ResNet-18** backbone as a feature extractor and a small trainable linear head on top, so training a new class takes seconds rather than hours.

## Features

- **Define your own classes** — add or remove categories on the fly, each backed by its own folder of training images
- **Train in the app** — click Train and watch live epoch/loss progress, no scripts to run
- **Classify a single image** or an **entire folder** at once, with a live preview, confidence score, and ETA
- **Persistent models** — trained weights and class metadata are saved to disk, so you can close the app and pick up where you left off
- **Activity log** — every action (classes added, images classified, training runs) is logged in-app

## How it works

The app uses a ResNet-18 pretrained on ImageNet as a fixed feature extractor — its weights are frozen and never updated. On top of it sits a single trainable linear layer (the "head") with one output per class you've defined. Training only updates this head, using the frozen backbone's features as input, which is why training stays fast even on CPU.

```
Image → ResNet-18 (frozen) → feature vector → Linear head (trainable) → class prediction
```

## Requirements

- Python 3.8+
- [PyTorch](https://pytorch.org/) and torchvision
- Pillow
- Tkinter (usually bundled with Python; on Linux you may need `sudo apt install python3-tk`)

Install the Python dependencies:

```bash
pip install torch torchvision pillow
```

## Getting started

1. Clone the repo:
   ```bash
   git clone https://github.com/Gemmiee/custom_class_image_classification_UI.git
   cd custom_class_image_classification_UI
   ```
2. Install dependencies (see above).
3. Run the app:
   ```bash
   python Image_Classifier_Custom_Training_FINAL.py
   ```

## Usage

1. **Add a class** — click **+ Add Class**, name it, and select a folder of example images for that class. This copies the images into `custom_classes/<class_name>/`.
2. **Repeat** for at least two classes — the model needs a minimum of two classes to train.
3. **Train** — click **Train** and watch the progress bar and activity log. Training runs for 30 epochs over your example images.
4. **Classify a single image** — browse to an image and click **Classify** to see the predicted class and confidence.
5. **Classify a folder** — point it at a folder of images (defaults to the `images/` folder) and click **Classify Folder** to run predictions on every image in it, with a live progress bar and ETA.

Your trained model and class list persist automatically — next time you launch the app, everything is reloaded from disk.

## Project structure

```
.
├── Image_Classifier_Custom_Training_FINAL.py   # main app (model + GUI)
├── images/                                     # default folder for batch classification
├── custom_classes/                             # created at runtime — training images per class
├── custom_classes.json                         # created at runtime — class name → index mapping
└── custom_head.pth                             # created at runtime — trained classifier head weights
```

## Notes

- The ResNet-18 backbone stays frozen at all times — only the final linear layer is trained, which keeps things lightweight and fast.
- Classifying is disabled until at least one training run has completed.
- Supported image formats: `.jpg`, `.jpeg`, `.png`, `.bmp`, `.gif`, `.webp`.
