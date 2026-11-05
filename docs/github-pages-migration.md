# GitHub Pages migration for aretedriver.dev

This portfolio is already static HTML. It can be hosted by GitHub Pages without Vercel.

## Deployment

The workflow at `.github/workflows/deploy-pages.yml` publishes the site's HTML, assets, robots.txt, and sitemap.xml from `main` after it is merged. It also supports manual runs.

Repository administrator steps:
1. Open **Settings → Pages** for `AreteDriver/arete-portfolio`.
2. Choose **GitHub Actions** as the build/deployment source.
3. Merge the migration PR after review; monitor **Actions → Deploy portfolio to GitHub Pages**.
4. Verify the temporary Pages URL first (usually `https://aretedriver.github.io/arete-portfolio/`). The site currently uses root-relative asset paths and a canonical URL pointing to `aretedriver.dev`, so the custom-domain deployment is the authoritative final check.
5. Confirm control and renewal status of `aretedriver.dev` with its domain registrar. Do not change domain records until the Pages deployment succeeds.
6. In **Settings → Pages**, set the custom domain to `aretedriver.dev` and follow GitHub's current DNS instructions. For the apex domain, GitHub documents A/AAAA or supported ALIAS/ANAME records. Remove conflicting old Vercel records. Configure `www` only if desired.
7. Once DNS resolves, turn on **Enforce HTTPS** in Pages and verify `https://aretedriver.dev/`.
8. Check that `https://aretedriver.dev/robots.txt`, `/sitemap.xml`, images and outgoing GitHub/Substack links work. Add the domain to Google Search Console and submit the sitemap.

## Follow-up cleanup
- Remove the two Vercel Analytics and Speed Insights script references from `index.html`. They are not needed on Pages.
- Review the CSP or Vercel-specific headers: any rules from `vercel.json` will not automatically carry over to GitHub Pages.
- Update the site README after a confirmed migration.
- Never treat a green Pages workflow as proof the custom domain or Google indexing is operational.
