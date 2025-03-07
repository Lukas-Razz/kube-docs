---
layout: article
title: JupyterHub
permalink: /docs/jupyterhub.html
key: jupyterhub
aside:
  toc: true
sidebar:
  nav: docs
---

# JupyterHub 

We provide a JupyterHub running on Kubernetes for every MetaCentrum member. The hub can be accessed on `hub.cerit-sc.cz`. Sign in with meta username (do not use @META, only username). 

## Choosing image

Any Jupyter notebook image can be run, options already provided:
- jupyter/minimal-notebook
- jupyter/datascience-notebook
- jupyter/scipy-notebook
- jupyter/tensorflow-notebook

If you choose custom, you have to provide image name together with its repo and optional tag - input text in format _repo/imagename:tag_.
`minimal-notebook` is chosen as default image.


## Choosing storage
By default, every notebook runs with persistent storage mounted to `/home/jovyan`. Therefore, we recommend to save the data to `/home/jovyan` directory to have them accessible every time notebook is spawned. 


Optionally, you can mount your MetaCentrum home - check the option and select the desired home. Now, it is possible to mount only one home per notebook. In hub, your home is located in `/home/meta/{meta-username}`.

Possible homes:

brno10-ceitec-hsm | brno11-elixir | brno12-cerit | brno14-ceitec | vestec1-elixir
--- | --- | --- | --- |--- 
brno1-cerit | brno2 | brno3-cerit | brno6  | praha1
brno8 | brno9-ceitec | budejovice1 | du-cesnet | praha2-natur
liberec3-tul | ostrava1 | ostrava2-archive | pruhonice1-ibot | praha5-elixir
plzen1 | plzen4-ntis                   
 

## Named servers
In the top left corner, go to `File &rarr; Hub Control Panel`. Fill in the `Server name` and click on `Add new server`, you will be presented with input form page. 

## Conda environment
Conda is supported in all provided images and we can assure its functionality. 

New conda environment in hub's terminal is created with command `conda create -n tenv --yes python=3.8 ipykernel nb_conda_kernels` (`ipykernel nb_conda_kernels` part is required, alternatively irkernel). 

Check if environment is installed with `conda env list`. You can use the environment right away, either by creating new notebook or changing the kernel of existing one (tab `Kernel` → `Change Kernel...` and choose the one you want to use).

## Install Conda packages
To install conda packages you have to create new conda environment (as described above). Then, install new packages in terminal into newly created environment e.g. `conda install keyring -n myenv`. 

Open new notebook and change the kernel in tab `Kernel` → `Change Kernel...` → `myenv` (or the nemae of kernel you installed packages into).


## Error handling
You receive _HTTP 500:Internal Server Error_ when accessing the URL `/user/your_name`. Most likely, this error is caused by:
1. You chose MetaCentrum home you haven't used before - The red 500 Error is followed by `Error in Authenticator.pre_spawn_start`
2. You chose MetaCentrum home you don't have access to - The red 500 Error is followed by `Error in Authenticator.pre_spawn_start`
3. While spawning, `Error: ImagePullBackOff` appears

Solutions:
1. Log out and log in back
2. You can not access the home even after logging out and back in - you are not permitted to use this particular home
3. Clicking on small arrow `Event log` provides more information. Most certainly, a message tagged `[Warning]` is somewhere among all of them and it provides more description. It is highly possible the repo and/or image name is misspelled. 
 - please wait for 10 minutes
 - The service has a timeout of 10 minutes and during this time, it is trying to create all necessary resources. Due to error, creation won't succeed and after 10 minutes you will see red progress bars with message `Spawn failed: pod/jupyter-[username] did not start in 600 seconds!`. At this point, it is sufficient to reload the page and click on `Relaunch server`.

## I've chosen wrong home! What now?!

If notebook is already running, in the top left corner, go to `File ` &rarr; `Hub Control Panel` and click red `Stop My Server`. In a couple of seconds, your container hub instance will be deleted (stop button disapperas) and you can again `Start Server` with different home. 

Alternatively, you can create another named server. Fill in the `Server name` and click on `Add new server`, you will be presented with input form page. 

All of your named server are accessible under `Hub Control panel` where you can manipulate with them (create, delete, log in to).


## Feature requests
Any tips for features or new notebook types are welcomed in RT.


