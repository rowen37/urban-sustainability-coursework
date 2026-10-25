# Lab 8 — E-scooter Object Detection

This project follows the course YOLO26 nano workflow to train a one-class object detector for urban `e-scooter` images. The submitted notebook is organized to match the instructor's Colab example and can be run from top to bottom on a T4 GPU.

## Class definition

An `e-scooter` is a stand-riding electric kick scooter with small wheels, a standing deck, a handlebar, and no seat. Bicycles, motorcycles, seated mopeds, strollers, and wheelchairs are excluded.

## Data

- 70 images from the Kaggle *Improper Scooter Parking Detection* dataset (MIT license).
- 30 self-captured photographs from Philadelphia.
- 100 images and 171 labeled instances in total.
- Fixed grouped 70/15/15 split: 70 training, 15 validation, and 15 test images.
- Same-scene self-captured images and identified visual duplicates remain in one split.

Per-image source and permission information is recorded in `sources.csv` inside the data archive. The exact grouped split is recorded in `split_assignment.json` in the same archive.

## Final submission files

- `lab8_escooter_colab.ipynb` — complete notebook with the report, training, evaluation, and qualitative examples.
- `escooter_lab8_data.zip` — 100 images, normalized LabelMe annotations, the source table, and the fixed split manifest. This archive is supplied through Google Drive and is intentionally excluded from the public Git repository because it includes self-captured photographs.

Open the notebook in [Google Colab](https://colab.research.google.com/drive/1I1OzWUK4ZQtcueVvw4foLkSBMUFQVDDa).

## Training and evaluation

- Hardware: Google Colab T4 GPU.
- Image size: 640 pixels.
- Maximum epochs: 20, with early stopping at epoch 12.
- Best checkpoint: epoch 2, selected using validation performance only.
- Batch size: 2.
- Random seed: 42.
- Test data were evaluated once after model selection.
- No confidence-threshold search or test-set tuning was performed. Precision and recall use the standard Ultralytics test summary.

## Final test results

| Metric | Result |
|---|---:|
| Precision | 0.724 |
| Recall | 0.484 |
| AP50 (TorchMetrics) | 0.630 |
| AP75 (TorchMetrics) | 0.319 |
| AP50–95 (TorchMetrics) | 0.310 |

AP50 counts a detection as correct at IoU ≥ 0.50. AP75 uses IoU ≥ 0.75, and AP50–95 averages AP over IoU thresholds from 0.50 to 0.95. The notebook also includes successful detections, missed detections, and false detections with a short error analysis.

## Running the notebook

Upload `escooter_lab8_data.zip` to the root of Google Drive, open the notebook in Colab, select a T4 GPU, and run all cells. The notebook saves its final result bundle to `MyDrive/escooter-lab8-results`.

## Limitations

The dataset is small and every image contains at least one scooter. Recall is therefore limited for small, distant, or partially occluded scooters, while visually similar bicycles can produce false detections. Results should be treated as a proof of concept. Future work should add more independent streets, weather and lighting conditions, riding scenes, and verified background-only or bicycle/motorcycle hard-negative images.
