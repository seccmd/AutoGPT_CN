# 高级设置

以下高级步骤适用于具有系统管理员经验的人员。如果您对这些步骤不熟悉，请参考[基础设置指南](../platform/getting-started.md)。

## 简介

对于高级设置，首先按照[基础设置指南](../platform/getting-started.md)启动并运行服务器。一旦服务器运行起来，您可以按照以下步骤根据您的具体需求配置服务器。

## 配置

### 通过环境变量设置配置

服务器使用环境变量来存储配置。您可以在项目根目录下的`.env`文件中设置这些环境变量。`.env`文件应如下所示：

```bash
# .env
KEY1=value1
KEY2=value2
```

服务器启动时会自动加载`.env`文件。您也可以在shell中直接设置环境变量。请参考您操作系统的文档，了解如何在当前会话中设置环境变量。

有效的选项列在构建器和服务器目录根目录下的`.env.example`文件中。您可以将`.env.example`文件复制为`.env`并根据需要修改值。

```bash
# 将.env.example文件复制为.env
cp .env.example .env
```

### 密钥目录

密钥目录位于`./secrets`。您可以在此目录中存储任何需要的密钥。服务器启动时会自动加载这些密钥。

一个名为`my_secret`的密钥示例如下：

```bash
# ./secrets/my_secret
my_secret_value
```

这在运行docker时非常有用，因为您可以将密钥复制到容器中，而不会在Dockerfile中暴露它们。

## 数据库选择

### PostgreSQL

我们使用Supabase PostgreSQL作为数据库。您需要将生成和运行prisma的命令替换为以下内容：

```bash
poetry run prisma generate --schema postgres/schema.prisma
```

这将生成PostgreSQL的Prisma客户端。您还需要在单独的容器中运行PostgreSQL数据库。您可以使用`rnd`目录中的`docker-compose.yml`文件来运行PostgreSQL数据库。

```bash
cd autogpt_platform/
docker compose up -d --build
```

然后您可以从`backend`目录运行迁移。

```bash
cd ../backend
prisma migrate dev --schema postgres/schema.prisma
```

## AutoGPT代理服务器高级设置

本指南将引导您完成一个使用外部数据库（PostgreSQL）的docker化设置。

### 设置

我们使用Poetry来管理依赖项。要设置项目，请在此目录中按照以下步骤操作：

0. 安装Poetry
    ```sh
    pip install poetry
    ```
    
1. 配置Poetry以在项目目录中使用.venv
    ```sh
    poetry config virtualenvs.in-project true
    ```

2. 进入poetry shell

   ```sh
   poetry shell
   ```
   
3. 安装依赖项

   ```sh
   poetry install
   ```
   
4. 将.env.example复制为.env

   ```sh
   cp .env.example .env
   ```

5. 生成Prisma客户端

   ```sh
   poetry run prisma generate
   ```
   

   > 如果Prisma为全局Python安装生成了客户端而不是虚拟环境，当前的解决方法是卸载全局Prisma包：
   >
   > ```sh
   > pip uninstall prisma
   > ```
   >
   > 然后再次运行生成。路径*应该*看起来像这样：  
   > `<some path>/pypoetry/virtualenvs/backend-TQIRSwR6-py3.12/bin/prisma`

6. 从/rnd文件夹运行postgres数据库

   ```sh
   cd autogpt_platform/
   docker compose up -d
   ```

7. 运行迁移（从backend文件夹）

   ```sh
   cd ../backend
   prisma migrate deploy
   ```

### 运行服务器

#### 直接启动服务器

运行以下命令：

```sh
poetry run app
```