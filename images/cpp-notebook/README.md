# notebook for cpp lectures

```shell
make build/cpp-notebook DOCKER_BUILD_ARGS="--progress=plain"
docker run -it --rm -p 10000:8888 -v "${PWD}":/home/jovyan/work quay.io/jupyter/cpp-notebook:latest
```
