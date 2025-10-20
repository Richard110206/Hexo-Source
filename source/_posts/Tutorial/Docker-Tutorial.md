---
title: Docker Tutorial
date: 2025-10-18 09:01:25
tags:
archive: true
category: Tutorial
category_bar: true
---


[DockerHub](https://hub.docker.com/repositories/richard110206)

dockerfile->dockerimage(如何正确设置容器的说明，存储在注册表中）->dockercontainer 

出现拉取`requests`时候超时的问题：
```bash
 => CANCELED [4/4] RUN pip3 install -r requirements.txt                 101.2s
```
在 `Dockerfile` 中配置全局镜像源为清华大学镜像源解决
```dockerfile
FROM python:3.9-slim-buster

COPY ./requirements.txt ./requirements.txt
COPY ./main.py ./main.py
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
RUN pip install -r requirements.txt
CMD ["python3", "./main.py"]
```
```bash
richard@RicharddeMacBook-Air test % docker build -t python-test .
[+] Building 9.0s (10/10) FINISHED                        docker:desktop-linux
 => [internal] load build definition from Dockerfile                      0.0s
 => => transferring dockerfile: 278B                                      0.0s
 => [internal] load metadata for docker.io/library/python:3.9-slim-buste  0.0s
 => [internal] load .dockerignore                                         0.0s
 => => transferring context: 2B                                           0.0s
 => [1/5] FROM docker.io/library/python:3.9-slim-buster@sha256:320a7a425  0.0s
 => => resolve docker.io/library/python:3.9-slim-buster@sha256:320a7a425  0.0s
 => [internal] load build context                                         0.0s
 => => transferring context: 63B                                          0.0s
 => CACHED [2/5] COPY ./requirements.txt ./requirements.txt               0.0s
 => CACHED [3/5] COPY ./main.py ./main.py                                 0.0s
 => [4/5] RUN pip config set global.index-url https://pypi.tuna.tsinghua  0.6s
 => [5/5] RUN pip install -r requirements.txt                             8.2s
 => exporting to image                                                    0.2s
 => => exporting layers                                                   0.1s
 => => exporting manifest sha256:141fb8ff8b81af29e322ca4900514b916baf04e  0.0s 
 => => exporting config sha256:e299cb6be9a3b32c54e48d1af9a0dac8cc3b4118e  0.0s 
 => => exporting attestation manifest sha256:33eba2c9f12699afb00e6e5ed9a  0.0s 
 => => exporting manifest list sha256:3b9393a9d83c2a31c938e4c7343ad95dea  0.0s 
 => => naming to docker.io/library/python-test:latest                     0.0s
 => => unpacking to docker.io/library/python-test:latest                  0.1s
richard@RicharddeMacBook-Air test % docker run python-test       
hello world
```