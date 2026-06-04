# skill-security-audit

A Claude Code skill that security-audits other Claude skills before you let them run.

A skill can be well-written and still read your files and post them to a webhook. Quality and safety are different questions. This one answers safety: **if I run this skill, what can it do to me, my data, or my users, and is any of it more than it needs?**

It reads the whole skill folder, the `SKILL.md` and every script it ships or calls, because the risk lives in the code that runs, not the prose. It rates seven dimensions by severity, and the verdict is governed by the single worst finding, never an average. One Critical means unsafe, even if the other six are clean.

## Install

```bash
git clone https://github.com/b1rdmania/skill-security-audit ~/.claude/skills/skill-security-audit
```

## Usage

```
security audit this skill: ~/.claude/skills/some-skill
is this skill safe to run?
what can this skill do to me?
```

Point it at a skill folder, a skill name, or pasted skill content. You get a severity-ranked report, the concrete attack each finding enables, and the fix, then an offer to apply the safe in-place mitigations: tighten the manifest, pin dependencies, add an approval gate.

## What it checks

1. **Least privilege** — does it request only the capabilities its task needs?
2. **Untrusted input and prompt injection** — does it ingest content it does not control, then act on it?
3. **Data exfiltration and privacy** — can data leave the trust boundary? (the read-private plus network combination)
4. **Destructive and irreversible actions** — what can it break that cannot be undone, and is there a gate?
5. **Secrets and credentials** — hardcoded tokens, secrets read and then sent somewhere
6. **Supply chain and code execution** — pipe-to-shell, runtime code downloads, floating dependencies
7. **Oversight and auditability** — can a human stop it, and could you prove what it did?

## What it does not do

- It does not review quality. Use [skill-auditor](https://github.com/b1rdmania/claude-skill-auditor) for that.
- It does not sandbox or enforce anything at runtime. It reads and reasons; it is an audit, not a guard.
- It does not run the skill. To gate and prove a real run, use [notary](https://github.com/b1rdmania/notary).

## Where it sits

Three skills for running code you did not write:

- [skill-auditor](https://github.com/b1rdmania/claude-skill-auditor) — is the skill any *good*? (quality)
- **skill-security-audit** — is the skill *safe*? (this)
- [notary](https://github.com/b1rdmania/notary) — prove what it *did*. (signed, human-approved execution receipts)

Audit quality → audit security → notarise execution.

## License

MIT.
