---
layout: mypost
title: How to connect jupyter in a remote server to local machine
categories: [tips]
---


## What to make
Access remote server with GPU in local by Jupyter Lab or Jupyter Notebook. The route is like this:  
local --> server --> Docker container --> jupyter  


## In remote server


### Installation
- nvidia-device
- cuda
- cudnn
- docker
- nvidia-docker


#### Check installed

- Check GPU: 
`nvidia-smi`

- Check cuda: 
`nvcc -V` or `cat /usr/local/cuda/version.txt`

- Check cudnn: 
`cat /usr/include/cudnn.h | grep CUDNN_MAJOR -A 2`  
Or: `cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`

- Check nvidia-docker: 
`nvidia-docker version`


### Pull image
- Pull the image: [ufoym/deepo:all-jupyter](https://hub.docker.com/r/ufoym/deepo/tags?page=1&name=jupyter)
`docker pull ufoym/deepo:all-jupyter`  
(nvidia-docker, jupyter and torch, etc are installed.)

- Start the container with port 9999
`nvidia-docker run -it -p 9999:9999 --name remote_jupyter ufoym/deepo bash`

- Run the jupyter in the container
Notebook: `jupyter notebook --ip 0.0.0.0 --port 9999 --allow-root`  
Lab: `jupyter notebook --ip 0.0.0.0 --port 9999 --allow-root`

(Restart container when stopped: `docker start [container id] -i`)

- We can see a start link like this: 
`http://127.0.0.1:9999/?token=9f7fa34be60196e61cc83627105a3a74c23bb9002206797a`


## In local machine
- Link local port 9999 to remote server port 9999
`ssh -L9999:localhost:9999 [remote user]@[remote IP]`

- Open the start link in browser
`http://127.0.0.1:9999/?token=9f7fa34be60196e61cc83627105a3a74c23bb9002206797a`


## Reference:
- Blog: https://towardsdatascience.com/using-jupyter-notebook-running-on-a-remote-docker-container-via-ssh-ea2c3ebb9055
- Image: https://github.com/ufoym/deepo