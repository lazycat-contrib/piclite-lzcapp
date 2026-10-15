# piclite-lzcapp

[PicLite 图轻](https://github.com/amiaoapp/PicLite) 的 LazyCat LPK 打包 —— 隐私优先的图片/GIF 压缩工具（Next.js standalone 容器）。

## 形态

- 单容器镜像：`ghcr.io/amiaoapp/piclite`，端口 3456，无密码体系、无持久化数据
- 镜像交付：mirror 模式（`ghcr.1ms.run/amiaoapp/piclite:{tag}`），国内拉取加速
- 版本发现：GHCR 严格 SemVer tag（`^\d+\.\d+\.\d+$`），排除 `X.Y` 别名与 `latest`
- 无需免密登录（应用本身无登录）

## 结构

- `package.yml` — LPK v2 包元数据（`community.lazycat.app.piclite`）
- `lzc-manifest.yml` — 运行结构：单服务 + upstreams + 源自上游 compose 的 healthcheck
- `lzc-build.yml` — 构建配置（无 buildscript，镜像引用）
- `.github/lazycat-action.yml` — ca-x/lazycat-github-action 配置（镜像版本源，mirror 交付，仅私有商店）
- `.github/workflows/lazycat.yml` — 每日自动跟版 + 发布喵喵商店

## 发布

推送到 `lazycat-contrib/piclite-lzcapp` 后，GitHub Action 自动：

1. 检查 GHCR 上游最新 SemVer tag
2. 校验加速镜像 digest 与源一致（`require_digest_match: true`）
3. 更新 manifest 镜像 tag 并构建 LPK
4. 产出 `community.lazycat.app.piclite-v<version>.lpk` Release 资产
5. 发布到喵喵商店（`APPSTORE_URL` / `APPSTORE_TOKEN`，可选 `APP_ID`、`PRIVATE_STORE_GROUP_CODES`）
