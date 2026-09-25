# dsh-terminal-preset

一个给 DeepSeek Harness 用的 bundle. 装进 profile 之后会多出一个 `Terminal` agent 预设: 常规编码工具, 外加 6 个持久 PTY 终端工具.

## 为什么需要它

`@deepseek-ai/dsh-tool-terminal` 不在 Desktop 的安装闭包里 (随发行只带了 `dsh-terminal` 与 `dsh-terminal-bash` 这两块), 也没有任何随发行预设挂它. 所以需要这个 bundle 把"包依赖"和"预设声明"一起带进 profile.

## 装法

在 Harness 的插件页 (或 `plugin_manager` 的 `install_bundle`) 里填:

```text
github:azazo1/dsh-terminal-preset
```

装完开一个新会话, 在预设选择器里选 `Terminal`. 选择器本身受通用设置里的 "代码工作工具" 开关控制.

## 内容

| 文件 | 作用 |
|---|---|
| `package.json` | 声明 `dsh.bundle.patch`, 并把 `@deepseek-ai/dsh-tool-terminal` 钉在 `0.1.7-rc.2` |
| `cordis.patch.yml` | 插入 `preset-terminal` 声明, 内含 PTY 组 (服务 + 后端 + 工具) |

钉住的 `0.1.7-rc.2` 对应 dsh 0.1.7-rc.2 那一代. Harness 升级到新一代之后, 这里要跟着改并重装, 否则预设那一行会解析到旧包.

## 从本机目录安装时的两个坑

如果不用 `github:` 而用本地目录装, 有两点要注意:

- profile 的 `package.json` 会记下绝对路径 (`file:/...`), 之后每次 pnpm 操作都要求那个目录还在. 目录被清理, 那次装/卸插件就会失败.
- 目录路径默认走 `link:`, 而 pnpm 不会安装被链接包自己的依赖, `dsh-tool-terminal` 就进不了 profile. 必须写成 `file:` 前缀.
