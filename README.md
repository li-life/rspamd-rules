# Rspamd Rules – li-life fork

This repository is a customized fork of [martinschaible/rspamd-rules](https://github.com/martinschaible/rspamd-rules), a collection of curated **Rspamd multimaps and map files** for spam filtering.

The purpose of this fork is to keep the original upstream rules and updates while allowing us to make adjustments.

> **Upstream:** [martinschaible/rspamd-rules](https://github.com/martinschaible/rspamd-rules)  
> **Fork:** [li-life/rspamd-rules](https://github.com/li-life/rspamd-rules)

## Why this fork exists

The upstream project is actively maintained and provides the base rule set used by this repository.

For our own mail systems we occasionally need to react more quickly to false positives, false negatives or customer-specific requirements. This fork allows us to make small adjustments without losing future upstream updates.

The goal is therefore:

- keep the repository as close to upstream as possible;
- receive new and updated rules from upstream;
- make adjustments;
- use map URLs hosted in this fork;
- minimize the number of changes that can cause merge conflicts.

## Upstream synchronization

The fork is periodically synchronized with the upstream repository using **GitHub Actions**.

Workflow:

```text
martinschaible/rspamd-rules
          │
          │ upstream changes
          ▼
     GitHub Actions
          │
          ▼
   li-life/rspamd-rules
          │
          ├─ upstream updates
          └─ li-life adjustments
```

The synchronization workflow is located at:

```text
.github/workflows/autoupdate-fork.yml
```

### Merge conflicts

Automatic synchronization works as long as upstream and this fork do not make incompatible changes to the same lines.

For this reason, our changes should be kept as small and isolated as possible. If both repositories modify the same section, the conflict must be resolved manually.

## Installation

The Rspamd configuration is based on `multimap.conf` and the corresponding configuration files.

The configuration files are normally placed in:

```text
/etc/rspamd/local.d/
```

Remote map files referenced by URL do not have to be copied manually to the Rspamd server. Rspamd downloads and caches them locally.

After changing configuration files, first check the configuration:

```bash
rspamadm configtest
```

Then restart Rspamd:

```bash
systemctl restart rspamd
```

## Repository structure

The project contains rules for different parts of an email and different spam categories, including for example:

```text
maps.d/
├─ base/
├─ body/
├─ header/
├─ sender/
├─ subject/
├─ lists/
├─ whitelist/
└─ legacy/
```

The rules include generic filters as well as topic-specific rules for areas such as:

- phishing;
- scam;
- malware;
- sales and marketing spam;
- finance and investment spam;
- health-related spam;
- seasonal campaigns;
- sender, subject and body patterns;
- URLs, domains and other message characteristics.

For the complete and current rule structure, see the repository contents and the upstream project documentation.

## Scoring

Rspamd symbols created by the multimap configuration can be assigned different scores.

Scores should always be adjusted carefully because increasing a symbol score can also increase the risk of **false positives**.

When changing scores or map rules, test the effect against both known spam and legitimate messages before deploying the change broadly.

## Updating rules

Normal upstream changes are received through the automatic synchronization workflow.

When making an adjustment:

1. keep the change as small as possible;
2. use a clear commit message;
3. avoid changing upstream files unnecessarily;
4. test the affected Rspamd configuration;
5. monitor the next automatic upstream synchronization for conflicts.

## Reporting issues

For problems caused by a **li-life-specific modification**, use this fork:

[github.com/li-life/rspamd-rules/issues](https://github.com/li-life/rspamd-rules/issues)

For questions, bugs or rule changes concerning the original project, please use the upstream repository:

- [Upstream issues](https://github.com/martinschaible/rspamd-rules/issues)
- [Upstream discussions](https://github.com/martinschaible/rspamd-rules/discussions)

## Credits

The original rule set and ongoing upstream development are maintained by **Martin Schaible**:

[github.com/martinschaible/rspamd-rules](https://github.com/martinschaible/rspamd-rules)

This fork is maintained by us.

## Disclaimer

Spam filtering rules can never guarantee perfect detection. Changes may cause false positives or allow unwanted messages to pass through.

Always test rule and score changes carefully before using them in production.
