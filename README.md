# skillpack-infrastructure-pivoting

The **Infrastructure pivoting** Skill Pack for Vineyard: a text investigation playbook that turns one infrastructure indicator (an IP, a domain, a certificate) into the connected infrastructure around it, one verified hop at a time.

| Field | Value |
| --- | --- |
| Identifier | `run.vineyard.skillpacks.infra_pivot` |
| Version | `2.5.1` |
| Applies to | `infrastructure.ip_address`, `infrastructure.domain`, `infrastructure.certificate`, `infrastructure.autonomous_system` |
| Requires | — |

## When to use
- The analyst hands you one indicator and asks what else is connected to it.
- You need to expand a lead into hosting, DNS, certificates, ASN, web fingerprints, origin, or history.
- You need to decide whether two indicators share an operator or only a provider.

Do not use it for a single fact about one node, or for a person/account/handle question. Use the matching collection plugin or "Account & identity pivoting" instead.

## Requirements
- Collection plugins for the hops you take: DNS, RDAP, certificate transparency, ASN, web probe, Wayback.
- Graph write access to stage nodes and edges.
- Analyst authorization for any active check. Active checks are desktop-only and run only on request.

`requires` is empty because the pack works with whatever the project has.

## Structure
The `overview` is the top-level Markdown card. Load one section for the hop you are on; each section body is a self-contained Markdown document with its own `#` title and `##` headings.

| Section | Load when | Output |
| --- | --- | --- |
| `discipline` | Before staging an edge, closing a cluster, or asserting a shared operator | An evidence-labelled edge, a population estimate, a stop decision |
| `background-values` | A shared value looks specific | Background or candidate, with the reason |
| `prioritize` | Many indicators or a limited query budget | A ranked seed list and a search plan |
| `settle` | A candidate has a gap | A promoted edge, a killed branch, or a named hop |
| `dns-pivot` | Domain/IP records | IP, nameserver, mail/alias nodes, candidate domains |
| `cert-pivot` | Certificate/host/CT result | Shared-certificate edges, siblings, default classification |
| `asn-pivot` | IP ASN or neighbourhood | AS node, announced-by edge, bounded search space |
| `web-footprint` | URL build or account question | Shares-a-build edges, account nodes, counts |
| `cdn-origin` | Host behind a CDN/WAF | Origin node with evidence-labelled edges |
| `historical` | Old snapshots matter | Dated edges and old contacts |

## How it decides that a shared attribute means something

It does not come from a fixed list of what counts as background. A shared attribute links two things only to the extent that it is rare.

`discipline` asks for an estimate: **if these two were unrelated, how likely is it that they would share this?** In other words, how many unrelated parties have the same attribute. The agent says what the judgement rests on, and measures when it can: how many domains that nameserver serves, how many prefixes that AS announces, how many hosts presented that certificate.

Narrowing is multiplication, not counting, but only across attributes that could have come from different acts. One hosting panel sets the nameservers, the mail records, and the certificate in one click, so "two of three fingerprints match" is usually one fact seen twice. An edge still needs a tool result that names both endpoints together.

## Layout

| Path | Purpose |
| --- | --- |
| `skillpacks/infra-pivot.skill.json` | The Skill Pack document (overview + sections + starters) |
| `.github/workflows/validate.yml` | CI validation |

## Listing / installing

Listed in the [registry](https://github.com/Vineyard-Intelligence/registry); browsable at [docs.vineyard.run](https://docs.vineyard.run/). The registry entry pins an immutable commit of this repo, and the document is served from it via jsDelivr.

## License

MIT
