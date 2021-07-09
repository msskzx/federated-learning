# GUIDE
## Install zerotier one


## Connect to Remote Machine
```
zerotier-cli join a84ac5c10af3be8c
ssh muhammad@10.244.159.98
```

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

### Use Docker without sudo

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

### New Dataset
Download

### Old Dataset

Downloaded from [here](https://www.cbica.upenn.edu/sbia/Spyridon.Bakas/MICCAI_BraTS/2018/MICCAI_BraTS_2018_Data_Training.zip) 

Information about the dataset available [here](https://www.med.upenn.edu/sbia/brats2018/data.html)

## Install/Run Clara SDK Docker Container

```
export dockerImage=nvcr.io/nvidia/clara-train-sdk:v3.1.01

docker pull $dockerImage

docker run -it --shm-size=1G --ulimit memlock=-1 --ulimit stack=67108864 --ipc=host --net=host --mount type=bind,source=/home/DATA/Task09_Spleen,target=/workspace/data $dockerImage /bin/bash
```

`source` contains dataset location which is mounted each time, you can remove it if you don't have the dataset ready.

to commit changes
```
docker ps

docker commit [CONTAINER_ID] deepc_fl
```

use new image in future runs

```
docker run -it --shm-size=1G --ulimit memlock=-1 --ulimit stack=67108864 --ipc=host --net=host --mount type=bind,source=/home/DATA/Task09_Spleen,target=/workspace/data deepc_fl /bin/bash
```

## Provisioning
inside `/opt/nvidia/medical/tools/` run:
```
python3 provision.py
```
which prints the passwords used for extraction 
```
cd packages
```
and run:
```
unzip -P $PASSWORD $ZIP_FILE -d $DIRECTORY_TO_EXTRACT_TO
```
current setup uses directories:
 - `./server`
 - `./admin`
 - `./client1`
 - `./client2`
 - `./client3`

 ```
unzip -P rFNG7KHwfLiDBlWP server.zip -d ./server
unzip -P ABFUpcfQOKYiZ7l3 org1-a.zip -d ./client1
unzip -P dE7k5WSiQOmvMsKn org1-b.zip -d ./client2
unzip -P AYElb0crGZ50Squa org2.zip -d ./client3
unzip -P lmyAU2IzJZhXKrqO admin@nvidia.com.zip -d ./admin
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
admin                 client2          it_org2     org2.zip                   researcher@org2.com.zip  researcher_org2
admin@nvidia.com.zip  client3          org1-a.zip  researcher@nvidia.com.zip  researcher_nvidia        server
client1               it@org2.com.zip  org1-b.zip  researcher@org1.com.zip    researcher_org1          server.zip
```

## Project config

Change the config in `opt/nvidia/medical/tools/project.yml`, mainly:
```
cn: localhost
```

commit changes to the image

## Server
In a new docker container, go inside `opt/nvidia/medical/tools/packages/server/startup/` and run:

```
sh start.sh
```

## Clients
In a new docker container, go inside `/packages/client1/startup/` and run:

```
sh start.sh
```

## Admin
In a new docker container, go inside`/packages/admin/startup/` and run:
```
sh fl.admin.sh
```
in the admin window type the admin e-mail `admin@nvidia.com`
check status as following
```
> check_status server
FL run number has not been set.
FL server status: training not started
Registered clients: 3 
------------------------------------------------------------------------
| CLIENT NAME | TOKEN                                | SUBMITTED MODEL |
------------------------------------------------------------------------
| org1-a      | 9d78aa21-93e2-4721-b0db-72e631b12e18 |                 |
| org1-b      | 8cd3fb28-1210-47f3-a122-544ccd2efed2 |                 |
| org2        | 275b0315-9305-443c-858d-220b44c7a805 |                 |
------------------------------------------------------------------------
Done [7316 usecs] 2021-07-09 14:31:00.641830
> check_status client
instance:org1-a : client name: org1-a   token: 9d78aa21-93e2-4721-b0db-72e631b12e18     status: training not started
instance:org1-b : client name: org1-b   token: 8cd3fb28-1210-47f3-a122-544ccd2efed2     status: training not started
instance:org2 : client name: org2       token: 275b0315-9305-443c-858d-220b44c7a805     status: training not started

Done [407687 usecs] 2021-07-09 14:31:05.973040
```

## Install Model
We used a model from Clara Medical Model Archive (MMAR). Inside the docker container we use:

### New Model
```
ngc registry model list nvidia/med/*
MODEL_NAME=clara_pt_spleen_ct_segmentation
VERSION=1
ngc registry model download-version nvidia/med/$MODEL_NAME:$VERSION --dest /opt/nvidia/medical/tools/packages/admin/transfer/
```
### Old Model
```
MODEL_NAME=clara_mri_seg_brain_tumors_br16_full_amp
VERSION=1
```
### Config

## (Optional) Missing Config Files

## Upload and Deploy Model

```
> set_run_number 123
Create a new run folder: run_123
Done [17737 usecs] 2021-06-23 16:19:09.272038
```
```
> upload_folder clara_pt_spleen_ct_segmentation_v1
Created folder /opt/nvidia/medical/tools/packages/server/startup/../transfer/clara_pt_spleen_ct_segmentation_v1
Done [1440278 usecs] 2021-06-23 16:41:15.208990
```
```
> deploy clara_pt_spleen_ct_segmentation_v1 server
mmar_server has been deployed.
Done [51335 usecs] 2021-06-23 16:41:28.026814
```
```
> deploy clara_pt_spleen_ct_segmentation_v1 client
instance:org1-a : No replies

Done [10058042 usecs] 2021-06-23 16:41:42.309283
```

## Start Training
```
> start server
Server training is starting....
Done [20034 usecs] 2021-06-23 16:45:39.487408
```
```
> start client
Server training has not started.
Done [18308 usecs] 2021-06-23 16:45:57.961999
```
## References
- [Install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
- [Install Docker Compose](https://docs.docker.com/compose/install/)
- [Install/Run Clara SDK Docker Container](https://docs.nvidia.com/clara/tlt-mi/nvmidl/installation.html)
- [Install Model](https://docs.nvidia.com/clara/tlt-mi/nvmidl/installation.html#downloading-the-models)
- [Model](https://ngc.nvidia.com/catalog/models/nvidia:med:clara_mri_seg_brain_tumors_br16_full_amp)
- [Provisioning](https://docs.nvidia.com/clara/tlt-mi/federated-learning/fl_user_guide.html#provisioning-a-federated-learning-project)
- [Run as Admin](https://docs.nvidia.com/clara/clara-train-archive/3.1/federated-learning/fl_user_guide.html#operate-running-federated-learning-as-an-administrator)