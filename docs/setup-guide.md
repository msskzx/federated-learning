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

## Install Docker Compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

## Download Data-set

Downloaded from [here](https://www.cbica.upenn.edu/sbia/Spyridon.Bakas/MICCAI_BraTS/2018/MICCAI_BraTS_2018_Data_Training.zip) 

Information about the dataset available [here](https://www.med.upenn.edu/sbia/brats2018/data.html)

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

## Provisioning

```
python3 provision.py

unzip -P $PASSWORD $ZIP_FILE -d $DIRECTORY_TO_EXTRACT_TO
```

Output and extrction location:
```
root@Zlightosaur:/opt/nvidia/medical/tools# python3 provision.py
Build docker.sh based on container: nvcr.io/nvidia/clara-train-sdk:v3.1.01
You may change the container image by setting FL_DOCKER_IMAGE environment variable
Using the default audit file.
Audit file: /opt/nvidia/medical/tools/audit.pkl

Using the default authz_config.json file.
Authorization configuration file: /opt/nvidia/medical/tools/authz_config.json

Using the default project yaml file.
Project yaml file: /opt/nvidia/medical/tools/project.yml.

This set of packages is generated for example_project project and server's cn is example.com.

Both authz_def.json and /opt/nvidia/medical/tools/authz_config.json are compliant with Authorization Policy

┏━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃ Password         ┃ Zip file name             ┃ Email of recipient (if provided) ┃
┡━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┩
│ DSRE3XHbcNu4iknP │ server.zip                │                                  │
│ BNvLOwH7xW2rpf90 │ org1-a.zip                │                                  │
│ 8Rt1DUIBFSquHzre │ org1-b.zip                │                                  │
│ BSxfz05AHVn39MPi │ org2.zip                  │                                  │
│ GMiYhq4W5Xl9udbv │ admin@nvidia.com.zip      │ admin@nvidia.com                 │
│ 86LoMOi3DZGJrwe2 │ researcher@nvidia.com.zip │ researcher@nvidia.com            │
│ 2neJWoYTyQZxqSah │ researcher@org1.com.zip   │ researcher@org1.com              │
│ wQqKrSuCXxJ9be25 │ researcher@org2.com.zip   │ researcher@org2.com              │
│ VEXxazsmIKW75P9U │ it@org2.com.zip           │ it@org2.com                      │
└──────────────────┴───────────────────────────┴──────────────────────────────────┘

Finished generating packages.
All generated zip files are inside /opt/nvidia/medical/tools/packages folder.
```
```
root@Zlightosaur:/opt/nvidia/medical/tools/packages# ls
admin                 it               org1-a.zip  org2.zip    researcher1  researcher@nvidia.com.zip  researcher@org2.com.zip  server.zip
admin@nvidia.com.zip  it@org2.com.zip  org1-b.zip  researcher  researcher2  researcher@org1.com.zip    server                   startup
```



## References
- [Install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
- [Install Docker Compose](https://docs.docker.com/compose/install/)
- [Install/Run Clara SDK Docker Container](https://docs.nvidia.com/clara/tlt-mi/nvmidl/installation.html)
- [Install Model](https://docs.nvidia.com/clara/tlt-mi/nvmidl/installation.html#downloading-the-models)
- [Model](https://ngc.nvidia.com/catalog/models/nvidia:med:clara_mri_seg_brain_tumors_br16_full_amp)
- [Provisioning](https://docs.nvidia.com/clara/tlt-mi/federated-learning/fl_user_guide.html#provisioning-a-federated-learning-project)