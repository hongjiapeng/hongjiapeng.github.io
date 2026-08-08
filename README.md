# JP Blog

这是 JP 的个人技术博客，基于 [Jekyll](https://jekyllrb.com/) 和 [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 构建，并部署到 GitHub Pages。

## 本地运行

```powershell
bundle install
bundle exec jekyll serve
```

打开 <http://localhost:4000> 查看本地站点。

新设备的完整安装和预览流程请参阅：[本地预览指南](docs/local-preview.md)。

文章位于 `_posts/`，站点导航页面位于 `_tabs/`，个人图片和历史文章图片保留在 `img/` 目录中。

## GitHub Pages 部署

仓库通过 `.github/workflows/pages-deploy.yml` 使用 GitHub Actions 构建并部署。工作流会初始化 Chirpy 的静态资源子模块，因此首次推送前请确认仓库的 Pages 构建源设置为 **GitHub Actions**。
