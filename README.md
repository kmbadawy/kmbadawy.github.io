# compphys-notes

Hugo + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) site.
Math via KaTeX (set `math: true` in a post's front matter), syntax
highlighting via Chroma, search via Fuse.js (`/search/`).

## Local dev

```bash
hugo server -D
```

## New post

```bash
hugo new posts/my-post-title.md
```

## Deploy (GitHub Pages)

Site is configured for `https://kmbadawy.github.io/`.

1. Create a GitHub repo named **exactly** `kmbadawy.github.io` (this exact
   name is what makes GitHub serve it at the root, no `/repo-name/` suffix).
2. Push this folder's contents to that repo's `main` branch.
3. In repo Settings → Pages → Source, select **GitHub Actions**.
4. Push to `main`. `.github/workflows/hugo.yaml` builds and deploys
   automatically. Site goes live at `https://kmbadawy.github.io/`.

To use a custom domain instead, add a `static/CNAME` file with the domain
and update `baseURL` accordingly.

## Structure

- `content/posts/` — posts (Markdown, front matter: `title`, `date`, `tags`,
  `math`, `summary`)
- `content/about.md` — about page
- `layouts/partials/extend_head.html` — KaTeX injection, gated on
  `math: true`
- `hugo.yaml` — site config, menu, taxonomies
