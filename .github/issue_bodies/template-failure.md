## 同步失败
@__OWNER__

自动 rebase 失败，存在冲突需要手动解决。

**上游新提交数:** __DIFF__
**上游 SHA:** `__UPSTREAM__`
**本地 SHA:** `__LOCAL__`

### 手动解决步骤

```bash
git remote add upstream https://github.com/iDvel/rime-ice.git
git fetch upstream main
git rebase upstream/main
# 解决冲突后
git add .
git rebase --continue
git push origin main --force
```

解决后请关闭此 issue。

---
🤖 此 issue 由 GitHub Actions 自动创建
