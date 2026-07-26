# skillpack-infrastructure-pivoting

The **Infrastructure pivoting** Skill Pack for Vineyard — a text investigation *playbook* the agent
can consult when turning one infrastructure indicator (an IP, a domain, a certificate) into the
connected footprint around it, one verifiable hop at a time.

A Skill Pack runs no code and requests no permissions of its own; it is guidance the agent follows,
surfaced to it through the `list_skills` / `load_skill` tools. Its only dependency is the Plugin
Pack(s) its steps call, declared in `requires`.

`requires` is deliberately empty here. The method is built on the graph read tools and on whatever
collection plugins the project happens to have: each section names the kind of plugin a hop wants
(DNS, RDAP, certificate transparency, ASN) and tells the agent to say which hop it cannot take when
none is installed, rather than filling the gap from memory. A skill pack whose steps genuinely cannot
proceed without a specific pack should list that pack in `requires` instead.

| Field | Value |
| --- | --- |
| Identifier | `run.vineyard.skillpacks.infra_pivot` |
| Applies to | `infrastructure.ip_address`, `infrastructure.domain`, `infrastructure.certificate`, `infrastructure.autonomous_system` |
| Requires | — |
| Sections | `discipline` — when to stop, and how not to invent links · `dns-pivot` — resolution, reverse and the records that expand it · `cert-pivot` — shared certs and CT-log siblings · `asn-pivot` — the announcing AS and its neighbourhood |

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
