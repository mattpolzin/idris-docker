
# (Unofficial) Idris Dockerfiles

## Project Progress
- [x] Ubuntu Dockerfile
  - Backend support
  - [x] Chez
  - [x] RefC
  - [x] Node
  - [x] Javascript
  - [ ] Other backend support
- [ ] Other Distro Dockerfiles
- [x] Nightly Builds (DockerHub)
- [ ] Documentation on building off of these images for CI

## Usage

### Running Locally
To run one of the Ubuntu Idris docker images, pick a supported version tag (**0.2.2**, **0.3.0**, **0.4.0**, or **nightly**) to specify after the colon and run the following command from an environment with a Docker daemon installed and running:

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

### Idv
In addition to different Docker images for each version of Idris 2, there is an image tagged `idv` that has the Idris Version Manager preinstalled in addition to all Idris 2 versions. This image allows you to switch between different Idris 2 versions with a single command, but the Docker image is larger because there are multiple Idris 2 versions pre-installed.

```shell
docker run -ti mattpolzin2/idris-docker:idv bash
$ idv list
* system (installed)
  0.4.0  (installed)
  0.3.0  (installed)
  0.2.2  (installed)
  0.2.1
  0.2.0
$ idv select 0.3.0

Idris 2 version 0.3.0 selected.

$ idris2 --version
Idris 2, version 0.3.0
$ idv select system

System copy of Idris 2 selected.

$ idris2 --version
Idris 2, version 0.4.0-nightly
```

Notice that the `system` version of Idris 2 is the version you will find in the `nightly` Docker image (i.e. it is _newer_ than the lastest stable version, which is also installed and selectable via Idv).

## Building Idris Docker Images
From the root directory of this repository, pick a stage to build (**idris-022**, **idris-030**, **idris-040**, or **idris-nightly**), and then:

```shell
docker build -t local/idris-docker:nightly --target idris-nightly -f Dockerfile.ubuntu .
```
