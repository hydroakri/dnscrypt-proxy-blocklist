# Browser default filter lists: evidence

Why each browser-derived source is in our lists. Evidence links are pinned to a commit. Last verified: 2026-11-09.

## Evidence

| Browser | Evidence (pinned) | What it shows |
| --- | --- | --- |
| uBlock Origin | [`assets/assets.json`@527a05c](https://github.com/gorhill/uBlock/blob/527a05c7af552a669fa2f51c29d5c7a214da7a2c/assets/assets.json) | Default-on lists. |
| Mullvad Browser | [`projects/browser/config` L93-L96 @470e878](https://gitlab.torproject.org/tpo/applications/tor-browser-build/-/blob/470e878d41e2c209809e933e6b6633b2a32b16ad/projects/browser/config#L93-L96) | Bundles uBO 1.75.0, enabled for Mullvad Browser only. No list override in the 16.0 build, so lists = [uBO 1.75.0 defaults](https://github.com/gorhill/uBlock/blob/1.75.0/assets/assets.json). Re-check after major releases. |
| LibreWolf | [`policies.json` L50-L54 @b403bfb](https://codeberg.org/librewolf/settings/src/commit/b403bfb02128/distribution/policies.json#L50-L54) | Installs plain uBO with no list config, so lists = uBO defaults. |
| Brave | [`list_catalog.json`@bc339fa](https://github.com/brave/adblock-resources/blob/bc339fabc27205097d278d0a4880a0e2cfbc278f/filter_lists/list_catalog.json) | Entries with `default_enabled: true`. Locale-enabled regional lists (`langs`) are out of scope. |
| Vanadium | [`.gclient`@99a6c82](https://github.com/GrapheneOS/Vanadium/blob/99a6c8285bf8/.gclient#L22-L224) | Lists built into the subresource filter. |
| Trivalent | [`copr_script.sh` L19-L46 @01eb66a](https://github.com/secureblue/trivalent-subresource-filter/blob/01eb66a21eb5/copr_script.sh#L19-L46) | Same as Vanadium, with extra lists. |
| Tor Browser | none | Ships no filter list. |

## Lists

`ABP/uBO`: direct download in ABP/uBO syntax. `DNS`: direct download of a domain or hosts export. `—`: none exists. Used by: M=Mullvad, L=LibreWolf, B=Brave, V=Vanadium, T=Trivalent.

### Global

| List | ABP/uBO | DNS | Used by | In ours |
| --- | --- | --- | --- | --- |
| uBlock filters (Ads, Badware, Privacy, Unbreak, Quick fixes) | [filters](https://ublockorigin.github.io/uAssets/filters/filters.txt), [badware](https://ublockorigin.github.io/uAssets/filters/badware.txt), [privacy](https://ublockorigin.github.io/uAssets/filters/privacy.txt), [unbreak](https://ublockorigin.github.io/uAssets/filters/unbreak.txt), [quick-fixes](https://ublockorigin.github.io/uAssets/filters/quick-fixes.txt) | — | M L B | uBO |
| uBlock Resource abuse | [resource-abuse.txt](https://ublockorigin.github.io/uAssets/filters/resource-abuse.txt) | — | B | uBO |
| uBlock Link shorteners | [ubo-link-shorteners.txt](https://ublockorigin.github.io/uAssets/filters/ubo-link-shorteners.txt) | — | B | uBO |
| uBlock Cookie notices | [annoyances-cookies.txt](https://ublockorigin.github.io/uAssets/filters/annoyances-cookies.txt) | — | B | uBO |
| EasyList | [easylist.txt](https://easylist.to/easylist/easylist.txt) | via [smed79 hosts](https://raw.githubusercontent.com/smed79/easylist-hosts/main/hosts) | M L B V T | big, mini, uBO |
| EasyPrivacy | [easyprivacy.txt](https://easylist.to/easylist/easyprivacy.txt) | via smed79 hosts | M L B V T | big, mini, uBO |
| EasyList AWRL | [antiadblockfilters.txt](https://easylist-downloads.adblockplus.org/antiadblockfilters.txt) | via smed79 hosts | V T | big, mini, uBO |
| Fanboy Annoyance (full) | [fanboy-annoyance.txt](https://secure.fanboy.co.nz/fanboy-annoyance.txt) | via smed79 hosts | T | big, mini, uBO |
| Fanboy Cookie (cookiemonster) | [fanboy-cookiemonster_ubo.txt](https://secure.fanboy.co.nz/fanboy-cookiemonster_ubo.txt) | — | B | uBO |
| Fanboy Mobile notifications | [fanboy-mobile-notifications.txt](https://secure.fanboy.co.nz/fanboy-mobile-notifications.txt) | — | B | uBO |
| Online Malicious URL Blocklist | [urlhaus-filter-ag-online.txt](https://curbengh.github.io/urlhaus-filter/urlhaus-filter-ag-online.txt) | [urlhaus-filter-hosts.txt](https://curbengh.github.io/malware-filter/urlhaus-filter-hosts.txt) | M L B | big, mini, uBO |
| Peter Lowe's | [adblockplus format](https://pgl.yoyo.org/adservers/serverlist.php?hostformat=adblockplus&showintro=0&mimetype=plaintext) | [hosts](https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts;showintro=0) | M L | big, mini, uBO |

### Regional (Vanadium and Trivalent)

All are in `ubo-filterlist.txt`. Lists with a DNS link are also in big and mini; `via smed79 hosts` is the converted combined hosts file above.

| List | ABP/uBO | DNS | Used by |
| --- | --- | --- | --- |
| Arabic | [liste_ar.txt](https://easylist-downloads.adblockplus.org/liste_ar.txt) | via smed79 hosts | V T |
| Bulgarian | [adblock_bg.txt](https://stanev.org/abp/adblock_bg.txt) | — | V T |
| Chinese | [easylistchina.txt](https://easylist-downloads.adblockplus.org/easylistchina.txt) | via smed79 hosts | V T |
| Czech & Slovak | [filters.txt](https://raw.githubusercontent.com/tomasko126/easylistczechandslovak/master/filters.txt) | via smed79 hosts | T |
| Dutch (EasyList) | [easylistdutch.txt](https://easylist-downloads.adblockplus.org/easylistdutch.txt) | via smed79 hosts | V |
| Dutch (AdGuard) | [filter_8_Dutch](https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_8_Dutch/filter.txt) | — | T |
| French | [liste_fr.txt](https://easylist-downloads.adblockplus.org/liste_fr.txt) | via smed79 hosts | V T |
| German | [easylistgermany.txt](https://easylist.to/easylistgermany/easylistgermany.txt) | via smed79 hosts | V T |
| Hebrew | [EasyListHebrew.txt](https://raw.githubusercontent.com/easylist/EasyListHebrew/master/EasyListHebrew.txt) | [hosts.txt](https://raw.githubusercontent.com/easylist/EasyListHebrew/master/hosts.txt) | V T |
| Indian | [indianlist.txt](https://easylist-downloads.adblockplus.org/indianlist.txt) | via smed79 hosts | V T |
| Indonesian (ABPindo) | [abpindo.txt](https://raw.githubusercontent.com/heradhis/indonesianadblockrules/master/subscriptions/abpindo.txt) | [hosts.txt](https://raw.githubusercontent.com/ABPindo/indonesianadblockrules/master/subscriptions/hosts.txt) | V T |
| Italian | [easylistitaly.txt](https://easylist-downloads.adblockplus.org/easylistitaly.txt) | via smed79 hosts | V T |
| Japanese (AdGuard) | [filter_7_Japanese](https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_7_Japanese/filter.txt) | — | T |
| Korean | [koreanlist.txt](https://easylist-downloads.adblockplus.org/koreanlist.txt) | via smed79 hosts | V T |
| Latvian | [latvian-list.txt](https://raw.githubusercontent.com/Latvian-List/adblock-latvian/master/lists/latvian-list.txt) | — | V T |
| Lithuanian | [easylistlithuania.txt](https://raw.githubusercontent.com/EasyList-Lithuania/easylist_lithuania/master/easylistlithuania.txt) | [EasyListLithuaniaHosts.txt](https://raw.githubusercontent.com/EasyList-Lithuania/easylist_lithuania/master/EasyListLithuaniaHosts.txt) | V T |
| Nordic / Norwegian | [NordicFiltersABP-Inclusion.txt](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/NorwegianExperimentalList%20alternate%20versions/NordicFiltersABP-Inclusion.txt) | [NordicHosts.txt](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/NorwegianExperimentalList%20alternate%20versions/NordicHosts.txt) | V T |
| Polish | [easylistpolish.txt](https://easylist-downloads.adblockplus.org/easylistpolish.txt) | via smed79 hosts | V T |
| Portuguese | [easylistportuguese.txt](https://easylist-downloads.adblockplus.org/easylistportuguese.txt) | via smed79 hosts | V T |
| Romanian (RoList) | [rolist.txt](https://zoso.ro/pages/rolist.txt) | — | V T |
| Russian (RuAdList) | [ruadlist.txt](https://easylist-downloads.adblockplus.org/ruadlist.txt) | via smed79 hosts | V T |
| Serbo-Croatian | [SerboCroatianList.txt](https://raw.githubusercontent.com/DandelionSprout/adfilt/refs/heads/master/SerboCroatianList.txt) | — | T |
| Spanish | [easylistspanish.txt](https://easylist-downloads.adblockplus.org/easylistspanish.txt) | via smed79 hosts | V T |
| Swedish (Frellwit) | [Frellwits-Swedish-Filter.txt](https://raw.githubusercontent.com/lassekongo83/Frellwits-filter-lists/master/Frellwits-Swedish-Filter.txt) | [Swedish-Hosts-File.txt](https://raw.githubusercontent.com/lassekongo83/Frellwits-filter-lists/master/Frellwits-Swedish-Hosts-File.txt) | T |
| Vietnamese (ABPVN) | [abpvn-IPl6HE.txt](https://abpvn.com/filter/abpvn-IPl6HE.txt) | — | V T |

### Not included: Brave's own lists

The lists in [`brave-lists/`](https://github.com/brave/adblock-lists/tree/master/brave-lists) (`brave-unbreak`, `brave-specific`, `brave-social`, `brave-android-specific`, `brave-sugarcoat`, `brave-ios-specific`, `brave-firstparty*`, `brave-cookie-specific`). They use Brave's engine syntax and have no DNS version.

### Extras (not browser defaults)

| List | ABP/uBO | DNS | In ours |
| --- | --- | --- | --- |
| AdGuard URL Tracking Protection | [privacy-removeparam.txt](https://ublockorigin.github.io/uAssets/filters/privacy-removeparam.txt) (uBO's), [filters/17.txt](https://filters.adtidy.org/extension/ublock/filters/17.txt) (AdGuard's), both subscribed | — | uBO |
| Actually Legitimate URL Shortener | [LegitimateURLShortener.txt](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/LegitimateURLShortener.txt) | — | uBO |
| AdGuard Annoyances | [filters/14.txt](https://filters.adtidy.org/extension/ublock/filters/14.txt) | — | uBO |
| Fanboy Annoyance (uBO) | [fanboy-annoyance_ubo.txt](https://secure.fanboy.co.nz/fanboy-annoyance_ubo.txt) | — | uBO |

[Privacy Guides](https://www.privacyguides.org/en/browser-extensions/) and [arkenfox](https://github.com/arkenfox/user.js/wiki/4.1-Extensions) recommend URL Tracking Protection and URL Shortener. The last two are uBO catalog lists under different names (next section).

## uBO catalog names vs our files

uBO's catalog ([`assets.json`@527a05c](https://github.com/gorhill/uBlock/blob/527a05c7af552a669fa2f51c29d5c7a214da7a2c/assets/assets.json)) names lists differently from upstream. All four below are off by default in uBO. Overlap measured 2026-11-09.

| uBO catalog name | Our entry | Overlap |
| --- | --- | --- |
| EasyList – Cookie Notices | Fanboy Cookie (cookiemonster) | identical, 28189 rules |
| EasyList – Other Annoyances (4032 rules) | Fanboy Annoyance (uBO), 54134 rules, which also includes the cookie and social parts | contains 100% of it |
| AdGuard – Cookie Notices, Popup Overlays, Mobile App Banners, Other Annoyances, Widgets | AdGuard Annoyances (`14.txt`), the combined list | identical, 59860 rules |
| AdGuard/uBO – URL Tracking Protection | uBO's own `privacy-removeparam.txt`, not AdGuard's file. We subscribe to both it and AdGuard's `17.txt`, so the result is their union | 96% overlap |

## Overlap and deduplication

Sources overlap on purpose, so each browser's defaults stay traceable. Checked on the released files, 2026-11-09.

| Output | What removes overlap | Result |
| --- | --- | --- |
| DNS (`.txt`, `.json`, `.srs`, `.rpz`) | `generate-domains-blocklist.py` removes duplicates and subdomains whose parent is also listed | 0 redundant entries in big (485365) and mini (144799) |
| `ubo-filterlist.txt` | `Deduplicate` in `ubo-sources.json` removes exact duplicate lines only | 0 duplicate lines in 440165 rules |

- uBO: rules that mean the same but differ in text are not merged. 1456 rules differ only in case or spacing and are kept, because case can matter in cosmetic selectors.
- uBO: Fanboy Cookie is contained in Fanboy Annoyance (uBO), which is contained in Fanboy Annoyance (full). The smaller two add nothing after deduplication.
