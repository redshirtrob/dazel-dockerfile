# Dazel Dockerfile

This repository is a demonstratio of a dazel bug. When the user
specifies `DAZEL_DOCKERFILE` in the `.dazelrc` file, and the
corresponding Dockerfile contains directives that copy files from
relative paths, then the image construction fails.

## TL;DR How Do I Reproduce This?
```bash
$ dazel build '//...'
Sending build context to Docker daemon   51.2kB
Step 1/3 : FROM insready/bazel
 ---> 8f30918a7d5f
Step 2/3 : COPY "hello-world" /
COPY failed: stat /var/lib/docker/tmp/docker-builder954504696/hello-world: no such file or directory
```

But it's easy to create the image once inside the "docker" directory:
```bash
$ docker build -t dazel-dockerfile .
Sending build context to Docker daemon  4.096kB
Step 1/3 : FROM insready/bazel
 ---> 8f30918a7d5f
Step 2/3 : COPY "hello-world" /
 ---> Using cache
 ---> 5ced0f7504a8
Step 3/3 : CMD "/hello-world"
 ---> Using cache
 ---> ef3c3810e5ea
Successfully built ef3c3810e5ea
Successfully tagged dazel-dockerfile:latest
rob @ ~/ws/dazel-image-file/docker - [] $ docker run --rm dazel-dockerfile
Hello, Dazel
$ 
```
