# GitHub Pages migration for aretedriver.dev

This portfolio is already static HTML. It can be hosted by GitHub Pages without Vercel.

## Deployment

The workflow at `.github/workflows/deploy-pages.yml` publishes the site's HTML, assets, robots.txt, and sitemap.xml from `main` after it is merged. It also supports manual runs.

Current deployment state:

- GitHub Pages uses the GitHub Actions source.
- The workflow publishes from `main` and the temporary Pages URL is live at `https://aretedriver.github.io/arete-portfolio/`.
- The custom domain and DNS migration remain separate, incomplete gates.

Remaining custom-domain steps:

1. Confirm control and renewal status of `aretedriver.dev` with its domain registrar.
2. In **Settings → Pages**, set the custom domain to `aretedriver.dev` and follow GitHub's current DNS instructions. For the apex domain, GitHub documents A/AAAA or supported ALIAS/ANAME records. Remove conflicting old Vercel records. Configure `www` only if desired.
3. Once DNS resolves, turn on **Enforce HTTPS** in Pages and verify `https://aretedriver.dev/`.
4. Check `https://aretedriver.dev/robots.txt`, `/sitemap.xml`, images, and outgoing links. Add the domain to Google Search Console and submit the sitemap.

## Vercel retirement

The Vercel analytics scripts and repository configuration have been removed. The Vercel project or GitHub App connection must also be disconnected in Vercel or GitHub settings; deleting `vercel.json` alone does not stop the external commit status.

Never treat a green Pages workflow as proof the custom domain or Google indexing is operational.
