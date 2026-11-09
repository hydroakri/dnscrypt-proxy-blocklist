# dnscrypt-proxy blocklist

Personal DNS blocklist, rebuilt by CI and published in several formats.

## Download

Pick one row. Each row has three mirrors with identical content.

| File | Format | Used by | GitHub Release | jsDelivr CDN | Raw |
| --- | --- | --- | --- | --- | --- |
| `blocklist.txt` | plain domains | dnscrypt-proxy | [link][r-big-txt] | [link][c-big-txt] | [link][w-big-txt] |
| `blocklist.json` | rule-set source | sing-box | [link][r-big-json] | [link][c-big-json] | [link][w-big-json] |
| `blocklist.srs` | rule-set binary | sing-box | [link][r-big-srs] | [link][c-big-srs] | [link][w-big-srs] |
| `blocklist.rpz` | RPZ zone | Unbound | [link][r-big-rpz] | [link][c-big-rpz] | [link][w-big-rpz] |
| `blocklist-mini.txt` | plain domains | dnscrypt-proxy | [link][r-mini-txt] | [link][c-mini-txt] | [link][w-mini-txt] |
| `blocklist-mini.json` | rule-set source | sing-box | [link][r-mini-json] | [link][c-mini-json] | [link][w-mini-json] |
| `blocklist-mini.srs` | rule-set binary | sing-box | [link][r-mini-srs] | [link][c-mini-srs] | [link][w-mini-srs] |
| `blocklist-mini.rpz` | RPZ zone | Unbound | [link][r-mini-rpz] | [link][c-mini-rpz] | [link][w-mini-rpz] |
| `ubo-filterlist.txt` | ABP/uBO syntax | uBlock Origin | [link][r-ubo] | [link][c-ubo] | [link][w-ubo] |

Updates are published only when the generated content changes (`state.json` stores a hash per variant).

## Which variant?

| Variant | Files | Content | Pick it when |
| --- | --- | --- | --- |
| **big** | `blocklist.*` | Everything in mini, plus HaGeZi, OISD, anti-AD, Dan Pollock's and other large lists | You want maximum coverage. |
| **mini** | `blocklist-mini.*` | Default filter lists of privacy browsers (Mullvad, LibreWolf, Brave, Vanadium, Trivalent) and their regional language lists, where a DNS-level export exists. Also includes a few extra lists browsers don't enable by default | You want a smaller list with fewer false positives. |
| **uBO** | `ubo-filterlist.txt` | URL/path rules, not a domain list | You need cosmetic, cookie-notice, or URL-parameter filtering. Add it as a custom uBO subscription. |

## Usage

### dnscrypt-proxy

Point `blocked_names_file` at `blocklist.txt` (or the mini file) in your blocklist settings.

### sing-box

Use `.srs` as a remote rule-set. The `.json` file is its source form.

### Unbound

```
rpz:
  name: "dnscrypt-proxy-blocklist"
  zonefile: "/path/to/blocklist.rpz"
  url: "https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.rpz"
```

The SOA refresh interval in the file controls how often Unbound re-polls `url:`.

### uBlock Origin

Dashboard → Filter lists → Import → paste the `ubo-filterlist.txt` URL.

## Limitations

- **Time windows:** entries in `domains-time-restricted.txt` (the `@tag` syntax) work only in the `.txt` format. In `.json`, `.srs` and `.rpz` they are blocked all the time.
- **Subdomains:** a domain entry also blocks its subdomains in dnscrypt-proxy. This is broader than the exact-match semantics of the upstream hosts files.
- **Overlap:** sources overlap on purpose. DNS lists drop duplicates and redundant subdomains. `ubo-filterlist.txt` drops exact duplicate lines only, so rules with the same meaning but different text stay. Details in [`BROWSER-DEFAULTS.md`](BROWSER-DEFAULTS.md#overlap-and-deduplication).
- **uBO list:** rules inside `!#if` blocks that the compiler cannot evaluate are dropped, for example Firefox-only and HTML-filtering rules from uBlock filters. uBO already ships those lists built in, so enable them there as well.
- **Mini variant:** lists that exist only in ABP/uBO syntax are not converted to domains, because a domain block cannot express their path rules and would cause false positives. This covers Brave's own lists, URL-parameter lists, and the Bulgarian, Latvian, RoList, ABPVN, Serbo-Croatian and AdGuard Japanese/Dutch regional lists. Use `ubo-filterlist.txt` for those.

## Sources

The config files are the source of truth. Edit them, not this README. Why each browser-derived source is included, with pinned evidence, is in [`BROWSER-DEFAULTS.md`](BROWSER-DEFAULTS.md).

| Output | Config | Merged by |
| --- | --- | --- |
| big | [`domains-blocklist.conf`](domains-blocklist.conf) | [`generate-domains-blocklist.py`](https://github.com/DNSCrypt/dnscrypt-proxy/wiki/Combining-Blocklists) |
| mini | [`domains-blocklist-mini.conf`](domains-blocklist-mini.conf) | same script |
| uBO | [`ubo-sources.json`](ubo-sources.json) | [`@adguard/hostlist-compiler`](https://github.com/AdguardTeam/HostlistCompiler) |

Local lists:

- [`domains-blocklist.txt`](domains-blocklist.txt): extra blocked domains
- [`domains-allowlist.txt`](domains-allowlist.txt): domains removed from the result
- [`domains-time-restricted.txt`](domains-time-restricted.txt): time-window entries

Main upstream projects:

- **big:** [anti-AD](https://github.com/privacy-protection-tools/anti-AD), [217heidai/adblockfilters](https://github.com/217heidai/adblockfilters), [HaGeZi](https://github.com/hagezi/dns-blocklists), [OISD](https://oisd.nl/), [Peter Lowe's](https://pgl.yoyo.org/adservers/), [Dan Pollock's](https://someonewhocares.org/hosts/), [URLhaus filter](https://github.com/curbengh/urlhaus-filter), plus every source of mini
- **mini:** URLhaus, Peter Lowe's, [smed79/easylist-hosts](https://github.com/smed79/easylist-hosts), 217heidai/adblockfilters (lite), and regional hosts exports (see the config)
- **uBO:** uBlock filters, EasyList/EasyPrivacy, AdGuard Annoyances and URL Tracking Protection, Fanboy lists, Peter Lowe's, URLhaus, and the regional lists that Vanadium and Trivalent ship

## See also

- [Privacy Guides: browser extensions](https://www.privacyguides.org/en/browser-extensions/)
- [arkenfox wiki: extensions](https://github.com/arkenfox/user.js/wiki/4.1-Extensions): why you may not need many extensions

<!-- Download link references -->
[r-big-txt]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist.txt
[r-big-json]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist.json
[r-big-srs]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist.srs
[r-big-rpz]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist.rpz
[r-mini-txt]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist-mini.txt
[r-mini-json]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist-mini.json
[r-mini-srs]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist-mini.srs
[r-mini-rpz]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist-mini.rpz
[r-ubo]: https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/ubo-filterlist.txt
[c-big-txt]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.txt
[c-big-json]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.json
[c-big-srs]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.srs
[c-big-rpz]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.rpz
[c-mini-txt]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist-mini.txt
[c-mini-json]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist-mini.json
[c-mini-srs]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist-mini.srs
[c-mini-rpz]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist-mini.rpz
[c-ubo]: https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/ubo-filterlist.txt
[w-big-txt]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist.txt
[w-big-json]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist.json
[w-big-srs]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist.srs
[w-big-rpz]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist.rpz
[w-mini-txt]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist-mini.txt
[w-mini-json]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist-mini.json
[w-mini-srs]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist-mini.srs
[w-mini-rpz]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist-mini.rpz
[w-ubo]: https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/ubo-filterlist.txt
