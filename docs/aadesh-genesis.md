# Aadesh Genesis

## Workflow

Prerequisite: Node.js 22 or newer. The project has no runtime npm dependencies;
`npm install` is not required for the CLI.

For CI or a clean checkout, use `npm ci` to verify the lockfile. For a source checkout
without a package install, `node tools/genesis.mjs ...` is also supported.

The Node CLI and dashboard are tested on Windows and Ubuntu in GitHub Actions. The
legacy `install.sh` and `tools/scaffold.sh` compatibility shims are POSIX scripts and
are exercised on Ubuntu; Windows users should invoke the Node CLI directly.

```text
init → spec → plan → approve → run → verify → checkpoint → recover
```

The CLI is provider-independent. An agent host may connect through `genesis agent`,
while Aadesh Genesis remains responsible for scope, permissions, persisted state, and
evidence. A process exit is never treated as proof of completion.

## Quick start

```sh
node tools/genesis.mjs init . --name "Demo" --objective "Build a task tracker"
node tools/genesis.mjs status .
node tools/genesis.mjs tasks . --json
node tools/genesis.mjs checkpoint .
node tools/genesis.mjs recover .
```

Use `genesis plan check` followed by `genesis approve` only after the specification
and task gates have been reviewed by a human. `genesis tasks` is read-only. The
dashboard and MCP index are also read-only; state-changing operations remain CLI
operations with audit records.

## Honest states

Evidence is bound to source and configuration hashes. Changes to relevant files or
task configuration make old evidence stale. Tasks can therefore be complete and
verified, implemented but unverified, blocked, failed, or awaiting approval without
the framework inventing success.

## Boundaries

The framework is not an operating-system sandbox. Keep repositories and agent
commands within trusted working directories, review authorization grants, and do not
place secrets in task context or repository files.
