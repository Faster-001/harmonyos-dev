---
name: harmony-next
description: HarmonyOS NEXT（API 12-23）离线文档导航 + 全链条开发流程
---

# HarmonyOS NEXT: 离线文档导航 + 全链条开发流程

## 初始化

调用技能时，先判断技能目录下是否存在 `init` 文件。若无，则进入初始化流程：

1. **必需组件**：检查 `rg` 命令是否存在：`rg --version`
   若无，提示“必须安装 rg(ripgrep) 命令才可使用此技能，请前往 https://github.com/BurntSushi/ripgrep 自行下载安装”，且在确认可用之前不得继续。
   若有，进入下一步。

2. **可选组件**：检查 `codelinter` 命令是否存在：`codelinter --version`
   若无，提示“你可以安装 HarmonyOS Command Line Tools 以启用代码检查功能，请前往 https://developer.huawei.com/consumer/cn/download/command-line-tools-for-hmos 自行下载安装”。

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

先在 `INDEX.md` 命中路径，再打开对应 `.md`。

- 先按关键词命中路径：`rg -n "UIAbility|AbilityStage|Want" references/INDEX.md`
- 查某个 `@ohos.*` 模块：`rg -n "@ohos\.app\.ability\.|@ohos\.ability\." references/JsEtsAPIReference/INDEX.md`
- 查 NDK/C API 头文件：`rg -n "^capi/headers/.*(napi|arkui|window|ability).*\.h\.md\s*$" references/JsEtsAPIReference/INDEX.md`

## 全链条开发流程

目标：由用户需求或自主发现确定优化目标，通过查阅文档获取资料，上手开发并通过代码检查。

### 确定目标

1. **用户需求**：用户输入需求或问题，如“我想实现一个登录功能”或“我遇到了一个异常”。
2. **自主发现**：用户要求自主迭代时，通过文档或代码发现新的问题或优化点，并将其作为开发目标。
3. **编排任务**：将目标编排为多个子任务，每个子任务对应一个功能模块或优化点。

### 文档查询

根据“离线文档导航”章节的指引，精确查询相关文档。这些文档将作为开发的参考资料。

### 开发实践

- 按照编排任务的顺序，依次实现每个子任务。
- 每个子任务完成后，进行代码检查 `codelinter -f json --fix .`，并及时修复 `"severity":"error"` 级别的问题。
- 重复以上步骤，直到所有子任务完成。

### 生成约束（避免踩坑）

- **不确定就查文档**：API 签名、入参、返回值以 `references/` 内文本为准，不凭经验补全。
- **ArkUI 优先声明式**：示例优先使用 `@Entry` / `@Component` / `build()`（除非文档明确是 NDK 或系统服务）。
