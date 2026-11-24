# Remote Build Runner

手动触发 GitHub Actions，通过 PAT 跨账号/跨仓库拉取私有源仓库并执行编译、镜像构建与 Release 发布（不向本仓库提交源码，也不上传中转 Artifact）。

## 必需 Secrets

在当前仓库 **Settings → Secrets and variables → Actions** 配置：

| 名称 | 必填 | 说明 |
| --- | --- | --- |
| `TARGET_PAT` | 是 | 拥有目标私有仓库权限的 PAT（Classic PAT 勾选 `repo`、`write:packages`、`read:packages`） |
| `TARGET_REPO` | 推荐 | 默认目标仓库，格式 `owner/repo`（也可在触发时通过 `target_repo` 输入覆盖） |
| `KEYSTORE_BASE64` | 发布必填 | Release 签名证书 Base64 |
| `KEYSTORE_PASSWORD` | 发布必填 | Keystore 密码 |
| `KEY_ALIAS` | 发布必填 | 密钥别名 |
| `KEY_PASSWORD` | 发布必填 | 密钥密码 |

## 手动运行方式

进入 **Actions → Remote Build & Release → Run workflow**：

- **预览版 (`preview`)**：`ref` 保持 `master`（默认分支），自动通过目标仓库 `automation/release-state` 账本分配编号、构建并将 Release 与镜像写回目标仓库。
- **正式版 (`stable`)**：先在目标仓库合并修改 `version.properties` 的发布 PR 并推送 `vX.Y.Z` 标签；随后选择 `channel = stable`、`ref = vX.Y.Z` 触发即可。
