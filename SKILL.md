---
name: harmony-next
description: HarmonyOS NEXT（API 12-23）离线文档导航 + 全链条开发流程
---

# HarmonyOS NEXT: 离线文档导航 + 全链条开发流程

## 初始化

调用技能时，先判断技能目录下是否存在 `init` 文件。若无，则进入初始化流程：

1. **必需组件**：检查 `rg` 命令是否存在：`rg -V`
   若无，提示“必须安装 rg(ripgrep) 命令才可使用此技能，请前往 https://github.com/BurntSushi/ripgrep 自行下载安装”，且在确认可用之前不得继续。
   若有，进入下一步。

2. **可选组件**：检查 `codelinter` 命令是否存在：`codelinter -v`
   若无，提示“你可以安装 HarmonyOS Command Line Tools 以启用自动查错功能，请前往 https://developer.huawei.com/consumer/cn/download/command-line-tools-for-hmos 自行下载安装”。

初始化结束后，在技能目录创建 `init` 文件。

## 离线文档导航

目标：在不盲读 `references/` 的前提下，快速定位到 1 个或少量目标 Markdown，然后只打开这些文件。

### 渐进式披露（按顺序走）

1. **先缩小范围（选 Kit 或任务）**
   - Kit 导航：`references/KITS.md`
   - 任务导向：`references/TASK_MAP.md`

2. **再精确命中文件（搜路径清单）**
   - 全库路径清单：`references/INDEX.md`
   - JS/ETS API 分桶清单：`references/JsEtsAPIReference/INDEX.md`

3. **仅在必要时浏览目录**
   `references/JsEtsAPIReference/` 已按桶分层：`modules/`、`topics/`、`capi/headers/`、`types/`、`errors/`、`guides/`。

### 常用检索（直接复制用）

- 先按关键词命中路径：`rg -n "UIAbility|AbilityStage|Want" references/INDEX.md`
- 查某个 `@ohos.*` 模块：`rg -n "@ohos\\.app\\.ability\\.|@ohos\\.ability\\." references/JsEtsAPIReference/INDEX.md`
- 查 NDK/C API 头文件：`rg -n "^capi/headers/.*(napi|arkui|window|ability).*\\.h\\.md$" references/JsEtsAPIReference/INDEX.md`

### 生成约束（避免踩坑）

- **不要全量读取**：先在 `INDEX.md` 命中路径，再打开对应 `.md`。
- **不确定就查文档**：API 签名、入参、返回值以 `references/` 内文本为准，不凭经验补全。
- **ArkUI 优先声明式**：示例优先使用 `@Entry` / `@Component` / `build()`（除非文档明确是 NDK 或系统服务）。

## 全链条开发流程


