# 发布流程

本文介绍如何发布一个新版本。

---

## 一、发布前准备

确保你正在 `main` 分支，并且代码是最新的：

```bash
git checkout main
git pull origin main
```

---

## 二、决定版本号

本项目使用 [语义化版本](https://semver.org/lang/zh-CN/)：

| 版本号变化 | 含义 | 例子 |
|---|---|---|
| 主版本号 | 破坏性变更，不兼容旧版本 | `1.0.0` → `2.0.0` |
| 次版本号 | 新增功能，向下兼容 | `1.0.0` → `1.1.0` |
| 修订号 | 修复 bug，向下兼容 | `1.0.0` → `1.0.1` |

### 常见场景

- 修复了一个线上 bug → 发补丁版本：`v1.0.1`
- 增加了新功能 → 发次版本：`v1.1.0`
- 改动很大，旧用户需要修改代码才能升级 → 发主版本：`v2.0.0`

---

## 三、打标签

标签就是给某个 commit 贴一个永久记号，表示"这是某个版本"。

用 GPG 签名标签：

```bash
git tag -s v0.1.0 -m "Release v0.1.0"
```

> `-s` 表示签名。如果仓库规则要求签名标签，这一步不能省略。

---

## 四、推送标签

```bash
git push origin v0.1.0
```

推送后，GitHub 会自动触发 `release.yml` 工作流，生成 Release 页面。

---

## 五、查看 Release

打开浏览器：

```
https://github.com/Setup-carlos/hello-open-source/releases
```

你应该能看到刚发布的版本。

---

## 六、完整命令

```bash
git checkout main
git pull origin main

git tag -s v0.1.0 -m "Release v0.1.0"
git push origin v0.1.0
```

---

## 七、如果 Release 没有自动生成

可能原因：

1. `.github/workflows/release.yml` 不存在于 `main` 分支
2. 标签没有推送到 GitHub
3. GitHub Actions 运行失败

排查方法：

```bash
# 查看本地标签
git tag

# 查看远程标签
git ls-remote --tags origin

# 查看 Actions 运行状态
gh run list --workflow release.yml
```

如果 `release.yml` 不在 `main` 上，需要走 PR 流程把它合进去。

---

## 八、手动创建 Release（备用）

如果自动流程坏了，可以手动创建：

```bash
gh release create v0.1.0 --title "v0.1.0" --notes "Release notes here"
```

---

## 九、发布检查清单

- [ ] `main` 分支代码最新
- [ ] CI 全部通过
- [ ] 版本号符合语义化规则
- [ ] 标签已用 GPG 签名
- [ ] 标签已推送到 GitHub
- [ ] Release 页面已生成
- [ ] `CHANGELOG.md` 已更新
