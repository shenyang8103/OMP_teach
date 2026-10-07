# teach

A teaching skill for **OMP** (the OMP agent harness). It teaches the user anything so it actually *locks in* and is *understood*, not just memorized — based on two verified teaching principles, with plan persistence, per-module checkpoints, study notes, optional source-material support, and an答疑 loop before each quiz.

This is a port/rewrite of the teaching system from [amosblomqvist/learn](https://github.com/amosblomqvist/learn) (originally built for the `pi` framework), adapted to OMP's tool/skill/agent model. **Huge thanks to [amosblomqvist](https://github.com/amosblomqvist) for the original system and ideas.**

## What it does

- **Probe → Plan → Teach loop.** Locates the user's understanding edge (floor + ceiling), builds a dependency-graph plan, gets approval, then teaches node by node.
- **Two principles.** Connected knowledge > disconnected facts; unconditional truths first. Every node: Motivate → Establish → Connect → 答疑 → Quiz-check.
- **Plan persistence.** On approval it writes `learn-plan.md` (mermaid dependency DAG + module checklist + progress pointer). Resuming later asks "continue or adjust" instead of re-probing.
- **Study notes.** Each session appends a review notebook named `目录名_yymmdd_x.md` (content dir name + date + session index). The skill never reads it back — it's for the human to review.
- **Optional source material.** First run asks whether the user has a reference folder (a subdir under the cwd). If yes, those files are the **authoritative** source; web search is only supplementary and defers to the material on conflict. The choice is recorded in `learn-plan.md` and never re-asked.
- **答疑 loop.** After teaching a node, a two-option choice appears: **马上测试 (default)** / **你有疑问**. Only when the user picks "你有疑问" do they get to ask; the loop repeats until they choose to test.
- **Randomized quiz answers.** Correct option position is derived from the current clock seconds (`seconds % N`) so it isn't always first; the derivation is never shown to the user.
- **Chinese-first.** All user-facing teaching output is in Chinese; English terms are kept parenthesized (e.g. `质数 (prime)`).
- **Researcher subagent.** A bundled `researcher` task-agent web-researches and returns a sourced brief for plan accuracy.

## Directory structure

```
teach/
├── package.json              # OMP skill manifest
├── README.md                 # this file
├── skills/
│   └── teach/
│       └── SKILL.md          # the skill definition (the real content)
└── agents/
    └── researcher.md         # bundled researcher task-agent
```

> Note: `agents/researcher.md` is referenced by `SKILL.md` and must be installed alongside the skill (see Install).

## Install (OMP)

OMP loads user-level skills from your agent directory. Copy (or symlink) this skill there:

```powershell
# Windows (PowerShell) — replace <USER> with your user name
$agent = "$env:USERPROFILE\.omp\agent"
Copy-Item -Recurse teach\skills\teach      $agent\skills\teach
Copy-Item -Recurse teach\agents\researcher.md  $agent\agents\researcher.md
```

On other shells the equivalent is:

```sh
AGENT="$HOME/.omp/agent"
cp -r teach/skills/teach        "$AGENT/skills/teach"
cp -r teach/agents/researcher.md "$AGENT/agents/researcher.md"
```

After install, `omp read skill://teach` should resolve from any working directory. To update later, re-run the copy (or point the skill at this repo via OMP's `skills.customDirectories`).

## Usage

In any OMP session, just ask it to teach you something, e.g.:

> 用 teach 技能教我质数

The skill will probe your level, propose a plan (with a mermaid map), wait for your go-ahead, then teach module by module — offering a checkpoint (continue next module / save & exit) after each one, and writing `learn-plan.md` + a `目录名_yymmdd_x.md` notebook. Reopen the same working directory later and it resumes from the saved plan.

## License

See the original project for spirit; this port is provided as-is for personal use. If you redistribute, please keep the attribution to [amosblomqvist/learn](https://github.com/amosblomqvist/learn).
