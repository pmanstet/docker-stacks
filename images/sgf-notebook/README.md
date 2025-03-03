# notebook for sgf lectures

## build/test
```shell
make build/sgf-notebook DOCKER_BUILD_ARGS="--progress=plain --no-cache"
docker run -it --rm -p 10000:8888 -v "${PWD}":/home/jovyan/work quay.io/jupyter/sgf-notebook:latest
```

## publish
```
OWN_REGISTRY= ...
OWN_PROJECT= ...
docker tag quay.io/jupyter/sgf-notebook "${OWN_REGISTRY}/${OWN_PROJECT}/sgf-notebook"
docker login "${OWN_REGISTRY}"
docker push "${OWN_REGISTRY}/${OWN_PROJECT}/sgf-notebook"
```
