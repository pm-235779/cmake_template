## Docker Instructions

If you have Docker installed, you can run the following commands to set up and use the Docker container. This guide has been updated to ensure clarity and fix any inconsistencies.

---

## Build the Docker Image
1. Navigate to the directory containing the `Dockerfile` (located in `.devcontainer`).
2. Run the following command to build the image:

```bash
docker build -f ./.devcontainer/Dockerfile --tag=my_project:latest .
docker build --tag=my_project:latest --build-arg GCC_VER=10 --build-arg LLVM_VER=11 .
docker build --tag=my_project:latest --build-arg USE_CLANG=1 .

```


```bash
docker build --tag=myproject:latest --build-arg GCC_VER=10 --build-arg LLVM_VER=11 .
```

The CC and CXX environment variables are set to GCC version 11 by default.
If you wish to use clang as your default CC and CXX environment variables, you
may do so like this:

```bash
docker build --tag=my_project:latest --build-arg USE_CLANG=1 .

```

You will be logged in as root, so you will see the `#` symbol as your prompt.
You will be in a directory that contains a copy of the `cpp_starter_project`;
any changes you make to your local copy will not be updated in the Docker image
until you rebuild it.
If you need to mount your local copy directly in the Docker image, see
[Docker volumes docs](https://docs.docker.com/storage/volumes/).
TLDR:

```bash
docker run -it \
	-v absolute_path_on_host_machine:absolute_path_in_guest_container \
	my_project:latest
```

You can configure and build [as directed above](#build) using these commands:

```bash
/starter_project# mkdir build
/starter_project# cmake -S . -B ./build
/starter_project# cmake --build ./build
```

You can configure and build using `clang-13`, without rebuilding the container,
with these commands:

```bash
/starter_project# mkdir build
/starter_project# CC=clang CXX=clang++ cmake -S . -B ./build
/starter_project# cmake --build ./build
```

For an interactive configuration, you can use ccmake instead of cmake:

````bash
ccmake -S . -B ./build

````
Troubleshooting
Common Issues
-fno-fat-lto-objects error:

This error occurs due to an unsupported optimization flag. To resolve this, ensure the compiler version matches the one specified in the Dockerfile. You can also explicitly set CXXFLAGS or modify the build scripts to exclude unsupported flags.
Warnings treated as errors:

If warnings are treated as errors during the build, disable the -Werror flag in the CMakeLists.txt or build script.
File Syncing Issues:

Ensure the paths for volume mounting (-v) are correct.
Testing the Docker Setup
Run the following command to validate the Docker environment:

````bash

docker build -f ./.devcontainer/Dockerfile --tag=my_project:latest . && docker run -it my_project:latest
Example: Build GUI Projects
A script called build_examples.sh is included to build example GUI projects in this container:
````


Example: Build GUI Projects
A script called build_examples.sh is included to build example GUI projects in this container:

````bash
Copy code
./build_examples.sh
````


All of the tools this project supports are installed in the Docker image;
enabling them is as simple as flipping a switch using the `ccmake` interface.
Be aware that some of the sanitizers conflict with each other, so be sure to
run them separately.

A script called `build_examples.sh` is provided to help you to build the example
GUI projects in this container.

