# pangmo5.dev

The root website lists the apps and is served by Cloudflare Workers Static
Assets. Each app has an independent Worker and GitHub Actions workflow:

| Repository | Canonical URL |
| --- | --- |
| PangMo5.github.io | https://pangmo5.dev/ |
| Tatami | https://tatami.pangmo5.dev/ |
| Amado | https://amado.pangmo5.dev/ |
| SwiftyCrow | https://swiftycrow.pangmo5.dev/ |

`wrangler.jsonc` owns the root custom domain. `_redirects` permanently sends
legacy project paths to their subdomains, preserving the rest of the path and
query string. This includes the Sparkle appcast URLs used by installed apps.

The `Deploy site` workflow copies an explicit list of public files into
`dist/`; repository configuration and credentials are never served as assets.
Configure `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` as repository or
`cloudflare` environment secrets before enabling CI deployment.

Deploy and verify all three app subdomains first. Then bind the root Worker
to `pangmo5.dev` and verify the root and all legacy redirects over HTTPS.
Preserve unrelated DNS records during cutover.

Keep `CNAME` and the GitHub Pages custom-domain setting for compatibility with
`pangmo5.github.io` links. Pages is no longer the production content origin;
it retains the GitHub-hosted redirect to `pangmo5.dev`. Do not disable Pages
without replacing and verifying that compatibility path.

For local verification, assemble `dist/` using the workflow's copy command,
then run `npx wrangler@4.131.2 dev`. Use `deploy --dry-run` to validate the
upload before changing production.
