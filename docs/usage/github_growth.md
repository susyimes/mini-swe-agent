# `mini-extra github-growth`

`mini-extra github-growth` is a read-only GitHub intake loop for projects that want
agentic self-improvement without direct self-mutation.

The command reads recent repository events, keeps a local cursor, filters for
useful signals, and writes a digest plus reviewable proposals. It does not push
code, edit issues, create pull requests, or change prompts/config by itself.

```bash
mini-extra github-growth \
  --repos SWE-agent/mini-swe-agent,susyimes/mini-swe-agent \
  --output-dir .mini-swe-agent/github-growth
```

For an hourly loop, run it from your scheduler or keep it alive explicitly:

```bash
mini-extra github-growth \
  --repos owner/repo \
  --interval-seconds 3600
```

Outputs:

- `github-growth-digest-<timestamp>.json`
- `github-growth-digest-<timestamp>.md`
- `latest.json`
- `latest.md`
- `state.json`

Set `GITHUB_TOKEN` when you need private repository access or higher rate limits.
Use a token with the smallest read-only scope that covers the repositories being
observed.

