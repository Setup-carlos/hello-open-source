# 新人参与指南：从零开始玩转开源项目

> 本文用大白话、生活化的例子，带你走一遍 GitHub 开源项目的标准流程。即使你从来没用过 GitHub，也能看懂、参与进来。

---

## 一、开源项目像什么？

想象一个开源项目就是一家**社区图书馆**：

- **仓库（Repository）**：图书馆本身，里面有很多书（代码文件）。
- **主分支（main）**：图书馆的正式书架，只能放审定过的书。
- **分支（Branch）**：你自己的临时书桌，你可以随便写草稿，不影响正式书架。
- **Pull Request（PR）**：你的投稿申请，意思是"我写好了一篇稿子，请你们审核后放进正式书架"。
- **合并（Merge）**：审核通过，把你的草稿正式收入书架。
- **Issue**：读者的留言板，"这本书有错字""我想看某类新书"。

所有人都能看到图书馆、提建议、参与整理。这就是开源。

---

## 二、第一次来，你需要准备什么？

### 2.1 告诉别人你是谁

Git 需要知道你的名字和邮箱，就像图书馆借书要登记。

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

### 2.2 拿到图书馆钥匙

你的电脑需要和 GitHub 建立安全连接。这个连接叫 SSH。

如果你还没钥匙，生成一把：

```bash
ssh-keygen -t ed25519 -C "你的邮箱"
```

然后把公钥（`id_ed25519.pub` 里的内容）复制到 GitHub：

> GitHub → Settings → SSH and GPG keys → New SSH key

### 2.3 安装 GitHub 命令行工具

```bash
gh auth login
```

按提示登录你的 GitHub 账号。

---

## 三、创建一个新项目

### 3.1 复制模板

我们已经准备好了一个标准模板。就像新店开业有统一装修方案。

```bash
cp -r open-source-template my-awesome-project
cd my-awesome-project
```

### 3.2 改店名

把模板里的占位信息改成你的项目信息：

- `README.md`：项目介绍
- `package.json`：项目名、描述、仓库地址
- `LICENSE`：版权信息

### 3.3 开业

```bash
git init
git checkout -b main
git add .
git commit -S -m "chore: initial commit"
```

`-S` 表示 GPG 签名，相当于给每份文件盖你的私章。

### 3.4 把图书馆开到 GitHub 上

```bash
gh repo create Setup-carlos/my-awesome-project --public --description "我的新项目"
git remote add origin git@github.com:Setup-carlos/my-awesome-project.git
git push -u origin main
```

---

## 四、给主书架加锁：分支保护

正式书架不能随便让人放书，要加几道锁。

### 4.1 打开规则页面

浏览器进入：

```
https://github.com/Setup-carlos/my-awesome-project/settings/rules
```

### 4.2 新建规则

1. 点 `New branch ruleset`
2. Name 写 `protect-main`
3. Target branches → `Add target` → `Include default branch`
4. 右上角 `⋯` → `Import ruleset`
5. 选择 `.github/ruleset-protect-main.json`
6. 点 `Create`

### 4.3 锁了什么？

- 不能直接往 `main` 推书
- 必须先交草稿（PR）
- 每份草稿要盖私章（GPG 签名）
- 自动检查要及格（CI 通过）
- 不能强拆书架（禁止 force push）
- 不能烧书（禁止删除分支）

---

## 五、一个人怎么工作？

你是馆长，但只有一个人，流程可以简化一点。

### 5.1 开始写新内容

```bash
git checkout main
git pull origin main

git checkout -b feat/add-login
```

`feat/add-login` 是你的临时书桌，名字叫"增加登录功能"。

### 5.2 命名习惯

分支名要让人一看就懂：

| 前缀 | 含义 | 例子 |
|---|---|---|
| feat | 新功能 | feat/dark-mode |
| fix | 修 bug | fix/login-error |
| docs | 改文档 | docs/readme-update |
| chore | 杂项 | chore/update-deps |
| test | 加测试 | test/auth-unit |

### 5.3 写代码并保存

```bash
# 改文件……
git add .
git commit -S -m "feat: add login page"
```

提交信息也要规范：

```text
feat: 新增登录页面
fix: 修复按钮颜色错误
docs: 更新使用说明
```

### 5.4 把草稿放到图书馆的暂存区

```bash
git push origin feat/add-login
```

### 5.5 提交审核申请（PR）

```bash
gh pr create --title "feat: add login page" --body "添加了登录页面，支持邮箱和密码登录。"
```

会返回一个链接，比如：

```
https://github.com/Setup-carlos/my-awesome-project/pull/1
```

### 5.6 自己检查并合并

因为你是唯一的人，模板设置了不需要别人审批。

打开 PR 页面：

- 看看改动对不对
- 等 CI 检查通过（变成绿色 ✓）
- 点 `Merge pull request`

### 5.7 收拾书桌

```bash
git checkout main
git pull origin main
git branch -d feat/add-login
git push origin --delete feat/add-login
```

临时书桌用完了就拆掉，保持整洁。

---

## 六、两个人怎么协作？

现在来了一个新伙伴 Alice。

### 6.1 给她办借书证

GitHub 仓库页面 → `Settings` → `Manage access` → `Invite a collaborator`

输入 Alice 的 GitHub 用户名，给 `Write` 权限。

### 6.2 Alice 加入项目

Alice 在自己电脑上：

```bash
git clone git@github.com:Setup-carlos/my-awesome-project.git
cd my-awesome-project
git checkout main
```

### 6.3 Alice 改 bug

```bash
git checkout -b fix/button-color
# 改代码
git add .
git commit -S -m "fix: correct button color"
git push origin fix/button-color
```

### 6.4 Alice 开 PR

```bash
gh pr create --title "fix: correct button color" --body "按钮在深色模式下看不清，已调整颜色。"
```

### 6.5 你审阅

你打开 PR 页面，看到 Alice 的改动。你可以：

- **Approve**：同意，可以合并
- **Comment**：写评论提问或建议
- **Request changes**：要求修改

审阅就像老师改作业，不是挑刺，是为了保证质量。

### 6.6 修改规则

多人协作时，应该要求至少 1 个人 approve 才能合并。

打开 ruleset 编辑页面：

```
https://github.com/Setup-carlos/my-awesome-project/settings/rules
```

编辑 `protect-main`，找到 `Require a pull request before merging`，把 `Required approvals` 从 `0` 改成 `1`。

---

## 七、有问题怎么提？

### 7.1 发现 bug

去仓库页面 → `Issues` → `New issue` → 选 `Bug report`

描述清楚：

- 什么问题
- 怎么复现
- 期望的结果
- 实际的结果

### 7.2 想要新功能

`Issues` → `New issue` → 选 `Feature request`

说清楚：

- 你想解决什么问题
- 希望有什么功能
- 有没有替代方案

### 7.3 直接改代码

如果你会改，不要只发 Issue，可以直接发 PR。

---

## 八、怎么发布版本？

代码稳定了，想对外发布一个版本：

```bash
git checkout main
git pull origin main

git tag -s v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

`-s` 表示签名标签，确保这个版本确实是你发的。

模板里的 `release.yml` 会自动帮你生成 Release 页面。

版本号规则（SemVer）：

- `v1.0.0`：大版本，可能不兼容
- `v1.1.0`：加新功能
- `v1.1.1`：修 bug

---

## 九、完整流程图

```text
┌─────────────┐
│  从 main 更新最新代码  │
└──────┬──────┘
       ▼
┌─────────────┐
│ 切功能分支 feat/xxx  │
└──────┬──────┘
       ▼
┌─────────────┐
│   写代码    │
└──────┬──────┘
       ▼
┌─────────────┐
│ git add + git commit -S │
└──────┬──────┘
       ▼
┌─────────────┐
│ git push origin feat/xxx │
└──────┬──────┘
       ▼
┌─────────────┐
│  在 GitHub 开 PR   │
└──────┬──────┘
       ▼
┌─────────────┐
│  CI 自动检查  │
└──────┬──────┘
       ▼
┌─────────────┐
│  审阅 / Approve  │
└──────┬──────┘
       ▼
┌─────────────┐
│  Merge 到 main  │
└──────┬──────┘
       ▼
┌─────────────┐
│  删除功能分支  │
└─────────────┘
```

---

## 十、你现在的任务清单

- [ ] 复制 `open-source-template` 创建新项目
- [ ] 改 README 和 package.json
- [ ] 初始化 Git，推送到 GitHub
- [ ] 配置分支保护 ruleset
- [ ] 练习一次完整 PR 流程
- [ ] 邀请一个朋友来协作

---

## 十一、常用命令速查表

| 想做什么 | 命令 |
|---|---|
| 查看当前状态 | `git status` |
| 查看分支 | `git branch` |
| 切换到 main | `git checkout main` |
| 更新 main | `git pull origin main` |
| 创建新分支 | `git checkout -b feat/xxx` |
| 暂存改动 | `git add .` |
| 提交改动 | `git commit -S -m "feat: xxx"` |
| 推分支 | `git push origin feat/xxx` |
| 创建 PR | `gh pr create` |
| 合并后清理 | `git branch -d feat/xxx` |
| 删除远程分支 | `git push origin --delete feat/xxx` |
| 打版本标签 | `git tag -s v1.0.0 -m "Release v1.0.0"` |
| 推标签 | `git push origin v1.0.0` |

---

## 最后

开源不复杂，就是**大家一起把东西变好**。

你不需要是大神才能参与。发现问题、改错别字、补充文档、提建议，都是贡献。

欢迎加入。
