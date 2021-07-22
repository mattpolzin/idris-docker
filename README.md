
# (Unofficial) Idris Dockerfiles

## Project Progress
- [x] Ubuntu Dockerfile
  - [x] Chez backend support
  - [ ] Other backend support
- [ ] Other Distro Dockerfiles
- [ ] Nightly Builds (DockerHub)
- [ ] Documentation on building off of these images

## Usage

### Running Locally
To run one of the Ubuntu Idris docker images, pick a supported version tag (**0.2.2**, **0.3.0**, **0.4.0**, or **nightly**) and run the following command from an environment with a Docker daemon installed and running:

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

## Building Idris Docker Images
From the root directory of this repository, pick a stage to build (**idris-022**, **idris-030**, **idris-040**, or **idris-nightly**), and then:

```shell
docker build -t local/idris-docker:nightly --target idris-nightly -f Dockerfile.ubuntu .
```
