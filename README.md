# Segmentation and Classification of Blood Cells to Detect Anomalies

This project builds an end-to-end pipeline to detect blood cells, segment their boundaries, extract morphological features, and flag potential anomalies from microscopic blood images.

The workflow combines:
- **YOLO (Ultralytics)** for cell detection (RBC and WBC),
- **Segment Anything Model (SAM)** for instance-level masks,
- **Mathematical morphology** for mask refinement,
- **Feature analysis** (area, circularity, solidity, smoothness) for quality control and anomaly-oriented insights.

## Project Objectives

- Detect red blood cells (RBC) and white blood cells (WBC) from blood smear images.
- Segment each detected cell with improved contour quality.
- Refine segmentation using morphology and edge-guided strategies.
- Compute robust shape descriptors for downstream anomaly analysis.
- Visualize each processing stage for interpretability.

## Main Pipeline

1. **Data preparation**
  - Convert annotation CSV to YOLO label format.
  - Split dataset into train/val/test.
  - Generate `data.yaml` for YOLO training.

2. **Detection training (YOLO)**
  - Train detector on RBC/WBC classes.
  - Save best checkpoint (`best.pt`) and training plots.

3. **Segmentation (SAM)**
  - Use YOLO bounding boxes as prompts.
  - Optionally use multi-point prompts for better masks.

4. **Morphological refinement**
  - Hole filling, closing, opening, smoothing.
  - Optional adaptive kernels based on mask area.
  - Optional edge-guided refinement (Canny-assisted).

5. **Analysis and quality checks**
  - Extract morphology features: area, perimeter, circularity, solidity, etc.
  - Separate touching cells using watershed.
  - Filter masks by quality thresholds.

6. **Batch processing and reporting**
  - Run on full test set.
  - Save visual outputs and summary statistics.

## Tech Stack

- Python
- Ultralytics YOLO
- Segment Anything (Meta)
- OpenCV
- NumPy, Pandas, SciPy
- scikit-image
- Matplotlib

## Dataset Notes

The notebook is currently configured for Kaggle paths:

- Images: `/kaggle/input/blood-cell-detection-dataset/images`
- Annotations: `/kaggle/input/blood-cell-detection-dataset/annotations.csv`

If you run locally, update these paths in the notebook accordingly.

## How to Run

### Option A: Kaggle (recommended for original notebook)

1. Upload/open `morphhh (1).ipynb` in Kaggle.
2. Add the dataset expected by the notebook.
3. Run cells in order.
4. The notebook will:
  - install required packages,
  - train YOLO,
  - download SAM checkpoint,
  - run segmentation and analysis,
  - save visual diagnostics and batch results.

### Option B: Local environment

1. Create a Python environment.
2. Install dependencies:

  ```bash
  pip install ultralytics scikit-image opencv-python matplotlib scipy pandas numpy torch
  pip install git+https://github.com/facebookresearch/segment-anything.git
  ```

3. Download SAM weights (`sam_vit_h_4b8939.pth`).
4. Update dataset paths in the notebook.
5. Run notebook cells sequentially.

## Key Outputs

- YOLO training artifacts (metrics, confusion matrix, `best.pt`)
- Per-image segmentation visualizations (before/after morphology)
- Watershed separation demos for touching cells
- Enhanced batch outputs in:
  - `all_segmented_results_enhanced/`
- Aggregate statistics (counts, area, circularity, solidity, smoothness)

## Repository Structure

- `morphhh (1).ipynb`: Full implementation notebook
- `README.md`: Project documentation

## Current Scope

- Implemented classes: **RBC** and **WBC**
- Focus: segmentation quality + morphology-based analysis for anomaly detection support

## Future Improvements

- Add explicit anomaly classifier from extracted features.
- Include platelet class if labeled data is available.
- Export structured reports (CSV/JSON) for clinical review.
- Package pipeline into modular Python scripts.
