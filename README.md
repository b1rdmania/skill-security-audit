# skill-security-audit

A Claude Code skill that security-audits other Claude skills before you let them run.

A skill can be well-written and still read your files and post them to a webhook. Quality and safety are different questions. This skill answers the safety one: **if I run this, what can it do to me, my data, or my users — and is any of it more than it needs?**

It reads the whole skill folder — the `SKILL.md` *and* every script it ships or calls, because the risk lives in the code that runs, not just the prose — and rates seven dimensions by severity. The verdict is governed by the single worst finding, never an average: one Critical means unsafe, even if the other six are clean.

## The seven dimensions

1. **Least privilege** — does it request only the capabilities its task needs?
2. **Untrusted input & prompt injection** — does it ingest content it doesn't control, then act on it?
3. **Data exfiltration & privacy** — can data leave the trust boundary? (the read-private + network combo)
4. **Destructive & irreversible actions** — what can it break that can't be undone, and is there a gate?
5. **Secrets & credentials** — hardcoded tokens, secrets read and then transmitted
6. **Supply chain & code execution** — pipe-to-shell, runtime code downloads, floating dependencies
7. **Oversight & auditability** — can a human stop it, and could you prove what it did?

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

Point it at a skill folder, a skill name, or pasted skill content. It returns a severity-ranked report, the concrete attack each finding enables, and the mitigation — then offers to apply the safe in-place fixes (tighten the manifest, pin dependencies, add an approval gate).

## Where it sits

Part of a trilogy for running skills you didn't write:

- [skill-auditor](https://github.com/b1rdmania/claude-skill-auditor) — is the skill any *good*? (quality)
- **skill-security-audit** — is the skill *safe*? (this)
- [notary](https://github.com/b1rdmania/notary) — prove what it *did*. (signed, human-approved execution receipts)

Audit quality → audit security → notarise execution.

## License

MIT.
