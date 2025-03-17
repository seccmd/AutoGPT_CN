# AutoGPT 自托管指南：入门教程

本教程将引导您完成在本地机器上设置 AutoGPT 的过程。

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/4Bycr6_YAMI?si=dXGhFeWrCK2UkKgj" title="YouTube 视频播放器" frameborder="0" allow="加速计; 自动播放; 剪贴板写入; 加密媒体; 陀螺仪; 画中画; 网页共享" referrerpolicy="严格来源-当跨源时" allowfullscreen></iframe></center>

## 介绍

本指南将帮助您设置项目的服务器和构建器。

<!-- 视频列在仓库根目录的 Readme.md 文件中 -->

我们也提供了视频格式的教程。您可以在这里查看：[如何设置自托管](https://github.com/Significant-Gravitas/AutoGPT?tab=readme-ov-file#how-to-setup-for-self-hosting)。

!!! 警告
    **不要遵循任何外部教程，因为它们很可能已经过时**

## 先决条件

要设置服务器，您需要安装以下内容：

- [Node.js](https://nodejs.org/en/)
- [Docker](https://docs.docker.com/get-docker/)
- [Git](https://git-scm.com/downloads)

### 检查是否已安装 Node.js 和 NPM

我们使用 Node.js 来运行前端应用程序。

如果您需要安装 Node.js 的帮助：
https://nodejs.org/en/download/

NPM 包含在 Node.js 中，但如果您需要安装 NPM 的帮助：
https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

您可以通过运行以下命令来检查是否已安装 Node.js 和 NPM：

```bash
node -v
npm -v
```

一旦安装了 Node.js，您可以继续下一步。

### 检查是否已安装 Docker 和 Docker Compose

Docker 用于容器化应用程序，而 Docker Compose 用于编排多容器 Docker 应用程序。

如果您需要安装 Docker 的帮助：
https://docs.docker.com/desktop/

Docker-compose 包含在 Docker Desktop 中，但如果您需要安装 Docker Compose 的帮助：
https://docs.docker.com/compose/install/

您可以通过运行以下命令来检查是否已安装 Docker：

```bash
docker -v
docker compose -v
```

一旦安装了 Docker 和 Docker Compose，您可以继续下一步。

## 设置

### 克隆仓库
第一步是将 AutoGPT 仓库克隆到您的计算机上。
为此，请在计算机上的文件夹中打开终端窗口并运行：
```
git clone https://github.com/Significant-Gravitas/AutoGPT.git
```
如果遇到问题，请遵循[此指南](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)。

完成后，您可以关闭此终端窗口。

### 运行后端服务

要运行后端服务，请按照以下步骤操作：

* 在仓库中，克隆子模块并导航到 `autogpt_platform` 目录：
  ```bash
   git submodule update --init --recursive --progress
   cd autogpt_platform
  ```
  此命令将初始化并更新仓库中的子模块。`supabase` 文件夹将被克隆到根目录。

* 将 `supabase/docker` 目录中的 `.env.example` 文件复制到 `autogpt_platform` 中的 `.env`：
  ```
   cp supabase/docker/.env.example .env
  ```
  此命令将 `.env.example` 文件复制到 `supabase/docker` 目录中的 `.env`。您可以修改 `.env` 文件以添加自己的环境变量。

* 运行后端服务：
  ```
   docker compose up -d --build
  ```
  此命令将以分离模式启动 `docker-compose.combined.yml` 文件中定义的所有必要后端服务。

### 运行前端应用程序

要运行前端应用程序，请按照以下步骤操作：

* 导航到 `autogpt_platform` 目录中的 `frontend` 文件夹：
  ```
   cd frontend
  ```

* 将 `frontend` 目录中的 `.env.example` 文件复制到同一目录中的 `.env`：
  ```
   cp .env.example .env
  ```
  您可以修改此文件夹中的 `.env` 以添加前端应用程序的自己的环境变量。

* 运行以下命令：
  ```
   npm install
   npm run dev
  ```
  此命令将安装必要的依赖项并以开发模式启动前端应用程序。

### 检查应用程序是否正在运行

您可以通过在浏览器中访问 [http://localhost:3000](http://localhost:3000) 来检查服务器是否正在运行。

**注意：**

默认情况下，不同服务的应用程序运行在以下端口：

前端 UI 服务器：3000
后端 WebSocket 服务器：8001
执行 API REST 服务器：8006

### 附加说明

您可能希望更改 `autogpt_platform/backend` 目录中 `.env` 文件中的加密密钥。

要生成新的加密密钥，请在 Python 中运行以下命令：

```python
from cryptography.fernet import Fernet;Fernet.generate_key().decode()
```

或者在 `autogpt_platform/backend` 目录中运行以下命令：

```bash
poetry run cli gen-encrypt-key
```

然后，将 `autogpt_platform/backend/.env` 文件中的现有密钥替换为新密钥。

!!! 注意
    *以下步骤是[运行后端服务](#running-the-backend-services)的替代方案*

<details>
<summary><strong>替代步骤</strong></summary>

#### AutoGPT 代理服务器（旧版）
这是创建下一代代理执行的初始项目，即 AutoGPT 代理服务器。
代理服务器将支持创建复合多代理系统，该系统利用 AutoGPT 代理和其他非代理组件作为其原语。

##### 文档

您可以访问 [AutoGPT 代理服务器文档](https://docs.agpt.co/#1-autogpt-server)。

##### 设置

我们使用 Poetry 来管理依赖项。要设置项目，请在此目录中按照以下步骤操作：

0. 安装 Poetry

```sh
pip install poetry
```
  
1. 配置 Poetry 以在项目目录中使用 .venv

```sh
  poetry config virtualenvs.in-project true
```

2. 进入 poetry shell

```sh
poetry shell
```

3. 安装依赖项

```sh
poetry install
```

4. 将 .env.example 复制到 .env

```sh
cp .env.example .env
```

5. 生成 Prisma 客户端

```sh
poetry run prisma generate
```

> 如果 Prisma 为全局 Python 安装生成客户端而不是虚拟环境，当前的缓解措施是卸载全局 Prisma 包：
>
> ```sh
> pip uninstall prisma
> ```
>
> 然后再次运行生成。路径应如下所示：  
> `<some path>/pypoetry/virtualenvs/backend-TQIRSwR6-py3.12/bin/prisma`

6. 迁移数据库。请小心，因为这会删除数据库中的当前数据。

```sh
docker compose up db -d
poetry run prisma migrate deploy
```
    
</details>


### 不使用 Docker 启动 AutoGPT 服务器

要在本地运行服务器，请在 autogpt_platform 文件夹中启动：

```sh
cd ..
```

运行以下命令以在 Docker 中运行数据库但在本地运行应用程序：

```sh
docker compose --profile local up deps --build --detach
cd backend
poetry run app
```

### 使用 Docker 启动 AutoGPT 服务器

运行以下命令以构建 Dockerfile：

```sh
docker compose build
```

运行以下命令以运行应用程序：

```sh
docker compose up
```

运行以下命令以在代码更改时自动重建，在另一个终端中：

```sh
docker compose watch
```

运行以下命令以关闭：

```sh
docker compose down
```

如果遇到悬空孤儿问题，请尝试：

```sh
docker compose down --volumes --remove-orphans && docker-compose up --force-recreate --renew-anon-volumes --remove-orphans  
```

## 开发

### 格式化与代码检查
项目中设置了自动格式化器和代码检查器。要运行它们：

安装：
```sh
poetry install --with dev
```

格式化代码：
```sh
poetry run format
```

代码检查：
```sh
poetry run lint
```

### 测试

要运行测试：

```sh
poetry run test
```

## 项目概述

当前项目有以下主要模块：

#### **blocks**

此模块存储所有代理块，这些块是可重用的组件，用于构建表示代理行为的图。

#### **data**

此模块存储在数据库中的逻辑模型。
它将数据库操作抽象为可由服务层调用的函数。
任何与 Prisma 对象或数据库交互的代码都应驻留在此模块中。
主要模型有：
* `block`：与图中使用的块相关的任何内容
* `execution`：与图执行相关的任何内容
* `graph`：与图、节点及其关系相关的任何内容

#### **execution**

此模块存储执行图的业务逻辑。
它目前有以下主要模块：
* `manager`：一个服务，用于消费图执行队列并执行图。它包含这两部分逻辑。
* `scheduler`：一个服务，用于根据 cron 表达式触发计划的图执行。它将执行请求推送到管理器。

#### **server**

此模块存储服务器 API 的逻辑。
它包含用于 API 的所有逻辑，该 API 允许客户端创建、执行和监视图及其执行。
此 API 服务与 `manager` 和 `scheduler` 中定义的其他服务交互。

#### **utils**

此模块存储项目中使用的实用函数。
目前，它有两个主要模块：
* `process`：包含生成新进程逻辑的模块。
* `service`：作为项目中所有服务的父类的模块。

## 服务通信

目前，只有 3 个活跃的服务：

- AgentServer（API，定义在 `server.py` 中）
- ExecutionManager（执行器，定义在 `manager.py` 中）
- ExecutionScheduler（调度器，定义在 `scheduler.py` 中）

这些服务在独立的 Python 进程中运行，并通过 IPC 进行通信。
创建了一个通信层（`service.py`）以将通信库与实现解耦。

目前，IPC 使用 Pyro5 完成，并以一种允许从不同进程调用用 `@expose` 装饰的函数的方式进行抽象。

## 添加新的代理块

要添加新的代理块，您需要创建一个继承自 `Block` 的新类，并提供以下信息：
* 所有块代码应驻留在 `blocks`（`backend.blocks`）模块中。
* `input_schema`：输入数据的模式，由 Pydantic 对象表示。
* `output_schema`：输出数据的模式，由 Pydantic 对象表示。
* `run` 方法：块的主要逻辑。
* `test_input` 和 `test_output`：块的示例输入和输出数据，将用于自动测试块。
* 您可以使用 `test_mock` 字段模拟块中声明的函数以进行单元测试。
* 完成块的创建后，您可以通过运行 `poetry run pytest -s test/block/test_block.py` 来测试它。