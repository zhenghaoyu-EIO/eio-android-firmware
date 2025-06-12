
# 贡献指南（Contributing Guide – 中文）

> **所有功能开发 / Bug 修复请使用 `feature/*` 分支；`main` 保持始终可编译。**

---

## 1. 同步主干

```bash
git checkout main
git pull origin main
```

---

## 2. 创建个人功能分支

```bash
git checkout -b feature/<简短主题>
# 例：feature/camera-preview
```

---

## 3. 编码并提交

```bash
git add .
git commit -m "feat: 完成 CameraX 预览初版"
```

请遵循 **Conventional Commits** 风格：  
`feat: …` / `fix: …` / `docs: …` / `refactor: …`

---

## 4. 推送并发起 Pull Request

```bash
git push -u origin feature/<简短主题>
```

在 GitHub 上创建 PR：

* **Title**：`feat: CameraX 预览初版`
* **Description**：功能点、影响范围、测试方法
* **Assignee / Reviewer**：添加 **@zhenghaoyu-EIO**

---

## 5. Code Review & Merge

1. CI 通过、无冲突、运行正常  
2. Reviewer 通过后点击 **Squash & Merge** 合并到 `main`  
3. 合并后自动删除 `feature/*` 分支（或手动删除）

---

## 6. 紧急修复流程

* 创建 `feature/hotfix-<问题描述>` 分支，同样走 PR 审核流程。  
* 合并后立即发布新的 APK / Tag。

---

> **禁止**：直接在 `main` 上开发或强推。  
> **鼓励**：小步提交，早开 PR，及时 Review。
