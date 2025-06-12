# Contributing Guide

> **All development must happen on feature branches.**  
> `main` must remain buildable at all times.

## 1 – Sync the main branch

```bash
git checkout main
git pull origin main
```

## 2 – Create your feature branch

```bash
git checkout -b feature/<brief-topic>
# e.g. feature/camera-preview
```

## 3 – Code & commit

```bash
git add .
git commit -m "feat: initial CameraX preview implementation"
```

## 4 – Push and open a Pull Request

```bash
git push -u origin feature/<brief-topic>
```

* **PR title**: `feat: CameraX preview`
* **Description**: what was added / affected / how to test
* **Reviewer**: assign **@zhenghaoyu-EIO**

## 5 – Review & merge

1. Ensure the branch *builds and runs* without errors.
2. Reviewer approves the PR.
3. Click **Squash & Merge** into `main`.
4. Delete the feature branch (GitHub will prompt).

---

### Emergency fixes

For critical issues create `feature/hotfix-<topic>` and follow the same process.

---

### Rules of thumb

* Never force‑push to `main`.
* Keep feature branches small and focused.
* Rebase onto `main` if the branch diverges for more than a few days.
