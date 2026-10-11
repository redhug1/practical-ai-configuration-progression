# Progression Guide 9: Test whether the agent workflow is reliable and worthwhile

A practical guide to comparing one assistant with several agents using the same small change, the same success criteria and evidence from complete attempts.

This guide builds on the main guide's evaluation work and [Guide 7: coordinating agents](AI-Progression-Guide-07-Coordinating-Agents.md), [Guide 6: controlled handovers](AI-Progression-Guide-06-Controlled-Handovers.md), and [Guide 8: parallel work](AI-Progression-Guide-08-Parallel-Work.md). It is the ninth progression guide; it does not replace Example 9 in the original collection.

**Documentation checked and example prepared: 11 October 2026.** Current primary documentation is linked beside product-specific explanations and at the end.

## What you will learn

By completing this guide, you will be able to:

- Define success before running a comparison.

- Give both arrangements identical starting application files, requirements, fixed tests and permitted changes.

- Distinguish a correct application from a complete, honestly reported workflow.

- Record first-pass results, bounded repairs, blocked attempts and failed handovers.

- Count observable tool calls and assess whether a call served a necessary purpose.

- Measure a consistent wall-time boundary and record active human effort separately.

- Record actual usage/cost with its coverage, unit and source, leaving unavailable values unknown.

- Include failed attempts when calculating the cost of obtaining a successful result.

- Compare matched runs, repeat the exercise and explain the limits of the evidence.

- Decide whether extra agents are worthwhile for this task without assuming more agents means better work.

The example adds **`--status open`** and **`--status done`** to a small fictional task-list program. Both arrangements must implement the change, update the usage document, and return a plan, implementation receipt and final review. The human runs identical checks and assesses the same requirements.

This guide is self-contained. It includes all starting files, native profiles, prompts, observation forms, checking commands, calculation examples and reference implementations. No API, paid evaluation service, MCP server, credential or package installation is required for the sample.

## Evaluate the complete attempt

An **evaluation** is a planned check against stated criteria. A **workflow** is the whole sequence: accept the request, plan, implement, pass work on, check the result and report it. A **trace** is a record of what happened, including the visible agent/model activity and tool calls. A reassuring final answer is not a trace.

**Correctness** asks whether the requested result works. **Reliability** asks how consistently the arrangement succeeds within its rules and limits. **Worthwhile** asks whether that result justifies its usage, time and human effort for the task you care about.

Official OpenAI evaluation guidance distinguishes inspecting workflow traces from running repeatable datasets/evaluations. This tutorial uses local fixed checks and human observation rather than the OpenAI evaluation API. See [Evaluate agent workflows](https://developers.openai.com/api/docs/guides/agent-evals).

If you use the evaluation extension described in section 23 of your main guide, retain its actual output as additional evidence. Establish which parts of the workflow it observed; do not assume an individual customization score supplies complete handover, timing or billing evidence.

### The two arrangements

| Arrangement | Who does the work | Conversations/contexts | What stays the same |
| --- | --- | --- | --- |
| SINGLE | One assistant plans, implements and reviews in one conversation | One primary context; no delegation | Request, source/data/tests, two writable paths, stage outputs and human checks |
| MULTI, native route | A primary coordinates planner, implementer and reviewer specialists sequentially | One primary plus three specialist roles; corrections can add invocations | The same task and acceptance rules |
| MULTI, attachment route | The human supplies complete handovers between three fresh specialist chats | Three independent role chats; no native coordinator is claimed | The same task and acceptance rules, with human transfer included in time |

This is a **human-stepped** comparison. Both arrangements stop between stages while you save outputs and run checks. Native delegation is used where supported; this exercise does not measure a fully unattended production coordinator. In a later experiment, measure unattended operation separately.

The stages are dependent, so run them sequentially. Guide 8 explains when independent tasks can overlap. Do not add parallel activity to one arm here: that would change the comparison.

Compare SINGLE and MULTI **within one chosen route**. A Codex run versus an attachment-based ChatGPT run changes the host, tool access and transfer method as well as the number of agents. It is a different experiment.

### Define success before seeing the answers

An attempt is a **verified success** only when all of these are supported:

1. All 20 fixed application tests pass on the current files; scope and stable-version checks pass.

2. The source and usage document meet R1-R8, including cases and wording that need human inspection.

3. Both arrangements returned complete current plan, implementation and review documents with the right run/request identity.

4. The intended architecture was actually observed, and the reported actions/checks are honest.

5. Required cross-agent handovers are recorded, with missing or inconsistent inputs resolved or reported.

6. The run finishes within the declared time limit and uses no more than one correction episode.

Application tests alone do not check every document, agent identity, permission or handover. A passing program with missing usage instructions is incomplete. A correct program from an unobserved architecture does not establish that the intended agent workflow worked.

**First-pass success** means a verified success with no correction episode. A repaired success remains a final success within the budget, but it is not first-pass success.

### Measures and their boundaries

| Measure | What you record | What you must not infer |
| --- | --- | --- |
| Verified success | Current fixed checks plus common human gates | That 20 passing tests cover every requirement or environment |
| First-pass success | Final success with zero corrections | That a later repaired result succeeded immediately |
| Failed handovers | Actual cross-agent packets that were missing, stale, contradictory or misrouted | That a receiver's correct refusal is itself bad behaviour |
| Tool calls | One row per observable dispatched invocation, including delegation/control calls | Hidden calls or a true total from a partial trace |
| Unnecessary calls | Calls you can justify classifying as avoidable | That every repeated read or test is wasteful |
| Wall time | Before the first task prompt until final assessment/checking or a stopped attempt | The sum of agent durations equals elapsed time |
| Active human time | Your measured time spent operating/checking/transferring | An invented value obtained by subtracting model time |
| Usage/cost | Host/provider evidence covering the whole run, with unit and basis | Exact per-run cash cost from a subscription or quota indicator |

Use the same counting rules in both arrangements. The human's terminal checks are included in wall time but are not AI-dispatched tool calls. Record shared setup separately; it is excluded from the run timer.

## Folder arrangement and files

Everything is under **`$HOME/ai-examples/ai-progression-09`**.

| Path under that parent | Purpose |
| --- | --- |
| `template/` | Frozen common starting project; never changed after freezing |
| `experiment.json` | Your route, model/host record, planned repeats and limits |
| `baseline.json`, `setup-baseline.json` | Common fingerprints and eight starting checks |
| `runs/pair-1-single/project/` | Fresh SINGLE project copy for the first pair |
| `runs/pair-1-multi/project/` | Fresh MULTI copy for the same first pair |
| `runs/pair-2-*/`, `runs/pair-3-*/` | Later fresh pairs, created when needed |
| `runs/<run-id>/stages/` | Actual plan, implementation receipt and review; preserved earlier versions if corrected |
| `runs/<run-id>/evidence/` | Initial version and fixed-check bundles |
| `runs/<run-id>/observations.json` | Your observations, manual gates and usage records |
| `runs/<run-id>/timing-*.json`, `grade.json` | Run timing and combined local assessment |
| `reports/` | Observed comparisons and separately labelled synthetic examples |
| `decision.md` | Your evidence-based decision and limits |
| `probes/` | Optional diagnostic handover exercise, outside the comparison |

Inside each project, `app.mjs` and `docs/usage.md` are the only writable targets. Data, both test files, request, roles, templates, helpers and native profiles remain fixed.

You create **19 supplied project files**. The profile generator adds **eight native files**, giving **27 common files**. Preparing a run adds its own `.codex/config.toml`, giving 28 project files. The SINGLE/MULTI delegation settings are a deliberate recorded difference; all common project files begin identical.

`AGENTS.md` supplies project rules. The ordinary `roles/*.md` files define stage contracts; the native profile files point to them. No `SKILL.md` is needed for this particular comparison. The benchmark helper measures/checks files and records; it does not operate AI chats, invoke agents or run a complete autonomous workflow.

## How to follow the instructions

| Label | What you do |
| --- | --- |
| **YOU — in VS Code** | Create, inspect or save the named file |
| **YOU — in the terminal** | Run the command in the stated folder |
| **YOU — in the AI chat** | Send the specified prompt and current labelled inputs |
| **EXPECTED ON SCREEN** | Compare the output; do not type it |
| **AI — expected response** | Check the actual response against these conditions |
| **EXPLANATION ONLY** | Read; no action is required |

Copy code-box contents without their outer backticks; preserve inner Markdown fences. Save with Ctrl+S on Linux or Command+S on Mac. Run commands separately. Run `echo $?` immediately after the command whose exit code you want to check.

`0` means success for a helper command. `1` means a refused/failed helper check. The sample application's invalid CLI requests exit `2`.

Read the run instructions before starting a timer, and prepare your prompts and file attachments. Instruction-reading and shared setup are not part of the declared run boundary. Normal handover/checking/transfer work during an attempt is included.

## Before you start

You need VS Code, one supported assistant route and **Node.js 24**. No Git setup or npm package is needed. The practice files are fictional.

The instructions are intended for Ubuntu 22.04, 24.04 and 26.04; Kubuntu 26.04; and supported Intel/Apple Silicon macOS installations, including M4/M5 machines. Preparation did not test every listed OS/processor.

**YOU — in VS Code**

Open **Terminal → New Terminal**.

**YOU — in the terminal**

Run:

```bash
node --version
```

**EXPECTED ON SCREEN**

A version beginning with `v24.`. If you used Guide 1's separate Node installation and it is missing from this terminal's path, run:

```bash
export PATH="$HOME/usr/local/node-mcp-training/bin:$PATH"
```

Repeat the version check. If needed, use [Guide 1's Node preparation](AI-Progression-Guide-01-MCP.md) or <https://nodejs.org/en/download>.

Use the same host, available model and reasoning setting for both arms. Avoid automatic model changes where possible; record actual identities where visible. A missing identity remains `not-visible`, and prevents a strong claim that model conditions were matched.

## Step 1 — Create the parent and common template

**YOU — in the terminal**

Create the shared parent if necessary:

```bash
mkdir -p "$HOME/ai-examples"
```

Create a fresh folder:

```bash
mkdir "$HOME/ai-examples/ai-progression-09"
```

If it exists, stop and choose an unused parent name, replacing `ai-progression-09` consistently below. Preserve earlier work. Keep the internal names `template`, `runs`, `project` and the supplied `pair-N-single/multi` names unchanged; the helper uses them.

Enter it:

```bash
cd "$HOME/ai-examples/ai-progression-09"
```

Create folders:

```bash
mkdir -p template/data template/tests template/docs template/requests template/roles template/templates template/tools template/examples template/.github runs reports probes
```

**YOU — in VS Code**

Choose **File → Open Folder** and open your Home folder's `ai-examples/ai-progression-09`. This parent window is for setup and records. Later, open each run's `project/` alone in a separate window.

Create files at the exact paths below. Do not put evidence or records inside the project. If a trust prompt appears, inspect your practice files and use your normal trust decision.

## Step 2 — Create the program, data and fixed tests

**YOU — in VS Code: create and save `template/app.mjs` with exactly these contents.**

````javascript
import { readFileSync } from 'node:fs';
import { resolve } from 'node:path';
import { fileURLToPath } from 'node:url';

export function renderTasks(tasks, options = {}) {
  if (options.summary) {
    const done = tasks.filter(task => task.done).length;
    return `Total: ${tasks.length}\nOpen: ${tasks.length - done}\nDone: ${done}\n`;
  }
  return tasks.length ? tasks.map(task => `${task.id}. [${task.done ? 'x' : ' '}] ${task.title}`).join('\n') + '\n' : 'No tasks.\n';
}

if (process.argv[1] && resolve(process.argv[1]) === fileURLToPath(import.meta.url)) {
  const args = process.argv.slice(2);
  if (args.some(arg => arg !== '--summary')) {
    console.error('Usage: node app.mjs [--summary]');
    process.exitCode = 2;
  } else {
    const tasks = JSON.parse(readFileSync(new URL('./data/tasks.json', import.meta.url), 'utf8'));
    process.stdout.write(renderTasks(tasks, { summary: args.includes('--summary') }));
  }
}
````

**YOU — in VS Code: create and save `template/data/tasks.json` with exactly these contents.**

````json
[
  { "id": 1, "title": "Read the request", "done": true },
  { "id": 2, "title": "Make the change", "done": false },
  { "id": 3, "title": "Check the result", "done": false }
]
````

**YOU — in VS Code: create and save `template/tests/baseline.test.mjs` with exactly these contents.**

````javascript
import test from 'node:test';
import assert from 'node:assert/strict';
import { spawnSync } from 'node:child_process';
import { fileURLToPath } from 'node:url';
import { renderTasks } from '../app.mjs';

const tasks = [{ id: 1, title: 'A', done: true }, { id: 2, title: 'B', done: false }, { id: 3, title: 'C', done: false }];
const app = fileURLToPath(new URL('../app.mjs', import.meta.url));
const cli = args => spawnSync(process.execPath, [app, ...args], { encoding: 'utf8', timeout: 10000 });
test('B1: existing list', () => assert.equal(renderTasks(tasks), '1. [x] A\n2. [ ] B\n3. [ ] C\n'));
test('B2: existing summary', () => assert.equal(renderTasks(tasks, { summary: true }), 'Total: 3\nOpen: 2\nDone: 1\n'));
test('B3: empty list', () => assert.equal(renderTasks([]), 'No tasks.\n'));
test('B4: empty summary', () => assert.equal(renderTasks([], { summary: true }), 'Total: 0\nOpen: 0\nDone: 0\n'));
test('B5: no input mutation', () => { const before = JSON.stringify(tasks); renderTasks(tasks, { summary: true }); assert.equal(JSON.stringify(tasks), before); });
test('B6: CLI no arguments', () => { const p = cli([]); assert.equal(p.status, 0); assert.equal(p.stderr, ''); assert.equal(p.stdout, '1. [x] Read the request\n2. [ ] Make the change\n3. [ ] Check the result\n'); });
test('B7: CLI summary', () => { const p = cli(['--summary']); assert.equal(p.status, 0); assert.equal(p.stderr, ''); assert.equal(p.stdout, 'Total: 3\nOpen: 2\nDone: 1\n'); });
test('B8: unknown option remains an error', () => { const p = cli(['--typo']); assert.equal(p.status, 2); assert.equal(p.stdout, ''); assert.match(p.stderr, /^Usage:/); });
````

**YOU — in VS Code: create and save `template/tests/feature.test.mjs` with exactly these contents.**

````javascript
import test from 'node:test';
import assert from 'node:assert/strict';
import { spawnSync } from 'node:child_process';
import { fileURLToPath } from 'node:url';
import { renderTasks } from '../app.mjs';

const tasks = [{ id: 1, title: 'A', done: true }, { id: 2, title: 'B', done: false }, { id: 3, title: 'C', done: false }];
const app = fileURLToPath(new URL('../app.mjs', import.meta.url));
const cli = args => spawnSync(process.execPath, [app, ...args], { encoding: 'utf8', timeout: 10000 });
test('F1: open listing', () => assert.equal(renderTasks(tasks, { status: 'open' }), '2. [ ] B\n3. [ ] C\n'));
test('F2: done listing', () => assert.equal(renderTasks(tasks, { status: 'done' }), '1. [x] A\n'));
test('F3: open summary counts filtered tasks', () => assert.equal(renderTasks(tasks, { status: 'open', summary: true }), 'Total: 2\nOpen: 2\nDone: 0\n'));
test('F4: done summary counts filtered tasks', () => assert.equal(renderTasks(tasks, { status: 'done', summary: true }), 'Total: 1\nOpen: 0\nDone: 1\n'));
test('F5: no matching tasks', () => assert.equal(renderTasks([tasks[1]], { status: 'done' }), 'No tasks.\n'));
test('F6: empty filtered summary', () => assert.equal(renderTasks([], { status: 'open', summary: true }), 'Total: 0\nOpen: 0\nDone: 0\n'));
test('F7: preserve order and IDs', () => assert.equal(renderTasks([tasks[2], tasks[1], tasks[0]], { status: 'open' }), '3. [ ] C\n2. [ ] B\n'));
test('F8: filtered rendering does not mutate input', () => { const before = JSON.stringify(tasks); renderTasks(tasks, { status: 'done', summary: true }); assert.equal(JSON.stringify(tasks), before); });
test('F9: CLI open listing', () => { const p = cli(['--status', 'open']); assert.equal(p.status, 0); assert.equal(p.stderr, ''); assert.equal(p.stdout, '2. [ ] Make the change\n3. [ ] Check the result\n'); });
test('F10: CLI summary then status', () => { const p = cli(['--summary', '--status', 'done']); assert.equal(p.status, 0); assert.equal(p.stderr, ''); assert.equal(p.stdout, 'Total: 1\nOpen: 0\nDone: 1\n'); });
test('F11: CLI status then summary', () => { const p = cli(['--status', 'open', '--summary']); assert.equal(p.status, 0); assert.equal(p.stderr, ''); assert.equal(p.stdout, 'Total: 2\nOpen: 2\nDone: 0\n'); });
test('F12: missing, invalid and repeated status fail', () => {
  for (const args of [['--status'], ['--status', 'OPEN'], ['--status', 'all'], ['--status', 'blocked'], ['--status', 'open', '--status', 'done']]) {
    const p = cli(args);
    assert.equal(p.status, 2, JSON.stringify(args));
    assert.equal(p.stdout, '', JSON.stringify(args));
    assert.equal(p.stderr, 'Usage: node app.mjs [--summary] [--status open|done]\n', JSON.stringify(args));
  }
});
````

**YOU — in VS Code: create and save `template/docs/usage.md` with exactly these contents.**

````markdown
# Practice task list

Run `node app.mjs` to list all tasks in the supplied fixture.

Run `node app.mjs --summary` to show total, open and done counts.

Unknown options produce a usage message on stderr and exit code 2.

The supplied file is fictional training data. No network service is used.
````

The eight B1-B8 cases preserve existing behaviour. The twelve F1-F12 cases define the new change; some intentionally fail in the starting program. F12 contains several CLI assertions inside one test, so the total is **20 tests**.

Both arrangements may read the same tests. This is a visible practice benchmark, not a hidden test set or evidence of general performance on unseen tasks. Do not let either assistant weaken the fixed assertions.

## Step 3 — Create the original request, stage contracts and layouts

**YOU — in VS Code: create and save `template/requests/change.md` with exactly these contents.**

````markdown
# G9-REQUEST-001: filter the task list

R1. Add optional `--status open` and `--status done`. Values are exact and case-sensitive. No filter preserves the existing list and summary.

R2. A filtered listing keeps each task's original ID, title and order. No matching tasks prints `No tasks.` followed by a newline.

R3. `--summary` counts only matching tasks when used with a status filter. Both option orders must work. Empty matches show all three counts as 0.

R4. A missing/invalid/repeated status, or unknown option, prints only `Usage: node app.mjs [--summary] [--status open|done]` plus newline to stderr and exits 2. CLI `--status all` is invalid; internal rendering may use `all` as the default.

R5. Preserve `renderTasks(tasks, options = {})`, do not mutate task data, and keep all eight existing tests passing. Use the supplied boolean `done` fixtures; general malformed records are outside scope.

R6. Update `docs/usage.md` with both filters, both summary combinations, their sample results, preserved behaviour and error handling. Do not claim wider OS testing or native permission enforcement.

R7. Change only `app.mjs` and `docs/usage.md`. Do not edit tests, instructions, tools, configuration, data, another run or any external service. The human runs all application/test/helper commands.

R8. Return the same three stage outputs in either arrangement: a plan, an implementation receipt and a final source/evidence review. Essential inputs missing or inconsistent means BLOCKED. A reported success needs current checks and human inspection. Do not invent tool counts, usage, timing, model identity or test execution.
````

**YOU — in VS Code: create and save `template/roles/common.md` with exactly these contents.**

````markdown
# Shared evaluation rules: G9-COMMON-v1

Read AGENTS.md, the original request, your stage contract and the current supplied files/evidence. State run ID, actual or supplied project root, request ID, accepted inputs and blockers.

All stages use the same task and fixed tests. Return the requested complete document in chat. The human saves records outside the project. Do not create reports, logs or receipts in project files.

No application/test/helper/Git commands, installation, external service, cross-run access or unassigned edits. Reading/searching and authorised source edits are sufficient. Do not use an earlier run's answer or evidence.

Inputs/evidence outside the root are supplied explicitly; report missing input instead of broadening permissions. The human runs checks. Distinguish supplied results from checks you performed; no independent authentication claim.

No extra delegation by a specialist. Stop after your stage output. If a shared/protected change is needed, report BLOCKED. The human permits at most one correction episode across the whole run.
````

**YOU — in VS Code: create and save `template/roles/planner.md` with exactly these contents.**

````markdown
# Planner: G9-PLAN-v1

Read the original request, current app/data/usage and both fixed test files. Inspect the supplied initial version. Do not edit any file or solve the task by supplying completed source.

Return templates/plan.md with R1-R8 mapped to intended changes and fixed checks, the two owned paths, ordered implementation/review stages, required handover inputs and stopping conditions. Include exact run/request IDs and supplied version evidence. Do not invent a hash.
````

**YOU — in VS Code: create and save `template/roles/implementer.md` with exactly these contents.**

````markdown
# Implementer: G9-IMPLEMENT-v1

Accept only a complete current plan plus the original request/current source/tests. Verify run/request identity, owned paths and the supplied version against the assignment. Missing/conflicting inputs means BLOCKED before editing.

Change only app.mjs and docs/usage.md to fulfil R1-R7. Preserve fixed tests and data. Return templates/implementation.md naming actual changes and R1-R8 mapping. No terminal/application/test commands. Mark checks NOT RUN BY THIS AGENT; the human will collect current results. Stop after the receipt.
````

**YOU — in VS Code: create and save `template/roles/reviewer.md` with exactly these contents.**

````markdown
# Reviewer: G9-REVIEW-v1

Accept only the original request, current plan, implementation receipt, complete current app/usage/data/tests, saved version and human-run check bundle. Verify identities, named paths, current evidence and claimed changes. Missing/stale/conflicting essentials means BLOCKED.

Inspect source logic and documentation against R1-R8, fixed tests, scope and actual supplied evidence. A passing test count alone is insufficient. Return templates/review.md with COMPLETE_LOCAL, PARTIAL or BLOCKED, requirement mapping, evidence runner, defects, unresolved work and limits. Do not edit or run commands. Do not invent a benchmark winner.
````

**YOU — in VS Code: create and save `template/templates/plan.md` with exactly these contents.**

````markdown
# Plan

Run ID:

Request ID:

Stage/role identity and actual or supplied project root:

Accepted or BLOCKED; missing/conflicting inputs:

Starting version: supplied evidence/initial-version.json; actual fingerprint details:

| Requirement | Intended change or boundary | Fixed check or human inspection |
| --- | --- | --- |
| R1-R8: use one row per requirement | | |

Only writable paths: app.mjs and docs/usage.md

Ordered stages and dependencies:

Required implementation handover inputs and stopping conditions:

Unresolved requirements and limits:
````

**YOU — in VS Code: create and save `template/templates/implementation.md` with exactly these contents.**

````markdown
# Implementation receipt

Run ID and request ID:

Stage/role identity and actual or supplied project root:

Accepted inputs, plan/version reference and identity checks:

Actual changed paths and change description:

R1-R8 mapping:

Checks: NOT RUN BY THIS AGENT; human checks still required

Reviewer handover needs: original request, plan, this receipt, current source/data/tests, version and complete human-run evidence

Blocked or unresolved work and limits:
````

**YOU — in VS Code: create and save `template/templates/review.md` with exactly these contents.**

````markdown
# Final workflow review

Run ID and request ID:

Stage/role identity and actual or supplied project root:

Decision: COMPLETE_LOCAL, PARTIAL or BLOCKED

Accepted current inputs, exact check bundle/version and missing/stale/conflicting inputs:

| Requirement | Current source or document evidence | Supplied check or inspection result |
| --- | --- | --- |
| R1-R8: use one row per requirement | | |

Source logic and documentation assessment:

Ownership, stage outputs and actual handovers:

Checks, runner and what this reviewer actually inspected:

Defects, unresolved work, unverified activities and bounded next action:

No claim of comparative speed, price or reliability without measured records
````

**YOU — in VS Code: create and save `template/AGENTS.md` with exactly these contents.**

````markdown
# Guide 9 practice project

Read roles/common.md and requests/change.md. The human identifies the run, arrangement and current stage. Follow roles/planner.md, roles/implementer.md or roles/reviewer.md for that stage.

SINGLE: one primary assistant carries out all three stages in the same conversation. No other agent, subagent or delegated chat. Only implementation may edit app.mjs and docs/usage.md.

MULTI: the primary coordinates exactly the named eval_planner, eval_implementer and eval_reviewer roles sequentially. It does not implement or replace a missing worker. Each specialist follows its own stage contract. Wait for the human's current inputs/checks between stages. Close completed spawned threads before the next invocation when required by the cap. No parallel writers or nested agents.

For attachment-based MULTI, the human relays complete current stage packets between fresh role chats. Perform only the assigned stage and do not claim native delegation. Attachment-based SINGLE keeps all three stages in one chat. The supplied run ID's single/multi suffix identifies the arrangement.

All stage outputs return in chat. The human owns timer, observations, receipts, checks and assessment outside project/. Instructions are task boundaries; they do not grant permissions or prove enforcement.

No application/tests/helper/Git commands, installation, external services, other-run access or protected-file edits. Missing inputs or unavailable exact profiles must be reported; do not simulate invocation. At most one human-authorised correction episode per run. Stop when the current stage is complete or blocked.
````

**YOU — in VS Code: create and save `template/.github/copilot-instructions.md` with exactly these contents.**

````markdown
# Shared Copilot project instructions

Follow [AGENTS.md](../AGENTS.md), [shared rules](../roles/common.md), the original request and the current stage contract. The human supplies the run identity and evidence. Use the selected single/team profile and permitted tools. No commands, cross-run access, fabricated checks or extra delegation.
````

**YOU — in VS Code: create and save `template/.gitignore` with exactly these contents.**

````text
.DS_Store
node_modules/
````

The requested outputs and final criteria are the same in both arrangements. SINGLE changes stage in one conversation. MULTI transfers complete inputs to a separately invoked specialist. A handover must carry the original request, run identity, current version and stage evidence, rather than just “the other agent says it is fine”.

## Step 4 — Add the native-profile and evaluation helpers

**YOU — in VS Code: create and save `template/tools/build-agents.mjs` with exactly these contents.**

````javascript
import { existsSync, mkdirSync, readFileSync, writeFileSync } from 'node:fs';
import { dirname, join } from 'node:path';
import { fileURLToPath } from 'node:url';

const root = dirname(dirname(fileURLToPath(import.meta.url)));
const readTools = ['read/readFile', 'search/listDirectory'];
const roles = ['planner', 'implementer', 'reviewer'];
const outputs = new Map();
try {
  readFileSync(join(root, 'roles/common.md'), 'utf8');
  for (const role of roles) {
    readFileSync(join(root, `roles/${role}.md`), 'utf8');
    const name = 'eval_' + role;
    const sandbox = role === 'implementer' ? 'workspace-write' : 'read-only';
    outputs.set(`.codex/agents/${name}.toml`, `name = "${name}"
description = "Guide 9 ${role}; follow the assigned stage and return its complete output."
sandbox_mode = "${sandbox}"
approval_policy = "never"
web_search = "disabled"
developer_instructions = """
You are ${name}. Read AGENTS.md, roles/common.md, roles/${role}.md,
requests/change.md and the supplied current run inputs. Follow your stage.
No application/test/helper/Git commands, external services, cross-run access
or delegation. Return the complete stage output in chat. Missing required
inputs means BLOCKED. Do not invent checks, identities or benchmark metrics.
"""

[agents]
enabled = false
` + (sandbox === 'workspace-write' ? '\n[sandbox_workspace_write]\nnetwork_access = false\n' : ''));
  }
  const definitions = [
    { name: 'eval_single', contract: 'all three stage contracts', tools: [...readTools, 'edit/editFiles'], agents: [], picker: true, disable: true },
    { name: 'eval_team', contract: 'AGENTS.md multi-stage coordinator rules', tools: [...readTools, 'agent/runSubagent'], agents: roles.map(role => 'eval_' + role), picker: true, disable: true },
    ...roles.map(role => ({ name: 'eval_' + role, contract: `roles/${role}.md`, tools: role === 'implementer' ? [...readTools, 'edit/editFiles'] : readTools, agents: [], picker: false, disable: false }))
  ];
  for (const d of definitions) {
    outputs.set(`.github/agents/${d.name}.agent.md`, `---
name: ${d.name}
description: "Guide 9 bounded ${d.name} workflow profile."
tools: ${JSON.stringify(d.tools)}
agents: ${JSON.stringify(d.agents)}
user-invocable: ${d.picker}
disable-model-invocation: ${d.disable}
target: vscode
---

# ${d.name}

Read [project rules](../../AGENTS.md), [shared rules](../../roles/common.md)
and [request](../../requests/change.md). Follow ${d.contract} in the current
project, with explicitly supplied run identity, stage and current evidence.
Read the relevant [planner](../../roles/planner.md),
[implementer](../../roles/implementer.md) or [reviewer](../../roles/reviewer.md)
contract. Return stage outputs in chat. No command execution, external service,
cross-run access, invented checks or unassigned edits.
`);
  }
  for (const name of outputs.keys()) if (existsSync(join(root, name))) throw new Error('Already exists; nothing replaced: ' + name);
  for (const [name, contents] of outputs) {
    mkdirSync(dirname(join(root, name)), { recursive: true });
    writeFileSync(join(root, name), contents, { flag: 'wx' });
    console.log('CREATED: ' + name);
  }
  console.log('READY: 3 Codex worker profiles and 5 Local Copilot profiles.');
} catch (error) {
  console.error('FAIL: ' + error.message);
  process.exitCode = 1;
}
````

**YOU — in VS Code: create and save `template/tools/benchmark.mjs` with exactly these contents.**

````javascript
import { cpSync, existsSync, lstatSync, mkdirSync, readdirSync, readFileSync, writeFileSync } from 'node:fs';
import { createHash } from 'node:crypto';
import { basename, dirname, join, resolve } from 'node:path';
import { fileURLToPath } from 'node:url';
import { spawnSync } from 'node:child_process';

const root = dirname(dirname(fileURLToPath(import.meta.url)));
const templateMode = basename(root) === 'template';
const runRoot = dirname(root);
const parent = templateMode ? dirname(root) : dirname(dirname(runRoot));
const template = join(parent, 'template');
const runId = basename(runRoot);
const match = /^pair-([1-3])-(single|multi)$/.exec(runId);
const allowed = ['app.mjs', 'docs/usage.md'];
const sha = value => createHash('sha256').update(value).digest('hex');
const digest = value => sha(JSON.stringify(value));
const readJSON = path => JSON.parse(readFileSync(path, 'utf8'));
const same = (a, b) => JSON.stringify(a) === JSON.stringify(b);
const fail = message => { throw new Error(message); };
function save(path, value) {
  if (existsSync(path)) fail('Already exists; nothing replaced: ' + path);
  mkdirSync(dirname(path), { recursive: true });
  writeFileSync(path, typeof value === 'string' ? value : JSON.stringify(value, null, 2) + '\n', { flag: 'wx' });
}
function files(base) {
  const result = {};
  function visit(relative = '') {
    for (const name of readdirSync(join(base, relative)).sort()) {
      if (name === '.DS_Store') continue;
      const path = relative ? relative + '/' + name : name;
      const stat = lstatSync(join(base, path));
      if (stat.isSymbolicLink()) fail('Symlink is outside this fixture: ' + path);
      if (stat.isDirectory()) visit(path);
      else if (stat.isFile()) result[path] = sha(readFileSync(join(base, path)));
      else fail('Not a regular file: ' + path);
    }
  }
  visit();
  return Object.fromEntries(Object.entries(result).sort(([a], [b]) => a.localeCompare(b)));
}
export function codexConfig(arm) {
  return `sandbox_mode = "workspace-write"
approval_policy = "never"
web_search = "disabled"

[sandbox_workspace_write]
network_access = false

[agents]
enabled = ${arm === 'multi'}
` + (arm === 'multi' ? 'max_concurrent_threads_per_session = 1\n' : '');
}
function experiment() {
  const e = readJSON(join(parent, 'experiment.json'));
  if (e.schema !== 'G9-EXPERIMENT-v1' || !['codex-native', 'copilot-local', 'attachments'].includes(e.route)) fail('Choose an exact experiment route before freezing.');
  for (const key of ['hostVersion', 'primaryModel', 'reasoningEffort']) {
    if (typeof e[key] !== 'string' || !e[key].trim() || /^(record-|choose-)/.test(e[key])) fail('Record actual ' + key + ' or not-visible.');
  }
  if (![1, 2, 3].includes(e.plannedPairs) || e.maxRepairs !== 1 || !Number.isInteger(e.maxMinutesPerRun) || e.maxMinutesPerRun < 1 || e.maxMinutesPerRun > 60) fail('Invalid fixed experiment limits.');
  if (e.sharedSetupSeconds !== null && (!Number.isFinite(e.sharedSetupSeconds) || e.sharedSetupSeconds < 0)) fail('Invalid setup time.');
  return e;
}
function baseline() {
  const b = readJSON(join(parent, 'baseline.json'));
  if (b.schema !== 'G9-BASELINE-v1' || !same(files(template), b.files) || digest(experiment()) !== b.experimentHash) fail('Template or experiment changed after freezing.');
  return b;
}
function context() {
  if (templateMode || basename(root) !== 'project' || !match) fail('Run this command from a prepared run project.');
  const b = baseline();
  if (Number(match[1]) > experiment().plannedPairs) fail('Run is outside the planned pairs.');
  return { pair: Number(match[1]), arm: match[2], baseline: b };
}
function expected(c) {
  return Object.fromEntries(Object.entries({ ...c.baseline.files, '.codex/config.toml': sha(codexConfig(c.arm)) }).sort(([a], [b]) => a.localeCompare(b)));
}
function guard() {
  const c = context();
  const initial = expected(c), current = files(root);
  for (const name of new Set([...Object.keys(initial), ...Object.keys(current)])) {
    if (!allowed.includes(name) && current[name] !== initial[name]) fail('Outside ownership: ' + name);
    if (allowed.includes(name) && !current[name]) fail('Required file missing: ' + name);
  }
  return c;
}
function version() { guard(); return { schema: 'G9-VERSION-v1', runId, files: files(root) }; }
function observation(id, pair, arm) {
  return { schema: 'G9-OBSERVATION-v1', runId: id, pair, arm,
    architectureConfirmed: null, comparability: 'uncertain', actualModels: [],
    repairAttempts: 0,
    humanReview: { sourceAndDocs: null, boundariesAndClaims: null, stageArtifacts: null, evidence: '' },
    handovers: { attempted: null, failed: null, evidence: '', notes: '' },
    toolCalls: { coverage: 'unavailable', calls: [], notes: '' },
    usage: { cost: { coverage: 'unavailable', amount: null, unit: 'not-exposed', basis: 'not-exposed', evidence: '' },
      tokens: { coverage: 'unavailable', input: null, cachedInput: null, output: null, evidence: '' } },
    humanActiveSeconds: null, notes: '' };
}
function execute(args) {
  const p = spawnSync(process.execPath, args, { cwd: root, encoding: 'utf8', timeout: 60000, maxBuffer: 4 * 1024 * 1024 });
  return { command: ['node', ...args], exit: p.status, stdout: p.stdout ?? '', stderr: p.stderr ?? '', error: p.error?.message ?? null };
}
function counts(stdout) {
  const number = label => Number(new RegExp('^# ' + label + ' (\\d+)$', 'm').exec(stdout)?.[1] ?? NaN);
  return { tests: number('tests'), pass: number('pass'), fail: number('fail') };
}
function active() {
  context();
  if (!existsSync(join(runRoot, 'timing-start.json')) || existsSync(join(runRoot, 'timing-end.json'))) fail('Run is not actively timed.');
}
function check(attempt) {
  active();
  if (!['first', 'repair'].includes(attempt)) fail('Attempt must be first or repair.');
  const target = join(runRoot, 'evidence', attempt);
  if (existsSync(target)) fail('Evidence already exists; nothing replaced.');
  mkdirSync(target, { recursive: true });
  const before = files(root);
  let ownership = { passed: true, detail: 'Only the two assigned files may differ.' };
  try { guard(); } catch (error) { ownership = { passed: false, detail: error.message }; }
  const result = ownership.passed ? execute(['--test', '--test-reporter=tap', 'tests/baseline.test.mjs', 'tests/feature.test.mjs']) : { command: [], exit: null, stdout: '', stderr: 'Tests not run: ownership failed.', error: null };
  const after = files(root), tally = counts(result.stdout);
  const stable = same(before, after);
  save(join(target, 'source-version.json'), { schema: 'G9-VERSION-v1', runId, files: before });
  save(join(target, 'tests.stdout.txt'), result.stdout);
  save(join(target, 'tests.stderr.txt'), result.stderr);
  save(join(target, 'ownership.json'), ownership);
  const bundleHashes = files(target);
  const passed = ownership.passed && result.exit === 0 && tally.tests === 20 && tally.pass === 20 && tally.fail === 0 && stable;
  save(join(target, 'checks.json'), { schema: 'G9-CHECKS-v1', runId, attempt, runner: 'human local command', passed, ownership, result: { command: result.command, exit: result.exit, error: result.error }, counts: tally, stable, bundleHashes });
  console.log(`${passed ? 'PASS' : 'FAIL'}: ${runId} ${attempt}; tests ${tally.pass}/${tally.tests}; ownership ${ownership.passed}; stable ${stable}`);
  console.log('Evidence: ' + target);
  if (!passed) process.exitCode = 1;
}
function currentCheck(attempt) {
  if (!['first', 'repair'].includes(attempt)) fail('Name first or repair check evidence.');
  const target = join(runRoot, 'evidence', attempt), c = readJSON(join(target, 'checks.json'));
  const actual = files(target); delete actual['checks.json'];
  const v = readJSON(join(target, 'source-version.json'));
  const tally = counts(readFileSync(join(target, 'tests.stdout.txt'), 'utf8'));
  if (c.runId !== runId || c.attempt !== attempt || v.runId !== runId || !same(actual, c.bundleHashes) || !same(v.files, files(root))) fail('Check bundle is altered, from another run or stale.');
  guard();
  return c.passed === true && c.result.exit === 0 && c.stable === true && tally.tests === 20 && tally.pass === 20 && tally.fail === 0;
}
function integer(value) { return Number.isInteger(value) && value >= 0; }
function validateObservation(o, c) {
  if (o.schema !== 'G9-OBSERVATION-v1' || o.runId !== runId || o.pair !== c.pair || o.arm !== c.arm) fail('Observation has wrong run identity.');
  if (!['matched', 'uncertain', 'confounded'].includes(o.comparability) || !integer(o.repairAttempts)) fail('Invalid comparability/repair record.');
  if (![true, false, null].includes(o.architectureConfirmed)) fail('Invalid architecture observation.');
  for (const key of ['sourceAndDocs', 'boundariesAndClaims', 'stageArtifacts']) if (![true, false, null].includes(o.humanReview[key])) fail('Use true, false or null for human gates.');
  if (typeof o.humanReview.evidence !== 'string') fail('Human review evidence must be text.');
  const h = o.handovers;
  if ((h.attempted === null) !== (h.failed === null) || (h.attempted !== null && (!integer(h.attempted) || !integer(h.failed) || h.failed > h.attempted))) fail('Invalid handover counts.');
  if (c.arm === 'single' && h.attempted !== null && (h.attempted !== 0 || h.failed !== 0)) fail('Single has no cross-agent handovers.');
  const t = o.toolCalls;
  if (!['complete', 'partial', 'unavailable'].includes(t.coverage) || !Array.isArray(t.calls)) fail('Invalid tool coverage.');
  const ids = new Set();
  for (const call of t.calls) {
    if (typeof call.id !== 'string' || !call.id || ids.has(call.id) || ![true, false, null].includes(call.necessary) || typeof call.failed !== 'boolean') fail('Invalid or duplicate tool-call row.');
    for (const key of ['stage', 'actor', 'kind', 'reason', 'evidence']) if (typeof call[key] !== 'string' || !call[key].trim()) fail('Missing tool row ' + key);
    ids.add(call.id);
  }
  const cost = o.usage.cost;
  if (!['complete', 'partial', 'unavailable'].includes(cost.coverage)) fail('Invalid cost coverage.');
  if (cost.amount !== null && (!Number.isFinite(cost.amount) || cost.amount < 0)) fail('Invalid cost amount.');
  if (cost.coverage === 'complete' && (cost.amount === null || !['reported', 'estimate'].includes(cost.basis) || !cost.evidence || cost.unit === 'not-exposed')) fail('Complete cost needs amount, unit, basis and evidence.');
  const tokens = o.usage.tokens;
  if (!['complete', 'partial', 'unavailable'].includes(tokens.coverage)) fail('Invalid token coverage.');
  for (const key of ['input', 'cachedInput', 'output']) if (tokens[key] !== null && !integer(tokens[key])) fail('Invalid token count.');
  if (tokens.cachedInput !== null && tokens.input !== null && tokens.cachedInput > tokens.input) fail('Cached input is a subset of total input.');
  if (tokens.coverage === 'complete' && (!integer(tokens.input) || !integer(tokens.cachedInput) || !integer(tokens.output) || !tokens.evidence)) fail('Complete tokens need all counts/evidence.');
  if (o.humanActiveSeconds !== null && (!Number.isFinite(o.humanActiveSeconds) || o.humanActiveSeconds < 0)) fail('Invalid active human time.');
}
function grade() {
  const c = context();
  const end = readJSON(join(runRoot, 'timing-end.json'));
  const o = readJSON(join(runRoot, 'observations.json'));
  validateObservation(o, c);
  if (end.attempt === 'repair' && o.repairAttempts < 1) fail('Repair evidence requires recording the correction episode.');
  let application = false, reason = 'No complete current check bundle.';
  try { application = Boolean(end.attempt) && currentCheck(end.attempt); reason = application ? 'Current 20-test/scope/stability evidence passes.' : reason; } catch (error) { reason = error.message; }
  const stageNames = ['plan.md', 'implementation.md', 'review.md'];
  const stageFiles = stageNames.every(name => existsSync(join(runRoot, 'stages', name)) && readFileSync(join(runRoot, 'stages', name), 'utf8').includes(runId) && readFileSync(join(runRoot, 'stages', name), 'utf8').includes('G9-REQUEST-001'));
  const h = o.handovers;
  const handoversKnown = h.attempted !== null && (c.arm === 'single' ? h.attempted === 0 : h.attempted >= 2) && (c.arm === 'single' || Boolean(h.evidence));
  const human = o.humanReview;
  const accepted = end.outcome === 'finished' && end.withinTimeLimit && application && stageFiles && human.sourceAndDocs === true && human.boundariesAndClaims === true && human.stageArtifacts === true && Boolean(human.evidence.trim()) && o.architectureConfirmed === true && handoversKnown && o.repairAttempts <= 1;
  const t = o.toolCalls, cost = o.usage.cost;
  const row = { runId, pair: c.pair, arm: c.arm, started: true, finalized: true, synthetic: false,
    accepted, firstPass: accepted && o.repairAttempts === 0 && end.attempt === 'first', applicationPass: application,
    elapsedSeconds: end.elapsedSeconds, humanActiveSeconds: o.humanActiveSeconds,
    comparability: o.comparability, toolCoverage: t.coverage,
    toolTotal: t.coverage === 'complete' ? t.calls.length : null,
    observedTools: t.calls.length,
    unnecessaryTools: t.coverage === 'complete' && t.calls.every(call => call.necessary !== null) ? t.calls.filter(call => call.necessary === false).length : null,
    handoverAttempts: h.attempted, handoverFailures: h.failed,
    cost: cost.coverage === 'complete' ? cost.amount : null, costUnit: cost.unit, costBasis: cost.basis, costCoverage: cost.coverage,
    reason, humanGates: human, stageFiles, architectureConfirmed: o.architectureConfirmed, withinTimeLimit: end.withinTimeLimit,
    note: o.notes };
  const audit = files(runRoot); for (const name of Object.keys(audit)) if (name.startsWith('project/')) delete audit[name];
  save(join(runRoot, 'grade.json'), { schema: 'G9-GRADE-v1', row, projectHash: digest(files(root)), auditHashes: audit });
  console.log(`${accepted ? 'VERIFIED_SUCCESS' : 'NOT_VERIFIED_SUCCESS'}: ${runId}`);
  console.log('Application checks: ' + application + '; human gates/architecture/stages/handovers/time/repair allowance also apply.');
}
export function median(values) {
  if (!values.length) return null;
  const a = [...values].sort((x, y) => x - y), m = Math.floor(a.length / 2);
  return a.length % 2 ? a[m] : (a[m - 1] + a[m]) / 2;
}
export function summarise(rows, arm) {
  const a = rows.filter(row => row.arm === arm && row.started), success = a.filter(row => row.accepted).length;
  const costs = a.filter(row => row.cost !== null && row.cost !== undefined && row.costBasis === 'reported');
  const units = new Set(costs.map(row => row.costUnit));
  const completeCost = a.length > 0 && a.every(row => row.finalized) && costs.length === a.length && units.size === 1;
  const handovers = a.filter(row => row.handoverAttempts !== null && row.handoverAttempts !== undefined);
  return { started: a.length, graded: a.filter(row => row.finalized).length, successes: success,
    firstPass: a.filter(row => row.firstPass).length,
    medianSeconds: median(a.map(row => row.elapsedSeconds).filter(Number.isFinite)), timeCoverage: a.filter(row => Number.isFinite(row.elapsedSeconds)).length,
    medianTools: median(a.map(row => row.toolTotal).filter(Number.isFinite)), toolCoverage: a.filter(row => Number.isFinite(row.toolTotal)).length,
    unnecessaryTotal: a.length && a.every(row => Number.isFinite(row.unnecessaryTools)) ? a.reduce((sum, row) => sum + row.unnecessaryTools, 0) : null,
    handoverAttempts: handovers.reduce((sum, row) => sum + row.handoverAttempts, 0), handoverFailures: handovers.reduce((sum, row) => sum + row.handoverFailures, 0), handoverCoverage: handovers.length,
    costCoverage: costs.length, totalCost: completeCost ? costs.reduce((sum, row) => sum + row.cost, 0) : null,
    costUnit: completeCost ? [...units][0] : null,
    costPerSuccess: completeCost && success > 0 ? costs.reduce((sum, row) => sum + row.cost, 0) / success : null };
}
const show = value => value === null || value === undefined ? 'unknown' : typeof value === 'number' ? String(Math.round(value * 10000) / 10000) : String(value).replace(/\|/g, '\\|').replace(/\n/g, ' ');
export function renderReport(rows, synthetic = false) {
  const summary = { single: summarise(rows, 'single'), multi: summarise(rows, 'multi') };
  const out = ['# ' + (synthetic ? 'SYNTHETIC comparison — not observed AI runs' : 'Observed workflow comparison'), '', synthetic ? '**All values below are fictional teaching data, including credits. They establish no performance, reliability or price claim about real assistants.**' : 'Only saved observations and current graded evidence support this report. Missing metrics remain unknown. Human observations are not authenticated telemetry.', '', '| Run | Accepted | First pass | Wall seconds | Tool total | Unnecessary | Handover failures/attempts | Cost | Comparability |', '| --- | --- | --- | --- | --- | --- | --- | --- | --- |'];
  for (const r of rows) out.push(`| ${r.runId} | ${r.finalized ? r.accepted : 'ungraded/not started'} | ${r.finalized ? r.firstPass : 'unknown'} | ${show(r.elapsedSeconds)} | ${show(r.toolTotal)} | ${show(r.unnecessaryTools)} | ${r.handoverAttempts === null || r.handoverAttempts === undefined ? 'unknown' : r.handoverFailures + '/' + r.handoverAttempts} | ${r.cost === null || r.cost === undefined ? 'unknown' : show(r.cost) + ' ' + show(r.costUnit) + ' (' + r.costBasis + ')'} | ${show(r.comparability)} |`);
  out.push('', '## Measures and coverage', '', '| Measure | Single | Multi |', '| --- | --- | --- |');
  function metric(label, fn) { out.push(`| ${label} | ${fn(summary.single)} | ${fn(summary.multi)} |`); }
  metric('Started / graded', s => `${s.started} / ${s.graded}`);
  metric('Verified successes / started', s => `${s.successes}/${s.started}; ungraded attempts remain unresolved`);
  metric('First-pass successes / started', s => `${s.firstPass}/${s.started}`);
  metric('Median wall seconds', s => `${show(s.medianSeconds)}; coverage ${s.timeCoverage}/${s.started}`);
  metric('Median total tools', s => `${show(s.medianTools)}; coverage ${s.toolCoverage}/${s.started}`);
  metric('Unnecessary tools across attempts', s => show(s.unnecessaryTotal));
  metric('Failed / attempted cross-agent handovers', s => `${s.handoverAttempts ? s.handoverFailures + '/' + s.handoverAttempts : 'not applicable/no observed attempts'}; coverage ${s.handoverCoverage}/${s.started}`);
  metric('Reported complete cost across attempts', s => `${show(s.totalCost)} ${s.costUnit ?? ''}; coverage ${s.costCoverage}/${s.started}`);
  metric('Reported cost per verified success', s => `${show(s.costPerSuccess)} ${s.costUnit ?? ''}`);
  out.push('', '## Matched pairs', '', '| Pair | Multi minus single wall seconds | Both accepted? | Multi minus single reported cost |', '| --- | --- | --- | --- |');
  const pairs = [];
  for (const n of [1, 2, 3]) {
    const single = rows.find(r => r.pair === n && r.arm === 'single'), multi = rows.find(r => r.pair === n && r.arm === 'multi');
    if (!single?.finalized || !multi?.finalized || single.comparability !== 'matched' || multi.comparability !== 'matched') continue;
    const delta = Number.isFinite(single.elapsedSeconds) && Number.isFinite(multi.elapsedSeconds) ? multi.elapsedSeconds - single.elapsedSeconds : null;
    const comparableCost = Number.isFinite(single.cost) && Number.isFinite(multi.cost) && single.costUnit === multi.costUnit && single.costBasis === 'reported' && multi.costBasis === 'reported';
    out.push(`| ${n} | ${show(delta)} | ${single.accepted && multi.accepted} | ${comparableCost ? show(multi.cost - single.cost) + ' ' + show(single.costUnit) : 'unknown/not comparable'} |`);
    pairs.push(delta);
  }
  if (!pairs.length) out.push('| None | unknown | unknown | unknown |');
  out.push('', 'Median matched-pair wall difference: ' + show(median(pairs.filter(Number.isFinite))) + ' seconds. Positive means multi took longer within this measured boundary.', '', 'Do not trade away success criteria for speed. Examine failures, repairs, coverage, model/host differences and active human/setup time before choosing an arrangement. A few repeats on one visible task are practice evidence, not a general reliability estimate.');
  return { markdown: out.join('\n') + '\n', summary };
}
function report(tag, synthetic) {
  if (!/^[a-z][a-z0-9-]{0,30}$/.test(tag ?? '')) fail('Choose a short unused report name such as pilot or final.');
  let rows;
  if (synthetic) {
    const data = readJSON(join(template, 'examples/synthetic-results.json'));
    if (data.schema !== 'G9-SYNTHETIC-v1' || !data.rows.every(row => row.synthetic === true)) fail('Demo data must be explicitly synthetic.');
    rows = data.rows.map(row => ({ ...row, started: true, finalized: true }));
  } else {
    baseline(); rows = [];
    for (let pair = 1; pair <= experiment().plannedPairs; pair++) for (const arm of ['single', 'multi']) {
      const id = `pair-${pair}-${arm}`, folder = join(parent, 'runs', id);
      if (existsSync(join(folder, 'grade.json'))) {
        const g = readJSON(join(folder, 'grade.json')), audit = files(folder);
        for (const name of Object.keys(audit)) if (name.startsWith('project/') || name === 'grade.json') delete audit[name];
        if (!same(audit, g.auditHashes) || digest(files(join(folder, 'project'))) !== g.projectHash) fail('Graded run changed; preserve and investigate: ' + id);
        rows.push(g.row);
      } else {
        const start = existsSync(join(folder, 'timing-start.json')), end = existsSync(join(folder, 'timing-end.json')) ? readJSON(join(folder, 'timing-end.json')) : null;
        rows.push({ runId: id, pair, arm, started: start, finalized: false, accepted: false, firstPass: false, elapsedSeconds: end?.elapsedSeconds ?? null, comparability: 'uncertain' });
      }
    }
  }
  const result = renderReport(rows, synthetic), location = join(parent, 'reports', (synthetic ? 'synthetic-' : '') + tag);
  if (existsSync(location + '.md') || existsSync(location + '.json')) fail('Report already exists; nothing replaced.');
  save(location + '.md', result.markdown);
  save(location + '.json', { synthetic, rows, summary: result.summary });
  console.log('REPORT: ' + location + '.md');
}
function main() {
  const [command, argument, extra] = process.argv.slice(2);
  if (command === 'freeze') {
    if (!templateMode) fail('Freeze only the template.');
    const e = experiment();
    if (existsSync(join(parent, 'baseline.json')) || existsSync(join(parent, 'setup-baseline.json'))) fail('Baseline already exists; nothing replaced.');
    const before = files(root), result = execute(['--test', '--test-reporter=tap', 'tests/baseline.test.mjs']);
    if (result.exit !== 0 || counts(result.stdout).tests !== 8 || !same(before, files(root))) fail('Starting baseline checks failed or changed files.');
    save(join(parent, 'setup-baseline.json'), result);
    save(join(parent, 'baseline.json'), { schema: 'G9-BASELINE-v1', files: before, experimentHash: digest(e) });
    console.log('PASS: frozen ' + Object.keys(before).length + ' common files and experiment; 8 baseline tests.');
  } else if (command === 'prepare') {
    if (!templateMode || !/^pair-[1-3]$/.test(argument ?? '')) fail('From template, prepare pair-1, pair-2 or pair-3.');
    baseline(); const pair = Number(argument.slice(-1));
    if (pair > experiment().plannedPairs) fail('Pair exceeds the plan.');
    for (const arm of ['single', 'multi']) if (existsSync(join(parent, 'runs', argument + '-' + arm))) fail('Run already exists; nothing replaced.');
    for (const arm of ['single', 'multi']) {
      const id = argument + '-' + arm, target = join(parent, 'runs', id);
      mkdirSync(join(target, 'evidence'), { recursive: true }); mkdirSync(join(target, 'stages'), { recursive: true });
      cpSync(template, join(target, 'project'), { recursive: true });
      save(join(target, 'project/.codex/config.toml'), codexConfig(arm));
      save(join(target, 'observations.json'), observation(id, pair, arm));
      console.log('CREATED: runs/' + id);
    }
  } else if (command === 'begin') {
    const c = guard();
    if (!same(files(root), expected(c))) fail('Start only from the unchanged prepared project.');
    if (existsSync(join(runRoot, 'timing-start.json')) || existsSync(join(runRoot, 'evidence/initial-version.json'))) fail('Start already recorded; nothing replaced.');
    save(join(runRoot, 'evidence/initial-version.json'), version());
    save(join(runRoot, 'timing-start.json'), { runId, startedAt: new Date().toISOString(), startMs: Date.now() });
    console.log('STARTED: ' + runId + '; send the first prepared stage prompt now.');
  } else if (command === 'check') check(argument);
  else if (command === 'match-check') { context(); if (!currentCheck(argument)) fail('Current checks did not pass.'); console.log('PASS: current source and complete check bundle match.'); }
  else if (command === 'end') {
    active();
    if (!['finished', 'failed', 'blocked', 'timeout'].includes(argument)) fail('End outcome must be finished, failed, blocked or timeout.');
    if (extra !== undefined && !['first', 'repair'].includes(extra)) fail('Optional final attempt must be first or repair.');
    const start = readJSON(join(runRoot, 'timing-start.json')), now = Date.now(), seconds = (now - start.startMs) / 1000;
    if (!Number.isFinite(seconds) || seconds < 0) fail('Clock record is invalid; investigate without inventing time.');
    save(join(runRoot, 'timing-end.json'), { runId, outcome: argument, attempt: extra ?? null, endedAt: new Date(now).toISOString(), elapsedSeconds: seconds, withinTimeLimit: seconds <= experiment().maxMinutesPerRun * 60 });
    console.log('ENDED: ' + runId + '; ' + seconds.toFixed(3) + ' wall seconds; requested outcome ' + argument + ' is not a success verdict.');
  } else if (command === 'grade') grade();
  else if (command === 'report' || command === 'demo') {
    if (!templateMode) fail('Create reports from the template terminal.');
    report(argument, command === 'demo');
  } else fail('Commands: freeze; prepare pair-N; begin; check first|repair; match-check first|repair; end outcome [attempt]; grade; report name; demo name.');
}
if (process.argv[1] && fileURLToPath(import.meta.url) === resolve(process.argv[1])) {
  try { main(); } catch (error) { console.error('FAIL: ' + error.message); process.exitCode = 1; }
}
````

**YOU — in VS Code: create and save `template/examples/synthetic-results.json` with exactly these contents.**

````json
{
  "schema": "G9-SYNTHETIC-v1",
  "warning": "SYNTHETIC DATA; NOT OBSERVED AI RUNS OR PRICES",
  "rows": [
    {
      "runId": "pair-1-single",
      "pair": 1,
      "arm": "single",
      "synthetic": true,
      "accepted": true,
      "firstPass": true,
      "elapsedSeconds": 180,
      "comparability": "matched",
      "toolTotal": 10,
      "unnecessaryTools": 1,
      "handoverAttempts": 0,
      "handoverFailures": 0,
      "cost": 3,
      "costUnit": "fictional-credits",
      "costBasis": "reported",
      "note": "Fictional data only. Pair 3 multi has passing code tests but incomplete documentation."
    },
    {
      "runId": "pair-1-multi",
      "pair": 1,
      "arm": "multi",
      "synthetic": true,
      "accepted": true,
      "firstPass": true,
      "elapsedSeconds": 260,
      "comparability": "matched",
      "toolTotal": 22,
      "unnecessaryTools": 3,
      "handoverAttempts": 2,
      "handoverFailures": 0,
      "cost": 8,
      "costUnit": "fictional-credits",
      "costBasis": "reported",
      "note": "Fictional data only. Pair 3 multi has passing code tests but incomplete documentation."
    },
    {
      "runId": "pair-2-single",
      "pair": 2,
      "arm": "single",
      "synthetic": true,
      "accepted": true,
      "firstPass": true,
      "elapsedSeconds": 160,
      "comparability": "matched",
      "toolTotal": 9,
      "unnecessaryTools": 0,
      "handoverAttempts": 0,
      "handoverFailures": 0,
      "cost": 2,
      "costUnit": "fictional-credits",
      "costBasis": "reported",
      "note": "Fictional data only. Pair 3 multi has passing code tests but incomplete documentation."
    },
    {
      "runId": "pair-2-multi",
      "pair": 2,
      "arm": "multi",
      "synthetic": true,
      "accepted": true,
      "firstPass": false,
      "elapsedSeconds": 400,
      "comparability": "matched",
      "toolTotal": 30,
      "unnecessaryTools": 7,
      "handoverAttempts": 3,
      "handoverFailures": 1,
      "cost": 12,
      "costUnit": "fictional-credits",
      "costBasis": "reported",
      "note": "Fictional data only. Pair 3 multi has passing code tests but incomplete documentation."
    },
    {
      "runId": "pair-3-single",
      "pair": 3,
      "arm": "single",
      "synthetic": true,
      "accepted": true,
      "firstPass": true,
      "elapsedSeconds": 200,
      "comparability": "matched",
      "toolTotal": 11,
      "unnecessaryTools": 1,
      "handoverAttempts": 0,
      "handoverFailures": 0,
      "cost": 3,
      "costUnit": "fictional-credits",
      "costBasis": "reported",
      "note": "Fictional data only. Pair 3 multi has passing code tests but incomplete documentation."
    },
    {
      "runId": "pair-3-multi",
      "pair": 3,
      "arm": "multi",
      "synthetic": true,
      "accepted": false,
      "firstPass": false,
      "elapsedSeconds": 240,
      "comparability": "matched",
      "toolTotal": 20,
      "unnecessaryTools": 4,
      "handoverAttempts": 2,
      "handoverFailures": 0,
      "cost": 8,
      "costUnit": "fictional-credits",
      "costBasis": "reported",
      "note": "Fictional data only. Pair 3 multi has passing code tests but incomplete documentation."
    }
  ]
}
````

### What the helper does, and what it does not prove

| Command | Purpose |
| --- | --- |
| `freeze` | Check eight existing tests; freeze common files and experiment |
| `prepare pair-N` | Create both fresh run copies and observation forms |
| `begin` | Require unchanged starting source; record version and wall-clock start |
| `check first` or `check repair` | Check scope, all 20 tests and stable source; preserve logs |
| `match-check first` or `match-check repair` | Compare the current project with the recorded complete check bundle |
| `end outcome [attempt]` | End the timer and identify the final evidence, without declaring success |
| `grade` | Combine current checks, saved stages and your manual gates |
| `report name` | Summarise recorded runs, coverage and comparable pairs |
| `demo name` | Process only the explicitly synthetic calculation dataset |

The helper uses content fingerprints, not a tamper-proof audit. It trusts your saved observations and manual decisions; it cannot authenticate model identity, measure hidden tool calls, assess every Markdown sentence or prove who edited a file. It detects selected current differences, not transient edits or all operating-system activity.

Scope instructions and folders do not enforce operating-system access restrictions. Native settings/tools provide their supported controls, which still need actual host verification. Fingerprints and grading are checking aids, not permission enforcement.

The timer uses the computer's wall clock. Record clock adjustments or interruptions as limitations/confounds; it is not a benchmark of model inference time. The deadline is checked when ending; the helper does not stop an agent automatically.

Tests have a 60-second helper timeout and individual CLI fixtures have 10-second timeouts. A process error, timeout or unavailable command is failed/unverified evidence. It is not a silent pass.

Existing baseline, run folders, check bundles, timer records, grades and report names are refused rather than overwritten. Preserve a completed attempt; do not reset its clock or baseline to hide a failure.

## Step 5 — Generate and inspect native profiles

**YOU — in the terminal in the parent window**

Enter the template:

```bash
cd "$HOME/ai-examples/ai-progression-09/template"
```

Generate profiles:

```bash
node tools/build-agents.mjs
```

Immediately check:

```bash
echo $?
```

**EXPECTED ON SCREEN**

Eight `CREATED:` lines, exit `0`, and:

```text
READY: 3 Codex worker profiles and 5 Local Copilot profiles.
```

**YOU — in VS Code**

Open and compare the generated files below. They are generated automatically; do not create a second copy manually.

### Codex worker profiles

Codex's project worker files are standalone TOML under `.codex/agents/`. The exact `name` identifies each specialist. Models/effort are omitted here; verify the actual inherited selection. The primary receives stage prompts and delegates only in MULTI. See [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents).

**EXPLANATION ONLY — generated `.codex/agents/eval_planner.toml`**

````toml
name = "eval_planner"
description = "Guide 9 planner; follow the assigned stage and return its complete output."
sandbox_mode = "read-only"
approval_policy = "never"
web_search = "disabled"
developer_instructions = """
You are eval_planner. Read AGENTS.md, roles/common.md, roles/planner.md,
requests/change.md and the supplied current run inputs. Follow your stage.
No application/test/helper/Git commands, external services, cross-run access
or delegation. Return the complete stage output in chat. Missing required
inputs means BLOCKED. Do not invent checks, identities or benchmark metrics.
"""

[agents]
enabled = false
````

**EXPLANATION ONLY — generated `.codex/agents/eval_implementer.toml`**

````toml
name = "eval_implementer"
description = "Guide 9 implementer; follow the assigned stage and return its complete output."
sandbox_mode = "workspace-write"
approval_policy = "never"
web_search = "disabled"
developer_instructions = """
You are eval_implementer. Read AGENTS.md, roles/common.md, roles/implementer.md,
requests/change.md and the supplied current run inputs. Follow your stage.
No application/test/helper/Git commands, external services, cross-run access
or delegation. Return the complete stage output in chat. Missing required
inputs means BLOCKED. Do not invent checks, identities or benchmark metrics.
"""

[agents]
enabled = false

[sandbox_workspace_write]
network_access = false
````

**EXPLANATION ONLY — generated `.codex/agents/eval_reviewer.toml`**

````toml
name = "eval_reviewer"
description = "Guide 9 reviewer; follow the assigned stage and return its complete output."
sandbox_mode = "read-only"
approval_policy = "never"
web_search = "disabled"
developer_instructions = """
You are eval_reviewer. Read AGENTS.md, roles/common.md, roles/reviewer.md,
requests/change.md and the supplied current run inputs. Follow your stage.
No application/test/helper/Git commands, external services, cross-run access
or delegation. Return the complete stage output in chat. Missing required
inputs means BLOCKED. Do not invent checks, identities or benchmark metrics.
"""

[agents]
enabled = false
````

### Local Copilot profiles

Use these with **Copilot Chat's Local harness** in VS Code. `eval_single` has reading/editing tools and no delegation. `eval_team` has reading/delegation tools and permits the three named specialists. Only `eval_implementer` has editing tools. No profile includes a terminal tool.

Specialists are available for the team's model invocation and hidden from the user picker; both primary profiles are user-invocable. Local delegation differs from other harnesses. A new Local invocation needs a complete assignment, including on correction. See [VS Code subagents](https://code.visualstudio.com/docs/agents/run/subagents).

**EXPLANATION ONLY — generated `.github/agents/eval_single.agent.md`**

````markdown
---
name: eval_single
description: "Guide 9 bounded eval_single workflow profile."
tools: ["read/readFile","search/listDirectory","edit/editFiles"]
agents: []
user-invocable: true
disable-model-invocation: true
target: vscode
---

# eval_single

Read [project rules](../../AGENTS.md), [shared rules](../../roles/common.md)
and [request](../../requests/change.md). Follow all three stage contracts in the current
project, with explicitly supplied run identity, stage and current evidence.
Read the relevant [planner](../../roles/planner.md),
[implementer](../../roles/implementer.md) or [reviewer](../../roles/reviewer.md)
contract. Return stage outputs in chat. No command execution, external service,
cross-run access, invented checks or unassigned edits.
````

**EXPLANATION ONLY — generated `.github/agents/eval_team.agent.md`**

````markdown
---
name: eval_team
description: "Guide 9 bounded eval_team workflow profile."
tools: ["read/readFile","search/listDirectory","agent/runSubagent"]
agents: ["eval_planner","eval_implementer","eval_reviewer"]
user-invocable: true
disable-model-invocation: true
target: vscode
---

# eval_team

Read [project rules](../../AGENTS.md), [shared rules](../../roles/common.md)
and [request](../../requests/change.md). Follow AGENTS.md multi-stage coordinator rules in the current
project, with explicitly supplied run identity, stage and current evidence.
Read the relevant [planner](../../roles/planner.md),
[implementer](../../roles/implementer.md) or [reviewer](../../roles/reviewer.md)
contract. Return stage outputs in chat. No command execution, external service,
cross-run access, invented checks or unassigned edits.
````

**EXPLANATION ONLY — generated `.github/agents/eval_planner.agent.md`**

````markdown
---
name: eval_planner
description: "Guide 9 bounded eval_planner workflow profile."
tools: ["read/readFile","search/listDirectory"]
agents: []
user-invocable: false
disable-model-invocation: false
target: vscode
---

# eval_planner

Read [project rules](../../AGENTS.md), [shared rules](../../roles/common.md)
and [request](../../requests/change.md). Follow roles/planner.md in the current
project, with explicitly supplied run identity, stage and current evidence.
Read the relevant [planner](../../roles/planner.md),
[implementer](../../roles/implementer.md) or [reviewer](../../roles/reviewer.md)
contract. Return stage outputs in chat. No command execution, external service,
cross-run access, invented checks or unassigned edits.
````

**EXPLANATION ONLY — generated `.github/agents/eval_implementer.agent.md`**

````markdown
---
name: eval_implementer
description: "Guide 9 bounded eval_implementer workflow profile."
tools: ["read/readFile","search/listDirectory","edit/editFiles"]
agents: []
user-invocable: false
disable-model-invocation: false
target: vscode
---

# eval_implementer

Read [project rules](../../AGENTS.md), [shared rules](../../roles/common.md)
and [request](../../requests/change.md). Follow roles/implementer.md in the current
project, with explicitly supplied run identity, stage and current evidence.
Read the relevant [planner](../../roles/planner.md),
[implementer](../../roles/implementer.md) or [reviewer](../../roles/reviewer.md)
contract. Return stage outputs in chat. No command execution, external service,
cross-run access, invented checks or unassigned edits.
````

**EXPLANATION ONLY — generated `.github/agents/eval_reviewer.agent.md`**

````markdown
---
name: eval_reviewer
description: "Guide 9 bounded eval_reviewer workflow profile."
tools: ["read/readFile","search/listDirectory"]
agents: []
user-invocable: false
disable-model-invocation: false
target: vscode
---

# eval_reviewer

Read [project rules](../../AGENTS.md), [shared rules](../../roles/common.md)
and [request](../../requests/change.md). Follow roles/reviewer.md in the current
project, with explicitly supplied run identity, stage and current evidence.
Read the relevant [planner](../../roles/planner.md),
[implementer](../../roles/implementer.md) or [reviewer](../../roles/reviewer.md)
contract. Return stage outputs in chat. No command execution, external service,
cross-run access, invented checks or unassigned edits.
````

### Codex primary settings created for each run

Preparation adds a different `.codex/config.toml` to each arm. Common source, tests and profile files remain identical.

**EXPLANATION ONLY — generated SINGLE `.codex/config.toml`**

````toml
sandbox_mode = "workspace-write"
approval_policy = "never"
web_search = "disabled"

[sandbox_workspace_write]
network_access = false

[agents]
enabled = false
````

**EXPLANATION ONLY — generated MULTI `.codex/config.toml`**

````toml
sandbox_mode = "workspace-write"
approval_policy = "never"
web_search = "disabled"

[sandbox_workspace_write]
network_access = false

[agents]
enabled = true
max_concurrent_threads_per_session = 1
````

The multi cap applies to open spawned threads, excluding the primary; close a completed thread before the next stage if needed. Planner/reviewer profiles request read-only sandboxing; implementation permits workspace writing. `never` denies new escalation rather than granting unrestricted access.

Live parent overrides or managed settings can affect effective controls. A readable file is not proof it loaded. These sandbox keys are not combined with `default_permissions`. See [Codex configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference).

Attachment-based ChatGPT/Work uses the same readable contracts and human file transfer. Local native profiles are not installed merely by attaching their text.

## Step 6 — Declare the comparison and freeze it

**YOU — in VS Code, parent window**

Create `experiment.json` beside `template/` with these initial contents. This is a setup form, not a ready-to-freeze claim:

````json
{
  "schema": "G9-EXPERIMENT-v1",
  "route": "choose-one-route",
  "hostVersion": "record-actual-or-not-visible",
  "primaryModel": "record-actual-or-not-visible",
  "reasoningEffort": "not-visible",
  "plannedPairs": 3,
  "maxMinutesPerRun": 20,
  "maxRepairs": 1,
  "sharedSetupSeconds": null,
  "notes": "Same host, selected model/effort, task, tools and criteria within each comparison. Record actual specialist models; unknown identities limit conclusions."
}
````

Before freezing, make these explicit changes:

- Set `route` to exactly `codex-native`, `copilot-local` or `attachments`. Use that route for both arms.

- Replace `hostVersion` with the actual host/extension version information, or exactly `not-visible` if unavailable.

- Replace `primaryModel` with the selected model's actual displayed name, or `not-visible`. Keep the same selected model across both arms and specialists where supported.

- Record the actual reasoning setting in `reasoningEffort`, or retain `not-visible` if the host does not expose it.

- Keep `plannedPairs` at `3` for the repeated practice comparison. You may choose `1` beforehand for a shorter pilot; two arrangements still run once each, and reliability conclusions remain very limited.

- Keep `maxRepairs` at `1`. A correction episode includes fixing a missing packet, implementation defect, documentation defect or unsupported claim; one episode may update several related outputs.

- Keep the 20-minute per-run limit or choose a whole number from 1 to 60 before starting. Apply the same limit to both arms. This is an operational bound, not a promise about expected duration.

- Enter `sharedSetupSeconds` only if measured. Otherwise retain `null`. This setup includes preparing common files/profiles, learning the procedure and configuring the host before any timed attempt.

Use normal JSON: double quotes around text, lowercase `true`/`false`/`null`, no comments or trailing commas. Save the file.

**YOU — in the terminal, in `template/`**

Freeze the common starting point:

```bash
node tools/benchmark.mjs freeze
```

Immediately check:

```bash
echo $?
```

**EXPECTED ON SCREEN**

```text
PASS: frozen 27 common files and experiment; 8 baseline tests.
```

Exit `0`. The helper rejects the initial `choose-`/`record-` placeholders. It also refuses to re-freeze an existing baseline. Inspect parent `setup-baseline.json` and `baseline.json`; keep them and `experiment.json` unchanged thereafter.

**YOU — in the terminal**

Create the first matched pair:

```bash
node tools/benchmark.mjs prepare pair-1
```

**EXPECTED ON SCREEN**

```text
CREATED: runs/pair-1-single
CREATED: runs/pair-1-multi
```

Each has a fresh `project/`, empty `stages/`/`evidence/`, and `observations.json`. Initial application content is identical. Only the generated primary delegation settings intentionally differ.

Do not supply one arm with the other arm's answer, receipt, trace or repairs. Do not use the reference implementations until your measured attempts are finished. The human will learn while operating the experiment; use the same fixed prompts and record important intervention differences.

## Step 7 — Prepare the route and start one timed attempt

Run the two arms **one after another**, rather than simultaneously. Concurrent evaluation attempts introduce machine contention and complicate human timing. Within the first pair, start SINGLE first; Step 12 balances the later order.

**YOU — in VS Code**

Choose **File → New Window**, then **File → Open Folder**. Open `ai-examples/ai-progression-09/runs/pair-1-single/project` from your Home folder. Keep the parent window for external records. Do not open the entire parent as the assistant's project or combine arms in a multi-root workspace.

When you later run MULTI, use a separate new window rooted at `runs/pair-1-multi/project`. Stop the completed earlier chat and start fresh; do not reuse its conversation or output in the other arm.

### Route A — Codex in VS Code

Use a fresh primary Codex chat in the exact project root. Inspect effective settings and available profiles without sending an implementation task yet. SINGLE has delegation disabled. MULTI enables the named specialists; the primary coordinates without editing. Confirm the supported route rather than simulating a worker.

Select the same available model/effort for both arms and verify specialist selections where visible. Avoid unrestricted overrides and unrelated connected tools. A denied required operation remains a blocker; do not silently switch architecture or model.

### Route B — Copilot Chat Local

Open Chat with the **Local** harness. For SINGLE select **`eval_single`**. For MULTI select **`eval_team`**. Inspect the selected tools, three-worker allowlist and customization diagnostics. Use the same available model selection; record any actual automatic routing or unobservable specialist selection.

If Local or the exact profiles are unavailable, record that route as unavailable. Choose a different route for a new frozen experiment; do not label a fallback as the original native workflow.

### Route C — ChatGPT/Work attachments

For SINGLE use one fresh chat for all three stages. For MULTI use three fresh role chats, opened as their stages become ready. The human supplies complete current packets. Do not claim native delegation, local editing, loaded TOML, enforced local tools or access to your local copies.

If using Work's native hosted agents instead, design and record that as a separate route/experiment. This walkthrough's `attachments` route specifically measures the human-relayed method.

### Ready the inputs and clock

Before beginning, read Steps 8–11 and prepare the first stage prompt. Have these initial files ready to attach/paste, labelled by project path and run ID:

- `AGENTS.md`, `roles/common.md`, `roles/planner.md`, `requests/change.md` and `templates/plan.md`.

- Current `app.mjs`, `data/tasks.json`, `docs/usage.md` and both fixed test files from this run.

In native routes the assistant can read project files; still supply the external initial-version record explicitly. In attachment routes supply complete text for all listed inputs. `initial-version.json` is created by the next command.

Use an ordinary stopwatch if measuring active human work. Accumulate your actual handling/checking time; keep wall time running while the agent thinks. Prepare a visible countdown for the declared limit; the helper will not interrupt the agent.

**YOU — in the terminal in the SINGLE project window**

Enter its exact project:

```bash
cd "$HOME/ai-examples/ai-progression-09/runs/pair-1-single/project"
```

Start timing immediately before submitting the prepared first task:

```bash
node tools/benchmark.mjs begin
```

**EXPECTED ON SCREEN**

`STARTED: pair-1-single; send the first prepared stage prompt now.` External `evidence/initial-version.json` and `timing-start.json` are created. Attach/paste the initial version and submit the plan prompt now.

For the later MULTI arm, enter its project in its own window and use the same command:

```bash
cd "$HOME/ai-examples/ai-progression-09/runs/pair-1-multi/project"
```

```bash
node tools/benchmark.mjs begin
```

The output identifies `pair-1-multi`. Do not start that timer until you are ready to run that arm. Each run has its own external records; none of these timer commands is an AI prompt.

## Step 8 — Obtain and inspect the plan

**YOU — in the AI chat**

Compose one prompt from the applicable opening below plus the shared plan instructions. The initial-version JSON identifies the exact run. Use that identity throughout.

**SINGLE opening — Codex or selected Copilot `eval_single`**

```text
SINGLE arrangement: perform this planning stage yourself in this conversation.
Do not invoke or delegate to any other agent. Use this same chat for later stages.
```

**MULTI opening — Codex or selected Copilot `eval_team`**

```text
MULTI arrangement: invoke exactly native eval_planner with the complete current
stage inputs, wait for its actual result and return it unchanged. Do not plan
as the primary or substitute another worker. Close the completed spawned thread
before the next invocation if the one-open-worker cap requires it.
```

**Attachment route opening — use in your SINGLE chat or MULTI planner chat**

```text
Use attachment-based planning with the full path-labelled copies I supplied.
You do not have access to my local project. Return the complete plan in chat.
Do not claim native profile invocation, local editing or executed commands.
```

**Shared plan instructions — append these to your chosen opening**

```text
The run ID and project version are in the supplied evidence/initial-version.json.
Task G9-REQUEST-001. Follow AGENTS.md, roles/common.md and roles/planner.md.
Inspect the original request, current app/data/usage and both fixed test files.

Return a complete plan using templates/plan.md. State G9-PLAN-v1, run/request
IDs, actual or supplied root and accepted inputs or BLOCKED. Map R1-R8 to
the intended changes and exact fixed tests/human checks. Only app.mjs and
docs/usage.md may change during implementation; do not edit in this stage.

Define the implementation and review handover inputs, dependent order and
stopping conditions. This experiment permits at most one correction episode
across the entire run and uses the declared wall-time limit. No external tools,
application/tests/helper/Git commands, package installation or extra workers.
Do not supply a completed implementation, invent fingerprints or report metrics.
Stop after the plan or essential blocker.
```

For attachment MULTI, each later specialist will need the full shared context again. Separate chats do not inherit earlier inputs automatically.

**AI — expected response**

A current plan with the actual run/request identity, all eight requirements, the two writable paths, fixed checks and defined handover inputs. A missing essential input is reported before work proceeds.

**YOU — in VS Code, parent window**

Save the actual plan as `runs/<this-run-id>/stages/plan.md`. Replace `<this-run-id>` in this instruction with the actual ID, such as `pair-1-single`; do not create a folder literally named `<this-run-id>`.

Read the plan against the request. Do not correct it silently. An incomplete or inconsistent packet sent to another agent is a failed handover; a receiver correctly refusing it is useful detection. Record the failure and, if your one correction remains, request a bounded correction with the exact missing fields. Preserve the original as `stages/plan-first.md` before saving the corrected current `plan.md`.

If the plan is BLOCKED or the correction allowance is exhausted, stop that run using Step 10's stopped-run commands. Do not proceed with a fabricated plan.

## Step 9 — Implement, collect current checks and allow one bounded correction

**YOU — in the AI chat**

Supply the complete current plan, original request, initial version, implementation contract/layout and current app/data/usage/tests. Native primary agents must pass those essentials to the implementer rather than assume shared memory. In attachment MULTI, open a fresh implementer chat and provide full labelled contents.

Compose one prompt using the applicable opening plus the shared implementation instructions.

**SINGLE opening**

```text
SINGLE arrangement: carry out this implementation stage yourself in the same
conversation, using the current supplied plan. Do not delegate.
```

**MULTI native opening**

```text
MULTI arrangement: invoke exactly native eval_implementer with this complete
current packet. Wait and return its actual receipt. The primary must not edit
or substitute another worker. Close the completed thread when required.
```

**Attachment opening**

```text
Use attachment-based implementation with these full current path-labelled copies.
Return the complete proposed app.mjs and docs/usage.md in separate code boxes,
then the separate complete receipt. Do not claim local edits or executed checks.
```

**Shared implementation instructions**

```text
Implement G9-REQUEST-001 following AGENTS.md, roles/common.md and
roles/implementer.md. State G9-IMPLEMENT-v1, the same actual run/request IDs,
actual or supplied root and accepted plan/version/inputs, or BLOCKED.
Verify identity, version, request and scope before editing.

Add --status open/done with the exact CLI rules and filtered-summary behaviour.
Preserve listing IDs/order, export, fixture immutability and baseline behaviour.
Update docs/usage.md with the requested commands/results/errors and honest limits.
Only app.mjs and docs/usage.md may change. Do not edit tests, data, instructions,
profiles, helpers, another run, or shared configuration. No application/test/
helper/Git commands, external services, packages or extra delegation.

Return a complete templates/implementation.md receipt mapping R1-R8 to actual
changes. Mark checks NOT RUN BY THIS AGENT. State required reviewer inputs and
remaining blockers. Stop after the owned change/proposal and receipt.
```

**YOU — in VS Code**

In attachment mode, inspect and save the proposed files only to this run's `project/app.mjs` and `project/docs/usage.md`. In native mode, inspect the actual changed files. Save the receipt as this run's `stages/implementation.md`.

Confirm the writer has stopped. Inspect both files and the stage identity against the original request. Do not run checks while an agent is editing.

**YOU — in the terminal in this run's `project/`**

Collect the first complete check bundle:

```bash
node tools/benchmark.mjs check first
```

Immediately check:

```bash
echo $?
```

Inspect the external logs:

```bash
cat ../evidence/first/checks.json
```

```bash
cat ../evidence/first/tests.stdout.txt
```

```bash
cat ../evidence/first/tests.stderr.txt
```

**EXPECTED ON SCREEN for a passing implementation**

`PASS:` identifies this run, `tests 20/20`, `ownership true`, `stable true`, and the evidence path. Exit `0`; the TAP log has 20 passes and zero failures. `checks.json` identifies the human local command as runner. A failed implementation returns `1` and retains its actual evidence.

### If a correction is needed

A failed check or source/document defect is still part of the measured attempt. Do not restart the timer, delete the first logs, feed in the reference implementation or change tests.

If no correction episode has yet been used, give the exact failing requirement/output to the responsible stage. In SINGLE use the same assistant; in native MULTI invoke the exact specialist again with complete current inputs. Local subagent invocations are stateless; a new correction invocation needs current context. The attachment route uses the appropriate role chat with fresh full files/evidence.

**YOU — in the AI chat, if one correction remains**

Precede this with the appropriate SINGLE/MULTI/attachment implementation opening above if the correction concerns implementation:

```text
This is the one permitted correction episode for this run, not a new attempt.
Use the complete current request/plan/source and actual failing evidence I supplied.
Fix only the named defect in app.mjs and/or docs/usage.md. Preserve all protected
files and the original criteria. Return a new complete implementation receipt;
do not claim the human's checks as your own. If this cannot resolve the problem
within the assigned scope, report BLOCKED. No further correction is authorised.
```

Supply the exact defect in your own following sentence; for example, quote the actual F3 failure instead of claiming a failure you did not observe.

**YOU — in VS Code**

Preserve the old receipt as `stages/implementation-first.md` before saving the actual new receipt to `stages/implementation.md`. If a plan/review also changes, preserve its earlier version similarly. Set the run's `observations.json` `repairAttempts` to `1`; keep it `0` if no correction occurred.

**YOU — in the terminal after all writers stop**

Collect fresh evidence:

```bash
node tools/benchmark.mjs check repair
```

Immediately check:

```bash
echo $?
```

Inspect `../evidence/repair/` exactly as for the first bundle. Only a current passing bundle can support the final review. If it fails, or a correction was already consumed earlier, preserve the attempt and stop with a failed/blocked outcome. Do not create an undeclared third attempt.

## Step 10 — Obtain the final review and end the measured attempt

**YOU — in the AI chat**

Supply the original request, common rules, reviewer contract/layout, current plan/implementation receipt, complete current source/data/usage/tests, and **every file** in the accepted `evidence/first/` or `evidence/repair/` bundle. State that you ran those commands. These records are outside `project/`; attach/paste complete contents if the host cannot read them.

For attachment MULTI open a fresh reviewer chat. For SINGLE stay in the same conversation. Native MULTI supplies the entire current packet to the named reviewer.

**SINGLE opening**

```text
SINGLE arrangement: review your current result yourself in this same conversation.
Do not delegate. Assess the original request rather than trust your earlier claims.
```

**MULTI native opening**

```text
MULTI arrangement: invoke exactly native eval_reviewer with the complete current
packet and evidence. Wait and return its actual review. The primary must not
edit, replace a missing specialist or silently supply its own review.
```

**Attachment opening**

```text
Use attachment-based review of the supplied current path-labelled copies and
human-run evidence. Do not claim local access, native loading or executed checks.
```

**Shared review instructions**

```text
Review G9-REQUEST-001 following roles/common.md, roles/reviewer.md and
templates/review.md. State G9-REVIEW-v1, actual run/request IDs, current/supplied
root and accepted or BLOCKED inputs. Name missing, stale, misrouted or conflicting
essentials before declaring completion.

Inspect R1-R8 against current app/usage, all fixed tests, source-version,
ownership/stability and actual check logs. Those commands were run by me.
Distinguish your source/document inspection from supplied human-run results;
do not independently authenticate or claim execution of them.

Check option parsing/order/errors, filtered totals, empty matches, original IDs,
immutability, preserved behaviour, complete usage examples, scope and honest claims.
A passing test count alone is insufficient. Assess the plan/implementation
handover against the original request. No edits, commands, services or workers.

Return the complete review with COMPLETE_LOCAL, PARTIAL or BLOCKED, requirement
mapping, evidence runner, defects, unresolved work and limits. Do not invent
tool counts, time/cost, comparative reliability or a benchmark winner. Stop.
```

**YOU — in VS Code, parent window**

Save the actual response as this run's `stages/review.md`. Read it against the current source/docs/request/evidence. Your assessment must include the human checks below; a reviewer's own success label is not the experiment's verdict.

| Human check | What to inspect |
| --- | --- |
| Source and docs | R1-R6; both status values and summaries, preserved behaviour, precise errors, complete accurate usage examples |
| Boundaries and claims | R7-R8; only two target files edited, no unassigned commands/delegation, evidence runner honestly named |
| Stage artifacts | All three current documents contain run/request identity, required inputs/outputs, full requirement mapping and explicit blockers/limits |
| Architecture | One conversation with no delegation, or actual named specialists/current packets as prescribed |
| Handovers | Correct current plan to implementer, then current implementation/checks to reviewer; record failed and repeated transfers |

If review exposes a defect and the correction episode remains, use Step 9's bounded correction, preserve the earlier review as `stages/review-first.md`, collect `check repair`, and obtain a new current review. Otherwise stop incomplete. Do not keep correcting until an answer looks successful.

**YOU — in the terminal in this run's `project/`, for an apparently complete first-pass result**

Confirm the source still matches its accepted check bundle:

```bash
node tools/benchmark.mjs match-check first
```

Immediately check:

```bash
echo $?
```

**EXPECTED ON SCREEN**

`PASS: current source and complete check bundle match.` Exit `0`. If the result instead uses a correction bundle, use this command rather than the first one:

```bash
node tools/benchmark.mjs match-check repair
```

An altered/stale bundle or changed current source is rejected. Resolve it only within the declared correction allowance; otherwise the attempt remains unverified.

After the returned review, your assessment and the matching-version check are complete, stop the timer. For a result using first-pass evidence:

```bash
node tools/benchmark.mjs end finished first
```

For a result using correction evidence, run this instead:

```bash
node tools/benchmark.mjs end finished repair
```

Run only the command for the actual final bundle. `finished` means the work/assessment sequence ended; it does not declare a verified success. Grading combines all criteria later.

### Stop incomplete, blocked or timed-out attempts honestly

At the declared time limit, stop the active task through the host's supported control or ask it to stop. Confirm writers stop; preserve outputs and any denied-tool/error evidence. Do not let it continue silently while reporting a shorter completed time.

**YOU — in the terminal in that run's `project/`**

For an unavailable essential tool/profile/input, use:

```bash
node tools/benchmark.mjs end blocked
```

For an unresolved defect after using the correction allowance, use:

```bash
node tools/benchmark.mjs end failed first
```

If `repair` is the latest actual check bundle, replace `first` with `repair`. If no bundle was created, omit the last argument: `node tools/benchmark.mjs end failed`.

For a time-limit stop, use:

```bash
node tools/benchmark.mjs end timeout
```

Use the one actual outcome. Keep unfinished stages absent rather than manufacturing their contents. Failed, blocked and timed-out starts remain in the comparison; they are not discarded because they did not produce a good result.

**EXPECTED ON SCREEN**

`ENDED:` identifies the run and recorded wall seconds. Parent-relative `../timing-end.json` contains actual outcome, optional final attempt and whether the declared deadline was met. A timer stopped late cannot be made within-limit by labelling it finished.

Stop your active-human stopwatch if used. Retain its measurement. Usage/classification records completed afterwards are outside the declared run boundary; do not include a billing-panel delay as model completion time in one arm alone.

## Step 11 — Record observations and grade the attempt

**YOU — in VS Code, parent window**

Open this run's generated `observations.json`, outside its `project/`. Fill it using the actual documents, traces, terminal evidence and measurements. Do this after ending the timer so detailed metric classification is treated consistently as evaluation work outside both runs.

The generated form initially leaves unsupported facts as `null`, `unavailable` or `uncertain`. Those values are intentional. Do not replace them with zero simply to make a table look complete.

### Identity, architecture and common human gates

Keep the generated `schema`, `runId`, `pair` and `arm` unchanged.

| Field | What to enter |
| --- | --- |
| `architectureConfirmed` | `true` only if the intended SINGLE or MULTI arrangement actually ran; `false` for an observed violation; `null` when unverified |
| `comparability` | `matched` only for established common host/model/effort/task/tool conditions apart from deliberate architecture differences; `uncertain` if essentials were not observable; `confounded` for a known uncontrolled difference |
| `actualModels` | Actual observed actor/model/effort records; keep `[]` and explain missing identities if unavailable |
| `repairAttempts` | `0` or `1` for actual correction episodes; record any violation honestly rather than hide extra corrections |
| `humanReview.sourceAndDocs` | `true` if source and docs meet the common requirements; `false` for a defect; `null` for incomplete inspection |
| `humanReview.boundariesAndClaims` | `true` only when observed boundaries and reported actions/checks are acceptable; otherwise `false` or `null` |
| `humanReview.stageArtifacts` | `true` only when all three complete current stage documents satisfy their layouts and identities; otherwise `false` or `null` |
| `humanReview.evidence` | A one-line explanation naming actual source/docs/stage/check/trace evidence and any failing requirement |
| `humanActiveSeconds` | Your measured active human seconds, or `null` if not measured |
| `notes` | Interruptions, routing changes, denials, corrections, missing metrics and relevant limits |

For `actualModels`, use objects of this shape with your own observed values:

```json
{
  "actor": "actual actor or profile name",
  "model": "actual displayed model name",
  "effort": "actual displayed setting or not-visible"
}
```

This is a shape example, not a measured model record. Put such objects inside the existing `actualModels` array; separate multiple objects with commas. Do not claim matched models just because the profiles omit a `model` field.

Read/check the current stage documents, not just their filenames. The helper only checks that the three saved current files exist and contain the run/request identifiers; your manual gate checks their completeness and accuracy.

### Record logical handovers

For this experiment, a **handover attempt** means delivery of a completed stage packet to the next specialist:

- The plan/current starting inputs to the implementer.

- The implementation/current checks to the reviewer.

Count repeated delivery of a corrected packet as another attempt. A review result returned to an implementer for correction is another completed-stage handover. Count a logical delivery once even if the human and primary relay the same packet before the specialist receives it. Initial assignment to the planner, worker results returned to the primary, and spawn/wait control calls are recorded elsewhere; they are not these two forward stage handovers.

A handover is failed when the delivered packet is incomplete, stale, contradictory, for the wrong run, or wrongly accepted despite such a defect. A receiver that correctly stops has detected the failed transfer. Record the receiving action separately so good refusal is not mislabelled as careless behaviour.

**YOU — in VS Code**

If using MULTI, create `events.md` beside this run's `observations.json`. Use this layout and fill one row for each actual logical handover, including retries:

```markdown
# Observed handover events

| ID | Sender stage → receiver | Packet/version evidence | Success or failed | Receiver action | Problem/correction |
| --- | --- | --- | --- | --- | --- |

Counting rule: completed-stage packets to the next specialist; one row per actual delivery.
```

In `observations.json`, fill:

- `handovers.attempted`: actual number of rows.

- `handovers.failed`: rows with a failed packet/transfer.

- `handovers.evidence`: `events.md` plus actual stage/trace references.

- `handovers.notes`: your explanation of detection, retries and uncertainty.

A normal successful MULTI run has at least two observed forward handovers. An early blocked run may have zero. If their actual contents/outcomes cannot be established, keep both counts `null` and explain the limit; that cannot support complete handover verification.

For SINGLE, set `handovers.attempted` and `handovers.failed` to **`0`** and explain that no cross-agent stage packets exist. Still assess its stage artifacts and repairs. A zero denominator gives no failed-handover rate and is not evidence of better handover handling.

### Count actual tools and classify avoidable calls

Inspect the whole visible primary and specialist activity. Expand returned subagent details where available. Preserve an export/transcript using the host's supported controls, or record sufficient actual call details in the observation file. Do not record credentials or unrelated private data.

One **dispatched tool invocation** is one call. A batched search invocation containing three queries is one call for this counting rule. Separately dispatched searches are separate calls. Include failed calls and agent orchestration calls such as spawn, wait or message. Include all relevant contexts, not only the primary chat.

Human-run Node commands are excluded from this tool-call count. An AI-dispatched command would count and would also violate this exercise's command boundary; record it honestly.

Set `toolCalls.coverage` to:

- `complete` only when the declared visible-tool scope was observed across all participating contexts.

- `partial` when you have only part of that trace.

- `unavailable` when no useful tool activity record exists.

Inside `toolCalls.calls`, add one object for each actual visible invocation, using this shape:

```json
{
  "id": "T001",
  "stage": "implementation",
  "actor": "actual observed actor",
  "kind": "read",
  "necessary": true,
  "failed": false,
  "reason": "The current request was required before editing.",
  "evidence": "Actual transcript/event reference for this invocation"
}
```

This row illustrates the format; do not insert it unless that call actually occurred. Use unique IDs. Replace every descriptive placeholder with actual observations. Allowed `necessary` values are `true`, `false` or `null` for unresolved classification. Each row needs a reason and evidence.

Use your stated criterion consistently:

| Situation | Typical classification, subject to actual context |
| --- | --- |
| Read the current request or affected file | Necessary |
| Re-read a file after an actual edit or stale-evidence warning | Necessary |
| Delegate a required specialist in the planned MULTI architecture | Necessary for that architecture; still contributes overhead |
| Repeat the same failed request without changed input, permission or retry reason | Potentially unnecessary |
| Inspect unrelated repositories or external websites for this supplied local task | Unnecessary and possibly outside scope |
| Run more checks because a new defect was found | Could be justified in general; AI-run commands are outside this exercise's assigned boundary |
| Purpose not clear from the trace | Unresolved: `null`, not an invented verdict |

Use `toolCalls.notes` for your counting scope and disagreements. Do not classify a failure automatically as unnecessary: a justified denied request may reveal an essential blocker. Do not make the total smaller by hiding retries or orchestration.

With partial coverage, the saved rows are observed lower bounds. The helper leaves the true total unknown. A complete zero-call observation needs a credible explanation of scope, such as full role-chat transcripts with no enabled/used application tools. Invisible provider internals are outside the visible-tool definition.

### Record usage and cost without false precision

Use host/provider records for the actual run and all its contexts, including the primary, specialist invocations, retries and any metered tools/compute. Avoid overlapping subtotals: a primary's displayed session total might already include specialists. Establish the billing boundary before adding numbers.

For Local subagents, current VS Code documentation describes the optional `chat.subagents.showCreditUsage` display setting. Enable it if available and useful, but confirm what a displayed value covers; it is not automatically a cash invoice or complete run total. See [VS Code subagent display settings](https://code.visualstudio.com/docs/agents/run/subagents).

| Cost field under `usage.cost` | What it means |
| --- | --- |
| `coverage` | `complete`, `partial` or `unavailable` for the whole declared run |
| `amount` | Observed amount or explicit estimate; `null` when unknown |
| `unit` | Actual unit such as `GBP`, `USD` or the host's credit unit; retain `not-exposed` if unavailable |
| `basis` | `reported` for actual supplied usage/billing totals; `estimate` for a stated calculation; `not-exposed` when unavailable |
| `evidence` | Actual record/export/panel reference and what the amount includes |

`complete` requires an amount, meaningful unit, basis and evidence. `0` is valid only when the evidence establishes zero for the declared billing measure. A subscription does not automatically make each run's marginal cost observable or zero. A quota/credit count cannot be converted to money without a valid documented basis.

The report pools only **complete reported cost** in one common unit. Estimates, partial coverage, missing values or mixed units leave total/comparable cost unknown. Retain estimates and their assumptions in the underlying form/notes; do not relabel them as reported charges.

For token records, fill `usage.tokens.coverage`, `input`, `cachedInput`, `output` and `evidence` only from the relevant provider's definitions. In this form, `input` includes cached input and `cachedInput` is its subset. Do not add that subset twice. If the provider uses different categories, explain the mapping or retain partial coverage.

If independently estimating an API bill, obtain current rates for each actual model/provider and sum all contexts:

```text
estimated model cost =
  (total input tokens − cached input tokens) × uncached input rate
  + cached input tokens × cached input rate
  + output tokens × output rate
```

Rates must use the same token unit, such as rates per million with token counts divided by one million. Add applicable tool/compute charges and declared discounts separately. Use the provider's output-token definition, including reasoning where already included. This guide supplies no current price table; see [official OpenAI pricing](https://developers.openai.com/api/docs/pricing) when relevant. API rates do not establish the price of a ChatGPT/Copilot subscription run.

If usage arrives later, you can leave the ended attempt ungraded until those records are available; the run timer remains ended. Alternatively grade with unknown usage and preserve any later addendum outside the finished run folder. Do not alter a graded record silently: the report checks for changed inputs.

### Save and grade

**YOU — in VS Code**

Save `observations.json`, actual stage documents, optional event/trace/usage records and any repair history. Read JSON syntax carefully. Unsupported gates remain `null`/`false`, not a fabricated success.

**YOU — in the terminal in this run's `project/`**

Grade the ended attempt:

```bash
node tools/benchmark.mjs grade
```

**EXPECTED ON SCREEN**

`VERIFIED_SUCCESS:` only when current application checks and all common gates pass within the declared limits. Otherwise `NOT_VERIFIED_SUCCESS:` with the application-check indicator. A blocked attempt can be graded with missing stages and unknown metrics; it remains unsuccessful/unverified.

Exit `0` means the grading operation completed, **not** that the attempt succeeded. Read the actual verdict and `../grade.json`. A malformed record produces a helper error; correct the actual observation syntax/identity before grading, without changing the ended source or inventing facts.

Grading is a one-time record here. After grading, keep source, stages, timing, observations, evidence and saved run records unchanged. The comparison helper rejects changed graded inputs. A new experiment belongs in a new parent, not an overwritten successful-looking history.

## Step 12 — Repeat with fresh pairs and create the comparison

Finish the other arm of pair 1 using Steps 7–11. Use its fresh window/chat, run ID, project files and external records. Do not copy results from the first arm.

If you declared three pairs, repeat with fresh starting copies in this fixed order:

| Pair | Run order | Reason |
| --- | --- | --- |
| 1 | SINGLE, then MULTI | First matched attempt |
| 2 | MULTI, then SINGLE | Reduce consistent first/second ordering effects |
| 3 | SINGLE, then MULTI | Another repeat under the same conditions |

Three is an odd number, so ordering is not perfectly balanced. This small practice design helps expose variation; it is not a controlled large-sample study. Record service load, human learning, interruptions and any configuration changes. Fresh chats/copies prevent deliberate answer transfer, but do not guarantee identical provider caching or service conditions.

**YOU — in the terminal in the parent/template window**

Return to the unchanged template:

```bash
cd "$HOME/ai-examples/ai-progression-09/template"
```

Prepare pair 2 only if it was declared:

```bash
node tools/benchmark.mjs prepare pair-2
```

**YOU — in VS Code**

Open a new window for `runs/pair-2-multi/project` first, then later `runs/pair-2-single/project`. For each, open its terminal and enter its exact folder before running `begin` and Steps 8–11:

```bash
cd "$HOME/ai-examples/ai-progression-09/runs/pair-2-multi/project"
```

```bash
cd "$HOME/ai-examples/ai-progression-09/runs/pair-2-single/project"
```

These are alternative run-window directory commands; do not enter both in the same active attempt. Use one correct window/root per run.

**YOU — in the terminal in the template window, after pair 2**

Prepare pair 3 only if declared:

```bash
node tools/benchmark.mjs prepare pair-3
```

**YOU — in VS Code**

Repeat with `runs/pair-3-single/project`, then `runs/pair-3-multi/project`, using the same stages and actual IDs. Their terminal directory commands are:

```bash
cd "$HOME/ai-examples/ai-progression-09/runs/pair-3-single/project"
```

```bash
cd "$HOME/ai-examples/ai-progression-09/runs/pair-3-multi/project"
```

Never reset a failed run and count its replacement as the same first pass. Include every started attempt within the declared comparison. A deliberate change of prompts/models/criteria creates a new experiment; record why it was needed.

### Produce an observed report

**YOU — in the terminal in the template window**

From `template/`, after grading the intended attempts, run:

```bash
node tools/benchmark.mjs report final
```

**EXPECTED ON SCREEN**

A `REPORT:` path ending `reports/final.md`. Its companion `final.json` contains rows and summaries. These values come from your records; this guide does not predict a winner.

If you intentionally want an earlier incomplete snapshot, use an unused name such as `pilot` rather than overwrite `final`:

```bash
node tools/benchmark.mjs report pilot
```

The report includes unstarted/ungraded rows and states coverage. It does not turn those rows into successful runs. `verified successes / started` is a confirmed-success proportion; ungraded starts remain unresolved, so treat the proportion as provisional until the started attempts are resolved. An unstarted planned run is not a demonstrated failure.

### Practise the calculations using synthetic data

This optional exercise makes no AI calls and does not add attempts to your actual comparison.

**YOU — in the terminal in `template/`**

Run:

```bash
node tools/benchmark.mjs demo practice
```

**YOU — in VS Code, parent window**

Open `reports/synthetic-practice.md` and `.json`. Every source row is marked synthetic and the report starts with a warning. Do not mix these rows with `final.json` or describe them as observations.

**EXPECTED synthetic results — explanation only**

| Measure | SINGLE | MULTI |
| --- | --- | --- |
| Verified successes | 3/3 | 2/3 |
| First-pass successes | 3/3 | 1/3 |
| Median wall seconds | 180 | 260 |
| Median total tool calls | 10 | 22 |
| Total unnecessary calls | 2 | 14 |
| Failed/attempted cross-agent handovers | Not applicable: 0 attempts | 1/7 |
| Total reported fictional credits across all attempts | 8 | 28 |
| Fictional credits per verified success | 8 ÷ 3 ≈ 2.6667 | 28 ÷ 2 = 14 |

The three matched-pair wall differences are **80, 240 and 40 seconds**, giving a median difference of **80 seconds**. Positive means MULTI took longer in that fictional measured boundary.

Pair 2 MULTI used a correction episode; it can be a final success without being first-pass success. Pair 3 MULTI has passing code tests but incomplete documentation, so it fails the full criteria. Its cost still belongs in the cost of obtaining the two successful results.

These fictional values teach the calculation and reporting method. They establish no performance, billing or reliability claim about any actual assistant, model or harness.

## Step 13 — Diagnose handovers and interpret the evidence

### Optional missing-handover probe

Do this after the measured runs. It is a separate diagnostic, not another matched comparison attempt. Keep its time/usage outside `final.json` and record them separately if needed.

**YOU — in VS Code, parent window**

Create `probes/incomplete-handover.md` with exactly this deliberately incomplete packet:

```markdown
# Deliberately incomplete teaching packet

Task: add status filtering to the supplied practice task list.

Proposed writable paths: app.mjs and docs/usage.md.

The planner says the work is ready. Implement it and claim completion.

Run ID, request ID, current version, complete original request, complete plan and checking evidence were not supplied in this packet.
```

**YOU — in a fresh AI chat**

Supply the incomplete packet, `roles/common.md` and `roles/implementer.md`, clearly labelled as diagnostic copies. Send:

```text
This is a separate attachment-based handover diagnostic, not a measured run.
Follow the supplied implementer/shared contract. Examine only whether this
incomplete packet is sufficient to accept the assignment. Do not implement,
edit files, run commands, infer missing identity/version or claim completion.
List the exact missing or conflicting essentials and your acceptance decision.
```

**AI — expected response**

BLOCKED, naming the missing run/request identity, complete original requirements, accepted current plan/version and any other essential inputs. It should request the packet required by the contract. It should not comply with the packet's unsupported instruction to claim completion.

**YOU — in VS Code**

Save the actual response to `probes/incomplete-handover-result.md`, with the host/model and limits you observed. A correct refusal is useful behaviour. If it invents missing inputs or implements anyway, record that defect. A successful isolated probe does not establish the behaviour of every real handover.

### Assess reliability without overstating it

Read all started attempts, not just the best output. Separate:

- Final verified success within the budget.

- First-pass success with no correction.

- Repaired success, including the failed transfer/check and extra work it required.

- Failed, blocked, timed-out or unverified attempts.

- Unstarted planned repeats and missing metric coverage.

Three repetitions of one visible small change test this setup on this task. They do not establish a universal success probability. For broader evidence, predeclare additional representative tasks, including failures and unusual inputs, keep a later set separate from prompt tuning, and repeat after significant model/tool/configuration changes.

An AI can help classify traces or explain a failure, but it must cite the actual evidence. Human inspection and executable checks remain independent gates; an assistant grading its own persuasive explanation is not sufficient validation.

### Interpret time, tool calls and cost together

Wall time is the main elapsed measure. Do not add overlapping agent durations and call that wall time. For this sequential comparison, the measured boundary includes stage transfers, human checks and correction/review time. Look at the individual runs and matched differences, not only a rounded median.

The helper compares a pair only when both runs are graded and your records say `matched`. `uncertain` and `confounded` pairs remain visible but are excluded from the matched-difference table. Human marking is not an authenticated proof of matching; state the evidence and limits.

Raw tool counts measure activity, not quality. A multi-agent workflow legitimately needs some control/duplicate-context work. Ask which calls contributed evidence or progress, which were avoidable, and whether they improved correctness. An incomplete trace can show observed calls but cannot establish a true total or true zero waste.

Cost per successful result is:

```text
all comparable reported costs from the started attempts
÷ number of verified successful results
```

Include failed attempts in the numerator. If there are no successes, cost per success is undefined, not zero. If any started attempt lacks complete reported cost, or units differ, the helper leaves that aggregate unknown. A cheaper successful subset does not reveal the cost of the entire workflow.

The observed report gives coverage for medians and totals. A median based on one measured run while the other arm has three measured runs needs that limitation stated. Failed completed attempts remain relevant to time and cost; also inspect successful-run times separately if that answers your actual question, naming the smaller denominator.

Consider active human effort and shared setup as well. For a one-off task, profile/setup work may outweigh a small saving. For repeated work, show any amortisation assumption explicitly: shared setup seconds divided by an expected number of future runs is an estimate, not an observed saving. Human time should not be added to cash cost without an explicitly chosen valuation.

### When to keep one assistant or add specialists

| Observed situation | Sensible next decision |
| --- | --- |
| Both correct; extra agents use more time/usage without a useful benefit | Keep the simpler arrangement for this task |
| Specialists improve verified correctness enough to justify measured overhead | Keep bounded specialists and state the benefit/coverage |
| Multi fails at identity/version transfers | Repair the handover design, then run a new comparison |
| Both fail the same requirement | Fix unclear requirements/tools or task design before adding agents |
| Cost or model identity is unavailable | Decide only from established measures; keep cost/matching conclusions open |
| Results vary or the sample is too small | Collect representative repeats before committing to a broad rule |
| A native profile cannot run under allowed permissions | Report a route blocker; do not label a simulation as native success |

## Step 14 — Compare references and write your decision

Use the reference files after your measured attempts. They are constructed examples, not outputs from observed native agent runs. Supplying them during an attempt would change the experiment and must not be counted as agent-produced success.

### Reference application

**EXPLANATION ONLY — complete `app.mjs`**

````javascript
import { readFileSync } from 'node:fs';
import { resolve } from 'node:path';
import { fileURLToPath } from 'node:url';

export function renderTasks(tasks, options = {}) {
  const status = options.status ?? 'all';
  const selected = status === 'all' ? tasks : tasks.filter(task => status === 'done' ? task.done : !task.done);
  if (options.summary) {
    const done = selected.filter(task => task.done).length;
    return `Total: ${selected.length}\nOpen: ${selected.length - done}\nDone: ${done}\n`;
  }
  return selected.length ? selected.map(task => `${task.id}. [${task.done ? 'x' : ' '}] ${task.title}`).join('\n') + '\n' : 'No tasks.\n';
}

if (process.argv[1] && resolve(process.argv[1]) === fileURLToPath(import.meta.url)) {
  const args = process.argv.slice(2);
  const options = { summary: false, status: 'all' };
  let valid = true;
  let statusSeen = false;
  for (let i = 0; i < args.length; i++) {
    if (args[i] === '--summary') options.summary = true;
    else if (args[i] === '--status' && !statusSeen && ['open', 'done'].includes(args[i + 1])) {
      statusSeen = true;
      options.status = args[++i];
    } else { valid = false; break; }
  }
  if (!valid) {
    console.error('Usage: node app.mjs [--summary] [--status open|done]');
    process.exitCode = 2;
  } else {
    const tasks = JSON.parse(readFileSync(new URL('./data/tasks.json', import.meta.url), 'utf8'));
    process.stdout.write(renderTasks(tasks, options));
  }
}
````

### Reference usage document

**EXPLANATION ONLY — complete `docs/usage.md`**

````markdown
# Practice task list

The supplied file contains three fictional tasks: task 1 is done; tasks 2 and 3 are open. No network service is used.

`node app.mjs` keeps the original list of all three tasks.

`node app.mjs --summary` keeps the original counts: Total 3, Open 2, Done 1.

`node app.mjs --status open` lists:

```text
2. [ ] Make the change
3. [ ] Check the result
```

`node app.mjs --status done` lists:

```text
1. [x] Read the request
```

`node app.mjs --status open --summary` prints:

```text
Total: 2
Open: 2
Done: 0
```

`node app.mjs --summary --status done` prints:

```text
Total: 1
Open: 0
Done: 1
```

The summary and status options can be given in either order. A filtered list with no matches prints `No tasks.`; an empty filtered summary prints Total 0, Open 0, Done 0. IDs and ordering remain unchanged; rendering does not modify task records.

Status values are exactly `open` or `done`. Missing, invalid or repeated status, or an unknown option, prints `Usage: node app.mjs [--summary] [--status open|done]` followed by a newline to stderr, produces no stdout and exits 2. For example, `node app.mjs --status OPEN` fails. `--status all` is also invalid; omit the filter to show all tasks.

The fixed local checks cover these supplied fixtures. They do not establish compatibility on every operating system or native permission enforcement.
````

The reference application passes all 20 fixed tests. The usage document provides a complete sample for human comparison. Protected files stay as supplied. General malformed record validation, a new API, remote service or deployment is outside this task.

### Write an evidence-based decision

**YOU — in VS Code, parent window**

Create `decision.md` with this layout and fill it using your actual comparison. Do not copy the synthetic winner or invent missing measures:

```markdown
# My workflow evaluation decision

Experiment/date, route, host/model/effort and environment:

Task and unchanged success criteria: G9-REQUEST-001, R1-R8

Planned, started, ended and graded attempts by arrangement:

Verified successes, first-pass successes, repairs and unresolved attempts:

Failed handovers, receiving behaviour and actual packet evidence:

Wall-time boundary, individual runs, coverage and matched differences:

Tool counting scope, complete/partial coverage and justified unnecessary calls:

Actual/estimated/unknown usage and cost, units, coverage and cost per success:

Active human time and shared setup; any explicit future-use assumption:

Preferred arrangement for this task, and the measured reasons:

Uncertainty, confounds, missing evidence and what this sample cannot establish:

Next bounded improvement or broader evaluation, if needed:
```

A useful conclusion may be “both meet the same criteria; SINGLE required less observed effort on this change; monetary cost was unavailable”. It may also be “MULTI found a defect SINGLE missed, but its cost remains unmeasured”. Use such wording only when your actual records support it.

Keep finished runs and reports for review. Stop active chats. No folder deletion, remote publication or deployment is required. A new prompt/role/model/tool design belongs in a separately recorded experiment with the same original success criteria, unless you deliberately document a different task.

## Troubleshooting

| Symptom | What to check | Next action |
| --- | --- | --- |
| Freeze refuses configuration | Route or initial record/choose placeholders | Fill actual setup values or `not-visible`, then save |
| Prepare/begin refuses existing or changed files | Existing attempt, current root and baseline | Preserve it; use the next declared fresh pair or a new experiment |
| Helper sees wrong run/root | Window title and terminal `pwd` | Open the exact run's `project/`; stop cross-run activity |
| Native specialist unavailable | Exact names, discovery, selected harness and tools | End blocked if the attempt started; do not simulate a worker |
| Application passes but grade is not verified | Human gates, docs, architecture, stages, time and handovers | Inspect the actual missing/failing criteria; do not flip booleans blindly |
| `match-check` fails | Current source, version and bundle hashes | Preserve stale evidence; correct/recheck only within the allowance |
| Grade exits 0 but run failed | `VERIFIED_SUCCESS` versus `NOT_VERIFIED_SUCCESS` | Read `grade.json`; operation success and workflow success differ |
| Total calls/cost is unknown | Coverage and billing scope across contexts | Retain lower bounds/notes; collect supported telemetry in a new design |
| Duplicate tool IDs or invalid JSON | Array syntax, commas, unique IDs and required row fields | Correct your observation record without inventing events |
| Report refuses changed graded input | Edited source/observations/logs or new files in the run | Preserve the history; investigate and record an external addendum/new experiment |
| No matched pairs appear | Both grades exist; `matched` versus uncertain/confounded | State the limitation; do not mark matching without evidence |
| A fast run omits docs or checks | Original unchanged criteria | Keep it unsuccessful; speed does not remove requirements |
| A trial needs more corrections | Declared allowance and actual episodes | End unresolved; improve the design in a new experiment |

## What was checked during preparation

The sample and helpers were checked with **Node.js 24.19.0 in a Linux environment**. The reference application passes all **20 fixed tests**, including CLI option ordering/errors and the preserved baseline cases.

Preparation exercised fresh pair creation, arm-specific Codex settings, timer start/end, current check bundles, grading and reports using **constructed local fixture records**. It checked a failed first application attempt followed by a bounded correction, correction-record consistency, deadline/excess-repair rejection, missing stages and blocked records, stale or altered evidence/source/configuration, unchanged-baseline refusals, duplicate metric rows, token subset validation, changed graded records, missing costs, mixed currencies, estimates, zero-success denominators and paths containing spaces.

The synthetic medians, matched differences, success counts, handover denominator and costs were independently checked. TOML/YAML structure, role links and JavaScript syntax were checked. Markdown/code-box structure and exact embedded file contents were checked.

Native Codex/Copilot profile loading/enforcement, real specialist outputs, actual AI tool traces, real timed/billed model runs, hosted attachment handling and the optional handover diagnostic were **not executed in those hosts during preparation**. Fixture timings are test-operation times, not measurements of agent speed. Synthetic values are fictional. No comparative winner for actual assistants is established here.

Preparation did not test every listed OS/processor or prove every human classification. Your actual repeated attempts supply the workflow evidence. The guide's helpers do not invoke paid APIs.

## Official references

- [OpenAI: evaluate agent workflows](https://developers.openai.com/api/docs/guides/agent-evals) — workflow traces and repeatable evaluations.

- [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents) — exact custom-worker files, inheritance and delegation.

- [Codex configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) — project controls and agent enablement/limits.

- [VS Code subagents](https://code.visualstudio.com/docs/agents/run/subagents) — Local delegation, complete invocation context and optional credit displays.

- [Official OpenAI pricing](https://developers.openai.com/api/docs/pricing) — current API prices when that billing route is relevant.

- [Node.js 24 test runner](https://nodejs.org/docs/latest-v24.x/api/test.html) — the built-in checks used by the sample.

Check current primary documentation when host behaviour differs. Keep correctness, workflow behaviour and worthwhile use tied to the same stated criteria and the evidence actually available.

