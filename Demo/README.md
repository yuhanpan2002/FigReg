# FigReg runnable demo

This demo provides a compiled FigReg executable, the EDM checkpoint, bundled runtime dependencies and sample input data. No separate Conda, Python, PyTorch or Open3D installation is required. The development source code, build artifacts and test records are not included.

## Download

The Google Drive upload is in progress. The download link will be added after upload verification.

The demo is distributed separately from GitHub as `FigRegDemo-linux-x64.tar.gz` (4.15 GB download; approximately 8.15 GB after extraction). Keep the entire extracted directory together, including `weights/edm_outdoor.ckpt` and the bundled libraries.

## Requirements

- Linux x86_64, or Windows with WSL2 and a Linux distribution. This executable does not run directly in Windows.
- An NVIDIA GPU with a compatible driver and CUDA support available inside Linux/WSL2. The bundled CUDA runtime does not replace the host GPU driver.
- Enough disk space for both the archive and extracted directory, plus generated results.

The demo was tested on Ubuntu 22.04 under WSL2 with an RTX 4060 (8 GB). Compatibility with other hardware and Linux distributions has not yet been independently validated. Run `nvidia-smi` inside Linux/WSL2 to check GPU visibility.

## Extract and run

In a Linux/WSL2 terminal, from the directory containing the downloaded archive:

```bash
tar -xzf FigRegDemo-linux-x64.tar.gz
cd FigRegDemo-linux-x64
./FigRegDemo
```

Enter the reference frame directory first, press Enter, then enter the source frame directory:

```text
Door/1-W-1
Door/1-W-3
```

The source is transformed into the reference coordinate system. Each invocation processes one pair and exits; enter `q` at either input prompt to cancel.

Alternatively, provide both paths as command-line arguments:

```bash
./FigRegDemo --reference Door/1-W-1 --source Door/1-W-3
```

The first run loads the checkpoint and warms up the model before registration. Wait for this step to complete. The executable directory must be writable to save results.

## Input data

The archive includes the same 12 sample frames available in the repository's [Data directory](../Data/): Door, Fender, Bumper and HT, each with `1-W-1`, `1-W-2` and `1-W-3`.

Short paths such as `Door/1-W-1` resolve relative to the demo's `Data` directory. Absolute frame-directory paths are also supported, including paths to your own data:

```bash
./FigRegDemo --reference /path/to/reference_frame --source /path/to/source_frame
```

Each frame must follow the same format as the samples:

```text
reference_frame/
├── reference_frame_Image.jpg
├── reference_frame.npz
└── reference_frame.ply
```

Use matching basenames for the directory and its files. The NPZ must contain pixel-aligned `XTemp`, `YTemp` and `Z` arrays with the same dimensions as the image; coordinates are in millimetres. Non-finite XYZ pixels are invalid. The supplied samples have image and XYZ-map dimensions of 1200 × 2048. A PLY file alone is insufficient. See [Data/README.md](../Data/README.md) for details.

## Results and timing

The terminal prints the selected branch, the 4 × 4 source-to-reference transformation matrix, `RMSE@2mm` and `Time`.

- Transformation translations are in millimetres.
- `RMSE@2mm` is a symmetric inlier nearest-neighbour RMSE after 0.5 mm voxel downsampling, using a 2 mm distance threshold. It measures alignment without ground-truth transformations.
- `Time` covers PCA, image matching and pose estimation, including the final GPU synchronization. It excludes data loading, model initialization and warmup, RMSE evaluation, printing and file saving. End-to-end wall-clock time is therefore longer.

Two result files are saved under `outputs/<timestamp>/<pair>/`:

| File | Contents |
| --- | --- |
| `transform.txt` | 4 × 4 source-to-reference transformation matrix |
| `registered.ply` | Merged registered point cloud: reference in blue, transformed source in orange |

No visualization window is opened. You can inspect the PLY with a point-cloud viewer.

This demo performs pose estimation without a subsequent ICP/G-ICP refinement step. Its timing and RMSE should be interpreted according to the definitions above, separately from the full manuscript evaluation.

## Troubleshooting

- **Permission denied:** run `chmod +x FigRegDemo` and retry.
- **CUDA unavailable:** check `nvidia-smi` inside Linux/WSL2 and the installed NVIDIA driver. The demo requires an NVIDIA GPU.
- **Checkpoint missing:** verify that `weights/edm_outdoor.ckpt` remains beside the executable in the extracted directory structure.
- **Input rejected:** check the frame-directory name, matching file basenames, NPZ keys and image/XYZ alignment. Quote paths containing spaces when using command-line arguments.

Third-party license notices are included in `THIRD_PARTY_NOTICES/`.
