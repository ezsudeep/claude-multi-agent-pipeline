# Multi-Agent Pipeline Skill

> Universal multi-agent orchestration framework for Claude — runs true parallel agents in Cowork and Claude Code, structured sequential pipeline in Claude Chat. Covers complex document work, codebase refactoring, research reports, data pipelines, and batch processing.

---

## About

This skill gives Claude a reusable 5-stage orchestration pipeline that adapts to whatever environment it's running in:

- **Claude Cowork** — true parallel sub-agents, file system access, long-running tasks
- **Claude Code** — true parallel via the `Task` tool, terminal access
- **Claude Chat** — disciplined sequential pipeline with named worker roles, in-conversation output

The pipeline follows: **Planner → Researcher → Workers → Reviewer → Assembler**

Claude automatically detects the environment and switches modes — no manual configuration needed.

---

## Installation

1. Download `multi-agent-pipeline.skill`
2. Open **Claude Desktop**
3. Go to **Settings → Skills**
4. Drag and drop the `.skill` file or click **Install**

---

## What It Does

### 5-Stage Pipeline

| Stage | Role | What Happens |
|-------|------|--------------|
| **Planner** | Breaks task into subtasks | Produces numbered subtask list, parallel groupings, per-worker context requirements |
| **Researcher** | Gathers all context | Reads source files, builds context bundle for workers so they don't hunt mid-task |
| **Workers** | Execute in parallel | Up to 3 specialist agents simultaneously (Cowork/Code) or sequentially (Chat) |
| **Reviewer** | QA gate | Checks cross-worker consistency, errors, completeness — approves or sends back |
| **Assembler** | Merges final output | Combines approved outputs, backs up originals, writes draft then renames to final |

### Environment Modes

| Capability | Chat | Cowork | Claude Code |
|------------|:----:|:------:|:-----------:|
| 5-stage pipeline | ✅ | ✅ | ✅ |
| True parallel agents | ❌ | ✅ | ✅ |
| File system read/write | ❌ | ✅ | ✅ |
| Worker isolation | ❌ | ✅ | ✅ |
| Long-running tasks | ❌ | ✅ | ✅ |
| Scheduled tasks | ❌ | ✅ | ❌ |
| Retry specific worker only | ❌ | ✅ | ✅ |
| Versioned file backups | ❌ | ✅ | ✅ |

---

## Trigger Phrases

The skill activates automatically on:

- `"run agents"`, `"parallel agents"`, `"multi-agent"`
- `"spawn agents"`, `"split into agents"`, `"orchestrate"`
- Any task with **3 or more distinct subtasks** — even without explicit agent keywords
- Large document audits, batch file processing, multi-section reports

---

## Task Prompt Templates

### Document / Thesis Audit
```
I have a [document type] at [path/folder].

Run parallel agents to:
1. Audit Chapter 4 statistics
2. Audit Chapter 5 narrative
3. Format all tables per APA 7

After all agents complete, compile corrections and apply to produce
a fixed version at [output path]. Backup the original first.
```

### Codebase Refactor
```
Refactor the codebase at [path].

Spawn parallel workers:
- api-worker: update all endpoints in /api to [new pattern]
- db-worker: migrate queries in /db to [new ORM]
- test-worker: update /tests to match new signatures

After review, merge all changes and run the test suite.
```

### Research Report
```
Write a [N]-section report on [topic].

Use:
- literature-worker: summarize [sources] into key findings
- data-worker: analyze [dataset] and extract statistics
- analysis-worker: synthesize findings and write conclusion

Assemble into a formatted [docx/md] at [output path].
```

### Batch Document Processing
```
Process all [file type] files in [folder].

For each file, spawn a worker to [task].
Run up to 3 workers in parallel.
Collect all outputs into a summary [format] at [output path].
```

---

## File Conflict Prevention

Built into the skill — always applied automatically in Full Mode:

```
Rule 1: Workers READ shared files, never WRITE to shared files
Rule 2: Each worker writes ONLY to /tmp/worker-{name}/
Rule 3: Only Assembler writes to the final output path
Rule 4: Backup before overwriting: cp file.docx file_backup_$(date +%s).docx
Rule 5: Write to draft first, rename to final after verification
```

---

## Global Cowork Instructions (Optional)

To activate the pipeline automatically for every Cowork task, go to **Settings → Cowork → Global Instructions** and paste:

```
For any task with 3 or more distinct subtasks, use the multi-agent pipeline:
1. Plan all subtasks before spawning any agent
2. Run independent subtasks as parallel workers (max 3 at once)
3. Each worker writes only to /tmp/worker-{name}/ — never to shared files
4. Review all outputs for consistency before assembling
5. Always backup files before overwriting: append _backup_{timestamp}
6. Write final output to draft first, then rename to final
```

---

## Real-World Example

The skill was validated against a live nursing research thesis audit in Claude Cowork:

- **Task:** Audit and correct a multi-chapter thesis (QoL Among Cancer Patients, EORTC QLQ-C30)
- **Workers spawned:** `ch5-auditor`, `ch4-auditor`, `table-formatter` — all 3 in parallel
- **Result:** Full correction list compiled, fix script generated, corrections applied to final docx

---

## Repo Structure

```
multi-agent-pipeline/
├── SKILL.md                  ← Skill instructions (auto-loaded by Claude)
├── README.md                 ← This file
├── LICENSE                   ← MIT
└── multi-agent-pipeline.skill ← Installable skill package
```

---

## Compatibility

| Environment | Support |
|-------------|---------|
| Claude Desktop (macOS / Windows) — Cowork | ✅ Full Mode |
| Claude Code (terminal) | ✅ Full Mode |
| Claude.ai web chat | ✅ Structured Mode |
| Claude mobile app | ✅ Structured Mode |

Requires: **Claude Pro, Max, Team, or Enterprise** for Cowork features.

---

## License

MIT — free to use, modify, and distribute.

---

*Built by [ezsudeep](https://github.com/ezsudeep) · Part of the Claude Skills collection*
