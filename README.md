# mos-templates

个人 MOS Hub 索引仓库。每个 `plugins/*.json` 条目对应一个插件实现仓库，
实现仓库里才有 `page/`、`functions`、`settings.json` 和 release `.deb`。

## 插件

| 插件 | 说明 | 实现仓库 |
| --- | --- | --- |
| HerdrCore | Headless terminal runtime for coding agents (`hc`) | [hustyanzu/mos-herdr-core](https://github.com/hustyanzu/mos-herdr-core) |

## 添加到 MOS

**Settings → System Configuration → MOS Hub Settings → Add Repository**

```text
https://github.com/hustyanzu/mos-templates
```

Refresh 后到 **Docker → MOS Hub → Plugins** 安装。

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