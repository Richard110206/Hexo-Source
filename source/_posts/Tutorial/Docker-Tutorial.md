---
title: Docker Tutorial
date: 2025-10-18 09:01:25
tags: [updating]
archive: true
category: Tutorial
category_bar: true
---

## Anaconda
### 环境管理
- `conda env list`或`conda info --envs` :查看所有已创建的环境（当前活动的环境会用一个星号 (*) 标出）
```bash
(base) richard@RicharddeMacBook-Air ~ % conda env list
# conda environments:
#
base                  *  /Users/richard/anaconda3
hexo-rag                 /Users/richard/anaconda3/envs/hexo-rag
yolov11-env              /Users/richard/anaconda3/envs/yolov11-env
```
- `conda create -n <env_name> python=<python_version> package1 package2 ...` :创建一个环境，指定Python版本和要安装的包
```bash
(base) richard@RicharddeMacBook-Air ~ % conda create -n my_env python=3.12 pandas numpy
# 创建名为 my_env 的环境，指定python版本为3.12，并安装 pandas 和 numpy 包
```

- `conda activate <env_name>` :激活一个环境（在行首显示当前环境名称）
```bash
(base) richard@RicharddeMacBook-Air ~ % conda activate my_env
(my_env) richard@RicharddeMacBook-Air ~ % 
```

- `conda deactivate` :退出当前环境
```bash
(my_env) richard@RicharddeMacBook-Air ~ % conda deactivate
(base) richard@RicharddeMacBook-Air ~ % conda deactivate
richard@RicharddeMacBook-Air ~ % 
# 可以退回到系统初始环境
```
- `conda create -n <new_env> --clone <env_name>` :克隆一个环境（创建一个新环境，包含与源环境相同的所有包和配置）
```bash
(base) richard@RicharddeMacBook-Air ~ % conda create -n new_env --clone my_env
Source:      /Users/richard/anaconda3/envs/my-env
Destination: /Users/richard/anaconda3/envs/new_env
Packages: 31
Files: 1
# 创建一个名为 new_env 的环境，包含与 my_env 环境相同的所有包和配置
```

- `conda remove -n <env_name> --all` :删除一个环境（删除指定环境及其所有包）
```bash
(base) richard@RicharddeMacBook-Air ~ % conda remove -n new_env --all

Remove all packages in environment /Users/richard/anaconda3/envs/new_env:
```
### 包管理
- ` conda list -n <env_name>` :列出一个环境的所有包，若无参数则默认列出当前环境的所有包
```bash
(base) richard@RicharddeMacBook-Air ~ % conda list
# packages in environment at /Users/richard/anaconda3:
#
# Name                    Version                   Build  Channel
_anaconda_depends         2024.02         py311_openblas_1  
abseil-cpp                20230802.0           h61975a4_2  
aiobotocore               2.7.0           py311hecd8cb5_0  
aiohttp                   3.9.3           py311h6c40b1e_0  
```
- `conda install -n <env_name> package1 package2 ` :在指定环境中安装包（可以安装指定版本的包，或限制版本范围）
```bash
conda install pandas=1.5.3  # 安装1.5.3版本
conda install "numpy>=1.21,<1.24"  # 安装1.21到1.24之间的版本
```
{%note danger%}
不要在conda环境中用pip安装包
- 导致不同版本的软件包依赖冲突
- conda 指令无法正常管理pip安装的软件包（安装/更新/卸载）
{%endnote %}

- `conda update -n <env_name> --all` :更新指定环境所有包到最新版本
```bash
(base) richard@RicharddeMacBook-Air ~ % conda update -n my_env --all
```
### 导出与导入环境
- `conda env export > environment.yml`：  将当前环境的包信息导出为`environment.yml`文件，方便他人复现：
```bash
(base) richard@RicharddeMacBook-Air ~ % conda env export -n yolov11-env > environment.yml
```
{%note info %}
- `-n yolov11-env`：用于指定要导出的环境名称（如果不写 -n，默认导出当前激活的环境）
{%endnote %}
- `conda env create -f environment.yml`：根据`environment.yml`文件快速创建相同环境
```bash
(base) richard@RicharddeMacBook-Air ~ % conda env create -f environment.yml
```
### 配置镜像源
- 配置清华、中科大等镜像源加速下载：
```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes  # 显示包的来源渠道
```
### 清理与维护
```bash
conda clean -p  # 清理未使用的包
conda clean -t  # 清理tar包缓存
conda clean -a  # 清理所有缓存（包括索引和未使用的包）
```

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