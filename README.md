# This is dnscrypt-proxy blocklist for personal use

https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist.txt  
https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.txt

Each run publishes two variants, each in four formats. A release/CDN push
only happens when the generated domain set actually changed since the last
run (`state.json` tracks a hash per variant; nothing changed → no new
release, no CDN purge).

## Variants

- **big** (`blocklist.*`) — full list, all sources below under "DNS domains".
- **mini** (`blocklist-mini.*`) — smaller variant for setups that don't need
  the full list. Sources: [HaGeZi Pro Mini](https://github.com/hagezi/dns-blocklists),
  [OISD Small](https://oisd.nl/), [HaGeZi Threat Intelligence Feeds (mini)](https://github.com/hagezi/dns-blocklists),
  [Online Malicious URL Blocklist](https://github.com/curbengh/urlhaus-filter),
  tracker list. Target size is under 10MB, measured per CI run rather than
  hard-capped — TIF mini alone is ~170k domains, so actual size depends on
  upstream list churn.

## Formats

- `.txt` — dnscrypt-proxy plain domain list (the original format).
- `.json` / `.srs` — [sing-box](https://sing-box.sagernet.org/) rule-set
  (JSON source + compiled binary).
- `.rpz` — [Unbound](https://unbound.docs.nlnetlabs.nl/) Response Policy
  Zone. Point Unbound at the released file with something like:
  ```
  rpz:
    name: "dnscrypt-proxy-blocklist"
    zonefile: "/path/to/blocklist.rpz"
    url: "https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.rpz"
  ```
  The SOA refresh interval in the file controls how often Unbound re-polls
  the `url:` source.

**Limitation:** entries in `domains-time-restricted.txt` (the `@tag`
time-window syntax) only work in the `.txt` format, which dnscrypt-proxy
itself interprets. The `.json`/`.srs`/`.rpz` formats have no time-window
concept, so those entries are blocked unconditionally in those formats.

## Some details

Lists are merged with the [`generate-domains-blocklist.py`](https://github.com/DNSCrypt/dnscrypt-proxy/wiki/Combining-Blocklists)
script from the official dnscrypt-proxy repo, which converts common
third-party list formats (hosts files, wildcard domain lists, etc.) into
dnscrypt-proxy's format, then dedupes and merges overlapping entries.

### DNS domains

[CHN: AdRules DNS List](https://github.com/Cats-Team/AdRules)  
[CHN: anti-AD](https://github.com/privacy-protection-tools/anti-AD)  
[CHN: 217heidai/adblockfilters](https://github.com/217heidai/adblockfilters) —
we only take its merged Chinese-domain hosts output (`adblockhosts.txt` for
big, `adblockhostslite.txt` for mini), not the full project. That upstream
project itself aggregates ~20 Chinese-focused sources (AdGuard Base/Chinese/
Mobile Ads/DNS filters, EasyList/EasyList China/EasyPrivacy, AdRules DNS
List, OISD Basic, AWAvenue Ads Rule, StevenBlack hosts, Dan Pollock's hosts,
and others), dedupes them, and prunes domains that no longer resolve. It
refreshes every 8 hours upstream.  
[Online Malicious URL Blocklist](https://github.com/curbengh/urlhaus-filter)  
[HaGeZi's Pro Blocklist & Threat Intelligence Feeds](https://github.com/hagezi/dns-blocklists)  
[OISD Blocklist Big](https://oisd.nl/)  
[Peter Lowe's Blocklist](https://pgl.yoyo.org/adservers/)  
[Dan Pollock's List](https://someonewhocares.org/hosts/)

### URL rules for ABP/uBO

[uBlock filters – Ads](https://github.com/uBlockOrigin/uAssets/blob/master/filters/filters.txt),
[Badware risks](https://github.com/uBlockOrigin/uAssets/blob/master/filters/badware.txt),
[Privacy](https://github.com/uBlockOrigin/uAssets/blob/master/filters/privacy.txt),
[Quick fixes](https://github.com/uBlockOrigin/uAssets/blob/master/filters/quick-fixes.txt),
[Unbreak](https://github.com/uBlockOrigin/uAssets/blob/master/filters/unbreak.txt) —
the five lists Mullvad Browser enables by default.  
[EasyList](https://easylist.to/easylist/easylist.txt),
[EasyPrivacy](https://easylist.to/easylist/easyprivacy.txt),
[EasyList Cookie List](https://secure.fanboy.co.nz/fanboy-cookiemonster.txt),
[Fanboy's Annoyance List](https://secure.fanboy.co.nz/fanboy-annoyance.txt)  
[EasyList Adblock Warning Removal List (AWRL)](https://github.com/easylist/antiadblockfilters) —
one of GrapheneOS Vanadium's three built-in primary filters.  
[AdGuard Annoyances](https://github.com/AdguardTeam/AdguardFilters/tree/master/AnnoyancesFilter)  
[➗ Actually Legitimate URL Shortener Tool](https://github.com/DandelionSprout/adfilt/discussions/163)  
[AdGuard URL Tracking Protection](https://github.com/AdguardTeam/AdguardFilters/raw/refs/heads/master/TrackParamFilter/sections/specific.txt)  
[Online Malicious URL Blocklist (uBO format)](https://malware-filter.gitlab.io/urlhaus-filter/urlhaus-filter-ag-online.txt) —
same project as the DNS-level source above, but matches at the full-URL/path level,
enabled by default in Mullvad Browser's uBO.

### Extended Reading:

[PrivacyGuide](https://www.privacyguides.org/en/browser-extensions/) recommends to use `AdGuard URL Tracking Protection` and [Actually Legitimate URL Shortener Tool](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/LegitimateURLShortener.txt) and [uBLockOrigin wiki](https://github.com/gorhill/uBlock/wiki/Reference-description-of-uBO-in-various-extensions-stores) mentioned they contained various list.

### And..

Read the [arkenfox wiki](https://github.com/arkenfox/user.js/wiki/4.1-Extensions) to get to know why you don't need so many extensions.
