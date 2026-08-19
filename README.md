<p align="center">
  <img src="assets/logo.svg" alt="Best Effort Industries" width="620">
</p>

<p align="center">
  <strong>A diversified portfolio of regrettable technology.</strong><br>
  No warranties expressed, implied, or considered.
</p>

---

This repository contains document BEI-000, the public corporate disclosure of
Best Effort Industries (besteffortindustries.com), the holding entity for a
portfolio of software products that were built and then, in most cases, left
running.

## The Entity

Best Effort Industries is a diversified technology group in the sense that the
products have nothing to do with each other. There is no strategy connecting
them, no shared platform underneath them, and no roadmap in front of them. Each
division was incorporated because the domain was available.

The group operates on a best-effort basis. This is not a service tier we
selected from a list of service tiers. It is the only one we offer, and it is
disclosed in the name specifically so that it cannot later be characterised as
a surprise.

Key positions:

- **Legal status: none whatsoever.** Best Effort Industries is not registered
  in any jurisdiction. Correspondence sent to it arrives nowhere.
- **Headcount: one.** This is also the single point of failure. The two facts
  are not tracked separately.
- **Warranties: not expressed, not implied, not considered.** The third clause
  is the load-bearing one.
- **Complaint handling.** Complaints are closed as WORKING AS INTENDED.
  Escalations are routed to `/dev/null` and acknowledged never.
- **Uptime.** Varies by division. One division guarantees none of it and is
  therefore the only one meeting its commitments in full.

## Operating Divisions

The register in `index.html` is the only place division numbers are assigned,
and it is the source of truth for what exists. It has two schedules:

- **Schedule A** lists divisions in service. They carry real, sequential IDs,
  assigned in order of going live, and the register links each one.
- **Schedule B** lists divisions pending indefinitely, under provisional queue
  numbers (`P-01`, `P-02`, ...). A P-number is a position rather than an
  identity: it lives in this table and nowhere else, so when a division leaves
  the queue the rows below it move up. A division's position may improve
  without it getting any closer to service. Schedule B rows carry no links, so those sites are unlisted rather
  than unreachable: most are deployed and answering on `*.pages.dev`, they are
  simply not advertised here.

Nothing outside the register records a division number. Repositories do not
keep a copy, because a copy would go stale and then go wrong.

### Where a division is hosted

**A division's canonical host is its own domain if it has one, and a subdomain
of `besteffortindustries.com` otherwise.**

That single rule decides every case. A division that owns a domain uses it, and
its subdomain exists only as an alias that redirects there; a division without
one is canonical on its subdomain, at no cost and with no renewal to forget.
Buying a domain is therefore an upgrade a site earns, never the price of going
live. Because the register links a division's name rather than printing its
address, a fleet split across both kinds of host looks identical in the table.

Subdomain labels are lowercase, unhyphenated, and as short as they can be while
staying unambiguous. Where a division owns a domain, the label matches that
domain's own label, so the alias is obvious.

| Division | Canonical host | Subdomain | Pages project |
| --- | --- | --- | --- |
| Best Effort Industries | `besteffortindustries.com` | (the zone itself) | `besteffortindustries` |
| No Nines Given | `noninesgiven.com` | `noninesgiven` (alias) | `noninesgiven` |
| Certificate Authority of Vibes | `rfc58008.com` | `rfc58008` (alias) | `rfc58008` |
| Merge Conflict Counselling | `mergeconflictcounselling.com` | `mergeconflict` (alias) | `mergeconflictcounselling` |
| Cache Invalidation That Grows with Your Patience | `invalid8.dev` | `invalid8` (alias) | `invalid8` |
| Null Island Tourism Board | `visitnullisland.com` | `nullisland` (alias) | `nullisland`, served from `nullisland-dbx.pages.dev` |
| BOM Squad | `bomsquad` subdomain | `bomsquad` | `bomsquad` |
| Avian Carrier Logistics | `avian` subdomain | `avian` | `aviancarrierlogistics` |
| The Leap Second Preservation Society | `leapsecond` subdomain | `leapsecond` | `leapsecondsociety` |
| Epoch Preparedness Administration | `epoch` subdomain | `epoch` | `epochpreparedness` |
| The Missing Days Commission | `missingdays` subdomain | `missingdays` | `missingdays` |
| The Anywhere on Earth Time Authority | `aoe` subdomain | `aoe` | `aoetimeauthority` |
| Bureau of Latency Enforcement | `latency` subdomain | `latency` | `latencyenforcement` |
| Memorial for the Genes Lost to Excel | `genes` subdomain | `genes` | `geneslosttoexcel` |
| Technical Debt Collector | none yet | `techdebt`, reserved | none yet |
| Committee for the Preservation of Legacy Systems | none yet | `legacy`, reserved | none yet |

One trap worth recording: `nullisland.pages.dev` belongs to an unrelated
project, so that division's deployment answers on `nullisland-dbx.pages.dev`.
Any DNS record for it must target the `-dbx` hostname, not the project name.

### Attaching a host

In the dashboard, **Workers & Pages -> the project -> Custom domains -> Set up
a custom domain**. Because the zone lives in this account, the proxied CNAME is
created for you, and Universal SSL already covers one level of subdomain, so no
certificate work is needed. Do not create the DNS record by hand beforehand: a
pre-existing CNAME blocks the flow.

For a division that owns a domain, add a Redirect Rule so the alias does not
serve a second copy: **Rules -> Redirect Rules**, matching
`http.host eq "<label>.besteffortindustries.com"`, redirecting 301 to
`concat("https://<the domain>", http.request.uri.path)`.

## Taking a division live

The procedure is also written into an HTML comment directly above the table in
`index.html`, on the assumption that whoever needs it will be looking at the
table and not at this file.

1. Move the row from Schedule B to the bottom of Schedule A.
2. Replace its provisional `P-` number with the next sequential ID.
3. Wrap the division's `.name` text in an anchor to its canonical host. The
   name is the link; the address is not repeated beside it.
4. Change `<span class="st soon">Coming soon</span>` to
   `<span class="st live">In service</span>`.
5. Renumber the remaining Schedule B rows so the queue closes up, and update the
   counts: the Entity Summary block and both schedule labels.

Then point the site itself at its canonical host: `rel=canonical`, `og:url`,
`og:image` and `twitter:image` in that repository's `index.html`, the domain
printed in `tools/og.html`, and `make og` to regenerate the card. Every absolute
URL must name a host that resolves, or link previews break and the canonical
tag points search engines at nothing.

There is no data file, no templating layer and no build step feeding this
table. Adding one would take longer than editing the table has ever taken.

---

## Development notes

The parody ends here. The rest of this file is accurate.

### Layout

A static, zero-build, zero-dependency site. Two HTML files and a handful of
generated images. There is no framework, no bundler and no `package.json`.
Cloudflare Pages serves the repository root exactly as it appears here.

```
index.html            the site
404.html              catch-all, served automatically by Cloudflare Pages
favicon.svg           icon source of truth (64px grid)
favicon.ico           16/32/48, generated
apple-touch-icon.png  180x180, generated
og.png                1200x630 share image, generated
assets/logo.svg       wordmark, text outlined, used at the top of this README
tools/og.html         source for og.png
tools/logo-src.svg    source for assets/logo.svg, text still live
tools/favicon-16.svg  pixel-grid 16px icon, used for the smallest .ico entry
Makefile              asset regeneration only, never runs at deploy time
_headers              Cloudflare Pages header rules
robots.txt            permissive
wrangler.toml         Cloudflare Pages configuration
```

The page makes zero requests to any external domain. Type is `Courier New`
with a `monospace` fallback, so there are no webfonts to host or wait for.

### Local preview

```sh
make serve          # python3 -m http.server 8000
```

Then open `http://localhost:8000`. A local server is preferable to opening the
file directly because the asset paths are root-absolute.

### Regenerating images

Only needed when the tagline, the wordmark or the icon changes. Requires
`google-chrome`, ImageMagick 7 (`magick`) and Inkscape on the machine doing the
regenerating — **none of them is needed to deploy**, because the outputs are
committed.

```sh
make assets         # everything below
make og             # og.png     <- tools/og.html, via headless Chrome
make favicon        # favicon.ico + apple-touch-icon.png <- the SVG sources
make logo           # assets/logo.svg <- tools/logo-src.svg, text outlined
```

`make logo` outlines the wordmark's text so the README renders the same
whether or not the viewer has Courier New. Inkscape rewrites the whole file,
so the `GENERATED` comment at the top has to be pasted back afterwards.

`make og` screenshots `tools/og.html` at exactly 1200x630 and quantises the
result, which keeps the file around 13 KB. If you change the tagline in
`index.html`, change it in `tools/og.html` too and re-run `make og`; nothing
links the two automatically.

On Linux, `Courier New` resolves through fontconfig to Liberation Mono, which
is metric-compatible. The rendered `og.png` therefore matches what most
non-Apple viewers see in the browser.

### Deploying

Wrangler is configured via `wrangler.toml`, so a deploy is one command from an
authenticated shell:

```sh
make deploy         # wrangler pages deploy .
```

### Which Cloudflare account this deploys to

This machine has two Cloudflare identities, and picking the wrong one deploys
this site into an unrelated organisation.

**Pages configuration cannot pin the account.** `account_id` is a Workers-only
key; putting it in a Pages `wrangler.toml` makes Wrangler refuse to run:

```
Configuration file for Pages projects does not support "account_id"
```

So the account is selected by **an auth profile bound to this directory**,
recorded in `~/.config/.wrangler/profiles/directory-bindings.json`:

```sh
wrangler auth activate personal    # already done; re-run after moving the repo
wrangler whoami                    # must print: Active profile: personal
```

Without a binding, Wrangler falls back to the `default` profile, which here is
the other organisation — and it will deploy there without asking. **Check
`whoami` before deploying.** The binding lives outside the repo, so a fresh
clone, a moved directory, or another machine all need `wrangler auth activate`
again.

One extra trap: Wrangler caches the resolved account in the untracked
`.wrangler/cache/wrangler-account.json` inside this directory. If a deploy ever
went to the wrong account from here, activating the right profile is **not**
enough — delete `.wrangler/` as well, or the cached account ID wins and the API
call fails with `Authentication error [code: 10000]`.

For CI, where profiles do not exist, set `CLOUDFLARE_ACCOUNT_ID` (the account to
deploy into) and `CLOUDFLARE_API_TOKEN` (credentials scoped to it) as
environment variables.

The Pages project is `besteffortindustries`, production branch `main`, with no
build command and the build output directory set to `/`. If you ever recreate
it from the dashboard, use exactly those values — there is nothing to build,
and any build command entered there will only make the deployment worse.

To wire the Git integration instead, connect the `holthe/besteffortindustries`
repository under **Workers & Pages → Create → Pages → Connect to Git** with the
same settings.

### Custom domain

`besteffortindustries.com` is registered at Simply.com, so the zone has to
reach Cloudflare before the Pages custom domain will validate. Steps, in order:

1. **Add the zone to Cloudflare.** Cloudflare dashboard → **Add a site** →
   `besteffortindustries.com` → Free plan. Cloudflare returns two assigned
   nameservers, of the form `xxx.ns.cloudflare.com`.
2. **Repoint the nameservers at Simply.com.** In the Simply.com control panel,
   open the domain's DNS/nameserver settings and replace the Simply
   nameservers with the two Cloudflare ones. Propagation is usually well under
   an hour. Cloudflare emails when the zone goes active.
3. **Attach the domain to the Pages project.** Cloudflare dashboard →
   **Workers & Pages** → `besteffortindustries` → **Custom domains** → **Set up
   a custom domain** → `besteffortindustries.com`. Because the zone is now on
   Cloudflare, the required record is created automatically:

   | Type  | Name | Content                              | Proxy   |
   | ----- | ---- | ------------------------------------ | ------- |
   | CNAME | `@`  | `besteffortindustries.pages.dev`      | Proxied |

   The apex record is flattened by Cloudflare, so a CNAME at `@` is valid here
   even though it would not be at a conventional DNS host.
4. **Repeat for `www`** if both should resolve:

   | Type  | Name  | Content                            | Proxy   |
   | ----- | ----- | ---------------------------------- | ------- |
   | CNAME | `www` | `besteffortindustries.pages.dev`    | Proxied |

5. **Wait for the certificate.** Issuance normally completes within a few
   minutes of the record appearing.

Until step 1 and 2 are done, the site is reachable at
`besteffortindustries.pages.dev`.

## License

Parody. The company does not exist, which is the joke and also the legal
position.
