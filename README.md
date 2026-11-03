# 实验报告正文通用 Skill

用于完成已有大学理工科课程实验 Word 实验报告中的实验正文，包括实验步骤、实验结果、截图和实验分析。

本仓库不依赖特定 Agent 产品。只要目标 Agent 能加载 Markdown 指令，就可以使用。

## 一键安装

如果目标 Agent 支持 GitHub Skill 安装命令，可直接执行：

```text
npx skills add wzimo519-arch/experiment-report-body
```

也可以直接对该 Agent 说：

```text
请从 GitHub 安装 experiment-report-body 这个 skill：
https://github.com/wzimo519-arch/experiment-report-body
```

这条自然语言请求只有在 Agent 具备 GitHub 访问能力和全局技能目录时才能自动完成。否则请使用下面的通用安装方式。

如果 npm 官方源不可达，可先切换到国内镜像：

```powershell
$env:npm_config_registry='https://registry.npmmirror.com'
npx --yes skills add wzimo519-arch/experiment-report-body
```

如果 `npx` 报 `ENOTCACHED`，但本机以前运行过 `skills`，可以直接调用 npm 缓存中最新的 CLI。`npx --offline skills` 按包名查找时不会复用带随机哈希的 `_npx` 缓存，因此会误报缓存不存在：

```powershell
$skillsCli = Get-ChildItem "$env:LOCALAPPDATA\npm-cache\_npx" -Recurse -Filter skills.cmd -ErrorAction SilentlyContinue |
  Sort-Object LastWriteTime -Descending |
  Select-Object -First 1
if (-not $skillsCli) { throw '未找到缓存的 skills CLI，请联网安装或手动导入。' }
& $skillsCli.FullName add wzimo519-arch/experiment-report-body
```

## 安装方式

### 目录型 Skill

如果 Agent 支持 `SKILL.md` 目录结构：

1. 打开该 Agent 的“技能”“插件”或“自定义指令”设置。
2. 在它指定的技能根目录中创建 `experiment-report-body` 文件夹。
3. 放入 `SKILL.md` 和 `references/lessons.md`。
4. 重新加载或重启 Agent，使技能列表更新。

通用安装命令（PowerShell；请把 `<目标 Agent 技能目录>` 替换为 Agent 设置中显示的实际目录）：

```powershell
$dst = '<目标 Agent 技能目录>\experiment-report-body'; New-Item -ItemType Directory -Force $dst | Out-Null; Copy-Item '.\SKILL.md' $dst -Force; Copy-Item '.\references' $dst -Recurse -Force
```

不同产品的技能根目录不同，应以 Agent 当前设置页或官方文档为准。

### 单文件自定义指令

如果 Agent 只能导入一份自定义指令：

1. 新建名为“实验报告正文”的技能或助手。
2. 导入 `SKILL.md` 的内容。
3. 若支持附加知识文件，一并导入 `references/lessons.md`。
4. 若不支持附加文件，只把与当前任务相关的经验补充到使用说明中。

## 常见 Agent

- DSH：使用软件中的技能、工具或自定义指令导入功能；支持目录时使用其设置中显示的技能目录。
- WorkBuddy：实际读取 `~/.workbuddy/skills/`；CLI 的 `codebuddy` 目标通常写入 `~/.codebuddy/skills/`，两者不是同一路径。
- TARE：使用其自定义指令或知识文件功能；按当前版本支持的方式导入。
- 其他 Agent：优先使用目录型 Skill；不支持时改用单文件自定义指令。

除已验证的 CodeBuddy 与 WorkBuddy 路径外，本项目不写死其他产品的安装路径，因为安装位置会随产品版本、安装方式和用户配置变化。

### CodeBuddy 与 WorkBuddy 自动同步

CLI 的 `codebuddy` 目标和 WorkBuddy 实际读取的目录不是同一个路径。推荐把两者都链接到 CLI 的 canonical 副本，而不是保留独立复制：

```powershell
$canonical = Join-Path $HOME '.agents\skills\experiment-report-body'
$links = @(
  (Join-Path $HOME '.codebuddy\skills\experiment-report-body'),
  (Join-Path $HOME '.workbuddy\skills\experiment-report-body')
)
foreach ($link in $links) {
  New-Item -ItemType Directory -Force (Split-Path -Parent $link) | Out-Null
  if (Test-Path $link) {
    $item = Get-Item $link -Force
    if ($item.LinkType -ne 'Junction' -or @($item.Target)[0] -ne $canonical) {
      throw "路径已存在且不是目标 Junction：$link；请先备份或移动旧副本。"
    }
    continue
  }
  New-Item -ItemType Junction -Path $link -Target $canonical
}
```

完成后，更新 CLI 的 canonical 副本即可自动同步 CodeBuddy 与 WorkBuddy：

```powershell
$env:npm_config_registry='https://registry.npmmirror.com'
npx --yes skills update experiment-report-body -g -y
```

## 调用

在支持技能选择的 Agent 中显式选择或引用 `experiment-report-body`。

在只支持对话的 Agent 中，可以先发送：

```text
请使用 experiment-report-body 规则完成接下来的实验正文任务。
```

## 使用前必须提供

1. 原始 Word 实验报告文件。
2. 代码写入目录。
3. 编写代码使用的编辑器，例如 VS Code 或 IntelliJ IDEA。

缺少任一项时，应停止并询问，不应自行猜测。

## 修改边界

只处理用户指定的实验正文。封面、个人信息、预习内容、成绩栏、签名、批注和其他实验章节由使用者本人填写。

运行或编译环境由 Agent 根据报告语言和本机环境检测，必要时按当前权限流程安装。

## 能力要求

完整闭环需要 Agent 能读取和修改 Word 文档、运行代码并检查渲染结果。若 Agent 不具备 Word 自动化或页面渲染能力，应明确说明限制，不能假装完成版式验收。

## 后续更新

经过验证、且会改变未来决策的经验，应追加到 `references/lessons.md`。完成更新后应运行目标 Agent 的 skill 校验工具；若没有官方校验器，至少检查 frontmatter、文件引用和必需输入是否完整。

## 版本记录

查看 [CHANGELOG.md](CHANGELOG.md)。
