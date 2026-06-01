# 以 VS Code 为主战场玩转 GitHub

> **核心原则**：VS Code 是操作台，GitHub 是同步与协作中心，Codex 是按需调用的执行助手——每次 diff 的最终判断权在你手里。

---

## 一、VS Code 界面解析

活动栏图标（从上到下）

| 图标 | 面板名 | 核心用途 |
|------|--------|----------|
| 📄 文件叠加图标 | **资源管理器** | 查看本地文件结构，打开文件 |
| 🔀 分支图标 | **源代码管理** | Git 核心面板：stage、commit、diff、解冲突 |
| 🖥️ 带×的电脑 | **远程资源管理器** | 管理远程仓库，无需 clone 直接浏览 |
| 🔗 节点图标 | 扩展（远程侧） | 管理运行在远程/容器内的插件 |
| 🐙 GitHub 图标 | **GitHub Pull Requests** | 在 VS Code 内看 PR、发评论、checkout 分支 |
| 🧩 拼图图标 | **扩展管理器** | 搜索、安装、禁用本地扩展 |

---

## 二、GitHub 仓库页面导航解析

顶部标签栏从左到右

| 标签 | 用途 |
|------|------|
| **`<> Code`** | 查看代码、分支、commit 历史、README；按 `.` 跳转网页版 VS Code |
| **`Issues`** | 记录 bug、功能需求；可作为 Codex 的任务来源 |
| **`Pull requests`** | review PR；**重点看 Files changed，不要只看 AI 写的描述** |
| **`Agents`** | GitHub Copilot Agent 任务管理入口，可分配 Issue 让 Agent 自动处理 |
| **`Actions`** | CI/CD 工作流；**merge 前必须确认这里是绿色** |
| **`Projects`** | 看板式任务管理，可关联 Issue 和 PR |
| **`Wiki`** | 项目文档空间，放详细说明、架构图 |
| **`Security and quality`** | 代码安全扫描、依赖漏洞告警、Secret 泄露检测 |
| **`Insights`** | 仓库数据统计：贡献者、提交频率、代码行数变化 |
| **`Settings`** | 仓库权限、分支保护规则、Webhook、Pages 等配置 |

---

## 三、每天的标准工作流

### 3.1 开始前：先拉再写

打开 VS Code → Source Control 面板（🔀）确认 Git 状态 → 点 **Sync Changes** 拉取最新代码。

### 3.2 写代码中：实时看 diff

点文件名即可查看左旧右新 diff。如果一个文件里混有多个任务的改动，可以**对单个改动块单独暂存（hunk staging）**，提交粒度更精准。

### 3.3 提交时：五项检查

在 Source Control 面板确认，缺一不可：

- [ ] 只出现本次任务相关文件，无多余改动
- [ ] 每个 diff 逐行看过：无误改、无遗留调试输出、无临时注释
- [ ] 程序能正常运行，测试或构建通过
- [ ] 无敏感信息：密码、token、`.env`、个人本地路径、大二进制文件
- [ ] commit message 格式清晰：`type: 描述`（如 `fix: 修复登录超时问题`）

检查完毕 → stage 相关文件 → **Commit** → **Sync Changes**。

### 3.4 冲突处理：不要凭感觉点按钮

冲突在 pull / merge / rebase 后触发，VS Code 给出四个操作按钮：

| 按钮 | 含义 | 建议 |
|------|------|------|
| **Accept Current** | 保留当前分支内容 | 视情况使用 |
| **Accept Incoming** | 采用合并进来的内容 | 视情况使用 |
| **Accept Both** | 两段都保留 | ⚠️ 慎用，常产生重复代码 |
| **Compare Changes** | 并排对比 | ✅ 先点这个，读懂上下文再决定 |

正确流程：先 Compare Changes → 读懂上下文 → 手动选择正确版本 → 保存文件 → Git 面板 stage → 继续 commit / merge / rebase → 验证程序正常运行。

---

## 四、什么时候才用 Codex

### 适合交给 Codex 的任务

- 跨文件批量修改
- 修报错、补测试
- 解释陌生代码
- 任何"知道要改什么、但改起来繁琐"的事

### 不适合交给 Codex 任务：
需要本地预览的前端效果、需要运行测试验证的业务逻辑、任何涉及 `--force` 或批量删除的操作——这些需要你亲手确认每一步。

---

## 五、完整联动流程图

```
GitHub 远程资源管理器（🖥️）快速浏览项目
  ↓ 决定开发 → clone 到本地
  ↓
资源管理器（📄）确认文件结构
  ↓
Source Control（🔀）确认 Git 状态 → Sync Changes 拉取最新
  ↓
从 GitHub Issues 找任务 ──→ 简单改动：直接在 VS Code 里改
                        └──→ 复杂改动：交给 Codex，改完回来逐个确认 diff
  ↓
Hunk staging 精准暂存 → 写 commit message → Commit
  ↓
Sync Changes（push 到 GitHub）
  ↓
GitHub Pull Requests 插件（🐙）发 PR / review Files changed
  ↓
GitHub Actions 插件确认 CI 绿色通过 → Merge
  ↓
Sync Changes 拉取最新 → 开始下一轮
```