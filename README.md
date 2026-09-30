# skillpack-infrastructure-pivoting

The **Infrastructure pivoting** Skill Pack for Vineyard — a text investigation *playbook* the agent
can consult when turning one infrastructure indicator (an IP, a domain, a certificate) into the
connected footprint around it, one verifiable hop at a time.

A Skill Pack runs no code and requests no permissions of its own; it is guidance the agent follows.
Its only dependency is the Plugin
Pack(s) its steps call, declared in `requires`.

`requires` is empty here: the pack works with whatever collection plugins the project has. Each
section names the kind of plugin a hop wants (DNS, RDAP, certificate transparency, ASN), and when none
is installed the agent says which hop it cannot take rather than filling the gap from memory.

| Field | Value |
| --- | --- |
| Identifier | `run.vineyard.skillpacks.infra_pivot` |
| Applies to | `infrastructure.ip_address`, `infrastructure.domain`, `infrastructure.certificate`, `infrastructure.autonomous_system`, `web.url` |
| Requires | — |
| Sections | `discipline` — when to stop, and how much a shared attribute is worth · `prioritize` — which seed to expand first when you hold many indicators · `settle` — the check that would decide an open candidate · `dns-pivot` — resolution, reverse and the records that expand it · `cert-pivot` — shared certs and CT-log siblings · `asn-pivot` — the announcing AS and its neighbourhood · `web-footprint` — favicon/header/DOM hashes that cluster hosts by what they serve · `cdn-origin` — find the origin behind a CDN/WAF and confirm it serves the site (passive routes by default; active routes desktop-only, on the analyst's explicit instruction) · `historical` — what the past says that today hides (Wayback hops) |

## How it decides that a shared attribute means something

Not from a fixed list of what counts as background — a shared attribute links two things only to
the extent it is rare.

`discipline` asks for an estimate: **if these two were unrelated, how likely is it that they would
share this?** — that is, how many unrelated parties wear the same attribute. The agent names what the
judgement rests on so the analyst can check it, and **measures rather than guessing wherever a number
is fetchable**: how many domains that nameserver serves, how many prefixes that AS announces, how many
hosts presented that certificate.

Narrowing is a product, not a count — but only across attributes that could have come from *different
acts*. One hosting panel sets the nameservers, the mail records and the certificate in a single click,
so "two of three fingerprints match" is usually one fact observed twice. An edge still needs a tool
result naming both endpoints together.

## Layout

| Path | Purpose |
| --- | --- |
| `skillpacks/infra-pivot.skill.json` | The Skill Pack document (overview + sections) |

## Listing / installing

Listed in the [registry](https://github.com/Vineyard-Intelligence/registry); browsable at
[docs.vineyard.run](https://docs.vineyard.run/). The registry entry pins an immutable commit of this
repo, and the document is served from it via jsDelivr.

## License

MIT
