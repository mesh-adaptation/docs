The following installation instructions assume a Linux or Windows Subsystem for Linux (WSL) operating system.
The mesh adaptation modules are dependent on a custom setup for Firedrake with PETSc from either a [Makefile](#default-approach) or a [Docker image](#installing-firedrake-via-docker-image).
Once Firedrake is installed (or if you already have a working installation), see the section on [installing Animate, Goalie, or Movement](#installing-animate-goalie-or-movement).

## Installing Firedrake with custom PETSc

Firedrake and PETSc are required by Animate, Goalie, and Movement. Choose from the following installation approaches for these packages.

### Default approach

The first approach uses the default MPI packages found in the path.
Run the following code to download and install Firedrake and PETSc using the associated Makefile and bespoke PETSc options:
```
curl -O https://raw.githubusercontent.com/mesh-adaptation/mesh-adaptation-docs/main/install/Makefile
curl -O https://raw.githubusercontent.com/mesh-adaptation/mesh-adaptation-docs/main/install/petsc_configure_options.txt
make install
```

The `make install` command accepts several optional arguments:
* `FIREDRAKE_ENV` for setting the Firedrake virtual environment name. Defaults to `firedrake-mmmyy`, where `mmm` is the first three letters of the current month and `yy` are the last two digits of the current year.
* `BUILD_DIR` for setting the build directory. Defaults to the current directory.
* `PETSC_CONFIGURE_FILE` for setting the PETSc configure options. Defaults to `petsc_configure_options.txt`.
These options may be combined, e.g.:
```
make install FIREDRAKE_ENV=firedrake-dev BUILD_DIR=/scratch PETSC_CONFIGURE_FILE=my_configure_file.txt
```

### Custom MPI approach

The second approach allows customisation of the MPI packages by passing arguments to the associated Makefile. First, download the Makefile and bespoke PETSc options using `curl`:
```
curl -O https://raw.githubusercontent.com/mesh-adaptation/mesh-adaptation-docs/main/install/Makefile
curl -O https://raw.githubusercontent.com/mesh-adaptation/mesh-adaptation-docs/main/install/petsc_configure_options.txt
```
When running `make install`, pass arguments for `MPICC`, `MPICXX`, `MPIF90`, and `MPIEXEC`:
```
make install MPICC=/path/to/mpicc MPICXX=/path/to/mpicxx MPIF90=/path/to/mpif90 MPIEXEC=/path/to/mpiexec
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
source /path/to/firedrake/bin/activate
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
The quickest way to do this is to use the `firedrake-update` command from within an active virtual environment.
This approach works well when there have been small changes to Firedrake and/or its associated dependencies and add-ons, but sometimes fails when there have been major changes.
See https://www.firedrakeproject.org/download.html for details on updating Firedrake.

If the `firedrake-update` approach fails then we recommend creating a whole new Firedrake installation using one of the approaches described above.
Note that the `--venv-name` command line option for the `firedrake-install` script can be useful for differentiating between different builds.