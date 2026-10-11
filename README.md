# UniSimSO-101 — MuJoCo USD Support for Windows

Experimental Windows-specific modifications to MuJoCo for loading OpenUSD scenes in the MuJoCo viewer, developed for the SO-101 simulation project.

## Status

- The modified viewer has been tested on the author's Windows development machine.
- The viewer successfully opens the author's SO-101 laboratory scene.
- A clean build and runtime setup on another machine have **not** been verified.
- This is an experimental modification, not an official MuJoCo release.

## Source baseline

This patch is based on the official [MuJoCo repository](https://github.com/google-deepmind/mujoco).

Base commit:

`297f5fc6592e89fc41960251e88b6cf94fd7573e`

The patch changes 15 tracked source and build-configuration files. It does not include compiled binaries, build directories, debug symbols, or other untracked experimental files.

## Repository contents

- `patches/mujoco-usd-windows.patch` — source modifications as a patch against the base commit.
- `README.md` — project status and setup notes.

## Apply the patch

Use a fresh MuJoCo checkout. Do not apply the patch to an unrelated version or an already modified checkout.

```powershell
git clone https://github.com/google-deepmind/mujoco.git
Set-Location .\mujoco
git checkout 297f5fc6592e89fc41960251e88b6cf94fd7573e
git apply --check "F:\mujoco-usd-windows-setup\patches\mujoco-usd-windows.patch"
git apply "F:\mujoco-usd-windows-setup\patches\mujoco-usd-windows.patch"
```

Change the patch path if you saved it somewhere else.

These commands apply the source changes only. They do not install OpenUSD or build MuJoCo.

## Known working local launch configuration

The author's PowerShell profile defines two commands:

- `mujoco` launches the standard viewer.
- `mujoco-usd` launches the viewer built with the USD modifications.

The USD-enabled viewer is currently launched from:

`F:\mujoco-usd\mujoco\build-release-usd\bin\Release\simulate.exe`

Before launching, the profile adds the following local directories to `PATH`:

- `F:\mujoco-usd\mujoco\build\_deps\openusd-build\bin`
- `F:\mujoco-usd\mujoco\build\_deps\openusd-build\lib`
- The Visual Studio 2026 debug CRT directory used on the development machine

These paths are machine-specific examples, not portable installation paths. The required runtime DLLs and Visual C++ runtime components have not been verified on a clean Windows installation.

The tested scene is a local SO-101 laboratory USD scene. The scene, its associated assets, and the author's local environment are not included here.

## Build environment and limitations

The development machine used Windows, Visual Studio 2026, and a Miniconda environment named `robotics`. The modified MuJoCo source tree includes USD decoder, camera/light, material, Python-binding, and build-system changes.

The exact dependency revisions, build options, and steps for reproducing the working build from a clean checkout still need to be confirmed. This README therefore does not claim that the current build is independently reproducible.

The current patch also contains diagnostic logging and Windows DLL instrumentation. These changes should be reviewed before treating the patch as a production release.

## Further verification needed

1. Confirm the exact OpenUSD source revision and dependency configuration.
2. Verify a clean build from the documented base commit.
3. Verify USD viewer startup and scene loading on another Windows installation.
4. Identify the runtime DLLs and Visual C++ runtime components required.
5. Review temporary diagnostic code and decide what should remain in the published patch.

No compiled binaries, build outputs, or local scene assets are distributed by this repository.

## Reference assets

The repository includes reference files from the SO-101 laboratory simulation project.

### USD scenes and robot model

- `assets/usd/LabSceneDefV2.usdc` — laboratory scene without the SO-101 arm.
- `assets/usd/LabSceneDefV2_WithSO-101.usd` — laboratory scene with the white SO-101 arm.
- `assets/usd/WorkingSO-101_White.usd` — white-recolored SO-101 model referenced by the combined scene.

### Blender source

- `assets/blender/LabScene.blend` — Blender source scene used as a reference for the laboratory environment.

### Dependencies and limitations

The USD files were inspected in the author's development environment. Their portability and rendering on a clean installation have not been verified.

The SO-101-related USD files reference `OmniPBR.mdl`, an external material definition commonly provided by NVIDIA Omniverse/Isaac Sim. This dependency is not included in the repository. Materials may therefore appear differently, or fail to resolve, in other environments.

These files are provided as reference assets. They do not constitute a complete, independently verified simulation package, and the original Blender scene may require additional software or assets to reproduce the author's workflow.