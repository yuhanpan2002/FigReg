# FigReg sample test data v1.1.0

12 industrial scan frames / 36 data files from https://github.com/yuhanpan2002/FigReg.
This is a sample for testing, not the full manuscript evaluation dataset.

| Group | Frames |
| --- | --- |
| Door | 1-W-1, 1-W-2, 1-W-3 |
| Fender | 1-W-1, 1-W-2, 1-W-3 |
| Bumper | 1-W-1, 1-W-2, 1-W-3 |
| HT | 1-W-1, 1-W-2, 1-W-3 |

## Frame structure

```text
Data/Door/1-W-1/
├── 1-W-1_Image.jpg
├── 1-W-1.npz
└── 1-W-1.ply
```

Each group has three frame directories with this structure. NPZ files contain processed pixel-aligned XYZ maps with keys `XTemp`, `YTemp`, `Z`, shape 1200 x 2048, and coordinates in millimetres. Non-finite XYZ pixels are invalid. Images and XYZ maps must remain paired. A generic PLY alone does not supply pixel-to-XYZ correspondences.

`manifest.json` records frame identifiers, dimensions, valid-pixel counts and file hashes. No ground-truth transformations, executable, model weights or source code are included.

## Download and preparation

Browse this directory, clone the repository, use GitHub **Code > Download ZIP**, or download [the data-only ZIP](https://github.com/yuhanpan2002/FigReg/releases/tag/data-v1.1.0).

Keep Data and its filenames unchanged. When a FigReg demo is available, place Data beside the executable. The first input is reference and the second source, for example:

```text
Door/1-W-1
Door/1-W-3
```

Relative paths refer to the demo's Data directory. Real absolute frame-directory paths also work. The demo is distributed separately; data alone does not run registration.

## Integrity

From the Data directory on Linux/WSL:

```bash
sha256sum -c SHA256SUMS
```
