# Cloudflare 一键部署 · 可复用工作流

任何 GitHub 仓库，只要引入这个工作流，就能自动部署到 Cloudflare Pages。

## 使用方法

在你的仓库中创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    uses: yuuuuub/cloudflare-deploy/.github/workflows/deploy.yml@main
    with:
      project-name: 你的项目名
    secrets:
      CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
      CLOUDFLARE_ACCOUNT_ID: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
```

可选参数：
- `deploy-dir`：部署目录（默认 `.`）
- `d1-database`：D1 数据库名（有则自动运行 `migrations/0001_init.sql`）

## 快速给所有仓库添加 Secrets

```bash
# 给单个仓库添加
gh secret set CLOUDFLARE_API_TOKEN -R yuuuuub/仓库名 -b "你的Token"
gh secret set CLOUDFLARE_ACCOUNT_ID -R yuuuuub/仓库名 -b "你的AccountID"

# 批量给所有仓库添加
for repo in $(gh repo list --json name -q '.[].name'); do
  echo "设置 $repo..."
  gh secret set CLOUDFLARE_API_TOKEN -R "yuuuuub/$repo" -b "你的Token"
  gh secret set CLOUDFLARE_ACCOUNT_ID -R "yuuuuub/$repo" -b "你的AccountID"
done
```
