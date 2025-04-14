The following installation instructions assume a Linux or Windows Subsystem for Linux (WSL) operating system.
The mesh adaptation modules are dependent on a custom setup for Firedrake with PETSc from either a [local installation](#local-firedrake-installation) or a [pre-built Docker image](#installing-firedrake-via-docker-image).
Once Firedrake is installed (or if you already have a working installation), see the section on [installing Animate, Goalie, or Movement](#installing-animate-goalie-or-movement).

## Local Firedrake installation

As described on the [Firedrake installation page](https://www.firedrakeproject.org/install.html#), a native installation of Firedrake is accomplished in 3 steps:
1. Installing systems dependencies
2. Installing PETSc
3. Installing Firedrake itself.

To use the mesh adaptation modules, we can install system dependencies (step 1) and Firedrake (step 3) exactly as described, but we must [customise the PETSc installation](https://www.firedrakeproject.org/install.html#id29) (step 2) to install additional packages: Eigen, ParMETIS, MMG, and ParMMG. We do that simply by passing the `--download-<PACKAGE>` flags when running PETSc `configure`:
```
python3 ../firedrake-configure --show-petsc-configure-options | xargs -L1 ./configure --download-eigen --download-parmetis --download-mmg --download-parmmg
```

## Docker container approach

A bespoke Firedrake Docker image exists and can be downloaded and run as an alternative to the above:
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

Once inside the container, check that your NVIDIA GPU has been detected by running `nvidia-smi` from the command line. If successful, information about your NVIDIA GPU should be displayed, similarly to this:
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

## Installing Mesh Adaptation modules

The Mesh Adaptation packages can be installed through the following options, denoting the package of interest by `<PACKAGE>`.
In both cases, make sure that you have activated the Python virtual environment that was created when you installed Firedrake.
Ensure that you have activated the Python virtual environment associated with your Firedrake installation before following the steps below:
```
source /path/to/venv/bin/activate
```

### Cloning via HTTPS

```
git clone https://github.com/mesh-adaptation/<PACKAGE>.git
cd <PACKAGE>
make install
```

### Cloning via SSH

```
git@github.com:mesh-adaptation/<PACKAGE>.git
cd <PACKAGE>
make install
```

## Updating

It is highly recommended to keep your Firedrake installation and Mesh Adaptation software stack up to date.
**We recommend updating the full stack at least once every two months.**

* To update Firedrake and PETSc, follow the [official Firedrake instructions](https://www.firedrakeproject.org/install.html#updating-firedrake) on how to do so.
* To update Animate/Goalie/Movement, simply change directory to where each one is located (`cd /path/to/package`) and run `git pull`.
