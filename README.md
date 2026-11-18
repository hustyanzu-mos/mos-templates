# mos-templates

个人 MOS Hub 索引仓库。每个 `plugins/*.json` 条目对应一个插件实现仓库，
实现仓库里才有 `page/`、`functions`、`settings.json` 和 release `.deb`。

## 插件

| 插件 | 说明 | 实现仓库 |
| --- | --- | --- |
| HerdrCore | Headless terminal runtime for coding agents (`hc`) | [hustyanzu-mos/mos-herdr-core](https://github.com/hustyanzu-mos/mos-herdr-core) |
| Neovim | Vim-based text editor with persistent config | [hustyanzu-mos/mos-neovim](https://github.com/hustyanzu-mos/mos-neovim) |
| MSEdit | Simple terminal text editor inspired by the MS-DOS Editor | [hustyanzu-mos/mos-msedit](https://github.com/hustyanzu-mos/mos-msedit) |
| Yazi | Blazing fast terminal file manager with previews | [hustyanzu-mos/mos-yazi](https://github.com/hustyanzu-mos/mos-yazi) |
| OpenCode v2 | OpenCode v2 with managed runtime; self-update off, server off by default | [hustyanzu-mos/mos-opencode-v2](https://github.com/hustyanzu-mos/mos-opencode-v2) |

## 添加到 MOS

**Settings → System Configuration → MOS Hub Settings → Add Repository**

```text
https://github.com/hustyanzu-mos/mos-templates
```

Refresh 后到 **Docker → MOS Hub → Plugins** 安装。

## 私有仓库 / GHCR

MOS 的 GitHub Token 同时用于私有 GitHub 仓库和 GHCR：

1. **Settings → Network → Token → GitHub Token** 填入 PAT。
2. 只访问私有 **插件仓库**（不用私有 GHCR）时，推荐 fine-grained token：
   - **Repository access:** Only select repositories
     - `mos-templates`
     - `mos-herdr-core`
     - `mos-neovim`
     - `mos-msedit`
     - `mos-yazi`
     - `mos-opencode-v2`
   - **Permissions:**

     | Permission | Access | 用途 |
     | --- | --- | --- |
     | `Contents` | Read-only | clone 私有仓库、读取 release、下载 `.deb` |
     | `Metadata` | Read-only | 必需（fine-grained PAT 自动包含） |

   - 不需要 `Packages`；GitHub Container Registry 目前只支持 classic PAT，
     fine-grained 的 UI 里可能没有这个权限，找不到是正常的。

3. 需要私有 **GHCR** 镜像时，用 classic PAT：
   - scope: `repo` + `read:packages`
   - 如果 package 在组织下，可能还需要 `read:org` 和 SSO 授权
   - 如果 package 链接到私有仓库，`repo` 是必须的（GitHub 的默认行为）

4. MOS 只有一个 GitHub Token 字段。如果既要私有插件仓库又要私有 GHCR，
   直接用同一个 classic PAT（`repo` + `read:packages`）最简单；`repo` 权限较宽，
   只用私有仓库时优先 fine-grained。

5. 如果私有索引仓库 clone 失败（fine-grained 的 clone URL 兼容性问题），
   也改用 classic PAT（`repo` + `read:packages`）。
6. MOS 会用这个 token：
   - clone 私有的 `mos-templates`
   - 通过 GitHub API 读取私有实现仓库的 release 和 `.deb`
   - 以 `ghcr.io` 认证拉取私有 Docker / Compose 镜像
7. token 加密保存在 `/boot/config/system/tokens.json`，只读使用；不要授予写权限。

> 图标和 README 链接是浏览器直接加载的 URL。如果 `mos-templates` 设为私有，
> 插件卡片图标可能显示不出来；建议索引仓库保持 public（只有元数据），
> 实现仓库 `mos-herdr-core` 保持 private。

> HerdrCore 的二进制来自公开的 `releases.herdr.dev`，不依赖 GitHub token；
> token 只用于私有的 Hub/实现仓库和私有 GHCR 镜像。

## 新增插件

1. 新建实现仓库（例如 `mos-mytool`），结构参考
   [MOS Plugin Development Guide](https://docs.mos-official.net/docs/Plugins/MOS-Plugin-Development-Guide)：
   `page/`、`functions`、`settings.json`、安装脚本、Release workflow。
2. 在本仓库添加 `plugins/mytool.json`：
   - `repository` 指向实现仓库
   - `icon` 指向本仓库 `images/mytool.png`
   - `readme_url` 指向实现仓库 README
3. 把图标放到 `images/mytool.png`。
4. 提交、推送，然后在 MOS 里 Refresh。

一个 Hub 仓库可以包含很多插件条目；每个实现仓库只负责一个插件。