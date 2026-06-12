# `mini-extra github-growth`

`mini-extra github-growth` is a GitHub intake and self-evolution controller for
mini-SWE-agent.

The command reads recent repository events, keeps a local cursor, filters for
useful signals, and writes a digest plus reviewable proposals. By default it does
not push code, edit issues, create pull requests, or change prompts/config by
itself.

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

## Self-evolution modes

The default mode is `digest`, which only observes GitHub and writes digest files.

Use `plan` when you want the controller to turn the digest into a concrete
self-improvement task for mini-SWE-agent:

```bash
mini-extra github-growth \
  --repos SWE-agent/mini-swe-agent \
  --evolution-mode plan \
  --repo-path .
```

Use `agent` when you want mini-SWE-agent to act on that task in its own checkout:

```bash
mini-extra github-growth \
  --repos SWE-agent/mini-swe-agent \
  --evolution-mode agent \
  --repo-path . \
  --branch-prefix codex/self-evolve
```

`agent` mode requires a clean git worktree by default. It creates a new branch,
invokes `mini` with a bounded task, writes the trajectory, and leaves the resulting
diff for review. It does not push or merge.

If no GitHub signal matches a pass, no self-evolution task is created. Add
`--force-evolve` to instead ask mini-SWE-agent to improve the growth controller
itself.

Set `GITHUB_TOKEN` when you need private repository access or higher rate limits.
Use a token with the smallest read-only scope that covers the repositories being
observed.

