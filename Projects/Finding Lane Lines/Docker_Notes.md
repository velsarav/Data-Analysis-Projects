# Automate the following

docker run -it --rm --entrypoint "/run.sh" -p 8888:8888 -v `pwd`:/src udacity/carnd-term1-starter-kit

## Docker Reference
[nvidia docker redference](https://devtalk.nvidia.com/default/topic/1032202/docker-and-nvidia-docker/using-a-jupyter-notebook-within-a-docker-container/)

## Get the Container ID
docker ps 

## Copy file to Container
 docker cp src/ 1bd92038cd22:/src

## Copy file from Container
docker cp 91c4e97e1f4d:/src .
