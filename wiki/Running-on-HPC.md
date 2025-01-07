1. Build the image in a sandbox from the Dockerhub
`singularity build --sandbox ./firedrake-parmmg docker://jwallwork/firedrake-parmmg`

2. Convert the sandboxed image to a Singularity container
`singularity build firedrake-parmmg.sif ./firedrake-parmmg`

*For more details, see the [Firedrake wiki page on building with Singularity](https://github.com/firedrakeproject/firedrake/wiki/singularity).*