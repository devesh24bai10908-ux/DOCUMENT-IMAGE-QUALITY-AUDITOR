# Document Image Quality Auditor

**A small, explainable computer-vision project using OpenCV and NumPy.**

## What it does

The program screens a scanned or photographed document image for five common quality risks:

- **Blur** — uses variance of the Laplacian as a sharpness proxy.
- **Skew** — estimates dominant near-horizontal line angles with a Hough transform.
- **Uneven illumination** — measures the spread between high and low percentiles of a downsampled local-intensity map.
- **Glare** — estimates the fraction of near-white, low-saturation pixels.
- **Low text/ink coverage** — estimates connected-component coverage after adaptive thresholding.

It writes a JSON metrics file and a diagnostic image. The tool is deliberately transparent: each flag is tied to a measurable feature and a visible threshold.

## Important limitation

This is an educational screening prototype, **not** an OCR system, document-authentication tool, or universal pass/fail standard. Thresholds are initial heuristics and can behave differently for receipts, forms, handwriting, photographs, tables, and dark-mode documents. A `PASS_HEURISTIC` result does not guarantee readability. Always inspect the original and validate thresholds on representative samples.

## Project structure

```text
document_image_quality_auditor/
├── document_auditor.py
├── document_image_quality_auditor.ipynb
├── README.md
├── requirements.txt
└── report.pdf
```

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

pip install -r requirements.txt
```

## Run

```bash
python document_auditor.py path/to/document.jpg --out results
```

Expected outputs:

- `results/document_audit.json` — metrics, flags, and interpretation note
- `results/document_diagnostic.jpg` — a visual summary overlay

Try several images with different conditions (sharp, blurred, tilted, unevenly lit, or glare-affected). Do not report a model accuracy unless you build and document a labeled evaluation set.

## Notebook

Open `document_image_quality_auditor.ipynb` in Jupyter or Google Colab. It explains the pipeline and lets you run the audit interactively. Upload your own sample image when prompted.

## Reproducible evaluation suggestion

Create a small test set of at least 30–50 images from permitted sources, with consent where applicable. Record a human judgment for each issue (blur, skew, glare, illumination, low text coverage). Tune thresholds on a calibration split and report precision/recall on a separate holdout split. Keep near-duplicate images in the same split to avoid leakage.

## Originality and attribution

This repository is newly authored for this project and uses standard, documented OpenCV operations. Computer-vision primitives and libraries are not exclusive to one author; cite OpenCV, NumPy, and any datasets or images you use. No similarity checker can be guaranteed to return a particular score. Follow your institution's rules, disclose assistance if required, and make sure you can explain and defend every design choice.

## Dependencies

See `requirements.txt`.
