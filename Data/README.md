# FigReg sample test data v1.1.0

12 industrial scan frames / 36 data files from https://github.com/yuhanpan2002/FigReg.
This is a sample for testing, not the full manuscript evaluation dataset.

Every group (Door, Fender, Bumper, HT) now uses the same frame identifiers: **1-W-1, 1-W-2, 1-W-3**.
Directories and all three filenames were renamed together. Only names changed; all 36 data-file contents and SHA-256 hashes are unchanged from v1.0.0. The identifiers are sample names, not original acquisition indices. See `FRAME_MAPPING.csv` for the original names.

## Layout

```text
Data/
├── Door/
├── Fender/
├── Bumper/
└── HT/
    ├── 1-W-1/
    │   ├── 1-W-1_Image.jpg
    │   ├── 1-W-1.npz
    │   └── 1-W-1.ply
    ├── 1-W-2/
    └── 1-W-3/
```

Each group has the same three-frame structure. NPZ files are the processed pixel-aligned XYZ maps used by the current pipeline, with keys `XTemp`, `YTemp`, `Z`, shape 1200 x 2048, and coordinates in millimetres. Non-finite XYZ pixels are invalid. Images and XYZ maps must remain paired. A generic PLY alone does not supply pixel-to-XYZ correspondences.

`manifest.json` records current/original frame identifiers, dimensions, valid-pixel counts and data-file hashes. The public manifest omits author storage paths. No ground-truth transformations, executable, model weights or source code are included.

## Download and preparation

Browse this directory, clone the repository, use GitHub **Code > Download ZIP**, or download [the data-only v1.1.0 ZIP](https://github.com/yuhanpan2002/FigReg/releases/tag/data-v1.1.0). These methods supply the same 36 data files with the current names. The v1.0.0 release remains available with the historical names.

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
