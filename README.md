# 实验报告正文 Skill

用于完成已有 Word 实验报告中的实验正文，包括实验步骤、实验结果、截图和实验分析。

## 安装

将 `experiment-report-body` 文件夹放入 Codex 技能目录：

```text
${CODEX_HOME:-~/.codex}/skills/experiment-report-body/
```

## 调用

```text
$experiment-report-body
```

## 使用前必须提供

1. 原始 Word 实验报告文件。
2. 代码写入目录。
3. 编写代码使用的编辑器，例如 VS Code 或 IntelliJ IDEA。

缺少任一项时，skill 会停止并询问，不会自行猜测。

## 修改边界

只处理用户指定的实验正文。封面、个人信息、预习内容、成绩栏、签名、批注和其他实验章节由使用者本人填写，skill 不会修改。

运行或编译环境由 agent 根据报告语言和本机环境检测，必要时按当前权限流程安装。

## 后续更新

经过验证、且会改变未来决策的经验，应追加到 `references/lessons.md`。完成更新后使用 `skill-creator` 的 `quick_validate.py` 校验。
