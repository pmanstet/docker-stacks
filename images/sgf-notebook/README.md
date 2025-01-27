# IUE specific Notebook for lectures

```shell
cd sgit.iue/jupyter/docker-stacks
mkdir -p ./images/cpp-notebook/sgit.iue/360251/
cp -r /home/manstetten/sgit.iue/360251/clang_repl_kernel ./images/cpp-notebook/sgit.iue/360251/clang_repl_kernel
mkdir -p ./images/cpp-notebook/sgit.iue/360033/
cp -r /home/manstetten/sgit.iue/360033/sgframework ./images/cpp-notebook/sgit.iue/360033/sgframework
cp -r /home/manstetten/sgit.iue/360033/materials_sources ./images/cpp-notebook/sgit.iue/360033/materials_sources

make build/cpp-notebook DOCKER_BUILD_ARGS="--progress=plain"
docker run -it --rm -p 10000:8888 -v "${PWD}":/home/jovyan/work quay.io/jupyter/cpp-notebook:latest

docker tag quay.io/jupyter/cpp-notebook quay.io/iue360/cpp-notebook 
docker login quay.io # username (iue360) + encrypted CLI passwd (obtain/set from red hat portal at https://quay.io/user/iue360/?tab=settings) 
docker push quay.io/iue360/cpp-notebook
```
