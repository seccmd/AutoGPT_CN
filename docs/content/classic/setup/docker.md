# AutoGPT + Docker 指南

!!! important
    使用 Compose 文件格式版本 3.9 需要 Docker Compose 版本 1.29.0 或更高。
    您可以通过运行以下命令检查系统上安装的 Docker Compose 版本：

    ```shell
    docker compose version
    ```

    这将显示系统上当前安装的 Docker Compose 版本。

    如果需要升级 Docker Compose 到新版本，可以按照 Docker 文档中的安装说明操作：https://docs.docker.com/compose/install/

## 基本设置

1. 确保已安装 Docker，参见 [要求](./index.md#requirements)
2. 为 AutoGPT 创建项目目录

    ```shell
    mkdir AutoGPT
    cd AutoGPT
    ```

3. 在项目目录中，创建名为 `docker-compose.yml` 的文件：

    <details>
    <summary>
      <= v0.4.7 版本的 <code>docker-compose.yml></code>
    </summary>

    ```yaml
    version: "3.9"
    services:
      auto-gpt:
        image: significantgravitas/auto-gpt
        env_file:
          - .env
        profiles: ["exclude-from-up"]
        volumes:
          - ./auto_gpt_workspace:/app/auto_gpt_workspace
          - ./data:/app/data
          ## 允许 auto-gpt 将日志写入磁盘
          - ./logs:/app/logs
          ## 如果希望使用这些文件，取消注释以下行
          ## 这些文件必须与此 docker-compose.yml 文件位于同一文件夹中
          #- type: bind
          #  source: ./azure.yaml
          #  target: /app/azure.yaml
    ```
    </details>

    <details>
    <summary>
      > v0.4.7 版本（包括 <code>master</code>）的 <code>docker-compose.yml></code>
    </summary>

    ```yaml
    version: "3.9"
    services:
      auto-gpt:
        image: significantgravitas/auto-gpt
        env_file:
          - .env
        ports:
          - "8000:8000"  # 如果只想在 TTY 模式下运行单个代理，请删除此行
        profiles: ["exclude-from-up"]
        volumes:
          - ./data:/app/data
          ## 允许 auto-gpt 将日志写入磁盘
          - ./logs:/app/logs
          ## 如果希望使用这些文件，取消注释以下行
          ## 这些文件必须与此 docker-compose.yml 文件位于同一文件夹中
          ## 组件配置文件
          #- type: bind
          #  source: ./config.json
          #  target: /app/config.json
    ```
    </details>


1. 下载 [`.env.template`][.env.template] 并保存为 AutoGPT 文件夹中的 `.env`。
2. 按照标准 [配置说明](./index.md#completing-the-setup) 操作，
   从第 3 步开始，不包括 `poetry install` 步骤。
3. 从 [Docker Hub] 拉取最新镜像

    ```shell
    docker pull significantgravitas/auto-gpt
    ```
4. _可选：挂载配置文件。_
      如果有组件配置文件，例如 `config.json`，将其放在 `classic/original_autogpt/data/` 目录中。或放在 `classic/original_autogpt/` 中并取消 `docker-compose.yml` 中挂载它的行。
      要了解更多关于配置的信息，请参阅 [组件配置](../../forge/components/components.md#json-configuration)

!!! note "Docker 仅支持无头浏览"
    AutoGPT 默认使用无头模式浏览器：`HEADLESS_BROWSER=True`。
    请不要在结合 Docker 使用时更改此设置，否则 AutoGPT 会崩溃。

[.env.template]: https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/.env.template
[Docker Hub]: https://hub.docker.com/r/significantgravitas/auto-gpt

## 开发者设置

!!! tip
    如果您已克隆仓库并已（或希望）对代码库进行更改，请使用此设置。

1. 复制 `.env.template` 为 `.env`。
2. 按照标准 [配置说明](./index.md#completing-the-setup) 操作，
   从第 3 步开始，不包括 `poetry install` 步骤。

## 使用 Docker 运行 AutoGPT

按照上述设置说明操作后，您可以使用以下命令运行 AutoGPT：

```shell
docker compose run --rm auto-gpt
```

这将创建并启动一个 AutoGPT 容器，并在应用程序停止后删除它。
这并不意味着您的数据会丢失：应用程序生成的数据存储在 `data` 文件夹中。

子命令和参数与 [用户指南] 中描述的相同：

* 运行 AutoGPT：
    ```shell
    docker compose run --rm auto-gpt serve
    ```
* 在 TTY 模式下运行 AutoGPT，使用连续模式。
    ```shell
    docker compose run --rm auto-gpt run --continuous
    ```
* 在 TTY 模式下运行 AutoGPT 并安装所有活动插件的依赖项：
    ```shell
    docker compose run --rm auto-gpt run --install-plugin-deps
    ```

如果您敢于尝试，也可以使用“普通”的 docker 命令构建并运行它：

```shell
docker build -t autogpt .
docker run -it --env-file=.env -v $PWD:/app autogpt
docker run -it --env-file=.env -v $PWD:/app --rm autogpt --gpt3only --continuous
```

[用户指南]: ../usage.md/#命令行界面