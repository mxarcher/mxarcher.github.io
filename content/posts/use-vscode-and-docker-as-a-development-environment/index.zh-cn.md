+++
title = "使用 vscode + docker 作为开发环境"
subtitle = ""
date = "2022-09-18T22:12:11+08:00"
description = ""
tags = ["vscode","docker","archlinux"]
comment = true
draft = false
+++

{{< admonition type=note title="Note" open=true >}} 
本文只是对 [使用Docker作为C++开发环境：适用于CLion与VSCode的配置 - 灰格猫的编程日记 (graueneko.com)](https://graueneko.com/archives/64/) 做了适合自己的定制
{{< /admonition >}}

## 文件目录结构
```bash
./.devcontainer
├── .devcontainer.json
├── docker-compose.yml
└── Dockerfile
```
## DockerFile 配置
```DockerFile
# Dockerfile
ROM archlinux:latest
ENV USER=mxarcher
ENV PASSWD=what
ENV WORKDIR=/home/${USER}/projects

# 配置清华源和科大源
RUN printf '\n\
    Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch\n\
    Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch' > /etc/pacman.d/mirrorlist

# 安装开发环境必要的包
RUN pacman -Syyu --noconfirm && \
    pacman -S --noconfirm base-devel clang llvm sudo git cmake boost

# 添加用户并配置密码
RUN useradd -m ${USER} && yes ${PASSWD} | passwd ${USER}

# 赋予sudo权限并允许无密码sudo
RUN echo ${USER}' ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN chmod 644 /etc/sudoers

USER ${USER}
RUN mkdir -p ${WORKDIR}
```
## docker-compose 文件配置
```yaml
# docker-compose.yml
version: "3.8"

services:
  projects-dev:
    restart: unless-stopped
    # 使用当前目录的 Dockerfile 来构建 docker 镜像
    build: .
    container_name: projects-dev
    hostname: "projects"
    # 如果在Dockerfile中修改过用户名，此处也要对应修改用户名和工作目录
    user: mxarcher
    working_dir: /home/mxarcher/projects
    # 修改安全配置，以运行gdb server
    security_opt:
      - seccomp:unconfined
    cap_add:
      - SYS_PTRACE
    # 通过 tailf 命令保持 container 不要退出的状态
    command: "tail -f /dev/null"
    volumes:
      # 本文件存放于.devcontainer/中，因而此处要把上级目录(源代码目录)挂载到工作目录
      - ..:/home/mxarcher/projects/
```
## .devcontainer 配置
```json
// .devcontainer.json
{
    "name": "Project Develop Env",
    "dockerComposeFile": "docker-compose.yml",
    "service": "projects-dev",
    "workspaceFolder": "/home/mxarcher/projects/",
    "remoteUser": "mxarcher"
}
```
