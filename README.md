# corky-scab – lesion quantification on potato tubers with K-Means

Classical computer-vision tool that segments the tuber from its (blue) background and quantifies **the % of surface covered by corky scab lesions**.

![Example](assets/example_output.png)

## Method
1. Tuber segmentation (HSV mask of the blue background, morphological opening, area filter).
2. Aggressive smoothing (Gaussian + bilateral + median) and CLAHE on the L channel.
3. Per-pixel features: **LAB-L, LAB-A, texture gradient, local contrast** → standardised.
4. **K-Means (2 clusters)**, lesion cluster identified automatically, then false-positive clean-up (morphology + small-component removal).
5. Output: lesion % of tuber area, 6-panel figure (PNG/PDF) and a results CSV.

Demo image: 183,915 px of tuber, **7.34 % lesion area**.

## Install & use
```bash
pip install .
```
```python
import corky_scab as cksc
perc, mask, fig = cksc.analyze_corky_scab("examples/3.0.jpg", return_fig=True)
cksc.save_figures(fig, out_dir="outputs", base_name="3_0_analysis")
df = cksc.analyze_corky_scab_to_df("examples/3.0.jpg"); cksc.save_results_df(df, "outputs/results.csv")
```

## Limitations
- Developed for photographs taken on a **blue background** with uniform lighting; the background mask will fail otherwise.
- **Not validated against manual lesion annotations or visual-rating scores** — percentages are model outputs, tuned by eye on a handful of images.
- The package prints progress with emojis and suppresses warnings globally; this is research-grade code.

> **Note:** originally developed and run in Google Colab. Google-Drive mounting cells were removed, Drive paths were rewritten as relative paths and outputs were stripped (figures of the original run are in `assets/`). The paths/cleanup were **not re-executed** after this change, so expect to adjust paths.

## Author
Tiago Alejo Herrador
