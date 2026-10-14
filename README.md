# Share 2026

IO Club 2026 年分享与学习记录，使用 VitePress 构建。

## 开发

需要 Node.js 24 或更高版本。

VitePress 固定使用稳定版 `1.6.4`，并提交 `package-lock.json` 以保证本地与 CI 安装一致。初始化时 `npm audit` 报告其开发工具依赖存在 3 项漏洞（2 项中危、1 项高危），暂无稳定版直接修复；后续升级时需复查。开发服务器请保持默认的本地监听方式。

```sh
npm ci
npm run docs:dev
```

```sh
npm run docs:build
npm run docs:preview
```

## 目录

- `docs/`：网站的 Markdown 内容。
- `docs/.vitepress/config.mts`：VitePress 配置。
- `.github/workflows/docs.yml`：自动构建与 Pages 部署。
- `docs/.vitepress/dist/`：构建输出，自动生成，不提交到 Git。

后续项目代码和分享材料可以继续放在仓库中。Actions 只发布构建后的 `docs/.vitepress/dist/`，不会把整个仓库作为网站发布。

## GitHub Actions

推送任意分支、向 `main` 提交 Pull Request，或手动运行工作流时，自动执行 `npm ci` 和 `npm run docs:build`。

Pages 部署默认关闭，尚未启用 Pages 时也能正常检查构建。启用后，仅 `main` 分支的推送或手动运行会部署网站；Pull Request 和其他分支只检查构建。

## 启用 GitHub Pages

1. 将代码推送到 GitHub：首次执行 `git push -u origin main`。
2. 在仓库 **Settings → Pages → Build and deployment → Source** 中选择 **GitHub Actions**。
3. 在 **Settings → Secrets and variables → Actions → Variables** 中新建仓库变量 `ENABLE_PAGES`，值设为 `true`。这是普通变量，不是 Secret。
4. 打开 **Actions → Build and deploy VitePress → Run workflow**，选择 `main` 并运行；也可以再次推送到 `main`。
5. 等待部署成功，访问 <https://io-club.github.io/share-2026/>。

该地址是预计的默认地址，部署成功前不会提供本项目的网站。组织或仓库已有自定义域名时，以 Pages 设置和部署结果显示的地址为准。

免费组织账户需要使用公开仓库才能启用 Pages；私有仓库的可用性取决于 GitHub 套餐。

## 同一仓库存放项目与网站

可以。GitHub Pages 支持直接为项目仓库发布站点，无需单独建立网站仓库，也无需将仓库命名为 `io-club.github.io`。后者仅用于组织主页。

本项目通过 Actions 发布构建产物，因此无需维护 `gh-pages` 分支，也无需将生成的 HTML 提交到 `main`。

VitePress 的 `base` 已设为 `/share-2026/`，适配默认项目站点地址。如将来改为直接绑定本仓库的自定义域名并从域名根目录提供服务，需要将 `base` 改为 `/`。

参考：[GitHub Pages 站点类型](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages)、[VitePress 部署指南](https://vitepress.dev/zh/guide/deploy)。
