# qws-java-standards

版本：1.1.0。

根据《代码格式标准》封装的 Java 开发规范 Skill。适用于生成、编写、修改和重构 Java 代码，以及用户明确要求的规范检查。无需独立脚本或额外服务。

## 安装到 Codex

解压发布包，将整个 `qws-java-standards` 文件夹放在以下位置之一：

- 用户级：`~/.agents/skills/qws-java-standards/`，供该用户的多个项目使用。
- 项目级：`<项目根目录>/.agents/skills/qws-java-standards/`，可随项目 Git 仓库共享。

Windows 用户级安装路径为 `%USERPROFILE%\.agents\skills\qws-java-standards\`。最终应直接存在 `qws-java-standards/SKILL.md`，避免多套一层同名目录。同一使用范围内尽量只保留一份安装。

首次安装的 PowerShell 示例（在 ZIP 所在目录执行）：

```powershell
$skillRoot = Join-Path $env:USERPROFILE '.agents\skills'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
Expand-Archive -Path .\qws-java-standards-v1.1.0.zip -DestinationPath $skillRoot
```

已安装旧版本时，先检查并备份自己的修改，再更新文件，避免直接覆盖本地定制。若 Codex 未发现新 Skill，请重启或重新打开任务。

## 使用

默认允许 AI 根据任务描述自动选用本 Skill。为了在特定任务中明确使用，可以输入：

```text
使用 $qws-java-standards 实现这个 Java 接口，并验证改动。
```

```text
使用 $qws-java-standards 检查当前 Java 改动，只报告问题，先不要修改。
```

Skill 是提供给 AI 的工作指导，不是编译器强制约束，自动选用也不保证每次发生。如果团队要求每次 Java 任务都遵循，可在项目 AGENTS.md 中明确要求使用该 Skill；需要机械强制的规则应再配合适合项目的静态检查或 CI。

## 规范内容与依赖

`SKILL.md` 完整包含九条规则，不需要访问作者本机的原始文件。按以下四类组织，R 编号与原文条款编号一致：

| 分类 | 原文条款 | 规则摘要 |
| --- | --- | --- |
| 工具类与对象创建 | 1、2、7 | 优先使用 Objects、StringUtils、CollectionUtil 判空；常规集合使用 Guava 工厂；JSON 处理优先使用项目 JsonUtil。 |
| 代码结构与引用 | 3、6 | if 分支使用花括号；优先 import，仅在同名类冲突时使用全限定名。 |
| 数据校验与业务值 | 4、5 | 校验通常放在保存或生成阶段，保留外部数据等例外；避免魔法值，优先复用常量和枚举。 |
| 迭代与改动范围 | 8、9 | 旧功能迭代尽可能复用已有代码；非严重问题不扩大范围，不擅自重构。 |

规范中涉及 Apache Commons Lang 3、Hutool、Guava，以及项目内部的 `com.guming.api.json.JsonUtil`。安装 Skill 不会自动给 Java 项目安装这些依赖，具体使用应以项目版本和依赖管理约定为准。

JSON 处理优先选择 `JsonUtil.toJson()`、`JsonUtil.of()`、`JsonUtil.ofList()`、`JsonUtil.ofMap()`。原文未给出方法签名，Skill 要求 AI 先查实际 API，不虚构参数、泛型或依赖坐标。

规范保留原文中的推荐用法和例外，同时补充避免改变业务语义的执行边界。当前源文档已使用正确的 Hutool 类名 `cn.hutool.core.collection.CollectionUtil`。

## 分发和维护

可直接分享 ZIP，也可将此文件夹存入 Git 仓库，供他人下载或通过 Codex 的 skill-installer 安装。该包是独立 Skill，不是已上架的插件。

更新规范时同步维护 SKILL.md 和版本号，并发布新的 ZIP。公开发布前由文档权利人确定授权范围；本包未擅自指定开源许可证。

建议在目标项目试用普通 Java 功能开发、特殊集合操作、外部数据校验、JSON 转换和旧功能小范围迭代。确认既遵守规范，又保持数据语义并避免无关重构。
