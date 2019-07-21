This repo contains the docker-compose.yml for defining the dependencies to run the Jupyter Notebook Image described in the Dockerfile
with the Jupyterhub container described in the Dockerfile.jupyterhub.
For more details visit our website https://www.dlm.med.fau.de/setting-jupyterhub-deep-learning/


### ac edits

in this version I've:
* pruned dependencies I personally don't need (like medical imaging libraries)
* moved to the nvidia cuda 10.0/ubuntu 18.04 container, and dealt with dependency changes stemming from that
* tried to clarify the use/install process a little by adding missing `.env` variables, cleaning up config files, adding comments, etc.
* added a shared data directory for common data so users aren't storing copies of the same data
* pinned the miniconda version
* specified use of locally built deep learning notebook container

the deep learning notebook container is still super, super huge, and could stand to be paired down even more. also, the python environment is still pretty messy, with copies of the same libraries being installed by both conda and pip alongside each other. but of course cleaning those things up is hugely time intensive because you've got to rebuild the container every time you make a change.

### install latex so you can make good graphs

if you're like me, you built these things a long time ago and don't have time to rebuild and debug the build, so you want a bandaid way to get latex in your deep learning environment because you're a glutton for punishment and you need latex in your matplotlib figures.

so, fun!

first, login to the running jupyterhub container as root:

```
docker exec -it --user root [container_tag_name] /bin/bash
```

then you'll have to update the package index 

```
apt-get update
```

then install texlive:

```
apt-get install texlive-full
```

now your container is 4GB+ larger, but you can use latex in your plots. also you have to do this every time your container starts.

### updating an ssl cert

aight, so maybe you've got a fancy let's encrypt SSL cert you want to use with this thing. and maybe that cert expired. here's what you gotta do:

1. make sure your server has `443` open to the world (scary, i know---you can close it back down after the next step)
1. run `certbot` or `letsencrypt` (maybe with `--certonly`, because). for example: `sudo letsencrypt certonly --standalone -d [FQDN]`
2. copy the keys (found in `/etc/letsencrypt/live/[FQDN]/`) to the `.ssl` dir in this repo
3. rename the files: `fullchain.pem` -> `jupyterhub.crt`, `privkey.pem` -> `jupyterhub.key`
4. `docker-compose down && docker-compose build && docker-compose up` to get the new keys into a new container

good to go. 
