# Hugging Face Space 部署说明

这套配置用于把仓库里的 Space 模板自动同步到 Hugging Face Space，并直接复用 GHCR 里的 `sub2api-v2` 镜像。

## 需要先准备的内容

1. 在 Hugging Face 创建一个 `Docker` 类型的 Space。
2. 在 GitHub 仓库里添加 secret：
   - `HF_TOKEN`
3. 在 GitHub 仓库里添加 variable：
   - `HF_SPACE_REPO_ID`
   - 示例：`gdtiti/sub2api-v2`

## 工作方式

- `GHCR Image` 在 `main` 成功后会生成 `ghcr.io/<owner>/<repo>:sha-<commit-sha>`。
- `deploy-hf-space.yml` 会取这次成功构建的 commit SHA，把 `deploy/hf-space/` 渲染成一个临时目录。
- 渲染后的 `Dockerfile` 会固定引用对应的 `sha-<commit-sha>` 镜像，不会漂移到别的版本。
- 然后用官方 `huggingface/hub-sync` 把这个临时目录同步到目标 Space。

## Hugging Face Space 里要配置的 Secrets

- `DATABASE_PASSWORD`
- `REDIS_PASSWORD`
- `ADMIN_PASSWORD`
- `JWT_SECRET`

## Hugging Face Space 里要配置的 Variables

- `DATABASE_HOST`
- `DATABASE_PORT`
- `DATABASE_USER`
- `DATABASE_DBNAME`
- `DATABASE_SSLMODE`
- `REDIS_HOST`
- `REDIS_PORT`
- `REDIS_DB`
- `REDIS_ENABLE_TLS`
- `ADMIN_EMAIL`

可选：

- `RUN_MODE`
- `SERVER_MODE`
- `TIMEZONE`
- `DATA_DIR`

## 运行注意事项

- Space 模板里已经设置：
  - `AUTO_SETUP=true`
  - `SERVER_HOST=0.0.0.0`
  - `SERVER_PORT=8080`
  - `DATA_DIR=/data`
- 如果不给 Space 持久化存储，`/data` 内容在重启后不保留，应用会在每次冷启动时重新执行 auto setup。
- 要减少重复初始化，建议给 Space 开持久化存储，并保留 `DATA_DIR=/data`。
- 外部 PostgreSQL 和 Redis 需要能从 Hugging Face Space 网络环境访问到。

## 手动触发

`deploy-hf-space.yml` 支持手动触发，可以指定：

- `image_ref`
  - 例如：`main`
  - 或：`sha-dcfc4dac144bc1e39a99967254c0cceeb6f44220`
- `hf_space_repo_id`
  - 例如：`gdtiti/sub2api-v2`
