
# (Unofficial) Idris Dockerfiles

## Project Progress
- [x] Ubuntu Dockerfile
  - Backend support
  - [x] Chez
  - [x] Racket
  - [x] RefC
  - [x] Node
  - [x] Javascript
  - [ ] Other backend support
- [ ] Other Distro Dockerfiles
- [x] Nightly Builds (DockerHub)

## Usage

### Running Locally
To run one of the Ubuntu Idris docker images, pick a supported version tag (**0.2.2**, **0.3.0**, **0.4.0**, **0.5.1**, **0.6.0**, **0.7.0**, or **nightly**) to specify after the colon and run the following command from an environment with a Docker daemon installed and running:

```shell
docker run -ti mattpolzin2/idris-docker:latest
```

You will be dropped into an Idris 2 REPL where you can play around.

Alternatively, run the image with `bash` and write an honest-to-goodness Idris source file:

```shell
docker run -ti mattpolzin2/idris-docker:latest bash
$ echo -e "main : IO ()\nmain = putStrLn \"Hello World\"" > Main.idr
$ idris2 -o hello Main.idr
$ ./build/exec/hello
Hello World
$ [...]
```

Take things one step further by mounting a directory on your computer to the Idris docker container; just be careful, you can delete files on your host machine from inside the Docker container this way!

```shell
docker run -ti -v "$(pwd)/hello-world:/root/hello-world" mattpolzin2/idris-docker:latest bash
$ cd hello-world
$ idris2 --build hello-world.ipkg
$ [...]
```

### Choosing a backend
The Idris compiler shipped with this Docker image is built using Chez Scheme, but you can choose to compile your programs using a number of different backends.

This Docker Image supports the following backends currently:
```shell
# Chez Scheme (default)
idris2 --build my-stuff.ipkg

# Racket
idris2 --cg racket my-stuff.ipkg

# C (only available from Idris 2 v0.3.0 onward)
idris2 --cg refc --build my-stuff.ipkg

# NodeJS
idris2 --cg node --build my-stuff.ipkg

# Javascript
idris2 --cg javascript --build my-stuff.ipkg
```

### Idv
In addition to different Docker images for each version of Idris 2, there is an image tagged `idv` that has the Idris Version Manager preinstalled in addition to all Idris 2 versions. This image allows you to switch between different Idris 2 versions with a single command, but the Docker image is larger because there are multiple Idris 2 versions pre-installed.

```shell
docker run -ti mattpolzin2/idris-docker:idv bash
$ idv list
* system (installed)
  0.7.0  (installed)
  0.6.0  (installed)
  0.5.1  (installed)
  0.4.0
  0.3.0
  0.2.2
  0.2.1
  0.2.0
$ idv select 0.3.0

Idris 2 version 0.6.0 selected.

$ idris2 --version
Idris 2, version 0.6.0
$ idv select system

System copy of Idris 2 selected.

$ idris2 --version
Idris 2, version 0.7.0-nightly
```

Notice that the `system` version of Idris 2 is the version you will find in the `nightly` Docker image (i.e. it is _newer_ than the lastest stable version, which is also installed and selectable via Idv).

### Using for GitHub Actions
These Docker images can be a great foundation for GitHub Actions based testing of Idris code. Because Idris is already installed in these images, you save time over downloading or even building Idris as part of the GitHub Action itself.

Here's an example of what a GitHub Action workflow file might look like (including installing `git` into the container, because `git` is not needed by the Idris container otherwise).

```yaml
name: Tests
on: push

jobs:
  tests:
    runs-on: ubuntu-latest
    container: mattpolzin2/idris-docker:nightly

    steps:
      - name: Install Dependencies
        run: apt-get update && apt-get -y install git
      - name: Checkout
        uses: actions/checkout@v2
      - name: Build
        run: make
      - name: Test
        run: make test
```

## Building Idris Docker Images
From the root directory of this repository, pick a stage to build (**idris-022**, **idris-030**, **idris-040**, **idris-051**, **idris-060**, **idris-070**, **idris-nightly**, or **idv**), and then:

```shell
docker build -t local/idris-docker:nightly --target idris-nightly -f Dockerfile.ubuntu .
```
