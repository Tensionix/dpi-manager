# Audion DPI Manager — user guide

**Contents**

- [Starting](#starting)
- [How the window is built](#how-the-window-is-built)
- [First start](#first-start)
- [ZAPRET](#zapret)
- [CONFIGS](#configs)
- [TUNE](#tune)
- [DNS](#dns)
- [LISTS](#lists)
- [JOURNAL](#journal)
- [DIAGNOSTICS](#diagnostics)
- [ZAPRET 2](#zapret-2)
- [BYEDPI](#byedpi)
- [Command line](#command-line)
- [What the program changes on the machine](#what-the-program-changes-on-the-machine)
- [Where things are](#where-things-are)
- [Licence and trademark](#licence-and-trademark)
- [If something is wrong](#if-something-is-wrong)

Audion DPI Manager is one window for the programs that get round traffic blocking. The main one is **Zapret** (the [Flowseal/zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube) package). Beside it: **zapret2** ([bol-van](https://github.com/bol-van/zapret2)) and **ByeDPI** ([romanvht/ByeDPIManager](https://github.com/romanvht/ByeDPIManager)).

The window works with the same files, services and registry entries as Flowseal's bat files. The `service.bat` menu and this window can be used alternately: they always agree.

## Starting

Start **Start.exe**. Windows asks for permission (UAC): without administrator rights the WinDivert driver cannot be loaded, a service cannot be installed, and DNS and the hosts file cannot be changed. If you decline, nothing starts and nothing changes. The prompt comes at every start: through Start.exe, and when `App\AudionDpiManager.exe` is opened directly. If the program still ends up without rights (started outside the usual way and the prompt was impossible), the status line says so, and the bypass, the service and DNS will not work.

The language is taken from the system at the first start (Russian on a Russian Windows, English on any other); after **RU | EN** is pressed the choice is remembered. **RU | EN** at the top right switches the language, the moon and sun buttons the theme, **a | A | A+** the font size.

## How the window is built

**The top row** — one row of three dark pieces: the name of the program and its version on the left, the DPI tool switches in the middle, the language, the theme and the font on the right. The height of the window is the scarcest thing the pages have, so what used to be two rows (the title, then a band of switches) is one.

The three big switches in the middle — ZAPRET, ZAPRET 2, BYEDPI — are not settings but a full switch of the DPI tool under the window: each has its own files, its own connection and its own meaning of the UPDATE and START buttons below. The colour of the line under the header shows which one is chosen (Zapret — blue, zapret2 — amber, ByeDPI — green). The indication is on the buttons themselves: under the name a light and the version ("what is installed": green — the version is the latest, amber — not installed or GitHub has a newer one, "1.10.3 → 1.10.4", gray — GitHub did not answer), and a green triangle by the name means "working now". All three may run at once, but the two interception bypasses (Zapret and zapret2) get in each other's way, so the program will not start the second one.

**The bottom bar** — permanent buttons. On the left the tools: CONFIGS, TUNE, DNS, LISTS, JOURNAL, DIAGNOSTICS, ABOUT. Press one and the tool opens in place of the application page; press it again (or Esc) to go back. On the right **UPDATE | START** act on the application chosen on top. A button with nothing to do is gray: UPDATE is gray when there is nothing to update, START when the application is not installed. While the application runs, START turns into STOP.

**The status line** above the buttons shows the last message (for 14 seconds) and, during a long operation, its name, a progress bar and STOP. JOURNAL (Ctrl+J) and F1 DOCS are beside it.

**The journal** records everything the program does and the output of winws.exe. One file per day in `._runtime\logs`.

## First start

1. Choose **ZAPRET** on top. If the package is missing it says "not installed" — press **UPDATE**: the program downloads the latest Flowseal release and installs it into the `Tools\zapret-discord-youtube` folder next to itself (an installed package in the old place, `zapret-discord-youtube` at the root, keeps working).
2. In the **STRATEGY** card pick a strategy. If you do not know which, `general (FAKE TLS AUTO)` is usually a good start.
3. Press **START**. The bypass runs: the "Bypass" line turns green.
4. Does not work? Open **TUNE**, the mode **READY**, press **START**: the program checks all strategies and shows which ones open more sites.
5. A working strategy can be installed as a Windows service with **AS SERVICE** — then the bypass starts with the system.

Also useful: turn on a secure DNS in the **DNS** section — without it the provider substitutes the addresses of blocked sites and no bypass helps.

## ZAPRET

**STATE.** The Zapret folder (another can be chosen), the version on disk and on GitHub, what runs and whether a service exists. **STOP** stops winws.exe (and the service if it runs); **REMOVE SERVICE** removes the service and the WinDivert driver completely.

The update (UPDATE) works like the old Audion Zapret Updater: "the folder is there — update it, it is not — install". The archive is downloaded while Zapret still runs (without the driver GitHub may not open); then the service, winws.exe and the driver are removed; your `lists\*-user.txt` lists, the game filter setting, the IPSet mode and the update-check flag are carried into the new version; the old files are cleared and the new ones put in place. If the driver will not unload, the replacement is prepared for the next Windows boot. The names in the archive are checked before unpacking: a path that leads out ("..", a drive, a network path), a device name or an NTFS stream rejects the whole archive.

**STRATEGY.** Switch buttons, one per `general*.bat` file. Under them: **RUN** (winws.exe with all the lists, without a console window), **AS SERVICE** (autostart), **TO THE CONFIGURATION FIELD** (the parameters go to the CONFIGS section), **COMMAND** (the full command line, can be copied).

**FILTERS.** The same as the items of `service.bat`: Game Filter (OFF, TCP + UDP, TCP, UDP and the ports — in versions from 1.10), IPSet (NONE, LIST, ALL), the update check in the bat files. The changes take effect after the bypass is restarted — a **RESTART THE BYPASS** button appears beside them.

## CONFIGS

**CONFIGURATION FIELD.** The parameters of winws.exe as text, a profile (everything up to `--new`) on its own line. The variables `%BIN%`, `%LISTS%`, `%GameFilterTCP%`, `%GameFilterUDP%` are filled in at the start: the configuration works from any folder and follows the game filter. Buttons: **RUN**, **AS SERVICE**, **SAVE TO CACHE**, **CHECK** (shows the command line and the missing files, starting nothing), **NEW**. Below are the buttons of the ready strategies: press one and its parameters land in the field. The **DNS before the start** chips tie a DNS mode to the configuration; it is applied before the start.

**CONFIGURATION CACHE.** Up to 200 entries. Everything you ran, saved or tested in TUNE lands here. When the room runs out, the entry not used for the longest time goes. **A star pins an entry — a pinned one never goes.** If all 200 are pinned, a new one will not fit: the program says so, and you unpin or delete one. The cross deletes an entry (a pinned one after a question). **CLEAR UNPINNED** removes everything except the pinned ones. The search looks through the title, the strategy and the parameters. A click on a row puts it in the field, ▶ runs it at once.

The cache is kept in `config\history\configs.json`.

**Size.** The configuration field stretches with the height of the window: what is around it (the name, DNS, the buttons, the strategies) takes its own room and everything left goes to the field. The window opens at 1280 × 860, where all buttons of the section are in view, and on a smaller screen it is made to fit the working area. The window can be dragged larger or smaller; if it is very low the section scrolls instead of cutting buttons off.

## TUNE

Parameter tuning: the program tries the variants by itself and shows which one opens more of the sites of `utils\targets.txt` (Discord, YouTube, Google and others). Before the tuning it counts how many sites open with no bypass at all, so that the result reads as "of the blocked ones this many opened".

Modes (**What to change**):

- **READY** — all the strategies of the package in turn. The simplest way to find a working one.
- **REPEATS** — changes `--dpi-desync-repeats` of the chosen base.
- **FOOLING** — changes `--dpi-desync-fooling` (ts, md5sig, badseq, badsum). Works only if the parameter is already in the strategy.
- **SPLIT** — changes `--dpi-desync-split-pos` (1, 2, midsld, sniext+1…).
- **ALL** — goes through the three above in turn; the best variant of each step becomes the base of the next.

**Base** — a strategy from ZAPRET or the text of the CONFIGS field. **Check**: QUICK (one site from each group, up to eight, no ping) or FULL (all sites and ping). The number of variants: 8, 16, 24 or 48.

While the tuning runs, the bypass is started and stopped for every variant — the internet may drop for seconds. A bypass that was running comes back after the tuning. **STOP** stops after the current variant.

The results are sorted: more sites opened, and with a tie a shorter response time. Every row has **TO FIELD**, **RUN**, **AS SERVICE** and a star. Every variant checked goes to the cache with its score (in the READY mode the five best, not all twenty).

An honest limit: the check goes over HTTPS sites. The Discord voice and UDP games are not checked by it.

## DNS

A bypass is powerless while the provider substitutes the DNS answers. Modes: **GOOGLE**, **CLOUDFLARE**, **QUAD9**, **ADGUARD** (AdGuard's "default" servers, `94.140.14.14` and `94.140.15.15`: they also block ads, trackers and phishing sites; the servers without filters, `94.140.14.140` and `94.140.14.141`, can be typed into **OWN**) and **OWN**. **APPLY** sets the servers (IPv4 and IPv6) on all connected adapters that have a way to the internet and turns on the encryption of the queries (DoH) if Windows can (Windows 11). Before the first change the state of the adapters is saved; **RESTORE** brings it back exactly: servers set by hand return (IPv4 and IPv6 each on its own), automatic ones become automatic again; DoH entries that were not there are taken out, the earlier ones get their values back. The adapter is found by its GUID, not by its number. An adapter that appeared between two changes is saved too (with its state at that moment). If something did not come back the program says so and keeps it in the backup: the button can be pressed again. A failed write is a failure: "DNS changed" appears only when the adapter really shows the new servers; if DoH encryption did not turn on, that is said too.

**Your own DNS.** The "Own DNS" field takes one line: the IP addresses of the servers (IPv4 and IPv6) and, if needed, a DoH address (`https://…`), separated by spaces or commas. For example: `1.1.1.1, 1.0.0.1, https://cloudflare-dns.com/dns-query`. How it is understood:

- The DoH address applies to all servers listed, so list servers of one provider.
- A DoH address alone is fine too: the program finds the servers by its name (or takes the address written in the DoH address itself).
- Windows knows only server addresses and DoH; the program names `tls://`, `quic://` and `sdns://` as unsuitable and does not take them.
- The field saves itself when you leave it. Start typing and the mode **OWN** turns on.
- **CHECK** asks every server listed, and the DoH address, a real question from this computer and shows whether and how fast they answer. It changes nothing and needs no administrator rights. A silent IPv6 server is not counted as a fault: many networks have no IPv6.

The mode **OWN** is also in CONFIGS among the "DNS before start" modes: a cache entry keeps its servers whole and does not depend on what the field says today.

If you have a Keenetic router, turn on "Request transit" in it.

## LISTS

**LISTS FROM GITHUB.** Zapret bypasses blocking only for the sites and addresses in its lists. Here ready large lists for Russia are connected; they are published on GitHub and updated every day or more often, and the program downloads them straight from there (only from `raw.githubusercontent.com`):

| Set | What it is | Entries |
| --- | --- | --- |
| runetfreedom · ru-blocked-community | addresses from community.antifilter.download: blocked ones that users reported | about 900 |
| runetfreedom · ru-blocked | all blocked addresses and subnets (antifilter.download) | about 88 thousand |
| runetfreedom · re-filter (IP) | addresses of the re:filter project | about 25 thousand |
| runetfreedom · Telegram, Twitter, Facebook | subnets of the three services | about 150 |
| Discord (subnets) | Discord voice servers: itdoginfo/allow-domains and re:filter | about 2,300 |
| itdoginfo · Russia inside (domains) | domains that need bypassing inside Russia | about 1,200 |
| Re:filter · community (domains) | popular and blocked domains | about 700 |
| Re:filter · domains_all (domains) | the full re:filter domain list | about 80 thousand |

The lists of runetfreedom/russia-blocked-geoip are rebuilt every 6 hours from antifilter.download and re:filter (GPL-3.0). The two large sets (ru-blocked and domains_all) carry a warning in their tooltip: they make the lists much longer, and winws reads them at every start.

How it works: the **ON** chip of a set is the whole gesture. An enabled set is written into the lists of the DPI tools chosen with the **Write into:** chips (ZAPRET, ZAPRET 2, BYEDPI; ZAPRET is on by default). For Zapret it goes **as a block between two marker lines** of this program (`# >>> Audion DPI Manager: name | entries | date` … `# <<< Audion DPI Manager: name`): domains go into `list-general-user.txt`, addresses into the real IPSet list (`ipset-all.txt`, or, while the IPSet mode is NONE or ALL, into its copy `ipset-all.txt.backup`, where the list waits). Your own lines beside them are never touched, and the blocks come out by the markers: switch a set off and the file is back byte for byte as it was. How the other two DPI tools are served is below, in "Lists for zapret2 and ByeDPI".

- **Addresses work only in the IPSet mode LIST.** A fresh Flowseal package is in the mode NONE. When you enable a set with addresses and the mode is different, the program asks whether to switch it: the mode is yours, it is not changed without asking. The domains of the sets work in any mode.
- The **↻** button of a set downloads it again, the icon beside it opens its repository on GitHub. **UPDATE ENABLED** does all enabled ones at once. The **ONCE A DAY** chip: while the window is open, the enabled sets older than 23 hours are downloaded by themselves (25 seconds after the start, then a check every hour). What is downloaded is checked line by line (addresses, subnets, domains), rubbish is dropped; an empty or broken answer never replaces the old list.
- After the Zapret package is updated or reinstalled, the enabled sets are written into it again. So they are after **UPDATE IPSET LIST**.
- **REMOVE FROM DPI TOOLS** takes everything this program wrote out of the lists of all DPI tools and switches the sets off; the sets themselves stay and can be enabled again.
- The **OWN DOMAINS** editor below does not show the blocks (they hold thousands of lines) and keeps them when it saves.
- Under the tool chips each tool has a line "what is in its lists now": the numbers are read from the files themselves, not taken from what the program meant to write.

**Lists for zapret2 and ByeDPI.** The same sets can be written into the other two DPI tools: turn their chips on in "Write into:". The DPI tool must be installed; if not, the program says so and makes nothing up for it (it writes after the installation). Each DPI tool reads its lists in its own way, so it is arranged differently:

| DPI tool | Where it is written | How it is used |
| --- | --- | --- |
| ZAPRET | `list-general-user.txt` and the IPSet list of the package, as blocks by markers | as before; addresses only in the IPSet mode LIST |
| ZAPRET 2 | domains — `Tools\zapret2\lists\list-user.txt`, addresses — `Tools\zapret2\lists\ipset-user.txt`, as blocks by markers (zapret2 lists take `#` comments) | domains work at once: the profiles of the sample select traffic by `list-user.txt`. Addresses need profiles with `--ipset`: the **PROFILES FOR ADDRESSES** chip in the ZAPRET 2 section. winws2 reloads a changed list by itself |
| BYEDPI | `Tools\ByeDPI\lists\hosts.txt` and `ipset.txt`, whole, entries only (ciadpi has no comments, markers in its files would become "domains") | only when the mode "by lists" is on in the BYEDPI section |

Names ciadpi does not understand (for example with an underscore) do not go into the ByeDPI file: the program says how many. An empty `ipset-user.txt` of zapret2 would mean "any address", so it always keeps one safe line `203.0.113.113/32` — like the mode NONE of Flowseal.

**OWN LISTS · IMPORT AND EXPORT.**

- **IMPORT FILE…** takes a `.txt`, `.lst`, `.csv` file (one entry per line, `#` is a comment) or an archive saved with EXPORT ALL. The program tells addresses from domains itself. Lines that are neither are dropped and counted. A file with the same name replaces the earlier list.
- **PASTE FROM CLIPBOARD** does the same with the text from the clipboard (the list "From clipboard").
- **EXPORT IP…** saves all addresses and subnets of the enabled sets to a text file, one per line: to take them to another program.
- **EXPORT ALL…** saves a `.zip` archive: all sets with their content and your three lists from the Zapret package (without the blocks of the sets). The same archive comes back with IMPORT FILE, on another computer too: sets with the same name are replaced, your package lists are merged line by line with those already there.

Entries are checked like this: an address is four dotted numbers or IPv6, with a mask length or without; `/0`, `0.x.x.x`, `127.x.x.x`, 224 and above are dropped (they do only harm in an ipset). A domain is a name with a dot; case, `*.` and dots at the ends are removed. The sets are kept in `config\history\lists\`.

**Flowseal's own lists:** **OWN DOMAINS** (`list-general-user.txt`), **EXCEPTIONS**, **IP EXCEPTIONS** — editable; the others lie in the package and open read-only. Looking never creates files: a user list that does not exist yet shows the text `service.bat` would put in it.

**IPSET.** **UPDATE IPSET LIST** downloads a fresh `ipset-all.txt` (the sets with addresses are written into it again afterwards).

**HOSTS.** The hosts file fixes the web version of Telegram and the Discord voice chat. **CHECK** compares it with Flowseal's reference; **WRITE TO HOSTS** appends a block between two marker lines of this program (a copy is made in `._runtime` first); **REMOVE BLOCK** removes exactly our block by the markers, the rest is not touched.

## JOURNAL

The live journal of the DPI tools: the lines `winws.exe`, `winws2.exe` and `ciadpi.exe` print as the program starts them, together with the events of their start and stop, from the start of the program. It is the journal of the dock at the bottom (Ctrl+J), sorted by DPI tool: the chips ALL, ZAPRET, ZAPRET 2, BYEDPI. **MANAGER** shows the journal ByeDPI Manager keeps in its own file (`Tools\ByeDPI\logs\bdmanager_date.log`) as it grows. FOLLOW scrolls to the new lines, COPY puts what is shown into the clipboard, CLEAR SCREEN empties the screen (the files stay), JOURNAL FILE opens the file of the day in Notepad. The output of the service and of programs that were not started by this window cannot be read: Windows does not give it out.

## DIAGNOSTICS

The Run Diagnostics item of `service.bat` as a table: the Base Filtering Engine service, the system proxy, TCP timestamps, Adguard, Killer, Intel Connectivity, Check Point, SmartByte, the bypass files (an antivirus often removes the driver), VPN, the secure DNS, hosts, a path with Cyrillic or in OneDrive, a stray WinDivert driver, the services of other bypasses. Green — fine, blue — a tip, amber — a remark, orange — a problem. Where the program can fix it there is a button. **CLEAR DISCORD CACHE** closes Discord and deletes its cache (you do not have to sign in again).

Looking changes nothing. The one exception is the same as in `service.bat`: the program turns TCP timestamps on by itself when it starts the bypass.

## ZAPRET 2

The second-generation DPI tool by bol-van: its strategies are Lua programs, winws2.exe only intercepts the traffic. **UPDATE** (or **INSTALL** in the section) downloads the release and takes out of it only what Windows needs: `winws2.exe`, the WinDivert driver, `lua\`, `files\fake\`, the filter samples. It all lies in `Tools\zapret2`. An update checks the archive and unpacks it aside, then moves the old files out of the way and puts the new ones in; if a file is in use, the previous version comes back whole.

The release has no ready strategies for Windows. There is one **PRESET** here — the "http, https, quic" example from the zapret2 documentation — in a field you edit; it works on the domain list **DOMAINS** (initially YouTube and Discord). The sets from GitHub (LISTS section, the ZAPRET 2 chip) are appended to this file as blocks: here you see and edit only your own lines.

The **PROFILES FOR ADDRESSES** chip adds, next to every preset profile that selects traffic by `list-user.txt`, a twin that selects it by `lists\ipset-user.txt` — LISTS puts the addresses of the sets there. The twins stand after the originals: a name from the list of domains wins, an address catches what has no name (QUIC, calls). Only the profile part (`--filter-…`, `--lua-desync`…) is copied; the global options (`--lua-init`, `--blob`, `--wf-…`) stay in one copy. The preset is saved at once; switch the chip off and the twins are removed, the preset is back byte for byte. **CHECK** shows the command line and the missing files. `%Z2%` in the text is the `Tools\zapret2` folder. **START** starts winws2.exe with the preset from the field (even an unsaved one; it is saved at the start). The strategy for zapret2 is picked by hand: blockcheck2 is not shipped in the Windows release.

## BYEDPI

**UPDATE** installs the ByeDPI Manager "All in One" package into `Tools\ByeDPI`: the management window, `ciadpi.exe`, ProxiFyre and the installers of dependencies. The Manager's settings (`config`) and the check lists (`proxytest`) are not touched by an update. The archive is checked and unpacked aside first; then the files are replaced, and if one is held by another program, everything is put back as it was and the version stays the old one.

**OPEN MANAGER** starts its window (strategy picking, the ProxiFyre mode for separate programs). **DEPENDENCIES** opens the `redist` folder — Windows Packet Filter and Visual C++ 2022 are needed only for ProxiFyre.

**CONNECTION** is a simple one: `ciadpi.exe` on a local port plus the Windows system proxy (`socks5://127.0.0.1:port`). **TAKE FROM MANAGER** takes the parameters and the port from its settings. On disconnect the previous proxy comes back unless another program changed it in the meantime. The default strategy is `-d1 -d3+s -s6+s -d9+s -s12+s -d15+s -s20+s -d25+s -s30+s -d35+s -r1+s -S -a1 -As -d1 -d3+s -s6+s -d9+s -s12+s -d15+s -s20+s -d25+s -s30+s -d35+s -S -a1`: two groups of splits and disorders at the SNI, the second (after `-As`) works when TLS breaks. `-S` (md5sig) exists only on Linux, so the program leaves it out of the command line, as ByeDPI Manager does on Windows. A settings file that still holds the default of earlier versions takes the new one; a strategy you wrote yourself is never touched.

**BY LISTS** — OFF, BY DOMAINS, BY ADDRESSES. ByeDPI can limit the strategy to a list: `--hosts file` (only these domains, subdomains included) or `--ipset file` (only these addresses); other traffic passes as it is. The lists are the sets from LISTS (the BYEDPI chip): the files `Tools\ByeDPI\lists\hosts.txt` and `ipset.txt`. How the program inserts the limit: ciadpi cuts its options into groups at every `--auto` (`-A`) and tests the limits of each group separately, so `--hosts`/`--ipset` is put at the head of **every** group — otherwise a group without the limit would keep working for all sites. A strategy that already has its own `--hosts`/`--ipset` stays as you wrote it. Domains and addresses cannot be combined: the conditions of one group add up (AND), so one mode is chosen. If the mode has no entry at all, the connection does not start and says why (ciadpi with an empty list would apply nothing). **COMMAND** shows the resulting command line and finds obstacles without starting anything.

## Command line

Two commands run with no window, for the builder of a release (they need no administrator rights and show no UAC prompt; they end with the exit code 0 when everything asked for is done, 1 when something failed, 2 when the command is not understood):

- `App\AudionDpiManager.exe --install-apps [all|zapret|zapret2|byedpi] [--force]` installs what the UPDATE buttons install: Flowseal's package into `Tools\zapret-discord-youtube`, zapret2 into `Tools\zapret2`, ByeDPI Manager into `Tools\ByeDPI`. Without `--force` only what is missing or newer.
- `App\AudionDpiManager.exe --reset-apps` sets the installed apps back to their own defaults: the blocks of this program in Flowseal's lists, Flowseal's own `*-user.txt` lists and game filter, zapret2's two lists, the list files of ByeDPI, log files. The apps stay. Only what lies in the project's folder: a Flowseal package the settings point to outside the project (or a link to another folder) is not touched; links (junctions) inside a package, such as `lists` or `utils`, are not followed either: the cleaning names them and skips them.

In the builder these are the step `[06] INSTALL APPS` of `builder_main.cmd` and the first thing `cleanup_project.cmd` does; the cleanup also removes `config\settings.json` (the program starts in Russian and the dark theme by its defaults), the history, the logs and the build folders. The cleanup installs nothing, and neither command builds the program.

### ByeDPI: the parameters and the settings of the Manager

**PARAMETERS…** opens the list of all the options of `ciadpi.exe`, in the groups of its manual (connection, scope, automatic mode, bypass tricks, Linux only), each with a short explanation and the button **INSERT**, which puts a ready template of the option at the end of the field of parameters. The search narrows the list by a name or a word. The options that exist only on Linux are shown too, marked "does not work on Windows": the program drops them from the command line, as ByeDPI Manager does.

**MANAGER SETTINGS** shows and changes the settings file of ByeDPI Manager (`Tools\ByeDPI\config\settings.json`), the same the tabs of its window show: the routing (OFF, PROXIFYRE, SYSTEM PROXY), the LAN, the list of programs for ProxiFyre (a process name, an exe or a folder per line; the buttons FILE… and FOLDER… add them), and what its window does (connect at start, start minimized, minimize to tray). **SAVE TO MANAGER** writes only these keys, everything else in the file (language, hotkey, history of strategies) stays. **GIVE TO MANAGER** in the connection card writes the parameters and the port from the fields into the same file; **TAKE FROM MANAGER** does the opposite. The Manager reads its file when it starts and writes it when it closes, so its window must be closed while you save here: the program refuses to write while it is open. The connection of this section does not use the routing settings; running programs through ProxiFyre, picking strategies and the autostart with Windows are done by the Manager window itself (OPEN MANAGER).

## What the program changes on the machine

- Starts and stops `winws.exe` / `winws2.exe` / `ciadpi.exe`.
- Installs and removes the `zapret` service, stops and deletes the WinDivert driver (by buttons).
- Turns TCP timestamps on (`netsh interface tcp set global timestamps=enabled`).
- Changes the DNS of the adapters (the DNS section) and the system proxy (BYEDPI → CONNECT). Both come back with buttons.
- Appends a block to `hosts` (LISTS → WRITE TO HOSTS) after a backup; the block is removed by a button.
- Writes the enabled sets of lists into the lists of the chosen DPI tools (LISTS → LISTS FROM GITHUB → "Write into"): for Zapret and zapret2 as blocks between marker lines (`list-general-user.txt` and the IPSet list of the package; `Tools\zapret2\lists\list-user.txt` and `ipset-user.txt`), for ByeDPI as files of its own, `Tools\ByeDPI\lists\hosts.txt` and `ipset.txt`. The blocks come out by the markers. It changes the IPSet mode only with your consent; the zapret2 preset only with the PROFILES FOR ADDRESSES chip, and the ByeDPI mode "by lists" only by your choice.
- Deletes the Discord cache and the services of other bypasses — only by their own buttons and after a question.

All of it is written to the journal. Nothing runs on the Windows scheduler. The program goes to the network only for versions, releases and lists on GitHub and — on your CHECK button in the DNS section — to the servers you named yourself. Once a day, while the window is open, it downloads the enabled sets of lists again; the ONCE A DAY chip in LISTS turns that off.

## Where things are

- `Start.exe` → `App\AudionDpiManager.exe` — the program.
- `Tools\zapret-discord-youtube` — the Flowseal package (another folder can be chosen; a folder of that name at the root, where versions before 1.6.0 kept it, is used while the new place is empty).
- `Tools\zapret2`, `Tools\ByeDPI` — installed by their sections on a button.
- `config\settings.json` — the language, the theme, the font, the Zapret folder, your own DNS. `config\history\` — the cache of configurations, the copies of DNS and the proxy; `config\history\lists\` — the sets of lists (their texts and `sets.json` with the switches). `config\zapret2\preset.txt` — your preset. `LICENSE` and `NOTICE` — the licence and the notice.
- `._runtime\` — journals, downloads, the cache of GitHub's answers, copies of hosts.

## Licence and trademark

Audion DPI Manager © 2026 Tensionix is distributed under the **Apache License 2.0** (the full text is the file `LICENSE`, the notice is `NOTICE`; both open from buttons in the ABOUT section). "Audion" and the Audion mark are trademarks of the author: the Apache 2.0 licence (section 6) does not give the right to use this name and mark, and a copy or a changed version of the program must not be presented as Audion. Third-party programs (Flowseal, zapret2, ByeDPI, WinDivert) and their lists are downloaded from GitHub by your button and stay under their own licences.

## If something is wrong

- **"The program is running without administrator rights".** Close the window and start Start.exe.
- **The bypass started and stopped at once.** The journal has the reason: usually a list file is missing or an antivirus removed the driver. DIAGNOSTICS shows the second.
- **The driver will not unload during an update.** The replacement is set for the next Windows boot; reboot.
- **Nothing opens after a DNS change.** DNS → RESTORE.
- **An antivirus complains about WinDivert.** It is a false alarm; add the Zapret folder to the exclusions.
