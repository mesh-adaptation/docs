The following installation instructions assume a Linux or Windows Subsystem for Linux (WSL) operating system.

The mesh adaptation modules are dependent on a custom setup for Firedrake with PETSc, as described in the [local installation](#local-firedrake-installation) section below. Once Firedrake is installed, see the section on [installing Animate, Goalie, or Movement](#installing-animate-goalie-or-movement).

Alternatively, you may use a [pre-built Docker image](#docker-container-approach) with Animate, Goalie, Movement, and all their dependencies preinstalled and ready to use.

## Local Firedrake installation

As described on the [Firedrake installation page](https://www.firedrakeproject.org/install.html#), a native installation of Firedrake is accomplished in 3 steps:
1. Installing systems dependencies
2. Installing PETSc
3. Installing Firedrake itself.

To use the mesh adaptation modules, we can install system dependencies (step 1) and Firedrake (step 3) exactly as described, but we must [customise the PETSc installation](https://www.firedrakeproject.org/install.html#id29) (step 2) to install additional packages: Eigen, ParMETIS, MMG, and ParMMG. We do that simply by passing the `--download-<PACKAGE>` flags when running PETSc `configure`:
```
python3 ../firedrake-configure --show-petsc-configure-options | xargs -L1 ./configure --download-eigen --download-parmetis --download-mmg --download-parmmg
```

### Reconfiguring an existing installation

If you have previously installed PETSc without the additional mesh adaptation packages listed above, you can install them using PETSc's [reconfigure](https://petsc.org/release/install/multibuild/#reconfigure) script as follows:
```
cd $PETSC_DIR
$PETSC_DIR/$PETSC_ARCH/lib/petsc/conf/reconfigure-$PETSC_ARCH.py --download-eigen --download-parmetis --download-mmg --download-parmmg
make PETSC_DIR=/opt/petsc PETSC_ARCH=arch-firedrake-default all
```

## Installing Mesh Adaptation modules

The Mesh Adaptation packages Animate, Goalie, and Movement are installed as follows, denoting the package of interest by `<PACKAGE>`.
First, ensure that you have activated the Python virtual environment associated with your Firedrake installation:
```
source /path/to/venv/bin/activate
```

Then clone the `<PACKAGE>` repository using [your preferred method](https://docs.github.com/en/get-started/git-basics/about-remote-repositories). For example, cloning with HTTPS URL is done as follows:
```
git clone https://github.com/mesh-adaptation/<PACKAGE>.git
```

Once cloned, the package can be [installed using pip](https://pip.pypa.io/en/stable/topics/local-project-installs/):
```
python3 -m pip install -e <PACKAGE>
```

## Updating

It is highly recommended to keep your Firedrake installation and Mesh Adaptation software stack up to date.
**We recommend updating the full stack at least once every two months.**

* To update Firedrake and PETSc, follow the [official Firedrake instructions](https://www.firedrakeproject.org/install.html#updating-firedrake) on how to do so.
* To update Animate/Goalie/Movement, simply change directory to where each one is located (`cd /path/to/package`) and run `git pull`.

## Docker container approach

We provide a bespoke Docker image with PETSc, Firedrake, Animate, Goalie, and Movement already preinstalled.

The image can be downloaded and run as follows:
```
docker pull ghcr.io/mesh-adaptation/firedrake-parmmg:latest
docker run -it ghcr.io/mesh-adaptation/firedrake-parmmg:latest
```

For more information on how to run docker containers, see the [official documentation](https://docs.docker.com/engine/containers/run/). For example, since all data inside a container is only accessible from inside the container, it is useful to create [filesystem mounts](https://docs.docker.com/engine/containers/run/#filesystem-mounts) to be able to access data from outside the container.

### Using GPUs inside a container

Passing the `--gpus` flag to `docker run` allows us to use GPUs inside containers (see [Docker documentation](https://docs.docker.com/reference/cli/docker/container/run/#gpus) for details). For example, to use all available GPUs, run the following:
```
docker run -it --gpus all ghcr.io/mesh-adaptation/firedrake-parmmg:latest
```

Once inside the container, we should first check that the GPUs have been detected. For NVIDIA GPUs we may do so by running `nvidia-smi` from the command line. If successful, information about your NVIDIA GPUs should be displayed, similarly to this:
```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 560.35.02              Driver Version: 560.94         CUDA Version: 12.6     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3060 Ti     On  |   00000000:01:00.0  On |                  N/A |
|  0%   39C    P8             19W /  220W |    1274MiB /   8192MiB |      3%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
```

Now you may proceed with installing GPU-supported software as normal. For example, to [install PyTorch](https://pytorch.org/get-started/locally/) with CUDA version 12.6 (as reported by `nvidia-smi` above), we may run:
```
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```
