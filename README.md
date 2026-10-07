# 野记山 · 个人博客

Hexo 博客，已配置好 GitHub Pages 自动部署。推送到 `main` 分支即自动上线，全免费。

## 首次上线（3 步）

1. 在 GitHub 新建一个仓库：
   - 想要 `你的用户名.github.io` 这个地址 → 仓库名就叫 `你的用户名.github.io`
   - 或者任意仓库名，地址会是 `你的用户名.github.io/仓库名`
2. 把本文件夹内容推送到该仓库的 `main` 分支：
   ```bash
   cd yejishan-blog
   git init -b main
   git add -A
   git commit -m "init blog"
   git remote add origin https://github.com/你的用户名/仓库名.git
   git push -u origin main
   ```
3. 打开仓库的 Settings → Pages → Build and deployment → Source 选择 **GitHub Actions**。

推送后约 1 分钟，博客就在 `https://你的用户名.github.io/` 上线了。

## 以后写文章

```bash
npx hexo new "文章标题"   # 在 source/_posts 下生成 Markdown
# 写完后
git add -A && git commit -m "new post" && git push
```

推送即自动重新部署，无需其他操作。

## 将来绑定 yejishan.com

1. 买下域名后，在 `source/` 下新建文件 `CNAME`，内容只有一行：`yejishan.com`
2. 把 `_config.yml` 里的 `url:` 改成 `https://yejishan.com`
3. 去域名 DNS 处添加 CNAME 记录指向 `你的用户名.github.io`
4. 推送，GitHub Pages 会自动签发 HTTPS 证书。
