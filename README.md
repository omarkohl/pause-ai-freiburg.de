# pause-ai-freiburg.de

Source of the [PauseAI Freiburg](https://pause-ai-freiburg.de) website.

Built with [Hugo](https://gohugo.io) and the
[Hextra](https://imfing.github.io/hextra/) theme. German is the
main language (served at `/`), English is served at `/en/`.

## Local development

Requirements: Hugo **extended** and Go (Hugo uses Go to download the theme
module). The versions are pinned in [`mise.toml`](mise.toml):

```sh
mise install       # or install Hugo extended and Go manually
hugo server        # http://localhost:1313, live reload
hugo server -D     # also show drafts
hugo build --gc --minify   # production build into public/
```

## Project structure

| Path               | Purpose                                         |
| ------------------ | ----------------------------------------------- |
| `hugo.toml`        | Site config, languages, menus, home page text   |
| `content/de/`      | German content                                  |
| `content/en/`      | English content                                 |
| `archetypes/`      | Templates for `hugo new content`                |
| `layouts/`         | Overrides and additions to the theme (events)   |
| `i18n/`            | Translations for strings used in `layouts/`     |
| `static/`          | Files copied as-is (images, favicon, …)         |

## Editing content

### Translations

German and English versions of a page are linked through the same
`translationKey` in the front matter. File names and URLs may differ between
languages (e.g. `content/de/termine/` and `content/en/events/`). A page without
a translation only appears in its own language.

### Events

```sh
hugo new content content/de/termine/2026-11-stammtisch.md
hugo new content content/en/events/2026-11-meetup.md
```

```yaml
---
title: "Stammtisch"
eventDate: 2026-11-14T18:00:00+01:00   # start, always with UTC offset
eventEnd: 2026-11-14T20:30:00+01:00    # optional
location: "Café XY, Freiburg"
summary: "Short text for the event list."
translationKey: "2026-11-stammtisch"
---
```

Events are split into *upcoming* and *past* at build time. The home page shows
the next three upcoming events. CI rebuilds the site daily so that past events
move to the "past" list automatically.

### Blog posts

```sh
hugo new content content/de/blog/mein-beitrag.md
```

New posts are drafts (`draft: true`). Remove that line to publish. Blog posts
with a date in the future are not published until that date (after the next
daily rebuild).

## Deployment

The [GitHub Actions workflow](.github/workflows/hugo.yaml) follows the
[official Hugo guide](https://gohugo.io/host-and-deploy/host-on-github-pages/):

- **Pull requests:** the site is built (no deploy) to catch errors.
- **Push to `main`**, daily schedule and manual run: build and deploy to
  GitHub Pages.

### One-time setup

1. GitHub repository → **Settings → Pages → Build and deployment → Source**:
   select **GitHub Actions**.
2. Settings → Pages → **Custom domain**: enter `pause-ai-freiburg.de` and save.
   No `CNAME` file is needed for Actions-based deployments.
3. At the DNS provider of `pause-ai-freiburg.de`, create:

   | Name  | Type  | Value                                     |
   | ----- | ----- | ----------------------------------------- |
   | `@`   | A     | `185.199.108.153`                         |
   | `@`   | A     | `185.199.109.153`                         |
   | `@`   | A     | `185.199.110.153`                         |
   | `@`   | A     | `185.199.111.153`                         |
   | `@`   | AAAA  | `2606:50c0:8000::153`                     |
   | `@`   | AAAA  | `2606:50c0:8001::153`                     |
   | `@`   | AAAA  | `2606:50c0:8002::153`                     |
   | `@`   | AAAA  | `2606:50c0:8003::153`                     |
   | `www` | CNAME | `<github-user-or-org>.github.io`          |

4. When the DNS check passes, enable **Enforce HTTPS**.
5. Recommended: [verify the domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)
   in the GitHub account or organization settings to prevent domain takeover.

See [GitHub's custom domain docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
for details.

### Deploy manually

Actions → **Build and deploy** → **Run workflow**.

## Updating

- **Hugo / Go:** change the versions in `mise.toml` (CI reads the Hugo version
  from there) and the `go` line in `go.mod`.
- **Theme:** `hugo mod get -u github.com/imfing/hextra && hugo mod tidy`,
  then check the site locally.
- **GitHub Actions:** Dependabot opens pull requests monthly.

## License

- **Code** (configuration, layouts, workflows, scripts): [MIT](LICENSE).
- **Content** (texts, images and other media, mainly in `content/` and
  `static/`): [CC BY-SA 4.0](LICENSE-CONTENT), unless otherwise marked.
