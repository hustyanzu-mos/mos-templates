# mos-templates

个人 MOS Hub 索引仓库。每个 `plugins/*.json` 条目对应一个插件实现仓库，
实现仓库里才有 `page/`、`functions`、`settings.json` 和 release `.deb`。

## 插件

| 插件 | 说明 | 实现仓库 |
| --- | --- | --- |
| HerdrCore | Headless terminal runtime for coding agents (`hc`) | [hustyanzu/mos-herdr-core](https://github.com/hustyanzu/mos-herdr-core) |
| Neovim | Vim-based text editor with persistent config | [hustyanzu/mos-neovim](https://github.com/hustyanzu/mos-neovim) |
| msedit | Microsoft Edit terminal editor with ICU runtime | [hustyanzu/mos-msedit](https://github.com/hustyanzu/mos-msedit) |

## 添加到 MOS

**Settings → System Configuration → MOS Hub Settings → Add Repository**

```text
https://github.com/hustyanzu/mos-templates
```

Refresh 后到 **Docker → MOS Hub → Plugins** 安装。

## 私有仓库 / GHCR

MOS 的 GitHub Token 同时用于私有 GitHub 仓库和 GHCR：

1. **Settings → Network → Token → GitHub Token** 填入 PAT。
2. 推荐 fine-grained token：
   - **Repository access:** Only select repositories（选择 `mos-templates`、`mos-herdr-core`）
   - **Permissions:** `Contents: Read`、`Metadata: Read`、`Packages: Read`
   - 如果私有索引仓库 clone 失败，改用 classic PAT（`repo` + `read:packages`）
3. MOS 会用这个 token：
   - clone 私有的 `mos-templates`
   - 通过 GitHub API 读取私有实现仓库的 release 和 `.deb`
   - 以 `ghcr.io` 认证拉取私有 Docker / Compose 镜像
4. token 加密保存在 `/boot/config/system/tokens.json`，只读使用；不要授予写权限。

> 图标和 README 链接是浏览器直接加载的 URL。如果 `mos-templates` 设为私有，
> 插件卡片图标可能显示不出来；建议索引仓库保持 public（只有元数据），
> 实现仓库 `mos-herdr-core` 保持 private。

> HerdrCore 的二进制来自公开的 `releases.herdr.dev`，不依赖 GitHub token；
> token 只用于私有的 Hub/实现仓库和私有 GHCR 镜像。

## 新增插件

1. 新建实现仓库（例如 `mos/foo`），结构参考
   [MOS Plugin Development Guide](https://docs.mos-official.net/docs/Plugins/MOS-Plugin-Development-Guide)：
   `page/`、`functions`、`settings.json`、安装脚本、Release workflow。
2. 在本仓库添加 `plugins/foo.json`：
   - `repository` 指向实现仓库
   - `icon` 指向本仓库 `images/foo.png`
   - `readme_url` 指向实现仓库 README
3. 把图标放到 `images/foo.png`。
4. 提交、推送，然后在 MOS 里 Refresh。

一个 Hub 仓库可以包含很多插件条目；每个实现仓库只负责一个插件。