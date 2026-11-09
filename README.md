# FigReg

**Fast Image-Guided Point Cloud Registration for Weak-Feature Industrial Components**

Yuhan Pan, Liping Yang, Fuqiang Gou, Lina Fu, Siyu Chen, Hui Zhang, Yongqiang Li, Hong Huang

FigReg registers partially overlapping scans of industrial components with broad, smooth surfaces and repeated structures. It uses co-acquired images to establish reliable correspondences, followed by geometric refinement of the point clouds.

## Method

1. **Match in 2D:** Normalize image orientations using principal-axis pre-alignment, then generate correspondences with a pretrained detector-free matcher. Composite MAD filtering rejects inconsistent displacement directions and lengths.
2. **Estimate the 3D pose:** Lift the retained image matches through pixel-aligned XYZ maps and estimate a coarse rigid transformation with RANSAC.
3. **Refine efficiently:** Apply density-adaptive pyramid G-ICP from coarse to fine resolution to obtain the final alignment.

![FigReg framework: image matching and composite MAD filtering, 2D-to-3D lifting, coarse registration, and pyramid refinement.](assets/framework.jpg)

*Figure 1 from the manuscript. Click the image to view the full-resolution framework.*

## Results at a glance

![Overall comparison of mean processing time and successful registrations across 124 industrial scan pairs.](assets/overall_comparison.png)

*Figure 7 from the manuscript. FigReg achieves the highest success rate and lowest mean processing time among the eight evaluated methods.*

- **Reliable and fast:** 124/124 successful registrations across four industrial components, with a task-weighted mean runtime of **0.52 s per pair**.
- **Submillimeter alignment:** Mean translation error of **0.26–0.30 mm** at approximately 50% acquisition overlap; mean alignment RMSE of **0.38–0.44 mm** under approximately 45° and 90° in-plane rotations.
- **Robust to noise:** **100% success on 52 pairs** with Gaussian noise added to both images and 3D coordinates.
- **Efficient refinement:** **42–49% lower total runtime** than single-level G-ICP on the Door and Near rotational subsets, with similar accuracy.

Results are reported in the manuscript using an Intel Core i7-10750H CPU and an NVIDIA RTX 2060 GPU (6 GB). Runtime is measured from memory-resident inputs to the final transformation.

## Resources

A sample test dataset is now available: **12 industrial scan frames across Door, Fender, Bumper and HT**. The full manuscript evaluation dataset and runnable demo are not included in this data-only release.

- [Browse Data/ in the repository](Data/) — the complete sample dataset, organized by component and frame.
- [Download test data v1.1.0](https://github.com/yuhanpan2002/FigReg/releases/download/data-v1.1.0/FigReg-test-data-v1.1.0.zip?download=1) — ZIP, 159.8 MB.
- [Release notes](https://github.com/yuhanpan2002/FigReg/releases/tag/data-v1.1.0).

| Group | Frames |
| --- | --- |
| Door | 1-W-1, 1-W-2, 1-W-3 |
| Fender | 1-W-1, 1-W-2, 1-W-3 |
| Bumper | 1-W-1, 1-W-2, 1-W-3 |
| HT | 1-W-1, 1-W-2, 1-W-3 |


Use the repository’s [Data/ directory](Data/) directly, clone the repository, or extract the data-only release ZIP. GitHub **Code > Download ZIP** also includes `Data/`. Each frame contains `<frame>_Image.jpg`, `<frame>.npz` and `<frame>.ply`. The NPZ contains the processed pixel-aligned `XTemp`, `YTemp`, `Z` maps in millimetres, with the same dimensions as its image. Keep the images and XYZ maps paired. See [Data/README.md](Data/README.md) for preparation and integrity checks.

When using the separately distributed demo, place `Data` beside the executable and enter the reference and source frame directories, for example `Door/1-W-1`, then `Door/1-W-3`. Real absolute frame-directory paths are also accepted. This release contains data only; no executable, model weights, ground-truth transformations or source code are included.

| Resource | Documentation |
| --- | --- |
| Environment setup | To be added |
| Data and preparation | [Browse Data/](Data/) or [Sample dataset v1.1.0](https://github.com/yuhanpan2002/FigReg/releases/tag/data-v1.1.0); README included in the ZIP |
| Code and demo usage | Demo release pending |
| Configuration | To be added |
