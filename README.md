# Practical AI Configuration - Progression

Twelve worked guides for connecting AI assistants to tools, organising larger tasks, coordinating agents, and checking whether the resulting workflows are reliable and useful.

This collection follows [Practical AI Configuration](https://github.com/redhug1/practical-ai-configuration), which introduces project instructions, reusable skills, supporting documents, and task prompts through a main guide and 31 worked examples.

The progression guides use the same plain-English approach, with explicit steps showing what you do, what you ask the assistant to do, and how to check the result. This README provides the starting point, reading advice, and guide index.

## Purpose

The original course explains how to give an assistant useful instructions and reusable procedures. This collection develops that foundation into practical workflows that retrieve information, use connected tools, pass work between roles, and produce results supported by evidence.

You will learn about **Model Context Protocol (MCP)**, which provides a standard way for an assistant application to communicate with programs offering tools and information. You will also learn about an **agent harness**: the software that runs an assistant, loads its configuration, supplies tools, and manages its supported agent features.

The guides distinguish written instructions from enforced permissions, proposed work from completed work, and reported success from results you can verify. Learning to configure these workflows does not mean retraining the underlying model.

## Who this is for

This collection is for individuals and teams who have begun using AI instruction files and skills and want to apply them to more substantial, checkable work.

You do not need previous experience building MCP servers or coordinating several agents. The guides introduce those subjects in stages. Some exercises involve programming tools, Git, or local services; follow the prerequisites in the guide you choose.

## Start with the original course

You will get the most from this collection if you first work through a substantial portion of [Practical AI Configuration](https://github.com/redhug1/practical-ai-configuration).

A useful preparation route is to:

- Read sections 1–14 of the [original main guide](https://github.com/redhug1/practical-ai-configuration/blob/main/AI-Configuration-Guide.md) to understand instruction files, supporting documents, assistant setup, and a first implementation session.

- Complete several of its worked examples, including examples involving project instructions, reusable skills, and checking an assistant's output.

- Work through [Example 31: multiple skills in one project](https://github.com/redhug1/practical-ai-configuration/blob/main/Examples/AI-Config-Example-31.md), particularly before combining skills with connected tools or configuring specialist agents.

- Use the further-training exercises in sections 16–21 to practise creating your own configuration, handling conflicting instructions, and assessing results.

You do not need to finish all 31 examples before starting here. You should, however, be comfortable creating the supplied files, following your chosen assistant route, running the stated checks, and explaining the different purposes of `AGENTS.md`, `SKILL.md`, reference documents, and individual prompts.

## What is included

| Location | Contents |
| --- | --- |
| `README.md` | Purpose, preparation advice, reading order, and links to all twelve guides |
| [Examples/](Examples/) | Twelve separate Markdown guides, each containing its own worked exercise and setup instructions |

The guides include the sample files, prompts, checking commands, expected results, troubleshooting, and reference material within each document. Follow their instructions to create the practice files on your computer.

The documents in `Examples/` are tutorials. Reading or downloading one does not automatically install its MCP server, skill, agent profiles, or permissions.

These guides have their own numbering, 1–12. They do not replace or renumber the original course's 31 examples.

## What you can achieve

After working through the material and checking your results, you should be able to:

- Explain MCP hosts, clients, servers, tools, resources, and prompts, and inspect what a particular connection actually provides.

- Retrieve information through a connected tool and identify the exact sources supporting an answer.

- Use credentials, restricted access, and approval settings appropriately, and recognise the limits of written instructions.

- Combine a reusable skill with retrieved evidence, including checks for stale information, missing inputs, and unsupported conclusions.

- Break a larger job into tasks with clear boundaries, dependencies, expected outputs, and stopping conditions.

- Configure specialist roles, pass usable evidence between them, and give a coordinator bounded work to delegate and assess.

- Keep parallel changes apart, integrate accepted results, and check the combined behaviour.

- Compare one assistant with several agents using the same success criteria, recording correctness, failures, time, tool use, and available cost evidence.

- Save progress, recover interrupted work, limit retries, and avoid duplicate actions before considering scheduled execution.

- Build a small MCP server when an existing tool cannot provide the required capability.

- Adapt a workflow to another harness while checking which configuration, permissions, and behaviours need changing.

## Suggested reading order

### 1. Connect tools and use evidence: guides 1–3

Begin with MCP, then practise access controls before combining a skill with a connected tool. This establishes how information is obtained, what the connection may do, and how to assess its results.

### 2. Organise work before adding agents: guide 4

Complete a staged application change with one assistant. Practise task boundaries, dependencies, and completion evidence before introducing delegation.

### 3. Introduce specialists and coordination: guides 5–8

Give specialists individual tasks, practise controlled handovers, add a coordinator, and then try independent work in separate workspaces. Keep the integration and final checks explicit.

### 4. Evaluate and recover workflows: guides 9–10

Compare complete attempts and record their practical limits. Then practise saved progress, interruption, retries, cancellation, and duplicate prevention. Introduce scheduling only after the supervised workflow behaves reliably.

### 5. Develop and adapt: guides 11–12

Guide 11 is an optional development branch. You can take it after guides 1–3 if building a small MCP server is relevant to you.

Guide 12 uses the inventory-reporting workflow from Guide 11 to practise moving between harnesses. Its starting files are supplied again, but understanding Guide 11 will make the adaptation easier.

Each guide supplies its own starting files. Reading earlier guides provides the knowledge for the next stage; you generally do not need to reuse their completed practice folders.

## Guide index

1. **[Connect your assistant to useful tools and information with MCP.](Examples/AI-Progression-Guide-01-MCP.md)**

    Learn what hosts, clients, servers, tools, resources, and prompts mean. Install an existing filesystem MCP server, inspect its advertised capabilities, connect an assistant, and verify its tool use and answer.

    *Worked example: retrieve information from fictional Maple Notes documents, distinguish current requirements from archived material, identify the supporting files and headings, and repeat a question after a source changes.*

2. **[Control what connected tools can read and change.](Examples/AI-Progression-Guide-02-Tool-Permissions.md)**

    Practise credentials, read-only access, restricted writing, host approvals, denied requests, unavailable services, and credential revocation. Check the difference between instructions and permissions enforced by the service.

    *Worked example: read a supplied local issue tracker, then allow and verify creation of a saved draft in its restricted training project. No real issue-tracker account is required.*

3. **[Combine skills with connected tools.](Examples/AI-Progression-Guide-03-Skills-and-Tools.md)**

    Connect a reusable procedure to available tools, assessment rules, and a report template. Check newly retrieved timestamps, units, calculations, and missing evidence before producing findings.

    *Worked example: use a service-reporting skill with synthetic Maple Notes monitoring data, cite the retrieved sources, identify stale observations and missing restore evidence, and repeat the report after the data changes.*

4. **[Break a larger job into clear, checkable tasks.](Examples/AI-Progression-Guide-04-Task-Planning.md)**

    Define boundaries, dependencies, required inputs, expected outputs, completion conditions, and reasons to stop. Keep a record of progress and revisit earlier work when checks reveal a problem.

    *Worked example: use one assistant to add open-task and completed-task filters to a small command-line application through implementation, testing, documentation, and final review.*

5. **[Create specialist agents with clearly defined responsibilities.](Examples/AI-Progression-Guide-05-Specialist-Agents.md)**

    Distinguish one assistant using several skills from separately configured specialist roles. Give each role suitable instructions, inputs, outputs, and supported access controls.

    *Worked example: ask a planner to plan a future feature, a reviewer to inspect the current program, and a documentation assistant to describe the repaired and checked version. Assign each task separately and test its boundaries.*

6. **[Pass work between agents in a controlled sequence.](Examples/AI-Progression-Guide-06-Controlled-Handovers.md)**

    Define what a handover contains, what its receiver must inspect, and how incomplete, outdated, or disputed work returns to the appropriate stage.

    *Worked example: a planner specifies an explicit all-task option, an implementer changes the program, and a reviewer checks it against the original approved specification and current evidence.*

7. **[Let a coordinating agent delegate and assemble the results.](Examples/AI-Progression-Guide-07-Coordinating-Agents.md)**

    Introduce orchestration: a coordinator assigns bounded work, tracks dependencies and returned results, checks the combined output, and records incomplete work. Inspect the chosen harness's actual configuration and delegation controls.

    *Worked example: assign code, tests, and documentation for a task-summary option, then produce a final report identifying completed, blocked, and unresolved work.*

8. **[Run independent tasks in parallel without conflicting changes.](Examples/AI-Progression-Guide-08-Parallel-Work.md)**

    Practise file ownership, separate working directories or Git worktrees, shared interfaces, and deliberate integration. Decide when parallel work offers little benefit.

    *Worked example: separate workers add a discount code and express delivery to independent components of an order-quotation program. Combine accepted changes in a third workspace and check the complete application.*

9. **[Test whether the agent workflow is reliable and worthwhile.](Examples/AI-Progression-Guide-09-Workflow-Evaluation.md)**

    Compare complete workflows using identical requirements and checks. Record correctness, failed handovers, tool use, elapsed time, human effort, and available usage or cost evidence, including failed attempts.

    *Worked example: run the same task-filtering change with one assistant and with several agents, repeat matched attempts, and assess the evidence without treating unavailable measurements as known.*

10. **[Make longer-running work recoverable and repeatable.](Examples/AI-Progression-Guide-10-Recoverable-Workflows.md)**

    Save progress, resume interrupted jobs, bound retries and time, cancel work, and avoid duplicate outputs. Practise supervised recurrence before optionally adding a schedule.

    *Worked example: generate a fictional project-health report and local issue draft, interrupt the process, and recover the same job without creating duplicate reports or draft records.*

11. **[Build a small MCP server for a tool you already use.](Examples/AI-Progression-Guide-11-Build-An-MCP-Server.md)**

    Follow an optional development branch when existing tools cannot supply the capability you need. Define clear inputs, structured results, useful errors, and tests.

    *Worked example: build a small Python MCP server that exposes a read-only summary of a fictional homelab inventory, then check its results and connect an assistant.*

12. **[Adapt the same workflow to another agent harness.](Examples/AI-Progression-Guide-12-Adapt-To-Another-Harness.md)**

    Separate portable instructions, skills, task definitions, and success criteria from product-specific configuration, permissions, and agent features. Record which behaviours were observed in each harness.

    *Worked example: reproduce the inventory-reporting workflow from Guide 11 in local Codex or Copilot, then in Claude Code. Compare the results and document the configuration and access changes.*

## Before starting a guide

You need VS Code, access to the assistant route you choose, and the software listed in that guide. Some exercises use Node.js, Python, Git, or a local practice service. Follow the specified versions and setup rather than assuming every guide needs the same tools.

Most exercises need only one assistant route. Guide 12's native migration exercise needs a starting harness and Claude Code as the second harness.

| Assistant route | How it is used |
| --- | --- |
| Codex in VS Code | Local project work, connected tools, and supported agent configuration |
| GitHub Copilot in VS Code | Local project work, MCP connections, and supported custom-agent features |
| ChatGPT or ChatGPT Work | The local connection or attachment-based route stated in the individual guide |
| Claude Code | The second native harness in Guide 12 |

The routes are not interchangeable in every exercise. An attachment-based conversation can practise instructions, role boundaries, and evidence review where a guide provides that route. It does not establish that a local MCP connection, installed skill, native delegation, or permission control worked.

Guide 1 requires an actual supported MCP connection for its main exercise. Follow its local assistant routes; uploading the practice documents alone would not complete that connection exercise. Later guides explain their available alternatives and what each route can demonstrate.

The practice instructions cover Ubuntu 22.04, 24.04, and 26.04; Kubuntu 26.04; and supported macOS installations on Intel and Apple Silicon Macs, including the stated M4/M5 examples. Check each guide's software requirements and verification notes. The age or processor of a computer alone does not establish compatibility.

### Create the shared practice folder once

1. On Ubuntu or Kubuntu, open Files or Dolphin and go to your Home folder. On a Mac, open Finder and choose Go → Home.

2. Create a folder named `ai-examples`. On Linux, right-click an empty area and choose New Folder or Create New → Folder. On a Mac, choose File → New Folder.

3. If `ai-examples` already exists from the original course, use it without replacing or deleting its contents.

Each guide explains how to create its own named folder inside it, such as `$HOME/ai-examples/ai-progression-05`. Use the exact folder name in that guide's instructions.

These practice folders are separate from this repository's `Examples/` folder, which holds the tutorial documents.

## How to follow and check the exercises

Follow the labelled steps in order. Instructions marked for the editor, terminal, file manager, or AI chat belong in that location. Expected output and explanatory material are there for comparison or understanding.

Use the supplied fictional data and practice services. Keep the original request, relevant files, and actual check results available when handing work to another role. Repeat affected checks when the source changes.

Some exercises deliberately produce a failed check, refused request, missing input, or interrupted run. Follow the expected result and recovery steps for that exercise.

Read each guide's verification notes. Local program tests and supplied reference answers do not prove that your assistant used the intended skill, tool, agent profile, or permission setting. Native assistant runs and other operating-system combinations may remain reader exercises.

Inspect actual tool activity and saved outputs. Record what passed, what failed, what was unavailable, and what remains unverified. Treat sample evaluation figures as illustrations, and collect your own measurements when comparing workflows.

Written instructions guide behaviour; host settings, service permissions, and execution controls determine the access they actually enforce. A scheduler likewise runs a configured workflow; it does not make an unreliable workflow reliable.

## Adapt a guide to your own task

Choose the closest guide or combination of guides and provide their full contents to your assistant. Ask for a new tutorial using the same structure, explicit instructions, and checking approach, with only the changes needed for your task.

Describe your operating system, chosen harness, available tools, intended result, permitted changes, and any access restrictions. Ask the assistant to identify important missing information and distinguish proposed checks from checks it has actually performed.

Begin with a small supervised version. Use the evaluation and recovery techniques before expanding its scope, adding agents, or scheduling it.

## Keeping the guidance current

Assistant features, MCP support, skill locations, agent configuration, and approval controls can change. Each guide links to official documentation for its product-specific details.

Check those sources when your installed tools behave differently, and repeat the relevant connection, permission, and workflow checks after significant configuration or software changes.
