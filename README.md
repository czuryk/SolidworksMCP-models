# SolidworksMCP optional vectorization models

Verified **DexiNed + TEED TorchScript weights** for the line-art vectorization
backend in [SolidworksMCP](https://github.com/czuryk/SolidworksMCP).

**The MCP server and ordinary CAD modeling work without these files.** Install
them only for `stroke_centerlines`, `stroke_edges` and `all_visible_edges` tracing.
These modes also require a CUDA-enabled PyTorch installation and NVIDIA GPU.

## Get the weights

This repository uses Git LFS for the binary models:

```powershell
git lfs install
git clone https://github.com/czuryk/SolidworksMCP-models.git
git -C SolidworksMCP-models lfs pull
```

The main SolidworksMCP repository can be downloaded with **Code → Download ZIP**
or cloned without Git LFS. Only this optional model download depends on LFS.

GitHub's source ZIP contains LFS pointers unless the repository's archive setting
includes LFS objects. Small text files beginning with
`version https://git-lfs.github.com/spec/v1` are not weights; the installer detects
and rejects them. A blocked LFS download does not prevent using the main server.

## Install into SolidworksMCP

From the main SolidworksMCP project folder, run:

```powershell
python scripts/install_vectorization_models.py --from-dir C:/Downloads/SolidworksMCP-models
```

The installer checks both SHA-256 values before copying anything. It accepts this
repository root or the `lineart` folder, performs no network requests, and places
the files in `solidworks_mcp/models/lineart/` by default.

Alternatively, set `SOLIDWORKS_MCP_LINEART_MODELS_DIR` in the MCP server's environment
to the absolute path of this repository's `lineart` folder. Reconnect the MCP
server after changing its launch environment. Existing installations with the
weights in the original server folder continue to work.

Call `expert_tools` with `action="call"`, `name="get_capabilities"`. The
`deep_vectorization.line_art` result reports missing/invalid files, verified
hashes and CUDA readiness. A tracing request returns `MODEL_WEIGHTS_MISSING` or
`MODEL_WEIGHTS_INVALID` with installation instructions when necessary.

| File | Size | Original model |
|---|---:|---|
| `lineart/dexined_biped_v2.pt` | 141,291,499 bytes | [DexiNed BIPED v2](https://github.com/xavysp/DexiNed) |
| `lineart/teed_biped_5.pt` | 296,344 bytes | [TEED BIPED checkpoint 5](https://github.com/xavysp/TEED) |

Exact hashes are in [manifest.json](manifest.json). These are the inference exports
used by SolidworksMCP; original upstream training checkpoints are not drop-in
replacements. Region segmentation uses separate SAM 2.1 and ViTMatte models;
see the main project's [setup guide](https://github.com/czuryk/SolidworksMCP/blob/main/docs/setup.md#optional-vectorization-models).

## License and LFS quota

The upstream projects use the MIT license. Preserve the authors' notices in
[LICENSES.md](LICENSES.md) when redistributing the weights.

Git LFS bandwidth is shared across the owner's account. Moving models here
isolates their download from the main project; it does not grant another quota.
