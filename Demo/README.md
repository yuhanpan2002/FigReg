# FigReg runnable demo

This demo provides a compiled FigReg executable, the EDM checkpoint, bundled runtime dependencies and sample input data. No separate Conda, Python, PyTorch or Open3D installation is required.

## Download

Download `FigRegDemo-linux-x64.tar.gz` from either location:

| Download | Access code |
| --- | --- |
| [Google Drive](https://drive.google.com/file/d/1LGltFonAwf4sPGXTRau4SLdcm_7X7zih/view) | Not required |
| [Baidu Netdisk (China mirror)](https://pan.baidu.com/s/1Xq4lspleHlzP_q1b2eChLA?pwd=8frh) | `8frh` |

Both links provide the same demo package. On Google Drive, use the download button on the file page. If a file-size or virus-scan confirmation appears, choose **Download anyway** to proceed.

The demo is distributed separately from GitHub as `FigRegDemo-linux-x64.tar.gz` (4.15 GB download; approximately 8.15 GB after extraction).

**The archive bundles the Python runtime, PyTorch, Open3D, CUDA runtime libraries and other required software dependencies, together with the model checkpoint and sample data. These bundled dependencies account for most of the package size. No separate Conda environment or Python/PyTorch/Open3D installation is needed: extract the archive and run `./FigRegDemo` to test the supplied data on a compatible Linux/WSL2 system with an NVIDIA GPU and driver.** Conda itself is not included or required.

Keep the entire extracted directory together, including `weights/edm_outdoor.ckpt` and the bundled libraries.

## Requirements

- Linux x86_64, or Windows with WSL2 and a Linux distribution. This executable does not run directly in Windows.
- An NVIDIA GPU with a compatible driver and CUDA support available inside Linux/WSL2. The bundled CUDA runtime does not replace the host GPU driver.
- Enough disk space for both the archive and extracted directory, plus generated results.

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

Use matching basenames for the directory and its files. The NPZ must contain pixel-aligned `XTemp`, `YTemp` and `Z` arrays with the same dimensions as the image; coordinates are in millimetres. Non-finite XYZ pixels are invalid. See [Data/README.md](../Data/README.md) for details.

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

Third-party license notices are included in `THIRD_PARTY_NOTICES/`.
