The following installation instructions assume a Linux or Windows Subsystem for Linux (WSL) operating system.
The mesh adaptation modules are dependent on a custom setup for Firedrake with PETSc from either a [local installation](#local-firedrake-installation) or a [pre-built Docker image](#installing-firedrake-via-docker-image).
Once Firedrake is installed (or if you already have a working installation), see the section on [installing Animate, Goalie, or Movement](#installing-animate-goalie-or-movement).

## Local Firedrake installation

As described on the [Firedrake installation page](https://www.firedrakeproject.org/install.html#), a native installation of Firedrake is accomplished in 3 steps:
1. Installing systems dependencies
2. Installing PETSc
3. Installing Firedrake itself.

To use the mesh adaptation modules, we can install system dependencies (step 1) and Firedrake (step 3) exactly as described, but we must [customise the PETSc installation](https://www.firedrakeproject.org/install.html#id29) (step 2) to install additional packages: chaco, eigen, parmetis, mmg, and parmmg. We do that simply by passing the `--download-<PACKAGE>` flags when running PETSc `configure`:
```
python3 ../firedrake-configure --show-petsc-configure-options | xargs -L1 ./configure --download-chaco --download-eigen --download-parmetis --download-mmg --download-parmmg
```

## Docker container approach

A bespoke Firedrake Docker container exists and can be downloaded and installed as an alternative to the above:
```
docker pull ghcr.io/mesh-adaptation/firedrake-parmmg:latest
docker run --rm -it -v ${HOME}:${HOME} ghcr.io/mesh-adaptation/firedrake-parmmg:latest
```

NOTE: by installing via a Docker image with ``${HOME}`` you are giving Docker access to your home space.

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