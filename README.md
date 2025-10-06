# Snow Cover Segmentation in the Alps (Sentinel‑2 + RF baseline)

This repository contains the final project for AI4EO: a reproducible pipeline for snow / no‑snow segmentation over the Alps using Sentinel‑2 L2A imagery. We compare a classical NDSI threshold with a supervised Random Forest classifier trained on the Scene Classification Layer (SCL) as pseudo‑labels. We use geographic cross‑validation (train left half → test right half, and vice‑versa).

<p align="center">
  <img src="figures/figure_comparison.png" alt="Comparison figure" />
</p>


## Repository layout

```
ai4eo-snow-segmentation/
├── notebooks/
│   └── Final_Project.ipynb        # end-to-end workflow (load bands, labels, train, eval, plot)
├── figures/
│   ├── sentinel_rgb.png           # true color context
│   └── figure_comparison.png      # RGB, SCL, NDSI, RF side-by-side
├── outputs/
│   └── metrics_geocv.txt          # precision/recall/F1/IoU for both splits
├── requirements.txt               # Python deps
├── environment.yml                # optional Conda env 
├── .gitignore
├── LICENSE
├── AI4EO-FinalReport.pdf          # final PDF report
└── README.md
```

## Quick start

1) i. Clone the repository:
   ```bash
   git clone https://github.com/bigwahbs/ai4eo-snow-segmentation.git
   cd ai4eo-snow-segmentation
   ```

  ii. Create the environment:
   ```bash
   conda env create -f environment.yml
   conda activate ai4eo-snow
   ```
   or, using pip:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

2) **Place data**  
This repository does not include raw satellite imagery (`.SAFE` folders) or large datasets.  
   You can obtain Sentinel-2 L2A products from the [Copernicus Open Access Hub](https://scihub.copernicus.eu/) or [Google Earth Engine](https://earthengine.google.com/).  
   Place the downloaded `.SAFE` folder in a directory of your choice and update the `SAFE_DIR` path in the notebook before running.


3) **Run the notebook**  
Open `notebooks/Final_Project.ipynb` and set:
```python
SAFE_DIR = "data/your data.SAFE"
```
Run all cells. Outputs and figures will be reproduced.

## Notes

- **Labels**: we derive Snow/No‑snow from Sentinel‑2 SCL (`11 = snow`, `4/5 = no snow`), ignoring others.
- **Features**: B02, B03, B04, B8A, B11, B12 + NDSI; all resampled to a common 20 m grid.
- **Split**: geographic (left/right halves) to avoid overly optimistic random-split scores.
- **Half‑black panel**: black denotes invalid/withheld regions (e.g., the training half or non‑labelled pixels).

## Citations / Acknowledgements

- Barrou‑Dumont et al., 2021. *Copernicus High‑Res Snow & Ice* (The Cryosphere).  
- Hall et al., 1995–2002. *MODIS Snow*.  
- Breiman, 2001. *Random Forests*.  
- scikit‑learn, rasterio, numpy, matplotlib.

Prepared by **Ahmed Habib & Lucas Velciov** · 2025-10-06
