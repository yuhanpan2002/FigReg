# FigReg sample test data v1.0.0

Selected industrial scan frames from https://github.com/yuhanpan2002/FigReg.
This directory contains 12 frames (36 data files); it is a sample for testing, not the full manuscript evaluation dataset.

| Group | Included frames |
| --- | --- |
| Door | 1-W-2, 1-W-3, 1-W-4 |
| Fender | 1-W-3, 1-W-4, 1-W-5 |
| Bumper | 1-W-1, 1-W-2, 1-W-3 |
| HT | 4-W-1, 4-W-2, 4-W-3 |

## Layout and interpretation

Each frame directory contains:

- `<frame>_Image.jpg`: the co-acquired image.
- `<frame>.npz`: processed, pixel-aligned XYZ maps with keys `XTemp`, `YTemp`, `Z`, each of shape 1200 x 2048. Coordinates are in millimetres. Non-finite XYZ pixels are invalid.
- `<frame>.ply`: the corresponding supplied point cloud.

The provided NPZ files are the processed versions used by the current registration pipeline. They must remain paired with their own images; a generic PLY alone does not supply the pixel-to-XYZ correspondence.

`manifest.json` lists frame dimensions, valid-pixel counts and file hashes. It omits the author's original storage paths. No ground-truth transformations, model weights, executable or source code are included.

## Preparation

The files are available directly in the repository and as a [ZIP release](https://github.com/yuhanpan2002/FigReg/releases/tag/data-v1.0.0). You can clone the repository, use GitHub **Code > Download ZIP**, or download the data-only release ZIP. All three methods provide the same 36 data files.

Keep the `Data` directory, its filenames and frame directories unchanged. When a FigReg demo is available, place `Data` beside its executable. The first input identifies the reference and the second the source, for example:

```text
Door/1-W-2
Door/1-W-4
```

These relative paths refer to the demo's Data directory. Real absolute frame-directory paths can also be used. The demo itself is distributed separately; this data-only release does not run registration.

## Integrity

From the Data directory on Linux/WSL:

```bash
sha256sum -c SHA256SUMS
```

For questions or data problems, open an issue in the project repository and include the group and frame names.
