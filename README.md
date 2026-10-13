# 野记山 · 个人博客

Hexo 8.1.2 静态博客，已上线 **https://hua2026-ai.github.io/**，由 GitHub Actions 自动部署（推送到 `main` 即上线）。

## 已发布

- [用 AI Coding Agent 像素级复刻一个网站：how2livebetter.net 实录](https://hua2026-ai.github.io/2026/10/13/ai-website-cloner-how2livebetter/) —— 基于 [ai-website-cloner-template](https://github.com/Hua2026-ai/ai-website-cloner-template) 的复刻实践与量化验证

## 本地操作

```bash
npm install
npx hexo server          # 本地预览 http://localhost:4000
npx hexo new "文章标题"   # 在 source/_posts 下生成 Markdown
```

写完后提交推送，GitHub Pages 自动重新部署：

```bash
git add -A && git commit -m "new post" && git push
```

## 将来绑定 yejishan.com

1. 在 `source/` 下新建 `CNAME`，内容只有一行：`yejishan.com`
2. 把 `_config.yml` 里的 `url:` 改成 `https://yejishan.com`
3. 去域名 DNS 添加 CNAME 记录指向 `hua2026-ai.github.io`
4. 推送，GitHub Pages 会自动签发 HTTPS 证书
