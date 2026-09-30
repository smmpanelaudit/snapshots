# Snapshots and measurements, smmpanelaudit.com

This repository holds the evidence behind the audits published on
[smmpanelaudit.com](https://smmpanelaudit.com/). Nothing here is interpretation: it is the pages as they were
served, on the day they were read, and the file the measurement was taken from.

## What is in here

```
snapshots/<date>/index.json                  what was fetched, when, with which status and hash
snapshots/<date>/<domain>/homepage.html      the bytes the server returned
snapshots/<date>/<domain>/terms.html
snapshots/<date>/<domain>/services.html
snapshots/<date>/<domain>/contact.html
snapshots/<date>/<domain>/api.html
measurements/<date>/measured.json            what was read off those pages, item by item, with the quotes
```

Files over 1 MB are stored gzipped, with `.gz` on the name and `gzipped: true` in `index.json`. A page that
answered HTTP 403, timed out or did not exist is recorded in `index.json` with the reason and no file: an
unreadable page is not a finding.

Each entry in `index.json` carries the URL, the time it was fetched in UTC, the HTTP status, the SHA-256 of
the bytes, the byte length, the final URL after redirects, where the file is stored, and the Wayback Machine
copy where the archive made one.

## Rehearsal runs are marked as such

Not every folder here is an audit. `snapshots/2026-09-30/` is a rehearsal: the tooling was run over all ten
panels on 30 September 2026 to check that it worked, one day before the measurement. The October 2026 audit
was measured on **1 October 2026**, and that is the only reading its results come from. Two folders do not
mean two measurements.

The authoritative marker is in the file itself. Every `index.json` carries a `purpose`, and every
`measured.json` carries the same field:

```
"purpose": "rehearsal"     a test of the tooling; no result from it is published
"purpose": "measurement"   the reading an audit is written from
"purpose": "dry run"       an early test of the snapshot system, September 2026
```

If you are checking a figure in an article, use the folder whose date matches the measurement date printed
on that article.

## Checking a hash

```
sha256sum snapshots/2026-10-01/example.com/homepage.html
```

On macOS, `shasum -a 256 <file>`. For a gzipped file, check the bytes as they were served:

```
gunzip -c snapshots/2026-10-01/example.com/services.html.gz | sha256sum
```

Compare the result with `sha256` for that page in `snapshots/<date>/index.json`. The same hash is checked at
every build of the site, so a file that no longer matches would stop the site from publishing.

## What the files do not tell you

A snapshot is one reading of one page on one day, taken while logged out, with no account and no purchase. It
does not show what a panel states behind a login, and it does not show what a panel delivers. The method,
the criteria and the weights are published at
[smmpanelaudit.com/methodology](https://smmpanelaudit.com/methodology/) before any panel is read.

`measurements/<date>/measured.json` records what a script proposed for each item and the evidence it was
proposed from. The published audit carries the author's confirmed reading, which can differ from the
proposal. Where the two differ, the article is what the site stands behind, and the correction log is at
[smmpanelaudit.com/corrections](https://smmpanelaudit.com/corrections/).

The site is operated by the team behind Boostero, an SMM panel that is measured in the same set, on the same
day, by the same formula as every other panel. Every audit is published under a named author, listed on the
site.

To report an error in a reading, use the contact page: [smmpanelaudit.com/contact](https://smmpanelaudit.com/contact/).
