# This is dnscrypt-proxy blocklist for personal use

https://github.com/hydroakri/dnscrypt-proxy-blocklist/releases/latest/download/blocklist.txt  
https://cdn.jsdelivr.net/gh/hydroakri/dnscrypt-proxy-blocklist@release/blocklist.txt
https://raw.githubusercontent.com/hydroakri/dnscrypt-proxy-blocklist/release/blocklist.txt

Each run publishes two DNS-domain variants (each in four formats) plus one
ABP/uBO filter list. A release/CDN push only happens when the generated set
actually changed since the last run (`state.json` tracks a hash per variant;
nothing changed → no new release, no CDN purge).

## Variants

- **big** (`blocklist.*`) — full list, all sources below under "DNS domains".
- **mini** (`blocklist-mini.*`) — smaller variant for setups that don't need
  the full list. Mirrors the default-enabled filter lists of privacy browsers
  (Mullvad, LibreWolf, Brave, Vanadium, Trivalent) as far as a DNS-level
  equivalent exists. Sources: [Online Malicious URL Blocklist](https://github.com/curbengh/urlhaus-filter),
  [Phishing URL Blocklist](https://github.com/curbengh/phishing-filter),
  [Peter Lowe's](https://pgl.yoyo.org/adservers/),
  [smed79/easylist-hosts](https://github.com/smed79/easylist-hosts)
  (derived from EasyList, EasyPrivacy, Fanboy Annoyance and regional lists),
  217heidai/adblockfilters (CN-focused lite aggregate, ~8.5k domains), plus hosts exports of
  [ABPindo](https://github.com/ABPindo/indonesianadblockrules),
  [EasyList Hebrew](https://github.com/easylist/EasyListHebrew),
  [EasyList Lithuania](https://github.com/EasyList-Lithuania/easylist_lithuania),
  [Frellwit's Swedish](https://github.com/lassekongo83/Frellwits-filter-lists) and
  [Dandelion Sprout's Nordic](https://github.com/DandelionSprout/adfilt)
  filters. Lists that exist only in ABP/uBO syntax (uBlock filters, cookie
  lists, Fanboy mobile, Brave's own lists, URL-parameter lists, and the
  Bulgarian, Latvian, RoList, ABPVN, Serbo-Croatian, AdGuard Japanese/Dutch
  regional lists) are deliberately not converted: their rules depend on paths,
  options and exceptions that a domain block cannot express, so converting them
  would add false positives. Use `ubo-filterlist.txt` for those. About 160k
  domains (roughly 45% smaller than the previous HaGeZi-based mini); actual size
  depends on upstream churn. Each source in `domains-blocklist-mini.conf` is
  annotated with the browsers that enable it. smed79 also includes lists
  browsers do not enable by default (Turkish, Persian, Spam404, ABP
  Dutch/Japanese). Domain entries also block their subdomains in dnscrypt-proxy,
  which is broader than the exact-match hosts semantics of the upstream files.
- **`ubo-filterlist.txt`** — ABP/uBlock Origin syntax, not a domain list. Add
  as a custom uBO subscription. Merged from
  ["URL rules for ABP/uBO"](#url-rules-for-abpubo) via
  [`ubo-sources.json`](ubo-sources.json).

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
[Phishing URL Blocklist](https://github.com/curbengh/phishing-filter), [smed79/easylist-hosts](https://github.com/smed79/easylist-hosts), and regional hosts lists (mini only; see Variants)  
[Dan Pollock's List](https://someonewhocares.org/hosts/)

### URL rules for ABP/uBO

These sources feed [`ubo-sources.json`](ubo-sources.json), merged by CI into
`ubo-filterlist.txt`; edit both together. Tor ships no default filter list;
Brave's baseline (EasyList/EasyPrivacy) is already covered below, so neither
gets its own entry.

#### uBlock Origin

[uBlock filters](https://github.com/uBlockOrigin/uAssets)
([Ads](https://ublockorigin.github.io/uAssets/filters/filters.txt),
[Badware risks](https://ublockorigin.github.io/uAssets/filters/badware.txt),
[Privacy](https://ublockorigin.github.io/uAssets/filters/privacy.txt),
[Quick fixes](https://ublockorigin.github.io/uAssets/filters/quick-fixes.txt),
[Unbreak](https://ublockorigin.github.io/uAssets/filters/unbreak.txt)) —
the five lists Mullvad Browser enables by default.

#### EasyList

[EasyList & EasyPrivacy](https://easylist.to/)
([EasyList](https://easylist.to/easylist/easylist.txt),
[EasyPrivacy](https://easylist.to/easylist/easyprivacy.txt))  
[EasyList Adblock Warning Removal List (AWRL)](https://github.com/easylist/antiadblockfilters)
([antiadblockfilters.txt](https://easylist-downloads.adblockplus.org/antiadblockfilters.txt)) —
one of GrapheneOS Vanadium's three built-in primary filters.

#### AdGuard

[AdGuard Annoyances](https://github.com/AdguardTeam/AdguardFilters/tree/master/AnnoyancesFilter)
([filters/14.txt](https://filters.adtidy.org/extension/ublock/filters/14.txt))  
[AdGuard URL Tracking Protection](https://github.com/AdguardTeam/AdguardFilters/tree/master/TrackParamFilter)
([filters/17.txt](https://filters.adtidy.org/extension/ublock/filters/17.txt))

#### Fanboy

[EasyList Cookie List](https://fanboy.co.nz/) (uBO catalog name: "EasyList – Cookie Notices")
& [Fanboy's Annoyance List](https://fanboy.co.nz/)
([Cookie List](https://secure.fanboy.co.nz/fanboy-cookiemonster_ubo.txt),
[Annoyance List](https://secure.fanboy.co.nz/fanboy-annoyance_ubo.txt)) —
hosted by Fanboy, not EasyList, despite the "EasyList Cookie List" name.

#### Independent

[➗ Actually Legitimate URL Shortener Tool](https://github.com/DandelionSprout/adfilt/discussions/163)
([LegitimateURLShortener.txt](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/LegitimateURLShortener.txt))  
[Online Malicious URL Blocklist (uBO format)](https://github.com/curbengh/urlhaus-filter)
([urlhaus-filter-ag-online.txt](https://urlhaus-filter.pages.dev/urlhaus-filter-ag-online.txt)) —
same project as the DNS-level source above, but matches at the full-URL/path level,
enabled by default in Mullvad Browser's uBO.

#### GrapheneOS Vanadium

Baked into Chromium's subresource filter at build time (not a subscribable
list), per [`.gclient`](https://github.com/GrapheneOS/Vanadium/blob/main/.gclient):
[Arabic](https://easylist-downloads.adblockplus.org/liste_ar.txt),
[Bulgarian](https://stanev.org/abp/adblock_bg.txt),
[Spanish](https://easylist-downloads.adblockplus.org/easylistspanish.txt),
[French](https://easylist-downloads.adblockplus.org/liste_fr.txt),
[German](https://easylist.to/easylistgermany/easylistgermany.txt),
[Hebrew](https://raw.githubusercontent.com/easylist/EasyListHebrew/master/EasyListHebrew.txt),
[Indian](https://easylist-downloads.adblockplus.org/indianlist.txt),
[Indonesian](https://raw.githubusercontent.com/heradhis/indonesianadblockrules/master/subscriptions/abpindo.txt),
[Italian](https://easylist-downloads.adblockplus.org/easylistitaly.txt),
[Korean](https://easylist-downloads.adblockplus.org/koreanlist.txt),
[Lithuanian](https://raw.githubusercontent.com/EasyList-Lithuania/easylist_lithuania/master/easylistlithuania.txt),
[Latvian](https://raw.githubusercontent.com/Latvian-List/adblock-latvian/master/lists/latvian-list.txt),
[Dutch](https://easylist-downloads.adblockplus.org/easylistdutch.txt),
[Norwegian/Nordic](<https://raw.githubusercontent.com/DandelionSprout/adfilt/master/NorwegianExperimentalList%20alternate%20versions/NordicFiltersABP-Inclusion.txt>),
[Polish](https://easylist-downloads.adblockplus.org/easylistpolish.txt),
[Portuguese](https://easylist-downloads.adblockplus.org/easylistportuguese.txt),
[Romanian](https://zoso.ro/pages/rolist.txt),
[Russian](https://easylist-downloads.adblockplus.org/ruadlist.txt),
[Vietnamese](https://abpvn.com/filter/abpvn-IPl6HE.txt),
[Chinese](https://easylist-downloads.adblockplus.org/easylistchina.txt) —
all of the above are also used by Trivalent (below) except where it uses a
different source for the same language.

#### secureblue Trivalent

Same setup as Vanadium, per
[`copr_script.sh`](https://github.com/secureblue/trivalent-subresource-filter/blob/live/copr_script.sh).
Shares Vanadium's list above, plus:
[Fanboy Annoyance List, full version](https://secure.fanboy.co.nz/fanboy-annoyance.txt)
(vs. the trimmed uBO version already listed under Fanboy above),
[Czech & Slovak](https://raw.githubusercontent.com/tomasko126/easylistczechandslovak/master/filters.txt),
[Dutch (AdGuard)](https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_8_Dutch/filter.txt)
(Trivalent uses this instead of Vanadium's EasyList Dutch above),
[Serbo-Croatian](https://raw.githubusercontent.com/DandelionSprout/adfilt/refs/heads/master/SerboCroatianList.txt),
[Swedish](https://raw.githubusercontent.com/lassekongo83/Frellwits-filter-lists/master/Frellwits-Swedish-Filter.txt),
[Japanese (AdGuard)](https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_7_Japanese/filter.txt).

### Extended Reading:

[PrivacyGuide](https://www.privacyguides.org/en/browser-extensions/) recommends to use `AdGuard URL Tracking Protection` and [Actually Legitimate URL Shortener Tool](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/LegitimateURLShortener.txt) and [uBLockOrigin wiki](https://github.com/gorhill/uBlock/wiki/Reference-description-of-uBO-in-various-extensions-stores) mentioned they contained various list.

### And..

Read the [arkenfox wiki](https://github.com/arkenfox/user.js/wiki/4.1-Extensions) to get to know why you don't need so many extensions.
