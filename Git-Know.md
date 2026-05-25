# How To Use Git

## 1. 三者分工

- **GitHub**：管远程仓库、PR、Issue、Actions CI/CD，是协作中心。
- **VS Code**：管本地开发、Git 面板操作、diff 确认、冲突解决；配合插件可在编辑器内完成绝大多数 Git 工作流。
- **Codex**：负责跨文件自动修改、修报错、补测试、解释陌生代码，改完后由人工在 VS Code 确认 diff 再提交。

**VS Code 活动栏图标对应功能（从上到下）：**

| 图标 | 面板 | 核心用途 |
|------|------|----------|
| 📄 资源管理器 | Explorer | 查看文件结构、打开文件 |
| 🔀 源代码管理 | Source Control | Git 面板：stage、commit、diff、stash |
| 🖥 远程资源管理器 | Remote Explorer | 管理远程连接（SSH、容器、Codespaces） |
| 📦 扩展（远程） | Extensions（远程侧） | 管理在远程/容器内运行的扩展 |
| 🐙 GitHub | GitHub Pull Requests | 在编辑器内查看 PR、Issue、checkout 分支、发表评论 |
| 🧩 扩展管理器 | Extensions | 搜索、安装、禁用本地扩展 |

---

## 2. GitHub 网页里的高效操作

- 仓库页面按 `.`：直接打开 `github.dev` 网页版 VS Code，免 clone 快速浏览或小改动。
- 仓库页面按 `?`：显示当前页面全部快捷键。
- 文件列表按 `t`：模糊搜索文件名，快速定位文件。
- 代码文件页面按 `y`：将 URL 固定到当前 commit hash，分享给他人不会因分支变动而失效。
- 任意页面按 `/`：聚焦搜索框。
- 文件页面点行号（或 `shift` 点多行）：复制精确行链接，适合指向具体问题。
- README、Issue、PR 评论均支持 Markdown；代码块用三个反引号 + 语言名，方便复制和高亮。

---

## 3. GitHub 的网页编辑选择

**适合网页直接编辑：**
- 改错别字、更新 README、调整配置小字段、快速查看项目结构。
- 临时文档改动可直接在网页 commit 或走 PR，无需本地环境。

**不适合网页编辑：**
- 大范围重构、需要安装依赖、需要运行测试或构建验证的功能、需要本地预览的前端页面。
- 凡涉及代码逻辑变更，务必让 Codex 或 VS Code 本地验证通过后再 push。

---

## 4. VS Code 里的 Git 面板使用大全

**活动栏 → 源代码管理（🔀）即 Git 面板，无需全靠命令行。**

**基本操作：**
- `Changes`：已修改但未暂存的文件列表；点文件查看左旧右新 diff。
- 文件旁 `+`：暂存整个文件；打开 diff 后可对单个改动块单独暂存，适合同一文件混有多个任务时拆分提交。
- `Staged Changes`：即将进入下次 commit 的内容。
- 提交框填写 commit message → 点 **Commit**。

**分支与同步：**
- 状态栏左下角：点击可切换分支、新建分支、查看当前分支名。
- `Publish Branch`：本地新建分支后首次推送到 GitHub。
- `Pull`：开始写代码前先拉一次，保持本地与远程同步。
- `Push`：commit 后将本地内容上传 GitHub。
- `Sync Changes`：等于 pull + push，日常同步首选。
- `Stash`：临时收起未提交改动，适合需要切分支或先拉远程时使用。

**辅助功能：**
- `Discard Changes`：丢弃本地修改，操作不可逆，点前务必确认。
- `Timeline`（资源管理器底部）：查看单个文件的历史改动记录。
- 面板右上角 `...` 菜单：包含 Pull、Push、Branch、Merge、Rebase、Stash 等进阶操作。

**插件增强（Git Graph / Git History）：**
- **Git Graph**（活动栏或命令面板 `Git Graph: View Git Graph`）：可视化分支树，直观查看 commit 历史、merge、rebase 关系，支持右键 checkout、cherry-pick、reset。
- **Git History**（右键文件 → `Git: View File History`）：按文件或按行查看完整修改历史，可对比任意两个版本。

---

## 5. VS Code 处理冲突的经验

- 冲突触发场景：`pull`、`merge`、`rebase` 后，本地与远程内容不兼容时自动标记。
- VS Code 在冲突文件顶部显示操作按钮：
  - **Accept Current**：保留当前分支内容。
  - **Accept Incoming**：采用合并进来的内容。
  - **Accept Both**：两段都保留（慎用，常产生重复代码）。
  - **Compare Changes**：并排对比，适合逻辑复杂时仔细看。
- 正确流程：先读上下文 → 手动选择正确版本 → 保存文件 → Git 面板 stage → 继续 commit / merge / rebase 流程。
- 不要凭感觉机械点按钮；每次解决后验证程序能正常运行。

---

## 6. Codex 权限和安全感

- Codex 默认在项目文件夹内工作；访问项目外文件、联网、安装依赖、写入受限目录时会弹权限确认。
- 以下操作需特别谨慎：批量删除、强制回退（`--force`）、覆盖文件、安装未知依赖。
- 让 Codex 操作 Git 前，要求它先执行 `git status`，确认不会误动无关文件。
- 工作区有未提交修改时，明确告知 Codex 不要覆盖。
- 大改动前先 commit 一个稳定版本，保留回退点。

---

## 7. Codex 和 VS Code 怎么配合

1. 用 VS Code 打开项目，掌握文件结构和当前 Git 状态（Source Control 面板）。
2. 把明确的任务描述给 Codex，负责跨文件修改、修报错、补测试、解释陌生代码。
3. Codex 改完后，回到 VS Code **Source Control 面板逐个查看 diff**，不要只看文件名。
4. diff 确认无误后，在 VS Code 里 stage → commit → sync。
5. diff 太大时：要求 Codex 拆分任务，或让它总结每个文件的改动原因。
6. 前端页面效果敏感时：让 Codex 启动本地服务并截图确认，再自己在浏览器验证，最后回 VS Code 检查代码变化。

---

## 8. GitHub 和 Codex 怎么配合

- 本地项目交给云端 Codex 或他人协作前，先 push 到 GitHub 保持同步。
- GitHub Issue 可作为任务说明；Codex 可按 Issue 编号直接定位需求并改代码。
- Codex 云端完成任务后的产物通常是 **PR**；在 GitHub 上 review → 评论 → merge。
  - 重点看 **Files changed** 标签页，不要只看 AI 生成的 PR 描述。
  - 利用 **GitHub Pull Requests 插件**（活动栏 🐙）可在 VS Code 内直接 review、评论、checkout PR 分支，无需切浏览器。
  - **GitHub Actions 插件**（活动栏命令面板）可在编辑器内查看 CI 运行状态和日志，PR merge 前确认构建通过。
- PR 过大时：要求 Codex 拆成更小的 PR，或先输出改动计划再逐步实施。
- 本地继续开发前，先从 GitHub pull / sync 最新代码。

---

## 9. 推荐联动流程

```text
GitHub 上找到仓库
  ↓ clone 到本地，或 VS Code 直接打开本地仓库
  ↓ 资源管理器（📄）确认文件结构
  ↓ Source Control（🔀）确认 Git 状态，先 Pull 拉取最新
  ↓ 把明确任务交给 Codex
  ↓ Codex 修改并运行验证
  ↓ 回到 Source Control 面板：Git Graph 看分支，逐个 diff 确认改动
  ↓ 只 stage 本次任务相关文件或代码块
  ↓ 写清 commit message → Commit
  ↓ Sync Changes / Push 到 GitHub
  ↓ GitHub Pull Requests 插件（🐙）或网页查看 PR
  ↓ GitHub Actions 插件确认 CI 通过 → merge
```

---

## 10. 提交前检查清单

- [ ] Source Control 面板里只出现本次任务相关文件，无多余改动。
- [ ] 每个 diff 逐行看过：无误改、乱码、遗留调试输出、临时注释代码。
- [ ] 程序能正常运行，测试或构建通过（可借助 GitHub Actions 插件确认 CI 状态）。
- [ ] 无敏感信息：密码、token、`.env`、个人本地路径、大二进制文件。
- [ ] commit message 简明表达本次改了什么（建议格式：`type: 描述`，如 `fix: 修复登录超时问题`）。
- [ ] commit 后已 push / sync 到 GitHub，或明确知道仍在本地分支。
- [ ] 如需他人 review：在 GitHub 或 VS Code GitHub Pull Requests 插件内发起 PR，指定 reviewer。
