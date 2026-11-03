# 更新日志

## 2026-11-03

- 创建“实验报告正文”技能，限定只处理实验步骤、结果、截图与分析。
- 将技能说明和已验证经验统一为中文。
- 将必需输入明确为原始报告、代码写入目录和代码编辑器。
- 增加目录型 Skill 与单文件自定义指令两种通用导入方式。
- 增加适用于 DSH、WorkBuddy、TARE 等 Agent 的通用安装说明。
- 增加 PowerShell 通用安装命令。
- 完成全局技能登记，来源记录为 GitHub 仓库 `wzimo519-arch/experiment-report-body`。
- 执行 `skills update experiment-report-body -g`，确认全局技能与上游一致。
- 增加 WorkBuddy 实际技能路径与 CLI `codebuddy` 目标目录不一致的说明。
- 补充 npm 官方源不可达时使用镜像，以及 `ENOTCACHED` 时调用已缓存 `skills.cmd` 的安装回退方法。
- 将 CodeBuddy 与 WorkBuddy 技能目录都改为指向 canonical 副本的目录联接，后续更新可自动同步。
