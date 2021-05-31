# GUIDE

## Install Docker

The Clara framework is maintained in a docker image

```
sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

sudo apt update

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl status docker
```

### Use Docker without sudo (optional)

We add a user group in order to use `docker` instead of `sudo docker`.

```
sudo usermod -aG docker ${USER}

su - ${USER}

id -nG
```

## Download Data-set

Downloaded from [here](https://www.cbica.upenn.edu/sbia/Spyridon.Bakas/MICCAI_BraTS/2018/MICCAI_BraTS_2018_Data_Training.zip) 

## Install/Run Clara SDK Docker Container

```
export dockerImage=nvcr.io/nvidia/clara-train-sdk:v3.1.01

docker pull $dockerImage

docker run -it --rm --shm-size=1G --ulimit memlock=-1 --ulimit stack=67108864 --ipc=host --net=host --mount type=bind,source=/your/dataset/location,target=/workspace/data $dockerImage /bin/bash
```

## Install Model
We used a model from Clara Medical Model Archive (MMAR). Inside the docker container we use:

```
ngc registry model list nvidia/med/*

MODEL_NAME=clara_mri_seg_brain_tumors_br16_full_amp
VERSION=1

ngc registry model download-version nvidia/med/$MODEL_NAME:$VERSION --dest /var/tmp
```

## References
- [Install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
- [Install/Run Clara SDK Docker Container](https://docs.nvidia.com/clara/tlt-mi/nvmidl/installation.html)
- [Install Model](https://docs.nvidia.com/clara/tlt-mi/nvmidl/installation.html#downloading-the-models)
- [Model](https://ngc.nvidia.com/catalog/models/nvidia:med:clara_mri_seg_brain_tumors_br16_full_amp)
